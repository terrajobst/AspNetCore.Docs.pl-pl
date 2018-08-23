---
title: Migracja z uwierzytelniania członkostwa ASP.NET do ASP.NET Core 2.0 tożsamości
author: isaac2004
description: Dowiedz się, jak przeprowadzić migrację istniejących aplikacji ASP.NET przy użyciu uwierzytelniania członkostwa platformy ASP.NET Core 2.0 tożsamości.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 82158ec500151a0bb61fb1da55a53684367d9a4e
ms.sourcegitcommit: 2e054638b69f2b14f6d67d9fa3664999172ee1b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2018
ms.locfileid: "41753481"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="f1b6a-103">Migracja z uwierzytelniania członkostwa ASP.NET do ASP.NET Core 2.0 tożsamości</span><span class="sxs-lookup"><span data-stu-id="f1b6a-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="f1b6a-104">Przez [Levin Tomasz](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="f1b6a-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="f1b6a-105">W tym artykule przedstawiono migracji schematu bazy danych dla aplikacji ASP.NET przy użyciu uwierzytelniania członkostwa platformy ASP.NET Core 2.0 tożsamości.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="f1b6a-106">Ten dokument zawiera kroki niezbędne do migracji schematu bazy danych dla aplikacji na podstawie członkostwa ASP.NET do schematu bazy danych używane dla tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="f1b6a-107">Aby uzyskać więcej informacji na temat migracji z uwierzytelniania opartego na członkostwa ASP.NET do produktu ASP.NET Identity, zobacz [Migrowanie istniejących aplikacji z członkostwa SQL do produktu ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="f1b6a-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="f1b6a-108">Aby uzyskać więcej informacji o tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości programu ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="f1b6a-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="f1b6a-109">Przegląd schematu członkostwa</span><span class="sxs-lookup"><span data-stu-id="f1b6a-109">Review of Membership schema</span></span>

<span data-ttu-id="f1b6a-110">Przed ASP.NET 2.0 deweloperzy zostały nadzorowania tworzenia cały proces uwierzytelniania i autoryzacji dla swoich aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="f1b6a-111">Za pomocą programu ASP.NET 2.0 członkostwa została wprowadzona, zapewniając standardowy rozwiązania do obsługi zabezpieczeń w aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="f1b6a-112">Deweloperzy udało się teraz bootstrap schematu do bazy danych programu SQL Server za pomocą [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) polecenia.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="f1b6a-113">Po uruchomieniu tego polecenia, następujące tabele zostały utworzone w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-113">After running this command, the following tables were created in the database.</span></span>

  ![Tabele członkostwa](identity/_static/membership-tables.png)

<span data-ttu-id="f1b6a-115">Aby przeprowadzić migrację istniejących aplikacji do platformy ASP.NET Core 2.0 tożsamości, dane w tabelach musi powinny być migrowane do tabel używanych przez nowy schemat tożsamości.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="f1b6a-116">ASP.NET Core 2.0 tożsamości schematu</span><span class="sxs-lookup"><span data-stu-id="f1b6a-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="f1b6a-117">Platforma ASP.NET Core 2.0 jest zgodna [tożsamości](/aspnet/identity/index) zasady, które wprowadzono w programie ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="f1b6a-118">Jeśli zasady są udostępniane, implementacja między struktury jest różne, nawet w różnych wersjach programu ASP.NET Core (zobacz [migracji, uwierzytelnianie i tożsamość do ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="f1b6a-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="f1b6a-119">Jest to najszybszy sposób, aby wyświetlić schemat dla platformy ASP.NET Core 2.0 tożsamości do utworzenia nowej aplikacji platformy ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="f1b6a-120">Wykonaj następujące kroki w programie Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="f1b6a-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="f1b6a-121">Wybierz **pliku** > **nowe** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="f1b6a-122">Utwórz nową **aplikacji sieci Web programu ASP.NET Core** i nazwij projekt *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-122">Create a new **ASP.NET Core Web Application** and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="f1b6a-123">Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="f1b6a-124">Ten szablon tworzy [stron Razor](xref:razor-pages/index) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="f1b6a-125">Przed kliknięciem przycisku **OK**, kliknij przycisk **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="f1b6a-126">Wybierz **indywidualne konta użytkowników** dla szablonów tożsamości.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="f1b6a-127">Na koniec kliknij **OK**, następnie **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="f1b6a-128">Program Visual Studio tworzy projekt przy użyciu szablonu platformy ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="f1b6a-129">Używa tożsamości platformy ASP.NET Core 2.0 [Entity Framework Core](/ef/core) do interakcji z bazą danych, do przechowywania danych uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="f1b6a-130">Aby nowo utworzoną aplikację, aby działać musi istnieć bazę danych do przechowywania tych danych.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="f1b6a-131">Po utworzeniu nowej aplikacji, to najszybszy sposób sprawdzić schematu w środowisku bazy danych jest utworzenie bazy danych, używając migracje platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="f1b6a-132">Ten proces tworzy bazę danych, albo lokalnie lub w innych miejscach, która naśladuje tego schematu.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="f1b6a-133">Przejrzyj dokumentację poprzedni, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="f1b6a-134">Aby utworzyć bazę danych ze schematem tożsamości platformy ASP.NET Core, uruchom `Update-Database` polecenia w programie Visual Studio **Konsola Menedżera pakietów** okna (PMC)&mdash;znajduje się w **narzędzia**  >  **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="f1b6a-135">Konsolę zarządzania Pakietami obsługuje uruchamianie poleceń platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="f1b6a-136">Entity Framework polecenia użyj parametrów połączenia dla bazy danych określonej w *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="f1b6a-137">Następujący ciąg połączenia jest przeznaczony dla bazy danych na *localhost* o nazwie *asp-net-core-identity*.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="f1b6a-138">W tym ustawieniu Entity Framework jest skonfigurowany do używania `DefaultConnection` parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="f1b6a-139">To polecenie powoduje utworzenie bazy danych określonej ze schematem i wszystkie dane potrzebne do inicjowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="f1b6a-140">Poniższa ilustracja przedstawia strukturę tabeli, który jest tworzony za pomocą powyższych kroków.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Tabele tożsamości](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="f1b6a-142">Migrowanie schematu</span><span class="sxs-lookup"><span data-stu-id="f1b6a-142">Migrate the schema</span></span>

<span data-ttu-id="f1b6a-143">Istnieją drobne różnice w strukturę tabeli i pola dla członkostwa i tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="f1b6a-144">Wzorzec zmienił się znacząco dla uwierzytelniania/autoryzacji za pomocą aplikacji ASP.NET i ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="f1b6a-145">Obiekty kluczy, które są nadal używane przy użyciu tożsamości są *użytkowników* i *role*.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="f1b6a-146">Poniżej przedstawiono mapowanie tabel dla *użytkowników*, *role*, i *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="f1b6a-147">Użytkownicy</span><span class="sxs-lookup"><span data-stu-id="f1b6a-147">Users</span></span>

| <span data-ttu-id="f1b6a-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="f1b6a-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="f1b6a-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="f1b6a-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="f1b6a-150">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-150">**Field Name**</span></span> | <span data-ttu-id="f1b6a-151">**Typ**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-151">**Type**</span></span>  |   <span data-ttu-id="f1b6a-152">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-152">**Field Name**</span></span> | <span data-ttu-id="f1b6a-153">**Typ**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="f1b6a-154">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="f1b6a-155">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-155">string</span></span>
|`UserName` | <span data-ttu-id="f1b6a-156">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="f1b6a-157">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-157">string</span></span>
|`Email` | <span data-ttu-id="f1b6a-158">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="f1b6a-159">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="f1b6a-160">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="f1b6a-161">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="f1b6a-162">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="f1b6a-163">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="f1b6a-164">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="f1b6a-165">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="f1b6a-166">bit</span><span class="sxs-lookup"><span data-stu-id="f1b6a-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="f1b6a-167">bit</span><span class="sxs-lookup"><span data-stu-id="f1b6a-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="f1b6a-168">Nie wszystkie mapowania pola przypominają relacji jeden do jednego z członkostwa platformy ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="f1b6a-169">Zgodnie z poprzednią tabelą przyjmuje domyślny schemat użytkownika członkostwa i mapowany do schematu tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="f1b6a-170">Inne niestandardowe pola, które były używane dla członkostwa muszą być mapowane ręcznie.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="f1b6a-171">W takie mapowanie istnieje nebyla nalezena Mapa Pro hasła, kryteria haseł i soli dla hasła nie jest wykonywana migracja między dwoma.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="f1b6a-172">**Zalecane jest, pozostaw hasło jako wartości null i poproś użytkowników, resetowania swoich haseł.**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="f1b6a-173">W produkcie ASP.NET Core Identity `LockoutEnd` powinna być ustawiona na jakąś datę w przyszłości, jeśli użytkownik jest zablokowany. Jest to pokazane w skrypcie migracji.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="f1b6a-174">Role</span><span class="sxs-lookup"><span data-stu-id="f1b6a-174">Roles</span></span>

| <span data-ttu-id="f1b6a-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="f1b6a-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="f1b6a-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="f1b6a-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="f1b6a-177">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-177">**Field Name**</span></span> | <span data-ttu-id="f1b6a-178">**Typ**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-178">**Type**</span></span>  |   <span data-ttu-id="f1b6a-179">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-179">**Field Name**</span></span> | <span data-ttu-id="f1b6a-180">**Typ**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="f1b6a-181">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-181">string</span></span> | `RoleId` | <span data-ttu-id="f1b6a-182">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-182">string</span></span>
|`Name` | <span data-ttu-id="f1b6a-183">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-183">string</span></span> | `RoleName` | <span data-ttu-id="f1b6a-184">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="f1b6a-185">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="f1b6a-186">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="f1b6a-187">Role użytkowników</span><span class="sxs-lookup"><span data-stu-id="f1b6a-187">User Roles</span></span>

| <span data-ttu-id="f1b6a-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="f1b6a-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="f1b6a-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="f1b6a-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="f1b6a-190">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-190">**Field Name**</span></span> | <span data-ttu-id="f1b6a-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-191">**Type**</span></span>  |   <span data-ttu-id="f1b6a-192">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-192">**Field Name**</span></span> | <span data-ttu-id="f1b6a-193">**Typ**</span><span class="sxs-lookup"><span data-stu-id="f1b6a-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="f1b6a-194">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-194">string</span></span> | `RoleId` | <span data-ttu-id="f1b6a-195">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-195">string</span></span>
|`UserId` | <span data-ttu-id="f1b6a-196">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-196">string</span></span> | `UserId` | <span data-ttu-id="f1b6a-197">string</span><span class="sxs-lookup"><span data-stu-id="f1b6a-197">string</span></span>

<span data-ttu-id="f1b6a-198">Odwołuj się do poprzednich tabel mapowania podczas tworzenia skryptu migracji dla *użytkowników* i *role*.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="f1b6a-199">W poniższym przykładzie założono, że masz dwie bazy danych na serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="f1b6a-200">Jedna baza danych zawiera istniejącego członkostwa ASP.NET schematu i danych.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="f1b6a-201">Inne bazy danych został utworzony za pomocą procedury opisanej wcześniej.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="f1b6a-202">Komentarze są wbudowane, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-202">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be initialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="f1b6a-203">Po zakończeniu tego skryptu utworzonej wcześniej aplikacji platformy ASP.NET Core Identity jest wypełniana użytkowników członkostwa.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="f1b6a-204">Użytkownicy muszą zmienić swoje hasła przed zalogowaniem się.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="f1b6a-205">Jeśli system członkostwa użytkowników za pomocą nazwy użytkownika, które nie odpowiada ich adresowi e-mail, zmiany są wymagane do aplikację utworzoną wcześniej, aby to umożliwić.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="f1b6a-206">Domyślny szablon oczekuje `UserName` i `Email` być takie same.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="f1b6a-207">W sytuacjach, w których są one różne, proces logowania musi zostać zmodyfikowana, aby użyć `UserName` zamiast `Email`.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="f1b6a-208">W `PageModel` strony logowania, znajdujący się w *Pages\Account\Login.cshtml.cs*, Usuń `[EmailAddress]` atrybut z *E-mail* właściwości.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="f1b6a-209">Zmień jej nazwę na *UserName*.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-209">Rename it to *UserName*.</span></span> <span data-ttu-id="f1b6a-210">Ta migracja wymaga zmiany wszędzie tam, gdzie `EmailAddress` jest wymieniony w *widoku* i *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="f1b6a-211">Wynik wygląda podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="f1b6a-211">The result looks like the following:</span></span>

 ![Naprawiono logowania](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="f1b6a-213">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="f1b6a-213">Next steps</span></span>

<span data-ttu-id="f1b6a-214">W tym samouczku przedstawiono sposób portu użytkowników z członkostwa SQL do platformy ASP.NET Core 2.0 tożsamości.</span><span class="sxs-lookup"><span data-stu-id="f1b6a-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="f1b6a-215">Aby uzyskać więcej informacji na temat tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="f1b6a-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
