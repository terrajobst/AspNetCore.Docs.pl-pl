---
title: "Konfigurowanie tożsamości platformy ASP.NET Core"
author: AdrienTorris
description: "Zrozumienie ASP.NET Core Identity wartości domyślne i skonfigurować różne właściwości tożsamości, aby użyć niestandardowej wartości."
manager: wpickett
ms.author: scaddie
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: cf7dcdb80f5edf9e10960cb08957793c36829a69
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="configure-identity"></a><span data-ttu-id="adeae-103">Konfigurowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="adeae-103">Configure Identity</span></span>

<span data-ttu-id="adeae-104">ASP.NET Core Identity ma wspólnego zachowania w aplikacji, takich jak zasady haseł, czasu blokady i ustawienia plików cookie, które można łatwo zastąpić w aplikacji `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="adeae-104">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="adeae-105">Zasady haseł</span><span class="sxs-lookup"><span data-stu-id="adeae-105">Passwords policy</span></span>

<span data-ttu-id="adeae-106">Domyślnie tożsamości wymaga haseł zawierających wielką literę, małą literę, cyfrę i znaków innych niż alfanumeryczne.</span><span class="sxs-lookup"><span data-stu-id="adeae-106">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="adeae-107">Istnieją także inne ograniczenia dotyczące.</span><span class="sxs-lookup"><span data-stu-id="adeae-107">There are also some other restrictions.</span></span> <span data-ttu-id="adeae-108">Aby uprościć ograniczeń hasła, należy zmodyfikować `ConfigureServices` metody `Startup` klasy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="adeae-108">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adeae-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="adeae-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="adeae-110">Platformy ASP.NET Core 2.0 dodane `RequiredUniqueChars` właściwości.</span><span class="sxs-lookup"><span data-stu-id="adeae-110">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="adeae-111">W przeciwnym razie opcje są takie same z platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="adeae-111">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adeae-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="adeae-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="adeae-113">`IdentityOptions.Password`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="adeae-113">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="adeae-114">Właściwość</span><span class="sxs-lookup"><span data-stu-id="adeae-114">Property</span></span>                | <span data-ttu-id="adeae-115">Opis</span><span class="sxs-lookup"><span data-stu-id="adeae-115">Description</span></span>                       | <span data-ttu-id="adeae-116">Domyślny</span><span class="sxs-lookup"><span data-stu-id="adeae-116">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="adeae-117">Wymaga liczbą z zakresu od 0 do 9 w haśle.</span><span class="sxs-lookup"><span data-stu-id="adeae-117">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="adeae-118">true</span><span class="sxs-lookup"><span data-stu-id="adeae-118">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="adeae-119">Minimalna długość hasła.</span><span class="sxs-lookup"><span data-stu-id="adeae-119">The minimum length of the password.</span></span> | <span data-ttu-id="adeae-120">6</span><span class="sxs-lookup"><span data-stu-id="adeae-120">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="adeae-121">Wymaga inne niż alfanumeryczne znaków w haśle.</span><span class="sxs-lookup"><span data-stu-id="adeae-121">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="adeae-122">true</span><span class="sxs-lookup"><span data-stu-id="adeae-122">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="adeae-123">Wymaga się wielką literą w haśle.</span><span class="sxs-lookup"><span data-stu-id="adeae-123">Requires an upper case character in the password.</span></span> | <span data-ttu-id="adeae-124">true</span><span class="sxs-lookup"><span data-stu-id="adeae-124">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="adeae-125">Wymaga się małą literą w haśle.</span><span class="sxs-lookup"><span data-stu-id="adeae-125">Requires a lower case character in the password.</span></span> | <span data-ttu-id="adeae-126">true</span><span class="sxs-lookup"><span data-stu-id="adeae-126">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="adeae-127">Wymaga liczby unikatowych znaków w haśle.</span><span class="sxs-lookup"><span data-stu-id="adeae-127">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="adeae-128">1</span><span class="sxs-lookup"><span data-stu-id="adeae-128">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="adeae-129">Blokady użytkownika</span><span class="sxs-lookup"><span data-stu-id="adeae-129">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="adeae-130">`IdentityOptions.Lockout`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="adeae-130">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="adeae-131">Właściwość</span><span class="sxs-lookup"><span data-stu-id="adeae-131">Property</span></span>                | <span data-ttu-id="adeae-132">Opis</span><span class="sxs-lookup"><span data-stu-id="adeae-132">Description</span></span>                       | <span data-ttu-id="adeae-133">Domyślny</span><span class="sxs-lookup"><span data-stu-id="adeae-133">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="adeae-134">Czas użytkownika zostanie zablokowane po wystąpieniu blokady.</span><span class="sxs-lookup"><span data-stu-id="adeae-134">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="adeae-135">5 minut</span><span class="sxs-lookup"><span data-stu-id="adeae-135">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="adeae-136">Liczba nieudanych prób uzyskania dostępu do momentu użytkownika jest zablokowane, jeśli funkcja blokady jest włączona.</span><span class="sxs-lookup"><span data-stu-id="adeae-136">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="adeae-137">5</span><span class="sxs-lookup"><span data-stu-id="adeae-137">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="adeae-138">Określa, czy nowego użytkownika można zablokować.</span><span class="sxs-lookup"><span data-stu-id="adeae-138">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="adeae-139">true</span><span class="sxs-lookup"><span data-stu-id="adeae-139">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="adeae-140">Zaloguj się, ustawienia</span><span class="sxs-lookup"><span data-stu-id="adeae-140">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="adeae-141">`IdentityOptions.SignIn`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="adeae-141">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="adeae-142">Właściwość</span><span class="sxs-lookup"><span data-stu-id="adeae-142">Property</span></span>                | <span data-ttu-id="adeae-143">Opis</span><span class="sxs-lookup"><span data-stu-id="adeae-143">Description</span></span>                       | <span data-ttu-id="adeae-144">Domyślny</span><span class="sxs-lookup"><span data-stu-id="adeae-144">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="adeae-145">Wymaga potwierdzone poczty e-mail do logowania.</span><span class="sxs-lookup"><span data-stu-id="adeae-145">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="adeae-146">false</span><span class="sxs-lookup"><span data-stu-id="adeae-146">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="adeae-147">Wymaga numeru telefonu potwierdzone do logowania.</span><span class="sxs-lookup"><span data-stu-id="adeae-147">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="adeae-148">false</span><span class="sxs-lookup"><span data-stu-id="adeae-148">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="adeae-149">Ustawienia weryfikacji użytkownika</span><span class="sxs-lookup"><span data-stu-id="adeae-149">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="adeae-150">`IdentityOptions.User`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="adeae-150">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="adeae-151">Właściwość</span><span class="sxs-lookup"><span data-stu-id="adeae-151">Property</span></span>                | <span data-ttu-id="adeae-152">Opis</span><span class="sxs-lookup"><span data-stu-id="adeae-152">Description</span></span>                       | <span data-ttu-id="adeae-153">Domyślny</span><span class="sxs-lookup"><span data-stu-id="adeae-153">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="adeae-154">Wymaga unikatowego adresu e-mail każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="adeae-154">Requires each User to have a unique email.</span></span> | <span data-ttu-id="adeae-155">false</span><span class="sxs-lookup"><span data-stu-id="adeae-155">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="adeae-156">Dozwolonych znaków nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="adeae-156">Allowed characters in the username.</span></span> | <span data-ttu-id="adeae-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="adeae-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="adeae-158">Ustawienia plików cookie aplikacji</span><span class="sxs-lookup"><span data-stu-id="adeae-158">Application's cookie settings</span></span>

<span data-ttu-id="adeae-159">Jak zasady haseł, wszystkie ustawienia pliku cookie aplikacji można zmienić w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="adeae-159">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="adeae-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="adeae-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="adeae-161">W obszarze `ConfigureServices` w `Startup` klasy, można skonfigurować plik cookie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="adeae-161">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="adeae-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="adeae-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="adeae-163">`CookieAuthenticationOptions`ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="adeae-163">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="adeae-164">Właściwość</span><span class="sxs-lookup"><span data-stu-id="adeae-164">Property</span></span>                | <span data-ttu-id="adeae-165">Opis</span><span class="sxs-lookup"><span data-stu-id="adeae-165">Description</span></span>                       | <span data-ttu-id="adeae-166">Domyślny</span><span class="sxs-lookup"><span data-stu-id="adeae-166">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="adeae-167">Nazwa pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="adeae-167">The name of the cookie.</span></span>  | <span data-ttu-id="adeae-168">.AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="adeae-168">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="adeae-169">Gdy ma wartość true, plik cookie nie jest dostępny ze skryptów po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="adeae-169">When true, the cookie isn't accessible from client-side scripts.</span></span>  |  <span data-ttu-id="adeae-170">true</span><span class="sxs-lookup"><span data-stu-id="adeae-170">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="adeae-171">Określa, ile czasu przechowywania biletu uwierzytelniania w pliku cookie pozostanie ważny od punktu, w którym jest tworzona.</span><span class="sxs-lookup"><span data-stu-id="adeae-171">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it's created.</span></span>  | <span data-ttu-id="adeae-172">14 dni</span><span class="sxs-lookup"><span data-stu-id="adeae-172">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="adeae-173">Gdy użytkownik nie ma autoryzacji, zostanie przekierowany do tej ścieżki do logowania.</span><span class="sxs-lookup"><span data-stu-id="adeae-173">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="adeae-174">/ / Logowanie się na koncie</span><span class="sxs-lookup"><span data-stu-id="adeae-174">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="adeae-175">Gdy użytkownik jest zalogowany, zostanie przekierowany do tej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="adeae-175">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="adeae-176">/ Konta/wylogowania</span><span class="sxs-lookup"><span data-stu-id="adeae-176">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="adeae-177">Gdy użytkownik nie powiodło się sprawdzanie autoryzacji, zostanie przekierowany do tej ścieżki.</span><span class="sxs-lookup"><span data-stu-id="adeae-177">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="adeae-178">W przypadku wartości true nowy plik cookie zostanie wystawiony nową godzinę wygaśnięcia po bieżącym plikiem cookie jest przekroczyło połowę okna wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="adeae-178">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="adeae-179">/Account/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="adeae-179">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="adeae-180">Określa nazwę parametru ciągu zapytania, która jest dołączana przez oprogramowanie pośredniczące, gdy kod stanu 401 nieautoryzowane zostanie zmieniona na przekierowanie 302 na ścieżkę logowania.</span><span class="sxs-lookup"><span data-stu-id="adeae-180">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="adeae-181">true</span><span class="sxs-lookup"><span data-stu-id="adeae-181">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="adeae-182">Jest to tylko istotne dla platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="adeae-182">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="adeae-183">Nazwa logiczna schemat danego uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="adeae-183">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="adeae-184">Ta flaga ma zastosowanie tylko dla platformy ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="adeae-184">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="adeae-185">W przypadku wartości true, uruchom na każde żądanie uwierzytelniania plików cookie i próbę zweryfikowania i rekonstrukcji serializacji podmiot zabezpieczeń, z którym utworzony.</span><span class="sxs-lookup"><span data-stu-id="adeae-185">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |
