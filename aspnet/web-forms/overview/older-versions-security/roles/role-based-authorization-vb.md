---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: Autoryzacja oparta na rolach (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten samouczek rozpoczyna się od przyjrzeć się jak w ramach ról kojarzy ról użytkownika z jego kontekstu zabezpieczeń. Następnie sprawdza, czy sposobu stosowania opartej na rolach adres URL...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: b8cf8cc5fa802eb812482b236c8b2825423027e7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390988"
---
<a name="role-based-authorization-vb"></a>Autoryzacja oparta na rolach (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> Ten samouczek rozpoczyna się od przyjrzeć się jak w ramach ról kojarzy ról użytkownika z jego kontekstu zabezpieczeń. Następnie sprawdza, czy sposobu stosowania reguł autoryzacji adresów URL opartej na rolach. Po, przyjrzymy się przy użyciu środków deklaracyjne i programowe dla Zmienianie wyświetlanych danych i funkcje oferowane przez strony ASP.NET.


## <a name="introduction"></a>Wprowadzenie

W <a id="_msoanchor_1"> </a> [ *autoryzacja na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczek widzieliśmy sposób używania Autoryzacja adresów URL, aby określić, co użytkownicy można znaleźć określonego zestawu stron. Za pomocą tylko niewielki znaczników w `Web.config`, firma Microsoft może wydać polecenie ASP.NET, aby zezwolić tylko uwierzytelnionym użytkownikom odwiedzić stronę. Lub firma Microsoft może określają, że zezwolono na tylko użytkowników Tito i Robert wskazują, że wszystkim uwierzytelnionym użytkownikom z wyjątkiem Sam były dozwolone.

Oprócz autoryzacji adresów URL również przyjrzeliśmy się deklaracyjne i programowe metod kontroli danych wyświetlanych i funkcje oferowane przez strony, na podstawie użytkownika, odwiedzając. W szczególności utworzyliśmy strony, którego zawartość bieżącego katalogu na liście. Każda osoba może odwiedzić tę stronę, ale tylko uwierzytelnionym użytkownikom można wyświetlać te pliki zawartości i tylko Tito można usunąć plików.

Stosowanie reguły autoryzacji na podstawie użytkownika, użytkownik może przejść do okropnej księgowych. Będzie łatwiejszy w utrzymaniu podejściem jest używać autoryzacji opartej na rolach. Dobra wiadomość jest równie pracy narzędzia naszych dyspozycji stosowania reguł autoryzacji oraz z rolami, jak w przypadku kont użytkowników. Reguły autoryzacji adresów URL można określić role zamiast użytkowników. Kontrolki widoku logowania, które powoduje wyświetlenie danych wyjściowych inne niż dla użytkowników uwierzytelnionych i anonimowych, można skonfigurować do wyświetlania różnych zawartości na podstawie ról zalogowanego użytkownika. I interfejs API ról zawiera metody do oznaczania role zalogowanego użytkownika.

Ten samouczek rozpoczyna się od przyjrzeć się jak w ramach ról kojarzy ról użytkownika z jego kontekstu zabezpieczeń. Następnie sprawdza, czy sposobu stosowania reguł autoryzacji adresów URL opartej na rolach. Po, przyjrzymy się przy użyciu środków deklaracyjne i programowe dla Zmienianie wyświetlanych danych i funkcje oferowane przez strony ASP.NET. Zaczynajmy!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Opis sposobu ról są skojarzone z kontekstu zabezpieczeń użytkownika

Zawsze, gdy żądanie dociera potoku platformy ASP.NET jest skojarzony z kontekstu zabezpieczeń, który zawiera informacje identyfikujące osoby zgłaszającej żądanie. Korzystając z uwierzytelniania formularzy, biletu uwierzytelniania jest używany jako token tożsamości. Tak jak Omówiliśmy to w <a id="_msoanchor_2"> </a> [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-vb.md) i <a id="_msoanchor_3"> </a> [ *formularzy Konfiguracja uwierzytelniania i Tematy zaawansowane* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) samouczki, `FormsAuthenticationModule` jest odpowiedzialny za sprawdzenie tożsamość podmiotu żądającego, która obsługuje je podczas [ `AuthenticateRequest` zdarzeń](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Jeśli zostanie znaleziony prawidłowy, ważność biletu, `FormsAuthenticationModule` dekoduje go do ustalenia tożsamości obiektu żądającego. Tworzy nową `GenericPrincipal` obiektu, a następnie przypisuje do `HttpContext.User` obiektu. Celem jednostki, takie jak `GenericPrincipal`, polega na określeniu nazwę użytkownika uwierzytelnionego i co należy ona do ról. W tym celu jest oczywiste fakt, że wszystkie obiekty jednostki mają `Identity` właściwości i `IsInRole(roleName)` metody. `FormsAuthenticationModule`, Jednak nie jest zainteresowany rejestrowania informacji o rolach i `GenericPrincipal` tworzy w obiekcie nie określono żadnych ról.

Jeśli w ramach ról jest włączona, [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) moduł HTTP kroki po `FormsAuthenticationModule` i identyfikuje ról użytkownika uwierzytelnionego podczas [ `PostAuthenticateRequest` zdarzeń](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), który generowane po `AuthenticateRequest` zdarzeń. Jeśli żądanie pochodzi z uwierzytelnionego użytkownika, `RoleManagerModule` zastępuje `GenericPrincipal` obiekt utworzony przez `FormsAuthenticationModule` i zastępuje go tekstem [ `RolePrincipal` obiektu](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). `RolePrincipal` Klasy za pomocą interfejsu API ról Określanie ról, jakie należy użytkownik.

Rysunek 1 przedstawia przepływu pracy trybu potokowego programu ASP.NET, podczas korzystania z uwierzytelniania formularzy i w ramach ról. `FormsAuthenticationModule` Najpierw wykonuje identyfikuje użytkownika za pośrednictwem jej biletu uwierzytelniania i tworzy nową `GenericPrincipal` obiektu. Następnie `RoleManagerModule` kroki i zastępuje `GenericPrincipal` obiekt z `RolePrincipal` obiektu.

Jeśli użytkownik anonimowy odwiedza witryny, ani `FormsAuthenticationModule` ani `RoleManagerModule` tworzy obiekt podmiotu zabezpieczeń.


[![Zdarzenia potoku platformy ASP.NET dla uwierzytelnionego użytkownika podczas korzystania z uwierzytelniania formularzy i w ramach ról](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**Rysunek 1**: zdarzenia potoku platformy ASP.NET dla uwierzytelniony użytkownik podczas za pomocą uwierzytelniania formularzy i struktury role ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Informacje o rolach w pliku Cookie do buforowania

`RolePrincipal` Obiektu `IsInRole(roleName)` wywołania metody `Roles`.`GetRolesForUser` Aby uzyskać role dla użytkownika w celu ustalenia, czy użytkownik jest członkiem *roleName*. Korzystając z `SqlRoleProvider`, powoduje to zapytanie do bazy danych magazynu ról. Korzystając z reguł autoryzacji adresów URL opartej na rolach `RolePrincipal`firmy `IsInRole` metoda zostanie wywołana na każde żądanie do strony, który jest chroniony przez reguły autoryzacji adresów URL opartej na rolach. Zamiast do wyszukiwania informacji w roli w bazie danych na każde żądanie `Roles` framework zawiera opcję Role użytkownika w pliku cookie w pamięci podręcznej.

Jeśli w ramach ról jest skonfigurowany do ról użytkownika w pliku cookie w pamięci podręcznej `RoleManagerModule` podczas potoku platformy ASP.NET tworzy plik cookie [ `EndRequest` zdarzeń](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Ten plik cookie jest używany w kolejnych żądań w `PostAuthenticateRequest`, czyli kiedy `RolePrincipal` obiekt zostanie utworzony. Jeśli plik cookie jest prawidłowy i nie wygasł, dane w pliku cookie zostanie przeanalizowana i używanych do wypełniania ról użytkownika, oszczędzając `RolePrincipal` od konieczności wywoływania `Roles` klasę, aby określić role użytkownika. Rysunek 2 przedstawia ten przepływ pracy.


[![Informacje o rolach użytkownika mogą być przechowywane w pliku Cookie w celu zwiększenia wydajności](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**Rysunek 2**: Użytkownik roli informacje mogą być przechowywane w pliku Cookie do poprawiania wydajności ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image6.png))


Domyślnie mechanizmu roli pamięci podręcznej plików cookie jest wyłączona. Można ją włączyć za pomocą `<roleManager>`; znaczników konfiguracji w `Web.config`. Omówiliśmy przy użyciu [ `<roleManager>` elementu](https://msdn.microsoft.com/library/ms164660.aspx) do określenia dostawców ról w <a id="_msoanchor_4"> </a> [ *tworzenie i zarządzanie rolami* ](creating-and-managing-roles-vb.md) samouczka dlatego konto powinno mieć już ten element w Twojej aplikacji `Web.config` pliku. Ustawienia plików cookie w pamięci podręcznej roli są określane jako atrybuty `<roleManager>`; element i są podsumowane w tabeli 1.

> [!NOTE]
> Ustawienia konfiguracji wymienione w tabeli 1 umożliwia określenie właściwości wynikowy cookie roli w pamięci podręcznej. Aby uzyskać więcej informacji na temat plików cookie, jak działają i ich właściwości, przeczytaj [w tym samouczku pliki cookie](http://www.quirksmode.org/js/cookies.html).


| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Opis</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Wartość logiczna wskazująca, czy jest używane buforowanie plików cookie. Wartość domyślna to `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Nazwa pliku cookie z pamięci podręcznej dla roli. Wartość domyślna to ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                Ścieżka dla pliku cookie nazwy ról. Atrybut path umożliwia zatem programistą, aby ograniczyć zakres pliku cookie do hierarchii określonego katalogu. Wartość domyślna to "/", która informuje przeglądarkę, aby wysłać plik cookie biletu uwierzytelniania na każde żądanie, wprowadzone do domeny.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Wskazuje, jakie metody są używane do ochrony plików cookie z pamięci podręcznej dla roli. Dozwolone wartości to: `All` (domyślnie); `Encryption`; `None`; i `Validation`. Odwołaj się do kroku 3 <a id="_anchor_5"> </a> [ *Konfiguracja uwierzytelniania formularzy i Tematy zaawansowane* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) samouczka, aby uzyskać więcej informacji na temat tych poziomów ochrony.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Wartość logiczna wskazująca, czy połączenia SSL jest wymagany do przesyłania pliku cookie uwierzytelniania. Wartość domyślna to `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Wartość logiczna wskazująca, czy limit czasu plików cookie jest resetowany każdorazowo użytkownik odwiedza witrynę podczas jednej sesji. Wartość domyślna to `false`. Ta wartość jest tylko istotne podczas `createPersistentCookie` ustawiono `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Określa czas w minutach, po którym wygasa plik cookie biletu uwierzytelniania. Wartość domyślna to `30`. Ta wartość jest tylko istotne podczas `createPersistentCookie` ustawiono `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Wartość logiczna określająca, czy plik cookie z pamięci podręcznej roli jest plik cookie sesji lub trwały plik cookie. Jeśli `false` (ustawienie domyślne), jest używany plik cookie sesji, który jest usuwany po zamknięciu przeglądarki. Jeśli `true`, trwały plik cookie jest używany; wygaśnie `cookieTimeout` liczba minut po jego utworzeniu lub po poprzednim wizyty, w zależności od wartości `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Określa wartość domeny pliku cookie. Wartość domyślna to ciąg pusty, co powoduje, że przeglądarka do korzystania z domeny, z którego został wydany (na przykład www.yourdomain.com). W takim przypadku plik cookie zostanie <strong>nie</strong> wysłania w przypadku wprowadzania żądań poddomen, na przykład admin.yourdomain.com. Jeśli chcesz, aby plik cookie, które mają być przekazane do wszystkimi domenami podrzędnymi trzeba dostosować `domain` atrybutu, ustawienie "yourdomain.com".                                                                                                                                                 |
|    `maxCachedResults`     | Określa maksymalną liczbę nazw ról, które są buforowane w pliku cookie. Wartość domyślna to 25. `RoleManagerModule` Nie tworzy plik cookie dla użytkowników, którzy należą do więcej niż `maxCachedResults` ról. W związku z tym `RolePrincipal` obiektu `IsInRole` metoda użyje `Roles` klasę, aby określić role użytkownika. Przyczyna `maxCachedResults` istnieje ponieważ wielu agentów użytkownika nie zezwalają na pliki cookie większych niż 4096 bajtów. Więc ten limit jest przeznaczona do zmniejszenia prawdopodobieństwa przekracza ten limit rozmiaru. W przypadku nazw ról ekstremalnie długie warto rozważ określenie mniejszej `maxCachedResults` wartość; contrariwise, jeśli masz bardzo krótko roli nazwy, należy prawdopodobnie zwiększyć tę wartość. |

**Tabela 1**: opcje konfiguracji roli pamięci podręcznej plików Cookie

Skonfigurujmy naszej aplikacji na używanie plików cookie z pamięci podręcznej nietrwałe roli. Aby to zrobić, należy zaktualizować `<roleManager>` element `Web.config` obejmujący następujące atrybuty powiązanych plików cookie:

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

Czy mogę zaktualizować `<roleManager>`; elementu przez dodanie trzy atrybuty: `cacheRolesInCookie`, `createPersistentCookie`, i `cookieProtection`. Ustawiając `cacheRolesInCookie` do `true`, `RoleManagerModule` będzie teraz automatycznie w pamięci podręcznej role użytkownika w pliku cookie, niż musieć go wyszukać informacje o rolach użytkownika na każde żądanie. Jawnie ustawić `createPersistentCookie` i `cookieProtection` atrybuty do `false` i `All`, odpowiednio. Technicznie rzecz biorąc nie musisz określić, że wartości tych atrybutów, ponieważ tylko ich przypisane do wartości domyślnych, ale umieścić je w tym miejscu umożliwiają jawnie wyczyść nie używam trwały plik cookie i czy plik cookie jest zaszyfrowany i zweryfikowane.

To wszystko jest do niego! Od tej pory w ramach ról będzie w pamięci podręcznej ról użytkowników w plikach cookie. Jeśli przeglądarka nie obsługuje pliki cookie lub jeśli ich pliki cookie są usuwane lub zgubione, jakiś sposób, to nic wielkiego — `RolePrincipal` po prostu użyje obiektu `Roles` klasy w przypadku, że pliki cookie nie (lub jeden z nieprawidłowymi lub wygasłymi) jest dostępny.

> [!NOTE]
> Wzorce firmy Microsoft &amp; zniechęca rozwiązania w zakresie grupy do przy użyciu roli trwałe pliki cookie z pamięci podręcznej. Ponieważ posiadanie pliku cookie z pamięci podręcznej roli jest wystarczające, aby udowodnić, że członkostwo w roli, jeśli haker jakiś sposób można uzyskać dostęp do prawidłowego użytkownika w pliku cookie on personifikacji tego użytkownika. Prawdopodobieństwo takich sytuacji zwiększa się, gdy plik cookie jest utrwalony w przeglądarce użytkownika. Aby uzyskać więcej informacji na temat tego zalecenia zabezpieczeń, jak również inne problemy dotyczące zabezpieczeń, zobacz [listy pytanie zabezpieczeń dla programu ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Krok 1: Definiowanie reguł autoryzacji opartej na rolach adresów URL

Zgodnie z opisem w <a id="_msoanchor_6"> </a> [ *autoryzacja na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczek, Autoryzacja adresów URL oferuje sposób ograniczyć dostęp do zestawu stron na użytkownika na podstawie użytkownika lub roli roli Podstawa. Reguły autoryzacji adresów URL są zapisane w `Web.config` przy użyciu [ `<authorization>` elementu](https://msdn.microsoft.com/library/8d82143t.aspx) z `<allow>` i `<deny>` elementów podrzędnych. Oprócz reguł autoryzacji dotyczące użytkowników omówione w poprzednich samouczkach każdego `<allow>` i `<deny>` elementu podrzędnego może również obejmować:

- Określonej roli
- Rozdzielana przecinkami lista ról

Na przykład reguły autoryzacji adresów URL udzielić dostępu do tych użytkowników z rolami administratorów i nadzorców, ale nie zezwoli na dostęp do wszystkich innych osób:

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

`<allow>` Elementem w znacznikach powyżej stwierdzający, że dozwolone są role administratorów i nadzorców; `<deny>`; element powoduje, że że *wszystkich* użytkownik nie będzie mógł.

Skonfigurujmy naszą aplikację tak, aby `ManageRoles.aspx`, `UsersAndRoles.aspx`, i `CreateUserWizardWithRoles.aspx` strony są dostępne jedynie w do tych użytkowników w roli administratora podczas `RoleBasedAuthorization.aspx` strona pozostaje dostępny dla wszystkich osób odwiedzających.

Aby to osiągnąć, Rozpocznij od dodania `Web.config` plik `Roles` folderu.


[![Dodaj plik Web.config w katalogu ról](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**Rysunek 3**: Dodaj `Web.config` plik `Roles` katalogu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image9.png))


Następnie dodaj następujące znaczniki konfiguracji w celu `Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

`<authorization>` Element `<system.web>` sekcja wskazuje, że tylko użytkownicy należący do roli Administratorzy mogą uzyskiwać dostęp do zasobów ASP.NET `Roles` katalogu. `<location>` Element definiuje alternatywny zestaw reguł autoryzacji adresów URL dla `RoleBasedAuthorization.aspx` strony, dzięki czemu wszyscy użytkownicy w celu odwiedzenia strony.

Po zapisaniu zmian do `Web.config`, zaloguj się jako użytkownik, który nie znajduje się w roli administratora, a następnie spróbuj odwiedzają jedną z chronionych stron. `UrlAuthorizationModule` Wykryje, że nie masz uprawnień, aby przejść do żądanego zasobu; w związku z tym, `FormsAuthenticationModule` nastąpi przekierowanie do strony logowania. Na stronie logowania następnie nastąpi przekierowanie do `UnauthorizedAccess.aspx` strony (zobacz rysunek 4). Końcowe przekierowanie ze strony logowania do `UnauthorizedAccess.aspx` występuje z powodu dodana do strony logowania w kroku 2 kodu <a id="_msoanchor_7"> </a> [ *autoryzacja na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczka. W szczególności, strony logowania automatycznie przekierowuje każdemu uwierzytelnionemu użytkownikowi na `UnauthorizedAccess.aspx` Jeśli ciąg zapytania zawiera `ReturnUrl` parametr, jako parametr wskazuje, że użytkownik dotarła do strony logowania po próbie wyświetlenia strony, nie miał uprawnienia do wyświetlenia.


[![Tylko użytkownicy należący do roli Administratorzy mogą wyświetlać chronione stron](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**Rysunek 4**: tylko użytkownicy należący do roli Administratorzy mogą wyświetlać strony chronione ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image12.png))


Wyloguj się, a następnie zaloguj się jako użytkownik, który znajduje się w roli administratora. Teraz można wyświetlić trzy strony chronione.


[![Tito mogą odwiedzić UsersAndRoles.aspx strony ponieważ jest on w roli administratora](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**Rysunek 5**: Tito mogą odwiedzić `UsersAndRoles.aspx` strony ponieważ jest on w roli administratora ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> Podczas określania reguł autoryzacji adresów URL — dla ról lub użytkowników — należy pamiętać, że zasady są przeanalizowany pojedynczo, od góry w dół. Gdy tylko zostanie znalezione dopasowanie, użytkownik jest udzielić lub odmówić dostępu, w zależności od jeśli dopasowanie został znaleziony w `<allow>` lub `<deny>` elementu. **Jeśli nie zostanie znalezione dopasowanie, użytkownik uzyskuje dostęp.** W związku z tym, jeśli chcesz ograniczyć dostęp do co najmniej jednego konta użytkownika, konieczne jest użycie `<deny>` element jako po ostatnim elemencie w konfiguracji autoryzacja adresu URL. **Jeśli Twoje reguły autoryzacji adresów URL nie zawierają**`<deny>`**elementu, wszyscy użytkownicy zostanie przyznany dostęp.** Bardziej szczegółowe omówienie dotyczące jak reguły autoryzacji adresów URL są analizowane, odwołaj się do "przyjrzeć się jak `UrlAuthorizationModule` przy użyciu reguł autoryzacji udzielić lub odmówić dostępu" sekcji <a id="_msoanchor_8"> </a> [  *Autoryzacja oparta na użytkowniku* ](../membership/user-based-authorization-vb.md) samouczka.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Krok 2: Ograniczanie funkcjonalności na podstawie ról aktualnie zalogowanego użytkownika

Adres URL autoryzacji sprawia, że go łatwo określić zgrubnym autoryzacji reguł tego stanu tożsamości, które są dozwolone, a te, które są zabronione możliwości wyświetlenia określonej strony (lub wszystkich stron w folderze i jego podfolderów). Jednak w niektórych przypadkach warto zezwala wszystkim użytkownikom na stronę, ale ograniczenia funkcji strony, na podstawie ról zaproszonych użytkowników. To może pociągać za sobą data Pokazywanie lub ukrywanie na podstawie roli użytkownika lub oferowanie dodatkowych funkcji dla użytkowników, które należą do określonej roli.

Takie zasady autoryzacji opartej na rolach szczegółową można zaimplementować deklaratywne lub programowo (lub za pomocą kombinacji obu). W następnej sekcji Zobaczymy się, jak zaimplementować deklaratywne szczegółowej autoryzacji za pomocą kontrolki widoku logowania. Poniżej omówimy techniki programistyczne. Zanim można przyjrzymy się stosowanie reguł autoryzacji szczegółową, jednak najpierw należy utworzyć którego funkcji zależy od roli użytkownika, odwiedzając jego stronę.

Utwórzmy strony zawierającej zestawienie wszystkich kont użytkowników w systemie w widoku GridView. Kontrolki GridView będzie zawierać nazwę użytkownika, adres e-mail, Data ostatniego logowania i komentarzy dotyczących użytkownika każdego użytkownika. Oprócz wyświetlania informacji dotyczących użytkownika, widoku GridView obejmują edycji i usuwania możliwości. Firma Microsoft będzie początkowo utworzyć tę stronę za pomocą edycji i usuń funkcje dostępne dla wszystkich użytkowników. W sekcji "Za pomocą kontrolki widoku logowania" i "Programowo Ograniczanie funkcjonalności" widzimy jak włączyć lub wyłączyć te funkcje, na podstawie roli zaproszonych użytkowników.

> [!NOTE]
> Strony ASP.NET, który jesteśmy kompilacji używa kontrolki widoku siatki, aby wyświetlić konta użytkowników. Ponieważ ten samouczek, który seria skupia się na uwierzytelnianie formularzy, autoryzacji, konta użytkowników i ról nie chcę poświęcać zbyt dużo czasu, omawiając przebiega w kontrolce GridView. Chociaż ten samouczek zawiera określone instrukcje krok po kroku dotyczące konfigurowania tej strony, nie delve szczegóły Dlaczego wprowadzono niektórych opcji, lub w wyniku renderowania posiadane szczególne właściwości efektu. Dla głębszego zbadania kontrolki GridView, zapoznaj się z moich *[Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)* serii samouczków.


Zacznij od otwarcia `RoleBasedAuthorization.aspx` stronie `Roles` folderu. Przeciągnij GridView ze strony na projektanta, a następnie ustaw jego `ID` do `UserGrid`. Za chwilę napiszemy kod, który wywołuje `Membership`.`GetAllUsers` Metoda i wiąże wynikowy `MembershipUserCollection` obiekt do kontrolki GridView. `MembershipUserCollection` Zawiera `MembershipUser` obiekt dla każdego konta użytkownika w systemie; `MembershipUser` obiekty mają właściwości, takie jak `UserName`,`Email`,`LastLoginDate` itd.

Zanim napiszemy kod, który wiąże kont użytkowników do siatki, najpierw zdefiniujmy pola GridView. W widoku GridView tagu inteligentnego, kliknij link "Edytuj kolumny", aby uruchomić okna dialogowego pola (zobacz rysunek 6). W tym miejscu Usuń zaznaczenie pola wyboru "automatycznie Generuj fields" w lewym dolnym rogu. Ponieważ chcemy, aby ta GridView obejmują edytowania i usuwania możliwości dodawania CommandField i ustaw jego `ShowEditButton` i `ShowDeleteButton` właściwości na wartość True. Następnie dodaj cztery pola do wyświetlania `UserName`, `Email`, `LastLoginDate`, i `Comment` właściwości. Użyj elementu BoundField dwie właściwości tylko do odczytu (`UserName` i `LastLoginDate`) i kontrolek TemplateField dla dwóch pól edycji (`Email` i `Comment`).

Pierwszy ekran elementu BoundField mają `UserName` właściwości; ustaw jego `HeaderText` i `DataField` właściwości "Nazwa_użytkownika". To pole nie będą edytowalne, więc ustawić jej `ReadOnly` właściwości na wartość True. Konfigurowanie `LastLoginDate` elementu BoundField, ustawiając jego `HeaderText` do "Ostatniego logowania" i jego `DataField` do "LastLoginDate". Teraz należy sformatować dane wyjściowe tego elementu BoundField, pojawia się tylko datę (zamiast daty i godziny). Aby to osiągnąć, należy ustawić tego elementu BoundField `HtmlEncode` wartość False dla właściwości i jego `DataFormatString` właściwość "{0:d}". Również ustawić `ReadOnly` właściwości na wartość True.

Ustaw `HeaderText` właściwości dwóch kontrolek TemplateField "Email" i "Comment".


[![W widoku GridView pola można skonfigurować za pomocą okna dialogowego pola](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**Rysunek 6**: GridView pola może być skonfigurowany za pośrednictwem okno dialogowe pola ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image18.png))


Teraz musisz zdefiniować `ItemTemplate` i `EditItemTemplate` kontrolek TemplateField "Comment" i "Email". Dodaj kontrolkę etykieta w sieci Web do każdego z `ItemTemplates` i wiązania ich `Text` właściwości w celu `Email` i `Comment` właściwości, odpowiednio.

TemplateField "Email", Dodaj pole tekstowe o nazwie `Email` do jego `EditItemTemplate` i powiąż jego `Text` właściwość `Email` właściwości przy użyciu dwukierunkowego wiązania danych. Dodaj RequiredFieldValidator i RegularExpressionValidator do `EditItemTemplate` aby upewnić się, że obiekt odwiedzający właściwości wiadomości E-mail do edycji wprowadził prawidłowy adres e-mail. TemplateField "Comment", można dodać w wielowierszowym polu tekstowym o nazwie `Comment` do jego `EditItemTemplate`. Ustaw pole tekstowe `Columns` i `Rows` właściwości do 40 i 4, odpowiednio, a następnie wiążą jego `Text` właściwość `Comment` właściwości przy użyciu dwukierunkowego wiązania danych.

Po skonfigurowaniu tych kontrolek TemplateField, ich oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

Podczas edytowania lub usuwania konta użytkownika, musimy wiedzieć, że użytkownik `UserName` wartości właściwości. Ustaw GridView `DataKeyNames` właściwość "Nazwa_użytkownika", aby te informacje są dostępne za pośrednictwem GridView `DataKeys` kolekcji.

Na koniec Dodaj kontrolki podsumowania walidacji do strony i ustaw jego `ShowMessageBox` właściwości na wartość True i jego `ShowSummary` wartość False dla właściwości. Przy użyciu tych ustawień podsumowania walidacji wyświetli alert po stronie klienta, jeśli użytkownik próbuje edytować konto użytkownika za pomocą adresu e-mail lub jest nieprawidłowy.

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

Firma Microsoft została zakończona pomyślnie oznaczeniu deklaracyjnym tej strony. Naszym kolejnym krokiem jest powiązać zbiór kont użytkowników z kontrolki GridView. Dodaj metodę o nazwie `BindUserGrid` do `RoleBasedAuthorization.aspx` strony związanym z kodem klasę, która wiąże `MembershipUserCollection` zwrócone przez `Membership.GetAllUsers` do `UserGrid` GridView. Wywołanie tej metody z `Page_Load` program obsługi zdarzeń pierwszej wizyty strony.

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

Przy użyciu tego kodu w miejscu odwiedź stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 7, powinien zostać wyświetlony GridView wyświetlanie informacji na temat poszczególnych kont użytkowników w systemie.


[![UserGrid GridView Wyświetla informacje dotyczące poszczególnych użytkowników w systemie](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**Rysunek 7**: `UserGrid` GridView Wyświetla informacje dotyczące każdego użytkownika w systemie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> `UserGrid` GridView Wyświetla listę wszystkich użytkowników w interfejsie zgodnym niestronicowanej. Ten interfejs prosta siatka nie jest przeznaczony do scenariuszy w przypadku, gdy istnieje kilka tuzinów lub większej liczby użytkowników. Jedną z opcji jest skonfigurować GridView w celu włączenia stronicowania. `Membership.GetAllUsers` Metoda ma dwa przeciążenia: jedną, która przyjmuje nie parametrów wejściowych i zwraca wszystkich użytkowników i taki, który przyjmuje wartości całkowitoliczbowe dla indeks strony i rozmiar strony, a następnie zwraca określony podzbiór użytkowników. Drugie przeciążenie może służyć do bardziej efektywnie strony za pomocą użytkowników, ponieważ zwraca ono tylko dokładne działu kont użytkowników zamiast *wszystkich* z nich. Jeśli masz tysiące konta użytkowników, warto wziąć pod uwagę interfejs na podstawie filtru, taki, który zawiera tylko tych użytkowników, których nazw zaczyna się od znaku wybranych dla wystąpienia. [ `Membership.FindUsersByName` Metoda](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) jest idealnym rozwiązaniem umożliwiających tworzenie interfejsów użytkownika pracujących w filtru. Przyjrzymy się tworzenia takiego interfejsu w przyszłości zapoznać się z samouczkiem.


W kontrolce GridView oferuje wbudowane edytowania i usuwania pomocy technicznej, gdy kontrolka jest powiązana z kontroli źródła danych prawidłowo skonfigurowane, takie jak SqlDataSource lub kontrolki ObjectDataSource. `UserGrid` GridView, jednak ma programowo powiązane dane; w związku z tym, firma Microsoft musi napisać kod, aby wykonać te dwa zadania. W szczególności, musimy utworzyć procedury obsługi zdarzeń dla GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, i `RowDeleting` zdarzenia, które są wywoływane, gdy użytkownik kliknie GridView Edycja, anulowanie, aktualizacji lub usuwanie przycisków.

Rozpocznij od utworzenia procedury obsługi zdarzeń dla GridView `RowEditing`, `RowCancelingEdit`, i `RowUpdating` zdarzenia, a następnie dodaj następujący kod:

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

`RowEditing` i `RowCancelingEdit` procedury obsługi zdarzeń po prostu ustaw GridView `EditIndex` właściwości, a następnie ponowne wiązanie użytkownika konta do siatki. Ciekawe rzeczy odbywa się w `RowUpdating` programu obsługi zdarzeń. Ta procedura obsługi zdarzeń, który rozpoczyna się poprzez zapewnienie, że dane jest prawidłowy, a następnie bierze `UserName` wartości z konta użytkownika edytowanych `DataKeys` kolekcji. `Email` i `Comment` pola tekstowe w dwóch kontrolek TemplateField `EditItemTemplate` s programowo odwołania do. Ich `Text` właściwości zawierają adres e-mail edytowany i komentarz.

Aby można było zaktualizować konto użytkownika za pośrednictwem interfejsu API członkostwa, należy najpierw uzyskać informacje o użytkowniku, co możemy zrobić poprzez wywołanie `Membership.GetUser(userName)`. Zwrócony `MembershipUser` obiektu `Email` i `Comment` właściwości następnie są aktualizowane przy użyciu wartości wprowadzone w dwóch pól tekstowych z interfejsu edycji. Na koniec te modyfikacje są zapisywane przy użyciu wywołania do [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). `RowUpdating` Program obsługi zdarzeń kończy, przywrócenie GridView do jego wstępnie edycji interfejsu.

Następnie należy utworzyć `RowDeleting` RowDeleting obsługi zdarzeń, a następnie dodaj następujący kod:

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

Powyższej procedury obsługi zdarzeń, który rozpoczyna się przez Przechwytywanie `UserName` wartość z GridView `DataKeys` kolekcji; to `UserName` wartość jest następnie przekazywany do klasy członkostwa [ `DeleteUser` metody](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). `DeleteUser` Metoda usuwa konto użytkownika z systemu, w tym członkostwa powiązane dane (takie jak role, jakie należy ten użytkownik). Po usunięciu użytkownika, siatki `EditIndex` jest ustawiona na wartość -1 (w przypadku, gdy użytkownik kliknął element usuwania podczas kolejnego wiersza jest w trybie edycji) i `BindUserGrid` metoda jest wywoływana.

> [!NOTE]
> Przycisk Usuń nie wymaga dowolny rodzaj potwierdzenie od użytkownika przed usunięciem konta użytkownika. Zachęcam Cię dodać jakąś formę potwierdzenie użytkownika, aby zmniejszyć prawdopodobieństwo konta są przypadkowo usunięte. Jednym z najprostszych sposobów, aby potwierdzić akcję jest poprzez okno dialogowe Potwierdź po stronie klienta. Aby uzyskać więcej informacji na temat tej techniki, zobacz [dodawanie potwierdzenia po stronie klienta podczas usuwania](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Upewnij się, że na tej stronie działa zgodnie z oczekiwaniami. Użytkownik powinien móc edytować adres e-mail użytkownika i komentarz, a także usunąć dowolne konto użytkownika. Ponieważ `RoleBasedAuthorization.aspx` strona jest dostępna dla wszystkich użytkowników, każdy użytkownik — nawet anonimowe osoby odwiedzające — tę stronę i edytować i usuwać konta użytkowników! Zaktualizujmy tę stronę, tak aby tylko użytkowników w rolach udostępniania Nadzorcom i Administratorzy mogą edytować adres e-mail użytkownika i komentarz, a tylko administratorzy mogą usunąć konto użytkownika.

W sekcji "Za pomocą kontrolki widoku logowania" analizuje za pomocą kontrolki widoku logowania, aby wyświetlić instrukcje specyficzne dla roli użytkownika. Jeśli osobę w roli administratora wizyty na tej stronie, firma Microsoft zostaną wyświetlone instrukcje dotyczące sposobu edytowania i usuwania użytkowników. Jeśli użytkownik w roli nadzorców osiągnie tę stronę, firma Microsoft zostaną wyświetlone instrukcje na edytowanie użytkowników. A jeśli odwiedzający jest anonimowa lub nie znajduje się w roli nadzorców lub Administratorzy, zostanie on wyświetlony komunikat wyjaśniający, że nie można edytować ani usunąć informacje o koncie użytkownika. W sekcji "Programowo Ograniczanie funkcjonalności" napiszemy kod, który programowo pokazuje lub ukrywa przyciski edytowania i usuwania, na podstawie roli użytkownika.

### <a name="using-the-loginview-control"></a>Za pomocą kontrolki widoku logowania

Jak widzieliśmy w poprzednich samouczkach kontrolki widoku logowania jest przydatny do wyświetlania różne interfejsy dla użytkowników uwierzytelnionych i anonimowych, ale kontrolki widoku logowania może również służyć do wyświetlania różny kod znaczników na podstawie ról użytkownika. Użyjmy kontrolki widoku logowania, aby wyświetlić różne instrukcje w zależności od roli zaproszonych użytkowników.

Początek, dodając widoku logowania powyżej `UserGrid` GridView. Omawianego wcześniej, kontrolki widoku logowania ma dwa wbudowane szablony: `AnonymousTemplate` i `LoggedInTemplate`. Podaj krótką wiadomość w obu tych szablonów, które informuje użytkownika, że nie można edytować ani usunąć informacje o użytkowniku.

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

Oprócz `AnonymousTemplate` i `LoggedInTemplate`, może zawierać kontrolki widoku logowania *RoleGroups*, które są specyficzne dla ról szablonów. Każdy RoleGroup zawiera tylko jedną właściwość `Roles`, która określa, jakie role RoleGroup dotyczy. `Roles` Właściwość można ustawić jedną rolę (na przykład "Administratorzy") lub rozdzielaną przecinkami listę ról (takich jak "Administratorzy, nadzorców").

Aby zarządzać RoleGroups, kliknij link "Edytuj RoleGroups" z formantu tagu inteligentnego, aby wyświetlić się Edytor kolekcji RoleGroup. Dodaj dwa nowe RoleGroups. Ustaw pierwszą RoleGroup `Roles` właściwość "Administratorzy" i celu "Nadzorców" sekundy.


[![Zarządzanie szablonami pełnionymi widoku logowania za pomocą edytora kolekcji RoleGroup](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**Rysunek 8**: Zarządzanie widoku logowania pełnionymi szablonów za pomocą RoleGroup — Edytor kolekcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image24.png))


Kliknij przycisk OK, aby zamknąć Edytor kolekcji RoleGroup; Spowoduje to zaktualizowanie oznaczeniu deklaracyjnym widoku logowania, aby uwzględnić `<RoleGroups>` sekcji z `<asp:RoleGroup>` element podrzędny dla każdego RoleGroup zdefiniowane w edytorze RoleGroup w kolekcji. Ponadto listy rozwijanej "Widoki" w tagu inteligentnego widoku logowania — która początkowo jest wyświetlana tylko `AnonymousTemplate` i `LoggedInTemplate` — obejmuje teraz także dodano RoleGroups.

Upravit kolekci RoleGroups, tak, aby użytkownicy w roli nadzorców są wyświetlane instrukcje na temat edytowania kont użytkowników, gdy użytkownicy w roli administratora przedstawiono instrukcje dotyczące edytowania i usuwania. Po wprowadzeniu tych zmian, oznaczeniu deklaracyjnym widoku usługi logowania powinien wyglądać podobnie do poniższej.

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

Po wprowadzeniu tych zmian, strony i można go znaleźć za pośrednictwem przeglądarki. Najpierw należy odwiedzić stronę jako użytkownik anonimowy. Powinien być wyświetlany komunikat, "użytkownik nie jest zalogowany do systemu. W związku z tym użytkownik nie może edytować lub usunąć informacje o użytkowniku." Następnie zaloguj się jako uwierzytelnionego użytkownika, ale taki, który nie jest ani w roli nadzorców ani administratorów. Teraz powinien zostać wyświetlony komunikat "nie jesteś członkiem ról nadzorców lub Administratorzy. W związku z tym użytkownik nie może edytować lub usunąć informacje o użytkowniku."

Następnie zaloguj się jako użytkownik będący członkiem roli nadzorców. Teraz powinien zostać wyświetlony nadzorców pełnionymi komunikatu (patrz rysunek 9). A jeśli logujesz się jako użytkownik w administratorów roli Administratorzy pełnionymi powinien zostać wyświetlony komunikat (zobacz rysunek 10).


[![Bruce jest wyświetlany komunikat pełnionymi nadzorców](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**Rysunek 9**: Bruce jest wyświetlany komunikat pełnionymi nadzorców ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image27.png))


[![Tito jest wyświetlany komunikat specyficzne dla ról administratorów](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**Na rysunku nr 10**: Tito jest wyświetlany komunikat specyficzne dla ról administratorów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image30.png))


Zrzuty ekranu w rysunku 9 i 10 show widoku logowania renderuje tylko jeden szablon, nawet wtedy, gdy zastosowanie wielu szablonów. Bruce i Tito są rejestrowane w użytkowników, ale widoku logowania powoduje wyświetlenie tylko pasujących RoleGroup i nie `LoggedInTemplate`. Ponadto Tito należy do administratorów i nadzorców ról, ale kontrolki widoku logowania powoduje wyświetlenie szablonu specyficzne dla ról administratorów zamiast nadzorców jeden.

Rysunek 11 pokazano przepływu pracy, wykorzystywane przez kontrolki widoku logowania do określenia szablonu do renderowania. Należy pamiętać, że jeśli istnieje więcej niż jeden RoleGroup określona, szablon widoku logowania powoduje wyświetlenie *pierwszy* RoleGroup, który jest zgodny. Innymi słowy w przypadku możemy umieścił RoleGroup nadzorców jako pierwszy RoleGroup i Administratorzy jako drugi, następnie po Tito odwiedzi tę stronę on widział komunikat nadzorców.


[![Określanie szablonu do renderowania kontrolki widoku logowania przepływ pracy](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**Rysunek 11**: przepływ pracy Określanie co szablon do renderowania kontrolki widoku logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Programowe ograniczanie funkcji

Podczas gdy kontrolki widoku logowania wyświetla różne instrukcje w zależności od roli użytkownika, odwiedzając stronę, przyciski Edytuj i Anuluj są widoczne dla wszystkich. Musimy programowe ukrywanie przyciski edytowania i usuwania anonimowe gości i użytkowników, którzy znajdują się w roli nadzorców ani administratorów. Potrzebujemy ukryć przycisk usuwania dla wszystkich użytkowników, którzy nie jest administratorem. W tym celu napiszemy ilość kodu, który programowo odwołuje się do edycji i usuwania LinkButtons i zestawów CommandField ich `Visible` właściwości `False`, jeśli to konieczne.

Najprostszym sposobem programowo odwołują się do formantów w CommandField jest najpierw dokonać jego konwersji do szablonu. W tym celu kliknij link "Edytuj kolumny" w tagu inteligentnego GridView wybierać CommandField listę bieżących pól i kliknij link "Konwertuj to pole na TemplateField". To jest przekształcany CommandField TemplateField z `ItemTemplate` i `EditItemTemplate`. `ItemTemplate` Zawiera edycji i usuwania LinkButtons podczas `EditItemTemplate` przechowuje aktualizacji i anulować LinkButtons.


[![Konwertowanie CommandField TemplateField](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**Rysunek 12**: Konwertuj CommandField do TemplateField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image36.png))


Aktualizowanie edycji i usuwania LinkButtons w `ItemTemplate`, ustawiając ich `ID` właściwości do wartości `EditButton` i `DeleteButton`, odpowiednio.

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

Zawsze, gdy powiązania danych z widoku GridView, widoku GridView wylicza rekordów w jego `DataSource` właściwości i generuje odpowiedni `GridViewRow` obiektu. Ponieważ do każdego `GridViewRow` obiekt zostanie utworzony, `RowCreated` jest wyzwalane zdarzenie. Aby ukryć przyciski edytowania i usuwania dla nieautoryzowanych użytkowników, należy utworzyć program obsługi zdarzeń dla tego zdarzenia i programowo odwoływać się do edycji i usuwania LinkButtons ustawienie ich `Visible` właściwości odpowiednio.

Utwórz procedurę obsługi zdarzeń `RowCreated` zdarzeń, a następnie dodaj następujący kod:

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

Należy pamiętać, że `RowCreated` zdarzenie jest generowane dla *wszystkich* GridView wierszy, w tym nagłówek, stopka, pager interfejsu i tak dalej. Chcemy programowo odwołania edytowanie i usuwanie LinkButtons, jeśli firma Microsoft mają do czynienia z wierszem danych, nie w trybie edycji (ponieważ jest to wiersz w trybie edycji zawiera przyciski aktualizacji i Anuluj zamiast edytowania i usuwania). To sprawdzenie odbywa się przez `If` instrukcji.

Jeśli firma Microsoft mają do czynienia z wierszem danych, który nie jest w trybie edycji, edytowanie i usuwanie LinkButtons do których istnieją odwołania i ich `Visible` właściwości jest ustawiona na podstawie wartości logiczne, zwracany przez `User` obiektu `IsInRole(roleName)` metody. `User` Obiekt odwołuje się do jednostki, utworzone przez `RoleManagerModule`; w związku z tym, `IsInRole(roleName)` metoda używa interfejsu API ról, aby ustalić, czy bieżący użytkownik należy do *roleName*.

> [!NOTE]
> Mogliśmy użyć klasy ról bezpośrednio, zastępując wywołanie `User.IsInRole(roleName)` wywołaniem [ `Roles.IsUserInRole(roleName)` metoda](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). I chcę korzystać z głównym przedmiotem `IsInRole(roleName)` metody w tym przykładzie ponieważ jest bardziej wydajne niż bezpośrednio za pomocą interfejsu API ról. Wcześniej w tym samouczku został skonfigurowany do roli menedżera role użytkownika w pliku cookie w pamięci podręcznej. To dane pliku cookie z pamięci podręcznej tylko jest wykorzystywany podczas podmiotu zabezpieczeń `IsInRole(roleName)` wywoływana jest metoda; bezpośrednie wywołania interfejsu API ról zawsze obejmują podróż magazynu ról. Nawet jeśli role nie są buforowane w pliku cookie, wywołanie głównym przedmiotem `IsInRole(roleName)` metoda jest zazwyczaj bardziej wydajne, ponieważ gdy jest wywoływana, aby po raz pierwszy dla żądania buforuje wyniki. Interfejs API ról, z drugiej strony, nie należy stosować buforowania. Ponieważ `RowCreated` zdarzenie jest uruchamiane jeden raz dla każdego wiersza w widoku GridView, za pomocą `User.IsInRole(roleName)` obejmuje tylko jeden podróży do magazynu ról, natomiast `Roles.IsUserInRole(roleName)` wymaga *N* podróży, gdzie *N* jest Liczba kont użytkowników, wyświetlane w siatce.


Przycisk Edytuj `Visible` właściwość jest ustawiona na `True` Jeśli użytkownik odwiedzający na tej stronie znajduje się w roli administratorów lub nadzorców; w przeciwnym razie ustawiana jest na `False`. Przycisk Usuń `Visible` właściwość jest ustawiona na `True` tylko wtedy, gdy użytkownik znajduje się w roli administratora.

Przetestuj tę stronę za pośrednictwem przeglądarki. W przypadku odwiedzenia strony jako użytkownik anonimowy lub użytkownika, który nie jest nadzorcy ani administratorem, CommandField jest pusta. jest nadal istnieje, ale jako alokowania elastycznego srebrny bez Edytuj lub usuń przyciski.

> [!NOTE]
> Istnieje możliwość ukryć CommandField całkowicie podczas bez przełożonego i użytkowników niebędących administratorami odwiedzania strony. Można pozostawić to w charakterze ćwiczenia dla czytnika.


[![Edytuj i usuń przyciski są ukryte Non-Nadzorcom i użytkownicy niebędący administratorami](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**Rysunek 13**: Edytuj i usuń przyciski są ukryte Non-Nadzorcom i użytkownicy niebędący administratorami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image39.png))


Jeśli użytkownik, który należy do roli nadzorców (ale nie do roli Administratorzy) odwiedza, widzi tylko przycisk Edytuj.


[![Przycisk Edytuj jest dostępny do opiekunów, przycisk Usuń jest ukryta](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**Rysunek 14**: podczas edycji przycisk jest dostępny do opiekunów, przycisk Usuń jest ukryty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image42.png))


A Jeśli Administrator odwiedza, ma ona dostęp do przyciski edytowania i usuwania.


[![Edytuj i usuń przyciski są dostępne tylko wtedy dla administratorów](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**Rysunek 15**: Edytuj i usuń przyciski są dostępne tylko wtedy dla administratorów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Krok 3: Stosowanie reguł autoryzacji opartej na rolach do metod i klas

W kroku 2, które firma Microsoft ograniczone edytowanie możliwości użytkowników w rolach udostępniania Nadzorcom i Administratorzy i usuń funkcje tylko dla administratorów. To było wykonywane przez ukrycie elementów interfejsu użytkownika skojarzonego dla nieautoryzowanych użytkowników za pomocą techniki programistyczne. Środki nie gwarantują nieautoryzowany użytkownik będzie mógł wykonania uprzywilejowanych akcji. Może to być elementy interfejsu użytkownika, które są dodawane później lub firma Microsoft nie pamiętam ukryć dla nieautoryzowanych użytkowników. Lub haker może wykryć inny sposób uzyskać strony ASP.NET, które można wykonać żądanej metody.

Prosty sposób, aby upewnić się, że do określonego elementu funkcje nie są dostępne przez nieautoryzowanego użytkownika jest do dekorowania tej klasy lub metody za pomocą [ `PrincipalPermission` atrybutu](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Gdy środowisko uruchomieniowe platformy .NET używa klasy lub wykonuje jeden z jego metod, sprawdza, czy upewnij się, że bieżący kontekst zabezpieczeń ma uprawnienia. `PrincipalPermission` Atrybutu zapewnia mechanizm, za pomocą którego możemy zdefiniować te reguły.

Przy użyciu przyjrzeliśmy się `PrincipalPermission` atrybutu w <a id="_msoanchor_9"> </a> [ *autoryzacja na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczka. W szczególności firma Microsoft pokazano, jak do dekorowania GridView `SelectedIndexChanged` i `RowDeleting` programu obsługi zdarzeń, aby ich może być wykonywane tylko przez funkcje uwierzytelniania użytkowników i Tito, odpowiednio. `PrincipalPermission` Atrybut działa równie dobrze z rolami.

Zobaczmy, jak działają przy użyciu `PrincipalPermission` atrybutu GridView `RowUpdating` i `RowDeleting` programów obsługi zdarzeń do Stanów Zjednoczonych zabraniają wykonywania nie autoryzowanych użytkowników. To wszystko, co należy zrobić, Dodaj odpowiedni atrybut na jego podstawie Każda definicja funkcji:

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

Atrybut `RowUpdating` programu obsługi zdarzeń mówią, tylko użytkownicy w roli administratorów lub nadzorców można wykonać obsługi zdarzeń, gdzie jako atrybut na `RowDeleting` program obsługi zdarzeń limitów wykonywania dla użytkowników w administratorów Rola.


> [!NOTE]
> `PrincipalPermission` Atrybutu jest reprezentowana jako klasa w `System.Security.Permissions` przestrzeni nazw. Pamiętaj dodać `Imports System.Security.Permissions` instrukcji w górnej części pliku klasy związane z kodem można zaimportować tej przestrzeni nazw.


Jeśli jakiś sposób, użytkowników niebędących administratorami podejmuje próbę wykonania `RowDeleting` programu obsługi zdarzeń lub innych kierownik lub użytkowników niebędących administratorami prób wykonania `RowUpdating` programu obsługi zdarzeń środowiska uruchomieniowego .NET zgłosi `SecurityException`.


[![Jeśli kontekst zabezpieczeń nie ma autoryzacji do wykonania metody, jest zgłaszany securityexception —](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**Rysunek 16**: Jeśli kontekst zabezpieczeń nie ma autoryzacji do wykonania metody `SecurityException` zgłaszany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image48.png))


Oprócz stron ASP.NET wiele aplikacji ma architekturę, która obejmuje różne warstwy, takie jak logiki biznesowej i warstwy dostępu do danych. Te warstwy są zwykle implementowane jako bibliotek klas i oferują klasy i metody służące do wykonywania funkcji powiązanych logiki i danych biznesowych. `PrincipalPermission` Atrybut jest przydatne w przypadku stosowania reguł autoryzacji do tych warstw, jak również.

Aby uzyskać więcej informacji na temat korzystania z `PrincipalPermission` atrybutu, aby zdefiniować reguły autoryzacji dla klasy i metody, zobacz [Scott Guthrie](https://weblogs.asp.net/scottgu/)firmy wpis w blogu [Dodawanie reguły autoryzacji w celu biznesowych i danych zapomocąwarstwy`PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Podsumowanie

W tym samouczku zobaczyliśmy, jak określić zgrubnym i szczegółową reguły autoryzacji na podstawie użytkownika ról. ASP. Funkcja autoryzacja adresu URL firmy NET umożliwia developer strony określić, jakie tożsamości są dozwolone lub odmowa dostępu do strony. Jak widzieliśmy w <a id="_msoanchor_10"> </a> [ *autoryzacja na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczków, Autoryzacja adresów URL, zasady mogą być stosowane na podstawie użytkownika przez użytkownika. Również mogą być stosowane na podstawie roli roli, ponieważ widzieliśmy w kroku 1 w tym samouczku.

Mogą być stosowane reguły autoryzacji szczegółową, deklaratywne lub programowo. W kroku 2 przyjrzeliśmy się przy użyciu funkcji RoleGroups kontrolki widoku logowania do renderowania danych wyjściowych inne niż na podstawie ról zaproszonych użytkowników. Również przyjrzeliśmy się sposoby programowo określić, czy użytkownik należy do określonej roli oraz sposobu odpowiednio dostosować funkcjonalność strony.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Dodawanie reguły autoryzacji w celu biznesowej i warstwy danych przy użyciu `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Badanie programu ASP.NET 2.0 na członkostwo, role i profil: Praca z rolami](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Lista pytań zabezpieczeń dla programu ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Dokumentacja techniczna dotycząca `<roleManager>` — Element](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku obejmują Suchi Banerjee i Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](assigning-roles-to-users-vb.md)
