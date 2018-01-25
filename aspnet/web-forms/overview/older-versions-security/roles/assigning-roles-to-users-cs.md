---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
title: "Przypisywanie ról do użytkowników (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku budujemy dwie strony ASP.NET, aby ułatwić zarządzanie, jakie użytkownicy należą do role. Pierwsza strona będzie zawierać urządzenia, aby zobaczyć..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: d522639a-5aca-421e-9a76-d73f95607f57
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
msc.type: authoredcontent
ms.openlocfilehash: 15d2b427e6fccfc82eab535200ba6878ab41b72e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="assigning-roles-to-users-c"></a>Przypisywanie ról do użytkowników (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.10.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_cs.pdf)

> W tym samouczku budujemy dwie strony ASP.NET, aby ułatwić zarządzanie, jakie użytkownicy należą do role. Pierwsza strona będzie zawierać urządzenia, aby zobaczyć, jakie użytkownicy należą do danej roli, jakie role dany użytkownik należy do oraz możliwość Przypisywanie lub usuwanie określonego użytkownika z określoną rolę. Na drugiej stronie firma Microsoft będzie rozszerzyć formancie CreateUserWizard co zawierają one krok przypisuje się role nowo utworzony użytkownik należy do. Jest to przydatne w scenariuszach, w którym administrator jest możliwość tworzenia nowych kont użytkowników.


## <a name="introduction"></a>Wprowadzenie

<a id="_msoanchor_1"> </a> [Poprzedniego samouczek](creating-and-managing-roles-cs.md) zbadane framework ról i `SqlRoleProvider`; widzieliśmy sposób użycia `Roles` klasa do tworzenia, pobierania i usuwania ról. Oprócz tworzenia i usuwania ról, należy mieć możliwość Przypisywanie lub usuwanie użytkowników z roli. Niestety program ASP.NET nie jest dostarczany z dowolnego formantów sieci Web do zarządzania, jakie użytkownicy należą do role. Zamiast tego należy utworzymy własnej stron ASP.NET, aby zarządzać skojarzenia. Dobre wieści jest, że dodawanie i usuwanie użytkowników do ról jest dość proste. `Roles` Klasa zawiera wiele metod dodawania jednego lub kilku użytkowników do co najmniej jedną rolę.

W tym samouczku budujemy dwie strony ASP.NET, aby ułatwić zarządzanie, jakie użytkownicy należą do role. Pierwsza strona będzie zawierać urządzenia, aby zobaczyć, jakie użytkownicy należą do danej roli, jakie role dany użytkownik należy do oraz możliwość Przypisywanie lub usuwanie określonego użytkownika z określoną rolę. Na drugiej stronie firma Microsoft będzie rozszerzyć formancie CreateUserWizard co zawierają one krok przypisuje się role nowo utworzony użytkownik należy do. Jest to przydatne w scenariuszach, w którym administrator jest możliwość tworzenia nowych kont użytkowników.

Dzieła!

## <a name="listing-what-users-belong-to-what-roles"></a>Wyświetlanie co użytkownicy należą do jakiego ról

Pierwszy biznesowych w kolejności w tym samouczku jest utworzyć stronę sieci web, z której użytkownicy mogą być przypisani do ról. Zanim firma Microsoft dotyczą nad jak do przypisywania użytkowników do ról umożliwia najpierw skoncentrować się na sposób określania, jakie użytkownicy należą do role. Istnieją dwa sposoby wyświetlania tych informacji: "według roli" lub "przez użytkownika." Firma Microsoft może umożliwić użytkownika wybierz rolę, a następnie je wyświetlić wszystkich użytkowników, którzy należą do roli ("według roli" Wyświetlanie), lub firma Microsoft może monitować obiektu odwiedzającego, aby wybrać użytkownika, a następnie wyświetlać je role przypisane do tego użytkownika ("przez użytkownika" Wyświetlanie).

Widok "według roli" jest przydatna w sytuacjach, gdy użytkownik chce wiedzieć zbiór użytkowników, którzy należą do określonej roli; Widok "przez użytkownika" jest idealne w przypadku, gdy użytkownik musi wiedzieć, ról określonego użytkownika. Załóżmy ma naszą stronę obejmujące "według roli" i "przez użytkownika" interfejsów.

Firma Microsoft rozpoczyna się od tworzenia interfejsu "przez użytkownika". Ten interfejs będzie składać się z listy rozwijanej i listy pól wyboru. Listy rozwijanej zostanie wypełniona zbiór użytkowników w systemie; pola wyboru powoduje wyliczenie ról. Wybór użytkownika z listy rozwijanej będzie sprawdzać tych ról, do której należy użytkownik. Osoby, odwiedzając stronę, a następnie sprawdzić, czy lub usuń zaznaczenie pola wyboru, aby dodać lub usunąć wybranego użytkownika z odpowiednie role.

> [!NOTE]
> Za pomocą listy rozwijanej listy do kont użytkowników nie jest idealnym wyborem w przypadku witryn sieci Web w przypadku, gdy mogą istnieć setki kont użytkowników. Listy rozwijanej umożliwia użytkownikowi wybierz jeden element z listy stosunkowo krótkich opcje. Szybko staje się niewygodna wraz z rozwojem liczbę elementów listy. Jeśli tworzysz witryny sieci Web, który będzie miał potencjalnie dużą liczbę kont użytkowników, warto wziąć pod uwagę przy użyciu interfejsu użytkownika alternatywnych, takich jak stronicowalnej widoku GridView lub filtrowanie interfejs, który zawiera listę poprosi o wybranie literę, a następnie tylko Pokazuje tych użytkowników, których nazw rozpoczyna się od wybranej litery.


## <a name="step-1-building-the-by-user-user-interface"></a>Krok 1: Tworzenie interfejsu użytkownika "Przez użytkownika"

Otwórz `UsersAndRoles.aspx` strony. W górnej części strony, Dodaj formant etykiety sieci Web o nazwie `ActionStatus` i wyczyszczenie jego `Text` właściwości. Użyjemy tej etykiety do przekazywania opinii na akcje wykonywane, wyświetlanie wiadomości, takie jak, "Tito użytkownik został dodany do roli Administratorzy" lub "Jisun użytkownik został usunięty z roli nadzorców." Aby można było wprowadzać wyróżnienia wiadomości, ustaw wartość etykiety `CssClass` dla właściwości "Ważne".

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample1.aspx)]

Następnie dodaj następującą definicję klasy CSS do `Styles.css` arkusza stylów:

[!code-css[Main](assigning-roles-to-users-cs/samples/sample2.css)]

Ta definicja CSS nakazuje przeglądarki do wyświetlania etykiet przy użyciu czcionki dużych, red. Rysunek 1 pokazuje w tym celu za pomocą projektanta programu Visual Studio.


[![Właściwość CssClass etykiety powoduje czcionki dużych, czerwony](assigning-roles-to-users-cs/_static/image2.png)](assigning-roles-to-users-cs/_static/image1.png)

**Rysunek 1**: Etykieta `CssClass` właściwości powoduje duży, czcionka czerwony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image3.png))


Następnie dodaj DropDownList do strony, ustaw jej `ID` właściwości `UserList`i ustaw jej `AutoPostBack` właściwości na wartość True. Aby wyświetlić listę wszystkich użytkowników w systemie użyjemy tego DropDownList. Kolekcja obiektów MembershipUser zostanie powiązany to lista DropDownList. Ponieważ chcemy DropDownList, aby wyświetlić właściwości nazwy użytkownika obiektu MembershipUser (i używać go jako wartość elementów listy) Ustaw DropDownList `DataTextField` i `DataValueField` właściwości "Nazwa_użytkownika".

Poniżej DropDownList, Dodaj elementu powtarzanego o nazwie `UsersRoleList`. Tego elementu powtarzanego spowoduje wyświetlenie listy wszystkich ról w systemie jako serię pola wyboru. Zdefiniuj elemencie powtarzanym `ItemTemplate` przy użyciu następujących deklaratywne kod znaczników:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample3.aspx)]

`ItemTemplate` Znaczników zawiera jeden formant wyboru sieci Web o nazwie `RoleCheckBox`. Pole wyboru `AutoPostBack` właściwość jest ustawiona na wartość True i `Text` właściwość jest powiązana z `Container.DataItem`. Powód składnia wiązania z danymi jest po prostu `Container.DataItem` jest ponieważ framework role zwraca listę nazw ról w postaci tablicy ciągów, a jest to tablicy ciągów, które firma Microsoft będzie wiążące powtarzanego. Dokładny opis dlaczego ta składnia jest używana do wyświetlania zawartości tablicy związany z formantem Web danych wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji w tej sprawie, zapoznaj się [powiązanie tablicy skalarnych do formantu sieci Web danych](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

W tym momencie znaczników deklaratywne Twojej "przez użytkownika" interfejsu powinien wyglądać podobnie do następującego:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample4.aspx)]

Teraz możemy przystąpić do pisania kodu, które można powiązać zestaw kont użytkowników do DropDownList i zestawu ról do powtarzanego. W klasie związanej z kodem strony, Dodaj metodę o nazwie `BindUsersToUserList` i drugiego o nazwie `BindRolesList`, używając następującego kodu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample5.cs)]

`BindUsersToUserList` Metoda pobiera wszystkie konta użytkowników w systemie za pomocą [ `Membership.GetAllUsers` metody](https://msdn.microsoft.com/library/dy8swhya.aspx). To polecenie zwróci [ `MembershipUserCollection` obiektu](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), która jest kolekcją [ `MembershipUser` wystąpień](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Ta kolekcja jest następnie związana z `UserList` DropDownList. `MembershipUser` Wystąpień w skład tej kolekcji zawiera wiele właściwości, takie jak `UserName`, `Email`, `CreationDate`, i `IsOnline`. Aby nakazać DropDownList, aby wyświetlić wartość `UserName` właściwości, upewnij się, że `UserList` lista DropDownList na `DataTextField` i `DataValueField` właściwości zostały ustawione na "Nazwa_użytkownika".

> [!NOTE]
> `Membership.GetAllUsers` Metoda ma dwa przeciążenia:, który akceptuje Brak parametrów wejściowych i zwraca wszystkich użytkowników, a taki, który przyjmuje wartości całkowite indeksu strony i rozmiaru strony i zwraca tylko określony podzbiór użytkowników. W przypadku dużych ilości konta użytkowników są wyświetlane w przypadku elementu interfejsu użytkownika stronicowalnej drugi przeciążenia może służyć do wydajniej strony przez użytkowników ponieważ zwraca dokładne podzbiór kont użytkowników, a nie dla wszystkich z nich.


`BindRolesToList` Metody rozpoczyna się od wywołania `Roles` klasy [ `GetAllRoles` metody](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx), która zwraca tablica ciągów zawierająca role w systemie. Ta tablica ciągów następnie jest powiązany z powtarzanego.

Na koniec należy wywołać te dwie metody, gdy strona jest ładowana jako pierwsza. Dodaj następujący kod do `Page_Load` obsługi zdarzeń:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample6.cs)]

Ten kod w miejscu Poświęć chwilę, aby odwiedzić stronę za pośrednictwem przeglądarki; na ekranie powinna wyglądać podobnie do na rysunku 2. Wszystkie konta użytkowników zostaną wypełnione na liście rozwijanej i, pod który, każda rola ma postać pola wyboru. Ponieważ firma Microsoft `AutoPostBack` właściwości DropDownList i element CheckBoxes na wartość True, zmiana wybranego użytkownika lub zaznaczenie lub usunięcie zaznaczenia roli powoduje odświeżenie strony. Żadna akcja jest wykonywana, jednak ponieważ firma Microsoft nie zostały jeszcze napisać kod, aby obsługiwać te akcje. Firma Microsoft będzie spełnienia tych zadań w dwóch następnych sekcjach.


[![Na stronie są wyświetlani użytkownicy i role](assigning-roles-to-users-cs/_static/image5.png)](assigning-roles-to-users-cs/_static/image4.png)

**Rysunek 2**: Wyświetla stronę użytkownicy i role ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Sprawdzanie role wybranego użytkownika należy do

Po pierwszym załadowaniu strony lub zawsze, gdy użytkownik wybiera nowego użytkownika z listy rozwijanej, należy zaktualizować `UsersRoleList`tego pola wyboru tak, że zaznaczono pole wyboru danej roli tylko wtedy, gdy wybrany użytkownik należy do tej roli. W tym celu Utwórz metodę o nazwie `CheckRolesForSelectedUser` z następującym kodem:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample7.cs)]

Powyższy kod rozpoczyna się przez określenie, który jest wybranego użytkownika. Następnie używa klasy ról [ `GetRolesForUser(userName)` metody](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) do zwrócenia określonego użytkownika zestawu ról w postaci tablicy ciągów. Następnie elementów elementu powtarzanego są wyliczone i każdego elementu `RoleCheckBox` programowo odwołuje się do pola wyboru. Pole wyboru jest zaznaczone, tylko wtedy, gdy rola odpowiadający mu znajduje się w `selectedUsersRoles` tablicy ciągów.

> [!NOTE]
> `selectedUserRoles.Contains<string>(...)` Składni nie zostanie skompilowany, jeśli używasz programu ASP.NET w wersji 2.0. `Contains<string>` Metody jest częścią [biblioteki LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), który jest nowym składnikiem programu ASP.NET 3.5. Jeśli nadal używasz platformę ASP.NET w wersji 2.0, użyj [ `Array.IndexOf<string>` metody](https://msdn.microsoft.com/library/eha9t187.aspx) zamiast tego.


`CheckRolesForSelectedUser` Metoda musi być wywoływany w przypadku dwóch: po pierwszym załadowaniu strony i zawsze, gdy `UserList` lista DropDownList na zaznaczony indeks zostanie zmieniona. W związku z tym wywołanie tej metody z `Page_Load` obsługi zdarzeń (po wywołań `BindUsersToUserList` i `BindRolesToList`). Ponadto tworzenie obsługi zdarzeń dla DropDownList `SelectedIndexChanged` zdarzeń i wywołać tę metodę z tego miejsca.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample8.cs)]

Z tego kodu w miejscu należy przetestować strony za pomocą przeglądarki. Ponieważ jednak `UsersAndRoles.aspx` strony obecnie nie ma zdolność do przypisywania użytkowników do ról, żaden użytkownik nie ma ról. Utworzymy interfejs do przypisywania użytkowników do ról za chwilę, więc możesz pobrać program word który kodu źródłowego i sprawdź, czy robi to później, lub można ręcznie dodać użytkowników do ról, wstawiając rekordów do `aspnet_UsersInRoles` tabeli w celu przetestowania tej functi onality teraz.

### <a name="assigning-and-removing-users-from-roles"></a>Przypisywanie i usuwanie użytkowników z ról

Gdy obiekt odwiedzający sprawdza lub usuwa zaznaczenie pola wyboru w `UsersRoleList` potrzebujemy, aby dodać lub usunąć wybranego użytkownika z roli odpowiedniego elementu powtarzanego. Pole wyboru `AutoPostBack` właściwości ma obecnie ustawioną wartość True, która powoduje odświeżenie strony w dowolnym momencie wyboru w elemencie powtarzanym jest zaznaczenie. Krótko mówiąc, należy utworzyć program obsługi zdarzeń dla pola wyboru `CheckChanged` zdarzeń. Ponieważ jest to pole wyboru w kontrolce elementu powtarzanego, należy ręcznie dodać instalację programu obsługi zdarzeń. Rozpocznij od dodania obsługi zdarzeń do klasy związane z kodem jako `protected` metody, w następujący sposób:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample9.cs)]

Wrócimy do pisanie kodu dla tego programu obsługi zdarzeń za chwilę. Ale pierwszym umożliwia zakończenie obsługi żmudne procesy zdarzeń. Od wyboru w elemencie powtarzanym `ItemTemplate`, Dodaj `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Ta składnia wiążący `RoleCheckBox_CheckChanged` program obsługi zdarzeń do `RoleCheckBox`w `CheckedChanged` zdarzeń.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample10.aspx)]

Nasze ostatnim zadaniem jest przeprowadzenie `RoleCheckBox_CheckChanged` obsługi zdarzeń. Należy uruchomić za pomocą odwołań do formantu wyboru, który wywołał zdarzenie, ponieważ to wystąpienie wyboru informuje, nam jakie rola została zaznaczenie za pośrednictwem jego `Text` i `Checked` właściwości. Korzystając z tych informacji oraz nazwy użytkownika wybranego użytkownika, możemy dodać, jak lub Usuń użytkownika z roli za pomocą `Roles` klasy [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) lub [ `RemoveUserFromRole` metody](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample11.cs)]

Powyższy kod rozpoczyna się od programowo odwołujące się do pola wyboru, który wywołał zdarzenie, który jest dostępny za pośrednictwem `sender` parametru wejściowego. Jeśli pole wyboru jest zaznaczone, wybrany użytkownik jest dodany do określonej roli, w przeciwnym razie są usuwane z roli. W obu przypadkach `ActionStatus` etykieta zostanie wyświetlony komunikat podsumowania wystarczy wykonać akcję.

Poświęć chwilę, aby przetestować tę stronę za pośrednictwem przeglądarki. Wybierz użytkownika Tito, a następnie dodaj Tito do ról administratorów i kontrolerów.


[![Dodano Tito dla administratorów i nadzorców ról](assigning-roles-to-users-cs/_static/image8.png)](assigning-roles-to-users-cs/_static/image7.png)

**Rysunek 3**: Tito został dodany do administratorów, jak i role nadzorców ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image9.png))


Następnie wybierz pozycję Bruce użytkownika z listy rozwijanej. Brak odświeżania strony i pól wyboru w elemencie powtarzanym są aktualizowane za pośrednictwem `CheckRolesForSelectedUser`. Ponieważ Bruce nie należy jeszcze do żadnej roli, dwa pola wyboru jest zaznaczony. Następnie dodaj Bruce do roli kontrolerów.


[![Bruce został dodany do roli kontrolerów](assigning-roles-to-users-cs/_static/image11.png)](assigning-roles-to-users-cs/_static/image10.png)

**Rysunek 4**: Bruce został dodany do roli kierownicy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image12.png))


Dodatkową weryfikację funkcjonalność `CheckRolesForSelectedUser` metody, wybierz użytkownika niż Tito lub Bruce. Należy zwrócić uwagę, jak automatycznie nie są zaznaczone pola wyboru, oznaczające, że należą one do żadnej roli. Wróć do Tito. Pola wyboru zarówno Administratorzy, jak i kontrolerów powinny być sprawdzane.

## <a name="step-2-building-the-by-roles-user-interface"></a>Krok 2: Tworzenie interfejsu użytkownika "Przez role"

Na tym etapie firma Microsoft wykonali interfejsu "przez użytkowników" i gotowe do rozpoczęcia działania interfejsu "przez role". Interfejs "przez role" monituje użytkownika o wybierz rolę z listy rozwijanej, a następnie wyświetla zbiór użytkowników, którzy należą do tej roli w widoku GridView.

Dodaj inny formant DropDownList `UsersAndRoles.aspx` strony. Umieść ten jednej kontrolce elementu powtarzanego, nadaj jej nazwę `RoleList`i ustaw jej `AutoPostBack` właściwości na wartość True. Poniżej, Dodaj element GridView i nadaj mu nazwę `RolesUserList`. Tego widoku GridView będzie zawierała listę użytkowników, którzy należą do wybranej roli. Ustaw w widoku GridView `AutoGenerateColumns` siatki właściwości na wartość False, dodane TemplateField `Columns` kolekcji i ustaw jej `HeaderText` dla właściwości "Użytkownicy". Zdefiniuj TemplateField `ItemTemplate` tak, aby wyświetlana jest wartość wyrażenia wiązania danych `Container.DataItem` w `Text` właściwość etykiety o nazwie `UserNameLabel`.

Po dodaniu i konfigurowanie widoku GridView, deklaratywne znaczników sieci "w roli" interfejsu powinien wyglądać podobny do następującego:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample12.aspx)]

Należy wypełnić `RoleList` DropDownList przy użyciu zestawu ról w systemie. Aby to zrobić, należy zaktualizować `BindRolesToList` metody w taki sposób, aby powiązania przy użyciu tablicy ciągów zwrócony przez `Roles.GetAllRoles` metodę `RolesList` DropDownList (, jak również `UsersRoleList` elementu powtarzanego).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample13.cs)]

Ostatnie dwa wiersze w `BindRolesToList` metody zostały dodane do zestawu ról, aby powiązać `RoleList` DropDownList formantu. Rysunek 5. pokazuje wynik końcowy podczas wyświetlania za pośrednictwem przeglądarki — listy rozwijanej wypełniane przy użyciu ról systemu.


[![Role są wyświetlane w RoleList DropDownList](assigning-roles-to-users-cs/_static/image14.png)](assigning-roles-to-users-cs/_static/image13.png)

**Rysunek 5**: role są wyświetlane w `RoleList` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Wyświetlanie użytkowników, którzy należą do wybranej roli

Po raz pierwszy załadowano strony lub jeśli zaznaczono nową rolę z `RoleList` DropDownList, należy wyświetlić listę użytkowników, którzy należą do tej roli w widoku GridView. Utwórz metodę o nazwie `DisplayUsersBelongingToRole` przy użyciu następującego kodu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample14.cs)]

Ta metoda uruchamia pobierając wybraną rolę z `RoleList` DropDownList. Następnie używa [ `Roles.GetUsersInRole(roleName)` metody](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) można pobrać ciągu tablicę nazw użytkowników, użytkowników, którzy należą do tej roli. Ta tablica jest następnie związana z `RolesUserList` widoku GridView.

Ta metoda musi być wywoływany w dwóch sytuacjach: podczas ładowania strony do i wybraną rolę `RoleList` DropDownList zmiany. W związku z tym zaktualizować `Page_Load` obsługi zdarzeń, aby ta metoda jest wywoływana po wywołaniu `CheckRolesForSelectedUser`. Następnie należy utworzyć programu obsługi zdarzeń dla `RoleList`w `SelectedIndexChanged` zdarzenia i wywołać tę metodę z tego miejsca, za.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample15.cs)]

Kod w miejscu `RolesUserList` tych użytkowników, którzy należą do wybranej roli powinien być wyświetlany w widoku GridView. Jak pokazano na rysunku 6, rola nadzorców zawiera dwa elementy członkowskie: Bruce i Tito.


[![Widoku GridView zawiera listę tych użytkowników, którzy należą do wybranej roli](assigning-roles-to-users-cs/_static/image17.png)](assigning-roles-to-users-cs/_static/image16.png)

**Rysunek 6**: widoku GridView Wyświetla listę użytkowników czy należą do roli wybrane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>Usuwanie użytkowników z wybranej roli

Ta funkcja pozwala rozszerzyć `RolesUserList` przyciski widoku GridView co zawierają one kolumny "Usuń". Klikając przycisk "Usuń" dla danego użytkownika spowoduje usunięcie ich z tej roli.

Rozpocznij od dodania pola przycisk usuwania do widoku GridView. To pole są wyświetlane od lewej najbardziej zachowanej i zmieniać jego `DeleteText` właściwości "Usuń" z "Delete" (wartość domyślna).


[![Dodaj](assigning-roles-to-users-cs/_static/image20.png)](assigning-roles-to-users-cs/_static/image19.png)

**Rysunek 7**: Dodawanie przycisk "Usuń" do widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image21.png))


Po kliknięciu przycisku "Usuń" ensues odświeżania strony i w widoku GridView `RowDeleting` zdarzenia. Należy utworzyć program obsługi zdarzeń dla tego zdarzenia i pisania kodu, który usuwa użytkownika z wybranej roli. Tworzenie obsługi zdarzeń, a następnie dodaj następujący kod:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample16.cs)]

Kod rozpoczyna się przez określenie nazwy wybranej roli. Następnie programowo odwołania `UserNameLabel` sterowania z wiersza, którego przycisk Usuń, przycisk został kliknięty w celu ustalenia, nazwa użytkownika do usunięcia. Ten użytkownik następnie zostaje usunięty z roli za pomocą wywołania `Roles.RemoveUserFromRole` metody. `RolesUserList` Widoku GridView zostanie następnie odświeżona i jest wyświetlany komunikat za pośrednictwem `ActionStatus` etykiety formantu.

> [!NOTE]
> Przycisk "Usuń" nie wymaga żadnych sortowania potwierdzenia przez użytkownika przed usunięciem użytkownika z roli. Zaprosić można dodać pewnego poziomu potwierdzenie przez użytkownika. Jednym z najprostszych sposobów potwierdzenie akcji jest za pomocą okna dialogowego Potwierdź po stronie klienta. Aby uzyskać więcej informacji na temat tej techniki, zobacz [Dodawanie potwierdzenie po stronie klienta podczas usuwania](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Rysunek nr 8 przedstawia strony po Tito użytkownik został usunięty z grupy kierownicy.


[![Niestety Tito nie jest już opiekuna](assigning-roles-to-users-cs/_static/image23.png)](assigning-roles-to-users-cs/_static/image22.png)

**Rysunek 8**: Niestety Tito nie jest już kierownik ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>Dodawanie nowych użytkowników do wybranej roli

Wraz z usuwanie użytkowników z wybranej roli, obiekt odwiedzający do tej strony należy mógł dodać użytkownika do wybranej roli. Najlepsze interfejsu związanych z dodawaniem użytkowników do wybranej roli zależy od liczby kont użytkowników, które chcesz mieć. Jeśli witryny sieci Web będzie znajdować się tylko kilka kont użytkowników dozen lub mniej, można tutaj użyć DropDownList. Jeśli może być tysiące kont użytkowników, czy chcesz dołączyć interfejsu użytkownika, który pozwala na obiekt odwiedzający do strony przy użyciu konta, wyszukaj określonego konta, lub filtrowanie kont użytkowników w dowolny sposób innych.

Na tej stronie można użyć bardzo prosty interfejs, który działa niezależnie od liczby kont użytkowników w systemie. To znaczy użyjemy pole tekstowe, monitowania użytkownika wpisz nazwę użytkownika użytkownika, którego chce dodać do wybranej roli. Jeśli nie istnieje żaden użytkownik o tej nazwie lub jeśli użytkownik jest już członkiem roli, firma Microsoft będzie wyświetlony komunikat w `ActionStatus` etykiety. Jednak jeśli użytkownik istnieje i nie jest członkiem roli, firma Microsoft będzie je dodać do roli i Odśwież siatki.

Dodaj pole tekstowe i poniżej widoku GridView. Ustaw pole tekstowe `ID` do `UserNameToAddToRole` i ustaw przycisku `ID` i `Text` właściwości `AddUserToRoleButton` i "Dodaj do roli użytkownika", odpowiednio.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample17.aspx)]

Następnie należy utworzyć `Click` programu obsługi zdarzeń dla `AddUserToRoleButton` i Dodaj następujący kod:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample18.cs)]

Większość kodu w `Click` obsługi zdarzeń wykonuje różne testy sprawdzania poprawności. Gwarantuje, że obiekt odwiedzający podać nazwę użytkownika w `UserNameToAddToRole` pole tekstowe, czy użytkownik istnieje w systemie i już nie należą do wybranej roli. Jeśli którakolwiek z tych sprawdza kończy się niepowodzeniem, zostanie wyświetlony komunikat odpowiednie w `ActionStatus` i program obsługi zdarzeń jest zakończony. Jeśli wszystkie kontroli pomyślnie, jest dodawany do roli za pomocą `Roles.AddUserToRole` metody. Po tym, że pole tekstowe w `Text` właściwości jest brany pod uwagę, widoku GridView zostanie odświeżona, a `ActionStatus` etykieta zostanie wyświetlony komunikat wskazujący, czy określony użytkownik został pomyślnie dodany do wybranej roli.

> [!NOTE]
> Aby upewnić się, że podany użytkownik nie należy już do wybranej roli, używamy [ `Roles.IsUserInRole(userName, roleName)` — metoda](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), która zwraca wartość Boolean wskazującą czy *userName* jest elementem członkowskim *roleName*. Używamy metody ponownie w <a id="_msoanchor_2"> </a> [następny samouczek](role-based-authorization-cs.md) Jeśli przyjrzymy się autoryzacji opartej na rolach.


Odwiedź stronę za pośrednictwem przeglądarki, a następnie wybierz rolę kontrolerów z `RoleList` DropDownList. Spróbuj wprowadzić nieprawidłowej nazwy użytkownika — powinien zostać wyświetlony komunikat z informacjami o tym, że użytkownik nie istnieje w systemie.


[![Nie można dodać użytkownika nieistniejącą do roli](assigning-roles-to-users-cs/_static/image26.png)](assigning-roles-to-users-cs/_static/image25.png)

**Rysunek 9**: nie można dodać użytkownika nieistniejąca do roli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image27.png))


Teraz Dodaj prawidłowego użytkownika. Przejdź dalej i ponownie dodać do roli nadzorców Tito.


[![Tito jest ponownie Kierownik!](assigning-roles-to-users-cs/_static/image29.png)](assigning-roles-to-users-cs/_static/image28.png)

**Na rysunku nr 10**: Tito jest ponownie Kierownik!  ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Krok 3: Aktualizowanie między "Przez użytkownika" i "Według roli" interfejsów

`UsersAndRoles.aspx` Udostępnia dwa interfejsy distinct do zarządzania użytkownikami i rolami. Obecnie następujące dwa interfejsy działa niezależnie od siebie, istnieje możliwość, że zmiany wprowadzone w jednym interfejsie nie zostaną odzwierciedlone natychmiast w innym. Na przykład, załóżmy, że obiekt odwiedzający do strony wybiera rolę kontrolerów z `RoleList` DropDownList, w której znajduje się lista Bruce i Tito jako elementy członkowskie. Następnie użytkownik wybiera Tito z `UserList` DropDownList, która sprawdza pola wyboru Administratorzy i kontrolerów w `UsersRoleList` elementu powtarzanego. Jeśli obiekt odwiedzający następnie usuwa zaznaczenie roli przełożonego z powtarzanego, Tito zostanie usunięty z roli kontrolerów, ale ta modyfikacja nie jest widoczna w interfejsie "według roli". Widoku GridView będzie widoczna Tito jako członek roli kontrolerów.

Aby naprawić to musimy odświeżyć widoku GridView zawsze, gdy rola to zaznaczenie z `UsersRoleList` elementu powtarzanego. Podobnie musimy odświeżyć powtarzanego zawsze, gdy użytkownik jest usunięte lub dodane do roli w interfejsie "według roli".

Powtarzanego w interfejsie "przez użytkownika" jest odświeżany przez wywołanie metody `CheckRolesForSelectedUser` metody. Interfejs "według roli" może być modyfikowany w `RolesUserList` w widoku GridView `RowDeleting` obsługi zdarzeń i `AddUserToRoleButton` przycisku `Click` obsługi zdarzeń. W związku z tym należy wywołać `CheckRolesForSelectedUser` metody z każdej z tych metod.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample19.cs)]

Podobnie, przez wywołanie metody odświeżeniu widoku GridView w interfejsie "według roli" `DisplayUsersBelongingToRole` metody i interfejsie "przez użytkownika" jest zmodyfikowane za pośrednictwem `RoleCheckBox_CheckChanged` programu obsługi zdarzeń. W związku z tym należy wywołać `DisplayUsersBelongingToRole` metody z tej obsługi zdarzeń.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample20.cs)]

Wprowadzone zmiany kodu pomocnicza "przez użytkownika" i "według roli" interfejsy teraz prawidłowo między aktualizacji. Aby to sprawdzić, odwiedź stronę za pośrednictwem przeglądarki, a następnie wybierz Tito i kontrolerów z `UserList` i `RoleList` DropDownLists, odpowiednio. Należy pamiętać, że jako wyczyścisz rolę nadzorców dla Tito z elementu powtarzanego w interfejsie "przez użytkownika" Tito zostanie automatycznie usunięta z widoku GridView w interfejsie "według roli". Ponowne dodanie Tito do roli kontrolerów z interfejsu "według roli" automatycznie sprawdza wyboru kontrolerów w interfejsie "przez użytkownika".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Krok 4: Dostosowywanie CreateUserWizard uwzględnienie kroku "Określ role"

W <a id="_msoanchor_3"> </a> [ *tworzenia kont użytkowników* ](../membership/creating-user-accounts-cs.md) samouczek widzieliśmy sposób użycia formancie CreateUserWizard sieci Web udostępnia interfejs służący do tworzenia nowego konta użytkownika. W formancie CreateUserWizard mogą być używane w jeden z dwóch sposobów:

- Sposób, aby użytkownicy mogli tworzyć własne konto użytkownika w witrynie i
- Jako środek dla administratorów utworzyć nowe konta

W pierwszym przypadku użycia odwiedzającego pochodzi z lokacją i wypełnia CreateUserWizard, wprowadzić swoje informacje do zarejestrowania w witrynie. W drugim przypadku administrator tworzy nowe konto dla innej osoby.

Gdy konto jest tworzone przez administratora dla niektórych innej osoby, może być przydatne umożliwić administratorowi Określ role, jakie nowe konto użytkownika należy do. W <a id="_msoanchor_4"> </a> [ *przechowywanie* *dodatkowe informacje o użytkowniku* ](../membership/storing-additional-user-information-cs.md) samouczek widzieliśmy sposobu dostosowywania CreateUserWizard przez dodanie dodatkowych `WizardSteps`. Oto jak dodać dodatkowy krok do CreateUserWizard celu określanie ról nowego użytkownika.

Otwórz `CreateUserWizardWithRoles.aspx` i dodać formancie CreateUserWizard o nazwie `RegisterUserWithRoles`. Ustawianie formantu `ContinueDestinationPageUrl` dla właściwości "~ / Default.aspx". Ponieważ pomysł, w tym miejscu jest, że administrator będzie używana ta formancie CreateUserWizard do tworzenia nowych kont użytkowników, ustaw dla formantu `LoginCreatedUser` wartość False dla właściwości. To `LoginCreatedUser` właściwość określa, czy użytkownik loguje się automatycznie jako użytkownik po prostu utworzyć i jego wartość domyślna to True. Firma Microsoft ustawić go na wartość False, ponieważ gdy administrator tworzy nowe konto chcemy zapewnić mu zalogowany jako siebie.

Następnie wybierz pozycję "Dodaj lub usuń `WizardSteps`..." z tagów inteligentnych CreateUserWizard, aby dodać nowy `WizardStep`, ustawienie jej `ID` do `SpecifyRolesStep`. Przenieś `SpecifyRolesStep WizardStep` tak, aby nastąpi po wykonaniu kroku "Logowania w tym nowe konto", ale przed wykonaniem kroku "Ukończone". Ustaw `WizardStep`w `Title` dla właściwości "Określ role", jego `StepType` właściwości `Step`i jego `AllowReturn` wartość False dla właściwości.


[![Dodaj](assigning-roles-to-users-cs/_static/image32.png)](assigning-roles-to-users-cs/_static/image31.png)

**Rysunek 11**: Dodaj "Określ role" `WizardStep` do CreateUserWizard ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image33.png))


Po tej zmianie znaczników deklaratywne Twojej CreateUserWizard powinien wyglądać podobne do poniższych:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample21.aspx)]

"Określ ról" `WizardStep`, Dodaj do elementu CheckBoxList o nazwie `RoleList`. Ta CheckBoxList spowoduje wyświetlenie listy dostępnych ról, włączanie osoby odwiedzając stronę sprawdzić, jakie role nowo utworzony użytkownik należy do.

Firma Microsoft pozostaną dwa zadania kodowania: najpierw musisz wypełnimy `RoleList` CheckBoxList z ról w systemie; drugie, należy dodać utworzonego użytkownika do wybranych ról, gdy użytkownik przesuwa z kroku "Określ role" do kroku "Zakończ". Firma Microsoft można wykonać pierwsze zadanie w `Page_Load` obsługi zdarzeń. Poniższy kod programowo odwołania `RoleList` wyboru pierwszego można znaleźć na stronie i powiązanie role w systemie go.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample22.cs)]

Powyższy kod powinien wyglądać znajomo. W <a id="_msoanchor_5"> </a> [ *przechowywanie* *dodatkowe informacje o użytkowniku* ](../membership/storing-additional-user-information-cs.md) samouczek użyliśmy dwa `FindControl` instrukcje, aby odwołać formant sieci Web z wewnątrz niestandardowego `WizardStep`. I kod, który wiąże role CheckBoxList została pobrana z wcześniej w tym samouczku.

Drugi zadania programowania musimy wiedzieć, po wykonaniu kroku "Określ role". Odwołania, który ma CreateUserWizard `ActiveStepChanged` zdarzenie, które są generowane, zawsze poruszania się od jednego kroku do innego. W tym miejscu możemy określić, jeśli użytkownik został osiągnięty kroku "Zakończ"; Jeśli tak, należy dodać użytkownika do wybranych ról.

Tworzenie procedury obsługi zdarzeń dla `ActiveStepChanged` zdarzeń i Dodaj następujący kod:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample23.cs)]

Jeśli użytkownik po prostu osiągnął kroku "Completed", program obsługi zdarzeń wylicza elementy `RoleList` CheckBoxList i użytkownik po prostu utworzyć jest przypisane do wybranych ról.

Odwiedź stronę tej strony za pośrednictwem przeglądarki. Pierwszym etapem CreateUserWizard jest standardowych czynności "Logowania w tym nowe konto", które monituje o podanie nazwy użytkownika nowego użytkownika, hasło, poczty e-mail i innych informacji o kluczu. Wprowadź informacje do utworzenia nowego użytkownika o nazwie Wanda.


[![Utwórz nowego użytkownika o nazwie Wanda](assigning-roles-to-users-cs/_static/image35.png)](assigning-roles-to-users-cs/_static/image34.png)

**Rysunek 12**: Tworzenie nowego Wanda o nazwie użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image36.png))


Kliknij przycisk "Utwórz użytkownika". Wewnętrznie wywołuje CreateUserWizard `Membership.CreateUser` metody tworzenia nowego konta użytkownika, a następnie następuje przejście do następnego kroku "Określ role." W tym polu są wyświetlane role systemu. Zaznacz pole wyboru kontrolerów i kliknij przycisk Dalej.


[![Wprowadzić Wanda jako członka roli kontrolerów](assigning-roles-to-users-cs/_static/image38.png)](assigning-roles-to-users-cs/_static/image37.png)

**Rysunek 13**: wprowadzić Wanda jako członka roli kierownicy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image39.png))


Kliknięcie przycisku Dalej powoduje odświeżenie strony i aktualizacje `ActiveStep` do kroku "Zakończ". W `ActiveStepChanged` program obsługi zdarzeń, konto użytkownika niedawno utworzona jest przypisany do roli kontrolerów. Aby to sprawdzić, wróć do `UsersAndRoles.aspx` i wybrać opcję kontrolerów z `RoleList` DropDownList. Jak pokazano na rysunku 14, kontrolerów teraz składają się z trzech użytkowników: Bruce, Tito i Wanda.


[![Bruce, Tito i Wanda są wszystkich kontrolerów](assigning-roles-to-users-cs/_static/image41.png)](assigning-roles-to-users-cs/_static/image40.png)

**Rysunek 14**: Bruce, Tito i Wanda są wszystkie kierownicy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image42.png))


## <a name="summary"></a>Podsumowanie

W ramach ról zapewnia metody do pobierania informacji o rolach i metody określania, jakie użytkownicy należą do określonej roli określonego użytkownika. Ponadto istnieje wiele metod do dodawania i usuwania co najmniej jednego użytkownika do co najmniej jedną rolę. W tym samouczku firma Microsoft skupia się na tylko dwa z tych metod: `AddUserToRole` i `RemoveUserFromRole`. Istnieją dodatkowe wariantów dodawania wielu użytkowników do jednej roli i przypisywania ról wiele do jednego użytkownika.

Ten samouczek obejmuje również wyglądu w formancie CreateUserWizard, aby uwzględnić rozszerzenie `WizardStep` umożliwia określanie ról nowo utworzonego użytkownika. Taki krok może pomóc administrator usprawnić proces tworzenia kont użytkowników dla nowych użytkowników.

W tym momencie zaobserwowano, jak tworzyć i usuwać role i jak dodawać i usuwać użytkowników z ról. Jednak firma Microsoft nie zostały jeszcze przyjrzeć się stosowania autoryzacji opartej na rolach. W <a id="_msoanchor_6"> </a> [następujących samouczek](role-based-authorization-cs.md) zajmiemy Definiowanie reguł autoryzacji adresów URL na podstawie roli ról oraz jak celu ograniczenia funkcji na poziomie strony na podstawie ról aktualnie zalogowanego użytkownika.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Omówienie narzędzia administracyjnej witryny sieci Web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Badanie ASP. Członkostwo w sieci, ról i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Wycofanie własne narzędzia administracyjnego witryny sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](creating-and-managing-roles-cs.md)
[dalej](role-based-authorization-cs.md)
