---
title: "Konfigurowanie tożsamości platformy ASP.NET Core"
author: AdrienTorris
description: "Zrozumienie ASP.NET Core Identity wartości domyślne i skonfigurować różne właściwości tożsamości, aby użyć niestandardowej wartości."
keywords: "Uwierzytelnianie ASP.NET Core tożsamości, zabezpieczeń"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 2861ca474e7e82da81943966394a92040ce96ab8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="configure-identity"></a>Konfigurowanie tożsamości

Niektóre zachowania domyślnego, które można łatwo zastąpić w aplikacji ma ASP.NET Core Identity `Startup` klasy.

## <a name="passwords-policy"></a>Zasady haseł

Domyślnie tożsamości wymaga haseł zawierających wielką literę, małą literę, cyfrę i znaków innych niż alfanumeryczne. Istnieją także inne ograniczenia dotyczące. Aby uprościć ograniczenia haseł, możesz to zrobić `Startup` klasy aplikacji.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Platformy ASP.NET Core 2.0 dodane `RequiredUniqueChars` właściwości. W przeciwnym razie opcje są takie same z platformy ASP.NET Core 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`ma następujące właściwości:
* `RequireDigit`: Wymaga liczbą z zakresu od 0 do 9 w haśle. Wartość domyślna to true.
* `RequiredLength`: Minimalna długość hasła. Wartość domyślna to 6.
* `RequireNonAlphanumeric`: Wymaga inne niż alfanumeryczne znaków w haśle. Wartość domyślna to true.
* `RequireUppercase`: Wymaga się wielką literą w haśle. Wartość domyślna to true.
* `RequireLowercase`: Wymaga się małą literą w haśle. Wartość domyślna to true.
* `RequiredUniqueChars`: Wymaga liczbę unikatowych znaków w haśle. Wartość domyślna to 1.


## <a name="users-lockout"></a>Blokady użytkownika

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`ma następujące właściwości:
* `DefaultLockoutTimeSpan`: Ilość czasu blokady użytkownika po wystąpieniu blokady. Wartość domyślna to 5 minut.
* `MaxFailedAccessAttempts`: Liczba nieudanych prób uzyskania dostępu do momentu użytkownika jest zablokowane, jeśli funkcja blokady jest włączona. Wartość domyślna to 5.
* `AllowedForNewUsers`: Określa, czy nowego użytkownika można zablokować. Wartość domyślna to true.


## <a name="sign-in-settings"></a>Zaloguj się, ustawienia

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`ma następujące właściwości:
* `RequireConfirmedEmail`: Wymaga potwierdzone poczty e-mail do logowania. Wartość domyślna to false.
* `RequireConfirmedPhoneNumber`: Wymaga numeru telefonu potwierdzone do logowania. Wartość domyślna to false.


## <a name="user-validation-settings"></a>Ustawienia weryfikacji użytkownika

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`ma następujące właściwości:
* `RequireUniqueEmail`: Wymaga unikatowego adresu e-mail każdego użytkownika. Wartość domyślna to false.
* `AllowedUserNameCharacters`: Dozwolonych znaków nazwy użytkownika. Wartość domyślna to abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.

## <a name="applications-cookie-settings"></a>Ustawienia plików cookie aplikacji

Jak zasady haseł, wszystkie ustawienia pliku cookie aplikacji można zmienić w `Startup` klasy.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

W obszarze `ConfigureServices` w `Startup` klasy, można skonfigurować plik cookie aplikacji.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

`CookieAuthenticationOptions`ma następujące właściwości:
* `Cookie.Name`: Nazwa pliku cookie. Wartość domyślna to. AspNetCore.Cookies.
* `Cookie.HttpOnly`: W przypadku wartości true, plik cookie nie jest dostępny ze skryptów po stronie klienta. Wartość domyślna to true.
* `ExpireTimeSpan`: Określa, ile czasu przechowywania biletu uwierzytelniania w pliku cookie pozostanie ważny od punktu, w którym jest tworzona. Wartość domyślna to 14 dni.
* `LoginPath`: Jeśli użytkownik nie ma autoryzacji, zostanie przekierowany do tej ścieżki do logowania. Wartość domyślna to/Account/logowania.
* `LogoutPath`: Jeśli użytkownik jest zalogowany, zostanie przekierowany do tej ścieżki. Wartość domyślna to/Account/wylogowania.
* `AccessDeniedPath`: W przypadku niepowodzenia autoryzacji wyboru użytkownik zostanie przekierowany do tej ścieżki. Wartość domyślna to/Account/AccessDenied.
* `SlidingExpiration`: W przypadku wartości true nowy plik cookie zostanie wystawiony nową godzinę wygaśnięcia po bieżącym plikiem cookie jest przekroczyło połowę okna wygaśnięcia. Wartość domyślna to true.
* `ReturnUrlParameter`: Parametr ReturnUrlParameter Określa nazwę parametru ciągu zapytania, która jest dołączana przez oprogramowanie pośredniczące, gdy kod stanu 401 nieautoryzowane zostanie zmieniona na przekierowanie 302 na ścieżkę logowania.
* `AuthenticationScheme`: To ma zastosowanie tylko dla platformy ASP.NET Core 1.x. Nazwa logiczna schemat danego uwierzytelniania.
* `AutomaticAuthenticate`: Ta flaga ma zastosowanie tylko dla platformy ASP.NET Core 1.x. W przypadku wartości true, uruchom na każde żądanie uwierzytelniania plików cookie i próbę zweryfikowania i rekonstrukcji serializacji podmiot zabezpieczeń, z którym utworzony.

