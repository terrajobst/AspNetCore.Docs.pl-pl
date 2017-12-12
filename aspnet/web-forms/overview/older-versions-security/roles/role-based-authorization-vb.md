---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: Autoryzacji opartej na rolach (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "W tym samouczku rozpoczyna się od przyjrzeć się jak framework ról kojarzy ról użytkownika z jego kontekstu zabezpieczeń. Następnie sprawdza, czy sposób stosowania opartej na rolach adresu URL..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 973e5705fc36b13e5e6ec861dd2ca6adfc0f50fe
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="role-based-authorization-vb"></a>Autoryzacji opartej na rolach (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> W tym samouczku rozpoczyna się od przyjrzeć się jak framework ról kojarzy ról użytkownika z jego kontekstu zabezpieczeń. Następnie bada sposób stosowania reguł autoryzacji adresów URL opartej na rolach. Po, że zajmiemy przy użyciu środków deklaratywne i programowych dla Zmienianie wyświetlanych danych oraz funkcje oferowane przez strony platformy ASP.NET.


## <a name="introduction"></a>Wprowadzenie

W <a id="_msoanchor_1"> </a> [ *autoryzacji na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczek widzieliśmy sposób użycia Autoryzacja adresów URL do określenia, jakie użytkownicy można odwiedzić zestaw stron. Z tylko niewielki kod znaczników w `Web.config`, firma Microsoft może nakazać ASP.NET, aby zezwolić tylko uwierzytelnionych użytkowników odwiedzić stronę. Lub firma Microsoft może czy tylko użytkowników Tito i Robert zostały mogą wskazywać, że zezwolono wszystkich uwierzytelnionych użytkowników z wyjątkiem Sam.

Oprócz Autoryzacja adresów URL również analizujemy deklaratywne i programowe techniki kontroli wyświetlanych danych oraz funkcje oferowane przez strony, na podstawie użytkownika, odwiedzając. W szczególności utworzono stronę wymienione zawartość bieżącego katalogu. Każda osoba, która może odwiedź tej strony, ale tylko użytkownicy uwierzytelnieni można wyświetlić zawartość plików i tylko Tito można usunąć plików.

Stosowanie reguły autoryzacji na podstawie użytkownika według mogą rosnąć w nightmare księgowości. Jest więcej utrzymaniu podejście do użycia autoryzacji opartej na rolach. Dobre wieści jest równie pracy narzędzia naszych dyspozycji stosowania reguły autoryzacji oraz z rolami, jak dla konta użytkownika. Reguł autoryzacji adresów URL można określić ról zamiast użytkowników. Kontrolki widoku logowania, która renderuje inny wynik dla użytkowników uwierzytelnionych i anonimowych, można skonfigurować do wyświetlania różnych zawartości na podstawie ról zalogowanego użytkownika. I interfejs API ról zawiera metody określania ról zalogowanego użytkownika.

W tym samouczku rozpoczyna się od przyjrzeć się jak framework ról kojarzy ról użytkownika z jego kontekstu zabezpieczeń. Następnie bada sposób stosowania reguł autoryzacji adresów URL opartej na rolach. Po, że zajmiemy przy użyciu środków deklaratywne i programowych dla Zmienianie wyświetlanych danych oraz funkcje oferowane przez strony platformy ASP.NET. Dzieła!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Opis sposobu role są skojarzone z kontekstu zabezpieczeń użytkownika

Zawsze, gdy żądanie dociera potoku ASP.NET jest skojarzona z kontekstem zabezpieczeń, który zawiera informacje identyfikujące żądającego. Podczas korzystania z uwierzytelniania formularzy, biletu uwierzytelniania jest używany jako token tożsamości. Jak wspomniano w <a id="_msoanchor_2"> </a> [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-vb.md) i <a id="_msoanchor_3"> </a> [ *formularzy Konfiguracja uwierzytelniania i Tematy zaawansowane* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) samouczki, `FormsAuthenticationModule` jest odpowiedzialny za określenie tożsamość podmiotu żądającego, co jest podczas [ `AuthenticateRequest` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx).

Jeśli zostanie znaleziony prawidłowy, wygasł biletu, `FormsAuthenticationModule` dekoduje go ustalić tożsamości obiektu żądającego. Tworzy nowy `GenericPrincipal` obiektów i przypisuje do `HttpContext.User` obiektu. Celem podmiot zabezpieczeń, takich jak `GenericPrincipal`, jest zidentyfikowanie nazwę uwierzytelnionego użytkownika i co należy ona do ról. Ten cel jest manipulacji fakt, że wszystkie obiekty główne ma `Identity` właściwości i `IsInRole(roleName)` metody. `FormsAuthenticationModule`, Jednak nie jest zainteresowana rejestrowania informacji o roli i `GenericPrincipal` tworzy w obiekcie nie określono żadnych ról.

Jeśli jest włączona w ramach ról, [ `RoleManagerModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.rolemanagermodule.aspx) moduł HTTP etapami po `FormsAuthenticationModule` i określa role uwierzytelnionego użytkownika podczas [ `PostAuthenticateRequest` zdarzenia](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.postauthenticaterequest.aspx), który generowane po `AuthenticateRequest` zdarzeń. Jeśli żądanie pochodzi z uwierzytelnionego użytkownika `RoleManagerModule` zastępuje `GenericPrincipal` obiekt utworzony przez `FormsAuthenticationModule` i zastępuje go tekstem [ `RolePrincipal` obiektu](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.aspx). `RolePrincipal` Klasa korzysta z interfejsu API ról w celu określenia role, jakie użytkownik należy do.

Rysunek 1 przedstawia przepływu pracy potoku ASP.NET, korzystając z uwierzytelniania formularzy i framework ról. `FormsAuthenticationModule` Wykonuje najpierw identyfikuje użytkownika za pośrednictwem jej biletu uwierzytelniania i tworzy nowy `GenericPrincipal` obiektu. Następnie `RoleManagerModule` etapami i zastępuje `GenericPrincipal` obiekt z `RolePrincipal` obiektu.

Jeśli użytkownik anonimowy odwiedzi witrynę, ani `FormsAuthenticationModule` ani `RoleManagerModule` tworzy obiekt główny.


[![Zdarzenia potoku platformy ASP.NET dla uwierzytelnionego użytkownika podczas korzystania z uwierzytelniania formularzy i Framework ról](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**Rysunek 1**: zdarzenia potoku platformy ASP.NET dla uwierzytelnienia użytkownika podczas przy użyciu uwierzytelniania formularzy i Framework ról ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Informacje o rolach w pliku Cookie do buforowania

`RolePrincipal` Obiektu `IsInRole(roleName)` wywołania metody `Roles`.`GetRolesForUser` Aby uzyskać role dla użytkownika w celu ustalenia, czy użytkownik jest członkiem *roleName*. Korzystając z `SqlRoleProvider`, powoduje to zapytanie do bazy danych magazynu roli. Korzystając z reguł autoryzacji adresów URL opartej na rolach `RolePrincipal`w `IsInRole` metoda zostanie wywołana na każde żądanie do strony, która jest chroniona przez reguł autoryzacji adresów URL opartej na rolach. Zamiast do wyszukiwania informacji roli w bazie danych na każde żądanie `Roles` framework zawiera opcję buforowania ról użytkownika w pliku cookie.

Jeśli w ramach ról jest skonfigurowany do pamięci podręcznej ról użytkownika w pliku cookie, `RoleManagerModule` tworzy plik cookie podczas potoku ASP.NET [ `EndRequest` zdarzeń](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.endrequest.aspx). Ten plik cookie jest używany w kolejnych żądań w `PostAuthenticateRequest`, czyli gdy `RolePrincipal` tworzony jest obiekt. Jeśli plik cookie jest prawidłowy i nie wygasł, dane w pliku cookie jest analizowana i używany do wypełnienia ról użytkownika, oszczędzając `RolePrincipal` z konieczności wywoływania `Roles` klasę, aby określić ról użytkownika. Rysunek 2 przedstawia ten przepływ pracy.


[![Informacje o rolach użytkownika mogą być przechowywane w pliku Cookie w celu zwiększenia wydajności](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**Rysunek 2**: Użytkownik roli informacje mogą być przechowywane w pliku Cookie do poprawiania wydajności ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image6.png))


Domyślnie mechanizm roli pamięci podręcznej plików cookie jest wyłączona. Można ją włączyć za pomocą `<roleManager>`; znaczników konfiguracji w `Web.config`. Omówiono przy użyciu [ `<roleManager>` elementu](https://msdn.microsoft.com/en-us/library/ms164660.aspx) Aby określić dostawców ról w <a id="_msoanchor_4"> </a> [ *tworzenie i zarządzanie rolami* ](creating-and-managing-roles-vb.md) samouczka Dlatego użytkownik ma już ten element w aplikacji `Web.config` pliku. Ustawienia pliku cookie pamięci podręcznej roli są określane jako atrybuty `<roleManager>`; element i są podsumowane w tabeli 1.

> [!NOTE]
> Ustawienia konfiguracji wymienione w tabeli 1 określić właściwości wynikowy cookie roli w pamięci podręcznej. Aby uzyskać więcej informacji na pliki cookie, jak działają i ich różnych właściwości odczytu [w tym samouczku plików cookie](http://www.quirksmode.org/js/cookies.html).


| **Właściwość** | **Opis** |
| --- | --- |
| `cacheRolesInCookie` | Wartość logiczna wskazująca, czy jest używane buforowanie plików cookie. Domyślnie `false`. |
| `cookieName` | Nazwa pliku cookie z pamięci podręcznej roli. Wartość domyślna to ". ASPXROLES". |
| `cookiePath` | Ścieżka do pliku cookie nazwy ról. Atrybut ścieżki umożliwia dewelopera ograniczyć zakres pliku cookie do hierarchii określonego katalogu. Wartość domyślna to "/", który informuje przeglądarkę przesyłają żądanie do domeny pliku cookie biletu uwierzytelniania. |
| `cookieProtection` | Wskazuje, jaki techniki służą do ochrony plików cookie z pamięci podręcznej roli. Dopuszczalne wartości to: `All` (domyślnie); `Encryption`; `None`; i `Validation`. Odwołaj się do kroku 3 <a id="_anchor_5"> </a> [ *konfiguracji uwierzytelniania formularzy i Tematy zaawansowane* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) samouczka, aby uzyskać więcej informacji na temat poziomów ochrony. |
| `cookieRequireSSL` | Wartość logiczna wskazująca, czy do przesyłania pliku cookie uwierzytelniania jest wymagane połączenie SSL. Wartość domyślna to `false`. |
| `cookieSlidingExpiration` | Wartość logiczna wskazująca, czy limit czasu plików cookie jest resetowany zawsze użytkownik odwiedza witrynę podczas jednej sesji. Wartość domyślna to `false`. Ta wartość jest tylko istotne podczas `createPersistentCookie` ma ustawioną wartość `true`. |
| `cookieTimeout` | Określa czas w minutach, po którym wygasa plik cookie biletu uwierzytelniania. Wartość domyślna to `30`. Ta wartość jest tylko istotne podczas `createPersistentCookie` ma ustawioną wartość `true`. |
| `createPersistentCookie` | Wartość logiczna określająca, czy plik cookie z pamięci podręcznej roli jest plik cookie sesji lub trwały plik cookie. Jeśli `false` (ustawienie domyślne), używany jest plik cookie sesji, które są usuwane po zamknięciu przeglądarki. Jeśli `true`, trwały plik cookie jest używany; wygaśnie `cookieTimeout` liczba minut po jego utworzeniu lub po poprzednim wizycie klienta, w zależności od wartości `cookieSlidingExpiration`. |
| `domain` | Określa wartość domeny pliku cookie. Wartość domyślna to pusty ciąg, który powoduje, że przeglądarka Użyj domeny, z którego zostało wydane (na przykład www.yourdomain.com). W takim przypadku plik cookie będzie **nie** wysłania w przypadku wprowadzania żądań poddomen, takich jak admin.yourdomain.com. Jeśli chcesz, aby plik cookie do przekazania do wszystkich domen podrzędnych musisz dostosować `domain` atrybutu ustawieniem dla niego "twoja_domena.com". |
| `maxCachedResults` | Określa maksymalną liczbę nazw ról, które są buforowane w pliku cookie. Wartość domyślna to 25. `RoleManagerModule` Nie tworzy plik cookie dla użytkowników, którzy należą do więcej niż `maxCachedResults` ról. W rezultacie `RolePrincipal` obiektu `IsInRole` metoda użyje `Roles` klasę, aby określić ról użytkownika. Powód `maxCachedResults` istnieje ponieważ wielu agentów użytkownika nie zezwalają na pliki cookie jest większy niż 4096 bajtów. Dlatego ten limit jest przeznaczona do zmniejszają prawdopodobieństwo występowania przekracza ten limit rozmiaru. Jeśli masz nazw ról bardzo długie, warto rozważ określenie mniejszej `maxCachedResults` wartości; contrariwise, jeśli rola bardzo krótkie nazwy, prawdopodobnie wzrasta tej wartości. |

**Tabela 1**: opcje konfiguracji roli pamięci podręcznej plików Cookie

Skonfigurujmy naszej aplikacji na używanie plików cookie z pamięci podręcznej trwałe roli. Aby to zrobić, należy zaktualizować `<roleManager>` element `Web.config` uwzględnienie następujących atrybutów związanych z pliku cookie:

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

Zaktualizowano `<roleManager>`; element przez dodanie trzy atrybuty: `cacheRolesInCookie`, `createPersistentCookie`, i `cookieProtection`. Przez ustawienie `cacheRolesInCookie` do `true`, `RoleManagerModule` będą teraz automatycznie buforowane ról użytkownika w pliku cookie, zamiast do wyszukiwania informacji o rolach użytkownika na każde żądanie. Jawnie ustawiona `createPersistentCookie` i `cookieProtection` atrybuty do `false` i `All`odpowiednio. Z technicznego punktu widzenia nie musisz określić wartości tych atrybutów, ponieważ właśnie je przypisane do wartości domyślnych, ale I umieść je tutaj być jawnie wyczyścić trwałe pliki cookie nie jest używany i czy plik cookie jest zaszyfrowany i sprawdzić ich poprawności.

To wszystko jest do niego! Odtąd w ramach ról będą buforowane ról użytkowników w plikach cookie. Jeśli przeglądarka obsługuje pliki cookie lub jeśli ich pliki cookie są usuwane lub zgubione, w dowolny sposób, jest nie transakcji big — `RolePrincipal` po prostu użyje obiektu `Roles` klasy w przypadku pliki cookie nie (lub jednego z nieprawidłowymi lub wygasłymi) jest dostępna.

> [!NOTE]
> Wzorce firmy Microsoft &amp; grupy rozwiązań odradza używania plików cookie z pamięci podręcznej roli trwałego. Ponieważ posiadania pliku cookie z pamięci podręcznej roli jest wystarczające, aby udowodnić członkostwo roli, jeśli haker jakiś sposób uzyskać dostęp do pliku cookie prawidłowego użytkownika on personifikacji tego użytkownika. Prawdopodobieństwo sytuacja zwiększa, jeśli plik cookie jest utrwalona w przeglądarce użytkownika. Aby uzyskać więcej informacji na ten zalecana ze względów bezpieczeństwa, jak również inne problemy z zabezpieczeniami, zapoznaj się [listy pytanie zabezpieczeń dla programu ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Krok 1: Definiowanie reguł autoryzacji opartej na rolach adresów URL

Zgodnie z opisem w <a id="_msoanchor_6"> </a> [ *autoryzacji na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczek, Autoryzacja adresów URL oferuje sposób ograniczyć dostęp do zestawu stron przez użytkownika lub roli roli podstawy. Reguł autoryzacji adresów URL są zapisane w `Web.config` przy użyciu [ `<authorization>` elementu](https://msdn.microsoft.com/en-us/library/8d82143t.aspx) z `<allow>` i `<deny>` elementy podrzędne. Oprócz reguł autoryzacji użytkownika związane z opisem w poprzedniej samouczki każdego `<allow>` i `<deny>` elementu podrzędnego mogą również obejmować:

- Określonej roli
- Rozdzielana przecinkami lista ról

Na przykład reguł autoryzacji adresów URL udzielić dostępu do tych użytkowników pełniących role administratorów i kontrolerów, ale odmowa dostępu do wszystkich innych użytkowników:

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

`<allow>` Elementu w znaczniku powyżej stany możliwość role administratorów i nadzorców; `<deny>`; element informuje, że *wszystkie* użytkownikowi w przypadku odmowy.

Skonfigurujmy naszą aplikację, aby `ManageRoles.aspx`, `UsersAndRoles.aspx`, i `CreateUserWizardWithRoles.aspx` strony są tylko dostępny dla użytkowników w roli administratora podczas `RoleBasedAuthorization.aspx` strona pozostaje dostępna dla wszystkich odwiedzających.

Aby to zrobić, Rozpocznij od dodania `Web.config` pliku `Roles` folderu.


[![Dodanie pliku Web.config w katalogu ról](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**Rysunek 3**: Dodaj `Web.config` pliku `Roles` katalogu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image9.png))


Następnie dodaj następujący znacznik konfiguracji `Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

`<authorization>` Element `<system.web>` sekcji wskazuje, że tylko użytkownicy należący do roli Administratorzy mogą uzyskać dostęp do zasobów ASP.NET `Roles` katalogu. `<location>` Element definiuje alternatywny zestaw reguł autoryzacji adresów URL dla `RoleBasedAuthorization.aspx` stronie, co pozwoli wszystkich użytkowników odwiedzić stronę.

Po zapisaniu zmian do `Web.config`, zaloguj się jako użytkownik, który nie znajduje się w roli administratora, a następnie spróbuj odwiedź jedną chronionych stron. `UrlAuthorizationModule` Wykryje, że nie masz uprawnień do można znaleźć żądanego zasobu; w rezultacie `FormsAuthenticationModule` nastąpi przekierowanie do strony logowania. Strona logowania następnie nastąpi przekierowanie do `UnauthorizedAccess.aspx` strony (patrz rysunek 4). Końcowe przekierowanie ze strony logowania do `UnauthorizedAccess.aspx` występuje z powodu kodu dodaliśmy do strony logowania w kroku 2 <a id="_msoanchor_7"> </a> [ *autoryzacji na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczka. W szczególności, strony logowania automatyczne przekierowanie każdy użytkownik uwierzytelniony do `UnauthorizedAccess.aspx` Jeśli ciąg zapytania zawiera `ReturnUrl` parametr, jako parametr wskazuje dotarły użytkownika na stronie logowania po próbie wyświetlenia strony, nie był uprawnień do wyświetlania.


[![Tylko użytkownicy należący do roli Administratorzy programu można wyświetlić strony chronione](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**Rysunek 4**: tylko użytkownicy należący do roli Administratorzy programu można wyświetlić strony chronione ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image12.png))


Wyloguj się, a następnie zaloguj się jako użytkownik, który należy do roli administratorów. Należy teraz możliwość wyświetlania trzy strony chronione.


[![Tito mogą odwiedzać UsersAndRoles.aspx strony ponieważ jest on w roli administratora](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**Rysunek 5**: Tito mogą odwiedzać `UsersAndRoles.aspx` strony ponieważ jest on w roli administratora ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> Podczas określania reguł autoryzacji adresów URL — dla ról lub użytkowników — należy pamiętać, że zasady są przeanalizowane pojedynczo, od góry w dół. Jak dopasowania zostanie znaleziony, użytkownik jest udzielono lub odmówiono dostępu, w zależności od if dopasowania został znaleziony w `<allow>` lub `<deny>` elementu. **Jeśli nie znaleziono, użytkownik otrzymuje dostęp.** W związku z tym, jeśli chcesz ograniczyć dostęp do co najmniej jednego konta użytkownika, konieczne jest użycie `<deny>` element jako ostatni element w konfiguracji autoryzacji adresu URL. **Jeśli nie ma reguł autoryzacji adresów URL**`<deny>`**elementu, wszyscy użytkownicy będą mieć dostęp.** Aby uzyskać dokładniejsze omówienie na jak są analizowane reguł autoryzacji adresów URL, odwołaj się do "przyjrzeć się jak `UrlAuthorizationModule` przy użyciu reguł autoryzacji do udzielania lub odmawiania dostępu" sekcji <a id="_msoanchor_8"> </a> [  *Autoryzacji na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczka.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Krok 2: Ograniczanie funkcjonalności na podstawie ról aktualnie zalogowanego użytkownika

Adres URL autoryzacji sprawia, że zasady go łatwo określić grubą autoryzacji tego stanu jakie tożsamości są dozwolone i te, które są zabronione wyświetlanie określonej strony (lub wszystkich stron w folderze i jego podfolderach). Jednak w niektórych przypadkach może chcemy zezwala wszystkim użytkownikom, odwiedź stronę, ale ograniczenia funkcji strony na podstawie ról zaproszonych użytkowników. Może to powodować Pokazywanie lub ukrywanie danych na podstawie roli użytkownika lub oferowanie dodatkowych funkcji do użytkowników, którzy należą do określonej roli.

Takie zasady autoryzacji opartej na rolach poprawnie ziarna można zaimplementować deklaratywnie lub programowo (albo przez kombinację obu). W następnej sekcji zostanie wyświetlone implementowania autoryzacji deklaratywne ziarna poprawnie za pomocą formantu LoginView. Następujące, przeanalizujemy programowe technik. Zanim można przyjrzymy stosowania reguły autoryzacji ziarna poprawnie, jednak najpierw należy utworzyć strony, których funkcji zależy od roli użytkownika, odwiedzając go.

Utwórzmy strona, która zawiera listę wszystkich kont użytkowników w systemie w widoku GridView. Widoku GridView będzie zawierać nazwy każdego użytkownika, adres e-mail, datę ostatniego logowania i komentarzy dotyczących użytkownika. Oprócz wyświetlania informacji o poszczególnych użytkowników, widoku GridView będzie zawierać edycji i Usuń możliwości. Firma Microsoft będzie początkowo utworzyć tę stronę w trybie Edytuj i usuń funkcje dostępne dla wszystkich użytkowników. Jak włączyć lub wyłączyć te funkcje na podstawie roli odwiedzającego użytkownika zostanie wyświetlone w sekcji "Za pomocą formantu LoginView" i "Programowo Ograniczanie funkcjonalności".

> [!NOTE]
> Strony ASP.NET, który mamy kompilacji używa kontrolki widoku siatki, aby wyświetlić konta użytkowników. Od tego samouczka, który seria skupia się na uwierzytelnianie formularzy, autoryzacji, kont użytkowników i role nie chcę spędzają zbyt dużo czasu dyskutować przebiega kontrolki widoku siatki. Ten samouczek zawiera określone instrukcje krok po kroku dotyczące konfigurowania tej strony, natomiast nie delve do szczegółów Dlaczego wprowadzono niektórych opcji lub właściwości określonym wpływ ma na renderowanych danych wyjściowych. Do zbadania kontrolki GridView, zapoznaj się z mojej  *[Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)*  samouczka serii.


Uruchamianie przez otwarcie `RoleBasedAuthorization.aspx` strony `Roles` folderu. Przeciągnij element GridView z stronę do projektanta i zestaw jej `ID` do `UserGrid`. Za chwilę możemy napisać kod, który wywołuje `Membership`.`GetAllUsers` Metoda i wiąże powstałe w ten sposób `MembershipUserCollection` obiekt do widoku GridView. `MembershipUserCollection` Zawiera `MembershipUser` obiekt dla każdego konta użytkownika w systemie; `MembershipUser` obiekty mają właściwości, takie jak `UserName`,`Email`,`LastLoginDate` itd.

Zanim firma Microsoft napisać kod, który wiąże konta użytkowników do siatki, najpierw zdefiniujmy pola w widoku GridView. W widoku GridView tagu inteligentnego, kliknij łącze "Edytuj kolumny", aby uruchomić okno dialogowe pola (zobacz rysunek 6). W tym miejscu Usuń zaznaczenie pola wyboru "automatycznie wygenerować pola" w lewym dolnym rogu. Ponieważ chcemy tego widoku GridView obejmują, edytowanie i usuwanie możliwości, Dodaj CommandField i ustawić jej `ShowEditButton` i `ShowDeleteButton` właściwości na wartość True. Następnie dodaj cztery pola do wyświetlania `UserName`, `Email`, `LastLoginDate`, i `Comment` właściwości. Użyj elementu BoundField dwie właściwości tylko do odczytu (`UserName` i `LastLoginDate`) i TemplateFields dwa pola edycji (`Email` i `Comment`).

Mieć pierwszy wyświetlanej BoundField `UserName` właściwości; ustaw jego `HeaderText` i `DataField` właściwości "Nazwa_użytkownika". To pole nie będzie można edytować, a więc ustaw jego `ReadOnly` właściwości na wartość True. Skonfiguruj `LastLoginDate` elementu BoundField przez ustawienie jej `HeaderText` do "Ostatniego logowania" i jego `DataField` do "LastLoginDate". Teraz należy sformatować dane wyjściowe tego elementu BoundField, tak aby tylko data jest wyświetlana (zamiast Data i godzina). Aby to zrobić, należy ustawić tego elementu BoundField `HtmlEncode` wartość False dla właściwości i jego `DataFormatString` dla właściwości "{0: d}". Również ustawić `ReadOnly` właściwości na wartość True.

Ustaw `HeaderText` właściwości dwóch TemplateFields "E-mail" i "Comment".


[![Można skonfigurować za pomocą okna dialogowego pól pola w widoku GridView](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**Rysunek 6**: widoku GridView może być skonfigurowany za pośrednictwem pola okno dialogowe pola ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image18.png))


Teraz musisz zdefiniować `ItemTemplate` i `EditItemTemplate` "E-mail" i "Comment" TemplateFields. Dodawanie formantu etykiety w sieci Web do każdego z `ItemTemplates` i powiąż ich `Text` właściwości `Email` i `Comment` właściwości, odpowiednio.

Dla TemplateField "E-mail" Dodaj pole tekstowe o nazwie `Email` do jego `EditItemTemplate` i powiąż jego `Text` właściwości `Email` właściwości przy użyciu dwukierunkowego wiązania z danymi. Dodaj RequiredFieldValidator i RegularExpressionValidator do `EditItemTemplate` do upewnij się, że obiekt odwiedzający edycji właściwości wiadomości E-mail ma prawidłowy adres e-mail. TemplateField "Comment" można dodać w wielowierszowym polu tekstowym o nazwie `Comment` do jego `EditItemTemplate`. Ustaw pole tekstowe `Columns` i `Rows` właściwości do 40 i 4, odpowiednio, a następnie powiązać jej `Text` właściwości `Comment` właściwości przy użyciu dwukierunkowego wiązania z danymi.

Po skonfigurowaniu tych TemplateFields, ich deklaratywne znaczników powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

Podczas edytowania lub usuwania konta użytkownika, musimy wiedzieć, że użytkownik `UserName` wartości właściwości. Ustaw w widoku GridView `DataKeyNames` dla właściwości "Nazwa_użytkownika", aby te informacje są dostępne w widoku GridView `DataKeys` kolekcji.

Na koniec Dodaj formant ValidationSummary do strony i ustaw jej `ShowMessageBox` właściwości na wartość True i jego `ShowSummary` wartość False dla właściwości. Przy użyciu tych ustawień ValidationSummary wyświetli alert po stronie klienta, jeśli użytkownik próbuje edytować konto użytkownika z brakujący lub nieprawidłowy adres e-mail.

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

Firma Microsoft została zakończona pomyślnie deklaratywne znacznika tej strony. Nasze następne zadanie to powiązać zestaw kont użytkowników z widoku GridView. Dodaj metodę o nazwie `BindUserGrid` do `RoleBasedAuthorization.aspx` strony klasę kodu, który wiąże `MembershipUserCollection` zwrócony przez `Membership.GetAllUsers` do `UserGrid` widoku GridView. Wywołanie tej metody z `Page_Load` obsługi zdarzeń pierwszej wizyty strony.

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

Z tego kodu w miejscu odwiedź stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 7, powinien zostać wyświetlony element GridView wyświetlania informacji na temat każdego konta użytkownika w systemie.


[![Element UserGrid GridView zawiera informacje dotyczące każdego użytkownika w systemie](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**Rysunek 7**: `UserGrid` GridView Wyświetla informacje o każdego użytkownika w systemie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> `UserGrid` Widoku GridView zawiera listę wszystkich użytkowników w interfejsie niestronicowanej. Ten interfejs prosta siatka nie jest przeznaczony do scenariuszy w przypadku, gdy istnieje kilka dozen lub większej liczby użytkowników. Jedną z opcji jest skonfigurowanie widoku GridView, aby włączyć stronicowanie. `Membership.GetAllUsers` Metoda ma dwa przeciążenia: taki, który akceptuje Brak parametrów wejściowych i zwraca wszystkich użytkowników i przyjmuje wartości całkowite indeksu strony i rozmiaru strony i zwraca tylko określony podzbiór użytkowników. Drugi przeciążenia może służyć do wydajniej strony przez użytkowników, ponieważ zwraca tylko dokładne podzbiór kont użytkowników zamiast *wszystkie* z nich. Jeśli masz tysiące kont użytkowników, można wziąć pod uwagę interfejs na podstawie filtru, który pokazuje tylko tych użytkowników, których nazw rozpoczyna się od znaku wybranych dla wystąpienia. [ `Membership.FindUsersByName` Metody](https://msdn.microsoft.com/en-us/library/system.web.security.membership.findusersbyname.aspx) nadaje się do tworzenia interfejsu użytkownika na podstawie filtru. Zajmiemy tworzenia takiego interfejsu w przyszłości samouczka.


Formant widoku GridView oferuje wbudowane edytowanie i usuwanie pomocy technicznej, gdy formant jest powiązany do kontroli źródła danych poprawnie skonfigurowane, takie jak SqlDataSource lub ObjectDataSource. `UserGrid` GridView, ale zawiera programowo powiązane dane; w związku z tym należy napisać kod, aby wykonać te dwa zadania. W szczególności, musimy tworzenie obsługi zdarzeń dla w widoku GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, i `RowDeleting` zdarzenia, które są uruchamiane, gdy użytkownik kliknie w widoku GridView edytować, Anuluj, Update, lub usuń przyciski.

Rozpocznij od utworzenia obsługi zdarzeń dla w widoku GridView `RowEditing`, `RowCancelingEdit`, i `RowUpdating` zdarzenia, a następnie dodaj następujący kod:

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

`RowEditing` i `RowCancelingEdit` procedury obsługi zdarzeń wystarczy ustawić w widoku GridView `EditIndex` właściwości, a następnie ponownie Utwórz wiązanie konta użytkownika do siatki. Interesujące elementy odbywa się w `RowUpdating` obsługi zdarzeń. Ten program obsługi zdarzeń, który uruchamia się za zapewnienie, że dane są prawidłowe i następnie grabs `UserName` wartości z konta użytkownika edytowanych `DataKeys` kolekcji. `Email` i `Comment` pól tekstowych w dwóch TemplateFields `EditItemTemplate` s następnie programowo odwołuje się. Ich `Text` właściwości zawierają adresu e-mail edytowanych i komentarza.

Aby można było zaktualizować konto użytkownika za pośrednictwem interfejsu API członkostwa, należy najpierw uzyskać informacje o użytkowniku, co możemy zrobić za pomocą wywołania `Membership.GetUser(userName)`. Zwrócona `MembershipUser` obiektu `Email` i `Comment` właściwości następnie są aktualizowane przy użyciu wartości wprowadzane do dwóch pól tekstowych w interfejsie edycji. Na koniec te zmiany są zapisywane w wyniku wywołania [ `Membership.UpdateUser` ](https://msdn.microsoft.com/en-us/library/system.web.security.membership.updateuser.aspx). `RowUpdating` Program obsługi zdarzeń wykonuje przez przywrócenie widoku GridView na interfejsie wstępnie edycji.

Następnie należy utworzyć `RowDeleting` RowDeleting obsługi zdarzeń, a następnie dodaj następujący kod:

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

Powyższe procedury obsługi zdarzeń, który rozpoczyna się od Przechwytywanie `UserName` wartość z zakresu od w widoku GridView `DataKeys` kolekcji; ten `UserName` wartości są następnie przekazywane do klasy członkostwa [ `DeleteUser` — metoda](https://msdn.microsoft.com/en-us/library/system.web.security.membership.deleteuser.aspx). `DeleteUser` Metoda usuwa konto użytkownika z systemu, w tym danych o członkostwie powiązane (takich jak role tego użytkownika należy do). Po usunięciu użytkownika, siatki `EditIndex` ma ustawioną wartość -1 (w przypadku, gdy użytkownik kliknął Delete podczas kolejnego wiersza jest w trybie edycji) i `BindUserGrid` metoda jest wywoływana.

> [!NOTE]
> Przycisk Usuń nie wymaga żadnych sortowania potwierdzenia przez użytkownika przed usunięciem konta użytkownika. I zachęca do dodania jakiegoś potwierdzenie użytkownika, aby zmniejszyć ryzyko konto zostanie przypadkowo usunięte. Jednym z najprostszych sposobów potwierdzenie akcji jest za pomocą okna dialogowego Potwierdź po stronie klienta. Aby uzyskać więcej informacji na temat tej techniki, zobacz [Dodawanie potwierdzenie po stronie klienta podczas usuwania](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Upewnij się, że ta strona działa zgodnie z oczekiwaniami. Można edytować adres e-mail tego użytkownika i komentarza, jak również usunąć dowolnego konta użytkownika. Ponieważ `RoleBasedAuthorization.aspx` strona jest dostępna dla wszystkich użytkowników, każdy użytkownik — nawet anonimowe odwiedzających — odwiedź tej strony i edytować lub usuwać konta użytkowników! Teraz należy zaktualizować tę stronę, tak aby tylko użytkownicy o rolach administratorów i kontrolerów można edytować adres e-mail użytkownika i komentarz, a tylko administratorzy mogą usuwać konta użytkownika.

W sekcji "Za pomocą formantu LoginView" wygląda, aby wyświetlić instrukcje specyficzne dla roli użytkownika za pomocą formantu LoginView. Osoby w roli administratora odwiedza tej strony, możemy wyświetli instrukcje na temat edytowania i usuwania użytkowników. Jeśli użytkownik roli nadzorców osiągnie tę stronę, możemy będą wyświetlane instrukcje na edytowanie użytkowników. A jeśli użytkownik jest anonimowa lub nie jest w roli nadzorców albo Administratorzy, wyświetlany będzie komunikat z informacjami o tym, że nie można edytować ani usunąć informacje o koncie użytkownika. W sekcji "Programowo Ograniczanie funkcjonalności" Firma Microsoft będzie pisania kodu, który programowo pokazuje lub ukrywa edytowanie i usuwanie przycisków na podstawie roli użytkownika.

### <a name="using-the-loginview-control"></a>Za pomocą formantu LoginView

Jak możemy przedstawiono w ciągu ostatnich samouczkach, formantu LoginView przydaje się do wyświetlania różnych interfejsów dla użytkowników uwierzytelnionych i anonimowych, ale formantu LoginView mogą służyć do wyświetlania różnych znaczników na podstawie ról użytkownika. Aby wyświetlić różne instrukcje na podstawie roli użytkownika zaproszonych Użyjmy formantu LoginView.

Start, dodając LoginView powyżej `UserGrid` widoku GridView. Jak mamy już i wspomniano, formantu LoginView ma dwa wbudowane szablony: `AnonymousTemplate` i `LoggedInTemplate`. Wpisz krótką wiadomość w obu tych szablonów, które informuje użytkownika, że nie można edytować ani usunąć informacje o użytkowniku.

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

Oprócz `AnonymousTemplate` i `LoggedInTemplate`, mogą obejmować formantu LoginView *RoleGroups*, które są specyficzne dla roli szablonów. Każdy RoleGroup zawiera tylko jedną właściwość `Roles`, który określa role, jakie dotyczy RoleGroup. `Roles` Właściwość można ustawić jedną rolę (na przykład "Administratorzy") lub rozdzielana przecinkami lista ról (na przykład "Administratorzy, kierownicy").

Aby zarządzać RoleGroups, kliknij łącze "Edytuj RoleGroups" tagów inteligentnych formantu, aby przełączyć się edytora kolekcji RoleGroup. Dodaj dwa nowe RoleGroups. Ustawianie pierwszego RoleGroup `Roles` właściwości, a do "Kierownicy" drugi "Administratorzy".


[![Zarządzaj szablonami specjalne LoginView za pomocą edytora kolekcji RoleGroup](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**Rysunek 8**: Zarządzanie LoginView specjalne szablonów za pomocą edytora kolekcji RoleGroup ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image24.png))


Kliknij przycisk OK, aby zamknąć edytora kolekcji RoleGroup; Spowoduje to zaktualizowanie LoginView deklaratywne znaczników, aby uwzględnić `<RoleGroups>` sekcji z `<asp:RoleGroup>` edytora kolekcji RoleGroup zdefiniowany element podrzędny, dla każdego RoleGroup. Ponadto listy rozwijanej "Widoków" w tagu inteligentnego LoginView — która początkowo wyświetlana po prostu `AnonymousTemplate` i `LoggedInTemplate` — zawiera teraz również dodany RoleGroups.

Edytuj RoleGroups, tak aby użytkownicy w roli nadzorców wyświetlane instrukcje dotyczące sposobu edytować kont użytkowników, gdy użytkownicy w roli administratora są wyświetlane instrukcje dotyczące edytowania i usuwania. Po wprowadzeniu tych zmian, znaczników deklaratywne Twojej LoginView powinien wyglądać podobny do następującego.

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

Po wprowadzeniu tych zmian, strony i można go znaleźć za pośrednictwem przeglądarki. Najpierw należy odwiedzić stronę jako użytkownik anonimowy. Powinien być wyświetlany komunikat, "użytkownik nie jest zalogowany do systemu. W związku z tym nie można edytować ani usunąć informacje o użytkowniku." Następnie zaloguj się jako użytkownik uwierzytelniony, ale taki, który nie jest ani w roli nadzorców ani administratorów. Teraz powinien zostać wyświetlony komunikat "nie jesteś członkiem ról nadzorców lub Administratorzy. W związku z tym nie można edytować ani usunąć informacje o użytkowniku."

Następnie zaloguj się jako użytkownik będący członkiem roli kontrolerów. Teraz powinien zostać wyświetlony nadzorców określonych ról komunikatu (patrz rysunek 9). A jeśli logujesz się jako użytkownik w roli Administratorzy poszczególnych ról powinien zostać wyświetlony komunikat (zobacz rysunek 10) administratorów.


[![Bruce jest wyświetlany komunikat specjalne kontrolerów](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**Rysunek 9**: Bruce jest wyświetlany komunikat specjalne nadzorców ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image27.png))


[![Tito jest wyświetlany komunikat określonych ról administratorów](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**Na rysunku nr 10**: Tito jest wyświetlany komunikat określonych ról administratorów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image30.png))


Zrzuty ekranu w rysunku 9 i 10 Pokaż LoginView renderuje tylko jeden szablon, nawet w przypadku zastosowania wielu szablonów. Bruce i Tito są rejestrowane w użytkowników, ale LoginView renderuje tylko pasujących RoleGroup i nie `LoggedInTemplate`. Ponadto Tito należy do roli administratorów i nadzorców jeszcze formantu LoginView renderuje szablonu określonych ról administratorów zamiast nadzorców jeden.

Rysunek 11 przedstawiono używane przez formantu LoginView, aby określić, którego szablonu do renderowania przepływu pracy. Należy pamiętać, że jeśli istnieje więcej niż jeden określony RoleGroup, renderuje szablonu LoginView *pierwszy* RoleGroup, który jest zgodny. Innymi słowy jeśli mamy umieścił RoleGroup nadzorców jako pierwszy RoleGroup i administratorów jako drugiego, następnie podczas Tito odwiedzin tej strony on zobaczy komunikat kontrolerów.


[![Przepływ pracy formantu LoginView do określenia, jakie szablon do renderowania](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**Rysunek 11**: przepływ pracy określania co szablon do renderowania formantu LoginView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Programowo ograniczenie funkcjonalności

Podczas odwzorowania formantu LoginView wyświetla różne instrukcje dla danej roli użytkownika, odwiedzając stronę, edytować i przyciski Anuluj są widoczne dla wszystkich. Musimy programowane ukrywanie edytowanie i usuwanie przycisków anonimowe gości i użytkowników, którzy są w roli nadzorców ani administratorów. Musimy Ukryj przycisk Usuń dla każdego użytkownika, który nie jest administratorem. Aby to osiągnąć firma Microsoft będzie zapisywać nieco kod odwołujący programowe CommandField edycji i Usuń LinkButtons i zestawy ich `Visible` właściwości `False`, jeśli to konieczne.

Najprostszym sposobem programowo odwołują się do formantów w elemencie CommandField jest najpierw przekonwertuj ją na szablon. Aby to zrobić, kliknij łącze "Edytuj kolumny" w widoku GridView tagów inteligentnych, wybierz z listy pól bieżącego CommandField i kliknij łącze "Konwertuj to pole na pole TemplateField". To włącza CommandField na pole TemplateField z `ItemTemplate` i `EditItemTemplate`. `ItemTemplate` Zawiera edytowanie i usuwanie LinkButtons podczas `EditItemTemplate` przechowuje aktualizacji i anulować LinkButtons.


[![Konwertuj CommandField na pole TemplateField](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**Rysunek 12**: konwertowanie CommandField do TemplateField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image36.png))


Edytowanie i usuwanie LinkButtons w aktualizacji `ItemTemplate`, ustawienie ich `ID` właściwości do wartości `EditButton` i `DeleteButton`odpowiednio.

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

Zawsze, gdy do widoku GridView jest powiązany z danymi, widoku GridView wylicza rekordy w jego `DataSource` właściwości i generuje odpowiadającego `GridViewRow` obiektu. Ponieważ do każdego `GridViewRow` obiekt jest tworzony, `RowCreated` zdarzenie jest wywoływane. Aby ukryć edytowanie i usuwanie przycisków dla nieautoryzowanych użytkowników, musimy Tworzenie procedury obsługi zdarzeń dla tego zdarzenia i programowo odwoływać się do edycji i Usuń LinkButtons ustawienie ich `Visible` właściwości odpowiednio.

Tworzenie procedury obsługi zdarzeń `RowCreated` zdarzeń, a następnie dodaj następujący kod:

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

Należy pamiętać, że `RowCreated` zdarzenie jest generowane dla *wszystkie* wierszy GridView, w tym nagłówek, stopka, interfejsu modułu stronicowania i tak dalej. Chcemy programowo odwołania edytowanie i usuwanie LinkButtons, jeśli firma Microsoft ma do czynienia z wierszem danych w trybie edycji (ponieważ jest w trybie edycji wiersz zawiera przyciski aktualizacji i Anuluj zamiast edytowanie i usuwanie). Ten test jest obsługiwany przez `If` instrukcji.

Jeśli firma Microsoft ma do czynienia z wierszem danych, który nie jest w trybie edycji, edytowanie i usuwanie LinkButtons są przywoływane i ich `Visible` właściwości jest ustawiona na podstawie wartości logiczne zwrócony przez `User` obiektu `IsInRole(roleName)` metody. `User` Odwołuje się do podmiotu zabezpieczeń utworzony przez obiekt `RoleManagerModule`; w rezultacie `IsInRole(roleName)` — metoda używa interfejsu API ról w celu określenia, czy bieżący użytkownik należy do *roleName*.

> [!NOTE]
> Można użyliśmy klasy ról bezpośrednio, zastępując wywołanie `User.IsInRole(roleName)` wywołaniem [ `Roles.IsUserInRole(roleName)` metody](https://msdn.microsoft.com/en-us/library/system.web.security.roles.isuserinrole.aspx). I chcę korzystać z obiekt główny `IsInRole(roleName)` metody w tym przykładzie ponieważ jest bardziej efektywne niż bezpośrednio za pomocą interfejsu API ról. Wcześniej w tym samouczku został skonfigurowany menedżera ról w pamięci podręcznej ról użytkownika w pliku cookie. To dane pliku cookie z pamięci podręcznej tylko jest wykorzystywany podczas podmiotu `IsInRole(roleName)` metoda jest wywoływana; bezpośrednie wywołania interfejsu API ról zawsze obejmują podróży do magazynu ról. Nawet jeśli role nie są buforowane w pliku cookie, obiekt główny wywoływania `IsInRole(roleName)` metoda jest zwykle wydajniejszym, ponieważ gdy jest wywoływana dla pierwszego żądania buforuje wyników. Interfejs API ról, z drugiej strony, nie należy stosować buforowania. Ponieważ `RowCreated` zdarzenie jest wywoływane raz dla każdego wiersza w widoku GridView, za pomocą `User.IsInRole(roleName)` obejmuje tylko jeden dostępu do magazynu ról, podczas gdy `Roles.IsUserInRole(roleName)` wymaga *N* podróży, gdzie *N* jest Liczba kont użytkowników wyświetlane w siatce.


Przycisk Edytuj `Visible` właściwość jest ustawiona na `True` , gdy użytkownik odwiedzający ta strona jest w roli administratorów lub nadzorców; w przeciwnym razie ustawiana jest na `False`. Przycisk Usuń `Visible` właściwość jest ustawiona na `True` tylko wtedy, gdy użytkownik jest w roli administratora.

Przetestuj tę stronę za pośrednictwem przeglądarki. W przypadku odwiedzenia strony jako użytkownik anonimowy lub użytkownik, który nie jest ani kierownik ani Administrator, CommandField jest pusta. ten nadal istnieje, ale jako alokowania srebrny bez Edytuj lub usuń przyciski.

> [!NOTE]
> Istnieje możliwość ukryć CommandField całkowicie po z systemem innym niż przełożonego i inni niż Administrator odwiedzania strony. I pozostaw to wykonywania dla czytnika.


[![Edytowanie i usuwanie przycisków są ukryte Non-zastępczego i użytkownicy niebędący administratorami](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**Rysunek 13**: edytowanie i usuwanie przycisków są ukryte Non-zastępczego i użytkownicy niebędący administratorami ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image39.png))


Jeśli odwiedza użytkownika należącego do roli kierownicy (ale nie do roli administratora), widzi on tylko przycisk Edytuj.


[![Przycisk Edytuj jest dostępny do kontrolerów, przycisk Usuń jest ukryty](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**Rysunek 14**: podczas edytowania przycisk jest dostępny do kontrolerów, jest ukryty przycisk Usuń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image42.png))


A Jeśli Administrator odwiedza, użytkownik ma dostęp do edytowanie i usuwanie przycisków.


[![Edytowanie i usuwanie przycisków są dostępne tylko administratorzy](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**Rysunek 15**: edytowanie i usuwanie przycisków są dostępne tylko wtedy, dla administratorów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Krok 3: Stosowanie reguł autoryzacji opartej na rolach do klasy i metody

W kroku 2, które firma Microsoft ograniczone edytowania możliwości użytkowników w rolach zastępczego i Administratorzy i usuwania możliwości tylko dla administratorów. To osiągnąć ukrywanie elementów interfejsu użytkownika skojarzonego nieautoryzowanych użytkowników przy użyciu techniki programowe. Środki gwarantuje, że nieautoryzowany użytkownik będzie mógł wykonania uprzywilejowanych akcji. Może to być elementy interfejsu użytkownika, które później zostaną dodane lub firma Microsoft nie pamiętasz ukrycia dla nieautoryzowanych użytkowników. Lub haker może wykryć inny sposób uzyskać strony ASP.NET do wykonania żądanej metody.

To prosty sposób sprawdzić, czy z określonym wystąpieniem funkcji nie jest dostępna przez nieautoryzowanego użytkownika do dekoracji tej klasy lub metody za pomocą [ `PrincipalPermission` atrybutu](https://msdn.microsoft.com/en-us/library/system.security.permissions.principalpermissionattribute.aspx). Gdy środowiska uruchomieniowego .NET korzysta z klasy lub wykonuje jeden z jego metod, sprawdza, aby upewnić się, że bieżący kontekst zabezpieczeń ma uprawnienia. `PrincipalPermission` Atrybutu zapewnia mechanizm, za pomocą którego możemy można zdefiniować te reguły.

Analizujemy przy użyciu `PrincipalPermission` atrybutu w <a id="_msoanchor_9"> </a> [ *autoryzacji na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczka. W szczególności widzieliśmy jak do dekoracji w widoku GridView `SelectedIndexChanged` i `RowDeleting` obsługi zdarzeń, aby ich może być wykonywane tylko przez użytkowników uwierzytelnionych i Tito, odpowiednio. `PrincipalPermission` Atrybut równie dobrze działa z rolami.

Umożliwia pokazanie sposobu używania `PrincipalPermission` atrybutu w widoku GridView `RowUpdating` i `RowDeleting` procedury obsługi zdarzeń należy zakazać wykonywanie-autoryzowanych użytkowników. To wszystko, co należy zrobić, dodać odpowiedni atrybut nad każdym definicji funkcji:

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

Atrybut `RowUpdating` obsługi zdarzeń decyduje, tylko użytkownicy o rolach administratorów lub nadzorców można wykonać program obsługi zdarzeń, gdzie jako atrybut na `RowDeleting` obsługi zdarzeń ogranicza wykonanie dla użytkowników w administratorów Rola.


> [!NOTE]
> `PrincipalPermission` Atrybutu jest reprezentowany jako klasa w `System.Security.Permissions` przestrzeni nazw. Pamiętaj dodać `Imports System.Security.Permissions` instrukcji w górnej części pliku CodeBehind klasy do zaimportowania tej przestrzeni nazw.


Jeśli jakiś sposób, bez uprawnień administratora podejmuje próbę wykonania `RowDeleting` obsługi zdarzeń lub z systemem innym niż przełożonego lub z systemem innym niż Administrator próbuje wykonać `RowUpdating` obsługi zdarzeń środowiska uruchomieniowego .NET zostanie podniesiony `SecurityException`.


[![Jeśli kontekst zabezpieczeń nie ma uprawnień do wykonania metody, jest zgłaszany SecurityException](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**Rysunek 16**: Jeśli kontekst zabezpieczeń nie ma uprawnień do wykonania metody `SecurityException` zgłoszono ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](role-based-authorization-vb/_static/image48.png))


Oprócz stron ASP.NET wiele aplikacji ma architekturę, która obejmuje różne warstwy, takie jak logiki biznesowej i warstwy dostępu do danych. Warstwy te zwykle są zaimplementowane jako bibliotek klas i oferują klasy i metody do wykonywania funkcji związanych logikę i dane biznesowe. `PrincipalPermission` Atrybut jest przydatne w przypadku stosowania reguł autoryzacji do także te warstwy.

Aby uzyskać więcej informacji na temat używania `PrincipalPermission` atrybutu definiują reguły autoryzacji na klasy i metody, zapoznaj się [Scott Guthrie](https://weblogs.asp.net/scottgu/)jego wpis w blogu [Dodawanie reguły autoryzacji w celu biznesowych i danych zapomocąwarstw`PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Podsumowanie

W tym samouczku analizujemy sposobu określania grubą i przeprowadzać ziarna reguły autoryzacji na podstawie użytkownika ról. ŚRODOWISKO ASP. Funkcja autoryzacji adresu URL w sieci umożliwia deweloperom strony Określ zezwolono na dostęp lub odmówiono dostępu do strony jakie tożsamości. Jak widzieliśmy w <a id="_msoanchor_10"> </a> [ *autoryzacji na podstawie użytkownika* ](../membership/user-based-authorization-vb.md) samouczek, Autoryzacja adresów URL można zastosować zasady na podstawie użytkownika przez użytkownika. Również mogą być stosowane na podstawie roli roli jako widzieliśmy w kroku 1 w tym samouczku.

Mogą być stosowane reguły autoryzacji ziarna poprawnie, deklaratywnego lub programowo. W kroku 2 analizujemy renderowania różne dane wyjściowe na podstawie ról odwiedzającego użytkownika za pomocą funkcji RoleGroups formantu LoginView. Analizujemy również sposoby programowane wyznaczanie, jeśli użytkownik należy do konkretnej roli oraz sposób odpowiednio zmienić na stronie funkcji.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Dodawanie reguły autoryzacji do działalności biznesowej i warstwy danych przy użyciu`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Badanie ASP.NET 2.0 na członkostwo, role i profilu: Praca z ról](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Lista pytań zabezpieczeń dla programu ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/ms998375.aspx)
- [Dokumentacja techniczna dotycząca `<roleManager>` — Element](https://msdn.microsoft.com/en-us/library/ms164660.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku obejmują Suchi Banerjee i Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](assigning-roles-to-users-vb.md)
