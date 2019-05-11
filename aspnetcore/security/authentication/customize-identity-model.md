---
title: Dostosowywanie modelu tożsamości w programie ASP.NET Core
author: ajcvickers
description: W tym artykule opisano, jak dostosować bazowy model danych Entity Framework Core dla tożsamości platformy ASP.NET Core.
ms.author: avickers
ms.date: 04/24/2019
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 53ce77e20722f3ba3282ff4455a0b70d30e635b0
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65536023"
---
# <a name="identity-model-customization-in-aspnet-core"></a>Dostosowywanie modelu tożsamości w programie ASP.NET Core

Przez [Arthur Vickers](https://github.com/ajcvickers)

Tożsamość platformy ASP.NET Core zapewnia platformę do zarządzania i przechowywania kont użytkowników w aplikacji platformy ASP.NET Core. Tożsamość zostanie dodany do projektu po **indywidualne konta użytkowników** jest wybrany jako mechanizm uwierzytelniania. Domyślnie, Identity korzysta z Entity Framework (EF) podstawowego modelu danych. W tym artykule opisano sposób dostosowywania modelu tożsamości.

## <a name="identity-and-ef-core-migrations"></a>Tożsamość i migracji programu EF Core

Przed zbadaniem modelu, warto zrozumieć, jak działa tożsamości za pomocą [migracji programu EF Core](/ef/core/managing-schemas/migrations/) do tworzenia i aktualizowania bazy danych. Na najwyższym poziomie proces jest:

1. Zdefiniuj lub zaktualizować [modelu danych w kodzie](/ef/core/modeling/).
1. Dodaj migrację do tłumaczenia w tym modelu na zmiany, które mogą być stosowane do bazy danych.
1. Sprawdź, czy migracja poprawnie reprezentuje zamiaru.
1. Zastosowanie migracji można zaktualizować bazy danych, aby był zsynchronizowany z modelem.
1. Powtórz kroki od 1 do 4, aby dodatkowo dostosować model i synchronizację bazy danych.

Dodaj i stosowanie migracje, użyj jednej z następujących metod:

* **Konsola Menedżera pakietów** okna (PMC), jeśli za pomocą programu Visual Studio. Aby uzyskać więcej informacji, zobacz [narzędzia EF Core PMC](/ef/core/miscellaneous/cli/powershell).
* .NET Core interfejsu wiersza polecenia, jeśli przy użyciu wiersza polecenia. Aby uzyskać więcej informacji, zobacz [narzędzi wiersza polecenia programu EF Core .NET](/ef/core/miscellaneous/cli/dotnet).
* Klikając **zastosować migracje** przycisk na stronę błędu, gdy aplikacja jest uruchomiona.

Platforma ASP.NET Core ma program obsługi strony błędów w czasie projektowania. Program obsługi można zastosować migracji, gdy aplikacja jest uruchomiona. Aplikacje produkcyjne zazwyczaj Generuj skrypty SQL z migracje i wdrażania zmian w bazie danych w ramach kontrolowanego aplikacji i wdrażania bazy danych.

Po utworzeniu nowej aplikacji przy użyciu tożsamości kroki 1 i 2 powyżej została już ukończona. Oznacza to, że istnieje już w modelu danych początkowych i początkowej migracji został dodany do projektu. Początkowej migracji nadal musi zostać zastosowana do bazy danych. Można zastosować początkowej migracji przy użyciu jednej z następujących metod:

* Uruchom `Update-Database` w konsoli zarządzania Pakietami.
* Uruchom `dotnet ef database update` w powłoce poleceń.
* Kliknij przycisk **zastosować migracje** przycisk na stronę błędu, gdy aplikacja jest uruchomiona.

Powtórz te czynności, jak zmiany zostały wprowadzone do modelu.

## <a name="the-identity-model"></a>Modelu tożsamości

### <a name="entity-types"></a>Typy jednostek

Model tożsamości składa się z następujących typów jednostki.

|Typ jednostki|Opis                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |Reprezentuje użytkownika.                                         |
|`Role`     |Reprezentuje rolę.                                           |
|`UserClaim`|Reprezentuje oświadczenie, które posiada użytkownik.                    |
|`UserToken`|Reprezentuje token uwierzytelniania dla użytkownika.               |
|`UserLogin`|Kojarzy użytkownika z logowaniem.                              |
|`RoleClaim`|Reprezentuje oświadczenie, które otrzymuje dla wszystkich użytkowników w ramach roli.|
|`UserRole` |Jednostka sprzężenia, która kojarzy użytkownikami i rolami.               |

### <a name="entity-type-relationships"></a>Relacje typów jednostek

[Typów jednostek](#entity-types) są powiązane ze sobą w następujący sposób:

* Każdy `User` może mieć wiele `UserClaims`.
* Każdy `User` może mieć wiele `UserLogins`.
* Każdy `User` może mieć wiele `UserTokens`.
* Każdy `Role` może mieć wiele skojarzonych `RoleClaims`.
* Każdy `User` może mieć wiele skojarzonych `Roles`, a każdy `Role` może być skojarzony z wieloma `Users`. Jest to relacja wiele do wielu, wymagającego sprzężenie tabeli w bazie danych. Tabela sprzężenia jest reprezentowany przez `UserRole` jednostki.

### <a name="default-model-configuration"></a>Domyślna konfiguracja modelu

Tożsamość definiuje wiele *klasy kontekst* , dziedziczą z <xref:Microsoft.EntityFrameworkCore.DbContext> do konfigurowania i używania tego modelu. Ta konfiguracja jest implementowana przy użyciu [EF Core kodu pierwszy Fluent API](/ef/core/modeling/) w <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> metody z klasy kontekstu. Domyślna konfiguracja polega:

```csharp
builder.Entity<TUser>(b =>
{
    // Primary key
    b.HasKey(u => u.Id);

    // Indexes for "normalized" username and email, to allow efficient lookups
    b.HasIndex(u => u.NormalizedUserName).HasName("UserNameIndex").IsUnique();
    b.HasIndex(u => u.NormalizedEmail).HasName("EmailIndex");

    // Maps to the AspNetUsers table
    b.ToTable("AspNetUsers");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.UserName).HasMaxLength(256);
    b.Property(u => u.NormalizedUserName).HasMaxLength(256);
    b.Property(u => u.Email).HasMaxLength(256);
    b.Property(u => u.NormalizedEmail).HasMaxLength(256);

    // The relationships between User and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each User can have many UserClaims
    b.HasMany<TUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

    // Each User can have many UserLogins
    b.HasMany<TUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

    // Each User can have many UserTokens
    b.HasMany<TUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

    // Each User can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
});

builder.Entity<TUserClaim>(b =>
{
    // Primary key
    b.HasKey(uc => uc.Id);

    // Maps to the AspNetUserClaims table
    b.ToTable("AspNetUserClaims");
});

builder.Entity<TUserLogin>(b =>
{
    // Composite primary key consisting of the LoginProvider and the key to use
    // with that provider
    b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
    b.Property(t => t.Name).HasMaxLength(maxKeyLength);

    // Maps to the AspNetUserTokens table
    b.ToTable("AspNetUserTokens");
});

builder.Entity<TRole>(b =>
{
    // Primary key
    b.HasKey(r => r.Id);

    // Index for "normalized" role name to allow efficient lookups
    b.HasIndex(r => r.NormalizedName).HasName("RoleNameIndex").IsUnique();

    // Maps to the AspNetRoles table
    b.ToTable("AspNetRoles");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.Name).HasMaxLength(256);
    b.Property(u => u.NormalizedName).HasMaxLength(256);

    // The relationships between Role and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each Role can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

    // Each Role can have many associated RoleClaims
    b.HasMany<TRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
});

builder.Entity<TRoleClaim>(b =>
{
    // Primary key
    b.HasKey(rc => rc.Id);

    // Maps to the AspNetRoleClaims table
    b.ToTable("AspNetRoleClaims");
});

builder.Entity<TUserRole>(b =>
{
    // Primary key
    b.HasKey(r => new { r.UserId, r.RoleId });

    // Maps to the AspNetUserRoles table
    b.ToTable("AspNetUserRoles");
});
```

### <a name="model-generic-types"></a>Typy ogólne w modelu

Tożsamość definiuje domyślny [środowiska uruchomieniowego języka wspólnego](/dotnet/standard/glossary#clr) typów (CLR) dla każdego z typów jednostek wymienionych powyżej. Te typy są poprzedzone z *tożsamości*:

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

Zamiast bezpośrednio przy użyciu tych typów, typy może służyć jako klay bazowe dla typów własnych aplikacji. `DbContext` Klas zdefiniowanych przez tożsamości są ogólne, w taki sposób, że różne typy CLR może służyć do co najmniej jeden z typów jednostek w modelu. Te typy rodzajowe również zezwolić `User` typu klucza podstawowego (PK) dane mają być zmienione.

W przypadku używania tożsamości z obsługą dla ról, <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> klasa powinna być używana. Na przykład:

```csharp
// Uses all the built-in Identity types
// Uses `string` as the key type
public class IdentityDbContext
    : IdentityDbContext<IdentityUser, IdentityRole, string>
{
}

// Uses the built-in Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityDbContext<TUser>
    : IdentityDbContext<TUser, IdentityRole, string>
        where TUser : IdentityUser
{
}

// Uses the built-in Identity types except with custom User and Role types
// The key type is defined by TKey
public class IdentityDbContext<TUser, TRole, TKey> : IdentityDbContext<
    TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>,
    IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<
    TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
    : IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
         where TUser : IdentityUser<TKey>
         where TRole : IdentityRole<TKey>
         where TKey : IEquatable<TKey>
         where TUserClaim : IdentityUserClaim<TKey>
         where TUserRole : IdentityUserRole<TKey>
         where TUserLogin : IdentityUserLogin<TKey>
         where TRoleClaim : IdentityRoleClaim<TKey>
         where TUserToken : IdentityUserToken<TKey>
```

Istnieje również możliwość użycia tożsamości bez role (tylko oświadczenia), w którym to przypadku <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> należy używać klasy:

```csharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey> : IdentityUserContext<
    TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>,
    IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<
    TUser, TKey, TUserClaim, TUserLogin, TUserToken> : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customize-the-model"></a>Dostosuj model

Punkt początkowy dla dostosowania modelu ma pochodzić od typu odpowiedniego kontekstu. Zobacz [modelu typów ogólnych](#model-generic-types) sekcji. Ten typ kontekstu zwyczajowo jest nazywany `ApplicationDbContext` i zostanie utworzony za pomocą szablonów platformy ASP.NET Core.

Kontekst jest używany do konfigurowania modelu na dwa sposoby:

* Podanie jednostek i typy kluczy dla parametrów typu ogólnego.
* Zastępowanie `OnModelCreating` do modyfikowania mapowanie tych typów.

Podczas zastępowania `OnModelCreating`, `base.OnModelCreating` powinny zostać wywołane najpierw; nadrzędnych konfiguracji powinna być wywoływana dalej. EF Core ogólnie ma zasady ostatni jednej usługi wins do konfiguracji. Na przykład jeśli `ToTable` dla typu jednostki wywoływana jest metoda najpierw o nazwie jedna tabela a następnie ponownie później z inną nazwę tabeli, nazwę tabeli drugie wywołanie jest używane.

### <a name="custom-user-data"></a>Danych niestandardowych użytkownika

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

[Danych niestandardowych użytkownika](xref:security/authentication/add-user-data) jest obsługiwana przez dziedziczenie z `IdentityUser`. Zwyczajowy nazwę tego typu jest `ApplicationUser`:

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Użyj `ApplicationUser` typu jako argumentu ogólnego kontekstu:

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
    }
}
```

Nie ma potrzeby zastąpić `OnModelCreating` w `ApplicationDbContext` klasy. EF Core mapuje `CustomTag` właściwości przez Konwencję. Jednak bazy danych musi zostać zaktualizowany, aby utworzyć nowy `CustomTag` kolumny. Aby utworzyć kolumnę, Dodaj migrację, a następnie zaktualizować bazy danych, zgodnie z opisem w [tożsamości i programem EF Core migracje](#identity-and-ef-core-migrations).

Aktualizacja *Pages/Shared/_LoginPartial.cshtml* i Zastąp `IdentityUser` z `ApplicationUser`:

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

Aktualizacja *Areas/Identity/IdentityHostingStartup.cs* lub `Startup.ConfigureServices` i Zastąp `IdentityUser` z `ApplicationUser`.

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

W programie ASP.NET Core 2.1 lub nowszej tożsamość jest dostarczany jako biblioteki klas Razor. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/scaffold-identity>. W związku z tym, poprzedni kod wymaga wywołania <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Jeśli Generator szkieletu tożsamości zostało użyte do dodania tożsamości plików do projektu, Usuń wywołanie funkcji `AddDefaultUI`. Aby uzyskać więcej informacji, zobacz:

* [Tworzenie szkieletu tożsamości](xref:security/authentication/scaffold-identity)
* [Dodawanie, pobieranie i usuwanie danych niestandardowych użytkownika w tożsamości](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a>Zmień typ klucza podstawowego

Zmiana typu danych kolumny klucza podstawowego, po utworzeniu bazy danych jest problematyczne na wiele systemów baz danych. Zmiana klucza produktu zazwyczaj polega na porzucenie i ponowne utworzenie tabeli. W związku z tym typy kluczy powinny być określone w początkowej migracji, po utworzeniu bazy danych.

Wykonaj następujące kroki, aby zmienić typ klucza produktu:

1. Jeśli baza danych została utworzona przed zmianą klucza produktu, uruchom `Drop-Database` (PMC) lub `dotnet ef database drop` (.NET Core interfejsu wiersza polecenia) go usunąć.
2. Po potwierdzeniu operacji usuwania bazy danych, należy usunąć początkowej migracji za pomocą `Remove-Migration` (PMC) lub `dotnet ef migrations remove` (.NET Core interfejsu wiersza polecenia).
3. Aktualizacja `ApplicationDbContext` pochodzi od klasy <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>. Określ nowy typ klucza `TKey`. Na przykład, aby użyć `Guid` typ klucza:

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    ::: moniker range=">= aspnetcore-2.0"

    W poprzednim kodzie klas ogólnych <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> i <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> muszą określać używać nowego typu klucza.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    W poprzednim kodzie klas ogólnych <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> i <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> muszą określać używać nowego typu klucza.

    ::: moniker-end

    `Startup.ConfigureServices` należy zaktualizować tak, aby za pomocą ogólnego konta użytkownika:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<IdentityUser<Guid>>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

4. Jeśli niestandardowa `ApplicationUser` klasa jest używana, aktualizować klasę, aby dziedziczyć `IdentityUser`. Na przykład:

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    Aktualizacja `ApplicationDbContext` można odwoływać się do niestandardowego `ApplicationUser` klasy:

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    Rejestrowanie klasy kontekstu bazy danych niestandardowych podczas dodawania usługi tożsamości w `Startup.ConfigureServices`:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    Typ danych klucza podstawowego jest wnioskowany, analizując <xref:Microsoft.EntityFrameworkCore.DbContext> obiektu.

    W programie ASP.NET Core 2.1 lub nowszej tożsamość jest dostarczany jako biblioteki klas Razor. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/scaffold-identity>. W związku z tym, poprzedni kod wymaga wywołania <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Jeśli Generator szkieletu tożsamości zostało użyte do dodania tożsamości plików do projektu, Usuń wywołanie funkcji `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    Typ danych klucza podstawowego jest wnioskowany, analizując <xref:Microsoft.EntityFrameworkCore.DbContext> obiektu.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> Metoda przyjmuje `TKey` typ wskazuje typ danych klucza podstawowego.

    ::: moniker-end

5. Jeśli niestandardowa `ApplicationRole` klasa jest używana, aktualizować klasę, aby dziedziczyć `IdentityRole<TKey>`. Na przykład:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    Aktualizacja `ApplicationDbContext` można odwoływać się do niestandardowego `ApplicationRole` klasy. Na przykład następującej klasy odwołuje się do niestandardowego `ApplicationUser` i niestandardowego `ApplicationRole`:

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Rejestrowanie klasy kontekstu bazy danych niestandardowych podczas dodawania usługi tożsamości w `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    Typ danych klucza podstawowego jest wnioskowany, analizując <xref:Microsoft.EntityFrameworkCore.DbContext> obiektu.

    W programie ASP.NET Core 2.1 lub nowszej tożsamość jest dostarczany jako biblioteki klas Razor. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/scaffold-identity>. W związku z tym, poprzedni kod wymaga wywołania <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Jeśli Generator szkieletu tożsamości zostało użyte do dodania tożsamości plików do projektu, Usuń wywołanie funkcji `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Rejestrowanie klasy kontekstu bazy danych niestandardowych podczas dodawania usługi tożsamości w `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    Typ danych klucza podstawowego jest wnioskowany, analizując <xref:Microsoft.EntityFrameworkCore.DbContext> obiektu.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Rejestrowanie klasy kontekstu bazy danych niestandardowych podczas dodawania usługi tożsamości w `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> Metoda przyjmuje `TKey` typ wskazuje typ danych klucza podstawowego.

    ::: moniker-end

### <a name="add-navigation-properties"></a>Dodawanie właściwości nawigacji

Zmiana konfiguracji modelu dla relacji może być trudniejsze niż wprowadzeniem innych zmian. Należy uważać, aby zastąpić istniejące relacje, zamiast tworzyć nowy, dodatkowe relacje. W szczególności zmienione relacja musi określać samą właściwość klucza obcego (klucz OBCY), jako istniejącą relację. Na przykład relacji między `Users` i `UserClaims` jest domyślnie określone w następujący sposób:

```csharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

Klucza Obcego dla tej relacji jest określony jako `UserClaim.UserId` właściwości. `HasMany` i `WithOne` są wywoływane bez argumentów, aby utworzyć relacje bez właściwości nawigacji.

Właściwość nawigacji, aby dodać `ApplicationUser` umożliwiająca skojarzone `UserClaims` przywoływanie od użytkownika:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

`TKey` Dla `IdentityUserClaim<TKey>` jest typ określony dla klucza podstawowego użytkowników. W tym przypadku `TKey` jest `string` ponieważ są używane wartości domyślne. Ma ona **nie** typu klucz podstawowy dla `UserClaim` typu jednostki.

Teraz, gdy istnieje właściwość nawigacji, muszą być skonfigurowane w `OnModelCreating`:

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();
        });
    }
}
```

Należy zauważyć, że relacja jest skonfigurowany, dokładnie tak, jak został wcześniej, tylko z określonych w wywołaniu do właściwości nawigacji `HasMany`.

Właściwości nawigacji istnieją tylko w modelu platformy EF, a nie bazy danych. Ponieważ klucza Obcego relacji nie została zmieniona, tego rodzaju zmiany modelu nie wymaga bazy danych do zaktualizowania. Można to sprawdzić przez dodanie migracji po wprowadzeniu zmian. `Up` i `Down` metody są puste.

### <a name="add-all-user-navigation-properties"></a>Dodaj wszystkich właściwości nawigacji użytkownika.

Przy użyciu powyższej sekcji jako wskazówkę, poniższy przykład umożliwia skonfigurowanie właściwości nawigacji jednokierunkowe dla wszystkich relacji na użytkownika:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne()
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });
    }
}
```

### <a name="add-user-and-role-navigation-properties"></a>Dodawanie użytkownika i roli właściwości nawigacji

Przy użyciu powyższej sekcji jako wskazówkę, poniższy przykład umożliwia skonfigurowanie właściwości nawigacji dla wszystkich relacji na użytkownika i roli:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        IdentityUserClaim<string>, ApplicationUserRole, IdentityUserLogin<string>,
        IdentityRoleClaim<string>, IdentityUserToken<string>>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();
        });

    }
}
```

Uwagi:

* W tym przykładzie zawiera również `UserRole` dołączyć obiektu, który jest wymagany do nawigacji w relacji wiele do wielu użytkowników do ról.
* Pamiętaj, aby zmienić typy właściwości nawigacji, aby uwzględniała `ApplicationXxx` typy są obecnie używane zamiast `IdentityXxx` typów.
* Pamiętaj, aby używać `ApplicationXxx` w ogólnej `ApplicationContext` definicji.

### <a name="add-all-navigation-properties"></a>Dodaj wszystkie właściwości nawigacji

Przy użyciu powyższej sekcji jako wskazówkę, poniższy przykład umożliwia skonfigurowanie właściwości nawigacji dla wszystkich relacji na wszystkich typów jednostek:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<ApplicationUserClaim> Claims { get; set; }
    public virtual ICollection<ApplicationUserLogin> Logins { get; set; }
    public virtual ICollection<ApplicationUserToken> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
    public virtual ICollection<ApplicationRoleClaim> RoleClaims { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserClaim : IdentityUserClaim<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationUserLogin : IdentityUserLogin<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationRoleClaim : IdentityRoleClaim<string>
{
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserToken : IdentityUserToken<string>
{
    public virtual ApplicationUser User { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin,
        ApplicationRoleClaim, ApplicationUserToken>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne(e => e.User)
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne(e => e.User)
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne(e => e.User)
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();

            // Each Role can have many associated RoleClaims
            b.HasMany(e => e.RoleClaims)
                .WithOne(e => e.Role)
                .HasForeignKey(rc => rc.RoleId)
                .IsRequired();
        });
    }
}
```

### <a name="use-composite-keys"></a>Użyj kluczy złożonych

Poprzednich sekcji pokazano, zmieniając typ użycia klucza w modelu tożsamości. Zmiana modelu klucza tożsamości do użycia kluczy złożonych nie jest obsługiwany lub zalecane. Przy użyciu klucza złożonego za pomocą tożsamości polega na zmianę, jak tożsamość Menedżer kod współdziała z modelu. To dostosowanie wykracza poza zakres tego dokumentu.

### <a name="change-tablecolumn-names-and-facets"></a>Zmienianie nazwy tabeli z kolumnami i zestawy reguł

Aby zmienić nazwy tabel i kolumn, należy wywołać `base.OnModelCreating`. Następnie należy dodać konfigurację, aby przesłonić wartości domyślne. Na przykład, aby zmienić nazwę w tabelach tożsamości:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.ToTable("MyUsers");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.ToTable("MyUserClaims");
    });

    modelBuilder.Entity<IdentityUserLogin<string>>(b =>
    {
        b.ToTable("MyUserLogins");
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.ToTable("MyUserTokens");
    });

    modelBuilder.Entity<IdentityRole>(b =>
    {
        b.ToTable("MyRoles");
    });

    modelBuilder.Entity<IdentityRoleClaim<string>>(b =>
    {
        b.ToTable("MyRoleClaims");
    });

    modelBuilder.Entity<IdentityUserRole<string>>(b =>
    {
        b.ToTable("MyUserRoles");
    });
}
```

Te przykłady korzystają z typów tożsamości domyślnej. Jeśli przy użyciu typu aplikacji, takich jak `ApplicationUser`, konfigurowanie tego typu, zamiast domyślnego typu.

Poniższy przykład umożliwia zmianę niektórych nazw kolumn:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(e => e.Email).HasColumnName("EMail");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.Property(e => e.ClaimType).HasColumnName("CType");
        b.Property(e => e.ClaimValue).HasColumnName("CValue");
    });
}
```

Niektóre typy kolumny bazy danych można skonfigurować z niektórymi *aspektami* (na przykład maksymalna `string` dozwoloną długość). W poniższym przykładzie ustawiono maksymalną długość kolumny dla kilku `string` właściwości w modelu:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(u => u.UserName).HasMaxLength(128);
        b.Property(u => u.NormalizedUserName).HasMaxLength(128);
        b.Property(u => u.Email).HasMaxLength(128);
        b.Property(u => u.NormalizedEmail).HasMaxLength(128);
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.Property(t => t.LoginProvider).HasMaxLength(128);
        b.Property(t => t.Name).HasMaxLength(128);
    });
}
```

### <a name="map-to-a-different-schema"></a>Mapowanie do innego schematu

Schematy może zachowywać się inaczej w dostawcy baz danych. Dla programu SQL Server, wartość domyślna jest utworzenie wszystkich tabel w *dbo* schematu. Tabele można tworzyć w różnych schematów. Na przykład:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a>Ładowanie z opóźnieniem

W tej sekcji zostanie dodana jego obsługa dla proxy ładowanych z opóźnieniem w modelu tożsamości. Ładowanie leniwy jest przydatne, ponieważ umożliwia ono pomyślne właściwości nawigacji, aby można używać bez zapewnienia ich jest załadowany.

Typy jednostek można wprowadzić odpowiednie dla powolne ładowanie na kilka sposobów, zgodnie z opisem w [dokumentacji programu EF Core](/ef/core/querying/related-data#lazy-loading). Dla uproszczenia należy użyć serwerów proxy ładowanych z opóźnieniem, co wymaga:

* Instalacja [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pakietu.
* Wywołanie <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> wewnątrz <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.
* Publiczna typów z `public virtual` właściwości nawigacji.

W poniższym przykładzie pokazano wywołanie `UseLazyLoadingProxies` w `Startup.ConfigureServices`:

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Zapoznaj się z powyższych przykładach, aby uzyskać wskazówki na temat dodawania właściwości nawigacji do typów jednostek.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authentication/scaffold-identity>

::: moniker-end
