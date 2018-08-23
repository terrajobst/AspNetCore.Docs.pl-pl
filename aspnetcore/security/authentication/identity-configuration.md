---
title: Konfigurowanie tożsamości platformy ASP.NET Core
author: AdrienTorris
description: Zrozumienie wartości domyślne tożsamości platformy ASP.NET Core i Dowiedz się, jak skonfigurować właściwości tożsamości, aby użyć niestandardowej wartości.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: c597eacbb21ed0968e6195f7b6dcb46d37ba80a5
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870920"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="e564d-103">Konfigurowanie tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e564d-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="e564d-104">Tożsamość platformy ASP.NET Core używa domyślnych wartości dla ustawienia, takie jak zasady haseł, blokowanie i plik cookie konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="e564d-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="e564d-105">Te ustawienia mogą zostać zastąpione w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="e564d-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="e564d-106">Opcji tożsamości</span><span class="sxs-lookup"><span data-stu-id="e564d-106">Identity options</span></span>

<span data-ttu-id="e564d-107">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) klasa przedstawia opcje, które mogą być używane do konfigurowania systemu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="e564d-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="e564d-108">`IdentityOptions` należy ustawić **po** wywoływania `AddIdentity` lub `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="e564d-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="e564d-109">Tożsamość oświadczeń</span><span class="sxs-lookup"><span data-stu-id="e564d-109">Claims Identity</span></span>

<span data-ttu-id="e564d-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) Określa [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) za pomocą właściwości pokazane w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="e564d-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="e564d-111">Właściwość</span><span class="sxs-lookup"><span data-stu-id="e564d-111">Property</span></span> | <span data-ttu-id="e564d-112">Opis</span><span class="sxs-lookup"><span data-stu-id="e564d-112">Description</span></span> | <span data-ttu-id="e564d-113">Domyślny</span><span class="sxs-lookup"><span data-stu-id="e564d-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e564d-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="e564d-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="e564d-115">Pobiera lub ustawia typ oświadczenia używany dla oświadczeń roli.</span><span class="sxs-lookup"><span data-stu-id="e564d-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="e564d-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="e564d-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="e564d-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="e564d-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="e564d-118">Pobiera lub ustawia typ oświadczenia używany dla oświadczeń sygnatury bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="e564d-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="e564d-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="e564d-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="e564d-120">Pobiera lub ustawia typ oświadczenia używany dla oświadczeń identyfikatora użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e564d-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="e564d-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="e564d-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="e564d-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="e564d-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="e564d-123">Pobiera lub ustawia typ oświadczenia używany dla oświadczenia nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e564d-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="e564d-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="e564d-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="e564d-125">Blokady</span><span class="sxs-lookup"><span data-stu-id="e564d-125">Lockout</span></span>

<span data-ttu-id="e564d-126">Blokada jest ustawiana w [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) metody:</span><span class="sxs-lookup"><span data-stu-id="e564d-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="e564d-127">Powyższy kod opiera się na `Login` tożsamość szablonu.</span><span class="sxs-lookup"><span data-stu-id="e564d-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="e564d-128">Opcje blokady są ustawiane w `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e564d-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="e564d-129">Poprzedni zestawów kodu [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) z wartościami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="e564d-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="e564d-130">Po pomyślnym uwierzytelnieniu resetuje liczbę prób dostępu nie powiodło się i zresetowanie zegara.</span><span class="sxs-lookup"><span data-stu-id="e564d-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="e564d-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) Określa [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) przy użyciu właściwości wyświetlane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="e564d-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="e564d-132">Właściwość</span><span class="sxs-lookup"><span data-stu-id="e564d-132">Property</span></span> | <span data-ttu-id="e564d-133">Opis</span><span class="sxs-lookup"><span data-stu-id="e564d-133">Description</span></span> | <span data-ttu-id="e564d-134">Domyślny</span><span class="sxs-lookup"><span data-stu-id="e564d-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e564d-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="e564d-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="e564d-136">Określa, jeśli nowy użytkownik może być zablokowane.</span><span class="sxs-lookup"><span data-stu-id="e564d-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="e564d-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="e564d-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="e564d-138">Czas blokady użytkownika po wystąpieniu blokady.</span><span class="sxs-lookup"><span data-stu-id="e564d-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="e564d-139">5 minut</span><span class="sxs-lookup"><span data-stu-id="e564d-139">5 minutes</span></span> |
| [<span data-ttu-id="e564d-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="e564d-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="e564d-141">Liczba nieudanych prób uzyskania dostępu do momentu użytkownika jest zablokowane, jeśli funkcja blokady jest włączona.</span><span class="sxs-lookup"><span data-stu-id="e564d-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="e564d-142">5</span><span class="sxs-lookup"><span data-stu-id="e564d-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="e564d-143">Hasło</span><span class="sxs-lookup"><span data-stu-id="e564d-143">Password</span></span>

<span data-ttu-id="e564d-144">Domyślnie tożsamości wymaga haseł zawierających wielkiej litery, małej litery, cyfry i znak inny niż alfanumeryczny.</span><span class="sxs-lookup"><span data-stu-id="e564d-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="e564d-145">Hasło musi mieć co najmniej 6 znaków.</span><span class="sxs-lookup"><span data-stu-id="e564d-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="e564d-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) można ustawić w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e564d-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="e564d-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) Określa [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) przy użyciu właściwości wyświetlane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="e564d-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="e564d-148">Właściwość</span><span class="sxs-lookup"><span data-stu-id="e564d-148">Property</span></span> | <span data-ttu-id="e564d-149">Opis</span><span class="sxs-lookup"><span data-stu-id="e564d-149">Description</span></span> | <span data-ttu-id="e564d-150">Domyślny</span><span class="sxs-lookup"><span data-stu-id="e564d-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e564d-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="e564d-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="e564d-152">Wymaga liczbą z zakresu od 0 do 9, w haśle.</span><span class="sxs-lookup"><span data-stu-id="e564d-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="e564d-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="e564d-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="e564d-154">Minimalna długość hasła.</span><span class="sxs-lookup"><span data-stu-id="e564d-154">The minimum length of the password.</span></span> | <span data-ttu-id="e564d-155">6</span><span class="sxs-lookup"><span data-stu-id="e564d-155">6</span></span> |

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e564d-156">| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Ma zastosowanie tylko do ASP.NET Core 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="e564d-156">| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="e564d-157">Musi zawierać cyfry, odrębnych znaków w haśle.</span><span class="sxs-lookup"><span data-stu-id="e564d-157">Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="e564d-158">| 1 |</span><span class="sxs-lookup"><span data-stu-id="e564d-158">| 1 |</span></span>

::: moniker-end

<span data-ttu-id="e564d-159">| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Wymaga małą literę w haśle.</span><span class="sxs-lookup"><span data-stu-id="e564d-159">| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Requires a lowercase character in the password.</span></span><span data-ttu-id="e564d-160"> | `true` | | [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Wymaga znaków niealfanumerycznych w haśle.</span><span class="sxs-lookup"><span data-stu-id="e564d-160"> | `true` | | [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Requires a non-alphanumeric character in the password.</span></span><span data-ttu-id="e564d-161"> | `true` | | [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Wymaga wielkiej litery w haśle.</span><span class="sxs-lookup"><span data-stu-id="e564d-161"> | `true` | | [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Requires an uppercase character in the password.</span></span> | `true` |

### <a name="sign-in"></a><span data-ttu-id="e564d-162">Zaloguj się</span><span class="sxs-lookup"><span data-stu-id="e564d-162">Sign-in</span></span>

<span data-ttu-id="e564d-163">Poniższy kod ustawia `SignIn` ustawień (wartości domyślne):</span><span class="sxs-lookup"><span data-stu-id="e564d-163">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="e564d-164">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) Określa [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) przy użyciu właściwości wyświetlane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="e564d-164">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="e564d-165">Właściwość</span><span class="sxs-lookup"><span data-stu-id="e564d-165">Property</span></span> | <span data-ttu-id="e564d-166">Opis</span><span class="sxs-lookup"><span data-stu-id="e564d-166">Description</span></span> | <span data-ttu-id="e564d-167">Domyślny</span><span class="sxs-lookup"><span data-stu-id="e564d-167">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e564d-168">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="e564d-168">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="e564d-169">Wymaga potwierdzony adres e-mail do logowania.</span><span class="sxs-lookup"><span data-stu-id="e564d-169">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="e564d-170">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="e564d-170">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="e564d-171">Wymaga numeru telefonu potwierdzone zalogować się.</span><span class="sxs-lookup"><span data-stu-id="e564d-171">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="e564d-172">Tokeny</span><span class="sxs-lookup"><span data-stu-id="e564d-172">Tokens</span></span>

<span data-ttu-id="e564d-173">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) Określa [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) przy użyciu właściwości wyświetlane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="e564d-173">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>


|                                                        <span data-ttu-id="e564d-174">Właściwość</span><span class="sxs-lookup"><span data-stu-id="e564d-174">Property</span></span>                                                         |                                                                                      <span data-ttu-id="e564d-175">Opis</span><span class="sxs-lookup"><span data-stu-id="e564d-175">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="e564d-176">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e564d-176">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="e564d-177">Pobiera lub ustawia `AuthenticatorTokenProvider` służący do weryfikowania logowania dwuskładnikowego z wystawcy uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="e564d-177">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="e564d-178">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e564d-178">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="e564d-179">Pobiera lub ustawia `ChangeEmailTokenProvider` używane do generowania tokenów używanych w wiadomości e-mail z potwierdzeniem zmiany wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="e564d-179">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="e564d-180">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e564d-180">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="e564d-181">Pobiera lub ustawia `ChangePhoneNumberTokenProvider` używane do generowania tokenów w przypadku zmiany numerów telefonów.</span><span class="sxs-lookup"><span data-stu-id="e564d-181">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="e564d-182">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e564d-182">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="e564d-183">Pobiera lub Ustawia dostawcę tokenu służącego do generowania tokenów używanych w wiadomości e-mail z potwierdzeniem konta.</span><span class="sxs-lookup"><span data-stu-id="e564d-183">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="e564d-184">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="e564d-184">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="e564d-185">Pobiera lub ustawia [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) używane do generowania tokenów używane w wiadomościach e-mail resetowania hasła.</span><span class="sxs-lookup"><span data-stu-id="e564d-185">Gets or sets the [IUserTwoFactorTokenProvider<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="e564d-186">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="e564d-186">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="e564d-187">Używany do budowy [dostawcy tokenu użytkownika](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) przy użyciu klucza używana jako nazwa dostawcy.</span><span class="sxs-lookup"><span data-stu-id="e564d-187">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="e564d-188">Użytkownik</span><span class="sxs-lookup"><span data-stu-id="e564d-188">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="e564d-189">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) Określa [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) przy użyciu właściwości wyświetlane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="e564d-189">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="e564d-190">Właściwość</span><span class="sxs-lookup"><span data-stu-id="e564d-190">Property</span></span> | <span data-ttu-id="e564d-191">Opis</span><span class="sxs-lookup"><span data-stu-id="e564d-191">Description</span></span> | <span data-ttu-id="e564d-192">Domyślny</span><span class="sxs-lookup"><span data-stu-id="e564d-192">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="e564d-193">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="e564d-193">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="e564d-194">Dozwolonych znaków nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e564d-194">Allowed characters in the username.</span></span> | <span data-ttu-id="e564d-195">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="e564d-195">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="e564d-196">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="e564d-196">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="e564d-197">0123456789</span><span class="sxs-lookup"><span data-stu-id="e564d-197">0123456789</span></span><br><span data-ttu-id="e564d-198">-._@+</span><span class="sxs-lookup"><span data-stu-id="e564d-198">-._@+</span></span> |
| [<span data-ttu-id="e564d-199">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="e564d-199">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="e564d-200">Wymaga każdy użytkownik musi mieć unikatowy adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="e564d-200">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="e564d-201">Ustawienia plików cookie</span><span class="sxs-lookup"><span data-stu-id="e564d-201">Cookie settings</span></span>

<span data-ttu-id="e564d-202">Konfigurowanie aplikacji plik cookie, `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e564d-202">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e564d-203">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) musi zostać wywołana **po** wywoływania `AddIdentity` lub `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="e564d-203">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="e564d-204">Aby uzyskać więcej informacji, zobacz [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="e564d-204">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>