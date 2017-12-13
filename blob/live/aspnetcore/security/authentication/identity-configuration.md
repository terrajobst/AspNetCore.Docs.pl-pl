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
# <a name="configure-identity"></a><span data-ttu-id="77f50-104">Konfigurowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="77f50-104">Configure Identity</span></span>

<span data-ttu-id="77f50-105">Niektóre zachowania domyślnego, które można łatwo zastąpić w aplikacji ma ASP.NET Core Identity `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="77f50-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="77f50-106">Zasady haseł</span><span class="sxs-lookup"><span data-stu-id="77f50-106">Passwords policy</span></span>

<span data-ttu-id="77f50-107">Domyślnie tożsamości wymaga haseł zawierających wielką literę, małą literę, cyfrę i znaków innych niż alfanumeryczne.</span><span class="sxs-lookup"><span data-stu-id="77f50-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="77f50-108">Istnieją także inne ograniczenia dotyczące.</span><span class="sxs-lookup"><span data-stu-id="77f50-108">There are also some other restrictions.</span></span> <span data-ttu-id="77f50-109">Aby uprościć ograniczenia haseł, możesz to zrobić `Startup` klasy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77f50-109">If you want to simplify password restrictions, you can do that in the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="77f50-110">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="77f50-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="77f50-111">Platformy ASP.NET Core 2.0 dodane `RequiredUniqueChars` właściwości.</span><span class="sxs-lookup"><span data-stu-id="77f50-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="77f50-112">W przeciwnym razie opcje są takie same z platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="77f50-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="77f50-113">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="77f50-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="77f50-114">`IdentityOptions.Password`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="77f50-114">`IdentityOptions.Password` has the following properties:</span></span>
* <span data-ttu-id="77f50-115">`RequireDigit`: Wymaga liczbą z zakresu od 0 do 9 w haśle.</span><span class="sxs-lookup"><span data-stu-id="77f50-115">`RequireDigit`: Requires a number between 0-9 in the password.</span></span> <span data-ttu-id="77f50-116">Wartość domyślna to true.</span><span class="sxs-lookup"><span data-stu-id="77f50-116">Defaults to true.</span></span>
* <span data-ttu-id="77f50-117">`RequiredLength`: Minimalna długość hasła.</span><span class="sxs-lookup"><span data-stu-id="77f50-117">`RequiredLength`: The minimum length of the password.</span></span> <span data-ttu-id="77f50-118">Wartość domyślna to 6.</span><span class="sxs-lookup"><span data-stu-id="77f50-118">Defaults to 6.</span></span>
* <span data-ttu-id="77f50-119">`RequireNonAlphanumeric`: Wymaga inne niż alfanumeryczne znaków w haśle.</span><span class="sxs-lookup"><span data-stu-id="77f50-119">`RequireNonAlphanumeric`: Requires a non-alphanumeric character in the password.</span></span> <span data-ttu-id="77f50-120">Wartość domyślna to true.</span><span class="sxs-lookup"><span data-stu-id="77f50-120">Defaults to true.</span></span>
* <span data-ttu-id="77f50-121">`RequireUppercase`: Wymaga się wielką literą w haśle.</span><span class="sxs-lookup"><span data-stu-id="77f50-121">`RequireUppercase`: Requires an upper case character in the password.</span></span> <span data-ttu-id="77f50-122">Wartość domyślna to true.</span><span class="sxs-lookup"><span data-stu-id="77f50-122">Defaults to true.</span></span>
* <span data-ttu-id="77f50-123">`RequireLowercase`: Wymaga się małą literą w haśle.</span><span class="sxs-lookup"><span data-stu-id="77f50-123">`RequireLowercase`: Requires a lower case character in the password.</span></span> <span data-ttu-id="77f50-124">Wartość domyślna to true.</span><span class="sxs-lookup"><span data-stu-id="77f50-124">Defaults to true.</span></span>
* <span data-ttu-id="77f50-125">`RequiredUniqueChars`: Wymaga liczbę unikatowych znaków w haśle.</span><span class="sxs-lookup"><span data-stu-id="77f50-125">`RequiredUniqueChars`: Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="77f50-126">Wartość domyślna to 1.</span><span class="sxs-lookup"><span data-stu-id="77f50-126">Defaults to 1.</span></span>


## <a name="users-lockout"></a><span data-ttu-id="77f50-127">Blokady użytkownika</span><span class="sxs-lookup"><span data-stu-id="77f50-127">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="77f50-128">`IdentityOptions.Lockout`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="77f50-128">`IdentityOptions.Lockout` has the following properties:</span></span>
* <span data-ttu-id="77f50-129">`DefaultLockoutTimeSpan`: Ilość czasu blokady użytkownika po wystąpieniu blokady.</span><span class="sxs-lookup"><span data-stu-id="77f50-129">`DefaultLockoutTimeSpan`: The amount of time a user is locked out when a lockout occurs.</span></span> <span data-ttu-id="77f50-130">Wartość domyślna to 5 minut.</span><span class="sxs-lookup"><span data-stu-id="77f50-130">Defaults to 5 minutes.</span></span>
* <span data-ttu-id="77f50-131">`MaxFailedAccessAttempts`: Liczba nieudanych prób uzyskania dostępu do momentu użytkownika jest zablokowane, jeśli funkcja blokady jest włączona.</span><span class="sxs-lookup"><span data-stu-id="77f50-131">`MaxFailedAccessAttempts`: The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> <span data-ttu-id="77f50-132">Wartość domyślna to 5.</span><span class="sxs-lookup"><span data-stu-id="77f50-132">Defaults to 5.</span></span>
* <span data-ttu-id="77f50-133">`AllowedForNewUsers`: Określa, czy nowego użytkownika można zablokować. Wartość domyślna to true.</span><span class="sxs-lookup"><span data-stu-id="77f50-133">`AllowedForNewUsers`: Determines if a new user can be locked out. Defaults to true.</span></span>


## <a name="sign-in-settings"></a><span data-ttu-id="77f50-134">Zaloguj się, ustawienia</span><span class="sxs-lookup"><span data-stu-id="77f50-134">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="77f50-135">`IdentityOptions.SignIn`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="77f50-135">`IdentityOptions.SignIn` has the following properties:</span></span>
* <span data-ttu-id="77f50-136">`RequireConfirmedEmail`: Wymaga potwierdzone poczty e-mail do logowania.</span><span class="sxs-lookup"><span data-stu-id="77f50-136">`RequireConfirmedEmail`: Requires a confirmed email to sign in.</span></span> <span data-ttu-id="77f50-137">Wartość domyślna to false.</span><span class="sxs-lookup"><span data-stu-id="77f50-137">Defaults to false.</span></span>
* <span data-ttu-id="77f50-138">`RequireConfirmedPhoneNumber`: Wymaga numeru telefonu potwierdzone do logowania.</span><span class="sxs-lookup"><span data-stu-id="77f50-138">`RequireConfirmedPhoneNumber`: Requires a confirmed phone number to sign in.</span></span> <span data-ttu-id="77f50-139">Wartość domyślna to false.</span><span class="sxs-lookup"><span data-stu-id="77f50-139">Defaults to false.</span></span>


## <a name="user-validation-settings"></a><span data-ttu-id="77f50-140">Ustawienia weryfikacji użytkownika</span><span class="sxs-lookup"><span data-stu-id="77f50-140">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="77f50-141">`IdentityOptions.User`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="77f50-141">`IdentityOptions.User` has the following properties:</span></span>
* <span data-ttu-id="77f50-142">`RequireUniqueEmail`: Wymaga unikatowego adresu e-mail każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="77f50-142">`RequireUniqueEmail`: Requires each User to have a unique email.</span></span> <span data-ttu-id="77f50-143">Wartość domyślna to false.</span><span class="sxs-lookup"><span data-stu-id="77f50-143">Defaults to false.</span></span>
* <span data-ttu-id="77f50-144">`AllowedUserNameCharacters`: Dozwolonych znaków nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="77f50-144">`AllowedUserNameCharacters`: Allowed characters in the username.</span></span> <span data-ttu-id="77f50-145">Wartość domyślna to abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span><span class="sxs-lookup"><span data-stu-id="77f50-145">Defaults to abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="77f50-146">Ustawienia plików cookie aplikacji</span><span class="sxs-lookup"><span data-stu-id="77f50-146">Application's cookie settings</span></span>

<span data-ttu-id="77f50-147">Jak zasady haseł, wszystkie ustawienia pliku cookie aplikacji można zmienić w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="77f50-147">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="77f50-148">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="77f50-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="77f50-149">W obszarze `ConfigureServices` w `Startup` klasy, można skonfigurować plik cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="77f50-149">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="77f50-150">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="77f50-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

<span data-ttu-id="77f50-151">`CookieAuthenticationOptions`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="77f50-151">`CookieAuthenticationOptions` has the following properties:</span></span>
* <span data-ttu-id="77f50-152">`Cookie.Name`: Nazwa pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="77f50-152">`Cookie.Name`: The name of the cookie.</span></span> <span data-ttu-id="77f50-153">Wartość domyślna to. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="77f50-153">Defaults to .AspNetCore.Cookies.</span></span>
* <span data-ttu-id="77f50-154">`Cookie.HttpOnly`: W przypadku wartości true, plik cookie nie jest dostępny ze skryptów po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="77f50-154">`Cookie.HttpOnly`: When true, the cookie is not accessible from client-side scripts.</span></span> <span data-ttu-id="77f50-155">Wartość domyślna to true.</span><span class="sxs-lookup"><span data-stu-id="77f50-155">Defaults to true.</span></span>
* <span data-ttu-id="77f50-156">`ExpireTimeSpan`: Określa, ile czasu przechowywania biletu uwierzytelniania w pliku cookie pozostanie ważny od punktu, w którym jest tworzona.</span><span class="sxs-lookup"><span data-stu-id="77f50-156">`ExpireTimeSpan`: Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span> <span data-ttu-id="77f50-157">Wartość domyślna to 14 dni.</span><span class="sxs-lookup"><span data-stu-id="77f50-157">Defaults to 14 days.</span></span>
* <span data-ttu-id="77f50-158">`LoginPath`: Jeśli użytkownik nie ma autoryzacji, zostanie przekierowany do tej ścieżki do logowania.</span><span class="sxs-lookup"><span data-stu-id="77f50-158">`LoginPath`: When a user is unauthorized, they will be redirected to this path to login.</span></span> <span data-ttu-id="77f50-159">Wartość domyślna to/Account/logowania.</span><span class="sxs-lookup"><span data-stu-id="77f50-159">Defaults to /Account/Login.</span></span>
* <span data-ttu-id="77f50-160">`LogoutPath`: Jeśli użytkownik jest zalogowany, zostanie przekierowany do tej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="77f50-160">`LogoutPath`: When a user is logged out, they will be redirected to this path.</span></span> <span data-ttu-id="77f50-161">Wartość domyślna to/Account/wylogowania.</span><span class="sxs-lookup"><span data-stu-id="77f50-161">Defaults to /Account/Logout.</span></span>
* <span data-ttu-id="77f50-162">`AccessDeniedPath`: W przypadku niepowodzenia autoryzacji wyboru użytkownik zostanie przekierowany do tej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="77f50-162">`AccessDeniedPath`: When a user fails an authorization check, they will be redirected to this path.</span></span> <span data-ttu-id="77f50-163">Wartość domyślna to/Account/AccessDenied.</span><span class="sxs-lookup"><span data-stu-id="77f50-163">Defaults to /Account/AccessDenied.</span></span>
* <span data-ttu-id="77f50-164">`SlidingExpiration`: W przypadku wartości true nowy plik cookie zostanie wystawiony nową godzinę wygaśnięcia po bieżącym plikiem cookie jest przekroczyło połowę okna wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="77f50-164">`SlidingExpiration`: When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span> <span data-ttu-id="77f50-165">Wartość domyślna to true.</span><span class="sxs-lookup"><span data-stu-id="77f50-165">Defaults to true.</span></span>
* <span data-ttu-id="77f50-166">`ReturnUrlParameter`: Parametr ReturnUrlParameter Określa nazwę parametru ciągu zapytania, która jest dołączana przez oprogramowanie pośredniczące, gdy kod stanu 401 nieautoryzowane zostanie zmieniona na przekierowanie 302 na ścieżkę logowania.</span><span class="sxs-lookup"><span data-stu-id="77f50-166">`ReturnUrlParameter`: The ReturnUrlParameter determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>
* <span data-ttu-id="77f50-167">`AuthenticationScheme`: To ma zastosowanie tylko dla platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="77f50-167">`AuthenticationScheme`: This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="77f50-168">Nazwa logiczna schemat danego uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="77f50-168">The logical name for a particular authentication scheme.</span></span>
* <span data-ttu-id="77f50-169">`AutomaticAuthenticate`: Ta flaga ma zastosowanie tylko dla platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="77f50-169">`AutomaticAuthenticate`: This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="77f50-170">W przypadku wartości true, uruchom na każde żądanie uwierzytelniania plików cookie i próbę zweryfikowania i rekonstrukcji serializacji podmiot zabezpieczeń, z którym utworzony.</span><span class="sxs-lookup"><span data-stu-id="77f50-170">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

