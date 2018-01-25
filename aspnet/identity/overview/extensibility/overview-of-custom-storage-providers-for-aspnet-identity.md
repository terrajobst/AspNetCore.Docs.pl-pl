---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: "Przegląd dostawców magazynu niestandardowego dla tożsamości ASP.NET | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "ASP.NET Identity jest rozszerzalny system, co pozwala na tworzenie własnego dostawcę magazynu i podłącz go do aplikacji bez ponownego pracy nazwę..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: f43f0a2dd80e26ecff15e5742e18264ddb5b26aa
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Przegląd dostawców magazynu niestandardowego dla tożsamości ASP.NET
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> ASP.NET Identity jest rozszerzalny system, co pozwala na tworzenie własnego dostawcę magazynu i podłącz go do aplikacji bez ponownego pracy aplikacji. W tym temacie opisano sposób tworzenia dostawcy magazynu dostosowanych dla tożsamości ASP.NET. Obejmuje on z ważnymi pojęciami dotyczącymi tworzenia własnego dostawcę magazynu, ale nie jest to przewodnik krok po kroku wdrażania dostawcy magazynu niestandardowego.
> 
> Przykład implementowanie dostawcy magazynu niestandardowych, zobacz [Implementowanie niestandardowego dostawcy magazynu tożsamości ASP.NET MySQL](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Ten temat został zaktualizowany na potrzeby programu ASP.NET 2.0 tożsamości.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Visual Studio 2013 z aktualizacją 2
> - ASP.NET Identity 2


## <a name="introduction"></a>Wprowadzenie

Domyślnie system ASP.NET Identity informacje użytkownika są przechowywane w bazie danych programu SQL Server i korzysta z programu Entity Framework Code First utworzyć bazę danych. Dla wielu aplikacji to rozwiązanie działa dobrze. Jednak może chcieć użyć innego typu trwałości mechanizmu, takiego jak magazyn tabel Azure, lub tabel bazy danych ze strukturą bardzo inną niż domyślna implementacja może już istnieć. W obu przypadkach można zapisać dostosowane dostawcy dla Twojego mechanizmu magazynowania i podłącz tego dostawcy do aplikacji.

ASP.NET Identity jest domyślnie włączone w wielu szablonów programu Visual Studio 2013. Możesz pobrać aktualizacje do tożsamości ASP.NET za pośrednictwem [pakietu Microsoft AspNet Identity EntityFramework NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Ten temat zawiera następujące sekcje:

- [Zrozumienie architektury](#architecture)
- [Zrozumienie przechowywanych danych](#data)
- [Utwórz Warstwa dostępu do danych](#dal)
- [Dostosowywanie klasa użytkownika](#user)
- [Dostosowywanie magazynu użytkowników](#userstore)
- [Dostosowywanie klasy roli](#role)
- [Dostosowywanie magazynu ról](#rolestore)
- [Skonfiguruj ponownie aplikacji do korzystania z nowego dostawcy magazynu](#reconfigure)
- [Implementacje dostawców niestandardowych magazynów](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Zrozumienie architektury

ASP.NET Identity składa się z klasy o nazwie menedżerów i magazynów. Menedżerowie to klasy wysokiego poziomu, które Deweloper aplikacji używane do wykonywania operacji, takich jak tworzenie użytkownika, w systemie tożsamości ASP.NET. Magazyny są klasy niższego poziomu, które określają sposób jednostek, takich jak użytkownicy i role, są zachowywane. Magazyny są ściśle powiązane z mechanizmu stanu trwałego, ale menedżerów są całkowicie niezależna od magazynów co oznacza, że można zastąpić mechanizmu stanu trwałego bez zakłócania całej aplikacji.

Na poniższym diagramie przedstawiono sposób interakcji aplikacji sieci web z wybranych menedżerów i magazyny interakcyjnie Warstwa dostępu do danych.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Aby utworzyć dostawcę magazynu dostosowanych dla tożsamości ASP.NET, należy utworzyć źródło danych, Warstwa dostępu do danych i klasy magazynu współpracujące z tą warstwą dostępu do danych. Aby kontynuować, przy użyciu tego samego menedżera interfejsów API do operacji danych użytkownika, ale teraz, gdy dane są zapisywane do innego magazynu systemu.

Nie trzeba dostosować klasy manager, ponieważ podczas tworzenia nowego wystąpienia interfejs UserManager lub RoleManager podać typ klasy użytkownika i przekazać wystąpienia klasy magazynu jako argument. Takie podejście umożliwia Podłącz klas dostosowanych do istniejącej struktury. Zobaczysz sposobu tworzenia wystąpienia interfejs UserManager i RoleManager z klas dostosowanych magazynu w sekcji [ponownie skonfigurować aplikację do użycia nowego dostawcę magazynu](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Zrozumienie przechowywanych danych

Do implementowania dostawcy magazynu niestandardowego, należy zrozumieć typy danych używanych z ASP.NET Identity i zdecydować, które funkcje mają zastosowanie do aplikacji.

| Dane | Opis |
| --- | --- |
| Użytkownicy | Zarejestrowani użytkownicy witryny sieci web. Zawiera nazwę identyfikator i nazwę użytkownika. Może obejmować skrótem hasła, jeśli użytkownicy logować się przy użyciu poświadczeń, które są specyficzne dla lokacji (a nie przy użyciu poświadczeń z witryny zewnętrznej, takich jak Facebook) i sygnaturę bezpieczeństwa, aby wskazać, czy wszystko zostało zmienione w poświadczenia użytkownika. Może również zawierać adres e-mail, numer telefonu, czy uwierzytelnianie dwuskładnikowe jest włączone, to bieżąca liczba niepowodzeń logowania, oraz czy konto zostało zablokowane. |
| Oświadczenia użytkowników | Zestaw instrukcji (lub oświadczenia) dotyczące użytkownika, które reprezentują tożsamość użytkownika. Można włączyć większa wyrażenia tożsamość użytkownika nie można osiągnąć za pomocą ról. |
| Identyfikatory logowania użytkownika | Informacje o dostawcy uwierzytelniania zewnętrznego (takich jak Facebook) używane podczas logowania użytkownika. |
| Role | Grup autoryzacji dla witryny. Zawiera nazwę roli identyfikator i roli (na przykład "Admin" lub "Pracownika"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Utwórz Warstwa dostępu do danych

W tym temacie założono, że znasz mechanizm trwałości, który ma być oraz sposobu tworzenia jednostek dla tego mechanizmu. Ten temat nie zawiera szczegółowe informacje o sposobie tworzenia repozytoria lub klas dostępu do danych; przekazuje on sugestie dotyczące decyzji projektowych, które należy rozważyć podczas pracy z ASP.NET Identity.

Masz dużą swobodę podczas projektowania repozytoria dla dostosowany dostawcy magazynu. Należy utworzyć repozytoria dla funkcji, które mają być używane w aplikacji. Na przykład jeśli nie używasz role w aplikacji, nie należy utworzyć magazyn dla ról lub ról użytkowników. Używanych technologii i istniejącej infrastruktury może wymagać struktura, która różni się bardzo od Domyślna implementacja tożsamości ASP.NET. W Twojej Warstwa dostępu do danych można zapewnić logikę do pracy z repozytoriami struktury.

Aby implementacja MySQL repozytoriów danych dla programu ASP.NET 2.0 tożsamości, zobacz [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

W warstwie dostępu do danych można zapewnić logiki do zapisywania danych z ASP.NET Identity ze źródłem danych. Warstwa dostępu do danych dla dostawcy magazynu dostosowanych może obejmować następujące klasy do przechowywania informacji o użytkownika i roli.

| Class | Opis | Przykład |
| --- | --- | --- |
| Kontekst | Hermetyzuje informacje, aby połączyć się z mechanizmu stanu trwałego i wykonywanie zapytań. Ta klasa jest podstawą warstwą dostępu do danych. Klas danych wymaga wystąpienia tej klasy, aby wykonywać operacje. Zostanie również zainicjować klas magazynu przy użyciu wystąpienia tej klasy. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Magazyn użytkownika | Przechowuje i pobiera informacje o użytkowniku (np. hash nazwę i hasło użytkownika). | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Rola magazynów | Przechowuje i pobiera informacje o rolach (takie jak nazwa roli). | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Magazyn UserClaims | Przechowuje i pobiera informacje o użytkownika (na przykład oświadczenia typu i wartości). | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Magazyn UserLogins | Przechowuje i pobiera dane logowania użytkownika (na przykład zewnętrznego dostawcę uwierzytelniania). | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| UserRole magazynu | Przechowuje i pobiera użytkownik jest przypisany do ról. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Ponownie wystarczy do zaimplementowania klasy, które mają być używane w aplikacji.

W klasach dostępu do danych musisz podać kod, aby wykonać operacje na danych z mechanizmu określonego trwałości. Na przykład w protokole MySQL klasa UserTable zawiera metody do wstawienia rekordu do tabeli bazy danych użytkowników. Zmienna o nazwie `_database` jest wystąpieniem klasy MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Po utworzeniu klas dostępu do danych, można utworzyć klasy magazynu, które wywołanie metody określonej w warstwie dostępu do danych.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Dostosowywanie klasa użytkownika

Podczas implementowania dostawcy magazynu, należy utworzyć klasę użytkownika, który jest odpowiednikiem [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) klasy w [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) przestrzeni nazw:

Na poniższym diagramie przedstawiono klasy IdentityUser, należy utworzyć i interfejs do zaimplementowania w tej klasie.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

[IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) interfejs definiuje właściwości, które interfejs UserManager próbuje wywołać podczas wykonywania żądanej operacji. Interfejs zawiera dwie właściwości — identyfikator i nazwa użytkownika. [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) interfejs umożliwia określenie typu klucza dla użytkownika za pomocą ogólnego **TKey** parametru. Typ właściwości Id jest zgodna z wartością parametru TKey.

Udostępnia również w ramach tożsamości [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) interfejsu (bez parametru ogólnego) Jeśli chcesz użyć wartości ciągu dla klucza.

Klasa IdentityUser implementuje IUser i zawiera wszelkie dodatkowe właściwości lub konstruktorów dla użytkowników w witrynie sieci web. W poniższym przykładzie przedstawiono klasę IdentityUser, która używa całkowitą dla klucza. W polu identyfikatora ustawiono **int** do dopasowania wartości parametru ogólnego. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Do ukończenia wdrożenia, zobacz [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Dostosowywanie magazynu użytkowników

Również utworzyć klasę magazynie UserStore, która udostępnia metody dla wszystkich operacji danych użytkownika. Ta klasa jest odpowiednikiem [magazynie UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) klasy w [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) przestrzeni nazw. W klasie magazynie UserStore zaimplementowaniem [elementy IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) i wszystkie interfejsy opcjonalne. Możesz wybrać które opcjonalne interfejsy do implementacji, na podstawie funkcję, którą chcesz zapewnić w aplikacji.

Na poniższej ilustracji przedstawiono klasy magazynie UserStore, którą należy utworzyć i odpowiednich interfejsów.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Domyślny szablon projektu w programie Visual Studio zawiera kod, który przyjęto założenie, że wiele interfejsów opcjonalne zostało wdrożonych w magazynie użytkownika. Domyślny szablon korzystają z magazynu użytkownika, musi implementować interfejsów opcjonalne w magazynie użytkownika lub alter kod szablonu już wywołać metod w interfejsach, które nie zostały zaimplementowane.

 W poniższym przykładzie przedstawiono klasę magazynu proste użytkownika. **TUser** parametru ogólnego przyjmuje typ własnej klasy user, który zazwyczaj jest to klasa IdentityUser zdefiniowany. **TKey** parametru ogólnego przyjmuje typ klucza użytkownika. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 W tym przykładzie Konstruktor, który przyjmuje parametr o nazwie *bazy danych* typu ExampleDatabase jest ilustracja sposób przekazywania w klasie dostępu do danych. Na przykład w implementacji MySQL ten konstruktor ma parametr typu MySQLDatabase. 

W elemencie UserStore klasy możesz użyć klas dostępu do danych, które zostały utworzone w celu wykonania operacji. Na przykład w implementacji MySQL magazynie UserStore klasa ma metodę CreateAsync, która używa wystąpienia UserTable, aby wstawić nowy rekord. **Wstaw** metoda **userTable** obiekt jest w tej samej metody, które zostało przedstawione w poprzedniej sekcji. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfejsy do implementacji w przypadku dostosowywania magazynu użytkowników

Następny obraz zawiera więcej informacji na temat funkcji zdefiniowanej w każdego interfejsu. Dziedziczy wszystkie interfejsy opcjonalne elementy IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **Elementy IUserStore**  
 [Elementy IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) interfejs jest tylko interfejsem musi implementować w magazynie użytkownika. Definiuje metody tworzenia, aktualizowania, usuwania i pobierania użytkowników.
- **IUserClaimStore**  
 [IUserClaimStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) interfejs definiuje metody musi implementować w magazynie użytkownika, aby włączyć oświadczenia użytkownika. Zawiera metody lub dodawanie, usuwanie i pobiera oświadczenia użytkownika.
- **IUserLoginStore**  
 [IUserLoginStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) definiuje metody musi implementować w magazynie użytkownika, aby włączyć zewnętrzni dostawcy uwierzytelniania. Zawiera metody dodawania, usuwania i pobierania logowania użytkownika i metodę pobierania na podstawie informacji logowania użytkownika.
- **IUserRoleStore**  
 [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) interfejs definiuje metody musi implementować w magazynie użytkownika do mapowania użytkownika do roli. Zawiera metody do dodawania, usuwania i pobierania ról użytkownika i metodę sprawdzania, czy użytkownik jest przypisany do roli.
- **IUserPasswordStore**  
 [IUserPasswordStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) interfejs definiuje metody musi implementować w magazynie użytkownika, aby zachować wartość skrótu hasła. Zawiera metody służące do pobierania i ustawiania skrótem hasła i metody, która wskazuje, czy użytkownik ma ustawione hasło.
- **IUserSecurityStampStore**  
 [Elementu IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) interfejs definiuje metody musi implementować w magazynie użytkownika do używania sygnaturę bezpieczeństwa dla wskazującą, czy informacje o koncie użytkownika został zmieniony . Ta sygnatura jest aktualizowany, gdy użytkownik zmieni hasło, lub dodaje lub usuwa logowania. Zawiera metody służące do pobierania i ustawiania sygnatury bezpieczeństwa.
- **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) interfejs definiuje metody musi wdrożenie i implementowanie uwierzytelniania dwuskładnikowego. Zawiera metody służące do pobierania i ustawiania czy uwierzytelnianie dwuskładnikowe jest włączone dla użytkownika.
- **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) interfejs definiuje metody musi implementować do przechowywania numeru telefonu użytkownika. Zawiera metody służące do pobierania i ustawiania numeru telefonu i określa, czy numer telefonu został potwierdzony.
- **IUserEmailStore**  
 [IUserEmailStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) interfejs definiuje metody musi implementować do przechowywania adresu e-mail użytkownika. Zawiera metody służące do pobierania i ustawiania adres e-mail i czy adres e-mail został potwierdzony.
- **IUserLockoutStore**  
 [IUserLockoutStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) interfejs definiuje metody musi implementować do przechowywania informacji na temat blokowania konta. Zawiera metody służące do pobierania bieżącą liczbę nieudanych prób dostępu, pobieranie i czy można zablokować konto, pobierania i ustawiania datę zakończenia blokady, zwiększając liczbę nieudanych prób i zresetowanie liczby nieudanych prób.
- **IQueryableUserStore**  
 [IQueryableUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) interfejs definiuje członków, należy zaimplementować w celu zapewnienia magazynu użytkowników z obsługą zapytań. Zawiera on właściwość, która zawiera użytkowników z obsługą zapytań.

 Implementowanie interfejsów, które są potrzebne w aplikacji; takie jak IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore i elementu IUserSecurityStampStore interfejsów, jak pokazano poniżej. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Do zakończenia wdrożenia (w tym wszystkie interfejsy), zobacz [(MySQL) w elemencie UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin i IdentityUserRole

Przestrzeń nazw Microsoft.AspNet.Identity.EntityFramework zawiera implementacje [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), i [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) klasy. Jeśli używasz tych funkcji, można tworzyć własne wersje tych klas i zdefiniuj właściwości do aplikacji. Jednak czasami jest bardziej wydajne nie ładuje te jednostki do pamięci podczas wykonywania podstawowych operacji (na przykład dodawania lub usuwania oświadczenia użytkownika). Zamiast tego klasy magazynu wewnętrznej bazy danych mogą wykonywać te operacje bezpośrednio w źródle danych. Na przykład wywołać metodę UserStore.GetClaimsAsync() userClaimTable.FindByUserId(user. Identyfikator) metodę można wykonać zapytania na tabeli bezpośrednio i powrócić do listy oświadczeń.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Dostosowywanie klasy roli

Podczas implementowania dostawcy magazynu, należy utworzyć klasę roli, który jest odpowiednikiem [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) klasy w [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) przestrzeni nazw:

Na poniższym diagramie przedstawiono klasy IdentityRole, należy utworzyć i interfejs do zaimplementowania w tej klasie.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

[Rolę IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) interfejs definiuje właściwości, które RoleManager próbuje wywołać podczas wykonywania żądanej operacji. Interfejs zawiera dwie właściwości — identyfikator i nazwę. [Rolę IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) interfejs umożliwia określenie typu klucza dla roli za pomocą ogólnego **TKey** parametru. Typ właściwości Id jest zgodna z wartością parametru TKey.

Udostępnia również w ramach tożsamości [rolę IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) interfejsu (bez parametru ogólnego) Jeśli chcesz użyć wartości ciągu dla klucza.

W poniższym przykładzie przedstawiono klasę IdentityRole, która używa całkowitą dla klucza. W polu identyfikatora ma ustawioną na liczby całkowite pasuje do wartości parametru ogólnego. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Do ukończenia wdrożenia, zobacz [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Dostosowywanie magazynu ról

Możesz również utworzyć elemencie RoleStore klasę, która udostępnia metody dla wszystkich operacji danych w przypadku ról. Ta klasa jest odpowiednikiem [elemencie RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) klas w przestrzeni nazw Microsoft.ASP.NET.Identity.EntityFramework. W elemencie RoleStore klasy, należy zaimplementować [interfejs IRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) i opcjonalnie [IQueryableRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) interfejs.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

W poniższym przykładzie przedstawiono klasę magazynu roli. Parametr ogólny TRole przyjmuje typ swojej klasy roli, która zwykle jest klasą IdentityRole zdefiniowane przez użytkownika. Parametr ogólny TKey przyjmuje typ klucza roli. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
 [Interfejs IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) interfejs definiuje metody służące do implementacji w klasie magazynu roli. Zawiera metody do tworzenia, aktualizowania, usuwania i pobierania ról.
- **RoleStore&lt;TRole&gt;**  
 Aby dostosować elemencie RoleStore, Utwórz klasę, która implementuje interfejs interfejs IRoleStore. Masz tylko do implementowania tej klasy, jeśli chcesz użyć ról w systemie. Konstruktor, który przyjmuje parametr o nazwie *bazy danych* typu ExampleDatabase jest ilustracja sposób przekazywania w klasie dostępu do danych. Na przykład w implementacji MySQL ten konstruktor ma parametr typu MySQLDatabase.  
  
 Do ukończenia wdrożenia, zobacz [(MySQL) w elemencie RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Skonfiguruj ponownie aplikacji do korzystania z nowego dostawcy magazynu

Wdrożono nowego dostawcę magazynu. Teraz musisz skonfigurować aplikację do używania tego dostawcy magazynu. Domyślny dostawca magazynu został dołączony do projektu, należy usunąć domyślnego dostawcę, Zamień dostawcy.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Zastąp domyślnego dostawcę magazynu w projekcie MVC

1. W **Zarządzaj pakietami NuGet** okna, odinstaluj **platformy Microsoft ASP.NET Identity EntityFramework** pakietu. Ten pakiet można znaleźć, wyszukując Identity.EntityFramework w pakiety zainstalowane.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png)Uzyskasz, jeśli chcesz również odinstalować Entity Framework. Jeśli użytkownik nie jest potrzebny w innych częściach aplikacji, należy ją odinstalować.
2. W pliku IdentityModels.cs w folderze modeli, usuń lub komentarz **ApplicationUser** i **ApplicationDbContext** klasy. W aplikacji MVC można usunąć cały plik IdentityModels.cs. W aplikacji formularzy sieci Web należy usunąć dwie klasy, ale upewnij się, że zachowasz Klasa pomocy, która znajduje się również w pliku IdentityModels.cs.
3. Jeśli Twój dostawca magazynu znajduje się w oddzielny projekt, Dodaj odwołanie do niej w aplikacji sieci web.
4. Zamień wszystkie odwołania do `using Microsoft.AspNet.Identity.EntityFramework;` przy użyciu instrukcji dla przestrzeni nazw dostawcy magazynu.
5. W **Startup.Auth.cs** klasy, zmień **ConfigureAuth** metodę, aby używać jednego wystąpienia odpowiedniego kontekstu. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. W aplikacji\_Otwórz folder początkowy **IdentityConfig.cs**. W klasie ApplicationUserManager zmienić **Utwórz** metodę, aby zwrócić manager użytkownika, który używa Sklepu użytkownika. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Zamień wszystkie odwołania do **ApplicationUser** z **IdentityUser**.
8. Domyślny projekt zawiera niektóre elementy w klasie użytkownika, które nie są zdefiniowane w interfejsie IUser; takie jak wiadomości E-mail, PasswordHash i GenerateUserIdentityAsync. Klasy użytkownika nie ma tych członków, należy ich wdrażania lub zmień kod, który używa tych elementów członkowskich.
9. Jeśli utworzono wystąpienia RoleManager zmiany tego kodu w celu użycia nowej klasie elemencie RoleStore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Domyślny projekt jest przeznaczony dla klasy użytkownika, który ma wartość ciągu dla klucza. Jeśli własnej klasy user ma inny typ klucza (na przykład liczba całkowita), należy zmienić projektu do pracy z danego typu. Zobacz [zmienić klucz podstawowy dla użytkowników w produkcie ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. W razie potrzeby Dodaj parametry połączenia do pliku Web.config.

<a id="other"></a>
## <a name="other-resources"></a>Inne zasoby

- Blog: [implementacja tożsamości platformy ASP.NET](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Samouczek i GIT kodu: [dostawcy tożsamości Simple.Data Asp.Net](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Samouczek:[ustawiania podstawowa kont tożsamości i wskazując zewnętrznej bazy danych](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Przez [ @xivSolutions ](https://twitter.com/xivSolutions).
- Samouczek[: Implementowanie dostawcy magazynu tożsamości ASP.NET MySQL niestandardowych](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Jednostki CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) przez [SoftFluent](http://www.softfluent.com/)
- [Magazyn tabel Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) przez Nowak James.
- Magazyn tabel Azure: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) przez [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant przez Wertheim Danielowi.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Ciąg elastycznej[h: elastycznej tożsamości](https://github.com/bmbsqd/elastic-identity) przez Bombsquad AB.
- [Bazy danych MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) przez Sheely Jonathanowi Sheely Jonathanowi.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) by Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) przez [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) przez [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Szablony T4 do generowania EF code magazynu użytkowników "bazy danych najpierw": [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
