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
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Migracja z uwierzytelniania członkostwa ASP.NET do platformy ASP.NET Core 2.0 tożsamości

Przez [Isaac Levin](https://isaaclevin.com)

W tym artykule przedstawiono migracji schematu bazy danych dla aplikacji ASP.NET przy użyciu uwierzytelniania członkostwa platformy ASP.NET Core 2.0 tożsamości.

> [!NOTE]
> Ten dokument zawiera kroki niezbędne do migracji schematu bazy danych dla aplikacji opartych na członkostwa ASP.NET do schematu bazy danych używane dla tożsamości ASP.NET Core. Aby uzyskać więcej informacji na temat migracji z uwierzytelnianie oparte na członkostwie w ASP.NET do tożsamości platformy ASP.NET, zobacz [migracji istniejącej aplikacji z członkostwa SQL do ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Aby uzyskać więcej informacji dotyczących tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości na platformy ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Przegląd schematu członkostwa

Przed składnika ASP.NET 2.0 deweloperzy zostały zlecił mu cały proces uwierzytelniania i autoryzacji dla aplikacji do ich tworzenia. Z programem ASP.NET 2.0 została wprowadzona członkostwa, umożliwiającego rozwiązanie do obsługi zabezpieczeń aplikacji ASP.NET. Deweloperzy było teraz bootstrap schematu w bazie danych programu SQL Server z [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) polecenia. Po uruchomieniu tego polecenia, poniższych tabelach zostały utworzone w bazie danych.

  ![Tabele członkostwa](identity/_static/membership-tables.png)

Aby przeprowadzić migrację istniejących aplikacji do platformy ASP.NET Core 2.0 tożsamości, dane w tych tabelach wymaga migracji do tabel używanych przez nowy schemat tożsamości.

## <a name="aspnet-core-identity-20-schema"></a>Schemat programu ASP.NET Core Identity 2.0

Następuje platformy ASP.NET Core 2.0 [tożsamości](/aspnet/identity/index) zasady wprowadzone w programie ASP.NET 4.5. Chociaż zasady jest udostępniana, implementacja między struktury jest różna, nawet wersji platformy ASP.NET Core (zobacz [migracji do składnika ASP.NET 2.0 podstawowe uwierzytelnianie i tożsamość](xref:migration/1x-to-2x/index)).

Jest to najszybszy sposób, aby wyświetlić schemat dla platformy ASP.NET Core 2.0 tożsamości do utworzenia nowej aplikacji platformy ASP.NET Core 2.0. Wykonaj następujące czynności w programie Visual Studio 2017:

* Select **File** > **New** > **Project**.
* Utwórz nową **aplikacji sieci Web platformy ASP.NET Core**i nazwij projekt *CoreIdentitySample*.
* Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**. Ten szablon tworzy [stron Razor](xref:mvc/razor-pages/index) aplikacji. Przed kliknięciem przycisku **OK**, kliknij przycisk **Zmień uwierzytelnianie**.
* Wybierz **indywidualnych kont użytkowników** dla szablonów tożsamości. Na koniec kliknij **OK**, następnie **OK**. Visual Studio tworzy projekt przy użyciu szablonu platformy ASP.NET Identity Core.

Używa tożsamości platformy ASP.NET Core 2.0 [Entity Framework Core](/ef/core) do interakcji z bazą danych z danymi uwierzytelniania. Aby dla nowo utworzonej aplikacji do pracy musi istnieć bazy danych do przechowywania tych danych. Po utworzeniu nowej aplikacji, to najszybszy sposób sprawdzić schematu w środowisku bazy danych jest utworzenie bazy danych przy użyciu programu Entity Framework migracji. Ten proces tworzy bazę danych, albo lokalnie lub w innych miejscach, która symuluje tego schematu. Zapoznaj się z dokumentacją poprzedniego, aby uzyskać więcej informacji.

Aby utworzyć bazę danych ze schematem ASP.NET Core Identity, uruchom `Update-Database` polecenia w programie Visual Studio **Konsola Menedżera pakietów** okna (PMC)&mdash;znajduje się w **narzędzia**  >  **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**. PMC obsługuje uruchomionych poleceń programu Entity Framework.

Entity Framework polecenia użyj parametrów połączenia dla bazy danych określonej w *appsettings.json*. Następujący ciąg połączenia bazy danych jest przeznaczony na *localhost* o nazwie *asp-net-core-identity*. W tym ustawieniu Entity Framework jest skonfigurowana do używania `DefaultConnection` parametry połączenia.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

To polecenie tworzy określony za pomocą schematu bazy danych i wszystkie dane potrzebne do inicjowania aplikacji. Poniższa ilustracja przedstawia struktury tabeli, która zostanie utworzona z powyższych kroków.

   ![Tabele tożsamości](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Migrowanie schematu

Brak niewielkie różnice w polach dla członkostwa i ASP.NET Core Identity i struktury tabeli. Wzorzec zmienił znacząco dla uwierzytelniania/autoryzacji dla aplikacji ASP.NET i ASP.NET Core. Klucza obiekty, które są nadal używane z tożsamością są *użytkowników* i *ról*. Poniżej przedstawiono tabele mapowania dla *użytkowników*, *ról*, i *roli użytkownika*.

### <a name="users"></a>Użytkownicy

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **Nazwa pola** | **Typ**  |   **Nazwa pola** | **Typ**  |
|`Id` | string | `aspnet_Users.UserId` | string
|`UserName` | string | `aspnet_Users.UserName` | string
|`Email` | string | `aspnet_Membership.Email` | string
|`NormalizedUserName` | string | `aspnet_Users.LoweredUserName` | string
|`NormalizedEmail` | string | `aspnet_Membership.LoweredEmail` | string
|`PhoneNumber` | string | `aspnet_Users.MobileAlias` | string
|`LockoutEnabled` | bit | `aspnet_Membership.IsLockedOut` | bit

> [!NOTE]
> Nie wszystkie mapowania pól przypominać relacje jeden do jednego z członkostwa ASP.NET Core Identity. Zgodnie z poprzednią tabelą ma domyślny schemat użytkownika członkostwa i mapuje go do schematu ASP.NET Core Identity. Wszelkie inne pola niestandardowe, które były używane do członkostwa muszą być mapowane ręcznie. W to mapowanie nie ma Brak mapy dla hasła, ponieważ zarówno kryteria haseł i soli hasła nie jest wykonywana migracja między dwoma. **Zalecane jest, pozostaw hasło jako null i poproś użytkowników o resetowanie haseł.** W produkcie ASP.NET Identity Core `LockoutEnd` powinna być ustawiona na jakąś datę w przyszłości, jeśli użytkownik jest zablokowany. Przedstawiono to w skryptu migracji.

### <a name="roles"></a>Role

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **Nazwa pola** | **Typ**  |   **Nazwa pola** | **Typ**  |
|`Id` | string | `RoleId` | string
|`Name` | string | `RoleName` | string
|`NormalizedName` | string | `LoweredRoleName` | string

### <a name="user-roles"></a>Role użytkowników

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **Nazwa pola** | **Typ**  |   **Nazwa pola** | **Typ**  |
|`RoleId` | string | `RoleId` | string
|`UserId` | string | `UserId` | string

Poprzedni tabele mapowania odwołań, podczas tworzenia skryptu migracji dla *użytkowników* i *ról*. W poniższym przykładzie założono, że masz dwie bazy danych na serwerze bazy danych. Jedna baza danych zawiera istniejącego schematu członkostwa ASP.NET i danych. Baza danych została utworzona przy użyciu kroków opisanych wcześniej. Komentarze są uwzględniane wbudowanego więcej szczegółów.

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

Po zakończeniu tego skryptu aplikacja ASP.NET Core Identity utworzony wcześniej jest wypełniana użytkowników członkostwa. Użytkownicy musieli zmienić swoje hasła przed zalogowaniem się.

> [!NOTE]
> Czy system członkostwa były użytkowników z nazwami użytkowników, które nie odpowiadają swój adres e-mail, są wymagane do aplikacji utworzone wcześniej, aby zmieścił się w tym zmiany. Domyślny szablon oczekuje `UserName` i `Email` być takie same. W sytuacjach, w których są one różne proces logowania wymaga modyfikacji w celu użycia `UserName` zamiast `Email`.

W `PageModel` strony logowania, znajdujący się w *Pages\Account\Login.cshtml.cs*, Usuń `[EmailAddress]` atrybutu z *E-mail* właściwości. Zmienić jego nazwę na *UserName*. Wymaga to zmianę wszędzie tam, gdzie `EmailAddress` jest wymieniony w *widoku* i *PageModel*. Wynik wygląda następująco:

 ![Stałe logowania](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób portu użytkowników z członkostwa SQL do platformy ASP.NET Core 2.0 tożsamości. Aby uzyskać więcej informacji dotyczących tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości](xref:security/authentication/identity).
