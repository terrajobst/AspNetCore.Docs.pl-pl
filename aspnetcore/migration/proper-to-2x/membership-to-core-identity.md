---
title: Migracja z uwierzytelniania członkostwa ASP.NET do ASP.NET Core 2.0 tożsamości
author: isaac2004
description: Dowiedz się, jak przeprowadzić migrację istniejących aplikacji ASP.NET przy użyciu uwierzytelniania członkostwa platformy ASP.NET Core 2.0 tożsamości.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 0b7001a311eeaaa78e3d52e2ec66d33ad057c381
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207411"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Migracja z uwierzytelniania członkostwa ASP.NET do ASP.NET Core 2.0 tożsamości

Przez [Levin Tomasz](https://isaaclevin.com)

W tym artykule przedstawiono migracji schematu bazy danych dla aplikacji ASP.NET przy użyciu uwierzytelniania członkostwa platformy ASP.NET Core 2.0 tożsamości.

> [!NOTE]
> Ten dokument zawiera kroki niezbędne do migracji schematu bazy danych dla aplikacji na podstawie członkostwa ASP.NET do schematu bazy danych używane dla tożsamości platformy ASP.NET Core. Aby uzyskać więcej informacji na temat migracji z uwierzytelniania opartego na członkostwa ASP.NET do produktu ASP.NET Identity, zobacz [Migrowanie istniejących aplikacji z członkostwa SQL do produktu ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Aby uzyskać więcej informacji o tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości programu ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Przegląd schematu członkostwa

Przed ASP.NET 2.0 deweloperzy zostały nadzorowania tworzenia cały proces uwierzytelniania i autoryzacji dla swoich aplikacji. Za pomocą programu ASP.NET 2.0 członkostwa została wprowadzona, zapewniając standardowy rozwiązania do obsługi zabezpieczeń w aplikacji ASP.NET. Deweloperzy udało się teraz bootstrap schematu do bazy danych programu SQL Server za pomocą [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) polecenia. Po uruchomieniu tego polecenia, następujące tabele zostały utworzone w bazie danych.

  ![Tabele członkostwa](identity/_static/membership-tables.png)

Aby przeprowadzić migrację istniejących aplikacji do platformy ASP.NET Core 2.0 tożsamości, dane w tabelach musi powinny być migrowane do tabel używanych przez nowy schemat tożsamości.

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core 2.0 tożsamości schematu

Platforma ASP.NET Core 2.0 jest zgodna [tożsamości](/aspnet/identity/index) zasady, które wprowadzono w programie ASP.NET 4.5. Jeśli zasady są udostępniane, implementacja między struktury jest różne, nawet w różnych wersjach programu ASP.NET Core (zobacz [migracji, uwierzytelnianie i tożsamość do ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).

Jest to najszybszy sposób, aby wyświetlić schemat dla platformy ASP.NET Core 2.0 tożsamości do utworzenia nowej aplikacji platformy ASP.NET Core 2.0. Wykonaj następujące kroki w programie Visual Studio 2017:

1. Wybierz **pliku** > **nowe** > **projektu**.
1. Utwórz nową **aplikacji sieci Web programu ASP.NET Core** projektu o nazwie *CoreIdentitySample*.
1. Wybierz **ASP.NET Core 2.0** w listy rozwijanej, a następnie wybierz **aplikacji sieci Web**. Ten szablon tworzy [stron Razor](xref:razor-pages/index) aplikacji. Przed kliknięciem przycisku **OK**, kliknij przycisk **Zmień uwierzytelnianie**.
1. Wybierz **indywidualne konta użytkowników** dla szablonów tożsamości. Na koniec kliknij **OK**, następnie **OK**. Program Visual Studio tworzy projekt przy użyciu szablonu platformy ASP.NET Core Identity.
1. Wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów** otworzyć **Konsola Menedżera pakietów** Okno (PMC).
1. Przejdź do katalogu głównego projektu, w konsoli zarządzania Pakietami i uruchom [Core Entity Framework (EF)](/ef/core) `Update-Database` polecenia.

    Tożsamości platformy ASP.NET Core 2.0 używa programu EF Core do interakcji z bazą danych, do przechowywania danych uwierzytelniania. Aby nowo utworzoną aplikację, aby działać musi istnieć bazę danych do przechowywania tych danych. Po utworzeniu nowej aplikacji, najszybszym sposobem sprawdź schemat w środowisku bazy danych jest do utworzenia bazy danych przy użyciu [migracji programu EF Core](/ef/core/managing-schemas/migrations/). Ten proces tworzy bazę danych, albo lokalnie lub w innych miejscach, która naśladuje tego schematu. Przejrzyj dokumentację poprzedni, aby uzyskać więcej informacji.

    EF Core polecenia użyj parametrów połączenia dla bazy danych określonej w *appsettings.json*. Następujący ciąg połączenia jest przeznaczony dla bazy danych na *localhost* o nazwie *asp-net-core-identity*. W tym ustawieniu programu EF Core jest skonfigurowany do używania `DefaultConnection` parametry połączenia.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```
1. Wybierz **widoku** > **Eksplorator obiektów SQL Server**. Rozwiń węzeł odpowiadający nazwie bazy danych określonej w `ConnectionStrings:DefaultConnection` właściwość *appsettings.json*.

    `Update-Database` Polecenia utworzona baza danych określona ze schematem i wszystkie dane potrzebne do inicjowania aplikacji. Poniższa ilustracja przedstawia strukturę tabeli, który jest tworzony za pomocą powyższych kroków.

    ![Tabele tożsamości](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Migrowanie schematu

Istnieją drobne różnice w strukturę tabeli i pola dla członkostwa i tożsamości platformy ASP.NET Core. Wzorzec zmienił się znacząco dla uwierzytelniania/autoryzacji za pomocą aplikacji ASP.NET i ASP.NET Core. Obiekty kluczy, które są nadal używane przy użyciu tożsamości są *użytkowników* i *role*. Poniżej przedstawiono mapowanie tabel dla *użytkowników*, *role*, i *UserRoles*.

### <a name="users"></a>Użytkownicy

|*Tożsamość<br>(dbo. AspNetUsers)*        ||*Członkostwo<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*||
|----------------------------------------|-----------------------------------------------------------|
|**Nazwa pola**                 |**Typ**|**Nazwa pola**                                    |**Typ**|
|`Id`                           |string  |`aspnet_Users.UserId`                             |string  |
|`UserName`                     |string  |`aspnet_Users.UserName`                           |string  |
|`Email`                        |string  |`aspnet_Membership.Email`                         |string  |
|`NormalizedUserName`           |string  |`aspnet_Users.LoweredUserName`                    |string  |
|`NormalizedEmail`              |string  |`aspnet_Membership.LoweredEmail`                  |string  |
|`PhoneNumber`                  |string  |`aspnet_Users.MobileAlias`                        |string  |
|`LockoutEnabled`               |bit     |`aspnet_Membership.IsLockedOut`                   |bit     |

> [!NOTE]
> Nie wszystkie mapowania pola przypominają relacji jeden do jednego z członkostwa platformy ASP.NET Core Identity. Zgodnie z poprzednią tabelą przyjmuje domyślny schemat użytkownika członkostwa i mapowany do schematu tożsamości platformy ASP.NET Core. Inne niestandardowe pola, które były używane dla członkostwa muszą być mapowane ręcznie. W takie mapowanie istnieje nebyla nalezena Mapa Pro hasła, kryteria haseł i soli dla hasła nie jest wykonywana migracja między dwoma. **Zalecane jest, pozostaw hasło jako wartości null i poproś użytkowników, resetowania swoich haseł.** W produkcie ASP.NET Core Identity `LockoutEnd` powinna być ustawiona na jakąś datę w przyszłości, jeśli użytkownik jest zablokowany. Jest to pokazane w skrypcie migracji.

### <a name="roles"></a>Role

|*Tożsamość<br>(dbo. AspNetRoles)*        ||*Członkostwo<br>(dbo.aspnet_Roles)*||
|----------------------------------------|-----------------------------------|
|**Nazwa pola**                 |**Typ**|**Nazwa pola**   |**Typ**         |
|`Id`                           |string  |`RoleId`         | string          |
|`Name`                         |string  |`RoleName`       | string          |
|`NormalizedName`               |string  |`LoweredRoleName`| string          |

### <a name="user-roles"></a>Role użytkowników

|*Tożsamość<br>(dbo. AspNetUserRoles)*||*Członkostwo<br>(dbo.aspnet_UsersInRoles)*||
|------------------------------------|------------------------------------------|
|**Nazwa pola**           |**Typ**  |**Nazwa pola**|**Typ**                   |
|`RoleId`                 |string    |`RoleId`      |string                     |
|`UserId`                 |string    |`UserId`      |string                     |

Odwołuj się do poprzednich tabel mapowania podczas tworzenia skryptu migracji dla *użytkowników* i *role*. W poniższym przykładzie założono, że masz dwie bazy danych na serwerze bazy danych. Jedna baza danych zawiera istniejącego członkostwa ASP.NET schematu i danych. Druga *CoreIdentitySample* baza danych została utworzona za pomocą procedury opisanej wcześniej. Komentarze są wbudowane, aby uzyskać więcej informacji.

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

Po zakończeniu poprzedniego skryptu utworzonej wcześniej aplikacji platformy ASP.NET Core Identity jest wypełniana użytkowników członkostwa. Użytkownicy muszą zmienić swoje hasła przed zalogowaniem się.

> [!NOTE]
> Jeśli system członkostwa użytkowników za pomocą nazwy użytkownika, które nie odpowiada ich adresowi e-mail, zmiany są wymagane do aplikację utworzoną wcześniej, aby to umożliwić. Domyślny szablon oczekuje `UserName` i `Email` być takie same. W sytuacjach, w których są one różne, proces logowania musi zostać zmodyfikowana, aby użyć `UserName` zamiast `Email`.

W `PageModel` strony logowania, znajdujący się w *Pages\Account\Login.cshtml.cs*, Usuń `[EmailAddress]` atrybut z *E-mail* właściwości. Zmień jej nazwę na *UserName*. Ta migracja wymaga zmiany wszędzie tam, gdzie `EmailAddress` jest wymieniony w *widoku* i *PageModel*. Wynik wygląda podobnie do poniższego:

 ![Naprawiono logowania](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób portu użytkowników z członkostwa SQL do platformy ASP.NET Core 2.0 tożsamości. Aby uzyskać więcej informacji na temat tożsamości platformy ASP.NET Core, zobacz [wprowadzenie do tożsamości](xref:security/authentication/identity).
