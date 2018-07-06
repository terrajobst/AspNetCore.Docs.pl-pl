---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Tworzenie ról i zarządzanie nimi (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten samouczek analizuje kroki niezbędne do skonfigurowania w ramach ról. Poniżej utworzymy stron sieci web do tworzenia i usuwania ról.
ms.author: aspnetcontent
ms.date: 03/24/2008
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: 84b9624aaae0cc98f1908b4521cee43bce6e856d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806875"
---
<a name="creating-and-managing-roles-c"></a>Tworzenie ról i zarządzanie nimi (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> Ten samouczek analizuje kroki niezbędne do skonfigurowania w ramach ról. Poniżej utworzymy stron sieci web do tworzenia i usuwania ról.


## <a name="introduction"></a>Wprowadzenie

W <a id="_msoanchor_1"> </a> [ *autoryzacja na podstawie użytkownika* ](../membership/user-based-authorization-cs.md) samouczka będziemy przyjrzano się przy użyciu Autoryzacja adresów URL do ograniczenia dostępu w przypadku niektórych użytkowników zestaw stron oraz zbadano deklaracyjne i programowe techniki dostosowywania funkcji strony ASP.NET na podstawie zaproszonych użytkownika. Udzielanie uprawnień dostępu do strony lub funkcji na podstawie użytkownika przez użytkownika, jednak może stać się okropnej konserwacji, w scenariuszach w przypadku, gdy istnieje wiele kont użytkowników, lub gdy uprawnienia użytkowników zmieniają się często. Ilekroć użytkownika zyskuje lub traci autoryzacji do wykonania określonego zadania, administrator musi zaktualizować odpowiednich reguł autoryzacji adresów URL, deklaratywne znaczników i kodu.

Niemniej przykład często pomoże do klasyfikowania użytkowników do grup lub *role* a następnie zastosować uprawnienia na podstawie roli przez rolę. Na przykład większość aplikacji sieci web ma określony zestaw stron lub zadania, które są przeznaczone tylko do użytkowników administracyjnych. Za pomocą technik wyniesione *autoryzacja na podstawie użytkownika* samouczku dodamy odpowiednie reguły autoryzacji adresów URL, deklaratywne znaczników i kodu, aby umożliwić określone konta użytkowników do wykonywania zadań administracyjnych. Ale jeśli dodano nowego administratora lub istniejącego administratora niezbędnych do jej prawa administracyjne odwołany, firma Microsoft musiałaby wróć i aktualizować pliki konfiguracji i stron sieci web. Przy użyciu ról jednak można utworzyć rolę o nazwie Administratorzy i przypisz tych zaufanych użytkowników do roli Administratorzy. Następnie dodamy odpowiednie reguły autoryzacji adresów URL, deklaratywne znaczników i kodu, aby umożliwić roli Administratorzy w celu wykonywania różnych zadań administracyjnych. Dzięki tej infrastruktury w miejscu Dodawanie nowych administratorów do witryny lub usunięcie istniejących jest proste i polega na tym lub usuwanie użytkownika z rolą administratora. Nie konfiguracji, oznaczeniu deklaracyjnym lub zmian w kodzie są niezbędne.

Program ASP.NET oferuje role umożliwiająca Definiowanie ról i skojarzenie ich z kontami użytkowników. Za pomocą framework ról, firma Microsoft można tworzyć i usuwać role należy dodać użytkowników lub usunąć użytkowników z roli, określić zbiór użytkowników, które należą do określonej roli i sprawdzić, czy użytkownik należy do określonej roli. Skonfigurowane w ramach ról firma Microsoft może ograniczyć dostęp do stron, na podstawie roli roli przy użyciu reguł autoryzacji adresów URL i Pokaż lub Ukryj dodatkowe informacje i funkcje na stronie, na podstawie ról aktualnie zalogowanego użytkownika.

Ten samouczek analizuje kroki niezbędne do skonfigurowania w ramach ról. Poniżej utworzymy stron sieci web do tworzenia i usuwania ról. W <a id="_msoanchor_2"> </a> [ *przypisywanie ról do użytkowników* ](assigning-roles-to-users-cs.md) samouczka przyjrzymy się jak dodawać i usuwać użytkowników z ról. A następnie w <a id="_msoanchor_3"> </a> [ *autoryzacji opartej na rolach* ](role-based-authorization-cs.md) samouczek, należy sprawdzić, jak ograniczyć dostęp do stron, na podstawie roli według roli oraz jak dostosować zależności funkcji strony w roli zaproszonych użytkowników. Zaczynajmy!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Dodawanie nowych stron ASP.NET

W tym samouczku i kolejne dwa firma Microsoft będzie mieć badanie różnych funkcji związanych z ról i funkcji. Musimy szereg stron ASP.NET do zaimplementowania tematy dotyczące badania w tych samouczkach. Umożliwia tworzenie tych stron i zaktualizuj mapy witryny.

Rozpocznij od utworzenia nowego folderu w projekcie o nazwie `Roles`. Następnie dodaj cztery nowe strony programu ASP.NET do `Roles` folderze łączenia każdego stronę z `Site.master` strony wzorcowej. Nazwa strony:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

Na tym etapie projektu w Eksploratorze rozwiązań powinny wyglądać podobnie do ekranu zrzut, jak pokazano na rysunku 1.


[![Cztery nowe strony zostały dodane do folderu ról](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Rysunek 1**: cztery nowe strony zostały dodane do `Roles` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image3.png))


Każda strona w tym momencie powinna mieć dwóch kontrolek zawartości, jeden dla każdej strony wzorcowej kontrolek ContentPlaceHolder: `MainContent` i `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Pamiętamy `LoginContent` firmy ContentPlaceHolder domyślne znaczników Wyświetla łącze do logowania się lub wylogowywania z lokacji, w zależności od tego, czy użytkownik jest uwierzytelniony. Obecność `Content2` zawartości kontrolki na stronie ASP.NET, jednak zastępuje znaczników domyślnej strony wzorcowej. Tak jak Omówiliśmy to w <a id="_msoanchor_4"> </a> [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczek, Zastępowanie domyślnego języka znaczników przydaje się na stronach, gdy nie chcemy wyświetlić skojarzone z logowaniem Opcje w lewej kolumnie.

Te cztery stron, chcemy wyświetlić znaczników domyślną stronę wzorcową dla `LoginContent` ContentPlaceHolder. W związku z tym, Usuń oznaczeniu deklaracyjnym dla `Content2` formantu zawartości. Po wykonaniu tej czynności, wszystkich znaczników cztery strony powinna zawierać tylko jeden formant zawartości.

Na koniec zaktualizuj mapy witryny (`Web.sitemap`) do uwzględnienia tych nowych stron sieci web. Dodaj następujący kod XML po `<siteMapNode>` dodaliśmy samouczki członkostwa.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Za pomocą mapy witryny aktualizacji odwiedź witrynę za pośrednictwem przeglądarki. Jak pokazano na rysunku 2, nawigacji po lewej stronie teraz zawiera elementy samouczki ról.


[![Cztery nowe strony zostały dodane do folderu ról](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Rysunek 2**: cztery nowe strony zostały dodane do `Roles` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Krok 2: Określanie i Konfigurowanie dostawcy Framework ról

Jak w ramach członkostwa w ramach ról jest kompilowanych na podstawie modelu dostawców. Zgodnie z opisem w <a id="_msoanchor_5"> </a> [ *podstawy zabezpieczeń i Obsługa platformy ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) samouczek, .NET Framework jest dostarczany z trzech dostawców wbudowanych ról: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), i [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). W tej serii samouczków skupia się na `SqlRoleProvider`, który używa jako magazynu ról bazy danych programu Microsoft SQL Server.

Wewnętrznie framework ról i `SqlRoleProvider` działają podobnie jak w ramach członkostwa i `SqlMembershipProvider`. .NET Framework zawiera `Roles` klasy, która służy jako interfejs API, aby w ramach ról. `Roles` Klasa zawiera metody statyczne, takie jak `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`, i tak dalej. Podczas wywoływania jednej z następujących metod `Roles` klasy deleguje wywołanie skonfigurowany dostawca. `SqlRoleProvider` Współpracuje z tabel pełnionymi (`aspnet_Roles` i `aspnet_UsersInRoles`) w odpowiedzi.

Aby można było używać `SqlRoleProvider` dostawcy w naszej aplikacji, należy określić, jakie bazy danych do użycia jako magazyn. `SqlRoleProvider` Oczekuje sklepu określoną rolę, aby mieć określone tabele bazy danych, widoków i procedur składowanych. Te obiekty wymagania bazy danych można dodać za pomocą [ `aspnet_regsql.exe` narzędzie](https://msdn.microsoft.com/library/ms229862.aspx). W tym momencie mamy już bazę danych przy użyciu schematu, które są niezbędne do `SqlRoleProvider`. Ponownie <a id="_msoanchor_6"> </a> [ *tworzenie schematu członkostwa w programie SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) samouczku utworzyliśmy bazę danych o nazwie `SecurityTutorials.mdf` i używać `aspnet_regsql.exe` umożliwiające dodanie aplikacji usługi, które uwzględnione obiekty bazy danych wymagane przez `SqlRoleProvider`. W związku z tym musimy poinformować szablon role, aby włączyć obsługę roli oraz używanie `SqlRoleProvider` z `SecurityTutorials.mdf` baza danych jako magazynu ról.

W ramach ról jest skonfigurowany za pośrednictwem &lt; `roleManager` &gt; element aplikacji w witrynie `Web.config` pliku. Domyślnie obsługa roli jest wyłączona. Aby ją włączyć, należy ustawić [ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/library/ms164660.aspx) elementu `enabled` atrybutu `true` w następujący sposób:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Domyślnie wszystkie aplikacje sieci web mają dostawcy ról, o nazwie `AspNetSqlRoleProvider` typu `SqlRoleProvider`. Ten dostawca domyślny jest zarejestrowany w `machine.config` (znajdujący się w `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

Dostawcy `connectionStringName` atrybut określa magazynu ról, które jest używane. `AspNetSqlRoleProvider` Dostawcy ustawia ten atrybut `LocalSqlServer`, który również jest zdefiniowany w `machine.config` i punkty, domyślnie do bazy danych programu SQL Server 2005 Express Edition w `App_Data` folder o nazwie `aspnet.mdf`.

W związku z tym jeśli bez określania żadnych informacji o dostawcy w naszej aplikacji możemy po prostu włącz framework role `Web.config` plik, aplikacja używa domyślnego zarejestrowany dostawca ról, `AspNetSqlRoleProvider`. Jeśli `~/App_Data/aspnet.mdf` bazy danych nie istnieje, środowisko uruchomieniowe ASP.NET utworzyć automatycznie i Dodaj schemat usług aplikacji. Jednak firma Microsoft nie chcesz używać `aspnet.mdf` bazy danych; zamiast chcemy do użycia `SecurityTutorials.mdf` bazy danych, który możemy wcześniej utworzony i dodany schemat usług aplikacji. Ta modyfikacja można zrobić na dwa sposoby:

- <strong>Określ wartość dla</strong><strong>`LocalSqlServer`</strong><strong>nazwa parametrów połączenia w</strong><strong>`Web.config`</strong><strong>.</strong> Przez zastąpienie `LocalSqlServer` wartość Nazwa parametrów połączenia w `Web.config`, możemy użyć domyślnej zarejestrowany dostawca ról (`AspNetSqlRoleProvider`) i jest poprawnie pracować `SecurityTutorials.mdf` bazy danych. Aby uzyskać więcej informacji na temat tej techniki, zobacz [Scott Guthrie](https://weblogs.asp.net/scottgu/)firmy wpis w blogu [Konfigurowanie platformy ASP.NET 2.0 usługi aplikacji, użyj programu SQL Server 2000 lub SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Dodaj nowego dostawcę zarejestrowanego typu</strong><strong>`SqlRoleProvider`</strong><strong>i konfigurowania jej</strong><strong>`connectionStringName`</strong><strong>ustawienie, aby wskazywał</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>bazy danych.</strong> Jest to metoda zalecana i używane w <a id="_msoanchor_7"> </a> [ *tworzenie schematu członkostwa w programie SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) samouczek to podejście będę używać w tym samouczku, jak również.

Dodaj następujące role konfiguracji znaczników `Web.config` pliku. Ten kod znaczników rejestruje nowego dostawcę o nazwie `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

Definiuje znaczniki powyżej `SecurityTutorialsSqlRoleProvider` jako domyślnego dostawcę (za pośrednictwem `defaultProvider` atrybutu w `<roleManager>` elementu). Ustawia również `SecurityTutorialsSqlRoleProvider`firmy `applicationName` ustawienie `SecurityTutorials`, która jest taka sama `applicationName` ustawienia używane przez dostawcy członkostwa (`SecurityTutorialsSqlMembershipProvider`). Gdy nie są wyświetlane w tym miejscu [ `<add>` elementu](https://msdn.microsoft.com/library/ms164662.aspx) dla `SqlRoleProvider` może również zawierać `commandTimeout` atrybutu, aby określić wartość limitu czasu bazy danych w ciągu kilku sekund. Wartość domyślna to 30.

Za pomocą tej konfiguracji znaczników w miejscu jesteśmy gotowi rozpocząć korzystanie z funkcji ról w ramach naszej aplikacji.

> [!NOTE]
> Powyższe znaczników konfiguracji ilustruje użycie &lt; `roleManager` &gt; elementu `enabled` i `defaultProvider` atrybutów. Istnieje szereg innych atrybutów, które mają wpływ na sposób w ramach ról kojarzy informacje o roli na podstawie użytkownika według. Będziemy sprawdzać tych ustawień w <a id="_msoanchor_8"> </a> [ *autoryzacji opartej na rolach* ](role-based-authorization-cs.md) samouczka.


## <a name="step-3-examining-the-roles-api"></a>Krok 3: Sprawdzenie interfejs API ról

Funkcje w ramach ról jest uwidaczniany za pomocą [ `Roles` klasy](https://msdn.microsoft.com/library/system.web.security.roles.aspx), zawierającą trzynaście metody statyczne do wykonywania operacji opartej na rolach. Gdy przyjrzymy się tworzenia i usuwania ról w kroki 4 i 6 użyjemy [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) i [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) metody, które dodać lub usunąć rolę z systemu.

Aby uzyskać listę wszystkich ról w systemie, użyj [ `GetAllRoles` metoda](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (zobacz krok 5). [ `RoleExists` Metoda](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) zwraca wartość logiczną wskazującą, czy określona rola istnieje.

W następnym samouczku będziemy sprawdzać Tworzenie skojarzeń użytkowników z rolami. `Roles` Klasy [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), i [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) metody Dodaj co najmniej jednego użytkownika do co najmniej jedną rolę. Aby usunąć użytkowników z ról, należy użyć [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), lub [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) metody.

W <a id="_msoanchor_9"> </a> [ *autoryzacji opartej na rolach* ](role-based-authorization-cs.md) samouczku przedstawiony zostanie sposób programowo pokazać lub ukryć funkcji na podstawie roli aktualnie zalogowanego użytkownika. Aby to osiągnąć, możemy użyć `Role` klasy [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), lub [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) metody.

> [!NOTE]
> Należy pamiętać o tym, które jest wywoływane, ilekroć jednej z następujących metod, `Roles` klasy deleguje wywołanie skonfigurowany dostawca. W naszym przypadku oznacza to, że wywołanie jest wysyłany do `SqlRoleProvider`. `SqlRoleProvider` Następnie wykonuje operację z odpowiednią bazą danych, na podstawie wywoływanej metody. Na przykład, kod `Roles.CreateRole("Administrators")` skutkuje `SqlRoleProvider` wykonywania `aspnet_Roles_CreateRole` procedurą składowaną, która wstawia nowy rekord do `aspnet_Roles` tabelę o nazwie Administratorzy.


W pozostałej części tego samouczka analizuje przy użyciu `Roles` klasy `CreateRole`, `GetAllRoles`, i `DeleteRole` metody do zarządzania rolami w systemie.

## <a name="step-4-creating-new-roles"></a>Krok 4: Tworzenie nowych ról

Role umożliwiają arbitralnie grupy użytkowników i najczęściej ta metoda grupowania jest używana do bardziej wygodne stosuje reguły autoryzacji. Jednak aby można było używać ról jako mechanizmu autoryzacji, najpierw należy zdefiniować role, jakie istnieje w aplikacji. Niestety program ASP.NET nie ma kontroli CreateRoleWizard. Aby dodać nowe role musimy utworzyć interfejs użytkownika odpowiedni i wywołania interfejsu API ról, określić główną przyczynę. Dobra wiadomość jest to bardzo łatwe.

> [!NOTE]
> W trakcie nie formantu sieci CreateRoleWizard Web ma [narzędzie Administratorskie witryny sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx), czyli lokalnej aplikacji platformy ASP.NET zaprojektowany na potrzeby przeglądania i zarządzania konfiguracją aplikacji sieci web. Jednak nie mam duże fanem narzędzie Administratorskie witryny sieci Web platformy ASP.NET z dwóch przyczyn. Najpierw jest nieco pracowały i środowisko użytkownika pozostawia znacznie być wskazane. Po drugie narzędzie Administratorskie witryny sieci Web platformy ASP.NET jest przeznaczony do działają tylko lokalnie, co oznacza, że konieczne będzie można utworzyć własną rolę zarządzania stron sieci web, jeśli trzeba zarządzać zdalnie ról w działającej witrynie. Tych dwóch powodów w tym samouczku, a następnie koncentruje się na tworzenie roli niezbędne narzędzia do zarządzania na stronie sieci web, a nie opierając się na narzędzie Administratorskie witryny sieci Web platformy ASP.NET.


Otwórz `ManageRoles.aspx` stronie `Roles` folderu oraz dodawać do strony pola tekstowego i kontrolki przycisku w sieci Web. Ustaw formant pola tekstowego `ID` właściwości `RoleName` i przycisku `ID` i `Text` właściwości w celu `CreateRoleButton` i Utwórz rolę, odpowiednio. W tym momencie oznaczeniu deklaracyjnym strony powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Następnie kliknij dwukrotnie `CreateRoleButton` formantu w projektancie, aby utworzyć przycisk `Click` program obsługi zdarzeń, a następnie dodaj następujący kod:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

Powyższy kod uruchamia, przypisując Nazwa roli przycięty, wprowadzić w `RoleName` polu tekstowym `newRoleName` zmiennej. Następnie `Roles` klasy `RoleExists` metoda jest wywoływana w celu określenia, czy rola `newRoleName` już istnieje w systemie. Jeśli rola nie istnieje, jest tworzony przez wywołanie `CreateRole` metody. Jeśli `CreateRole` metody jest przekazywana nazwa roli, która już istnieje w systemie, `ProviderException` wyjątku. Dlatego kod najpierw sprawdza, aby upewnić się, że rola nie istnieje już w systemie przed wywołaniem `CreateRole`. `Click` Programu obsługi zdarzeń stwierdza, usuwając `RoleName` pola `Text` właściwości.

> [!NOTE]
> Możesz się zastanawiać, co się stanie, jeśli użytkownik nie wprowadzić dowolną wartość w `RoleName` pola tekstowego. Jeśli wartość przekazywana do `CreateRole` metodą jest `null` lub pustym ciągiem, wyjątek jest zgłaszany. Podobnie jeśli nazwa roli zawiera przecinek jest zgłaszany wyjątek. W związku z tym strony powinna zawierać kontrolek weryfikacji, aby upewnić się, użytkownik musi wprowadzić roli i że nie zawiera żadnych przecinkami. Opuścić w charakterze ćwiczenia dla czytnika.


Utwórz rolę o nazwie Administratorzy. Odwiedź stronę `ManageRoles.aspx` stronie za pośrednictwem przeglądarki, wpisz w polu tekstowym administratorów (patrz rysunek 3), a następnie kliknij przycisk Utwórz rolę.


[![Utworzenie roli administratorów](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Rysunek 3**: Utworzenie roli administratorów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image9.png))


Co się dzieje? Występuje odświeżenie strony, ale nie ma żadnych wizualnych oznak aktywacji, który faktycznie został rolę dodawane do systemu. Firma Microsoft aktualizuje tę stronę w kroku 5, aby uwzględnić wizualną opinię. Teraz, jednak możesz sprawdzić, przechodząc do utworzono rolę `SecurityTutorials.mdf` bazy danych i wyświetlanie danych za pomocą `aspnet_Roles` tabeli. Jak pokazano na rysunku 4, `aspnet_Roles` tabela zawiera rekord o rolach administratorów po prostu dodanego przez producenta.


[![Aspnet_Roles tabela zawiera wiersz dla administratorów](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Rysunek 4**: `aspnet_Roles` tabela zawiera wiersz dla administratorów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Krok 5: Wyświetlanie ról w systemie

Możemy rozszerzyć `ManageRoles.aspx` strony, aby dołączyć listę bieżących ról w systemie. W tym celu należy dodać kontrolki widoku siatki do strony i ustaw jego `ID` właściwość `RoleList`. Następnie dodaj metodę do klasy CodeBehind strony o nazwie `DisplayRolesInGrid` przy użyciu następującego kodu:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

`Roles` Klasy `GetAllRoles` metoda zwraca wszystkie role w systemie jako tablicę ciągów. Ta tablica ciągów następnie jest powiązany z kontrolki GridView. Aby powiązać listę ról w widoku GridView ładowaniu strony, należy wywołać `DisplayRolesInGrid` metody na stronie `Page_Load` programu obsługi zdarzeń. Poniższy kod wywołuje tę metodę, gdy najpierw odwiedzenia strony, ale nie na kolejne ogłaszania zwrotnego.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Przy użyciu tego kodu w miejscu odwiedź stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 5, powinien zostać wyświetlony siatkę z jedną kolumną etykietą elementu. Siatka zawiera wiersz dla roli Administratorzy, dodane w kroku 4.


[![Kontrolki GridView są wyświetlane role w jednej kolumnie](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Rysunek 5**: widoku GridView są wyświetlane role w jednej kolumnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image15.png))


Kontrolki GridView wyświetla pojedynczy kolumnę z etykietą elementu, ponieważ w widoku GridView `AutoGenerateColumns` właściwość jest ustawiona na wartość True (domyślnie), co powoduje, że GridView automatycznie utworzyć kolumnę dla każdej właściwości w jego `DataSource`. Tablica ma tylko jedną właściwość, która reprezentuje elementów w tablicy, dlatego jednej kolumny w widoku GridView.

Podczas wyświetlania danych przy użyciu GridView, chcę jawnie Definiowanie kolumn, a nie ich generowane niejawnie, przy widoku GridView. Jawne określenie kolumny jest znacznie łatwiejsze do formatowania danych, zmienić kolejność kolumn i wykonywania innych typowych zadań. W związku z tym zaktualizuj GridView oznaczeniu deklaracyjnym tak, aby jej kolumn są jawnie zdefiniowane.

Rozpocznij od GridView `AutoGenerateColumns` wartość False dla właściwości. Następnie dodaj TemplateField do siatki, ustaw jego `HeaderText` właściwość role i skonfiguruj jego `ItemTemplate` tak, aby wyświetlał zawartości tablicy. Aby to osiągnąć, należy dodać formant etykiety w sieci Web o nazwie `RoleNameLabel` do `ItemTemplate` i powiąż jego `Text` właściwość `Container.DataItem`.

Te właściwości i `ItemTemplate`jego zawartość, można ustawić deklaratywne lub przy użyciu GridView pola, okno dialogowe i Edytuj szablony interfejsu. Aby uzyskać dostęp do pola okno dialogowe, kliknij łącze Edytowanie kolumn w tagu inteligentnego GridView. Następnie usunąć zaznaczenie pola wyboru pól Generuj automatycznie, aby ustawić `AutoGenerateColumns` właściwości na wartość False i Dodaj TemplateField w kontrolce GridView, ustawiając jego `HeaderText` właściwości roli. Aby zdefiniować `ItemTemplate`firmy zawartości, wybierz opcję Edytuj szablony z GridView tagu inteligentnego. Przeciągnij formant etykiety w sieci Web na `ItemTemplate`, ustaw jego `ID` właściwości `RoleNameLabel`oraz skonfigurować jego ustawienia powiązań danych, aby jego `Text` właściwość jest powiązana z `Container.DataItem`.

Niezależnie od tego, jakie podejście, możesz użyć wynikowy oznaczeniu deklaracyjnym GridView powinien wyglądać podobnie do następujących, gdy wszystko będzie gotowe.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Zawartość tablicy są wyświetlane przy użyciu składni wiązania danych `<%# Container.DataItem %>`. Dokładny opis dlaczego ta składnia jest używana podczas wyświetlania zawartości tablicy powiązany z kontrolki GridView wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji dotyczących tej kwestii, zobacz [powiązanie tablicy skalarną z formantu sieci Web danych](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Obecnie `RoleList` GridView tylko jest powiązana z listy ról, jeśli najpierw odwiedzenia strony. Musimy odświeżyć siatki, po każdym dodaniu nowej roli. Aby to zrobić, należy zaktualizować `CreateRoleButton` przycisku `Click` programu obsługi zdarzeń, tak że wywołuje `DisplayRolesInGrid` metodę, jeśli utworzono nową rolę.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Teraz, gdy użytkownik dodaje nową rolę `RoleList` GridView zawiera po prostu dodać roli w zwrotu, zapewniając wizualną opinię, że rola została pomyślnie utworzona. Na przykład można znaleźć `ManageRoles.aspx` strony za pośrednictwem przeglądarki, a następnie dodaj rolę o nazwie nadzorców. Po kliknięciu przycisku Utwórz rolę nastąpi odświeżenie strony i siatki zostanie zaktualizowana, aby obejmować administratorów, a także nową rolę, nadzorców.


[![Rola nadzorców ma, zostały dodane](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Rysunek 6**: nadzorcy rola ma dodano ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Krok 6: Usuwanie ról

W tym momencie użytkownik może utworzyć nową rolę i wyświetlić wszystkie istniejące role z `ManageRoles.aspx` strony. Teraz użytkownicy również usuwania ról. `Roles.DeleteRole` Metoda ma dwa przeciążenia:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -Usuwa rolę *roleName*. Wyjątek jest generowany, jeśli rola zawiera co najmniej jednego członka.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -Usuwa rolę *roleName*. Jeśli *throwOnPopulateRole* jest `true`, a następnie jest zgłaszany wyjątek, jeśli rola zawiera co najmniej jednego członka. Jeśli *throwOnPopulateRole* jest `false`, a następnie rola zostanie usunięty, czy zawiera on żadnych elementów członkowskich, czy nie. Wewnętrznie `DeleteRole(roleName)` wywołania metody `DeleteRole(roleName, true)`.

`DeleteRole` Metoda również spowoduje zgłoszenie wyjątku, jeśli *roleName* jest `null` ani być pustym ciągiem lub jeśli *roleName* zawiera przecinek. Jeśli *roleName* nie istnieje w systemie, `DeleteRole` zakończy się niepowodzeniem w trybie dyskretnym, bez zgłaszania wyjątku.

Możemy rozszerzyć GridView w `ManageRoles.aspx` obejmujący usuwania przycisku, po kliknięciu spowoduje usunięcie wybranej roli. Rozpocznij, dodając do kontrolki GridView przycisk Usuń, przechodząc do okna dialogowego pól i dodawanie przycisk Usuń, który znajduje się w obszarze opcji CommandField. Wprowadzić Delete znajdujący się po lewej stronie kolumny i ustaw jego `DeleteText` właściwości do usunięcia roli.


[![Dodaj przycisk usuwania do kontrolki RoleList GridView](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Rysunek 7**: przycisk Usuń, aby dodać `RoleList` kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image21.png))


Po dodaniu przycisk Usuń, oznaczeniu deklaracyjnym w widoku GridView powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Następnie należy utworzyć procedurę obsługi zdarzeń dla GridView `RowDeleting` zdarzeń. To zdarzenie, które jest wywoływane na odświeżenie strony, po kliknięciu przycisku Usuń rolę. Dodaj następujący kod do narzędzia obsługi zdarzeń.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

Zaczyna się kod, odwołując się do programowego `RoleNameLabel` formant w wierszu kliknięcia przycisku którego usunięcia roli sieci Web. `Roles.DeleteRole` Następnie wywoływana jest metoda, przekazując `Text` z `RoleNameLabel` i `false`, a tym samym usunąć rolę, niezależnie od tego, czy znajdują się użytkownicy, wszelkie skojarzone z rolą. Na koniec `RoleList` GridView są odświeżane, tak aby po prostu usunąć roli nie będzie już wyświetlany w siatce.

> [!NOTE]
> Przycisk Usuń rolę nie wymaga dowolny rodzaj potwierdzenie od użytkownika przed usunięciem roli. Jednym z najprostszych sposobów, aby potwierdzić akcję jest poprzez okno dialogowe Potwierdź po stronie klienta. Aby uzyskać więcej informacji na temat tej techniki, zobacz [dodawanie potwierdzenia po stronie klienta podczas usuwania](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


## <a name="summary"></a>Podsumowanie

Wiele aplikacji sieci web ma niektórych reguł autoryzacji lub funkcji na poziomie strony, które jest dostępne tylko dla niektórych klas użytkowników. Na przykład może być zestaw stron sieci web, które tylko administratorzy będą mieć dostęp. Zamiast definiować te reguły autoryzacji na podstawie użytkownika według, często jest bardziej użyteczny do zdefiniowania reguł na podstawie roli. Zamiast jawnie, umożliwiając użytkownikom Scott i Jisun dostępu administracyjnego stron sieci web, będzie łatwiejszy w utrzymaniu podejście jest aby umożliwić członkom roli Administratorzy programu dostęp do tych stron, a następnie do oznaczania Scott i Jisun jako należące do użytkowników Grupy administratorów.

W ramach ról ułatwia tworzenie ról i zarządzanie nimi. W tym samouczku zbadaliśmy sposobu skonfigurowania struktury role, aby użyć `SqlRoleProvider`, który używa jako magazynu ról bazy danych programu Microsoft SQL Server. Utworzyliśmy również strony sieci web, która wyświetla istniejące role w systemie i pozwala dotyczące nowych ról, które ma zostać utworzony i istniejące do usunięcia. W kolejnych samouczkach zobaczymy, jak przypisać użytkowników do ról i sposób stosowania autoryzacji opartej na rolach.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Badanie programu ASP.NET 2.0 na członkostwo, role i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Instrukcje: Używanie menedżera ról na platformie ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Dostawców ról](https://msdn.microsoft.com/library/aa478950.aspx)
- [Stopniowe własne narzędzia do administrowania witryny sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Dokumentacja techniczna dotycząca `<roleManager>` — Element](https://msdn.microsoft.com/library/ms164660.aspx)
- [Przy użyciu członkostwa i interfejsów API Manager roli](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku obejmują Alicja Maziarz, Suchi Banerjee i Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](assigning-roles-to-users-cs.md)
