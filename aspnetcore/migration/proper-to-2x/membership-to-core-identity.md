---
title: Migracja z uwierzytelniania członkostwa ASP.NET do ASP.NET Core 2.0 tożsamości
author: isaac2004
description: Dowiedz się, jak przeprowadzić migrację istniejących aplikacji ASP.NET przy użyciu uwierzytelniania członkostwa platformy ASP.NET Core 2.0 tożsamości.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3b708da13ff9f2887eee87ea17844312a4fe1b8d
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264722"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="bd4ca-103">Migracja z uwierzytelniania członkostwa ASP.NET do ASP.NET Core 2.0 tożsamości</span><span class="sxs-lookup"><span data-stu-id="bd4ca-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="bd4ca-104">Przez [Levin Tomasz](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="bd4ca-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="bd4ca-105">W tym artykule przedstawiono migracji schematu bazy danych dla aplikacji ASP.NET przy użyciu uwierzytelniania członkostwa platformy ASP.NET Core 2.0 tożsamości.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="bd4ca-106">Ten dokument zawiera kroki niezbędne do migracji schematu bazy danych dla aplikacji na podstawie członkostwa ASP.NET do schematu bazy danych używane dla tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="bd4ca-107">Aby uzyskać więcej informacji na temat migracji z uwierzytelniania opartego na członkostwa ASP.NET do produktu ASP.NET Identity, zobacz [Migrowanie istniejących aplikacji z członkostwa SQL do produktu ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="bd4ca-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="bd4ca-108">Aby uzyskać więcej informacji o tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości programu ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="bd4ca-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="bd4ca-109">Przegląd schematu członkostwa</span><span class="sxs-lookup"><span data-stu-id="bd4ca-109">Review of Membership schema</span></span>

<span data-ttu-id="bd4ca-110">Przed ASP.NET 2.0 deweloperzy zostały nadzorowania tworzenia cały proces uwierzytelniania i autoryzacji dla swoich aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="bd4ca-111">Za pomocą programu ASP.NET 2.0 członkostwa została wprowadzona, zapewniając standardowy rozwiązania do obsługi zabezpieczeń w aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="bd4ca-112">Deweloperzy udało się teraz bootstrap schematu do bazy danych programu SQL Server za pomocą [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) polecenia.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="bd4ca-113">Po uruchomieniu tego polecenia, następujące tabele zostały utworzone w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-113">After running this command, the following tables were created in the database.</span></span>

  ![Tabele członkostwa](identity/_static/membership-tables.png)

<span data-ttu-id="bd4ca-115">Aby przeprowadzić migrację istniejących aplikacji do platformy ASP.NET Core 2.0 tożsamości, dane w tabelach musi powinny być migrowane do tabel używanych przez nowy schemat tożsamości.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="bd4ca-116">ASP.NET Core 2.0 tożsamości schematu</span><span class="sxs-lookup"><span data-stu-id="bd4ca-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="bd4ca-117">Platforma ASP.NET Core 2.0 jest zgodna [tożsamości](/aspnet/identity/index) zasady, które wprowadzono w programie ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="bd4ca-118">Jeśli zasady są udostępniane, implementacja między struktury jest różne, nawet w różnych wersjach programu ASP.NET Core (zobacz [migracji, uwierzytelnianie i tożsamość do ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="bd4ca-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="bd4ca-119">Jest to najszybszy sposób, aby wyświetlić schemat dla platformy ASP.NET Core 2.0 tożsamości do utworzenia nowej aplikacji platformy ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="bd4ca-120">Wykonaj następujące kroki w programie Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="bd4ca-120">Follow these steps in Visual Studio 2017:</span></span>

1. <span data-ttu-id="bd4ca-121">Wybierz **pliku** > **nowe** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-121">Select **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="bd4ca-122">Utwórz nową **aplikacji sieci Web programu ASP.NET Core** projektu o nazwie *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-122">Create a new **ASP.NET Core Web Application** project named *CoreIdentitySample*.</span></span>
1. <span data-ttu-id="bd4ca-123">Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="bd4ca-124">Ten szablon tworzy [stron Razor](xref:razor-pages/index) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="bd4ca-125">Przed kliknięciem przycisku **OK**, kliknij przycisk **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-125">Before clicking **OK**, click **Change Authentication**.</span></span>
1. <span data-ttu-id="bd4ca-126">Wybierz **indywidualne konta użytkowników** dla szablonów tożsamości.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="bd4ca-127">Na koniec kliknij **OK**, następnie **OK**.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="bd4ca-128">Program Visual Studio tworzy projekt przy użyciu szablonu platformy ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>
1. <span data-ttu-id="bd4ca-129">Wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów** otworzyć **Konsola Menedżera pakietów** Okno (PMC).</span><span class="sxs-lookup"><span data-stu-id="bd4ca-129">Select **Tools** > **NuGet Package Manager** > **Package Manager Console** to open the **Package Manager Console** (PMC) window.</span></span>
1. <span data-ttu-id="bd4ca-130">Przejdź do katalogu głównego projektu, w konsoli zarządzania Pakietami i uruchom [Core Entity Framework (EF)](/ef/core) `Update-Database` polecenia.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-130">Navigate to the project root in PMC, and run the [Entity Framework (EF) Core](/ef/core) `Update-Database` command.</span></span>

    <span data-ttu-id="bd4ca-131">Tożsamości platformy ASP.NET Core 2.0 używa programu EF Core do interakcji z bazą danych, do przechowywania danych uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-131">ASP.NET Core 2.0 Identity uses EF Core to interact with the database storing the authentication data.</span></span> <span data-ttu-id="bd4ca-132">Aby nowo utworzoną aplikację, aby działać musi istnieć bazę danych do przechowywania tych danych.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-132">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="bd4ca-133">Po utworzeniu nowej aplikacji, najszybszym sposobem sprawdź schemat w środowisku bazy danych jest do utworzenia bazy danych przy użyciu [migracji programu EF Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="bd4ca-133">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using [EF Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="bd4ca-134">Ten proces tworzy bazę danych, albo lokalnie lub w innych miejscach, która naśladuje tego schematu.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-134">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="bd4ca-135">Przejrzyj dokumentację poprzedni, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-135">Review the preceding documentation for more information.</span></span>

    <span data-ttu-id="bd4ca-136">EF Core polecenia użyj parametrów połączenia dla bazy danych określonej w *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-136">EF Core commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="bd4ca-137">Następujący ciąg połączenia jest przeznaczony dla bazy danych na *localhost* o nazwie *asp-net-core-identity*.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="bd4ca-138">W tym ustawieniu programu EF Core jest skonfigurowany do używania `DefaultConnection` parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-138">In this setting, EF Core is configured to use the `DefaultConnection` connection string.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. <span data-ttu-id="bd4ca-139">Wybierz **widoku** > **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-139">Select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="bd4ca-140">Rozwiń węzeł odpowiadający nazwie bazy danych określonej w `ConnectionStrings:DefaultConnection` właściwość *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-140">Expand the node corresponding to the database name specified in the `ConnectionStrings:DefaultConnection` property of *appsettings.json*.</span></span>

    <span data-ttu-id="bd4ca-141">`Update-Database` Polecenia utworzona baza danych określona ze schematem i wszystkie dane potrzebne do inicjowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-141">The `Update-Database` command created the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="bd4ca-142">Poniższa ilustracja przedstawia strukturę tabeli, który jest tworzony za pomocą powyższych kroków.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-142">The following image depicts the table structure that's created with the preceding steps.</span></span>

    ![Tabele tożsamości](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="bd4ca-144">Migrowanie schematu</span><span class="sxs-lookup"><span data-stu-id="bd4ca-144">Migrate the schema</span></span>

<span data-ttu-id="bd4ca-145">Istnieją drobne różnice w strukturę tabeli i pola dla członkostwa i tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-145">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="bd4ca-146">Wzorzec zmienił się znacząco dla uwierzytelniania/autoryzacji za pomocą aplikacji ASP.NET i ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-146">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="bd4ca-147">Obiekty kluczy, które są nadal używane przy użyciu tożsamości są *użytkowników* i *role*.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-147">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="bd4ca-148">Poniżej przedstawiono mapowanie tabel dla *użytkowników*, *role*, i *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-148">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="bd4ca-149">Użytkownicy</span><span class="sxs-lookup"><span data-stu-id="bd4ca-149">Users</span></span>

|<span data-ttu-id="bd4ca-150">*Identity<br>(dbo.AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="bd4ca-150">*Identity<br>(dbo.AspNetUsers)*</span></span>        ||<span data-ttu-id="bd4ca-151">*Członkostwo<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="bd4ca-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span></span>||
|----------------------------------------|-----------------------------------------------------------|
|<span data-ttu-id="bd4ca-152">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-152">**Field Name**</span></span>                 |<span data-ttu-id="bd4ca-153">**Typ**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-153">**Type**</span></span>|<span data-ttu-id="bd4ca-154">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-154">**Field Name**</span></span>                                    |<span data-ttu-id="bd4ca-155">**Typ**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-155">**Type**</span></span>|
|`Id`                           |<span data-ttu-id="bd4ca-156">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-156">string</span></span>  |`aspnet_Users.UserId`                             |<span data-ttu-id="bd4ca-157">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-157">string</span></span>  |
|`UserName`                     |<span data-ttu-id="bd4ca-158">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-158">string</span></span>  |`aspnet_Users.UserName`                           |<span data-ttu-id="bd4ca-159">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-159">string</span></span>  |
|`Email`                        |<span data-ttu-id="bd4ca-160">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-160">string</span></span>  |`aspnet_Membership.Email`                         |<span data-ttu-id="bd4ca-161">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-161">string</span></span>  |
|`NormalizedUserName`           |<span data-ttu-id="bd4ca-162">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-162">string</span></span>  |`aspnet_Users.LoweredUserName`                    |<span data-ttu-id="bd4ca-163">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-163">string</span></span>  |
|`NormalizedEmail`              |<span data-ttu-id="bd4ca-164">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-164">string</span></span>  |`aspnet_Membership.LoweredEmail`                  |<span data-ttu-id="bd4ca-165">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-165">string</span></span>  |
|`PhoneNumber`                  |<span data-ttu-id="bd4ca-166">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-166">string</span></span>  |`aspnet_Users.MobileAlias`                        |<span data-ttu-id="bd4ca-167">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-167">string</span></span>  |
|`LockoutEnabled`               |<span data-ttu-id="bd4ca-168">bit</span><span class="sxs-lookup"><span data-stu-id="bd4ca-168">bit</span></span>     |`aspnet_Membership.IsLockedOut`                   |<span data-ttu-id="bd4ca-169">bit</span><span class="sxs-lookup"><span data-stu-id="bd4ca-169">bit</span></span>     |

> [!NOTE]
> <span data-ttu-id="bd4ca-170">Nie wszystkie mapowania pola przypominają relacji jeden do jednego z członkostwa platformy ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-170">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="bd4ca-171">Zgodnie z poprzednią tabelą przyjmuje domyślny schemat użytkownika członkostwa i mapowany do schematu tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-171">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="bd4ca-172">Inne niestandardowe pola, które były używane dla członkostwa muszą być mapowane ręcznie.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-172">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="bd4ca-173">W takie mapowanie istnieje nebyla nalezena Mapa Pro hasła, kryteria haseł i soli dla hasła nie jest wykonywana migracja między dwoma.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-173">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="bd4ca-174">**Zalecane jest, pozostaw hasło jako wartości null i poproś użytkowników, resetowania swoich haseł.**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-174">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="bd4ca-175">W produkcie ASP.NET Core Identity `LockoutEnd` powinna być ustawiona na jakąś datę w przyszłości, jeśli użytkownik jest zablokowany. Jest to pokazane w skrypcie migracji.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-175">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="bd4ca-176">Role</span><span class="sxs-lookup"><span data-stu-id="bd4ca-176">Roles</span></span>

|<span data-ttu-id="bd4ca-177">*Tożsamość<br>(dbo. AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="bd4ca-177">*Identity<br>(dbo.AspNetRoles)*</span></span>        ||<span data-ttu-id="bd4ca-178">*Członkostwo<br>(dbo.aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="bd4ca-178">*Membership<br>(dbo.aspnet_Roles)*</span></span>||
|----------------------------------------|-----------------------------------|
|<span data-ttu-id="bd4ca-179">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-179">**Field Name**</span></span>                 |<span data-ttu-id="bd4ca-180">**Typ**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-180">**Type**</span></span>|<span data-ttu-id="bd4ca-181">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-181">**Field Name**</span></span>   |<span data-ttu-id="bd4ca-182">**Typ**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-182">**Type**</span></span>         |
|`Id`                           |<span data-ttu-id="bd4ca-183">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-183">string</span></span>  |`RoleId`         | <span data-ttu-id="bd4ca-184">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-184">string</span></span>          |
|`Name`                         |<span data-ttu-id="bd4ca-185">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-185">string</span></span>  |`RoleName`       | <span data-ttu-id="bd4ca-186">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-186">string</span></span>          |
|`NormalizedName`               |<span data-ttu-id="bd4ca-187">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-187">string</span></span>  |`LoweredRoleName`| <span data-ttu-id="bd4ca-188">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-188">string</span></span>          |

### <a name="user-roles"></a><span data-ttu-id="bd4ca-189">Role użytkowników</span><span class="sxs-lookup"><span data-stu-id="bd4ca-189">User Roles</span></span>

|<span data-ttu-id="bd4ca-190">*Tożsamość<br>(dbo. AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="bd4ca-190">*Identity<br>(dbo.AspNetUserRoles)*</span></span>||<span data-ttu-id="bd4ca-191">*Członkostwo<br>(dbo.aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="bd4ca-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span></span>||
|------------------------------------|------------------------------------------|
|<span data-ttu-id="bd4ca-192">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-192">**Field Name**</span></span>           |<span data-ttu-id="bd4ca-193">**Typ**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-193">**Type**</span></span>  |<span data-ttu-id="bd4ca-194">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-194">**Field Name**</span></span>|<span data-ttu-id="bd4ca-195">**Typ**</span><span class="sxs-lookup"><span data-stu-id="bd4ca-195">**Type**</span></span>                   |
|`RoleId`                 |<span data-ttu-id="bd4ca-196">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-196">string</span></span>    |`RoleId`      |<span data-ttu-id="bd4ca-197">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-197">string</span></span>                     |
|`UserId`                 |<span data-ttu-id="bd4ca-198">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-198">string</span></span>    |`UserId`      |<span data-ttu-id="bd4ca-199">string</span><span class="sxs-lookup"><span data-stu-id="bd4ca-199">string</span></span>                     |

<span data-ttu-id="bd4ca-200">Odwołuj się do poprzednich tabel mapowania podczas tworzenia skryptu migracji dla *użytkowników* i *role*.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-200">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="bd4ca-201">W poniższym przykładzie założono, że masz dwie bazy danych na serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-201">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="bd4ca-202">Jedna baza danych zawiera istniejącego członkostwa ASP.NET schematu i danych.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-202">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="bd4ca-203">Druga *CoreIdentitySample* baza danych została utworzona za pomocą procedury opisanej wcześniej.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-203">The other *CoreIdentitySample* database was created using steps described earlier.</span></span> <span data-ttu-id="bd4ca-204">Komentarze są wbudowane, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-204">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
USE aspnetdb

-- INSERT USERS
INSERT INTO CoreIdentitySample.dbo.AspNetUsers
            (Id,
             UserName,
             NormalizedUserName,
             PasswordHash,
             SecurityStamp,
             EmailConfirmed,
             PhoneNumber,
             PhoneNumberConfirmed,
             TwoFactorEnabled,
             LockoutEnd,
             LockoutEnabled,
             AccessFailedCount,
             Email,
             NormalizedEmail)
SELECT aspnet_Users.UserId,
       aspnet_Users.UserName,
       -- The NormalizedUserName value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Users.UserName),
       -- Creates an empty password since passwords don't map between the 2 schemas
       '',
       /*
        The SecurityStamp token is used to verify the state of an account and
        is subject to change at any time. It should be initialized as a new ID.
       */
       NewID(),
       /*
        EmailConfirmed is set when a new user is created and confirmed via email.
        Users must have this set during migration to reset passwords.
       */
       1,
       aspnet_Users.MobileAlias,
       CASE
         WHEN aspnet_Users.MobileAlias IS NULL THEN 0
         ELSE 1
       END,
       -- 2FA likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         -- Setting lockout date to time in the future (1,000 years)
         WHEN aspnet_Membership.IsLockedOut = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_Membership.IsLockedOut,
       /*
        AccessFailedAccount is used to track failed logins. This is stored in
        Membership in multiple columns. Setting to 0 arbitrarily.
       */
       0,
       aspnet_Membership.Email,
       -- The NormalizedEmail value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Membership.Email)
FROM   aspnet_Users
       LEFT OUTER JOIN aspnet_Membership
                    ON aspnet_Membership.ApplicationId =
                       aspnet_Users.ApplicationId
                       AND aspnet_Users.UserId = aspnet_Membership.UserId
       LEFT OUTER JOIN CoreIdentitySample.dbo.AspNetUsers
                    ON aspnet_Membership.UserId = AspNetUsers.Id
WHERE  AspNetUsers.Id IS NULL

-- INSERT ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetRoles(Id, Name)
SELECT RoleId, RoleName
FROM aspnet_Roles;

-- INSERT USER ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetUserRoles(UserId, RoleId)
SELECT UserId, RoleId
FROM aspnet_UsersInRoles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="bd4ca-205">Po zakończeniu poprzedniego skryptu utworzonej wcześniej aplikacji platformy ASP.NET Core Identity jest wypełniana użytkowników członkostwa.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-205">After completion of the preceding script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="bd4ca-206">Użytkownicy muszą zmienić swoje hasła przed zalogowaniem się.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-206">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="bd4ca-207">Jeśli system członkostwa użytkowników za pomocą nazwy użytkownika, które nie odpowiada ich adresowi e-mail, zmiany są wymagane do aplikację utworzoną wcześniej, aby to umożliwić.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-207">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="bd4ca-208">Domyślny szablon oczekuje `UserName` i `Email` być takie same.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-208">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="bd4ca-209">W sytuacjach, w których są one różne, proces logowania musi zostać zmodyfikowana, aby użyć `UserName` zamiast `Email`.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-209">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="bd4ca-210">W `PageModel` strony logowania, znajdujący się w *Pages\Account\Login.cshtml.cs*, Usuń `[EmailAddress]` atrybut z *E-mail* właściwości.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-210">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="bd4ca-211">Zmień jej nazwę na *UserName*.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-211">Rename it to *UserName*.</span></span> <span data-ttu-id="bd4ca-212">Ta migracja wymaga zmiany wszędzie tam, gdzie `EmailAddress` jest wymieniony w *widoku* i *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-212">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="bd4ca-213">Wynik wygląda podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="bd4ca-213">The result looks like the following:</span></span>

 ![Naprawiono logowania](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="bd4ca-215">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="bd4ca-215">Next steps</span></span>

<span data-ttu-id="bd4ca-216">W tym samouczku przedstawiono sposób portu użytkowników z członkostwa SQL do platformy ASP.NET Core 2.0 tożsamości.</span><span class="sxs-lookup"><span data-stu-id="bd4ca-216">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="bd4ca-217">Aby uzyskać więcej informacji na temat tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="bd4ca-217">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
