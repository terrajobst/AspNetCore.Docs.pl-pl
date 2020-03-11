---
title: Dostosowywanie modelu tożsamości w ASP.NET Core
author: ajcvickers
description: W tym artykule opisano sposób dostosowywania bazowego modelu danych Entity Framework Core dla ASP.NET Core tożsamości.
ms.author: avickers
ms.date: 07/01/2019
uid: security/authentication/customize_identity_model
ms.openlocfilehash: f549fdff4a416b5fadcb2b1078b051bbab8e402e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656080"
---
# <a name="identity-model-customization-in-aspnet-core"></a>Dostosowywanie modelu tożsamości w ASP.NET Core

Autor [Arthur Vickers](https://github.com/ajcvickers)

ASP.NET Core Identity oferuje strukturę służącą do zarządzania kontami użytkowników w aplikacjach ASP.NET Core i ich przechowywania. Tożsamość jest dodawana do projektu w przypadku wybrania jako mechanizmu uwierzytelniania **poszczególnych kont użytkowników** . Domyślnie tożsamość wykorzystuje podstawowy model danych Entity Framework (EF). W tym artykule opisano sposób dostosowywania modelu tożsamości.

## <a name="identity-and-ef-core-migrations"></a>Tożsamość i migracje EF Core

Przed badaniem modelu warto zrozumieć, jak tożsamość współpracuje z [EF Core migracji](/ef/core/managing-schemas/migrations/) , aby utworzyć i zaktualizować bazę danych. Na najwyższego poziomu proces jest:

1. Zdefiniuj lub zaktualizuj [model danych w kodzie](/ef/core/modeling/).
1. Dodaj migrację, aby przetłumaczyć ten model na zmiany, które można zastosować do bazy danych programu.
1. Sprawdź, czy migracja jest poprawnie reprezentowana przez zamiary.
1. Zastosuj migrację, aby zaktualizować bazę danych do synchronizacji z modelem.
1. Powtórz kroki od 1 do 4, aby udoskonalić model i zachować synchronizację bazy danych.

Aby dodać i zastosować migracje, należy użyć jednej z następujących metod:

* Okno **konsoli Menedżera pakietów** (PMC), jeśli jest używany program Visual Studio. Aby uzyskać więcej informacji, zobacz [EF Core PMC Tools](/ef/core/miscellaneous/cli/powershell).
* Interfejs wiersza polecenia platformy .NET Core w przypadku używania wiersza polecenia. Aby uzyskać więcej informacji, zobacz [EF Core narzędzia wiersza polecenia platformy .NET](/ef/core/miscellaneous/cli/dotnet).
* Po uruchomieniu aplikacji kliknij przycisk **Zastosuj migracje** na stronie błędu.

ASP.NET Core ma program obsługi stron błędów czasu projektowania. Program obsługi może zastosować migracje, gdy aplikacja jest uruchomiona. Aplikacje produkcyjne zwykle generują skrypty SQL z migracji i wdrażają zmiany w bazie danych w ramach kontrolowanego wdrożenia aplikacji i bazy danych.

Po utworzeniu nowej aplikacji używającej tożsamości, kroki 1 i 2 powyżej zostały już ukończone. Oznacza to, że początkowy model danych już istnieje, a migracja początkowa została dodana do projektu. Migracja początkowa nadal musi zostać zastosowana do bazy danych programu. Migrację początkową można zastosować przy użyciu jednej z następujących metod:

* Uruchom `Update-Database` w PMC.
* Uruchom `dotnet ef database update` w powłoce poleceń.
* Po uruchomieniu aplikacji kliknij przycisk **Zastosuj migracje** na stronie błędu.

Powtórz powyższe kroki, ponieważ wprowadzono zmiany w modelu.

## <a name="the-identity-model"></a>Model tożsamości

### <a name="entity-types"></a>Typy jednostek

Model tożsamości składa się z następujących typów jednostek.

|Typ jednostki|Opis                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |Reprezentuje użytkownika.                                         |
|`Role`     |Reprezentuje rolę.                                           |
|`UserClaim`|Reprezentuje stwierdzenie, że użytkownik posiada.                    |
|`UserToken`|Reprezentuje token uwierzytelniania dla użytkownika.               |
|`UserLogin`|Kojarzy użytkownika z logowaniem.                              |
|`RoleClaim`|Reprezentuje zastrzeżenie przyznane wszystkim użytkownikom w ramach roli.|
|`UserRole` |Jednostka sprzężenia, która kojarzy użytkowników i role.               |

### <a name="entity-type-relationships"></a>Relacje typu jednostki

[Typy jednostek](#entity-types) są powiązane ze sobą w następujący sposób:

* Każdy `User` może mieć wiele `UserClaims`.
* Każdy `User` może mieć wiele `UserLogins`.
* Każdy `User` może mieć wiele `UserTokens`.
* Każda `Role` może mieć wiele skojarzonych `RoleClaims`.
* Każda `User` może mieć wiele skojarzonych `Roles`, a każda `Role` może być skojarzona z wieloma `Users`ami. Jest to relacja wiele do wielu, która wymaga tabeli sprzężenia w bazie danych. Tabela sprzężenia jest reprezentowana przez jednostkę `UserRole`.

### <a name="default-model-configuration"></a>Domyślna konfiguracja modelu

Tożsamość definiuje wiele *klas kontekstu* , które dziedziczą z [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) , aby skonfigurować model i korzystać z niego. Ta konfiguracja odbywa się przy użyciu [interfejsu API EF Core Code First Fluent](/ef/core/modeling/) w metodzie [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating) klasy Context. Domyślna konfiguracja to:

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

### <a name="model-generic-types"></a>Typy ogólne modelu

Tożsamość definiuje domyślne typy [środowiska uruchomieniowego języka wspólnego](/dotnet/standard/glossary#clr) (CLR) dla każdego z wymienionych powyżej typów jednostek. Wszystkie te typy są poprzedzone prefiksem *:*

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

Zamiast bezpośrednio używać tych typów, typy mogą służyć jako klasy bazowe dla własnych typów aplikacji. Klasy `DbContext` zdefiniowane przez tożsamość są ogólne, w taki sposób, aby można było używać różnych typów CLR dla co najmniej jednego typu jednostki w modelu. Te typy ogólne umożliwiają zmianę typu danych klucza podstawowego (PK) `User`.

W przypadku korzystania z tożsamości z obsługą ról należy użyć klasy <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>. Na przykład:

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

Istnieje również możliwość użycia tożsamości bez ról (tylko oświadczenia). w takim przypadku należy użyć klasy <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601>:

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

## <a name="customize-the-model"></a>Dostosowywanie modelu

Punktem początkowym dostosowywania modelu jest wychodzenie z odpowiedniego typu kontekstu. Zapoznaj się z sekcją [typy ogólne modelu](#model-generic-types) . Ten typ kontekstu jest zwykle nazywany `ApplicationDbContext` i jest tworzony przez szablony ASP.NET Core.

Kontekst służy do konfigurowania modelu na dwa sposoby:

* Dostarczanie typów jednostek i kluczy dla parametrów typu ogólnego.
* Zastępowanie `OnModelCreating`, aby zmodyfikować mapowanie tych typów.

Podczas zastępowania `OnModelCreating`należy najpierw wywołać `base.OnModelCreating`. nadrzędna konfiguracja powinna być wywoływana dalej. EF Core zwykle ma zasady dotyczące ostatniego skonfigurowania usługi WINS. Na przykład jeśli metoda `ToTable` dla typu jednostki jest wywoływana najpierw z jedną nazwą tabeli, a następnie ponownie później z inną nazwą tabeli, używana jest nazwa tabeli w drugim wywołaniu.

### <a name="custom-user-data"></a>Niestandardowe dane użytkownika

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

[Niestandardowe dane użytkownika](xref:security/authentication/add-user-data) są obsługiwane przez dziedziczenie z `IdentityUser`. Nazwa tego typu jest niestandardowa `ApplicationUser`:

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Użyj `ApplicationUser` typu jako argumentu ogólnego dla kontekstu:

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

Nie ma potrzeby przesłonięcia `OnModelCreating` w klasie `ApplicationDbContext`. EF Core mapuje Właściwość `CustomTag` według Konwencji. Bazę danych należy jednak zaktualizować, aby utworzyć nową kolumnę `CustomTag`. Aby utworzyć kolumnę, Dodaj migrację, a następnie zaktualizuj bazę danych zgodnie z opisem w temacie [Identity and EF Core migrations](#identity-and-ef-core-migrations).

Zaktualizuj *strony/Shared/_LoginPartial. cshtml* i zastąp `IdentityUser` `ApplicationUser`:

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

Zaktualizuj *obszary/Identity/IdentityHostingStartup. cs* lub `Startup.ConfigureServices` i zastąp `IdentityUser` `ApplicationUser`.

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

W ASP.NET Core 2,1 lub nowszej tożsamość jest dostarczana jako Biblioteka klas Razor. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/scaffold-identity>. W związku z tym poprzedzający kod wymaga wywołania do <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Jeśli do dodawania plików tożsamości do projektu użyto szkieletu tożsamości, Usuń wywołanie do `AddDefaultUI`. Aby uzyskać więcej informacji, zobacz:

* [Tworzenie szkieletu tożsamości](xref:security/authentication/scaffold-identity)
* [Dodawanie, pobieranie i usuwanie niestandardowych danych użytkownika do tożsamości](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a>Zmień typ klucza podstawowego

Zmiana typu danych kolumny klucza podstawowego jest nieproblemowa w wielu systemach baz danych. Zmiana klucza PK zazwyczaj obejmuje porzucenie i ponowne utworzenie tabeli. W związku z tym typy kluczy należy określić podczas początkowej migracji podczas tworzenia bazy danych.

Wykonaj następujące kroki, aby zmienić typ klucza PK:

1. Jeśli baza danych została utworzona przed zmianą PK, uruchom polecenie `Drop-Database` (PMC) lub `dotnet ef database drop` (interfejs wiersza polecenia platformy .NET Core), aby je usunąć.
2. Po potwierdzeniu usunięcia bazy danych Usuń migrację początkową z `Remove-Migration` (PMC) lub `dotnet ef migrations remove` (interfejs wiersza polecenia platformy .NET Core).
3. Zaktualizuj klasę `ApplicationDbContext`, aby dziedziczyć z <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>. Określ nowy typ klucza dla `TKey`. Na przykład, aby użyć `Guid` typu klucza:

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

    W poprzednim kodzie należy określić klasy generyczne <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> i <xref:Microsoft.AspNetCore.Identity.IdentityRole%601>, aby użyć nowego typu klucza.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    W poprzednim kodzie należy określić klasy generyczne <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> i <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601>, aby użyć nowego typu klucza.

    ::: moniker-end

    `Startup.ConfigureServices` należy zaktualizować, aby można było używać użytkownika generycznego:

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

4. Jeśli używana jest niestandardowa Klasa `ApplicationUser`, zaktualizuj klasę, aby dziedziczyć po `IdentityUser`. Na przykład:

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    Zaktualizuj `ApplicationDbContext`, aby odwoływać się do niestandardowej klasy `ApplicationUser`:

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

    Zarejestrowanie niestandardowej klasy kontekstu bazy danych podczas dodawania usługi tożsamości w `Startup.ConfigureServices`:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    Typ danych klucza podstawowego jest wywnioskowany przez analizowanie obiektu [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .

    W ASP.NET Core 2,1 lub nowszej tożsamość jest dostarczana jako Biblioteka klas Razor. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/scaffold-identity>. W związku z tym poprzedzający kod wymaga wywołania do <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Jeśli do dodawania plików tożsamości do projektu użyto szkieletu tożsamości, Usuń wywołanie do `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    Typ danych klucza podstawowego jest wywnioskowany przez analizowanie obiektu [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    Metoda <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> akceptuje typ `TKey` wskazujący typ danych klucza podstawowego.

    ::: moniker-end

5. Jeśli używana jest niestandardowa Klasa `ApplicationRole`, zaktualizuj klasę, aby dziedziczyć po `IdentityRole<TKey>`. Na przykład:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    Zaktualizuj `ApplicationDbContext`, aby odwoływać się do niestandardowej klasy `ApplicationRole`. Na przykład następująca Klasa odwołuje się do niestandardowego `ApplicationUser` i niestandardowego `ApplicationRole`:

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Zarejestrowanie niestandardowej klasy kontekstu bazy danych podczas dodawania usługi tożsamości w `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    Typ danych klucza podstawowego jest wywnioskowany przez analizowanie obiektu [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .

    W ASP.NET Core 2,1 lub nowszej tożsamość jest dostarczana jako Biblioteka klas Razor. Aby uzyskać więcej informacji, zobacz <xref:security/authentication/scaffold-identity>. W związku z tym poprzedzający kod wymaga wywołania do <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Jeśli do dodawania plików tożsamości do projektu użyto szkieletu tożsamości, Usuń wywołanie do `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Zarejestrowanie niestandardowej klasy kontekstu bazy danych podczas dodawania usługi tożsamości w `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    Typ danych klucza podstawowego jest wywnioskowany przez analizowanie obiektu [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Zarejestrowanie niestandardowej klasy kontekstu bazy danych podczas dodawania usługi tożsamości w `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    Metoda <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> akceptuje typ `TKey` wskazujący typ danych klucza podstawowego.

    ::: moniker-end

### <a name="add-navigation-properties"></a>Dodawanie właściwości nawigacji

Zmiana konfiguracji modelu dla relacji może być trudniejsza niż wprowadzanie innych zmian. Należy zwrócić uwagę, aby zastąpić istniejące relacje zamiast tworzyć nowe, dodatkowe relacje. W szczególności zmieniona relacja musi określać tę samą właściwość klucza obcego (FK) jak istniejąca relacja. Na przykład relacja między `Users` i `UserClaims` jest domyślnie określona w następujący sposób:

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

OBCY dla tej relacji jest określony jako właściwość `UserClaim.UserId`. `HasMany` i `WithOne` są wywoływane bez argumentów, aby utworzyć relację bez właściwości nawigacji.

Dodaj właściwość nawigacji do `ApplicationUser`, która pozwala na odwoływanie się do `UserClaims` przez użytkownika:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

`TKey` dla `IdentityUserClaim<TKey>` jest typem określonym dla klucza PK użytkowników. W tym przypadku `TKey` jest `string`, ponieważ są używane wartości domyślne. **Nie** jest to typ podstawowy dla typu jednostki `UserClaim`.

Teraz, gdy istnieje właściwość nawigacji, należy ją skonfigurować w `OnModelCreating`:

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

Należy zauważyć, że relacja jest konfigurowana dokładnie tak, jak była wcześniej, tylko z właściwością nawigacji określoną w wywołaniu `HasMany`.

Właściwości nawigacji istnieją tylko w modelu EF, a nie w bazie danych. Ponieważ obcy dla relacji nie uległ zmianie, ten rodzaj zmiany modelu nie wymaga aktualizacji bazy danych. Można to sprawdzić przez dodanie migracji po dokonaniu zmiany. Metody `Up` i `Down` są puste.

### <a name="add-all-user-navigation-properties"></a>Dodaj wszystkie właściwości nawigacji użytkownika

Korzystając z powyższej sekcji jako wskazówki, Poniższy przykład konfiguruje jednokierunkowe właściwości nawigacji dla wszystkich relacji na użytkowniku:

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

### <a name="add-user-and-role-navigation-properties"></a>Dodawanie właściwości nawigacji użytkownika i roli

Korzystając z powyższej sekcji jako wskazówki, Poniższy przykład konfiguruje właściwości nawigacji dla wszystkich relacji dla użytkownika i roli:

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

* Ten przykład obejmuje również jednostkę sprzężenia `UserRole`, która jest wymagana do nawigowania po relacji wiele-do-wielu od użytkowników do ról.
* Pamiętaj, aby zmienić typy właściwości nawigacji, aby odzwierciedlić, że typy `ApplicationXxx` są teraz używane zamiast typów `IdentityXxx`.
* Należy pamiętać, aby użyć `ApplicationXxx` w definicji `ApplicationContext` generycznej.

### <a name="add-all-navigation-properties"></a>Dodaj wszystkie właściwości nawigacji

Korzystając z powyższej sekcji jako wskazówki, Poniższy przykład konfiguruje właściwości nawigacji dla wszystkich relacji dla wszystkich typów jednostek:

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

Poprzednie sekcje przedstawiają zmianę typu klucza używanego w modelu tożsamości. Zmiana modelu klucza tożsamości w celu korzystania z kluczy złożonych nie jest obsługiwana lub zalecana. Użycie klucza złożonego z tożsamością obejmuje zmianę sposobu, w jaki kod programu Identity Manager współdziała z modelem. To dostosowanie wykracza poza zakres tego dokumentu.

### <a name="change-tablecolumn-names-and-facets"></a>Zmiana nazw tabel/kolumn i aspektów

Aby zmienić nazwy tabel i kolumn, wywołaj `base.OnModelCreating`. Następnie Dodaj konfigurację, aby zastąpić dowolne ustawienia domyślne. Na przykład, aby zmienić nazwę wszystkich tabel tożsamości:

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

W tych przykładach użyto domyślnych typów tożsamości. Jeśli używasz typu aplikacji, takiego jak `ApplicationUser`, skonfiguruj ten typ zamiast typu domyślnego.

Poniższy przykład zmienia nazwy niektórych kolumn:

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

Niektóre typy kolumn bazy danych można skonfigurować przy użyciu określonych *aspektów* (na przykład maksymalna dozwolona długość `string`). Poniższy przykład ustawia maksymalną długość kolumny dla kilku `string` właściwości w modelu:

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

### <a name="map-to-a-different-schema"></a>Mapuj na inny schemat

Schematy mogą działać inaczej niż dostawcy baz danych. W przypadku SQL Server wartością domyślną jest utworzenie wszystkich tabel w schemacie *dbo* . Tabele można tworzyć w innym schemacie. Na przykład:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a>ładowanie z opóźnieniem

W tej sekcji zostanie dodana obsługa serwerów proxy ładowania opóźnionego w modelu tożsamości. Ładowanie z opóźnieniem jest przydatne, ponieważ umożliwia korzystanie z właściwości nawigacji bez uprzedniego załadowania.

Typy jednostek mogą być odpowiednie do ładowania z opóźnieniem na kilka sposobów, zgodnie z opisem w [dokumentacji EF Core](/ef/core/querying/related-data#lazy-loading). Dla uproszczenia Użyj serwerów proxy ładowania z opóźnieniem, które wymagają:

* Instalacja pakietu [Microsoft. EntityFrameworkCore. proxy](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) .
* Wywołanie <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> wewnątrz [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).
* Typy jednostek publicznych z `public virtual` właściwościami nawigacji.

Poniższy przykład ilustruje wywoływanie `UseLazyLoadingProxies` w `Startup.ConfigureServices`:

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Zapoznaj się z powyższymi przykładami, aby uzyskać wskazówki dotyczące dodawania właściwości nawigacji do typów jednostek.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authentication/scaffold-identity>

::: moniker-end
