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
# <a name="configure-identity"></a><span data-ttu-id="0472f-104">Konfigurowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="0472f-104">Configure Identity</span></span>

<span data-ttu-id="0472f-105">ASP.NET Core Identity ma wspólnego zachowania w aplikacji, takich jak zasady haseł, czasu blokady i ustawienia plików cookie, które można łatwo zastąpić w aplikacji `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="0472f-105">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="0472f-106">Zasady haseł</span><span class="sxs-lookup"><span data-stu-id="0472f-106">Passwords policy</span></span>

<span data-ttu-id="0472f-107">Domyślnie tożsamości wymaga haseł zawierających wielką literę, małą literę, cyfrę i znaków innych niż alfanumeryczne.</span><span class="sxs-lookup"><span data-stu-id="0472f-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="0472f-108">Istnieją także inne ograniczenia dotyczące.</span><span class="sxs-lookup"><span data-stu-id="0472f-108">There are also some other restrictions.</span></span> <span data-ttu-id="0472f-109">Aby uprościć ograniczeń hasła, należy zmodyfikować `ConfigureServices` metody `Startup` klasy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0472f-109">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0472f-110">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0472f-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0472f-111">Platformy ASP.NET Core 2.0 dodane `RequiredUniqueChars` właściwości.</span><span class="sxs-lookup"><span data-stu-id="0472f-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="0472f-112">W przeciwnym razie opcje są takie same z platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="0472f-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0472f-113">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0472f-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="0472f-114">`IdentityOptions.Password`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="0472f-114">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="0472f-115">Właściwość</span><span class="sxs-lookup"><span data-stu-id="0472f-115">Property</span></span>                | <span data-ttu-id="0472f-116">Opis</span><span class="sxs-lookup"><span data-stu-id="0472f-116">Description</span></span>                       | <span data-ttu-id="0472f-117">Domyślny</span><span class="sxs-lookup"><span data-stu-id="0472f-117">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="0472f-118">Wymaga liczbą z zakresu od 0 do 9 w haśle.</span><span class="sxs-lookup"><span data-stu-id="0472f-118">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="0472f-119">true</span><span class="sxs-lookup"><span data-stu-id="0472f-119">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="0472f-120">Minimalna długość hasła.</span><span class="sxs-lookup"><span data-stu-id="0472f-120">The minimum length of the password.</span></span> | <span data-ttu-id="0472f-121">6</span><span class="sxs-lookup"><span data-stu-id="0472f-121">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="0472f-122">Wymaga inne niż alfanumeryczne znaków w haśle.</span><span class="sxs-lookup"><span data-stu-id="0472f-122">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="0472f-123">true</span><span class="sxs-lookup"><span data-stu-id="0472f-123">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="0472f-124">Wymaga się wielką literą w haśle.</span><span class="sxs-lookup"><span data-stu-id="0472f-124">Requires an upper case character in the password.</span></span> | <span data-ttu-id="0472f-125">true</span><span class="sxs-lookup"><span data-stu-id="0472f-125">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="0472f-126">Wymaga się małą literą w haśle.</span><span class="sxs-lookup"><span data-stu-id="0472f-126">Requires a lower case character in the password.</span></span> | <span data-ttu-id="0472f-127">true</span><span class="sxs-lookup"><span data-stu-id="0472f-127">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="0472f-128">Wymaga liczby unikatowych znaków w haśle.</span><span class="sxs-lookup"><span data-stu-id="0472f-128">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="0472f-129">1</span><span class="sxs-lookup"><span data-stu-id="0472f-129">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="0472f-130">Blokady użytkownika</span><span class="sxs-lookup"><span data-stu-id="0472f-130">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="0472f-131">`IdentityOptions.Lockout`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="0472f-131">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="0472f-132">Właściwość</span><span class="sxs-lookup"><span data-stu-id="0472f-132">Property</span></span>                | <span data-ttu-id="0472f-133">Opis</span><span class="sxs-lookup"><span data-stu-id="0472f-133">Description</span></span>                       | <span data-ttu-id="0472f-134">Domyślny</span><span class="sxs-lookup"><span data-stu-id="0472f-134">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="0472f-135">Czas użytkownika zostanie zablokowane po wystąpieniu blokady.</span><span class="sxs-lookup"><span data-stu-id="0472f-135">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="0472f-136">5 minut</span><span class="sxs-lookup"><span data-stu-id="0472f-136">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="0472f-137">Liczba nieudanych prób uzyskania dostępu do momentu użytkownika jest zablokowane, jeśli funkcja blokady jest włączona.</span><span class="sxs-lookup"><span data-stu-id="0472f-137">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="0472f-138">5</span><span class="sxs-lookup"><span data-stu-id="0472f-138">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="0472f-139">Określa, czy nowego użytkownika można zablokować.</span><span class="sxs-lookup"><span data-stu-id="0472f-139">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="0472f-140">true</span><span class="sxs-lookup"><span data-stu-id="0472f-140">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="0472f-141">Zaloguj się, ustawienia</span><span class="sxs-lookup"><span data-stu-id="0472f-141">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="0472f-142">`IdentityOptions.SignIn`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="0472f-142">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="0472f-143">Właściwość</span><span class="sxs-lookup"><span data-stu-id="0472f-143">Property</span></span>                | <span data-ttu-id="0472f-144">Opis</span><span class="sxs-lookup"><span data-stu-id="0472f-144">Description</span></span>                       | <span data-ttu-id="0472f-145">Domyślny</span><span class="sxs-lookup"><span data-stu-id="0472f-145">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="0472f-146">Wymaga potwierdzone poczty e-mail do logowania.</span><span class="sxs-lookup"><span data-stu-id="0472f-146">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="0472f-147">false</span><span class="sxs-lookup"><span data-stu-id="0472f-147">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="0472f-148">Wymaga numeru telefonu potwierdzone do logowania.</span><span class="sxs-lookup"><span data-stu-id="0472f-148">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="0472f-149">false</span><span class="sxs-lookup"><span data-stu-id="0472f-149">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="0472f-150">Ustawienia weryfikacji użytkownika</span><span class="sxs-lookup"><span data-stu-id="0472f-150">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="0472f-151">`IdentityOptions.User`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="0472f-151">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="0472f-152">Właściwość</span><span class="sxs-lookup"><span data-stu-id="0472f-152">Property</span></span>                | <span data-ttu-id="0472f-153">Opis</span><span class="sxs-lookup"><span data-stu-id="0472f-153">Description</span></span>                       | <span data-ttu-id="0472f-154">Domyślny</span><span class="sxs-lookup"><span data-stu-id="0472f-154">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="0472f-155">Wymaga unikatowego adresu e-mail każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0472f-155">Requires each User to have a unique email.</span></span> | <span data-ttu-id="0472f-156">false</span><span class="sxs-lookup"><span data-stu-id="0472f-156">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="0472f-157">Dozwolonych znaków nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0472f-157">Allowed characters in the username.</span></span> | <span data-ttu-id="0472f-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="0472f-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="0472f-159">Ustawienia plików cookie aplikacji</span><span class="sxs-lookup"><span data-stu-id="0472f-159">Application's cookie settings</span></span>

<span data-ttu-id="0472f-160">Jak zasady haseł, wszystkie ustawienia pliku cookie aplikacji można zmienić w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="0472f-160">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0472f-161">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0472f-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0472f-162">W obszarze `ConfigureServices` w `Startup` klasy, można skonfigurować plik cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0472f-162">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0472f-163">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0472f-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="0472f-164">`CookieAuthenticationOptions`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="0472f-164">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="0472f-165">Właściwość</span><span class="sxs-lookup"><span data-stu-id="0472f-165">Property</span></span>                | <span data-ttu-id="0472f-166">Opis</span><span class="sxs-lookup"><span data-stu-id="0472f-166">Description</span></span>                       | <span data-ttu-id="0472f-167">Domyślny</span><span class="sxs-lookup"><span data-stu-id="0472f-167">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="0472f-168">Nazwa pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="0472f-168">The name of the cookie.</span></span>  | <span data-ttu-id="0472f-169">. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="0472f-169">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="0472f-170">Gdy ma wartość true, plik cookie nie jest dostępny ze skryptów po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="0472f-170">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="0472f-171">true</span><span class="sxs-lookup"><span data-stu-id="0472f-171">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="0472f-172">Określa, ile czasu przechowywania biletu uwierzytelniania w pliku cookie pozostanie ważny od punktu, w którym jest tworzona.</span><span class="sxs-lookup"><span data-stu-id="0472f-172">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="0472f-173">14 dni</span><span class="sxs-lookup"><span data-stu-id="0472f-173">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="0472f-174">Gdy użytkownik nie ma autoryzacji, zostanie przekierowany do tej ścieżki do logowania.</span><span class="sxs-lookup"><span data-stu-id="0472f-174">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="0472f-175">/ / Logowanie się na koncie</span><span class="sxs-lookup"><span data-stu-id="0472f-175">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="0472f-176">Gdy użytkownik jest zalogowany, zostanie przekierowany do tej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="0472f-176">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="0472f-177">/ Konta/wylogowania</span><span class="sxs-lookup"><span data-stu-id="0472f-177">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="0472f-178">Gdy użytkownik nie powiodło się sprawdzanie autoryzacji, zostanie przekierowany do tej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="0472f-178">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="0472f-179">W przypadku wartości true nowy plik cookie zostanie wystawiony nową godzinę wygaśnięcia po bieżącym plikiem cookie jest przekroczyło połowę okna wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="0472f-179">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="0472f-180">/ Konta/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="0472f-180">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="0472f-181">Określa nazwę parametru ciągu zapytania, która jest dołączana przez oprogramowanie pośredniczące, gdy kod stanu 401 nieautoryzowane zostanie zmieniona na przekierowanie 302 na ścieżkę logowania.</span><span class="sxs-lookup"><span data-stu-id="0472f-181">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="0472f-182">true</span><span class="sxs-lookup"><span data-stu-id="0472f-182">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="0472f-183">Jest to tylko istotne dla platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="0472f-183">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="0472f-184">Nazwa logiczna schemat danego uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="0472f-184">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="0472f-185">Ta flaga ma zastosowanie tylko dla platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="0472f-185">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="0472f-186">W przypadku wartości true, uruchom na każde żądanie uwierzytelniania plików cookie i próbę zweryfikowania i rekonstrukcji serializacji podmiot zabezpieczeń, z którym utworzony.</span><span class="sxs-lookup"><span data-stu-id="0472f-186">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |