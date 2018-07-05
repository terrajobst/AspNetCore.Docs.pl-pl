---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: Omówienie niestandardowych dostawców magazynu dla produktu ASP.NET Identity | Dokumentacja firmy Microsoft
author: tfitzmac
description: ASP.NET Identity jest rozszerzalny system, co pozwala na tworzenie własnego dostawcę magazynu i podłącz go do aplikacji bez ponownego pracy likacji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.technology: ''
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c0f4badabb9c6886bceb2e084f39276a07359dbb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364478"
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Omówienie niestandardowych dostawców magazynu dla produktu ASP.NET Identity
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity jest rozszerzalny system, co pozwala na tworzenie własnego dostawcę magazynu i podłącz go do aplikacji bez ponownego pracy aplikacji. W tym temacie opisano, jak utworzyć dostawcę dostosowane magazynu dla produktu ASP.NET Identity. Poruszono w nim ważne pojęcia do tworzenia własnego dostawcy magazynu, ale nie jest to przewodnik krok po kroku implementowanie dostawcy magazynu niestandardowego.
> 
> Aby uzyskać przykład implementowanie dostawcy magazynu niestandardowego, zobacz [Implementowanie niestandardowego dostawcy magazynu tożsamości ASP.NET MySQL](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> W tym temacie został zaktualizowany na potrzeby tożsamości ASP.NET w wersji 2.0.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Visual Studio 2013 z aktualizacją Update 2
> - ASP.NET Identity 2


## <a name="introduction"></a>Wprowadzenie

Domyślnie system produktu ASP.NET Identity przechowuje informacje o użytkowniku w bazie danych programu SQL Server i korzysta z programu Entity Framework Code First utworzyć bazę danych. W przypadku wielu aplikacji ta metoda działa dobrze. Jednak warto użyć innego typu mechanizmu stanu trwałego, takich jak Azure Table Storage, lub tabele bazy danych ze strukturą bardzo inną niż domyślna implementacja może już istnieć. W obu przypadkach można pisać niestandardowe dostawcy dla Twojej mechanizm magazynu i podłączyć tego dostawcy do aplikacji.

ASP.NET Identity jest domyślnie włączone w wielu szablonów programu Visual Studio 2013. Możesz pobrać aktualizacje do produktu ASP.NET Identity za pośrednictwem [pakiet NuGet platformy Microsoft AspNet tożsamości EntityFramework](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Ten temat zawiera następujące sekcje:

- [Omówienie architektury](#architecture)
- [Dane są przechowywane informacje](#data)
- [Tworzenie warstwy dostępu do danych](#dal)
- [Dostosowywanie klasy użytkownika](#user)
- [Dostosowywanie magazynu użytkowników](#userstore)
- [Dostosowywanie klasy roli](#role)
- [Dostosowywanie magazynu ról](#rolestore)
- [Zmień konfigurację aplikacji do korzystania z nowego dostawcę magazynu](#reconfigure)
- [Inne implementacje niestandardowi dostawcy magazynu](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Omówienie architektury

ASP.NET Identity składa się z klasy o nazwie menedżerów i magazynów. Menedżerowie to ogólne klasy, które używa Deweloper aplikacji, aby wykonywać operacje, takie jak tworzenie użytkownika w systemie produktu ASP.NET Identity. Znajdują się sklepy, klasy niższego poziomu, które określają, jak jednostki, takie jak użytkownicy i role, są zachowywane. Magazyny są ściśle powiązane z mechanizmu stanu trwałego, ale menedżerów są całkowicie niezależni od magazynów co oznacza, że można zastąpić mechanizmu stanu trwałego, bez zakłócania działania całej aplikacji.

Na poniższym diagramie przedstawiono, jak aplikacja sieci web współdziałają z menedżerów i magazyny interakcji z warstwy dostępu do danych.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Aby utworzyć dostawcę dostosowane magazynu dla produktu ASP.NET Identity, należy utworzyć źródło danych, warstwy dostępu do danych i klasy magazynu, które współdziałają z tą warstwą dostępu do danych. Możesz kontynuować korzystanie z Menedżera tych samych interfejsów API do wykonywania operacji na danych użytkownika, ale teraz, gdy dane są zapisywane do innego magazynu systemu.

Nie trzeba dostosować klasy manager, ponieważ podczas tworzenia nowego wystąpienia menedżera UserManager lub RoleManager podać typ klasy użytkownika i przekazać wystąpienia klasy magazynu jako argument. Takie podejście umożliwia Podłącz Twoich zajęciach dostosowane do istniejącej struktury. Pokazano, jak utworzyć interfejs UserManager i RoleManager z Twoich zajęciach dostosowane magazynu w sekcji [ponownie skonfigurować aplikację do korzystania z nowego dostawcę magazynu](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Dane są przechowywane informacje

Do implementowania dostawcy magazynu niestandardowego, należy zapoznać się z typami danych używanych w produkcie ASP.NET Identity i zdecydować, które funkcje mają zastosowanie do aplikacji.

| Dane | Opis |
| --- | --- |
| Użytkownicy | Zarejestrowani użytkownicy witryny sieci web. Zawiera nazwę identyfikator i nazwę użytkownika. Może zawierać skrótem hasła, jeżeli użytkownicy logują się przy użyciu poświadczeń, które są specyficzne dla lokacji (zamiast przy użyciu poświadczeń z witryny zewnętrznej, takich jak Facebook) oraz sygnatura bezpieczeństwa, aby wskazać, czy wszystko się zmieniło w poświadczeń użytkownika. Może również zawierać adres e-mail, numer telefonu, czy uwierzytelnianie dwuskładnikowe jest włączone, bieżącą liczbę nieudanych prób logowania, oraz czy konto zostało zablokowane. |
| Oświadczenia użytkownika | Zestaw instrukcji (lub oświadczenia) o użytkowniku, które reprezentują tożsamość użytkownika. Aby umożliwić większego wyrażenia tożsamość użytkownika nie można osiągnąć za pomocą ról. |
| Identyfikatory logowania użytkownika | Informacje o dostawcy uwierzytelniania zewnętrznych (takich jak Facebook) do użycia podczas logowania użytkownika. |
| Role | Grupy autoryzacji dla danej witryny. Zawiera identyfikator i roli nazwę roli (na przykład "Admin" lub "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Tworzenie warstwy dostępu do danych

W tym temacie założono, że czytelnik zna mechanizmu stanu trwałego, które będą korzystać i jak tworzyć jednostki dla tego mechanizmu. Ten temat nie zawiera szczegółowe informacje na temat tworzenia repozytoriów lub klas dostępu do danych; Zamiast tego zapewnia pewne sugestie dotyczące decyzji projektowych potrzebne podczas pracy w produkcie ASP.NET Identity.

Masz wiele swobody przy projektowaniu repozytoriów dla dostosowany przechowywania dostawcy. Musisz utworzyć repozytoriów dla funkcji, które będą używane w aplikacji. Na przykład jeśli nie używasz role w aplikacji, nie trzeba utworzyć magazynu dla ról lub ról użytkownika. Z technologii i istniejącej infrastruktury może wymagać to struktura, która jest bardzo inna niż domyślna implementacja produktu ASP.NET Identity. W warstwie dostępu do danych możesz zapewnić logikę do pracy ze strukturą repozytoriów.

Aby MySQL implementacja repozytoria danych dla tożsamości ASP.NET w wersji 2.0, zobacz [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

W warstwie dostępu do danych, możesz zapewnić logikę, aby zapisać dane z produktu ASP.NET Identity do źródła danych. Warstwa dostępu do danych dla dostawcy magazynu dostosowanych może obejmować następujące klasy do przechowywania informacji o użytkowniku i ról.

| Class | Opis | Przykład |
| --- | --- | --- |
| Kontekst | Hermetyzuje informacje, aby połączyć się z mechanizmu stanu trwałego i wykonywania zapytań. Ta klasa jest decydujące znaczenie dla Twojej warstwy dostępu do danych. Inne klasy danych będzie wymagać wystąpienia tej klasy, aby wykonywać operacje. Zostanie również inicjują Twoich zajęciach magazynu z wystąpieniem tej klasy. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Magazyn użytkownika | Przechowuje oraz pobiera informacje o użytkowniku (na przykład skrót nazwy i hasła użytkownika). | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Rola magazynu | Przechowuje oraz pobiera informacje o roli (np. nazwy roli). | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Magazyn w oświadczeniach Userclaim | Przechowuje oraz pobiera informacje o użytkownika (na przykład typ oświadczenia i wartości). | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Magazyn UserLogins | Przechowuje oraz pobiera dane logowania użytkownika (np. dostawcę uwierzytelniania zewnętrznych). | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| UserRole magazynu | Przechowuje oraz pobiera role, których użytkownik jest przypisany do. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Ponownie wystarczy do zaimplementowania klasy, które będą używane w aplikacji.

W klasach dostępu do danych musisz podać kod w celu wykonywania operacji na danych dla Twojego mechanizmu stanu trwałego określonej. Na przykład w ramach implementacji MySQL, klasa UserTable zawiera metodę, aby wstawić nowy rekord w tabeli bazy danych użytkowników. Zmienna o nazwie `_database` jest wystąpieniem klasy MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Po utworzeniu usługi klas dostępu do danych, należy utworzyć klasy magazynu, które wywołać określonej metody w warstwie dostępu do danych.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Dostosowywanie klasy użytkownika

Podczas implementowania dostawcy magazynu, należy utworzyć klasę użytkownika, który jest odpowiednikiem [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) klasy w [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) przestrzeni nazw:

Na poniższym diagramie przedstawiono klasy IdentityUser, należy utworzyć i interfejs do zaimplementowania w tej klasie.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

[IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) interfejs definiuje właściwości, które interfejs UserManager podejmuje próbę wywołania, gdy wykonywanie żądanej operacji. Interfejs zawiera dwie właściwości — identyfikator i nazwa użytkownika. [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) interfejs umożliwia określenie typu klucza dla użytkownika za pomocą ogólnego **TKey** parametru. Typ właściwości identyfikatora pasuje do wartości parametru TKey.

Struktura tożsamości są także [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) interfejsu (bez parametru ogólnego) kiedy chcesz użyć wartości ciągu dla klucza.

Klasa IdentityUser implementuje IUser i zawiera żadnych dodatkowych właściwości lub konstruktory dla użytkowników w witrynie sieci web. Poniższy przykład przedstawia klasę IdentityUser, korzystającą z liczbą całkowitą dla klucza. Pole identyfikatora jest równa **int** być zgodna z wartością parametru ogólnego. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Aby uzyskać pełną implementację, zobacz [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Dostosowywanie magazynu użytkowników

Możesz również utworzyć klasę magazynie UserStore, która zapewnia metody wszystkie operacje na danych użytkownika. Ta klasa jest odpowiednikiem [magazynie UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) klasy w [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) przestrzeni nazw. W klasie magazynie UserStore zaimplementować [elementy IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) i opcjonalnie interfejsów. Możesz wybrać opcjonalne interfejsy, które można zaimplementować, w oparciu o funkcjonalność, którą możesz podać w aplikacji.

Na poniższej ilustracji przedstawiono klasy magazynie UserStore, które należy utworzyć i odpowiednich interfejsów.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Domyślny szablon projektu w programie Visual Studio zawiera kod, który przyjęto założenie, że wiele interfejsów opcjonalne zostało wdrożonych w magazynie użytkownika. Używasz szablonu domyślnego z magazynem użytkownika, musi implementować interfejsy opcjonalny w magazynie użytkownika lub zmienić kod szablonu nie jest już wywoływanie metod w interfejsach, który nie został zaimplementowany.

 Poniższy przykład przedstawia klasę magazynu proste użytkownika. **TUser** parametru ogólnego przyjmuje typ, czyli klasy IdentityUser zdefiniowano klasy użytkownika. **TKey** parametru ogólnego przyjmuje typ klucza użytkownika. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 W tym przykładzie Konstruktor, który przyjmuje parametr o nazwie *bazy danych* typu ExampleDatabase jest ilustruje sposób przekazywania w klasie dostępu do danych. Na przykład w implementacji MySQL, ten konstruktor przyjmuje parametr typu MySQLDatabase. 

W elemencie UserStore klasy możesz użyć klas dostępu do danych, które zostały utworzone w celu wykonania operacji. Na przykład w implementacji MySQL magazynie UserStore klasa ma metodę CreateAsync, która używa wystąpienia UserTable Aby wstawić nowy rekord. **Wstaw** metody **userTable** obiekt jest w tej samej metody, który został wyświetlony w poprzedniej sekcji. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfejsy do implementacji podczas dostosowywania magazynu użytkowników

Na następnej ilustracji przedstawiono więcej informacji na temat funkcji zdefiniowane w każdego interfejsu. Dziedziczy wszystkie interfejsy opcjonalne elementy IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **Elementy IUserStore**  
  [Elementy IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) interfejs jest jedyną interfejsu, należy zaimplementować w magazynie użytkownika. Definiuje metody do tworzenia, aktualizowania i pobierania użytkowników.
- **IUserClaimStore**  
  [IUserClaimStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) interfejs definiuje metody należy zaimplementować w magazynie użytkownika, aby włączyć oświadczenia użytkownika. Zawiera metody lub dodawania, usuwania i pobierania oświadczeń użytkowników.
- **IUserLoginStore**  
  [IUserLoginStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) definiuje metody należy zaimplementować w magazynie użytkownika, aby umożliwić dostawcy uwierzytelniania zewnętrznego. Zawiera metody dodawania, usuwania i pobierania danych logowania użytkownika i metodę pobierania na podstawie informacji logowania użytkownika.
- **IUserRoleStore**  
  [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) interfejs definiuje metody należy zaimplementować w magazynie użytkownika do mapowania użytkownika do roli. Zawiera metody służące do dodawania, usuwania i pobierania ról użytkownika i metodę sprawdzania, jeśli użytkownik jest przypisany do roli.
- **IUserPasswordStore**  
  [IUserPasswordStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) interfejs definiuje metody, należy zaimplementować w magazynie użytkownika, aby utrwalić skrótu hasła. Zawiera metody do pobierania i ustawiania skrótem hasła, a metoda, która wskazuje, czy użytkownik ma ustawione hasło.
- **IUserSecurityStampStore**  
  [IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) interfejs definiuje metody, należy zaimplementować w magazynie użytkownika na potrzeby wskazującą, czy informacje o koncie użytkownika została zmieniona przez sygnaturę bezpieczeństwa . Ta sygnatura jest aktualizowany, gdy użytkownik zmieni hasło, lub dodaje lub usuwa nazwy logowania. Zawiera metody do pobierania i ustawiania sygnatury bezpieczeństwa.
- **IUserTwoFactorStore**  
  [IUserTwoFactorStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) interfejs definiuje metody muszą implementacji i implementowanie uwierzytelniania dwuskładnikowego. Zawiera metody do pobierania i ustawiania czy uwierzytelnianie dwuskładnikowe jest włączone dla użytkownika.
- **IUserPhoneNumberStore**  
  [IUserPhoneNumberStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) interfejs definiuje metody musi implementować, aby zapisać numery telefonów użytkowników. Zawiera metody do pobierania i ustawiania numeru telefonu i tego, czy numer telefonu został potwierdzony.
- **IUserEmailStore**  
  [IUserEmailStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) interfejs definiuje metody musi implementować, aby przechowywać adresy e-mail użytkowników. Zawiera metody do pobierania i ustawiania adresu e-mail i tego, czy adres e-mail został potwierdzony.
- **IUserLockoutStore**  
  [IUserLockoutStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) interfejs definiuje metody musi implementować do przechowywania informacji na temat blokowania kont. Zawiera metody służące do pobierania bieżącą liczbę nieudanych prób dostępu, pobieranie i ustawienia, czy można zablokować konto, pobieranie i ustawianie blokady Data zakończenia, zwiększając liczbę nieudanych prób i zresetowanie liczby nieudanych prób.
- **IQueryableUserStore**  
  [IQueryableUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) interfejs definiuje członków, należy zaimplementować w celu zapewnienia magazynu użytkowników z obsługą zapytań. Zawiera właściwości, która zawiera użytkowników, obsługą zapytań.

  Implementowanie interfejsów, które są potrzebne w Twojej aplikacji; takie jak IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore i IUserSecurityStampStore interfejsów, jak pokazano poniżej. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Aby uzyskać pełną implementację (w tym wszystkie interfejsy), zobacz [(MySQL) w magazynie UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin i IdentityUserRole

Przestrzeń nazw Microsoft.AspNet.Identity.EntityFramework zawiera implementacje [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), i [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) klasy. Jeśli używasz tych funkcji można tworzyć własne wersje tych klas oraz zdefiniować właściwości dla aplikacji. Jednak czasami jest bardziej wydajne, nie były ładowane te jednostki do pamięci podczas wykonywania podstawowych operacji (takich jak dodawanie lub usuwanie oświadczeń użytkownika). Zamiast tego klasy magazynu zaplecza mogą wykonywać te operacje bezpośrednio w źródle danych. Na przykład wywołać metodę UserStore.GetClaimsAsync() userClaimTable.FindByUserId(user. Identyfikator) metoda do wykonania zapytania w tabeli bezpośrednio i powrócić do listy oświadczeń.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Dostosowywanie klasy roli

Podczas implementowania dostawcy magazynu, należy utworzyć klasy roli, która jest odpowiednikiem [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) klasy w [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) przestrzeni nazw:

Na poniższym diagramie przedstawiono klasy IdentityRole, należy utworzyć i interfejs do zaimplementowania w tej klasie.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

[Rolę IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) interfejs definiuje właściwości, które RoleManager podejmuje próbę wywołania, gdy wykonywanie żądanej operacji. Interfejs zawiera dwie właściwości — identyfikator i nazwę. [Rolę IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) interfejs umożliwia określenie typu klucza dla roli za pomocą ogólnego **TKey** parametru. Typ właściwości identyfikatora pasuje do wartości parametru TKey.

Struktura tożsamości są także [rolę IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) interfejsu (bez parametru ogólnego) kiedy chcesz użyć wartości ciągu dla klucza.

Poniższy przykład przedstawia klasę IdentityRole, korzystającą z liczbą całkowitą dla klucza. Pole identyfikatora ustawiono na liczby całkowite pasuje do wartości parametru ogólnego. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Aby uzyskać pełną implementację, zobacz [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Dostosowywanie magazynu ról

Możesz również utworzyć klasę elemencie RoleStore, która zapewnia metody wszystkie operacje na danych w ramach ról. Ta klasa jest odpowiednikiem [elemencie RoleStore&lt;element TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) klasy w przestrzeni nazw Microsoft.ASP.NET.Identity.EntityFramework. W elemencie RoleStore klasy, należy zaimplementować [interfejs IRoleStore&lt;element TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) i opcjonalnie [IQueryableRoleStore&lt;element TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) interfejs.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Poniższy przykład przedstawia klasę magazynu ról. Parametr ogólny element TRole przyjmuje typ klasy usługi roli, która zwykle jest to klasa IdentityRole, zdefiniowane przez użytkownika. Parametr ogólny TKey przyjmuje typ klucza usługi roli. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  [Interfejs IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) interfejs definiuje metody służące do zaimplementowania w klasie magazynu ról. Zawiera ona metody tworzenia, aktualizowania, usuwania i pobierania ról.
- **RoleStore&lt;TRole&gt;**  
  Aby dostosować elemencie RoleStore, należy utworzyć klasę, która implementuje interfejs interfejs IRoleStore. Musisz zaimplementować tę klasę, jeśli chcesz użyć ról w Twoim systemie. Konstruktor, który przyjmuje parametr o nazwie *bazy danych* typu ExampleDatabase jest ilustruje sposób przekazywania w klasie dostępu do danych. Na przykład w implementacji MySQL, ten konstruktor przyjmuje parametr typu MySQLDatabase.  
  
  Aby uzyskać pełną implementację, zobacz [(MySQL) w elemencie RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Zmień konfigurację aplikacji do korzystania z nowego dostawcę magazynu

Udało Ci się wdrożyć nowego dostawcy magazynu. Teraz musisz skonfigurować aplikację do używania tego dostawcy magazynu. Jeśli domyślny dostawca magazynu został uwzględniony w projekcie, należy usunąć domyślny dostawca i zastąp go przy użyciu dostawcy.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Zamień domyślny dostawca magazynu w projekcie MVC

1. W **Zarządzaj pakietami NuGet** okna, odinstaluj **Microsoft ASP.NET Identity EntityFramework** pakietu. Ten pakiet można znaleźć, wyszukując w pakietach zainstalowane Identity.EntityFramework.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) Uzyskasz, jeśli chcesz także odinstalowanie programu Entity Framework. Jeśli użytkownik nie jest potrzebny w innych częściach aplikacji, należy go odinstalować.
2. W pliku IdentityModels.cs w folderze modeli, usunąć lub skomentować **ApplicationUser** i **ApplicationDbContext** klasy. W aplikacji MVC możesz usunąć cały plik IdentityModels.cs. W aplikacji formularzy sieci Web należy usunąć dwie klasy, ale upewnij się, że zachowasz klasy pomocnika, która znajduje się również w pliku IdentityModels.cs.
3. Jeśli Twój dostawca magazynu znajduje się w osobnym projekcie, należy dodać odwołanie do niego w aplikacji sieci web.
4. Zastąp wszystkie odwołania do `using Microsoft.AspNet.Identity.EntityFramework;` z za pomocą instrukcji dla przestrzeni nazw z dostawcą magazynu.
5. W **Startup.Auth.cs** klasy, zmienić **ConfigureAuth** metodę, aby używać jednego wystąpienia odpowiedniego kontekstu. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. W aplikacji\_Otwórz folder początkowy **IdentityConfig.cs**. W klasie ApplicationUserManager zmienić **Utwórz** metodę, aby zwrócić manager użytkownika, który używa Sklepu użytkownika. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Zastąp wszystkie odwołania do **ApplicationUser** z **IdentityUser**.
8. Domyślny projekt zawiera niektóre elementy członkowskie klasy użytkownika, które nie są zdefiniowane w interfejsie IUser; np. poczty E-mail, PasswordHash i GenerateUserIdentityAsync. Klasy użytkownika nie ma tych członków, musisz je zastosować lub zmień kod, który używa tych elementów członkowskich.
9. Jeśli utworzono wszystkie wystąpienia elementu RoleManager, należy zmienić ten kod w celu użycia nowej klasie elemencie RoleStore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Domyślny projekt jest przeznaczony dla klasy użytkownika, który ma wartość ciągu dla klucza. Jeśli klasa użytkownika ma inny typ klucza (na przykład liczba całkowita), należy zmienić projekt, aby pracować z danego typu. Zobacz [zmiana klucza podstawowego dla użytkowników w produkcie ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Jeśli to konieczne, należy dodać parametry połączenia do pliku Web.config.

<a id="other"></a>
## <a name="other-resources"></a>Inne zasoby

- Blog: [wdrażanie produktu ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Samouczek i GIT kodu: [dostawca tożsamości platformy Asp.Net Simple.Data](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Samouczek:[konfigurowania podstawowych kont tożsamości i wskazując zewnętrznej bazy danych](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Przez [ @xivSolutions ](https://twitter.com/xivSolutions).
- Samouczek[: Implementowanie dostawcy magazynu MySQL niestandardowe produktu ASP.NET Identity](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Jednostki CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) przez [SoftFluent](http://www.softfluent.com/)
- [Usługa Azure Table Storage](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) przez James Randall.
- Usługa Azure Table Storage: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) przez [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant przez Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Ciąg elastycznej[h: elastyczne tożsamości](https://github.com/bmbsqd/elastic-identity) przez Bombsquad AB.
- [Bazy danych MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) przez Sheely Jonathana Sheely Jonathana.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) przez Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) przez [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) przez [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Szablony T4 do generowania EF code magazynu użytkowników "bazy danych najpierw": [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
