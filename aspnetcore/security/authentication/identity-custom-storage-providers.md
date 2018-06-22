---
title: Dostawcy magazynu niestandardowego dla ASP.NET Core Identity
author: ardalis
description: Dowiedz się, jak skonfigurować magazyn na niestandardowych dostawców dla ASP.NET Core Identity.
ms.author: riande
ms.date: 05/24/2017
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 11c49d630c922b0aa91678277e9553bf0c25134d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278430"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Dostawcy magazynu niestandardowego dla ASP.NET Core Identity

Przez [Steve Smith](https://ardalis.com/)

Tożsamość platformy ASP.NET Core to rozszerzalny system, dzięki czemu można utworzyć dostawcy magazynu niestandardowego i podłącz go do aplikacji. W tym temacie opisano sposób tworzenia dostawcy magazynu dostosowanych do ASP.NET Core Identity. Obejmuje z ważnymi pojęciami dotyczącymi tworzenia własnego dostawcę magazynu, ale nie jest przewodnik krok po kroku.

[Wyświetlanie lub pobieranie próbki z usługi GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Wprowadzenie

Domyślnie system ASP.NET Core Identity przechowuje informacje o użytkownikach w bazie danych programu SQL Server przy użyciu programu Entity Framework Core. Wiele aplikacji to rozwiązanie działa dobrze. Można jednak używać mechanizmu stanu trwałego różnych lub schemat danych. Na przykład:

* Możesz użyć [Azure Table Storage](https://docs.microsoft.com/azure/storage/) lub innym magazynie danych.
* Tabele bazy danych ma inną strukturę. 
* Możesz skorzystać z różnymi danymi dostępu podejścia, takie jak [Dapper](https://github.com/StackExchange/Dapper). 

W każdym z tych przypadków możesz zapisać dostosowane dostawcy dla Twojego mechanizmu magazynowania i podłącz tego dostawcy do aplikacji.

Tożsamość platformy ASP.NET Core znajduje się w szablonach projektu w programie Visual Studio z opcją "Indywidualnych kont użytkowników".

Korzystając z interfejsu wiersza polecenia platformy .NET Core, Dodaj `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>Architektura ASP.NET Core Identity

ASP.NET Core Identity składa się z klasy o nazwie menedżerów i magazynów. *Menedżerowie* są klasy wysokiego poziomu, które używa Deweloper aplikacji do wykonywania operacji, takich jak tworzenie tożsamości użytkownika. *Magazyny* są klasy niższego poziomu, które określają sposób jednostek, takich jak użytkownicy i role, są zachowywane. Postępuj zgodnie z magazynów [wzorca repozytorium](http://deviq.com/repository-pattern/) i są ściśle powiązane z mechanizmu stanu trwałego. Menedżerowie są całkowicie niezależna od magazynów, co oznacza, że można zastąpić mechanizmu stanu trwałego bez zmieniania kodu aplikacji (z wyjątkiem konfiguracji).

Na poniższym diagramie przedstawiono, jak aplikacji sieci web współdziała z menedżerów, gdy magazynów interakcyjnie Warstwa dostępu do danych.

![Aplikacje platformy ASP.NET Core współdziałają z menedżerów (na przykład "Interfejs UserManager", "RoleManager"). Menedżerowie pracy z magazynami (na przykład "magazynie UserStore"), które komunikują się ze źródłem danych przy użyciu biblioteki, takich jak Entity Framework Core.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Aby utworzyć dostawcę magazynu niestandardowego, należy utworzyć źródła danych, Warstwa dostępu do danych i klasy magazynu, które interakcji z tą warstwą dostępu do danych (zielony i szare pola na powyższym diagramie). Nie trzeba dostosować kierowników lub swój kod aplikacji, który wchodzi w interakcję z nimi (powyżej pola niebieski).

Podczas tworzenia nowego wystąpienia klasy `UserManager` lub `RoleManager` udostępniają typ klasy użytkownika i przekazać wystąpienia klasy magazynu jako argument. Takie podejście umożliwia Podłącz klas dostosowanych do platformy ASP.NET Core. 

[Ponownie skonfigurować aplikację do używania nowego dostawcę magazynu](#reconfigure-app-to-use-new-storage-provider) pokazano, jak utworzyć wystąpienia `UserManager` i `RoleManager` z dostosowanych magazynu.

## <a name="aspnet-core-identity-stores-data-types"></a>Typy danych są przechowywane tożsamości platformy ASP.NET Core

[ASP.NET Core Identity](https://github.com/aspnet/identity) typy danych są szczegółowo opisane w poniższych sekcjach:

### <a name="users"></a>Użytkownicy

Zarejestrowani użytkownicy witryny sieci web. [IdentityUser](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) typu może być rozszerzony lub jako przykład wykorzystano typu niestandardowego. Nie trzeba dziedziczyć po typie konkretnej implementacji własne rozwiązania magazynu tożsamości niestandardowej.

### <a name="user-claims"></a>Oświadczenia użytkowników

Zestaw instrukcji (lub [oświadczeń](/dotnet/api/system.security.claims.claim)) dotyczące użytkownika, które reprezentują tożsamość użytkownika. Można włączyć większa wyrażenia tożsamość użytkownika nie można osiągnąć za pomocą ról.

### <a name="user-logins"></a>Identyfikatory logowania użytkownika

Informacje o dostawcy uwierzytelniania zewnętrznego (takiej jak Facebook lub kontem Microsoft) używane podczas logowania użytkownika. [Przykład](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Role

Grup autoryzacji dla witryny. Zawiera nazwę roli identyfikator i roli (na przykład "Admin" lub "Pracownika"). [Przykład](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>Warstwa dostępu do danych

W tym temacie założono, że znasz mechanizm trwałości, który ma być oraz sposobu tworzenia jednostek dla tego mechanizmu. W tym temacie nie zawierają szczegółowe informacje o sposobie tworzenia repozytoria lub klas dostępu do danych; Podczas pracy z ASP.NET Core Identity zapewnia sugestie dotyczące decyzji projektowych.

Masz dużą swobodę podczas projektowania dla dostawcy magazynu dostosowanych Warstwa dostępu do danych. Należy utworzyć mechanizmy trwałości dla funkcji, które mają być używane w aplikacji. Na przykład jeśli nie używasz role w aplikacji, nie trzeba utworzyć magazynu dla ról lub skojarzenia roli użytkownika. Używanych technologii i istniejącej infrastruktury może wymagać struktura, która różni się bardzo od Domyślna implementacja ASP.NET Core Identity. W Twojej Warstwa dostępu do danych można zapewnić logikę do pracy ze strukturą implementacji magazynu.

Warstwa dostępu do danych zawiera logikę do zapisywania danych z ASP.NET Core Identity ze źródłem danych. Warstwa dostępu do danych dla dostawcy magazynu dostosowanych może obejmować następujące klasy do przechowywania informacji o użytkownika i roli.

### <a name="context-class"></a>Context — Klasa

Hermetyzuje informacje, aby połączyć się z mechanizmu stanu trwałego i wykonywanie zapytań. Kilka klas danych wymagają wystąpienia tej klasy, zazwyczaj są realizowane za pośrednictwem iniekcji zależności. [Przykład](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Magazyn użytkownika

Przechowuje i pobiera informacje o użytkowniku (np. hash nazwę i hasło użytkownika). [Przykład](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Rola magazynów

Przechowuje i pobiera informacje o rolach (takie jak nazwa roli). [Przykład](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Magazyn UserClaims

Przechowuje i pobiera informacje o użytkownika (na przykład oświadczenia typu i wartości). [Przykład](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Magazyn UserLogins

Przechowuje i pobiera dane logowania użytkownika (na przykład zewnętrznego dostawcę uwierzytelniania). [Przykład](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>UserRole magazynu

Przechowuje i pobiera role, które są przypisane do użytkowników, którzy. [Przykład](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

**Porada:** tylko zaimplementować klasy mają być używane w aplikacji.

W klasach dostępu do danych należy podać kod do wykonania operacji danych dla Twojego mechanizmu stanu trwałego. Na przykład w ramach dostawcy niestandardowego, może mieć następujący kod, aby utworzyć nowego użytkownika w *przechowywania* klasy:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

Logika implementacji tworzenia użytkownika znajduje się w `_usersTable.CreateAsync` metody, pokazano poniżej.

## <a name="customize-the-user-class"></a>Dostosowywanie klasa użytkownika

Podczas implementowania dostawcy magazynu, należy utworzyć klasę użytkownika, który jest odpowiednikiem [ `IdentityUser` klasy](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).

Co najmniej musi zawierać własnej klasy user `Id` i `UserName` właściwości.

`IdentityUser` Klasa definiuje właściwości który `UserManager` wywołań podczas wykonywania żądanej operacji. Domyślny typ `Id` właściwość jest ciągiem, ale może dziedziczyć `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` i określ innego typu. Platformę oczekuje Wdrażanie magazynu do obsługi konwersje typów danych.

## <a name="customize-the-user-store"></a>Dostosowywanie magazynu użytkowników

Utwórz `UserStore` klasy, która udostępnia metody dla wszystkich operacji danych użytkownika. Ta klasa jest odpowiednikiem [magazynie UserStore<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) klasy. W Twojej `UserStore` klasy, implementować `IUserStore<TUser>` i opcjonalnie interfejsów wymagany. Możesz wybrać które opcjonalne interfejsy do implementacji, oparta na funkcjach dostępnych w aplikacji.

### <a name="optional-interfaces"></a>Opcjonalne interfejsów

- IUserRoleStore /dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1
- IUserClaimStore /dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1
- IUserPasswordStore /dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1
- Elementu IUserSecurityStampStore <!-- make these all links and remove / -->
- IUserEmailStore
- IPhoneNumberStore
- IQueryableUserStore
- IUserLoginStore
- IUserTwoFactorStore
- IUserLockoutStore

Opcjonalne interfejsy dziedziczyć `IUserStore`. Można wyświetlić użytkowników częściowo zaimplementowany próbki przechowywania [tutaj](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

W ramach `UserStore` klasy, użyj klasy dostępu do danych, które zostały utworzone w celu wykonania operacji. Te są przekazywane za pomocą iniekcji zależności. Na przykład w programie SQL Server z implementacją Dapper `UserStore` klasa ma `CreateAsync` metodę, która używa wystąpienia `DapperUsersTable` Aby wstawić nowy rekord:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfejsy do implementacji w przypadku dostosowywania magazynu użytkowników

- **Elementy IUserStore**  
 [Elementy IUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) interfejs jest tylko interfejsem musi implementować w magazynie użytkownika. Definiuje metody tworzenia, aktualizowania, usuwania i pobierania użytkowników.
- **IUserClaimStore**  
 [IUserClaimStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interfejs definiuje metody zaimplementowaniem włączania oświadczeń użytkownika. Zawiera metody dodawania, usuwania i pobierania oświadczeń użytkowników.
- **IUserLoginStore**  
 [IUserLoginStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) definiuje metody wdrożenia można włączyć dostawcy uwierzytelniania zewnętrznego. Zawiera metody dodawania, usuwania i pobierania logowania użytkownika i metodę pobierania na podstawie informacji logowania użytkownika.
- **IUserRoleStore**  
 [IUserRoleStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) interfejs definiuje metody wdrożenia do mapowania użytkownika do roli. Zawiera metody do dodawania, usuwania i pobierania ról użytkownika i metodę sprawdzania, czy użytkownik jest przypisany do roli.
- **IUserPasswordStore**  
 [IUserPasswordStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interfejs definiuje metody wdrożenia do utrwalenia haseł mieszanych. Zawiera metody służące do pobierania i ustawiania skrótem hasła i metody, która wskazuje, czy użytkownik ma ustawione hasło.
- **IUserSecurityStampStore**  
 [Elementu IUserSecurityStampStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interfejs definiuje metody wdrożenia do użycia sygnaturę bezpieczeństwa dla wskazującą, czy informacje o koncie użytkownika został zmieniony. Ta sygnatura jest aktualizowany, gdy użytkownik zmieni hasło, lub dodaje lub usuwa logowania. Zawiera metody służące do pobierania i ustawiania sygnatury bezpieczeństwa.
- **IUserTwoFactorStore**  
 [IUserTwoFactorStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interfejs definiuje metody wdrożenia do obsługi uwierzytelniania dwuskładnikowego. Zawiera metody służące do pobierania i ustawiania czy uwierzytelnianie dwuskładnikowe jest włączone dla użytkownika.
- **IUserPhoneNumberStore**  
 [IUserPhoneNumberStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interfejs definiuje metody zaimplementowaniem do przechowywania numeru telefonu użytkownika. Zawiera metody służące do pobierania i ustawiania numeru telefonu i określa, czy numer telefonu został potwierdzony.
- **IUserEmailStore**  
 [IUserEmailStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) interfejs definiuje metody implementacji przechowywania adresów e-mail użytkownika. Zawiera metody służące do pobierania i ustawiania adres e-mail i czy adres e-mail został potwierdzony.
- **IUserLockoutStore**  
 [IUserLockoutStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interfejs definiuje metody implementacji przechowywania informacji na temat blokowania konta. Zawiera metody do śledzenia nieudanych prób dostępu i blokady.
- **IQueryableUserStore**  
 [IQueryableUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interfejs definiuje elementy zaimplementować zapewnienie magazynu użytkowników z obsługą zapytań.

Można zaimplementować interfejsów, które są wymagane w aplikacji. Na przykład:

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

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin i IdentityUserRole

`Microsoft.AspNet.Identity.EntityFramework` Przestrzeń nazw zawiera implementacje [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), i [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) klasy. Jeśli używasz tych funkcji, można tworzyć własne wersje tych klas i zdefiniuj właściwości do aplikacji. Jednak czasami jest bardziej wydajne nie ładuje te jednostki do pamięci podczas wykonywania podstawowych operacji (na przykład dodawania lub usuwania oświadczenia użytkownika). Zamiast tego klasy magazynu wewnętrznej bazy danych mogą wykonywać te operacje bezpośrednio w źródle danych. Na przykład `UserStore.GetClaimsAsync` można wywołać metody `userClaimTable.FindByUserId(user.Id)` metodę można wykonać zapytania w tabeli bezpośrednio i powrócić do listy oświadczeń.

## <a name="customize-the-role-class"></a>Dostosowywanie klasy roli

Podczas implementowania dostawcy magazynu ról, można utworzyć typu niestandardowej roli zabezpieczeń. Nie musi on implementować konkretnego interfejsu, ale musi on mieć `Id` i zwykle mają `Name` właściwości.

Poniżej przedstawiono przykład klasy roli:

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Dostosowywanie magazynu ról

Można utworzyć `RoleStore` klasy, która udostępnia metody dla wszystkich operacji danych na rolach. Ta klasa jest odpowiednikiem [elemencie RoleStore<TRole> ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) klasy. W `RoleStore` klasa implementuje `IRoleStore<TRole>` i opcjonalnie `IQueryableRoleStore<TRole>` interfejsu.

- **IRoleStore&lt;TRole&gt;**  
 [Interfejs IRoleStore](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) interfejs definiuje metody służące do implementacji klasy magazynu roli. Zawiera metody do tworzenia, aktualizowania, usuwania i pobierania ról.
- **RoleStore&lt;TRole&gt;**  
 Aby dostosować `RoleStore`, Utwórz klasę, która implementuje `IRoleStore` interfejsu. 

## <a name="reconfigure-app-to-use-new-storage-provider"></a>Ponownie skonfigurować aplikację do używania nowego dostawcę magazynu

Po wdrożeniu dostawcy magazynu, możesz skonfigurować aplikację, aby go użyć. Jeśli aplikacja używany domyślny dostawca, zastąp go niestandardowego dostawcy.

1. Usuń `Microsoft.AspNetCore.EntityFramework.Identity` pakietu NuGet.
1. Jeśli dostawca magazynu znajduje się w oddzielnych projektu lub pakietu, Dodaj odwołanie do niej.
1. Zamień wszystkie odwołania do `Microsoft.AspNetCore.EntityFramework.Identity` przy użyciu instrukcji dla przestrzeni nazw dostawcy magazynu.
1. W `ConfigureServices` metody, zmień `AddIdentity` metoda do użycia z niestandardowych typów. Możesz utworzyć własne metody rozszerzenia dla tego celu. Zobacz [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) przykład.
1. Jeśli korzystasz z ról, należy zaktualizować `RoleManager` do użycia z `RoleStore` klasy.
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

- [Dostawcy magazynu niestandardowego dla tożsamości ASP.NET](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Core Identity](https://github.com/aspnet/identity) — to repozytorium zawiera łącza do społeczności utrzymywane dostawców magazynu.
