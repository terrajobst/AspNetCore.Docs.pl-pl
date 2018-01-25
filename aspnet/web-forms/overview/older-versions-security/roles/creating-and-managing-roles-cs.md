---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: "Tworzenie i zarządzanie rolami (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku sprawdza kroki niezbędne do skonfigurowania w ramach ról. Po tym, że budujemy stron sieci web do tworzenia i usuwania ról."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: b2b13a2a3b242877060aaec2257b2a742ac8d674
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="creating-and-managing-roles-c"></a>Tworzenie i zarządzanie rolami (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> W tym samouczku sprawdza kroki niezbędne do skonfigurowania w ramach ról. Po tym, że budujemy stron sieci web do tworzenia i usuwania ról.


## <a name="introduction"></a>Wprowadzenie

W <a id="_msoanchor_1"> </a> [ *autoryzacji na podstawie użytkownika* ](../membership/user-based-authorization-cs.md) samouczku będziemy przeglądał przy użyciu Autoryzacja adresów URL do ograniczenia określonych użytkowników z zestawu stron i zbadane deklaratywne i programowe techniki dostosowywania oparte na odwiedzającego użytkownika funkcji strony platformy ASP.NET. Udzielanie uprawnień dostępu do strony lub funkcji na podstawie użytkownika przez użytkownika, jednak może stać się nightmare konserwacji, w scenariuszach, w których istnieje wiele kont użytkowników, lub gdy uprawnienia użytkowników zmieniają się często. Ilekroć użytkownik uzyska lub utraci autoryzacji do wykonania określonego zadania, administrator musi zaktualizować odpowiednich reguł autoryzacji adresów URL, deklaratywne znaczników i kodu.

Pomaga zwykle do klasyfikowania użytkowników na grupy lub *ról* , a następnie zastosować uprawnienia na podstawie roli przez rolę. Na przykład większość aplikacji sieci web ma określone strony lub zadań, które są przeznaczone tylko do użytkowników administracyjnych. Przy użyciu technik rozpoznane w *autoryzacji na podstawie użytkownika* samouczek, dodamy odpowiednich reguł autoryzacji adresów URL, deklaratywne znaczników i kodu, aby umożliwić określone konta użytkowników do wykonywania zadań administracyjnych. Ale jeśli dodano nowego administratora lub istniejącego administratora niezbędnych do jej uprawnienia do administrowania odwołany, czy mamy wrócić i zaktualizować pliki konfiguracji i stron sieci web. Z rolami jednak firma Microsoft może utworzyć rolę o nazwie Administratorzy i przypisz tych zaufanych użytkowników do roli administratora. Następnie dodamy odpowiednich reguł autoryzacji adresów URL, deklaratywne znaczników i kodu, aby umożliwić roli administratorów do wykonywania różnych zadań administracyjnych. Z tej infrastruktury w miejscu dodanie nowych administratorów do lokacji lub usunięcie istniejących jest tak proste, jak łącznie lub usunięcie użytkownika z rolą administratora. Nie konfiguracji, deklaratywne znaczników lub zmiany kodu są niezbędne.

Program ASP.NET oferuje framework ról, zdefiniowanie ról i kojarzenie je z kontami użytkowników. Z architekturą ról, firma Microsoft może tworzyć i usuwać role dodawać lub usuwać użytkowników z roli, ustalić zbiór użytkowników, którzy należą do określonej roli i sprawdzić, czy użytkownik należy do określonej roli. Skonfigurowane w ramach ról możemy ograniczyć dostęp do strony na podstawie roli roli przy użyciu reguł autoryzacji adresów URL i Pokaż lub Ukryj dodatkowe informacje lub funkcji na stronie na podstawie ról aktualnie zalogowanego użytkownika.

W tym samouczku sprawdza kroki niezbędne do skonfigurowania w ramach ról. Po tym, że budujemy stron sieci web do tworzenia i usuwania ról. W <a id="_msoanchor_2"> </a> [ *przypisywanie ról użytkowników* ](assigning-roles-to-users-cs.md) samouczka przedstawiono, jak dodawać i usuwać użytkowników z ról. I w <a id="_msoanchor_3"> </a> [ *autoryzacji opartej na rolach* ](role-based-authorization-cs.md) samouczka należy sprawdzić, jak ograniczyć dostęp do stron na podstawie roli ról oraz jak ustawić zależności funkcji strony w roli zaproszonych użytkowników. Dzieła!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1: Dodawanie nowych stron ASP.NET

W tym samouczku i dwie firma Microsoft będzie można badanie różnych funkcji związanych z ról i funkcji. Szereg stron ASP.NET do zaimplementowania tematy zbadać w całym te samouczki są wymagane. Umożliwia tworzenie tych stron i zaktualizuj mapy witryny.

Rozpocznij od utworzenia nowego folderu do projektu o nazwie `Roles`. Następnie dodaj czterech nowych stron ASP.NET do `Roles` folderu łączenie każdej strony zawierającej `Site.master` strony wzorcowej. Nazwa strony:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

W tym momencie projektu Eksploratora rozwiązań powinien wyglądać podobnie do ekranu zrzut pokazano na rysunku 1.


[![Dodano czterech nowych stron w folderze ról](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Rysunek 1**: czterech nowych stron zostały dodane do `Roles` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image3.png))


Każdej strony w tym momencie powinna mieć dwóch formantów zawartości, jeden dla każdej strony wzorcowej Elementy ContentPlaceHolders: `MainContent` i `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Odwołania, który `LoginContent` znaczników domyślnego elementu ContentPlaceHolder na zawiera łącze do logowania się lub wylogowywania lokacji, w zależności od tego, czy użytkownik jest uwierzytelniony. Obecność `Content2` zawartości kontrolki na stronie platformy ASP.NET, jednak zastępuje znaczników domyślnej strony wzorcowej. Jak wspomniano w <a id="_msoanchor_4"> </a> [ *omówienie uwierzytelniania formularzy* ](../introduction/an-overview-of-forms-authentication-cs.md) samouczek, Zastępowanie domyślnego znacznika jest przydatny do strony, w którym firma Microsoft nie mają być wyświetlane związane z logowaniem Opcje w lewej kolumnie.

Dla tych czterech stron, jednak chęć pokazania znaczników domyślnej strony wzorcowej dla `LoginContent` ContentPlaceHolder. W związku z tym usunąć deklaratywne kod znaczników dla `Content2` zawartości formantu. Po wykonaniu tej czynności, każdy z czterech strony znaczników powinna zawierać tylko jeden formant zawartości.

Na koniec Przyjrzyjmy aktualizacji mapy witryny (`Web.sitemap`) do uwzględnienia tych nowych stron sieci web. Dodaj następujący kod XML po `<siteMapNode>` dodaliśmy samouczki członkostwa.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Z mapy witryny zaktualizowany odwiedź witrynę za pośrednictwem przeglądarki. Jak pokazano na rysunku 2, nawigacji po lewej stronie teraz obejmuje elementy samouczki ról.


[![Dodano czterech nowych stron w folderze ról](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Rysunek 2**: czterech nowych stron zostały dodane do `Roles` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Krok 2: Określanie i Konfigurowanie dostawcy Framework ról

Jak framework członkostwa w ramach ról została stworzona z modelu dostawcy. Zgodnie z opisem w <a id="_msoanchor_5"> </a> [ *podstawowe informacje o zabezpieczeniach i obsługę ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) samouczek, .NET Framework jest dostarczany z trzech dostawców wbudowanych ról: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), i [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Ten samouczek seria skupia się wokół `SqlRoleProvider`, który korzysta z bazy danych programu Microsoft SQL Server do przechowywania roli.

Poniżej obejmuje framework ról i `SqlRoleProvider` służbowych, podobnie jak w ramach członkostwa i `SqlMembershipProvider`. .NET Framework zawiera `Roles` klasy, która służy jako interfejsu API Framework ról. `Roles` Klasa ma statycznej metody, takie jak `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`, itd. Po wywołaniu jednej z tych metod, `Roles` klasy deleguje wywołanie skonfigurowanego dostawcy. `SqlRoleProvider` Współpracuje z tabel określonych ról (`aspnet_Roles` i `aspnet_UsersInRoles`) w odpowiedzi.

Aby można było używać `SqlRoleProvider` dostawcy w naszej aplikacji, należy określić co baza danych ma być używana jako magazyn. `SqlRoleProvider` Oczekuje magazynu określoną rolę do określonych tabel bazy danych, widoków i procedur składowanych. Te obiekty wymagania bazy danych można dodać za pomocą [ `aspnet_regsql.exe` narzędzia](https://msdn.microsoft.com/library/ms229862.aspx). W tym momencie mamy już bazę danych ze schematem niezbędne do `SqlRoleProvider`. W <a id="_msoanchor_6"> </a> [ *tworzenie schematu członkostwa w programie SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) samouczek utworzyliśmy bazy danych o nazwie `SecurityTutorials.mdf` i używać `aspnet_regsql.exe` można dodać aplikacji usług, które zawierają obiekty bazy danych, wymagane przez `SqlRoleProvider`. W związku z tym musimy sprawdzić framework role, aby włączyć obsługę roli i do użycia `SqlRoleProvider` z `SecurityTutorials.mdf` bazy danych jako magazyn ról.

W ramach ról jest skonfigurowana za pośrednictwem &lt; `roleManager` &gt; element w aplikacji `Web.config` pliku. Domyślnie obsługa roli jest wyłączona. Aby go włączyć, należy ustawić [ &lt; `roleManager` &gt; ](https://msdn.microsoft.com/library/ms164660.aspx) elementu `enabled` atrybutu `true` w następujący sposób:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Domyślnie wszystkie aplikacje sieci web ma dostawcy role o nazwie `AspNetSqlRoleProvider` typu `SqlRoleProvider`. Ten dostawca domyślny jest zarejestrowany w `machine.config` (znajdujący się w `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

Dostawcy `connectionStringName` atrybut określa magazynu roli, który jest używany. `AspNetSqlRoleProvider` Dostawcy ten atrybut ustawia `LocalSqlServer`, która jest także zdefiniowana w `machine.config` i punkty domyślnie bazą danych programu SQL Server 2005 Express Edition w `App_Data` folder o nazwie `aspnet.mdf`.

W związku z tym jeśli pozwolimy po prostu framework role bez określania informacji o dostawcy w naszej aplikacji `Web.config` plik, aplikacja używa domyślny zarejestrowany dostawca ról, `AspNetSqlRoleProvider`. Jeśli `~/App_Data/aspnet.mdf` bazy danych nie istnieje, automatycznie go utworzyć i dodać schematu usług aplikacji środowiska wykonawczego programu ASP.NET. Jednak nie chcemy użyć `aspnet.mdf` bazy danych; zamiast chcemy użyć `SecurityTutorials.mdf` bazy danych, który mamy już utworzono i schemat usług aplikacji, aby dodać. Ta modyfikacja można zrobić w jeden z dwóch sposobów:

- **Określ wartość ***`LocalSqlServer`*** Nazwa ciągu połączenia w ***`Web.config`***.** Zastępując `LocalSqlServer` wartości nazwy parametrów połączenia w `Web.config`, możemy użyć zarejestrowany domyślny dostawca ról (`AspNetSqlRoleProvider`) i będzie on działać poprawnie z `SecurityTutorials.mdf` bazy danych. Aby uzyskać więcej informacji na temat tej techniki, zobacz [Scott Guthrie](https://weblogs.asp.net/scottgu/)w blogu, [Konfigurowanie ASP.NET 2.0 usługi aplikacji, użyj programu SQL Server 2000 lub SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- **Dodaj nowego dostawcę zarejestrowany dla typu ***`SqlRoleProvider`*** i skonfigurować jej ***`connectionStringName`*** ustawienie wskazujące ***`SecurityTutorials.mdf`*** bazy danych.** Jest to metoda I zalecane i używane w <a id="_msoanchor_7"> </a> [ *tworzenie schematu członkostwa w programie SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) samouczek, a jest to rozwiązanie będzie używać w tym samouczku, jak również.

Dodaj następujące role konfiguracji znaczników `Web.config` pliku. Ten kod znaczników rejestruje nowego dostawcę o nazwie `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

Definiuje powyżej znacznika `SecurityTutorialsSqlRoleProvider` jako domyślny dostawca (za pośrednictwem `defaultProvider` atrybutu w `<roleManager>` elementu). Ustawia również `SecurityTutorialsSqlRoleProvider`w `applicationName` ustawienie `SecurityTutorials`, który jest taki sam `applicationName` ustawienia używane przez dostawcę członkostwa (`SecurityTutorialsSqlMembershipProvider`). Gdy nie są wyświetlane w tym miejscu [ `<add>` elementu](https://msdn.microsoft.com/library/ms164662.aspx) dla `SqlRoleProvider` może również zawierać `commandTimeout` atrybutu, aby określić limit czasu bazy danych, w sekundach. Wartość domyślna to 30.

Z tej konfiguracji znacznika w miejscu możemy gotowe do rozpoczęcia korzystania z funkcji ról w naszej aplikacji.

> [!NOTE]
> Pokazano powyżej znaczników konfiguracji przy użyciu &lt; `roleManager` &gt; elementu `enabled` i `defaultProvider` atrybutów. Istnieje wiele innych atrybutów, które mają wpływ na sposób kojarzenia informacje o rolach dla użytkowników przez użytkowników w ramach ról. Omówione zostaną te ustawienia w <a id="_msoanchor_8"> </a> [ *autoryzacji opartej na rolach* ](role-based-authorization-cs.md) samouczka.


## <a name="step-3-examining-the-roles-api"></a>Krok 3: Badanie interfejs API ról

Funkcje w ramach ról jest udostępniane za pośrednictwem funkcji [ `Roles` klasy](https://msdn.microsoft.com/library/system.web.security.roles.aspx), zawierającą trzynaście metody statyczne do wykonywania operacji na podstawie ról. Gdy przyjrzymy się tworzenia i usuwania ról w kroki 4 i 6 używamy [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) i [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) metody, które dodać lub usunąć rolę z systemu.

Aby uzyskać listę wszystkich ról w systemie, należy użyć [ `GetAllRoles` metody](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (zobacz krok 5). [ `RoleExists` Metoda](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) zwraca wartość logiczną wskazującą, czy określona rola istnieje.

W następnym samouczku omówione zostanie Tworzenie skojarzeń użytkowników z rolami. `Roles` Klasy [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), i [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) metody Dodaj co najmniej jednego użytkownika do co najmniej jedną rolę. Aby usunąć użytkowników z ról, należy użyć [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), lub [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) metody.

W <a id="_msoanchor_9"> </a> [ *autoryzacji opartej na rolach* ](role-based-authorization-cs.md) samouczka przedstawiono sposoby programowo Pokaż lub Ukryj funkcje na podstawie roli aktualnie zalogowanego użytkownika. W tym celu możemy użyć `Role` klasy [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), lub [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) metody.

> [!NOTE]
> Należy pamiętać, wywoływaną w dowolnym momencie, w jednej z tych metod, `Roles` klasy deleguje wywołanie skonfigurowanego dostawcy. W tym przypadku oznacza to, że wywołanie jest wysyłany do `SqlRoleProvider`. `SqlRoleProvider` Następnie wykonuje operację odpowiednią bazę danych na podstawie wywołanej metody. Na przykład kod `Roles.CreateRole("Administrators")` powoduje `SqlRoleProvider` wykonywania `aspnet_Roles_CreateRole` przechowywane procedury, która wstawia nowy rekord w `aspnet_Roles` tabeli o nazwie Administratorzy.


W pozostałej części tego samouczka analizuje przy użyciu `Roles` klasy `CreateRole`, `GetAllRoles`, i `DeleteRole` metody zarządzania ról w systemie.

## <a name="step-4-creating-new-roles"></a>Krok 4: Tworzenie nowych ról

Role umożliwiają arbitralnie grupy użytkowników, a najczęściej ta metoda grupowania jest używana do wygodny sposób można zastosować reguł autoryzacji. Aby można było używać jako mechanizmu autoryzacji ról najpierw musimy jednak zdefiniować role istnieje w aplikacji. Niestety program ASP.NET nie zawiera formantu CreateRoleWizard. Aby dodać nowe role należy utworzyć odpowiedni interfejs i wywołania interfejsu API ról nad. Dobre wieści jest, że jest to bardzo łatwe do wykonania.

> [!NOTE]
> Mimo że znajduje się kontrolka nie CreateRoleWizard sieci Web, [narzędzia administrowania witryną sieci Web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx), czyli lokalnych aplikacji ASP.NET, mające na celu pomoc przy przeglądaniu i zarządzanie konfiguracją aplikacji sieci web. Jednak nie mam big wentylator narzędzia administrowania witryną sieci Web platformy ASP.NET z dwóch przyczyn. Najpierw jest nieco buggy i środowisko użytkownika opuszcza się być pożądane. Po drugie narzędzie do administrowania witryną sieci Web programu ASP.NET jest przeznaczona do tylko pracę lokalną, co oznacza, że konieczne będzie można utworzyć własną rolę zarządzania stron sieci web, jeśli należy zarządzać zdalnie ról lokacji na żywo. Z tych dwóch przyczyn tego samouczka i następnych koncentruje się na tworzenie ról wymaganych narzędzi do zarządzania na stronie sieci web zamiast polegania na narzędzie do administrowania witryną sieci Web programu ASP.NET.


Otwórz `ManageRoles.aspx` strony `Roles` folderu i Dodaj pole tekstowe i formantu przycisku sieci Web ze stroną. Ustawianie formantu TextBox `ID` właściwości `RoleName` i przycisku `ID` i `Text` właściwości `CreateRoleButton` i Utwórz rolę odpowiednio. W tym momencie znaczników deklaratywne ze strony powinien wyglądać podobnie do następującego:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Następnie kliknij dwukrotnie `CreateRoleButton` przycisku kontrolki projektanta, aby utworzyć `Click` obsługi zdarzeń, a następnie dodaj następujący kod:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

Przypisując wprowadzona w Nazwa roli przycięte zaczyna się kod powyżej `RoleName` polu tekstowym `newRoleName` zmiennej. Następnie `Roles` klasy `RoleExists` metoda jest wywoływana w celu określenia, czy rola `newRoleName` już istnieje w systemie. Jeśli rola istnieje, jest tworzony za pomocą wywołania `CreateRole` metody. Jeśli `CreateRole` metody jest przekazywany Nazwa roli, która już istnieje w systemie, `ProviderException` wyjątku. Dlatego kod najpierw sprawdza, upewnij się, że rola nie istnieje już w systemie przed wywołaniem `CreateRole`. `Click` Obsługi zdarzeń stwierdza, czyszcząc `RoleName` w pole tekstowe `Text` właściwości.

> [!NOTE]
> Możesz się zastanawiać, co się stanie, jeśli użytkownik nie poda żadnych wartości do `RoleName` pola tekstowego. Jeśli wartość przekazywana do `CreateRole` jest metoda `null` lub pusty ciąg, wyjątek jest wywoływane. Podobnie jeśli nazwa roli zawiera przecinek jest wyjątek. W rezultacie strona powinna zawierać formanty walidacji, aby upewnić się, że użytkownik wprowadza rolę i że nie zawiera żadnych przecinkami. Pozostaw I wykonywania dla czytnika.


Utwórz rolę o nazwie Administratorzy. Odwiedź stronę `ManageRoles.aspx` strony za pośrednictwem przeglądarki, wpisz w polu tekstowym administratorów (patrz rysunek 3), a następnie kliknij przycisk Utwórz rolę.


[![Utworzenie roli administratorów](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Rysunek 3**: Utworzenie roli administratorów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image9.png))


Co się stanie? Występuje odświeżenie strony, ale nie ma żadnych wizualnych oznak aktywacji, który był roli dodane do systemu. Aktualizujemy tej strony w kroku 5, aby uwzględnić wizualne. Teraz, jednak można sprawdzić utworzono rolę, przechodząc do `SecurityTutorials.mdf` bazy danych i wyświetlanie danych z `aspnet_Roles` tabeli. Jak pokazano na rysunku 4, `aspnet_Roles` tabela zawiera rekord role administratorów po prostu dodane.


[![Aspnet_Roles tabela zawiera wiersz dla administratorów](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Rysunek 4**: `aspnet_Roles` tabela zawiera wiersz dla administratorów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Krok 5: Wyświetlanie ról w systemie

Ta funkcja pozwala rozszerzyć `ManageRoles.aspx` stronę, aby dołączyć listę bieżących ról w systemie. W tym celu Dodaj formant widoku GridView do strony i ustawić jej `ID` właściwości `RoleList`. Następnie dodaj metodę do klasy związane z kodem strony o nazwie `DisplayRolesInGrid` przy użyciu następującego kodu:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

`Roles` Klasy `GetAllRoles` metoda zwraca wszystkie role w systemie jako tablicę ciągów. Ta tablica ciągów następnie jest powiązany z widoku GridView. Aby można było powiązać lista ról w widoku GridView jest najpierw załadować strony, należy wywołać `DisplayRolesInGrid` metoda ze strony `Page_Load` obsługi zdarzeń. Poniższy kod wywołuje tę metodę, jeśli najpierw odwiedzenia strony, ale nie na kolejnych ogłaszania zwrotnego.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Z tego kodu w miejscu odwiedź stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku nr 5, siatce powinna zostać wyświetlona z jedną kolumną z etykietą elementu. Siatka zawiera wiersz dla roli Administratorzy, dodane w kroku 4.


[![Widoku GridView są wyświetlane role w jednej kolumnie](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Rysunek 5**: widoku GridView są wyświetlane role w jednej kolumnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image15.png))


Widoku GridView wyświetla pojedynczy kolumnę z etykietą elementu, ponieważ w widoku GridView `AutoGenerateColumns` właściwość jest ustawiona na True (domyślnie), co powoduje, że można automatycznie utworzyć kolumn dla każdej właściwości w widoku GridView jego `DataSource`. Tablicy ma tylko jedną właściwość, która reprezentuje elementów w tablicy, dlatego jednej kolumny w widoku GridView.

Podczas wyświetlania danych z widoku GridView, wolę jawnie zdefiniuj kolumn, a nie ich niejawnie wygenerowany przez widoku GridView. Definiując jawnie kolumny jest znacznie łatwiejsze do formatowania danych, zmienić kolejność kolumn i wykonywanie innych częstych zadań. W związku z tym teraz zaktualizować deklaratywne znaczników w widoku GridView tak, aby jawnie zdefiniowanych kolumn.

Rozpocznij od ustawienia w widoku GridView `AutoGenerateColumns` wartość False dla właściwości. Następnie dodaj pole TemplateField do siatki, ustaw jej `HeaderText` właściwości do ról i skonfiguruj jego `ItemTemplate` tak, aby Wyświetla zawartość tablicy. W tym celu Dodaj formant etykiety sieci Web o nazwie `RoleNameLabel` do `ItemTemplate` i powiąż jego `Text` właściwości `Container.DataItem`.

Te właściwości i `ItemTemplate`jego zawartość można ustawić deklaratywnie lub przez okno dialogowe pola w widoku GridView i Edytuj szablony interfejsu. Aby uzyskać dostęp do pola — okno dialogowe, kliknij łącze Edytowanie kolumn w tagów inteligentnych w widoku GridView. Następnie usuń zaznaczenie pola wyboru automatycznego generowania pola wyboru, aby ustawić `AutoGenerateColumns` właściwości na wartość False i Dodaj pole TemplateField do widoku GridView, ustawiając jego `HeaderText` właściwości do roli. Aby zdefiniować `ItemTemplate`jego zawartość, wybierz opcję Edytuj szablony z tagów inteligentnych w widoku GridView. Przeciągnij formant etykiety sieci Web na `ItemTemplate`, ustaw jej `ID` właściwości `RoleNameLabel`i skonfigurować jego ustawienia wiązania z danymi, aby jego `Text` właściwość jest powiązana z `Container.DataItem`.

Niezależnie od tego, jakiego używasz podejście wynikowy znaczników deklaratywne w widoku GridView powinien wyglądać podobnie do następujących, gdy wszystko będzie gotowe.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Zawartość tablicy są wyświetlane przy użyciu składni wiązania z danymi `<%# Container.DataItem %>`. Dlaczego ta składnia jest używany podczas wyświetlania zawartości tablicy powiązany z widoku GridView dokładny opis wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji w tej sprawie, zapoznaj się [powiązanie tablicy skalarnych do formantu sieci Web danych](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Obecnie `RoleList` widoku GridView jest powiązany tylko z listy ról, gdy najpierw odwiedzenia strony. Musimy odświeżyć siatki przy każdym dodaniu nowej roli. Aby to zrobić, należy zaktualizować `CreateRoleButton` przycisku `Click` obsługi zdarzeń, tak że wywołuje `DisplayRolesInGrid` metodę, jeśli utworzono nową rolę.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Teraz, gdy użytkownik dodaje nową rolę `RoleList` GridView zawiera po prostu dodane roli ogłaszania zwrotnego, zapewniając wizualne roli została pomyślnie utworzona. Na przykład można znaleźć `ManageRoles.aspx` strony za pośrednictwem przeglądarki, a następnie dodaj rolę o nazwie kontrolerów. Po kliknięciu przycisku Utwórz rolę, nastąpi odświeżenie strony i siatki zostanie zaktualizowana, aby obejmować administratorów, a także nową rolę, kontrolerów.


[![Rola nadzorców ma zostały dodane](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Rysunek 6**: rola nadzorców został dodany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Krok 6: Usuwanie ról

W tym momencie użytkownik może tworzyć nowej roli i wyświetlania wszystkich istniejących ról z `ManageRoles.aspx` strony. Teraz użytkownicy również usunięcie ról. `Roles.DeleteRole` Metoda ma dwa przeciążenia:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx)— Usuwa rolę *roleName*. Jeśli rola zawiera co najmniej jednego członka, jest zgłaszany wyjątek.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx)— Usuwa rolę *roleName*. Jeśli *throwOnPopulateRole* jest `true`, a następnie jest zgłaszany wyjątek, jeśli rola zawiera co najmniej jednego członka. Jeśli *throwOnPopulateRole* jest `false`, następnie roli jest usuwany, czy lub nie zawiera żadnych elementów członkowskich. Wewnętrznie `DeleteRole(roleName)` wywołania metody `DeleteRole(roleName, true)`.

`DeleteRole` Metody również zgłosić wyjątek, jeśli *roleName* jest `null` lub ciąg pusty lub jeśli *roleName* zawiera przecinek. Jeśli *roleName* nie istnieje w systemie, `DeleteRole` nie jest w trybie dyskretnym, który wywołał wyjątek.

Ta funkcja pozwala rozszerzyć w widoku GridView `ManageRoles.aspx` uwzględnienie usunięcia przycisku, po kliknięciu Usuwa wybraną rolę. Rozpocznij od dodania przycisk usuwania do widoku GridView, przechodząc do okna dialogowego pól i dodawanie przycisk Usuń, który znajduje się w obszarze opcji CommandField. Należy usunąć przycisk kolumny w lewo i ustaw jej `DeleteText` właściwość, aby usunąć rolę.


[![Dodawanie przycisku Usuń do RoleList widoku GridView](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Rysunek 7**: Dodawanie przycisk Usuń, aby `RoleList` GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-and-managing-roles-cs/_static/image21.png))


Po dodaniu przycisk Usuń, deklaratywne znaczników w widoku GridView powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Następnie należy utworzyć program obsługi zdarzeń dla w widoku GridView `RowDeleting` zdarzeń. To jest zdarzenie, które jest wywoływane na odświeżenie strony, po kliknięciu przycisku Usuń rolę. Dodaj następujący kod do obsługi zdarzeń.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

Programowo odwołując zaczyna się kod `RoleNameLabel` formant w wierszu kliknął przycisk którego Usuń rolę sieci Web. `Roles.DeleteRole` Następnie wywoływana jest metoda, przekazując `Text` z `RoleNameLabel` i `false`, co usunąć rolę, niezależnie od tego, czy istnieją użytkowników skojarzone z rolą. Na koniec `RoleList` widoku GridView zostanie odświeżony, aby tylko usunięte rola nie jest już widoczna w siatce.

> [!NOTE]
> Przycisk Usuń rolę nie wymaga dowolny rodzaj potwierdzenie przez użytkownika przed usunięciem roli. Jednym z najprostszych sposobów potwierdzenie akcji jest za pomocą okna dialogowego Potwierdź po stronie klienta. Aby uzyskać więcej informacji na temat tej techniki, zobacz [Dodawanie potwierdzenie po stronie klienta podczas usuwania](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


## <a name="summary"></a>Podsumowanie

Wiele aplikacji sieci web ma niektórych reguł autoryzacji lub funkcji na poziomie strony jest dostępna tylko dla określonych klas użytkowników. Na przykład może być zestaw stron sieci web, do których dostęp tylko Administratorzy. Zamiast Definiowanie te reguły autoryzacji na podstawie użytkownika według, często jest bardziej użyteczne do definiowania reguł na podstawie roli. Oznacza to, zamiast jawnie zezwolenie użytkownikom Scott i Jisun dostęp administracyjny stron sieci web, więcej utrzymaniu podejście jest umożliwienie członkowie roli administratora dostęp do tych stron, a następnie do oznaczania Scott i Jisun jako należące do użytkowników Grupy administratorów.

W ramach ról ułatwia tworzenie i zarządzanie rolami. W tym samouczku będziemy zbadane sposobu skonfigurowania struktury role, aby użyć `SqlRoleProvider`, który korzysta z bazy danych programu Microsoft SQL Server do przechowywania roli. Utworzono również strony sieci web, który zawiera listę istniejących ról w systemie i zezwala na nowe role do utworzenia i istniejące do usunięcia. W kolejnych samouczkach zostanie wyświetlone sposobu przypisywania użytkowników do ról i sposób stosowania autoryzacji opartej na rolach.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Badanie ASP.NET 2.0 na członkostwo, role i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Porady: Korzystanie z menedżera ról w programie ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Dostawcy roli](https://msdn.microsoft.com/library/aa478950.aspx)
- [Wycofanie własne narzędzia administracyjnego witryny sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Dokumentacja techniczna dotycząca `<roleManager>` — Element](https://msdn.microsoft.com/library/ms164660.aspx)
- [Przy użyciu członkostwa i interfejsów API menedżera ról](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku obejmują Alicja Maziarz, Suchi Banerjee i Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Next](assigning-roles-to-users-cs.md)
