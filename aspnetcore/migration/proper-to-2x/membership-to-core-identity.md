---
title: Migracja z uwierzytelniania członkostwa ASP.NET do platformy ASP.NET Core 2.0 tożsamości
author: isaac2004
description: Dowiedz się, jak przeprowadzić migrację istniejących aplikacji ASP.NET przy użyciu uwierzytelniania członkostwa platformy ASP.NET Core 2.0 tożsamości.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="77c4f-103">Migracja z uwierzytelniania członkostwa ASP.NET do platformy ASP.NET Core 2.0 tożsamości</span><span class="sxs-lookup"><span data-stu-id="77c4f-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="77c4f-104">Przez [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="77c4f-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="77c4f-105">W tym artykule przedstawiono migracji schematu bazy danych dla aplikacji ASP.NET przy użyciu uwierzytelniania członkostwa platformy ASP.NET Core 2.0 tożsamości.</span><span class="sxs-lookup"><span data-stu-id="77c4f-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="77c4f-106">Ten dokument zawiera kroki niezbędne do migracji schematu bazy danych dla aplikacji opartych na członkostwa ASP.NET do schematu bazy danych używane dla tożsamości ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77c4f-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="77c4f-107">Aby uzyskać więcej informacji na temat migracji z uwierzytelnianie oparte na członkostwie w ASP.NET do tożsamości platformy ASP.NET, zobacz [migracji istniejącej aplikacji z członkostwa SQL do ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="77c4f-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="77c4f-108">Aby uzyskać więcej informacji dotyczących tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości na platformy ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="77c4f-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="77c4f-109">Przegląd schematu członkostwa</span><span class="sxs-lookup"><span data-stu-id="77c4f-109">Review of Membership schema</span></span>

<span data-ttu-id="77c4f-110">Przed składnika ASP.NET 2.0 deweloperzy zostały zlecił mu cały proces uwierzytelniania i autoryzacji dla aplikacji do ich tworzenia.</span><span class="sxs-lookup"><span data-stu-id="77c4f-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="77c4f-111">Z programem ASP.NET 2.0 została wprowadzona członkostwa, umożliwiającego rozwiązanie do obsługi zabezpieczeń aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="77c4f-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="77c4f-112">Deweloperzy było teraz bootstrap schematu w bazie danych programu SQL Server z [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) polecenia.</span><span class="sxs-lookup"><span data-stu-id="77c4f-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="77c4f-113">Po uruchomieniu tego polecenia, poniższych tabelach zostały utworzone w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="77c4f-113">After running this command, the following tables were created in the database.</span></span>

  ![Tabele członkostwa](identity/_static/membership-tables.png)

<span data-ttu-id="77c4f-115">Aby przeprowadzić migrację istniejących aplikacji do platformy ASP.NET Core 2.0 tożsamości, dane w tych tabelach wymaga migracji do tabel używanych przez nowy schemat tożsamości.</span><span class="sxs-lookup"><span data-stu-id="77c4f-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="77c4f-116">Schemat programu ASP.NET Core Identity 2.0</span><span class="sxs-lookup"><span data-stu-id="77c4f-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="77c4f-117">Następuje platformy ASP.NET Core 2.0 [tożsamości](/aspnet/identity/index) zasady wprowadzone w programie ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="77c4f-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="77c4f-118">Chociaż zasady jest udostępniana, implementacja między struktury jest różna, nawet wersji platformy ASP.NET Core (zobacz [migracji do składnika ASP.NET 2.0 podstawowe uwierzytelnianie i tożsamość](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="77c4f-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="77c4f-119">Jest to najszybszy sposób, aby wyświetlić schemat dla platformy ASP.NET Core 2.0 tożsamości do utworzenia nowej aplikacji platformy ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="77c4f-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="77c4f-120">Wykonaj następujące czynności w programie Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="77c4f-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="77c4f-121">Select **File** > **New** > **Project**.</span><span class="sxs-lookup"><span data-stu-id="77c4f-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="77c4f-122">Utwórz nową **aplikacji sieci Web platformy ASP.NET Core**i nazwij projekt *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="77c4f-122">Create a new **ASP.NET Core Web Application**, and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="77c4f-123">Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="77c4f-123">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span> <span data-ttu-id="77c4f-124">Ten szablon tworzy [stron Razor](xref:mvc/razor-pages/index) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77c4f-124">This template produces a [Razor Pages](xref:mvc/razor-pages/index) app.</span></span> <span data-ttu-id="77c4f-125">Przed kliknięciem przycisku **OK**, kliknij przycisk **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="77c4f-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="77c4f-126">Wybierz **indywidualnych kont użytkowników** dla szablonów tożsamości.</span><span class="sxs-lookup"><span data-stu-id="77c4f-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="77c4f-127">Na koniec kliknij **OK**, następnie **OK**.</span><span class="sxs-lookup"><span data-stu-id="77c4f-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="77c4f-128">Visual Studio tworzy projekt przy użyciu szablonu platformy ASP.NET Identity Core.</span><span class="sxs-lookup"><span data-stu-id="77c4f-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="77c4f-129">Używa tożsamości platformy ASP.NET Core 2.0 [Entity Framework Core](/ef/core) do interakcji z bazą danych z danymi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="77c4f-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="77c4f-130">Aby dla nowo utworzonej aplikacji do pracy musi istnieć bazy danych do przechowywania tych danych.</span><span class="sxs-lookup"><span data-stu-id="77c4f-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="77c4f-131">Po utworzeniu nowej aplikacji, to najszybszy sposób sprawdzić schematu w środowisku bazy danych jest utworzenie bazy danych przy użyciu programu Entity Framework migracji.</span><span class="sxs-lookup"><span data-stu-id="77c4f-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="77c4f-132">Ten proces tworzy bazę danych, albo lokalnie lub w innych miejscach, która symuluje tego schematu.</span><span class="sxs-lookup"><span data-stu-id="77c4f-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="77c4f-133">Zapoznaj się z dokumentacją poprzedniego, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="77c4f-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="77c4f-134">Aby utworzyć bazę danych ze schematem ASP.NET Core Identity, uruchom `Update-Database` polecenia w programie Visual Studio **Konsola Menedżera pakietów** okna (PMC)&mdash;znajduje się w **narzędzia**  >  **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="77c4f-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="77c4f-135">PMC obsługuje uruchomionych poleceń programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="77c4f-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="77c4f-136">Entity Framework polecenia użyj parametrów połączenia dla bazy danych określonej w *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="77c4f-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="77c4f-137">Następujący ciąg połączenia bazy danych jest przeznaczony na *localhost* o nazwie *asp-net-core-identity*.</span><span class="sxs-lookup"><span data-stu-id="77c4f-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="77c4f-138">W tym ustawieniu Entity Framework jest skonfigurowana do używania `DefaultConnection` parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="77c4f-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="77c4f-139">To polecenie tworzy określony za pomocą schematu bazy danych i wszystkie dane potrzebne do inicjowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77c4f-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="77c4f-140">Poniższa ilustracja przedstawia struktury tabeli, która zostanie utworzona z powyższych kroków.</span><span class="sxs-lookup"><span data-stu-id="77c4f-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Tabele tożsamości](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="77c4f-142">Migrowanie schematu</span><span class="sxs-lookup"><span data-stu-id="77c4f-142">Migrate the schema</span></span>

<span data-ttu-id="77c4f-143">Brak niewielkie różnice w polach dla członkostwa i ASP.NET Core Identity i struktury tabeli.</span><span class="sxs-lookup"><span data-stu-id="77c4f-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="77c4f-144">Wzorzec zmienił znacząco dla uwierzytelniania/autoryzacji dla aplikacji ASP.NET i ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="77c4f-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="77c4f-145">Klucza obiekty, które są nadal używane z tożsamością są *użytkowników* i *ról*.</span><span class="sxs-lookup"><span data-stu-id="77c4f-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="77c4f-146">Poniżej przedstawiono tabele mapowania dla *użytkowników*, *ról*, i *roli użytkownika*.</span><span class="sxs-lookup"><span data-stu-id="77c4f-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="77c4f-147">Użytkownicy</span><span class="sxs-lookup"><span data-stu-id="77c4f-147">Users</span></span>

| <span data-ttu-id="77c4f-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="77c4f-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="77c4f-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="77c4f-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="77c4f-150">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="77c4f-150">**Field Name**</span></span> | <span data-ttu-id="77c4f-151">**Typ**</span><span class="sxs-lookup"><span data-stu-id="77c4f-151">**Type**</span></span>  |   <span data-ttu-id="77c4f-152">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="77c4f-152">**Field Name**</span></span> | <span data-ttu-id="77c4f-153">**Typ**</span><span class="sxs-lookup"><span data-stu-id="77c4f-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="77c4f-154">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="77c4f-155">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-155">string</span></span>
|`UserName` | <span data-ttu-id="77c4f-156">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="77c4f-157">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-157">string</span></span>
|`Email` | <span data-ttu-id="77c4f-158">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="77c4f-159">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="77c4f-160">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="77c4f-161">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="77c4f-162">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="77c4f-163">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="77c4f-164">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="77c4f-165">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="77c4f-166">bit</span><span class="sxs-lookup"><span data-stu-id="77c4f-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="77c4f-167">bit</span><span class="sxs-lookup"><span data-stu-id="77c4f-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="77c4f-168">Nie wszystkie mapowania pól przypominać relacje jeden do jednego z członkostwa ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="77c4f-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="77c4f-169">Zgodnie z poprzednią tabelą ma domyślny schemat użytkownika członkostwa i mapuje go do schematu ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="77c4f-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="77c4f-170">Wszelkie inne pola niestandardowe, które były używane do członkostwa muszą być mapowane ręcznie.</span><span class="sxs-lookup"><span data-stu-id="77c4f-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="77c4f-171">W to mapowanie nie ma Brak mapy dla hasła, ponieważ zarówno kryteria haseł i soli hasła nie jest wykonywana migracja między dwoma.</span><span class="sxs-lookup"><span data-stu-id="77c4f-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="77c4f-172">**Zalecane jest, pozostaw hasło jako null i poproś użytkowników o resetowanie haseł.**</span><span class="sxs-lookup"><span data-stu-id="77c4f-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="77c4f-173">W produkcie ASP.NET Identity Core `LockoutEnd` powinna być ustawiona na jakąś datę w przyszłości, jeśli użytkownik jest zablokowany. Przedstawiono to w skryptu migracji.</span><span class="sxs-lookup"><span data-stu-id="77c4f-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="77c4f-174">Role</span><span class="sxs-lookup"><span data-stu-id="77c4f-174">Roles</span></span>

| <span data-ttu-id="77c4f-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="77c4f-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="77c4f-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="77c4f-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="77c4f-177">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="77c4f-177">**Field Name**</span></span> | <span data-ttu-id="77c4f-178">**Typ**</span><span class="sxs-lookup"><span data-stu-id="77c4f-178">**Type**</span></span>  |   <span data-ttu-id="77c4f-179">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="77c4f-179">**Field Name**</span></span> | <span data-ttu-id="77c4f-180">**Typ**</span><span class="sxs-lookup"><span data-stu-id="77c4f-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="77c4f-181">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-181">string</span></span> | `RoleId` | <span data-ttu-id="77c4f-182">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-182">string</span></span>
|`Name` | <span data-ttu-id="77c4f-183">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-183">string</span></span> | `RoleName` | <span data-ttu-id="77c4f-184">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="77c4f-185">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="77c4f-186">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="77c4f-187">Role użytkowników</span><span class="sxs-lookup"><span data-stu-id="77c4f-187">User Roles</span></span>

| <span data-ttu-id="77c4f-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="77c4f-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="77c4f-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="77c4f-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="77c4f-190">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="77c4f-190">**Field Name**</span></span> | <span data-ttu-id="77c4f-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="77c4f-191">**Type**</span></span>  |   <span data-ttu-id="77c4f-192">**Nazwa pola**</span><span class="sxs-lookup"><span data-stu-id="77c4f-192">**Field Name**</span></span> | <span data-ttu-id="77c4f-193">**Typ**</span><span class="sxs-lookup"><span data-stu-id="77c4f-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="77c4f-194">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-194">string</span></span> | `RoleId` | <span data-ttu-id="77c4f-195">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-195">string</span></span>
|`UserId` | <span data-ttu-id="77c4f-196">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-196">string</span></span> | `UserId` | <span data-ttu-id="77c4f-197">string</span><span class="sxs-lookup"><span data-stu-id="77c4f-197">string</span></span>

<span data-ttu-id="77c4f-198">Poprzedni tabele mapowania odwołań, podczas tworzenia skryptu migracji dla *użytkowników* i *ról*.</span><span class="sxs-lookup"><span data-stu-id="77c4f-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="77c4f-199">W poniższym przykładzie założono, że masz dwie bazy danych na serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="77c4f-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="77c4f-200">Jedna baza danych zawiera istniejącego schematu członkostwa ASP.NET i danych.</span><span class="sxs-lookup"><span data-stu-id="77c4f-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="77c4f-201">Baza danych została utworzona przy użyciu kroków opisanych wcześniej.</span><span class="sxs-lookup"><span data-stu-id="77c4f-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="77c4f-202">Komentarze są uwzględniane wbudowanego więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="77c4f-202">Comments are included inline for more details.</span></span>

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
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
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

<span data-ttu-id="77c4f-203">Po zakończeniu tego skryptu aplikacja ASP.NET Core Identity utworzony wcześniej jest wypełniana użytkowników członkostwa.</span><span class="sxs-lookup"><span data-stu-id="77c4f-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="77c4f-204">Użytkownicy musieli zmienić swoje hasła przed zalogowaniem się.</span><span class="sxs-lookup"><span data-stu-id="77c4f-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="77c4f-205">Czy system członkostwa były użytkowników z nazwami użytkowników, które nie odpowiadają swój adres e-mail, są wymagane do aplikacji utworzone wcześniej, aby zmieścił się w tym zmiany.</span><span class="sxs-lookup"><span data-stu-id="77c4f-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="77c4f-206">Domyślny szablon oczekuje `UserName` i `Email` być takie same.</span><span class="sxs-lookup"><span data-stu-id="77c4f-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="77c4f-207">W sytuacjach, w których są one różne proces logowania wymaga modyfikacji w celu użycia `UserName` zamiast `Email`.</span><span class="sxs-lookup"><span data-stu-id="77c4f-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="77c4f-208">W `PageModel` strony logowania, znajdujący się w *Pages\Account\Login.cshtml.cs*, Usuń `[EmailAddress]` atrybutu z *E-mail* właściwości.</span><span class="sxs-lookup"><span data-stu-id="77c4f-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="77c4f-209">Zmienić jego nazwę na *UserName*.</span><span class="sxs-lookup"><span data-stu-id="77c4f-209">Rename it to *UserName*.</span></span> <span data-ttu-id="77c4f-210">Wymaga to zmianę wszędzie tam, gdzie `EmailAddress` jest wymieniony w *widoku* i *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="77c4f-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="77c4f-211">Wynik wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="77c4f-211">The result looks like the following:</span></span>

 ![Stałe logowania](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="77c4f-213">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="77c4f-213">Next steps</span></span>

<span data-ttu-id="77c4f-214">W tym samouczku przedstawiono sposób portu użytkowników z członkostwa SQL do platformy ASP.NET Core 2.0 tożsamości.</span><span class="sxs-lookup"><span data-stu-id="77c4f-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="77c4f-215">Aby uzyskać więcej informacji dotyczących tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="77c4f-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
