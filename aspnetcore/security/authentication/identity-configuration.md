---
title: "Konfigurowanie tożsamości platformy ASP.NET Core"
author: AdrienTorris
description: "Zrozumienie ASP.NET Core Identity wartości domyślne i skonfigurować różne właściwości tożsamości, aby użyć niestandardowej wartości."
keywords: "Uwierzytelnianie ASP.NET Core tożsamości, zabezpieczeń"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a>Konfigurowanie tożsamości

ASP.NET Core Identity ma wspólnego zachowania w aplikacji, takich jak zasady haseł, czasu blokady i ustawienia plików cookie, które można łatwo zastąpić w aplikacji `Startup` klasy.

## <a name="passwords-policy"></a>Zasady haseł

Domyślnie tożsamości wymaga haseł zawierających wielką literę, małą literę, cyfrę i znaków innych niż alfanumeryczne. Istnieją także inne ograniczenia dotyczące. Aby uprościć ograniczeń hasła, należy zmodyfikować `ConfigureServices` metody `Startup` klasy aplikacji.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

Platformy ASP.NET Core 2.0 dodane `RequiredUniqueChars` właściwości. W przeciwnym razie opcje są takie same z platformy ASP.NET Core 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`ma następujące właściwości:

| Właściwość                | Opis                       | Domyślny |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | Wymaga liczbą z zakresu od 0 do 9 w haśle. | true |
| `RequiredLength`        | Minimalna długość hasła. | 6 |
| `RequireNonAlphanumeric`| Wymaga inne niż alfanumeryczne znaków w haśle. | true |
| `RequireUppercase`      | Wymaga się wielką literą w haśle. | true |
| `RequireLowercase`      | Wymaga się małą literą w haśle. | true |
| `RequiredUniqueChars`   | Wymaga liczby unikatowych znaków w haśle. | 1 |


## <a name="users-lockout"></a>Blokady użytkownika

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`ma następujące właściwości:

| Właściwość                | Opis                       | Domyślny |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | Czas użytkownika zostanie zablokowane po wystąpieniu blokady.  | 5 minut  |
| `MaxFailedAccessAttempts` | Liczba nieudanych prób uzyskania dostępu do momentu użytkownika jest zablokowane, jeśli funkcja blokady jest włączona.  | 5 |
| `AllowedForNewUsers` | Określa, czy nowego użytkownika można zablokować.  | true |

## <a name="sign-in-settings"></a>Zaloguj się, ustawienia

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`ma następujące właściwości:

| Właściwość                | Opis                       | Domyślny |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | Wymaga potwierdzone poczty e-mail do logowania. | false  |
| `RequireConfirmedPhoneNumber` |  Wymaga numeru telefonu potwierdzone do logowania. | false  |

## <a name="user-validation-settings"></a>Ustawienia weryfikacji użytkownika

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`ma następujące właściwości:

| Właściwość                | Opis                       | Domyślny |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | Wymaga unikatowego adresu e-mail każdego użytkownika. | false  |
| `AllowedUserNameCharacters`  | Dozwolonych znaków nazwy użytkownika. | abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ |


## <a name="applications-cookie-settings"></a>Ustawienia plików cookie aplikacji

Jak zasady haseł, wszystkie ustawienia pliku cookie aplikacji można zmienić w `Startup` klasy.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)

W obszarze `ConfigureServices` w `Startup` klasy, można skonfigurować plik cookie aplikacji.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

`CookieAuthenticationOptions`ma następujące właściwości:

| Właściwość                | Opis                       | Domyślny |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | Nazwa pliku cookie.  | . AspNetCore.Cookies.  |
| `Cookie.HttpOnly`  | Gdy ma wartość true, plik cookie nie jest dostępny ze skryptów po stronie klienta.  |  true |
| `ExpireTimeSpan`  | Określa, ile czasu przechowywania biletu uwierzytelniania w pliku cookie pozostanie ważny od punktu, w którym jest tworzona.  | 14 dni  |
| `LoginPath`  | Gdy użytkownik nie ma autoryzacji, zostanie przekierowany do tej ścieżki do logowania. | / / Logowanie się na koncie  |
| `LogoutPath`  | Gdy użytkownik jest zalogowany, zostanie przekierowany do tej ścieżki.  | / Konta/wylogowania  |
| `AccessDeniedPath`  | Gdy użytkownik nie powiodło się sprawdzanie autoryzacji, zostanie przekierowany do tej ścieżki.  |   |
| `SlidingExpiration`  | W przypadku wartości true nowy plik cookie zostanie wystawiony nową godzinę wygaśnięcia po bieżącym plikiem cookie jest przekroczyło połowę okna wygaśnięcia.  | / Konta/AccessDenied |
| `ReturnUrlParameter`  | Określa nazwę parametru ciągu zapytania, która jest dołączana przez oprogramowanie pośredniczące, gdy kod stanu 401 nieautoryzowane zostanie zmieniona na przekierowanie 302 na ścieżkę logowania.  |  true |
| `AuthenticationScheme`  | Jest to tylko istotne dla platformy ASP.NET Core 1.x. Nazwa logiczna schemat danego uwierzytelniania. |  |
| `AutomaticAuthenticate`  | Ta flaga ma zastosowanie tylko dla platformy ASP.NET Core 1.x. W przypadku wartości true, uruchom na każde żądanie uwierzytelniania plików cookie i próbę zweryfikowania i rekonstrukcji serializacji podmiot zabezpieczeń, z którym utworzony.  |  |