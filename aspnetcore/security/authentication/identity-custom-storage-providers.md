---
title: Niestandardowi dostawcy magazynu dla produktu ASP.NET Core Identity
author: ardalis
description: Dowiedz się, jak skonfigurować niestandardowi dostawcy magazynu dla produktu ASP.NET Core Identity.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: ccd56d0c15639e1ad29094e947f8055702ee2264
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833673"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Niestandardowi dostawcy magazynu dla produktu ASP.NET Core Identity

Przez [Steve Smith](https://ardalis.com/)

Tożsamość platformy ASP.NET Core jest rozszerzalny system, co pozwala na tworzenie dostawcy magazynu niestandardowego i podłącz go do swojej aplikacji. W tym temacie opisano sposób tworzenia dostawcy magazynu dostosowanych do tożsamości platformy ASP.NET Core. Opisano ważne pojęcia do tworzenia własnego dostawcy magazynu, ale nie jest przewodnik krok po kroku.

[Wyświetlanie lub pobieranie przykładowy z serwisu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Wprowadzenie

Domyślnie system tożsamości platformy ASP.NET Core przechowuje informacje o użytkowniku w bazie danych programu SQL Server przy użyciu platformy Entity Framework Core. Wiele aplikacji ta metoda działa dobrze. Można jednak używać mechanizmu stanu trwałego różnych lub schemat danych. Na przykład:

* Możesz użyć [Azure Table Storage](/azure/storage/) lub w innym magazynie danych.
* Tabele bazy danych ma inną strukturę. 
* Możesz też chcieć skorzystać z różnymi danymi dostępu podejścia, takie jak [programem Dapper](https://github.com/StackExchange/Dapper). 

W każdym z tych przypadków możesz napisać dostosowane dostawcy dla Twojego mechanizm magazynu i dołączyć tego dostawcy w swojej aplikacji.

Tożsamość platformy ASP.NET Core znajduje się w szablonach projektu w programie Visual Studio przy użyciu opcji "Pojedyncze konta użytkowników".

Korzystając z interfejsu wiersza polecenia platformy .NET Core, Dodaj `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>Architektura tożsamości platformy ASP.NET Core

Tożsamości platformy ASP.NET Core składa się z klasy o nazwie menedżerów i magazynów. *Menedżerowie* są ogólne klasy, które używa Deweloper aplikacji, aby wykonywać operacje, takie jak tworzenie tożsamości użytkownika. *Magazyny* są klasami niższego poziomu, które określają, jak jednostki, takie jak użytkownicy i role, są zachowywane. Magazyny oparte na wzorcu repozytorium i są ściśle powiązane z mechanizmu stanu trwałego. Menedżerowie są całkowicie niezależni od magazynów, co oznacza, że można zastąpić mechanizmu stanu trwałego, bez konieczności zmieniania kodu aplikacji (z wyjątkiem konfiguracji).

Na poniższym diagramie przedstawiono, jak aplikacja sieci web współdziała z menedżerów, natomiast sklepy interakcji z warstwy dostępu do danych.

![Aplikacje platformy ASP.NET Core pracować z menedżerów (na przykład "Interfejs UserManager", "RoleManager"). Menedżerowie pracy z magazynami (na przykład "magazynie UserStore"), które komunikują się ze źródłem danych przy użyciu biblioteki, takich jak Entity Framework Core.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Aby utworzyć dostawcę magazynu niestandardowego, należy utworzyć źródło danych, warstwy dostępu do danych i klasy magazynu, które współdziałają z tą warstwą dostępu do danych (zielony i szary pola na powyższym diagramie). Nie trzeba dostosować menedżerów lub w kodzie aplikacji, która wchodzi w interakcję z nimi (powyżej pola niebieski).

Podczas tworzenia nowego wystąpienia klasy `UserManager` lub `RoleManager` udostępniają typ klasy użytkownika i przekazać wystąpienia klasy magazynu jako argument. Takie podejście umożliwia Podłącz Twoich zajęciach dostosowane do platformy ASP.NET Core. 

[Ponownie skonfigurować aplikację do używania nowego dostawcę magazynu](#reconfigure-app-to-use-a-new-storage-provider) pokazuje, jak utworzyć `UserManager` i `RoleManager` ze sklepem dostosowane.

## <a name="aspnet-core-identity-stores-data-types"></a>Typy danych są przechowywane tożsamości platformy ASP.NET Core

[Tożsamości platformy ASP.NET Core](https://github.com/aspnet/identity) typy danych są szczegółowo opisane w poniższych sekcjach:

### <a name="users"></a>Użytkownicy

Zarejestrowani użytkownicy witryny sieci web. [IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser) typ może być rozszerzony lub jako przykład użyto dla niestandardowego typu. Nie trzeba dziedziczyć z określonego typu, aby zaimplementować własnego rozwiązania magazynu tożsamości niestandardowej.

### <a name="user-claims"></a>Oświadczenia użytkownika

Zestaw instrukcji (lub [oświadczeń](/dotnet/api/system.security.claims.claim)) o użytkowniku, które reprezentują tożsamość użytkownika. Aby umożliwić większego wyrażenia tożsamość użytkownika nie można osiągnąć za pomocą ról.

### <a name="user-logins"></a>Identyfikatory logowania użytkownika

Informacje o dostawcy uwierzytelniania zewnętrznych (takich jak Facebook lub konta Microsoft) do użycia podczas logowania użytkownika. [Przykład](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Role

Grupy autoryzacji dla danej witryny. Zawiera identyfikator i roli nazwę roli (na przykład "Admin" lub "Employee"). [Przykład](/dotnet/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>Warstwa dostępu do danych

W tym temacie założono, że czytelnik zna mechanizmu stanu trwałego, które będą korzystać i jak tworzyć jednostki dla tego mechanizmu. W tym temacie nie zawierają szczegółowe informacje dotyczące sposobu tworzenia repozytoriów lub klas dostępu do danych; Podczas pracy z usługą tożsamości platformy ASP.NET Core, zapewnia sugestie dotyczące decyzji projektowych.

Masz dużo swobodę podczas projektowania warstwy dostępu do danych dla dostawcy magazynu dostosowane. Musisz utworzyć mechanizmów stanów trwałych dla funkcji, które będą używane w aplikacji. Na przykład jeśli nie używasz role w aplikacji, nie trzeba utworzyć magazynu dla ról lub powiązania roli użytkownika. Z technologii i istniejącej infrastruktury może wymagać to struktura, która jest bardzo inna niż domyślna implementacja tożsamości platformy ASP.NET Core. W warstwie dostępu do danych możesz zapewnić logikę do pracy ze strukturą implementacji magazynu.

Warstwa dostępu do danych zawiera logikę do zapisywania danych z platformy ASP.NET Core Identity ze źródłem danych. Warstwa dostępu do danych dla dostawcy magazynu dostosowanych może obejmować następujące klasy do przechowywania informacji o użytkowniku i ról.

### <a name="context-class"></a>Context — Klasa

Hermetyzuje informacje, aby połączyć się z mechanizmu stanu trwałego i wykonywania zapytań. Kilka klas danych wymaga wystąpienia tej klasy zwykle zapewniane za pomocą iniekcji zależności. [Przykład](/dotnet/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Magazyn użytkownika

Przechowuje oraz pobiera informacje o użytkowniku (na przykład skrót nazwy i hasła użytkownika). [Przykład](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Role Storage

Przechowuje oraz pobiera informacje o roli (np. nazwy roli). [Przykład](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Magazyn w oświadczeniach Userclaim

Przechowuje oraz pobiera informacje o użytkownika (na przykład typ oświadczenia i wartości). [Przykład](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Magazyn UserLogins

Przechowuje oraz pobiera dane logowania użytkownika (np. dostawcę uwierzytelniania zewnętrznych). [Przykład](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>UserRole Storage

Przechowuje oraz pobiera, które role są przypisane do użytkowników, którzy. [Przykład](/dotnet/api/microsoft.aspnet.identity.corecompat.userstore-1)

**PORADA:** Implementację tylko klasy, których zamierzasz używać w aplikacji.

Klasy dostępu do danych zawiera kod, aby wykonywać operacje na danych dla Twojego mechanizmu stanu trwałego. Na przykład w ramach niestandardowego dostawcy może mieć następujący kod, aby utworzyć nowego użytkownika w *przechowywania* klasy:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

Logika implementacji do tworzenia użytkownika znajduje się w `_usersTable.CreateAsync` przedstawiona poniżej metoda.

## <a name="customize-the-user-class"></a>Dostosowywanie klasy użytkownika

Podczas implementowania dostawcy magazynu, należy utworzyć klasę użytkownika, który jest odpowiednikiem [klasy IdentityUser](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuser).

Jako minimum, musi zawierać klasy user `Id` i `UserName` właściwości.

`IdentityUser` Klasy definiuje właściwości, `UserManager` wywołań podczas przeprowadzania zażądano operacji. Domyślny typ `Id` właściwość ma postać ciągu, ale może dziedziczyć `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` i określ innego typu. Struktura oczekuje, że implementacja magazynu do obsługi konwersji typu danych.

## <a name="customize-the-user-store"></a>Dostosowywanie magazynu użytkowników

Utwórz `UserStore` klasę, która zapewnia metody wszystkie operacje na danych użytkownika. Ta klasa jest odpowiednikiem [magazynie UserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) klasy. W swojej `UserStore` klasy, implementować `IUserStore<TUser>` i opcjonalnie interfejsy wymagane. Możesz wybrać interfejsy, które opcjonalnie zaimplementować, w oparciu o funkcjonalność podana w swojej aplikacji.

### <a name="optional-interfaces"></a>Opcjonalne interfejsów

* [IUserRoleStore](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)
* [IUserClaimStore](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)
* [IUserPasswordStore](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)
* [IUserSecurityStampStore](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)
* [IUserEmailStore](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)
* [IUserPhoneNumberStore](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1)
* [IQueryableUserStore](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)
* [IUserLoginStore](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)
* [IUserTwoFactorStore](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)
* [IUserLockoutStore](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)

Opcjonalne interfejsy dziedziczyć `IUserStore<TUser>`. Możesz zobaczyć częściowo zaimplementowany przykładowego użytkownika przechowywane w [przykładową aplikację](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

W ramach `UserStore` klasy, użyj klasy dostępu do danych, które zostały utworzone w celu wykonania operacji. Są one przekazywane przy użyciu iniekcji zależności. Na przykład w programie SQL Server z programem Dapper implementacji `UserStore` klasa ma `CreateAsync` metodę, która korzysta z wystąpienia `DapperUsersTable` Aby wstawić nowy rekord:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfejsy do implementacji podczas dostosowywania magazynu użytkowników

* **IUserStore**  
 [Elementy IUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) interfejs jest jedyną interfejsu, należy zaimplementować w magazynie użytkownika. Definiuje metody do tworzenia, aktualizowania i pobierania użytkowników.
* **IUserClaimStore**  
 [IUserClaimStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interfejs definiuje metody wdrożenia włączania oświadczeń użytkowników. Zawiera metody dodawania, usuwania i pobierania oświadczeń użytkowników.
* **IUserLoginStore**  
 [IUserLoginStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) definiuje metody implementacji umożliwiające dostawcy uwierzytelniania zewnętrznego. Zawiera metody dodawania, usuwania i pobierania danych logowania użytkownika i metodę pobierania na podstawie informacji logowania użytkownika.
* **IUserRoleStore**  
 [IUserRoleStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) interfejs definiuje metody wdrożenia do mapowania użytkownika do roli. Zawiera metody służące do dodawania, usuwania i pobierania ról użytkownika i metodę sprawdzania, jeśli użytkownik jest przypisany do roli.
* **IUserPasswordStore**  
 [IUserPasswordStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interfejs definiuje metody wdrożenia można utrwalić haseł w formie skrótu. Zawiera metody do pobierania i ustawiania skrótem hasła, a metoda, która wskazuje, czy użytkownik ma ustawione hasło.
* **IUserSecurityStampStore**  
 [IUserSecurityStampStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interfejs definiuje metody implementacji wskazującą, czy informacje o koncie użytkownika został zmieniony na użytek sygnatury bezpieczeństwa. Ta sygnatura jest aktualizowany, gdy użytkownik zmieni hasło, lub dodaje lub usuwa nazwy logowania. Zawiera metody do pobierania i ustawiania sygnatury bezpieczeństwa.
* **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interfejs definiuje metody wdrożenia do obsługi uwierzytelniania dwuskładnikowego. Zawiera metody do pobierania i ustawiania czy uwierzytelnianie dwuskładnikowe jest włączone dla użytkownika.
* **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interfejs definiuje metody implementacji przechowywania numerów telefonów użytkownika. Zawiera metody do pobierania i ustawiania numeru telefonu i tego, czy numer telefonu został potwierdzony.
* **IUserEmailStore**  
 [IUserEmailStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) interfejs definiuje metody implementacji przechowywania adresów e-mail użytkownika. Zawiera metody do pobierania i ustawiania adresu e-mail i tego, czy adres e-mail został potwierdzony.
* **IUserLockoutStore**  
 [IUserLockoutStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interfejs definiuje metody implementacji przechowywania informacji na temat blokowania kont. Zawiera metody do śledzenia nieudanych prób dostępu i blokady.
* **IQueryableUserStore**  
 [IQueryableUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interfejs definiuje członków, możesz wdrożyć w celu zapewnienia magazynu użytkowników z obsługą zapytań.

Implementuje się interfejsy, które są potrzebne w Twojej aplikacji. Na przykład:

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin i IdentityUserRole

`Microsoft.AspNet.Identity.EntityFramework` Przestrzeń nazw zawiera implementacje [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/dotnet/api/microsoft.aspnet.identity.corecompat.identityuserlogin), i [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) klasy. Jeśli używasz tych funkcji można tworzyć własne wersje tych klas i zdefiniuj właściwości dla aplikacji. Jednak czasami jest bardziej wydajne, nie były ładowane te jednostki do pamięci podczas wykonywania podstawowych operacji (takich jak dodawanie lub usuwanie oświadczeń użytkownika). Zamiast tego klasy magazynu zaplecza mogą wykonywać te operacje bezpośrednio w źródle danych. Na przykład `UserStore.GetClaimsAsync` można wywołać metody `userClaimTable.FindByUserId(user.Id)` metodę do wykonania zapytania w tabeli bezpośrednio i powrócić do listy oświadczeń.

## <a name="customize-the-role-class"></a>Dostosowywanie klasy roli

Podczas implementowania dostawcy magazynu ról, można utworzyć typ roli niestandardowej. Potrzebujesz implementuje danego interfejsu, ale musi on mieć `Id` i zwykle mają `Name` właściwości.

Oto przykład klasy roli:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Dostosowywanie magazynu ról

Możesz utworzyć `RoleStore` klasę, która zapewnia metody wszystkie operacje na danych w ramach ról. Ta klasa jest odpowiednikiem [elemencie RoleStore&lt;element TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) klasy. W `RoleStore` klasy należy zaimplementować `IRoleStore<TRole>` i opcjonalnie `IQueryableRoleStore<TRole>` interfejsu.

* **IRoleStore&lt;TRole&gt;**  
 [Interfejs IRoleStore&lt;element TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) interfejs definiuje metody służące do zaimplementowania w klasie magazynu ról. Zawiera ona metody tworzenia, aktualizowania i pobierania ról.
* **RoleStore&lt;TRole&gt;**  
 Aby dostosować `RoleStore`, Utwórz klasę, która implementuje `IRoleStore<TRole>` interfejsu. 

## <a name="reconfigure-app-to-use-a-new-storage-provider"></a>Ponownie skonfigurować aplikację do używania nowego dostawcę magazynu

Po wdrożeniu dostawcy magazynu, możesz skonfigurować aplikację do używania go. Jeśli aplikacja używany domyślny dostawca, zastąp go niestandardowego dostawcy.

1. Usuń `Microsoft.AspNetCore.EntityFramework.Identity` pakietu NuGet.
1. Jeśli dostawca magazynu znajduje się w osobnym projekcie lub pakietu, należy dodać odwołanie do niej.
1. Zastąp wszystkie odwołania do `Microsoft.AspNetCore.EntityFramework.Identity` z za pomocą instrukcji dla przestrzeni nazw z dostawcą magazynu.
1. W `ConfigureServices` metodę, zmień `AddIdentity` metodę, aby użyć niestandardowych typów. Można tworzyć swoje własne metody rozszerzenia, w tym celu. Zobacz [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) przykład.
1. Jeśli używasz ról, należy zaktualizować `RoleManager` do użycia z `RoleStore` klasy.
1. Zaktualizuj parametry połączenia i poświadczenia do konfiguracji aplikacji.

Przykład:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>Odwołania

* [Niestandardowi dostawcy magazynu dla produktu ASP.NET Identity 4.x](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
* [Tożsamości platformy ASP.NET Core](https://github.com/aspnet/identity) &ndash; to repozytorium zawiera linki do społeczności utrzymywane dostawców magazynu.
