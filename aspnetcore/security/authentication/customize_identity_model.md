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
# <a name="identity-model-customization"></a><span data-ttu-id="36a82-103">Dostosowywanie modelu tożsamości</span><span class="sxs-lookup"><span data-stu-id="36a82-103">Identity model customization</span></span>

<span data-ttu-id="36a82-104">Przez [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="36a82-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="36a82-105">Tożsamość platformy ASP.NET Core zapewnia platformę do zarządzania i przechowywanie kont użytkowników w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="36a82-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core applications.</span></span> <span data-ttu-id="36a82-106">Tożsamość jest dodawany do projektu, w przypadku wybrania jako mechanizmu uwierzytelniania "Indywidualnych kont użytkowników".</span><span class="sxs-lookup"><span data-stu-id="36a82-106">Identity is added to your project when "Individual User Accounts" is selected as the authentication mechanism.</span></span> <span data-ttu-id="36a82-107">Domyślnie tożsamości korzysta z Framework jednostki (EF) podstawowe dane modelu.</span><span class="sxs-lookup"><span data-stu-id="36a82-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="36a82-108">W tym artykule opisano sposób dostosowywania modelu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="36a82-108">This article describes how to customize the Identity model.</span></span>

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="36a82-109">Tożsamość i EF Core migracji</span><span class="sxs-lookup"><span data-stu-id="36a82-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="36a82-110">Przed badanie modelu, warto zrozumieć sposób działania tożsamości z [migracje Core EF](/ef/core/managing-schemas/migrations/) do tworzenia i aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="36a82-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="36a82-111">Na najwyższym poziomie proces jest:</span><span class="sxs-lookup"><span data-stu-id="36a82-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="36a82-112">Zdefiniuj lub zaktualizuj [modelu danych w kodzie](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="36a82-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="36a82-113">Dodaj migracji do tłumaczenia tego modelu na zmiany, które można stosować do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="36a82-113">Add a migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="36a82-114">Sprawdź, czy migracja poprawnie reprezentuje zamiaru.</span><span class="sxs-lookup"><span data-stu-id="36a82-114">Check that the migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="36a82-115">Zastosuj migracji w celu zaktualizowania bazy danych, aby był zsynchronizowany z modelem.</span><span class="sxs-lookup"><span data-stu-id="36a82-115">Apply the migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="36a82-116">Powtórz kroki 1 – 4 można dodatkowo doprecyzować modelu i synchronizowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="36a82-116">Repeat steps 1-4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="36a82-117">Migracje są dodawane i stosowane przy użyciu:</span><span class="sxs-lookup"><span data-stu-id="36a82-117">Migrations are added and applied using:</span></span>

* <span data-ttu-id="36a82-118">[Konsola Menedżera pakietów](/ef/core/miscellaneous/cli/powershell) (PMC), jeśli używasz programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36a82-118">The [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) if you are using Visual Studio.</span></span>
* <span data-ttu-id="36a82-119">[Dotnet polecenia](/ef/core/miscellaneous/cli/dotnet) w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="36a82-119">The [dotnet commands](/ef/core/miscellaneous/cli/dotnet) on the command line.</span></span>
* <span data-ttu-id="36a82-120">Po uruchomieniu aplikacji, należy kliknąć łącze migracji na stronie błędu.</span><span class="sxs-lookup"><span data-stu-id="36a82-120">Selecting the migrations link on the error page when the application is run.</span></span>

<span data-ttu-id="36a82-121">Platformy ASP.NET Core ma błąd w czasie opracowywania obsługi strony, który może służyć do zastosowania migracji, gdy aplikacja jest uruchamiana.</span><span class="sxs-lookup"><span data-stu-id="36a82-121">ASP.NET Core has a development-time error page handler that can be used to apply migrations when the application is run.</span></span> <span data-ttu-id="36a82-122">Dla aplikacji produkcyjnych, często jest więcej odpowiednie do generowania skryptów SQL z migracji i wdrożyć je w bazie danych w ramach kontrolowanego wdrożenia aplikacji i baz danych.</span><span class="sxs-lookup"><span data-stu-id="36a82-122">For production applications, it is often more appropriate to generate SQL scripts from the migrations and deploy these to the database as part of a controlled application and database deployment.</span></span>

<span data-ttu-id="36a82-123">Podczas tworzenia nowej aplikacji przy użyciu tożsamości kroki 1 i 2 została już ukończona.</span><span class="sxs-lookup"><span data-stu-id="36a82-123">When a new application using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="36a82-124">Oznacza to model początkowych danych już istnieje, a początkowej migracji został dodany do projektu.</span><span class="sxs-lookup"><span data-stu-id="36a82-124">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="36a82-125">Początkowa migracji nadal musi zostać zastosowana do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="36a82-125">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="36a82-126">Początkowa migracji można wykonać, uruchamiając Update-Database (PMC), dotnet ef bazy danych (.NET Core CLI) polecenia update, lub za pomocą strony błędu, gdy aplikacja jest uruchamiana.</span><span class="sxs-lookup"><span data-stu-id="36a82-126">The initial migration can be done by running the Update-Database (PMC), the dotnet ef database update (.NET Core CLI) command, or by using the error page when the application is run.</span></span>

<span data-ttu-id="36a82-127">Powyższych kroków należy powtórzyć zmiany w modelu.</span><span class="sxs-lookup"><span data-stu-id="36a82-127">The preceding steps need to be repeated as changes are made to the model.</span></span>

<a name="identity-model"></a>

## <a name="the-identity-model"></a><span data-ttu-id="36a82-128">Modelu tożsamości</span><span class="sxs-lookup"><span data-stu-id="36a82-128">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="36a82-129">Typy jednostek</span><span class="sxs-lookup"><span data-stu-id="36a82-129">Entity types</span></span>

<span data-ttu-id="36a82-130">Modelu tożsamości składa się z siedmiu typów jednostek:</span><span class="sxs-lookup"><span data-stu-id="36a82-130">The Identity model consists of seven entity types:</span></span>

* <span data-ttu-id="36a82-131">`User` -reprezentuje użytkownika</span><span class="sxs-lookup"><span data-stu-id="36a82-131">`User` - represents the user</span></span>
* <span data-ttu-id="36a82-132">`Role` -reprezentuje roli</span><span class="sxs-lookup"><span data-stu-id="36a82-132">`Role` - represents a role</span></span>
* <span data-ttu-id="36a82-133">`UserClaim` -reprezentuje oświadczenie, które posiadają użytkownika</span><span class="sxs-lookup"><span data-stu-id="36a82-133">`UserClaim` - represents a claim that a user possess</span></span>
* <span data-ttu-id="36a82-134">`UserToken` -reprezentuje tokenu uwierzytelniania użytkownika</span><span class="sxs-lookup"><span data-stu-id="36a82-134">`UserToken` - represents an authentication token for a user</span></span>
* <span data-ttu-id="36a82-135">`UserLogin` — Umożliwia skojarzenie użytkownika z logowaniem</span><span class="sxs-lookup"><span data-stu-id="36a82-135">`UserLogin` - associates a user with a login</span></span>
* <span data-ttu-id="36a82-136">`RoleClaim` -reprezentuje oświadczenie, który został udzielony dla wszystkich użytkowników w roli</span><span class="sxs-lookup"><span data-stu-id="36a82-136">`RoleClaim` - represents a claim that is granted to all users within a role</span></span>
* <span data-ttu-id="36a82-137">`UserRole` -jednostki, która kojarzy użytkowników i ról Dołącz</span><span class="sxs-lookup"><span data-stu-id="36a82-137">`UserRole` - join entity that associates users and roles</span></span>

### <a name="entity-type-relationships"></a><span data-ttu-id="36a82-138">Relacje typów jednostek</span><span class="sxs-lookup"><span data-stu-id="36a82-138">Entity type relationships</span></span>

<span data-ttu-id="36a82-139">Te typy jednostek są powiązane ze sobą w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="36a82-139">These entity types are related to each other in the following ways:</span></span>

* <span data-ttu-id="36a82-140">Każdy `User` może mieć wiele `UserClaims`</span><span class="sxs-lookup"><span data-stu-id="36a82-140">Each `User` can have many `UserClaims`</span></span>
* <span data-ttu-id="36a82-141">Każdy `User` może mieć wiele `UserLogins`</span><span class="sxs-lookup"><span data-stu-id="36a82-141">Each `User` can have many `UserLogins`</span></span>
* <span data-ttu-id="36a82-142">Każdy `User` może mieć wiele `UserTokens`</span><span class="sxs-lookup"><span data-stu-id="36a82-142">Each `User` can have many `UserTokens`</span></span>
* <span data-ttu-id="36a82-143">Każdy `Role` może mieć wiele skojarzonych `RoleClaims`</span><span class="sxs-lookup"><span data-stu-id="36a82-143">Each `Role` can have many associated `RoleClaims`</span></span>
* <span data-ttu-id="36a82-144">Każdego `User` może mieć wiele skojarzonych `Roles`, a każdy `Role` może być skojarzony z wieloma użytkownikami</span><span class="sxs-lookup"><span data-stu-id="36a82-144">Each `User` can have many associated `Roles`, and each `Role` can be associated with many Users</span></span>
  * <span data-ttu-id="36a82-145">Jest to relację wiele do wielu, co wymaga sprzężenia tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="36a82-145">This is a many-to-many relationship, which requires a join table in the database.</span></span> <span data-ttu-id="36a82-146">W tabeli sprzężenia jest reprezentowana przez `UserRole` jednostki.</span><span class="sxs-lookup"><span data-stu-id="36a82-146">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="36a82-147">Domyślna konfiguracja modelu</span><span class="sxs-lookup"><span data-stu-id="36a82-147">Default model configuration</span></span>

<span data-ttu-id="36a82-148">Tożsamość definiuje różne "kontekstu klasy", które dziedziczą z [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) do konfigurowania i korzystania z modelu.</span><span class="sxs-lookup"><span data-stu-id="36a82-148">Identity defines a variety of "context classes" which inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="36a82-149">Ta konfiguracja jest implementowana przy użyciu [EF Core kod pierwszego interfejsu API Fluent](/ef/core/modeling/) w [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) metody klasy kontekstu.</span><span class="sxs-lookup"><span data-stu-id="36a82-149">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) method of the context class.</span></span> <span data-ttu-id="36a82-150">Domyślna konfiguracja polega:</span><span class="sxs-lookup"><span data-stu-id="36a82-150">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="36a82-151">Typy ogólne modelu</span><span class="sxs-lookup"><span data-stu-id="36a82-151">Model generic types</span></span>

<span data-ttu-id="36a82-152">Tożsamość definiuje domyślnych typów CLR dla poszczególnych typów jednostek wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="36a82-152">Identity defines default CLR types for each of the entity types listed above.</span></span> <span data-ttu-id="36a82-153">Te typy są poprzedzane prefiksem "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, i `IdentityUserRole`.</span><span class="sxs-lookup"><span data-stu-id="36a82-153">These types are all prefixed with "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, and `IdentityUserRole`.</span></span>

<span data-ttu-id="36a82-154">Zamiast bezpośrednio przy użyciu tego typu, typów może służyć jako klasy podstawowej dla typów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="36a82-154">Rather than using these types directly, the types can be used as base classes for the application's own types.</span></span> <span data-ttu-id="36a82-155">`DbContext` Klas zdefiniowanych przez tożsamości są ogólne w taki sposób, że można używać różnych typów CLR dla jednego lub więcej typów jednostek w modelu.</span><span class="sxs-lookup"><span data-stu-id="36a82-155">The `DbContext` classes defined by Identity are generic such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="36a82-156">Te typy ogólne pozwalają również typ klucza podstawowego użytkownika zostanie zmieniony.</span><span class="sxs-lookup"><span data-stu-id="36a82-156">These generic types also allow for the type of the User primary key to be changed.</span></span>

<span data-ttu-id="36a82-157">Podczas korzystania z tożsamości z pomocy technicznej dla ról, [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) klasy powinien być używany:</span><span class="sxs-lookup"><span data-stu-id="36a82-157">When using Identity with support for roles, an [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) class should be used:</span></span>

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

<span data-ttu-id="36a82-158">Istnieje również możliwość korzystania z tożsamości bez ról (tylko oświadczenia), w którym to przypadku `IdentityUserContext` klasy powinien być używany:</span><span class="sxs-lookup"><span data-stu-id="36a82-158">It is also possible to use Identity without roles (only claims), in which case an `IdentityUserContext` class should be used:</span></span>


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

## <a name="customizing-the-model"></a><span data-ttu-id="36a82-159">Dostosowywanie modelu</span><span class="sxs-lookup"><span data-stu-id="36a82-159">Customizing the model</span></span>

<span data-ttu-id="36a82-160">Punkt początkowy Dostosowywanie modelu ma pochodzić od typu odpowiedniego kontekstu; zobacz poprzednią sekcję.</span><span class="sxs-lookup"><span data-stu-id="36a82-160">The starting point for customizing the model is to derive from the appropriate context type; see the preceding section.</span></span> <span data-ttu-id="36a82-161">Ten typ kontekstu zwyczajowo jest nazywany `ApplicationDbContext` i zostanie utworzony za pomocą szablonów ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="36a82-161">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="36a82-162">Kontekst jest używany do konfigurowania modelu na dwa sposoby:</span><span class="sxs-lookup"><span data-stu-id="36a82-162">The context is used to configure the model in two ways:</span></span>
* <span data-ttu-id="36a82-163">Podając typów jednostek i klucz dla parametrów typu ogólnego.</span><span class="sxs-lookup"><span data-stu-id="36a82-163">By supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="36a82-164">Przez zastąpienie `OnModelCreating` do modyfikowania mapowania typu.</span><span class="sxs-lookup"><span data-stu-id="36a82-164">By overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="36a82-165">W przypadku przesłaniania `OnModelCreating`, `base.OnModelCreating` powinna być wywoływana po pierwsze, zastępowanie konfiguracji powinna być wywoływana dalej.</span><span class="sxs-lookup"><span data-stu-id="36a82-165">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first, the overriding configuration should be called next.</span></span> <span data-ttu-id="36a82-166">Podstawowe EF ma zazwyczaj zasady ostatnich jednej usługi wins konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="36a82-166">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="36a82-167">Na przykład jeśli `ToTable` dla jednostki typu jest wywołane najpierw o nazwie jednej tabeli, a następnie ponownie później z inną nazwę tabeli, a następnie nazwę tabeli w drugie wywołanie jest ten, który jest używany.</span><span class="sxs-lookup"><span data-stu-id="36a82-167">For example, if the `ToTable` for an entity type is called first with one table name and then again later with a different table name, then the table name in the second call is the one that is used.</span></span>

### <a name="using-a-custom-user-type"></a><span data-ttu-id="36a82-168">Przy użyciu niestandardowego typu użytkownika</span><span class="sxs-lookup"><span data-stu-id="36a82-168">Using a custom User type</span></span>

<span data-ttu-id="36a82-169">Aby użyć niestandardowego typu użytkownika, utworzenia typu i ma on dziedziczyć `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="36a82-169">To use a custom User type, create the type and have it inherit from `IdentityUser`.</span></span> <span data-ttu-id="36a82-170">Zwyczajowe nazwę tego typu jest `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="36a82-170">It is customary to name this type `ApplicationUser`.</span></span> <span data-ttu-id="36a82-171">Ten typ ma zazwyczaj dodatkowe właściwości nie w typie podstawowym, w przeciwnym razie nie byłoby żadnej wartości w jego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="36a82-171">This type will typically have additional properties not in the base type, otherwise there would be no value in creating it.</span></span> <span data-ttu-id="36a82-172">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="36a82-172">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="36a82-173">Następnie można użyć tego typu jako argument rodzajowy kontekstu:</span><span class="sxs-lookup"><span data-stu-id="36a82-173">Next use this type as a generic argument for the context:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="36a82-174">Aktualizacja `ConfigureServices` do używania nowych `ApplicationUser` klasy:</span><span class="sxs-lookup"><span data-stu-id="36a82-174">Update `ConfigureServices` to use the new `ApplicationUser` class:</span></span>

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="36a82-175">Nie istnieje potrzeba do przesłonięcia `OnModelCreating` tutaj, ponieważ przypisze EF Core `CustomTag` właściwości przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="36a82-175">There is no need to override `OnModelCreating` here because EF Core will map the `CustomTag` property by convention.</span></span> <span data-ttu-id="36a82-176">Jednak bazy danych musi zostać zaktualizowany w celu pobrania nowego tokenu `CustomTag` kolumny.</span><span class="sxs-lookup"><span data-stu-id="36a82-176">However, the database will need to be updated to get a new `CustomTag` column.</span></span> <span data-ttu-id="36a82-177">Aby to zrobić, Dodaj migracji, a następnie zaktualizować bazę danych, zgodnie z opisem w [tożsamości i migracje Core EF](#identity-migrations).</span><span class="sxs-lookup"><span data-stu-id="36a82-177">To do this, add a migration, and then update the database as described in  [Identity and EF Core Migrations](#identity-migrations).</span></span>

### <a name="changing-the-key-type"></a><span data-ttu-id="36a82-178">Zmiana typu klucza</span><span class="sxs-lookup"><span data-stu-id="36a82-178">Changing the key type</span></span>

<span data-ttu-id="36a82-179">Zmiana typu to kolumna klucza podstawowego (PK), po utworzeniu bazy danych stanowi problem w wielu systemach bazy danych.</span><span class="sxs-lookup"><span data-stu-id="36a82-179">Changing the type of a primary key (PK) column after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="36a82-180">Zmiana klucza produktu zazwyczaj pociąga za sobą usunięcie i ponowne utworzenie tabeli.</span><span class="sxs-lookup"><span data-stu-id="36a82-180">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="36a82-181">W związku z tym zalecane jest, że typów kluczy można określić w początkowej migracji tak, aby typów kluczy docelowej są tworzone po utworzeniu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="36a82-181">Therefore, it is recommended that key types be specified in the initial migration such that the target key types are created when the database is created.</span></span>

<span data-ttu-id="36a82-182">Jeśli utworzono bazę danych, następnie `Drop-Database` (PMC) lub `dotnet ef database drop` (.NET Core CLI) można go usunąć.</span><span class="sxs-lookup"><span data-stu-id="36a82-182">If the database has been created, then `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) can be used to delete it.</span></span>

<span data-ttu-id="36a82-183">Po potwierdza, że baza danych nie istnieje, Usuń początkowy migracji z `Remove-Migration` (PMC) lub `dotnet ef migrations remove` (.NET Core CLI).</span><span class="sxs-lookup"><span data-stu-id="36a82-183">Once it is confirmed that the database doesn't exist,  remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>

<span data-ttu-id="36a82-184">Aktualizacja `ApplicationDbContext` używać innej klasy podstawowej, określając nowy typ klucza dla `TKey`.</span><span class="sxs-lookup"><span data-stu-id="36a82-184">Update the `ApplicationDbContext` to use a different base class, specifying the new key type for `TKey`.</span></span> <span data-ttu-id="36a82-185">Na przykład, aby użyć `Guid` klucza:</span><span class="sxs-lookup"><span data-stu-id="36a82-185">For example, to use a `Guid` key:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="36a82-186">Zwróć uwagę, że klas rodzajowych `IdentityUser<TKey>` i `IdentityRole<TKey>` musi być także określona do użycia nowego typu klucza.</span><span class="sxs-lookup"><span data-stu-id="36a82-186">Notice that the generic classes `IdentityUser<TKey>` and `IdentityRole<TKey>` must also be specified to use the new key type.</span></span> <span data-ttu-id="36a82-187">`ConfigureServices` należy zaktualizować tak, aby użyć ogólnego użytkownika:</span><span class="sxs-lookup"><span data-stu-id="36a82-187">`ConfigureServices` must be updated to use the generic user:</span></span>

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="36a82-188">Jeśli niestandardowego `ApplicationUser` jest używany, zmodyfikuj go odziedziczone `IdentityUser<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="36a82-188">If a custom `ApplicationUser` is being used, update it to inherit from `IdentityUser<TKey>`.</span></span> <span data-ttu-id="36a82-189">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="36a82-189">For example:</span></span>

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

### <a name="adding-navigation-properties"></a><span data-ttu-id="36a82-190">Dodawanie właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="36a82-190">Adding navigation properties</span></span>

<span data-ttu-id="36a82-191">Zmiana konfiguracji modelu dla relacji może być trudniejsze niż innych zmian.</span><span class="sxs-lookup"><span data-stu-id="36a82-191">Changing the model configuration for relationships can be a more difficult than making other changes.</span></span> <span data-ttu-id="36a82-192">Należy uważać, aby zastąpić istniejące relacje, zamiast tworzyć nowe relacje dodatkowe.</span><span class="sxs-lookup"><span data-stu-id="36a82-192">Care must be taken to replace the existing relationships rather than create a new additional relationships.</span></span> <span data-ttu-id="36a82-193">W szczególności zmienione relacja musi określać tej samej właściwości klucza obcego jako istniejącą relację.</span><span class="sxs-lookup"><span data-stu-id="36a82-193">In particular, the changed relationship must specify the same foreign key property as the existing relationship.</span></span> <span data-ttu-id="36a82-194">Na przykład relacja między `Users` i `UserClaims` jest domyślnie określony jako:</span><span class="sxs-lookup"><span data-stu-id="36a82-194">For example, the relationship between `Users` and `UserClaims` is by default specified as:</span></span>

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

<span data-ttu-id="36a82-195">Klucz obcy dla tej relacji został określony jako `UserClaim.UserId` właściwości.</span><span class="sxs-lookup"><span data-stu-id="36a82-195">The foreign key for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="36a82-196">`HasMany` i `WithOne` są nazywane bez argumentów do utworzenia relacji bez właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="36a82-196">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="36a82-197">Właściwość nawigacji, aby dodać `ApplicationUser` , które pozwolą skojarzone `UserClaims` przywoływanie od użytkownika:</span><span class="sxs-lookup"><span data-stu-id="36a82-197">Add a navigation property to `ApplicationUser` that will allow associated `UserClaims` to be referenced from the user:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="36a82-198">Należy pamiętać, że `TKey` dla `IdentityUserClaim<TKey>` jest typ określony dla klucza podstawowego użytkowników — w tym przypadku `string` ponieważ firma Microsoft korzysta z ustawień domyślnych.</span><span class="sxs-lookup"><span data-stu-id="36a82-198">Note that the `TKey` for `IdentityUserClaim<TKey>` is type specified for the primary key of users--in this case `string` since we're using the defaults.</span></span> <span data-ttu-id="36a82-199">Jest **nie** typu klucza podstawowego dla `UserClaim` typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="36a82-199">It is **not** the primary key type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="36a82-200">Teraz, gdy istnieje właściwość nawigacji muszą być skonfigurowane w `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="36a82-200">Now that the navigation property exists it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="36a82-201">Zwróć uwagę, że relacja jest skonfigurowany dokładnie tak, jak sprzed, tylko w przypadku właściwości nawigacji określone w wywołaniu `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="36a82-201">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="36a82-202">Właściwości nawigacji istnieje tylko w modelu EF, a nie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="36a82-202">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="36a82-203">Ponieważ klucz obcy dla relacji nie została zmieniona, tego rodzaju zmiany modelu nie wymaga bazy danych do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="36a82-203">Because the foreign key for the relationship has not changed, this kind of model change does not require the database to be updated.</span></span> <span data-ttu-id="36a82-204">Można to sprawdzić, dodając migracji po wprowadzeniu zmian: `Up` i `Down` metody są puste.</span><span class="sxs-lookup"><span data-stu-id="36a82-204">This can be checked by adding a migration after making the change: the `Up` and `Down` methods are empty.</span></span>

### <a name="adding-all-user-navigation-properties"></a><span data-ttu-id="36a82-205">Dodanie wszystkich właściwości nawigacji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="36a82-205">Adding all User navigation properties</span></span>

<span data-ttu-id="36a82-206">Korzystając z sekcji powyżej jako wskazówek, Oto przykład skonfigurowanie właściwości jednokierunkowe nawigacji dla wszystkich relacji na użytkownika:</span><span class="sxs-lookup"><span data-stu-id="36a82-206">Using the section above as guidance, here is an example that configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="adding-user-and-role-navigation-properties"></a><span data-ttu-id="36a82-207">Dodawanie użytkownika i roli właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="36a82-207">Adding User and Role navigation properties</span></span>

<span data-ttu-id="36a82-208">Korzystając z sekcji powyżej jako wskazówek, Oto konfiguruje właściwości nawigacji dla wszystkich relacji użytkownika i roli — na przykład:</span><span class="sxs-lookup"><span data-stu-id="36a82-208">Using the section above as guidance, here is an example that configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="36a82-209">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="36a82-209">Notes:</span></span>

* <span data-ttu-id="36a82-210">W tym przykładzie obejmuje również `UserRole` dołączyć jednostki, który jest wymagany do nawigacji relacji wiele do wielu użytkowników do ról.</span><span class="sxs-lookup"><span data-stu-id="36a82-210">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="36a82-211">Pamiętaj, aby zmienić typy właściwości nawigacji, aby odzwierciedlić który `ApplicationXxx` typy są teraz używane zamiast `IdentityXxx` typów.</span><span class="sxs-lookup"><span data-stu-id="36a82-211">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="36a82-212">Pamiętaj, aby użyć `ApplicationXxx` w ogólnej `ApplicationContext` definicji.</span><span class="sxs-lookup"><span data-stu-id="36a82-212">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="adding-all-navigation-properties"></a><span data-ttu-id="36a82-213">Dodawanie wszystkie właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="36a82-213">Adding all navigation properties</span></span>

<span data-ttu-id="36a82-214">Korzystając z sekcji powyżej jako wskazówek, Oto przykład konfiguruje właściwości nawigacji dla wszystkich relacji dla wszystkich typów jednostek:</span><span class="sxs-lookup"><span data-stu-id="36a82-214">Using the section above as guidance, here is an example that configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="using-composite-keys"></a><span data-ttu-id="36a82-215">Za pomocą kluczy złożonych</span><span class="sxs-lookup"><span data-stu-id="36a82-215">Using composite keys</span></span>

<span data-ttu-id="36a82-216">Poprzednich sekcjach przedstawiona zmiana typu użycia klucza w modelu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="36a82-216">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="36a82-217">Zmiana modelu klucza tożsamości do użycia klucze złożone nie jest obsługiwany lub zalecane.</span><span class="sxs-lookup"><span data-stu-id="36a82-217">Changing the Identity key model to use composite keys is not supported or recommended.</span></span> <span data-ttu-id="36a82-218">Przy użyciu klucza złożonego z tożsamością wymagałoby zmiana, jak kod menedżera tożsamości współdziała z modelu, który jest nieobsługiwany dostosowania i poza zakres tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="36a82-218">Using a composite key with Identity would involve changing how the Identity manager code interacts with the model, which is an unsupported customization and beyond the scope of this document.</span></span>

### <a name="changing-tablecolumn-names-and-facets"></a><span data-ttu-id="36a82-219">Zmiana nazwy tabeli z kolumnami i aspektów</span><span class="sxs-lookup"><span data-stu-id="36a82-219">Changing table/column names and facets</span></span>

<span data-ttu-id="36a82-220">Aby zmienić nazwy tabel i kolumn, należy wywołać `base.OnModelCreating`, a następnie dodaj konfigurację, aby przesłonić wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="36a82-220">To change the names of tables and columns, call `base.OnModelCreating`, and then add configuration to override any of the defaults.</span></span> <span data-ttu-id="36a82-221">Na przykład, aby zmienić nazwę wszystkie tabele tożsamości:</span><span class="sxs-lookup"><span data-stu-id="36a82-221">For example, to change the name of all the identity tables:</span></span>

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

<span data-ttu-id="36a82-222">Należy pamiętać, że te przykłady typów tożsamości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="36a82-222">Note that these examples use the default Identity types.</span></span> <span data-ttu-id="36a82-223">Jeśli używasz typu aplikacji, takich jak `ApplicationUser` skonfiguruj typu zamiast domyślnego typu.</span><span class="sxs-lookup"><span data-stu-id="36a82-223">If you are using an application type, such as `ApplicationUser` then configure that type instead of the default type.</span></span>

<span data-ttu-id="36a82-224">Oto przykład zmieniający niektóre nazwy kolumn:</span><span class="sxs-lookup"><span data-stu-id="36a82-224">Here is an example that changes some column names:</span></span>

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

<span data-ttu-id="36a82-225">Niektóre typy kolumn bazy danych można skonfigurować z niektórymi _aspekty_ takie jak długość maksymalną dozwoloną.</span><span class="sxs-lookup"><span data-stu-id="36a82-225">Some types of database columns can be configured with certain _facets_ such as the maximum string length allowed.</span></span> <span data-ttu-id="36a82-226">Oto przykład, który ustawia maksymalną długość kolumny kilka właściwości ciągu w modelu:</span><span class="sxs-lookup"><span data-stu-id="36a82-226">Here is an example that sets column max lengths for several string properties in the model:</span></span>

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

### <a name="mapping-to-a-different-schema"></a><span data-ttu-id="36a82-227">Mapowanie do innego schematu</span><span class="sxs-lookup"><span data-stu-id="36a82-227">Mapping to a different schema</span></span>

<span data-ttu-id="36a82-228">Schematy mogą zachowywać się inaczej w dostawców innej bazy danych, ale dla programu SQL Server, wartość domyślna to utworzyć wszystkie tabele w schemacie "właściciela".</span><span class="sxs-lookup"><span data-stu-id="36a82-228">Schemas can behave differently in different database providers, but for SQL Server, the default is to create all tables in the "dbo" schema.</span></span> <span data-ttu-id="36a82-229">Można to zmienić zamiast tego utworzyć tabele w innym schematem.</span><span class="sxs-lookup"><span data-stu-id="36a82-229">This can be changed to instead create the tables in a different schema.</span></span> <span data-ttu-id="36a82-230">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="36a82-230">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a><span data-ttu-id="36a82-231">Powolne ładowanie</span><span class="sxs-lookup"><span data-stu-id="36a82-231">Lazy loading</span></span>

<span data-ttu-id="36a82-232">W tej sekcji jest dodana obsługa serwerów proxy opóźnionego ładowania w modelu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="36a82-232">In this section support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="36a82-233">Lazy ładowania jest przydatne, ponieważ umożliwia ono pomyślne właściwości nawigacji, aby można używać bez zapewnienia są one załadowany.</span><span class="sxs-lookup"><span data-stu-id="36a82-233">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they are loaded.</span></span>

<span data-ttu-id="36a82-234">Typy jednostek można wprowadzić odpowiednie do opóźnionego ładowania na kilka sposobów, zgodnie z opisem w [EF podstawowej dokumentacji](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="36a82-234">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="36a82-235">Dla uproszczenia używamy opóźnionego ładowania serwerów proxy, który wymaga:</span><span class="sxs-lookup"><span data-stu-id="36a82-235">For simplicity, we will use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="36a82-236">Instalacja programu [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="36a82-236">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="36a82-237">Wywołanie `.UseLazyLoadingProxies()` wewnątrz `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="36a82-237">A call to `.UseLazyLoadingProxies()` inside `AddDbContext`.</span></span>
* <span data-ttu-id="36a82-238">Typy jednostek publicznych z właściwościami publiczny wirtualny nawigacji.</span><span class="sxs-lookup"><span data-stu-id="36a82-238">Public entity types with public virtual navigation properties.</span></span>

<span data-ttu-id="36a82-239">Oto przykład wywołania metody `.UseLazyLoadingProxies()`:</span><span class="sxs-lookup"><span data-stu-id="36a82-239">Here is an example of calling `.UseLazyLoadingProxies()`:</span></span>

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="36a82-240">Odwołaj się do przykłady powyżej dodawania właściwości nawigacji do typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="36a82-240">Refer back to the examples above for adding navigation properties to the entity types.</span></span>
