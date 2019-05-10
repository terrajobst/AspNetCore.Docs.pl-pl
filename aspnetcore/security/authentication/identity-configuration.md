---
title: Konfigurowanie tożsamości platformy ASP.NET Core
author: AdrienTorris
description: Zrozumienie wartości domyślne tożsamości platformy ASP.NET Core i Dowiedz się, jak skonfigurować właściwości tożsamości, aby użyć niestandardowej wartości.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898801"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="524d9-103">Konfigurowanie tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="524d9-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="524d9-104">Tożsamość platformy ASP.NET Core używa domyślnych wartości dla ustawienia, takie jak zasady haseł, blokowanie i plik cookie konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="524d9-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="524d9-105">Te ustawienia mogą zostać zastąpione w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="524d9-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="524d9-106">Opcji tożsamości</span><span class="sxs-lookup"><span data-stu-id="524d9-106">Identity options</span></span>

<span data-ttu-id="524d9-107">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) klasa przedstawia opcje, które mogą być używane do konfigurowania systemu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="524d9-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="524d9-108">`IdentityOptions` należy ustawić **po** wywoływania `AddIdentity` lub `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="524d9-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="524d9-109">Tożsamość oświadczeń</span><span class="sxs-lookup"><span data-stu-id="524d9-109">Claims Identity</span></span>

<span data-ttu-id="524d9-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) Określa [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) za pomocą właściwości pokazane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="524d9-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="524d9-111">Właściwość</span><span class="sxs-lookup"><span data-stu-id="524d9-111">Property</span></span> | <span data-ttu-id="524d9-112">Opis</span><span class="sxs-lookup"><span data-stu-id="524d9-112">Description</span></span> | <span data-ttu-id="524d9-113">Domyślny</span><span class="sxs-lookup"><span data-stu-id="524d9-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="524d9-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="524d9-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="524d9-115">Pobiera lub ustawia typ oświadczenia używany dla oświadczeń roli.</span><span class="sxs-lookup"><span data-stu-id="524d9-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="524d9-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="524d9-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="524d9-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="524d9-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="524d9-118">Pobiera lub ustawia typ oświadczenia używany dla oświadczeń sygnatury bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="524d9-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="524d9-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="524d9-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="524d9-120">Pobiera lub ustawia typ oświadczenia używany dla oświadczeń identyfikatora użytkownika.</span><span class="sxs-lookup"><span data-stu-id="524d9-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="524d9-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="524d9-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="524d9-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="524d9-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="524d9-123">Pobiera lub ustawia typ oświadczenia używany dla oświadczenia nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="524d9-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="524d9-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="524d9-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="524d9-125">Blokady</span><span class="sxs-lookup"><span data-stu-id="524d9-125">Lockout</span></span>

<span data-ttu-id="524d9-126">Blokada jest ustawiana w [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) metody:</span><span class="sxs-lookup"><span data-stu-id="524d9-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="524d9-127">Powyższy kod opiera się na `Login` tożsamość szablonu.</span><span class="sxs-lookup"><span data-stu-id="524d9-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="524d9-128">Opcje blokady są ustawiane w `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="524d9-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="524d9-129">Poprzedni zestawów kodu [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) z wartościami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="524d9-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="524d9-130">Po pomyślnym uwierzytelnieniu resetuje liczbę prób dostępu nie powiodło się i zresetowanie zegara.</span><span class="sxs-lookup"><span data-stu-id="524d9-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="524d9-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) Określa [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) przy użyciu właściwości wyświetlane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="524d9-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="524d9-132">Właściwość</span><span class="sxs-lookup"><span data-stu-id="524d9-132">Property</span></span> | <span data-ttu-id="524d9-133">Opis</span><span class="sxs-lookup"><span data-stu-id="524d9-133">Description</span></span> | <span data-ttu-id="524d9-134">Domyślny</span><span class="sxs-lookup"><span data-stu-id="524d9-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="524d9-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="524d9-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="524d9-136">Określa, jeśli nowy użytkownik może być zablokowane.</span><span class="sxs-lookup"><span data-stu-id="524d9-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="524d9-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="524d9-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="524d9-138">Czas blokady użytkownika po wystąpieniu blokady.</span><span class="sxs-lookup"><span data-stu-id="524d9-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="524d9-139">5 minut</span><span class="sxs-lookup"><span data-stu-id="524d9-139">5 minutes</span></span> |
| [<span data-ttu-id="524d9-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="524d9-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="524d9-141">Liczba nieudanych prób uzyskania dostępu do momentu użytkownika jest zablokowane, jeśli funkcja blokady jest włączona.</span><span class="sxs-lookup"><span data-stu-id="524d9-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="524d9-142">5</span><span class="sxs-lookup"><span data-stu-id="524d9-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="524d9-143">Hasło</span><span class="sxs-lookup"><span data-stu-id="524d9-143">Password</span></span>

<span data-ttu-id="524d9-144">Domyślnie tożsamości wymaga haseł zawierających wielkiej litery, małej litery, cyfry i znak inny niż alfanumeryczny.</span><span class="sxs-lookup"><span data-stu-id="524d9-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="524d9-145">Hasło musi mieć co najmniej 6 znaków.</span><span class="sxs-lookup"><span data-stu-id="524d9-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="524d9-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) można ustawić w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="524d9-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="524d9-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) Określa [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) przy użyciu właściwości wyświetlane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="524d9-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="524d9-148">Właściwość</span><span class="sxs-lookup"><span data-stu-id="524d9-148">Property</span></span> | <span data-ttu-id="524d9-149">Opis</span><span class="sxs-lookup"><span data-stu-id="524d9-149">Description</span></span> | <span data-ttu-id="524d9-150">Domyślny</span><span class="sxs-lookup"><span data-stu-id="524d9-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="524d9-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="524d9-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="524d9-152">Wymaga liczbą z zakresu od 0 do 9, w haśle.</span><span class="sxs-lookup"><span data-stu-id="524d9-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="524d9-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="524d9-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="524d9-154">Minimalna długość hasła.</span><span class="sxs-lookup"><span data-stu-id="524d9-154">The minimum length of the password.</span></span> | <span data-ttu-id="524d9-155">6</span><span class="sxs-lookup"><span data-stu-id="524d9-155">6</span></span> |
| [<span data-ttu-id="524d9-156">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="524d9-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="524d9-157">Wymaga małą literę w haśle.</span><span class="sxs-lookup"><span data-stu-id="524d9-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="524d9-158">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="524d9-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="524d9-159">Wymaga znaków niealfanumerycznych w haśle.</span><span class="sxs-lookup"><span data-stu-id="524d9-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="524d9-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="524d9-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="524d9-161">Ma zastosowanie tylko do ASP.NET Core 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="524d9-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="524d9-162">Musi zawierać cyfry, odrębnych znaków w haśle.</span><span class="sxs-lookup"><span data-stu-id="524d9-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="524d9-163">1</span><span class="sxs-lookup"><span data-stu-id="524d9-163">1</span></span> |
| [<span data-ttu-id="524d9-164">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="524d9-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="524d9-165">Wymaga wielkiej litery w haśle.</span><span class="sxs-lookup"><span data-stu-id="524d9-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="524d9-166">Właściwość</span><span class="sxs-lookup"><span data-stu-id="524d9-166">Property</span></span> | <span data-ttu-id="524d9-167">Opis</span><span class="sxs-lookup"><span data-stu-id="524d9-167">Description</span></span> | <span data-ttu-id="524d9-168">Domyślny</span><span class="sxs-lookup"><span data-stu-id="524d9-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="524d9-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="524d9-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="524d9-170">Wymaga liczbą z zakresu od 0 do 9, w haśle.</span><span class="sxs-lookup"><span data-stu-id="524d9-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="524d9-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="524d9-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="524d9-172">Minimalna długość hasła.</span><span class="sxs-lookup"><span data-stu-id="524d9-172">The minimum length of the password.</span></span> | <span data-ttu-id="524d9-173">6</span><span class="sxs-lookup"><span data-stu-id="524d9-173">6</span></span> |
| [<span data-ttu-id="524d9-174">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="524d9-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="524d9-175">Wymaga małą literę w haśle.</span><span class="sxs-lookup"><span data-stu-id="524d9-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="524d9-176">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="524d9-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="524d9-177">Wymaga znaków niealfanumerycznych w haśle.</span><span class="sxs-lookup"><span data-stu-id="524d9-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="524d9-178">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="524d9-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="524d9-179">Wymaga wielkiej litery w haśle.</span><span class="sxs-lookup"><span data-stu-id="524d9-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="524d9-180">Logowanie</span><span class="sxs-lookup"><span data-stu-id="524d9-180">Sign-in</span></span>

<span data-ttu-id="524d9-181">Poniższy kod ustawia `SignIn` ustawień (wartości domyślne):</span><span class="sxs-lookup"><span data-stu-id="524d9-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="524d9-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) Określa [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) przy użyciu właściwości wyświetlane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="524d9-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="524d9-183">Właściwość</span><span class="sxs-lookup"><span data-stu-id="524d9-183">Property</span></span> | <span data-ttu-id="524d9-184">Opis</span><span class="sxs-lookup"><span data-stu-id="524d9-184">Description</span></span> | <span data-ttu-id="524d9-185">Domyślny</span><span class="sxs-lookup"><span data-stu-id="524d9-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="524d9-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="524d9-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="524d9-187">Wymaga potwierdzony adres e-mail do logowania.</span><span class="sxs-lookup"><span data-stu-id="524d9-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="524d9-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="524d9-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="524d9-189">Wymaga numeru telefonu potwierdzone zalogować się.</span><span class="sxs-lookup"><span data-stu-id="524d9-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="524d9-190">Tokeny</span><span class="sxs-lookup"><span data-stu-id="524d9-190">Tokens</span></span>

<span data-ttu-id="524d9-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) Określa [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) przy użyciu właściwości wyświetlane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="524d9-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>

|                                                        <span data-ttu-id="524d9-192">Właściwość</span><span class="sxs-lookup"><span data-stu-id="524d9-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="524d9-193">Opis</span><span class="sxs-lookup"><span data-stu-id="524d9-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="524d9-194">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="524d9-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="524d9-195">Pobiera lub ustawia `AuthenticatorTokenProvider` służący do weryfikowania logowania dwuskładnikowego z wystawcy uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="524d9-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="524d9-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="524d9-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="524d9-197">Pobiera lub ustawia `ChangeEmailTokenProvider` używane do generowania tokenów używanych w wiadomości e-mail z potwierdzeniem zmiany wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="524d9-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="524d9-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="524d9-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="524d9-199">Pobiera lub ustawia `ChangePhoneNumberTokenProvider` używane do generowania tokenów w przypadku zmiany numerów telefonów.</span><span class="sxs-lookup"><span data-stu-id="524d9-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="524d9-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="524d9-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="524d9-201">Pobiera lub Ustawia dostawcę tokenu służącego do generowania tokenów używanych w wiadomości e-mail z potwierdzeniem konta.</span><span class="sxs-lookup"><span data-stu-id="524d9-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="524d9-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="524d9-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="524d9-203">Pobiera lub ustawia [IUserTwoFactorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) używane do generowania tokenów używane w wiadomościach e-mail resetowania hasła.</span><span class="sxs-lookup"><span data-stu-id="524d9-203">Gets or sets the [IUserTwoFactorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="524d9-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="524d9-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="524d9-205">Używany do budowy [dostawcy tokenu użytkownika](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) przy użyciu klucza używana jako nazwa dostawcy.</span><span class="sxs-lookup"><span data-stu-id="524d9-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="524d9-206">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="524d9-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="524d9-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) Określa [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) przy użyciu właściwości wyświetlane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="524d9-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="524d9-208">Właściwość</span><span class="sxs-lookup"><span data-stu-id="524d9-208">Property</span></span> | <span data-ttu-id="524d9-209">Opis</span><span class="sxs-lookup"><span data-stu-id="524d9-209">Description</span></span> | <span data-ttu-id="524d9-210">Domyślny</span><span class="sxs-lookup"><span data-stu-id="524d9-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="524d9-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="524d9-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="524d9-212">Dozwolonych znaków nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="524d9-212">Allowed characters in the username.</span></span> | <span data-ttu-id="524d9-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="524d9-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="524d9-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="524d9-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="524d9-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="524d9-215">0123456789</span></span><br><span data-ttu-id="524d9-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="524d9-216">-.\_@+</span></span> |
| [<span data-ttu-id="524d9-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="524d9-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="524d9-218">Wymaga każdy użytkownik musi mieć unikatowy adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="524d9-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="524d9-219">Ustawienia plików cookie</span><span class="sxs-lookup"><span data-stu-id="524d9-219">Cookie settings</span></span>

<span data-ttu-id="524d9-220">Konfigurowanie aplikacji plik cookie, `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="524d9-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="524d9-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) musi zostać wywołana **po** wywoływania `AddIdentity` lub `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="524d9-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="524d9-222">Aby uzyskać więcej informacji, zobacz [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="524d9-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>

## <a name="password-hasher-options"></a><span data-ttu-id="524d9-223">Opcje Hasher hasła</span><span class="sxs-lookup"><span data-stu-id="524d9-223">Password Hasher options</span></span>

<span data-ttu-id="524d9-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> pobiera i ustawia opcje tworzenia skrótu hasła.</span><span class="sxs-lookup"><span data-stu-id="524d9-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> gets and sets options for password hashing.</span></span>

| <span data-ttu-id="524d9-225">Opcja</span><span class="sxs-lookup"><span data-stu-id="524d9-225">Option</span></span> | <span data-ttu-id="524d9-226">Opis</span><span class="sxs-lookup"><span data-stu-id="524d9-226">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | <span data-ttu-id="524d9-227">Tryb zgodności używany podczas mieszania nowych haseł.</span><span class="sxs-lookup"><span data-stu-id="524d9-227">The compatibility mode used when hashing new passwords.</span></span> <span data-ttu-id="524d9-228">Wartość domyślna to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="524d9-228">Defaults to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="524d9-229">Pierwszy bajt skrótem hasła, o nazwie *format znacznika*, określa numer wersji to algorytm mieszania używany do tworzenia skrótu hasła.</span><span class="sxs-lookup"><span data-stu-id="524d9-229">The first byte of a hashed password, called a *format marker*, specifies the version of the hashing algorithm used to hash the password.</span></span> <span data-ttu-id="524d9-230">Podczas weryfikowania haseł przed skrót, <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> metoda wybiera poprawny algorytm, oparty na pierwszy bajt.</span><span class="sxs-lookup"><span data-stu-id="524d9-230">When verifying a password against a hash, the <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> method selects the correct algorithm based on the first byte.</span></span> <span data-ttu-id="524d9-231">Klient jest w stanie do uwierzytelniania niezależnie od tego z nich użyto wersję algorytmu wyznaczania wartości skrótu hasła.</span><span class="sxs-lookup"><span data-stu-id="524d9-231">A client is able to authenticate regardless of which version of the algorithm was used to hash the password.</span></span> <span data-ttu-id="524d9-232">Ustawienie trybu zgodności dotyczy mieszanie *nowych haseł*.</span><span class="sxs-lookup"><span data-stu-id="524d9-232">Setting the compatibility mode affects the hashing of *new passwords*.</span></span> |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | <span data-ttu-id="524d9-233">Liczba iteracji, używane podczas tworzenia skrótów haseł przy użyciu PBKDF2.</span><span class="sxs-lookup"><span data-stu-id="524d9-233">The number of iterations used when hashing passwords using PBKDF2.</span></span> <span data-ttu-id="524d9-234">Ta wartość jest tylko wtedy, gdy używane <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> ustawiono <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="524d9-234">This value is only used when the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> is set to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="524d9-235">Wartość musi być dodatnią liczbą całkowitą i wartość domyślna to `10000`.</span><span class="sxs-lookup"><span data-stu-id="524d9-235">The value must be a positive integer and defaults to `10000`.</span></span> |

<span data-ttu-id="524d9-236">W poniższym przykładzie <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> ustawiono `12000` w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="524d9-236">In the following example, the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> is set to `12000` in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
