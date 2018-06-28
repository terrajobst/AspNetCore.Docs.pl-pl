---
title: Dostosowywanie modelu tożsamości
author: ajcvickers
description: W tym artykule opisano sposób dostosowywania odpowiedni model danych Entity Framework Core dla ASP.NET Core Identity.
ms.author: avickers
ms.date: 04/12/2018
ms.prod: asp.net-core
uid: security/authentication/customize_identity_model
ms.openlocfilehash: b44b4fd0f24d245b969588a7226ea6aacbe2a722
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036916"
---
# <a name="identity-model-customization"></a>Dostosowywanie modelu tożsamości

Przez [Arthur Vickers](https://github.com/ajcvickers)

Tożsamość platformy ASP.NET Core zapewnia platformę do zarządzania i przechowywanie kont użytkowników w aplikacji platformy ASP.NET Core. Tożsamość jest dodawany do projektu, w przypadku wybrania jako mechanizmu uwierzytelniania "Indywidualnych kont użytkowników". Domyślnie tożsamości korzysta z Framework jednostki (EF) podstawowe dane modelu. W tym artykule opisano sposób dostosowywania modelu tożsamości.

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a>Tożsamość i EF Core migracji

Przed badanie modelu, warto zrozumieć sposób działania tożsamości z [migracje Core EF](/ef/core/managing-schemas/migrations/) do tworzenia i aktualizacji bazy danych. Na najwyższym poziomie proces jest:

1. Zdefiniuj lub zaktualizuj [modelu danych w kodzie](/ef/core/modeling/).
1. Dodaj migracji do tłumaczenia tego modelu na zmiany, które można stosować do bazy danych.
1. Sprawdź, czy migracja poprawnie reprezentuje zamiaru.
1. Zastosuj migracji w celu zaktualizowania bazy danych, aby był zsynchronizowany z modelem.
1. Powtórz kroki 1 – 4 można dodatkowo doprecyzować modelu i synchronizowania bazy danych.

Migracje są dodawane i stosowane przy użyciu:

* [Konsola Menedżera pakietów](/ef/core/miscellaneous/cli/powershell) (PMC), jeśli używasz programu Visual Studio.
* [Dotnet polecenia](/ef/core/miscellaneous/cli/dotnet) w wierszu polecenia.
* Po uruchomieniu aplikacji, należy kliknąć łącze migracji na stronie błędu.

Platformy ASP.NET Core ma błąd w czasie opracowywania obsługi strony, który może służyć do zastosowania migracji, gdy aplikacja jest uruchamiana. Dla aplikacji produkcyjnych, często jest więcej odpowiednie do generowania skryptów SQL z migracji i wdrożyć je w bazie danych w ramach kontrolowanego wdrożenia aplikacji i baz danych.

Podczas tworzenia nowej aplikacji przy użyciu tożsamości kroki 1 i 2 została już ukończona. Oznacza to model początkowych danych już istnieje, a początkowej migracji został dodany do projektu. Początkowa migracji nadal musi zostać zastosowana do bazy danych. Początkowa migracji można wykonać, uruchamiając Update-Database (PMC), dotnet ef bazy danych (.NET Core CLI) polecenia update, lub za pomocą strony błędu, gdy aplikacja jest uruchamiana.

Powyższych kroków należy powtórzyć zmiany w modelu.

<a name="identity-model"></a>

## <a name="the-identity-model"></a>Modelu tożsamości

### <a name="entity-types"></a>Typy jednostek

Modelu tożsamości składa się z siedmiu typów jednostek:

* `User` -reprezentuje użytkownika
* `Role` -reprezentuje roli
* `UserClaim` -reprezentuje oświadczenie, które posiadają użytkownika
* `UserToken` -reprezentuje tokenu uwierzytelniania użytkownika
* `UserLogin` — Umożliwia skojarzenie użytkownika z logowaniem
* `RoleClaim` -reprezentuje oświadczenie, który został udzielony dla wszystkich użytkowników w roli
* `UserRole` -jednostki, która kojarzy użytkowników i ról Dołącz

### <a name="entity-type-relationships"></a>Relacje typów jednostek

Te typy jednostek są powiązane ze sobą w następujący sposób:

* Każdy `User` może mieć wiele `UserClaims`
* Każdy `User` może mieć wiele `UserLogins`
* Każdy `User` może mieć wiele `UserTokens`
* Każdy `Role` może mieć wiele skojarzonych `RoleClaims`
* Każdego `User` może mieć wiele skojarzonych `Roles`, a każdy `Role` może być skojarzony z wieloma użytkownikami
  * Jest to relację wiele do wielu, co wymaga sprzężenia tabeli w bazie danych. W tabeli sprzężenia jest reprezentowana przez `UserRole` jednostki.

### <a name="default-model-configuration"></a>Domyślna konfiguracja modelu

Tożsamość definiuje różne "kontekstu klasy", które dziedziczą z [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) do konfigurowania i korzystania z modelu. Ta konfiguracja jest implementowana przy użyciu [EF Core kod pierwszego interfejsu API Fluent](/ef/core/modeling/) w [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) metody klasy kontekstu. Domyślna konfiguracja polega:

```CSharp
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

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common database restrictions
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

Tożsamość definiuje domyślnych typów CLR dla poszczególnych typów jednostek wymienionych powyżej. Te typy są poprzedzane prefiksem "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, i `IdentityUserRole`.

Zamiast bezpośrednio przy użyciu tego typu, typów może służyć jako klasy podstawowej dla typów aplikacji. `DbContext` Klas zdefiniowanych przez tożsamości są ogólne w taki sposób, że można używać różnych typów CLR dla jednego lub więcej typów jednostek w modelu. Te typy ogólne pozwalają również typ klucza podstawowego użytkownika zostanie zmieniony.

Podczas korzystania z tożsamości z pomocy technicznej dla ról, [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) klasy powinien być używany:

```CSharp
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
public class IdentityDbContext<TUser, TRole, TKey>
    : IdentityDbContext<TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>, IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
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

Istnieje również możliwość korzystania z tożsamości bez ról (tylko oświadczenia), w którym to przypadku `IdentityUserContext` klasy powinien być używany:


```CSharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey>
    : IdentityUserContext<TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
    : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customizing-the-model"></a>Dostosowywanie modelu

Punkt początkowy Dostosowywanie modelu ma pochodzić od typu odpowiedniego kontekstu; zobacz poprzednią sekcję. Ten typ kontekstu zwyczajowo jest nazywany `ApplicationDbContext` i zostanie utworzony za pomocą szablonów ASP.NET Core.

Kontekst jest używany do konfigurowania modelu na dwa sposoby:
* Podając typów jednostek i klucz dla parametrów typu ogólnego.
* Przez zastąpienie `OnModelCreating` do modyfikowania mapowania typu.

W przypadku przesłaniania `OnModelCreating`, `base.OnModelCreating` powinna być wywoływana po pierwsze, zastępowanie konfiguracji powinna być wywoływana dalej. Podstawowe EF ma zazwyczaj zasady ostatnich jednej usługi wins konfiguracji. Na przykład jeśli `ToTable` dla jednostki typu jest wywołane najpierw o nazwie jednej tabeli, a następnie ponownie później z inną nazwę tabeli, a następnie nazwę tabeli w drugie wywołanie jest ten, który jest używany.

### <a name="using-a-custom-user-type"></a>Przy użyciu niestandardowego typu użytkownika

Aby użyć niestandardowego typu użytkownika, utworzenia typu i ma on dziedziczyć `IdentityUser`. Zwyczajowe nazwę tego typu jest `ApplicationUser`. Ten typ ma zazwyczaj dodatkowe właściwości nie w typie podstawowym, w przeciwnym razie nie byłoby żadnej wartości w jego tworzenia. Na przykład:

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Następnie można użyć tego typu jako argument rodzajowy kontekstu:

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Aktualizacja `ConfigureServices` do używania nowych `ApplicationUser` klasy:

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Nie istnieje potrzeba do przesłonięcia `OnModelCreating` tutaj, ponieważ przypisze EF Core `CustomTag` właściwości przez Konwencję. Jednak bazy danych musi zostać zaktualizowany w celu pobrania nowego tokenu `CustomTag` kolumny. Aby to zrobić, Dodaj migracji, a następnie zaktualizować bazę danych, zgodnie z opisem w [tożsamości i migracje Core EF](#identity-migrations).

### <a name="changing-the-key-type"></a>Zmiana typu klucza

Zmiana typu to kolumna klucza podstawowego (PK), po utworzeniu bazy danych stanowi problem w wielu systemach bazy danych. Zmiana klucza produktu zazwyczaj pociąga za sobą usunięcie i ponowne utworzenie tabeli. W związku z tym zalecane jest, że typów kluczy można określić w początkowej migracji tak, aby typów kluczy docelowej są tworzone po utworzeniu bazy danych.

Jeśli utworzono bazę danych, następnie `Drop-Database` (PMC) lub `dotnet ef database drop` (.NET Core CLI) można go usunąć.

Po potwierdza, że baza danych nie istnieje, Usuń początkowy migracji z `Remove-Migration` (PMC) lub `dotnet ef migrations remove` (.NET Core CLI).

Aktualizacja `ApplicationDbContext` używać innej klasy podstawowej, określając nowy typ klucza dla `TKey`. Na przykład, aby użyć `Guid` klucza:

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Zwróć uwagę, że klas rodzajowych `IdentityUser<TKey>` i `IdentityRole<TKey>` musi być także określona do użycia nowego typu klucza. `ConfigureServices` należy zaktualizować tak, aby użyć ogólnego użytkownika:

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Jeśli niestandardowego `ApplicationUser` jest używany, zmodyfikuj go odziedziczone `IdentityUser<TKey>`. Na przykład:

```CSharp
public class ApplicationUser : IdentityUser<Guid>
{
    public string CustomTag { get; set; }
}

public class ApplicationDbContext : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="adding-navigation-properties"></a>Dodawanie właściwości nawigacji

Zmiana konfiguracji modelu dla relacji może być trudniejsze niż innych zmian. Należy uważać, aby zastąpić istniejące relacje, zamiast tworzyć nowe relacje dodatkowe. W szczególności zmienione relacja musi określać tej samej właściwości klucza obcego jako istniejącą relację. Na przykład relacja między `Users` i `UserClaims` jest domyślnie określony jako:

```CSharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

Klucz obcy dla tej relacji został określony jako `UserClaim.UserId` właściwości. `HasMany` i `WithOne` są nazywane bez argumentów do utworzenia relacji bez właściwości nawigacji.

Właściwość nawigacji, aby dodać `ApplicationUser` , które pozwolą skojarzone `UserClaims` przywoływanie od użytkownika:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

Należy pamiętać, że `TKey` dla `IdentityUserClaim<TKey>` jest typ określony dla klucza podstawowego użytkowników — w tym przypadku `string` ponieważ firma Microsoft korzysta z ustawień domyślnych. Jest **nie** typu klucza podstawowego dla `UserClaim` typu jednostki.

Teraz, gdy istnieje właściwość nawigacji muszą być skonfigurowane w `OnModelCreating`:

```CSharp
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

Zwróć uwagę, że relacja jest skonfigurowany dokładnie tak, jak sprzed, tylko w przypadku właściwości nawigacji określone w wywołaniu `HasMany`.

Właściwości nawigacji istnieje tylko w modelu EF, a nie bazy danych. Ponieważ klucz obcy dla relacji nie została zmieniona, tego rodzaju zmiany modelu nie wymaga bazy danych do zaktualizowania. Można to sprawdzić, dodając migracji po wprowadzeniu zmian: `Up` i `Down` metody są puste.

### <a name="adding-all-user-navigation-properties"></a>Dodanie wszystkich właściwości nawigacji użytkownika.

Korzystając z sekcji powyżej jako wskazówek, Oto przykład skonfigurowanie właściwości jednokierunkowe nawigacji dla wszystkich relacji na użytkownika:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```CSharp
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

### <a name="adding-user-and-role-navigation-properties"></a>Dodawanie użytkownika i roli właściwości nawigacji

Korzystając z sekcji powyżej jako wskazówek, Oto konfiguruje właściwości nawigacji dla wszystkich relacji użytkownika i roli — na przykład:

```CSharp
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

```CSharp
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

* W tym przykładzie obejmuje również `UserRole` dołączyć jednostki, który jest wymagany do nawigacji relacji wiele do wielu użytkowników do ról.
* Pamiętaj, aby zmienić typy właściwości nawigacji, aby odzwierciedlić który `ApplicationXxx` typy są teraz używane zamiast `IdentityXxx` typów.
* Pamiętaj, aby użyć `ApplicationXxx` w ogólnej `ApplicationContext` definicji.

### <a name="adding-all-navigation-properties"></a>Dodawanie wszystkie właściwości nawigacji

Korzystając z sekcji powyżej jako wskazówek, Oto przykład konfiguruje właściwości nawigacji dla wszystkich relacji dla wszystkich typów jednostek:

```CSharp
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

```CSharp
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

### <a name="using-composite-keys"></a>Za pomocą kluczy złożonych

Poprzednich sekcjach przedstawiona zmiana typu użycia klucza w modelu tożsamości. Zmiana modelu klucza tożsamości do użycia klucze złożone nie jest obsługiwany lub zalecane. Przy użyciu klucza złożonego z tożsamością wymagałoby zmiana, jak kod menedżera tożsamości współdziała z modelu, który jest nieobsługiwany dostosowania i poza zakres tego dokumentu.

### <a name="changing-tablecolumn-names-and-facets"></a>Zmiana nazwy tabeli z kolumnami i aspektów

Aby zmienić nazwy tabel i kolumn, należy wywołać `base.OnModelCreating`, a następnie dodaj konfigurację, aby przesłonić wartości domyślne. Na przykład, aby zmienić nazwę wszystkie tabele tożsamości:

```CSharp
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

Należy pamiętać, że te przykłady typów tożsamości domyślnej. Jeśli używasz typu aplikacji, takich jak `ApplicationUser` skonfiguruj typu zamiast domyślnego typu.

Oto przykład zmieniający niektóre nazwy kolumn:

```CSharp
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

Niektóre typy kolumn bazy danych można skonfigurować z niektórymi _aspekty_ takie jak długość maksymalną dozwoloną. Oto przykład, który ustawia maksymalną długość kolumny kilka właściwości ciągu w modelu:

```CSharp
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

### <a name="mapping-to-a-different-schema"></a>Mapowanie do innego schematu

Schematy mogą zachowywać się inaczej w dostawców innej bazy danych, ale dla programu SQL Server, wartość domyślna to utworzyć wszystkie tabele w schemacie "właściciela". Można to zmienić zamiast tego utworzyć tabele w innym schematem. Na przykład:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a>Powolne ładowanie

W tej sekcji jest dodana obsługa serwerów proxy opóźnionego ładowania w modelu tożsamości. Lazy ładowania jest przydatne, ponieważ umożliwia ono pomyślne właściwości nawigacji, aby można używać bez zapewnienia są one załadowany.

Typy jednostek można wprowadzić odpowiednie do opóźnionego ładowania na kilka sposobów, zgodnie z opisem w [EF podstawowej dokumentacji](/ef/core/querying/related-data#lazy-loading). Dla uproszczenia używamy opóźnionego ładowania serwerów proxy, który wymaga:

* Instalacja programu [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pakietu.
* Wywołanie `.UseLazyLoadingProxies()` wewnątrz `AddDbContext`.
* Typy jednostek publicznych z właściwościami publiczny wirtualny nawigacji.

Oto przykład wywołania metody `.UseLazyLoadingProxies()`:

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Odwołaj się do przykłady powyżej dodawania właściwości nawigacji do typów jednostek.
