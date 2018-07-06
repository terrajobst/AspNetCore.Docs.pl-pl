---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: Przypisywanie ról do użytkowników (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku utworzymy dwie strony ASP.NET, aby ułatwić zarządzanie, jakie użytkownicy należą do jakich ról. Pierwsza strona będzie zawierać urządzenia, aby zobaczyć, co...
ms.author: aspnetcontent
ms.date: 03/24/2008
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: 41b73ec54ec2f174bca1d0ee2965ff8e0438ccba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839781"
---
<a name="assigning-roles-to-users-vb"></a>Przypisywanie ról do użytkowników (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> W tym samouczku utworzymy dwie strony ASP.NET, aby ułatwić zarządzanie, jakie użytkownicy należą do jakich ról. Pierwsza strona będzie zawierać urządzenia, aby zobaczyć, jakie użytkownicy należą do danej roli role, jakie jest dany użytkownik należy do oraz możliwość przypisać lub usunąć wybranego użytkownika z określoną rolę. Na drugiej stronie firma Microsoft będzie rozszerzyć kontroli CreateUserWizard aby obejmowała krok, aby określić, jakie role nowo utworzony użytkownik należy do. Jest to przydatne w scenariuszach, w którym administrator jest możliwość tworzenia nowych kont użytkowników.


## <a name="introduction"></a>Wprowadzenie

<a id="_msoanchor_1"> </a> [Poprzedniego samouczka](creating-and-managing-roles-vb.md) badany w ramach ról i `SqlRoleProvider`; widzieliśmy sposób używania `Roles` klasa do tworzenia, pobierania i usuwania ról. Oprócz tworzenia i usuwania ról, musimy mieć możliwość Przypisywanie lub usuwanie użytkowników z roli. Niestety program ASP.NET nie jest dostarczany z dowolnej kontrolki sieci Web do zarządzania, jakie użytkownicy należą do ról, jakie. Firma Microsoft zamiast tego należy utworzyć własną strony ASP.NET, aby zarządzać te skojarzenia. Dobra wiadomość jest tym dodawania i usuwania użytkowników do ról jest całkiem proste. `Roles` Klasa zawiera szereg metod dodawania co najmniej jednego użytkownika do co najmniej jedną rolę.

W tym samouczku utworzymy dwie strony ASP.NET, aby ułatwić zarządzanie, jakie użytkownicy należą do jakich ról. Pierwsza strona będzie zawierać urządzenia, aby zobaczyć, jakie użytkownicy należą do danej roli role, jakie jest dany użytkownik należy do oraz możliwość przypisać lub usunąć wybranego użytkownika z określoną rolę. Na drugiej stronie firma Microsoft będzie rozszerzyć kontroli CreateUserWizard aby obejmowała krok, aby określić, jakie role nowo utworzony użytkownik należy do. Jest to przydatne w scenariuszach, w którym administrator jest możliwość tworzenia nowych kont użytkowników.

Zaczynajmy!

## <a name="listing-what-users-belong-to-what-roles"></a>Lista użytkownicy należą do jakich ról

Pierwszym zadaniem biznesowym na potrzeby tego samouczka polega na utworzeniu strony sieci web, z którego użytkownicy mogą być przypisani do ról. Przed możemy określić główną przyczynę dotyczą jak przypisać użytkowników do ról, możemy najpierw koncentrować się na sposób określania, jakie użytkownicy należą do jakich ról. Istnieją dwa sposoby, aby wyświetlić te informacje: "za rolę" lub ""user. Umożliwiamy osoby odwiedzającej wybierz rolę, a następnie je pokazać wszystkich użytkowników, którzy należą do roli ("za rolę" Wyświetlanie) lub firma Microsoft może monitować obiektu odwiedzającego, aby wybrać użytkownika, a następnie wyświetlać je ról przypisanych do danego użytkownika ("przez użytkownika" Wyświetlanie).

Widok "za rolę" jest przydatna w sytuacjach, gdy użytkownik chce wiedzieć, zbiór użytkowników, które należą do określonej roli; Widok "przez użytkownika" jest idealnym rozwiązaniem w przypadku, gdy użytkownik musi wiedzieć, ról określonego użytkownika. Przyjrzyjmy się naszą stronę "za rolę" i "przez użytkownika" interfejsów.

Zaczniemy od utworzenia interfejsu "przez użytkownika". Ten interfejs będzie składać się z listy rozwijanej i listę pól wyboru. Listy rozwijanej zostanie wypełniony zbiór użytkowników w systemie; Zaznacz pola wyboru powoduje wyliczenie ról. Wybranie użytkownika z listy rozwijanej będzie sprawdzać te role, do której należy użytkownik. Osoby, odwiedzając stronę można, a następnie zaznacz lub usuń zaznaczenie pola wyboru, aby dodać lub usunąć wybranego użytkownika z odpowiednie role.

> [!NOTE]
> Korzystanie z rozwijanej listy do kont użytkowników nie jest idealnym wyborem w przypadku witryn sieci Web w przypadku, gdy mogą istnieć setki kont użytkowników. Listy rozwijanej umożliwia użytkownikowi wybrać jeden element z listy względnie krótkich opcji. Szybko staje się one nieporęczne za wraz ze wzrostem natężenia liczbę elementów listy. Jeśli tworzysz witrynę internetową, która będzie miał potencjalnie dużej liczby kont użytkowników, warto wziąć pod uwagę przy użyciu interfejsu użytkownika alternatywnych takie jak stronicowanej GridView lub filtrowanie interfejs, który zawiera listę poprosi list wyboru i następnie tylko Pokazuje tych użytkowników, których nazwa użytkownika, który rozpoczyna się od litery wybrane.


## <a name="step-1-building-the-by-user-user-interface"></a>Krok 1: Tworzenie interfejsu użytkownika "Przez użytkownika"

Otwórz `UsersAndRoles.aspx` strony. W górnej części strony, należy dodać formant etykiety w sieci Web o nazwie `ActionStatus` i wyczyszczenie jego `Text` właściwości. Użyjemy tej etykiety się opinią na temat akcji wykonywanych, wyświetlania komunikatów, takie jak, "Tito użytkownik został dodany do roli Administratorzy" lub "Jisun użytkownika zostało usunięte z roli nadzorców." Aby można było wprowadzać wyróżnienia wiadomości, ustaw właściwość etykiety `CssClass` właściwość "Ważne".

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

Następnie dodaj poniższą definicję klasy CSS do `Styles.css` arkusza stylów:

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Ta definicja CSS powoduje, że przeglądarka do wyświetlania etykiet przy użyciu czcionki dużych, red. Rysunek 1 pokazuje, w tym celu za pomocą projektanta programu Visual Studio.


[![Właściwość CssClass etykiety powoduje czcionki dużych, czerwony](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Rysunek 1**: Etykieta `CssClass` właściwości powoduje duże, czcionka Red ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image3.png))


Następnie dodaj kontrolki DropDownList do strony, ustaw jego `ID` właściwości `UserList`i ustaw jego `AutoPostBack` właściwości na wartość True. Użyjemy tej metody DropDownList, aby wyświetlić listę wszystkich użytkowników w systemie. Kolekcja obiektów MembershipUser będą powiązane tej metody DropDownList. Ponieważ chcemy, aby DropDownList do wyświetlania właściwości nazwy użytkownika obiektu MembershipUser (i używać go jako wartość elementów listy), należy ustawić DropDownList `DataTextField` i `DataValueField` właściwości "Nazwa_użytkownika".

Poniżej metody DropDownList, Dodaj Repeater o nazwie `UsersRoleList`. Tego elementu powtarzanego spowoduje wyświetlenie listy wszystkich ról w systemie jako serię pola wyboru. Zdefiniuj Repeater `ItemTemplate` przy użyciu następujących oznaczeniu deklaracyjnym:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

`ItemTemplate` Znaczników zawiera jeden formant pola wyboru w sieci Web o nazwie `RoleCheckBox`. Pole wyboru `AutoPostBack` właściwość jest ustawiona na wartość True i `Text` właściwość jest powiązana z `Container.DataItem`. Przyczyna składnia wiązania danych jest po prostu `Container.DataItem` ponieważ framework role zwraca listę nazw ról w postaci tablicy ciągów i jest to tablica ciągu, który firma Microsoft będzie powiązywanie powtarzanego. Dokładny opis dlaczego ta składnia jest używana do wyświetlania zawartości tablicy powiązany z formantu sieci Web danych wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji dotyczących tej kwestii, zobacz [powiązanie tablicy skalarną z formantu sieci Web danych](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

W tym momencie "przez użytkownika" interfejsu w oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

Teraz możemy przystąpić do pisania kodu, aby powiązać zbiór kont użytkowników do metody DropDownList i zestaw ról na potrzeby powtarzanego. W klasie CodeBehind strony, Dodaj metodę o nazwie `BindUsersToUserList` i drugiego o nazwie `BindRolesList`, używając następującego kodu:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

`BindUsersToUserList` Metoda pobiera wszystkie konta użytkownika systemu za pośrednictwem [ `Membership.GetAllUsers` metoda](https://msdn.microsoft.com/library/dy8swhya.aspx). Spowoduje to zwrócenie [ `MembershipUserCollection` obiektu](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), który jest kolekcją [ `MembershipUser` wystąpień](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Ta kolekcja jest następnie powiązany z `UserList` DropDownList. `MembershipUser` Wystąpień tego korzeń kolekcji zawiera szereg właściwości, takich jak `UserName`, `Email`, `CreationDate`, i `IsOnline`. Aby można było wydać polecenie DropDownList, aby wyświetlić wartość `UserName` właściwości, upewnij się, że `UserList` firmy DropDownList `DataTextField` i `DataValueField` właściwości zostały ustawione na "Nazwa_użytkownika".

> [!NOTE]
> `Membership.GetAllUsers` Metoda ma dwa przeciążenia: jedną, która przyjmuje nie parametrów wejściowych i zwraca wszystkich użytkowników oraz jedna, która przyjmuje wartości całkowitoliczbowe dla indeks strony i rozmiar strony, a następnie zwraca określony podzbiór użytkowników. W przypadku dużych ilości konta użytkowników są wyświetlane w elemencie interfejsu użytkownika stronicowanej drugie przeciążenie można efektywniej strony za pomocą użytkowników, ponieważ zwraca ono tylko dokładne podzbiór kont użytkowników, a nie dla wszystkich z nich.


`BindRolesToList` Metoda uruchamia się przez wywołanie metody `Roles` klasy [ `GetAllRoles` metoda](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), która zwraca tablicę ciągów, zawierający role w systemie. Ta tablica ciągów następnie jest powiązany z elementu powtarzanego.

Na koniec należy wywołać te dwie metody, gdy strona jest ładowana jako pierwsza. Dodaj następujący kod do `Page_Load` program obsługi zdarzeń:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

Przy użyciu tego kodu w miejscu Poświęć chwilę na stronę za pośrednictwem przeglądarki; ekran powinien wyglądać podobnie jak na rysunku 2. Wszystkie konta użytkowników zostaną wypełnione na liście rozwijanej, a poniżej, każda rola ma postać pola wyboru. Ponieważ ustawiliśmy `AutoPostBack` właściwości kontrolki DropDownList i pola wyboru o wartości True, zmiana wybranego użytkownika lub zaznaczenie lub usunięcie zaznaczenia roli powoduje odświeżenie strony. Jest wykonywana żadna akcja, jednak, ponieważ trzeba jeszcze napisać kod, aby obsługiwać te akcje. Firma Microsoft będzie czoła tych zadań w dwóch następnych sekcjach.


[![Zostanie wyświetlona strona użytkownikami i rolami](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Rysunek 2**: zostanie wyświetlona strona Użytkownicy i role ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Sprawdzanie role wybranego użytkownika należy do

Po pierwszym załadowaniu strony lub zawsze wtedy, gdy użytkownik wybiera nowego użytkownika z listy rozwijanej, musimy zaktualizować `UsersRoleList`firmy pola wyboru, aby tylko wtedy, gdy wybrany użytkownik należy do tej roli, zaznaczone jest pole wyboru danej roli. W tym celu należy utworzyć metodę o nazwie `CheckRolesForSelectedUser` następującym kodem:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

Powyższy kod uruchamia, ustalając będący wybranego użytkownika. Następnie używa klasy ról [ `GetRolesForUser(userName)` metoda](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) do zwrócenia określonego użytkownika zestawu ról w postaci tablicy ciągów. Następnie są wyliczane elementów elementu powtarzanego i każdego elementu `RoleCheckBox` programowo odwołuje się do pola wyboru. Pole wyboru jest zaznaczone, tylko wtedy, gdy rola odnosi się do znajduje się w `selectedUsersRoles` tablicy ciągów.

> [!NOTE]
> `Linq.Enumerable.Contains(Of String)(...)` Składni nie zostanie skompilowany, jeśli używasz programu ASP.NET w wersji 2.0. `Contains(Of String)` Metody jest częścią [biblioteki LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), który jest nowym składnikiem w programie ASP.NET 3.5. Jeśli nadal używasz platformę ASP.NET w wersji 2.0, użyj [ `Array.IndexOf(Of String)` metoda](https://msdn.microsoft.com/library/eha9t187.aspx) zamiast tego.


`CheckRolesForSelectedUser` Metoda musi zostać wywołana w dwóch przypadkach: po pierwszym załadowaniu strony i zawsze wtedy, gdy `UserList` wybranego indeksu DropDownList firmy zostanie zmieniony. W związku z tym, wywołanie tej metody z `Page_Load` programu obsługi zdarzeń (po wywołania `BindUsersToUserList` i `BindRolesToList`). Ponadto utworzyć program obsługi zdarzeń dla metody DropDownList `SelectedIndexChanged` zdarzenia i wywołać tę metodę w tym miejscu.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

Przy użyciu tego kodu w miejscu można przetestować stronę za pośrednictwem przeglądarki. Jednak ponieważ `UsersAndRoles.aspx` strony aktualnie brakuje możliwość przypisywania użytkowników do ról, żaden użytkownik nie ma ról. Utworzymy interfejs do przypisywania użytkowników do ról za chwilę, dzięki czemu możesz pobrać program word, ten kod działa i sprawdź, czy robi to później, lub można ręcznie dodawać użytkowników do ról, wstawiając rekordów w `aspnet_UsersInRoles` tabeli w celu przetestowania tej functi teraz onality.

### <a name="assigning-and-removing-users-from-roles"></a>Przypisywanie i usuwanie użytkowników z ról

Gdy obiekt odwiedzający sprawdza lub usuwa zaznaczenie pola wyboru w `UsersRoleList` musimy dodać lub usunąć wybranego użytkownika z roli odpowiedniego elementu powtarzanego. Pole wyboru `AutoPostBack` jest obecnie ustawioną wartość true, co powoduje odświeżenie strony, w dowolnym momencie wyboru w elemencie powtarzanym jest zaznaczony. Krótko mówiąc, musimy utworzyć program obsługi zdarzeń dla pola wyboru `CheckChanged` zdarzeń. Ponieważ jest to pole wyboru w kontrolce elementu powtarzanego, należy ręcznie dodać podstawami programu obsługi zdarzeń. Rozpocznij od dodania obsługi zdarzeń do klasy CodeBehind jako `Protected` metody, w następujący sposób:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

Firma Microsoft zwróci napisać kod dla tego programu obsługi zdarzeń w kilka minut. Jednak najpierw zakończmy nadmiar obsługi zdarzeń. Z pola wyboru w elemencie powtarzanym `ItemTemplate`, Dodaj `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Ta składnia przewody `RoleCheckBox_CheckChanged` procedurę obsługi zdarzeń do `RoleCheckBox`firmy `CheckedChanged` zdarzeń.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

Nasze ostatnim zadaniem jest realizacja `RoleCheckBox_CheckChanged` programu obsługi zdarzeń. Potrzebujemy rozpocząć, odwołując się do formantu CheckBox, która wywołała zdarzenie, ponieważ to wystąpienie wyboru informuje NAS, jaką rolę został zaznaczony lub niezaznaczony za pośrednictwem jego `Text` i `Checked` właściwości. Przy użyciu tych informacji oraz nazwa użytkownika dla wybranego użytkownika, możemy dodać albo go usunąć z roli za pomocą `Roles` klasy [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) lub [ `RemoveUserFromRole` metoda](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

Powyższy kod, który rozpoczyna się od programowe odwoływanie się do pola wyboru, która wywołała zdarzenie, które jest dostępne za pośrednictwem `sender` parametr wejściowy. Jeśli pole wyboru jest zaznaczone, wybrany użytkownik zostanie dodany do określonej roli, w przeciwnym razie zostaną usunięte z roli. W obu przypadkach `ActionStatus` etykieta zostanie wyświetlony komunikat podsumowania akcję wykonywaną po prostu.

Poświęć chwilę, w celu przetestowania tej strony za pośrednictwem przeglądarki. Wybierz użytkownika Tito, a następnie dodaj Tito do ról administratorów i nadzorców.


[![Tito została dodana do administratorów i ról nadzorcy](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Rysunek 3**: Tito została dodana do administratorów i ról nadzorców ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image9.png))


Następnie wybierz użytkownika Bruce z listy rozwijanej. Brak odświeżenie strony i pól wyboru Repeater są aktualizowane przy użyciu `CheckRolesForSelectedUser`. Ponieważ Bruce nie należy jeszcze do żadnej roli, dwa pola wyboru są nie zaznaczone. Następnie dodaj Bruce do roli nadzorców.


[![Bruce został dodany do roli nadzorcy](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Rysunek 4**: Bruce został dodany do roli nadzorców ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image12.png))


Dodatkową weryfikację funkcjonalności `CheckRolesForSelectedUser` metody, wybierz użytkownika innych niż Tito lub Bruce. Należy zauważyć, jak automatycznie nie są zaznaczone pola wyboru, oznaczające, że należą one do żadnej roli. Wróć do Tito. Powinno być zaznaczone pola wyboru zarówno Administratorzy, jak i nadzorców.

## <a name="step-2-building-the-by-roles-user-interface"></a>Krok 2: Tworzenie interfejsu użytkownika "Przez role"

W tym momencie możemy zostały ukończone interfejsu "przez"Użytkownicy i gotowe do rozpoczęcia co dzień do czynienia interfejsu "przez role". Interfejs "przez role" monituje użytkownika o wybierz rolę z listy rozwijanej, a następnie wyświetla zbiór użytkowników, którzy należą do tej roli w widoku GridView.

Dodaj inny formant DropDownList `UsersAndRoles.aspx page`. Umieść ten jednej kontrolce elementu powtarzanego, nadaj jej nazwę `RoleList`i ustaw jego `AutoPostBack` właściwości na wartość True. Poniżej, Dodaj GridView i nadaj mu nazwę `RolesUserList`. Ta GridView wyświetli listę użytkowników, którzy należą do wybranej roli. Ustaw GridView `AutoGenerateColumns` siatki właściwości na wartość False, dodane TemplateField `Columns` kolekcji, a następnie ustaw jego `HeaderText` właściwość "Użytkownicy". Zdefiniuj TemplateField `ItemTemplate` tak, aby wyświetlał wartość wyrażenia wiązania danych `Container.DataItem` w `Text` właściwość etykietę o nazwie `UserNameLabel`.

Po dodaniu i konfigurowanie kontrolki GridView, "za rolę" interfejsu w oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

Należy wypełnić `RoleList` DropDownList przy użyciu zestawu ról w systemie. Aby to zrobić, należy zaktualizować `BindRolesToList` metody, dlatego ten typ powiązania przy użyciu tablicy ciągów zwrócony przez `Roles.GetAllRoles` metodę, aby `RolesList` DropDownList (także `UsersRoleList` elementu powtarzanego).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

Ostatnie dwa wiersze w `BindRolesToList` metody zostały dodane do zestawu ról, aby powiązać `RoleList` kontrolki DropDownList. Rysunek 5. pokazuje wynik końcowy podczas wyświetlania za pośrednictwem przeglądarki — listy rozwijanej, wypełnione z rolami systemu.


[![Role są wyświetlane w RoleList DropDownList](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Rysunek 5**: role są wyświetlane w `RoleList` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Wyświetlanie użytkowników, którzy należą do wybranej roli

Po pierwszym załadowaniu strony lub po wybraniu nowej roli z `RoleList` DropDownList, należy wyświetlić listę użytkowników, którzy należą do tej roli w widoku GridView. Utwórz metodę o nazwie `DisplayUsersBelongingToRole` przy użyciu następującego kodu:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Ta metoda rozpoczyna uzyskując wybranej roli z `RoleList` DropDownList. Następnie używa [ `Roles.GetUsersInRole(roleName)` metoda](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) można pobrać ciągu na tablicę nazw użytkowników, którzy należą do tej roli. Ta tablica jest następnie powiązany z `RolesUserList` GridView.

Ta metoda musi zostać wywołana w dwóch przypadkach: podczas wczytywania strony i wybranej roli w `RoleList` DropDownList zmiany. W związku z tym, zaktualizuj `Page_Load` programu obsługi zdarzeń, aby ta metoda jest wywoływana po wywołaniu `CheckRolesForSelectedUser`. Następnie należy utworzyć program obsługi zdarzeń dla `RoleList`firmy `SelectedIndexChanged` zdarzenia i zbyt wywołać tę metodę z tego miejsca.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

Przy użyciu tego kodu w miejscu `RolesUserList` GridView powinien być wyświetlany użytkowników, którzy należą do wybranej roli. Jak pokazano na rysunku 6, rola nadzorców składa się z dwóch elementów: Bruce i Tito.


[![Kontrolki GridView zawiera listę użytkowników, którzy należą do wybranej roli](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Rysunek 6**: GridView zawiera listę tych użytkowników, należy do wybrane roli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Usuwanie użytkowników w wybranej roli

Możemy rozszerzyć `RolesUserList` przyciski GridView aby obejmowała kolumny "Usuń". Klikając przycisk "Usuń" dla danego użytkownika spowoduje usunięcie ich z tej roli.

Rozpocznij, dodając pola przycisku usuwania do kontrolki GridView. Należy to pole, które są wyświetlane jako najbardziej zachowanej po lewej stronie i zmień jego `DeleteText` właściwość "Delete" (wartość domyślna) "Usuń".


[![Dodaj](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Rysunek 7**: Dodawanie przycisk "Usuń" do kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image21.png))


Po kliknięciu przycisku "Usuń" ensues odświeżenie strony i GridView `RowDeleting` zdarzenie jest wywoływane. Potrzebujemy utworzyć program obsługi zdarzeń dla tego zdarzenia i napisać kod, który usuwa użytkownika z wybranej roli. Utwórz procedurę obsługi zdarzeń, a następnie dodaj następujący kod:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

Zaczyna się kod poprzez określenie nazwy wybranej roli. Następnie programowo odwołania `UserNameLabel` sterowania z wiersza, w której przycisk "Usuń" został kliknięty w celu ustalenia, nazwa użytkownika do usunięcia. Ten użytkownik zostaje następnie usunięty z roli poprzez wywołanie `Roles.RemoveUserFromRole` metody. `RolesUserList` GridView jest następnie odświeżona i zostanie wyświetlony komunikat za pośrednictwem `ActionStatus` kontrolka etykiety.

> [!NOTE]
> Przycisk "Usuń" nie jest wymagane dowolny rodzaj potwierdzenie od użytkownika przed usunięciem użytkownika z roli. Zapraszam WAS do dodania pewien stopień potwierdzenie przez użytkownika. Jednym z najprostszych sposobów, aby potwierdzić akcję jest poprzez okno dialogowe Potwierdź po stronie klienta. Aby uzyskać więcej informacji na temat tej techniki, zobacz [dodawanie potwierdzenia po stronie klienta podczas usuwania](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Rysunek 8 przedstawia stronę po użytkownik Tito został usunięty z grupy nadzorców.


[![Niemniej Tito nie jest już nadzorcy](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Rysunek 8**: Niestety Tito nie jest już nadzorcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Dodawanie nowych użytkowników do wybranej roli

Oraz usuwanie użytkowników w wybranej roli, odwiedzający do tej strony powinny również można dodać użytkownika do wybranej roli. Najlepsze interfejsu związanych z dodawaniem użytkowników do wybranej roli, zależy od liczby kont użytkowników, których spodziewasz się. Jeśli witryny sieci Web będzie przechowywać kilka kont użytkowników kilkudziesięciu lub mniej, można użyć kontrolki DropDownList w tym miejscu. Może być tysiące konta użytkowników, należy uwzględnić interfejs użytkownika, który pozwala na obiekt odwiedzający do strony za pomocą konta, wyszukaj określonego konta, lub filtrować konta użytkownika w dowolny sposób innych.

Na tej stronie Użyjmy bardzo prosty interfejs, który działa niezależnie od liczby kont użytkowników w systemie. To znaczy będziemy używać pole tekstowe monitowania użytkownika wpisz nazwę użytkownika, którego chce dodać do wybranej roli. Jeśli żaden użytkownik o tej nazwie istnieje, lub jeśli użytkownik jest już członkiem roli, firma Microsoft będzie wyświetlenie komunikatu w `ActionStatus` etykiety. Jednak jeśli użytkownik istnieje i nie jest elementem członkowskim roli, utworzymy je dodać do roli i Odśwież siatki.

Dodaj pole tekstowe i przycisk poniżej kontrolki GridView. Ustaw pole tekstowe `ID` do `UserNameToAddToRole` i ustaw właściwość `ID` i `Text` właściwości `AddUserToRoleButton` i "Dodawanie użytkownika do roli", odpowiednio.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

Następnie należy utworzyć `Click` program obsługi zdarzeń dla `AddUserToRoleButton` i Dodaj następujący kod:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

Większość kodu w `Click` programu obsługi zdarzeń wykonuje różne sprawdzanie poprawności. Gwarantuje to, że odwiedzający podano nazwę użytkownika w `UserNameToAddToRole` pola tekstowego, czy użytkownik istnieje w systemie i czy już nie należą do wybranej roli. Jeśli któregoś z powyższych sprawdza, czy kończy się niepowodzeniem, odpowiedni komunikat jest wyświetlany w `ActionStatus` i program obsługi zdarzeń jest został zakończony. Jeśli wszystkie testy zostaną wykryte, użytkownik zostanie dodany do roli przy użyciu `Roles.AddUserToRole` metody. Poniżej pola tekstowego firmy `Text` właściwość jest wyczyszczone, odświeżeniu widoku GridView i `ActionStatus` Etykieta wyświetla komunikat informujący, że podany użytkownik zostało pomyślnie dodane do wybranej roli.

> [!NOTE]
> Aby upewnić się, że podany użytkownik należy już do wybranej roli, użyjemy [ `Roles.IsUserInRole(userName, roleName)` metoda](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), która zwraca wartość logiczną wskazującą czy *userName* jest elementem członkowskim *roleName*. Firma Microsoft użyje tej metody, ponownie w <a id="_msoanchor_2"> </a> [następnego samouczka](role-based-authorization-vb.md) Jeśli przyjrzymy się autoryzacji opartej na rolach.


Odwiedź stronę za pośrednictwem przeglądarki, a następnie wybierz rolę nadzorców z `RoleList` DropDownList. Spróbuj wprowadzić nieprawidłowej nazwy użytkownika — powinien zostać wyświetlony komunikat wyjaśniający, że użytkownik nie istnieje w systemie.


[![Nie można dodać nieistniejącego użytkownika do roli](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Rysunek 9**: nie można dodać użytkownika nieistniejąca do roli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image27.png))


Teraz należy spróbować dodać prawidłowego użytkownika. Przejdź dalej i ponownie dodać Tito do roli nadzorców.


[![Tito jest ponownie nadzorcy!](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Na rysunku nr 10**: Tito jest ponownie nadzorcy!  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Krok 3: Aktualizowanie wielu "Przez użytkownika" i "Za rolę" interfejsów

`UsersAndRoles.aspx` Strona oferuje dwa różne interfejsy do zarządzania użytkownikami i rolami. Obecnie te dwa interfejsy działają niezależnie od siebie, więc istnieje możliwość, że zmiany wprowadzone w jednym interfejsie zostaną nie natychmiast odzwierciedlone w innych. Załóżmy, że obiekt odwiedzający do strony wybiera rolę nadzorców z `RoleList` DropDownList, w którym jest wyświetlana lista Bruce i Tito jako elementy członkowskie. Następnie użytkownik wybiera Tito z `UserList` DropDownList, która sprawdza administratorów i nadzorców pól wyboru w `UsersRoleList` elementu powtarzanego. Jeśli obiekt odwiedzający następnie usuwa zaznaczenie nadzorcy rola w elemencie powtarzanym, Tito jest usuwany z roli nadzorców, ale ta modyfikacja nie jest odzwierciedlana w interfejsie "za rolę". Kontrolki GridView w dalszym ciągu będzie Tito jako członek roli nadzorców.

Aby rozwiązać ten musimy odświeżyć widoku GridView zawsze wtedy, gdy rola zaznaczony lub niezaznaczony z `UsersRoleList` elementu powtarzanego. Podobnie musimy odświeżyć powtarzanego zawsze wtedy, gdy użytkownik jest usunięte lub dodane do roli z poziomu interfejsu "za rolę".

Powtarzanego w interfejsie "przez użytkownika" są odświeżane, wywołując `CheckRolesForSelectedUser` metody. Interfejs "za rolę" mogą być modyfikowane w `RolesUserList` GridView `RowDeleting` programu obsługi zdarzeń i `AddUserToRoleButton` przycisku `Click` programu obsługi zdarzeń. W związku z tym, należy wywołać `CheckRolesForSelectedUser` metody z każdej z tych metod.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

Podobnie, GridView w interfejsie "za rolę" są odświeżane, wywołując `DisplayUsersBelongingToRole` metody i interfejsu "przez użytkownika" jest modyfikowany przez `RoleCheckBox_CheckChanged` programu obsługi zdarzeń. W związku z tym, należy wywołać `DisplayUsersBelongingToRole` metody z tej obsługi zdarzeń.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

Za pomocą tych zmian kodu "przez użytkownika" i "za rolę" interfejsy teraz poprawnie wielu aktualizacji. Aby to sprawdzić, odwiedź stronę za pośrednictwem przeglądarki, a następnie wybierz Tito i udostępniania Nadzorcom z `UserList` i `RoleList` kontrolek DropDownList, odpowiednio. Należy zauważyć, że ponieważ usuniesz zaznaczenie pola wyboru roli nadzorców dla Tito z elementu powtarzanego w interfejsie "przez użytkownika", Tito zostanie automatycznie usunięty z GridView w interfejsie "za rolę". Ponowne dodanie Tito powrót do opiekunów roli z interfejsu "za rolę" automatycznie sprawdza, czy nadzorców pole wyboru w interfejsie "przez użytkownika".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Krok 4: Dostosowywanie CreateUserWizard obejmujący kroku "Określ role"

W <a id="_msoanchor_3"> </a> [ *tworzenie kont użytkowników* ](../membership/creating-user-accounts-vb.md) samouczek widzieliśmy, jak używać kontrolki CreateUserWizard sieci Web udostępnia interfejs do tworzenia nowego konta użytkownika. Kontrolka CreateUserWizard może służyć w jeden z dwóch sposobów:

- Co oznacza, że dla użytkowników zewnętrznych utworzyć własne konto użytkownika w witrynie i
- Co oznacza, że administratorzy mogą tworzyć nowe konta

W pierwszym przypadku użycia obiekt odwiedzający pochodzi z lokacją i wypełnia CreateUserWizard, wprowadzając swoje informacje do zarejestrowania się w witrynie. W drugim przypadku administrator tworzy nowe konto dla innej osoby.

Gdy konto jest tworzona przez administratora na rzecz innych osób, może być przydatne umożliwić administratorowi Określ role, jakie nowe konto użytkownika należy do. W <a id="_msoanchor_4"> </a> [ *przechowywanie* *dodatkowe informacje o użytkowniku* ](../membership/storing-additional-user-information-vb.md) samouczek widzieliśmy sposobu dostosowywania CreateUserWizard przez dodanie dodatkowych `WizardSteps`. Przyjrzyjmy się jak dodać dodatkowy krok do CreateUserWizard, aby określić ról nowego użytkownika.

Otwórz `CreateUserWizardWithRoles.aspx` strony, a następnie dodaj formancie CreateUserWizard o nazwie `RegisterUserWithRoles`. Ustaw dla formantu `ContinueDestinationPageUrl` właściwość "~ / Default.aspx". Ponieważ w tym miejscu chodzi o to, że administrator będzie używany tej kontrolki CreateUserWizard do tworzenia nowych kont użytkowników, ustawić `LoginCreatedUser` wartość False dla właściwości. To `LoginCreatedUser` właściwość określa, czy obiekt odwiedzający automatycznie jest zalogowany jako użytkownik nowo utworzoną i jego wartość domyślna to True. Możemy ustawić wartość False, ponieważ gdy administrator tworzy nowe konto chcemy zapewnić mu zalogowano jako siebie.

Następnie wybierz pozycję "Dodaj/Usuń `WizardSteps`..." w tagu inteligentnego CreateUserWizard i Dodaj nową `WizardStep`, ustawiając jego `ID` do `SpecifyRolesStep`. Przenieś `SpecifyRolesStep WizardStep` tak, aby nastąpi po wykonaniu kroku "Logowania konto nowego konta", ale przed wykonaniem kroku "Ukończony". Ustaw `WizardStep`firmy `Title` właściwość "Określ role", jego `StepType` właściwości `Step`i jego `AllowReturn` wartość False dla właściwości.


[![Dodaj](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Rysunek 11**: Dodaj "Określ role" `WizardStep` do CreateUserWizard ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image33.png))


Po tej zmianie swoje CreateUserWizard oznaczeniu deklaracyjnym powinien wyglądać następująco:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

"Określ role" `WizardStep`, Dodaj do elementu CheckBoxList o nazwie `RoleList.` CheckBoxList ten zostanie wyświetlona lista dostępnych ról, umożliwiając osoby odwiedzając stronę sprawdzić, jakie role nowo utworzony użytkownik należy do.

Firma Microsoft są pozostawiane z dwóch zadań kodowania: najpierw musisz wypełnimy `RoleList` CheckBoxList przy użyciu ról w systemie; po drugie, należy dodać do wybranych ról utworzonego użytkownika, gdy użytkownik przesuwa się w kroku "Określ role" do kroku "Zakończono". Firma Microsoft może wykonać pierwsze zadanie w `Page_Load` programu obsługi zdarzeń. Poniższy kod programowo odwołania `RoleList` wyboru pierwszego można znaleźć na stronie i powiązanie ról w systemie go.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

Powyższy kod powinien wyglądać znajomo. W <a id="_msoanchor_5"> </a> [ *przechowywanie* *dodatkowe informacje o użytkowniku* ](../membership/storing-additional-user-information-vb.md) samouczka użyliśmy dwa `FindControl` instrukcje, aby odwoływać się do formantu sieci Web z w ramach niestandardowego `WizardStep`. I kod, który wiąże role CheckBoxList została pobrana z wcześniej w tym samouczku.

Aby można było wykonać drugie zadanie programowania, musimy wiedzieć, kiedy zostały ukończone w kroku "Określ role". Odwołania, który ma CreateUserWizard `ActiveStepChanged` zdarzenie, które są generowane każdorazowo poruszania się od jednego kroku do innego. W tym miejscu możemy określić, jeśli użytkownik osiągnął kroki "Zakończono". Jeśli tak, należy dodać użytkownika do wybranych ról.

Utwórz procedurę obsługi zdarzeń dla `ActiveStepChanged` zdarzeń i Dodaj następujący kod:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Jeśli użytkownik po prostu osiągnął kroku "Completed", program obsługi zdarzeń wylicza elementy `RoleList` CheckBoxList a nowo utworzoną użytkownik jest przypisany do wybranych ról.

Odwiedź tę stronę za pośrednictwem przeglądarki. Pierwszym etapem CreateUserWizard jest kroku "Logowania konto nowego konta" Standardowa monituje o podanie nazwy użytkownika nowego użytkownika, hasło, adres e-mail i inne informacje o kluczu. Wprowadź informacje, aby utworzyć nowego użytkownika o nazwie Wanda.


[![Tworzenie nowego użytkownika o nazwie Wanda](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Rysunek 12**: Tworzenie nowego Wanda o nazwie użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image36.png))


Kliknij przycisk "Create User". Wywołuje wewnętrznie CreateUserWizard `Membership.CreateUser` metody tworzenia nowego konta użytkownika, a następnie następuje przejście do następnego kroku "Określ role." W tym miejscu są wyświetlane role systemu. Zaznacz pole wyboru Nadzorcom i kliknij przycisk Dalej.


[![Wprowadzić Wanda jako członka roli nadzorcy](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Rysunek 13**: wprowadzić Wanda jako członka roli nadzorców ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image39.png))


Jeśli klikniesz pozycję dalej powoduje odświeżenie strony i aktualizacje `ActiveStep` do kroku "Zakończono". W `ActiveStepChanged` ostatnio utworzone konto programu obsługi zdarzeń jest przypisywana rola nadzorców. Aby to sprawdzić, wróć do `UsersAndRoles.aspx` strony i wybierz nadzorców z `RoleList` DropDownList. Jak pokazano na rysunku 14, nadzorców teraz składają się z trzech użytkowników: Bruce Tito i Wanda.


[![Bruce, Tito i Wanda są wszystkie nadzorcy](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Rysunek 14**: Bruce, Tito i Wanda są wszystkie nadzorców ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>Podsumowanie

W ramach ról zapewnia metody do pobierania informacji o rolach i metody określania, jakie użytkownicy należą do określonej roli danego użytkownika. Ponadto istnieje szereg metod dodawania i usuwania co najmniej jednego użytkownika do co najmniej jedną rolę. W tym samouczku skupiliśmy się na tylko dwóch z następujących metod: `AddUserToRole` i `RemoveUserFromRole`. Istnieją dodatkowe wariantów przeznaczony do dodawania wielu użytkowników do jednej roli i przypisać role wiele do jednego użytkownika.

Ten samouczek zawiera również przyjrzeć się rozszerzanie kontroli CreateUserWizard obejmujący `WizardStep` umożliwia określanie ról nowo utworzonego użytkownika. Taki krok może pomóc administratorowi usprawnienie procesu tworzenia kont użytkowników dla nowych użytkowników.

W tym momencie Zaobserwowaliśmy, jak tworzyć i usuwać role i jak dodawać i usuwać użytkowników z ról. Ale mamy jeszcze do wzięcia pod stosowania autoryzacji opartej na rolach. W <a id="_msoanchor_6"> </a> [kolejnym samouczku](role-based-authorization-vb.md) przyjrzymy się definiowania reguł autoryzacji adresów URL na podstawie roli według roli oraz jak ograniczyć funkcjonalność na poziomie strony na podstawie ról aktualnie zalogowanego użytkownika.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Omówienie narzędzia do administrowania witryny sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Badanie ASP. Członkostwo w sieci, ról i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Stopniowe własne narzędzia do administrowania witryny sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](creating-and-managing-roles-vb.md)
> [dalej](role-based-authorization-vb.md)
