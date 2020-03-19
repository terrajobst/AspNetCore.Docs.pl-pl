---
title: Uwierzytelnianie wieloskładnikowe w ASP.NET Core
author: damienbod
description: Dowiedz się, jak skonfigurować uwierzytelnianie wieloskładnikowe (MFA) w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: rick-anderson
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Identity
uid: security/authentication/mfa
ms.openlocfilehash: 6220688d53f0718ca5be5f63dd5d9539d37e2391
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2020
ms.locfileid: "79520198"
---
# <a name="multi-factor-authentication-in-aspnet-core"></a>Uwierzytelnianie wieloskładnikowe w ASP.NET Core

Autor [Damien Bowden](https://github.com/damienbod)

Uwierzytelnianie wieloskładnikowe (MFA) to proces, w którym użytkownik jest proszony podczas logowania w celu uzyskania dodatkowych form identyfikacji. Ten monit może być wprowadzeniem kodu z Cellphone, użycie klucza FIDO2 lub w celu przeskanowania odcisku palca. Gdy wymagana jest druga forma uwierzytelniania, zabezpieczenia są rozszerzane. Dodatkowy czynnik nie jest łatwo uzyskiwany ani duplikowany przez osobę atakującą.

W tym artykule opisano następujące zagadnienia:

* Co to jest MFA i jakie są zalecane przepływy uwierzytelniania MFA
* Konfigurowanie uwierzytelniania wieloskładnikowego dla stron administracyjnych przy użyciu ASP.NET Core Identity
* Wyślij wymaganie logowania MFA do serwera OpenID Connect Connect
* Wymuś ASP.NET Core OpenID Connect Połącz klienta, aby wymagać uwierzytelniania wieloskładnikowego

## <a name="mfa-2fa"></a>MFA, FUNKCJI 2FA

Uwierzytelnianie wieloskładnikowe wymaga co najmniej dwóch typów dowodu tożsamości, takich jak coś, czego wiadomo, co Ci się podoba, lub sprawdzanie biometryczne użytkownika do uwierzytelnienia.

Uwierzytelnianie dwuskładnikowe (funkcji 2FA) jest podobne do podzestawu MFA, ale różnica polega na tym, że MFA może wymagać co najmniej dwóch czynników, aby potwierdzić tożsamość.

### <a name="mfa-totp-time-based-one-time-password-algorithm"></a>TOTP MFA (algorytm hasła jednorazowego oparty na czasie)

Uwierzytelnianie wieloskładnikowe przy użyciu usługi TOTP to obsługiwana implementacja przy użyciu IdentityASP.NET Core. Może być używany razem z dowolną zgodną aplikacją uwierzytelniania, w tym:

* Aplikacja Microsoft Authenticator
* Aplikacja Google Authenticator

Aby uzyskać szczegółowe informacje dotyczące implementacji, zobacz następujący link:

[Włącz generowanie kodu QR dla aplikacji TOTP Authenticator w ASP.NET Core](xref:security/authentication/identity-enable-qrcodes)

### <a name="mfa-fido2-or-passwordless"></a>FIDO2 MFA lub hasła

FIDO2 jest obecnie:

* Najbezpieczniejszym sposobem osiągnięcia usługi MFA.
* Jedyny przepływ usługi MFA, który chroni przed atakami wyłudzania informacji.

W tej chwili ASP.NET Core nie obsługuje bezpośrednio FIDO2. FIDO2 można używać w przypadku przepływów MFA lub bezhaseł.

Azure Active Directory zapewnia obsługę przepływów FIDO2 i bezhaseł. Aby uzyskać więcej informacji, zobacz [Opcje uwierzytelniania bezhasło dla Azure Active Directory](/azure/active-directory/authentication/concept-authentication-passwordless).

### <a name="mfa-sms"></a>SMS USŁUGI MFA

Funkcja MFA z programem SMS zwiększa bezpieczeństwo w dużej porównaniu z uwierzytelnianiem haseł (pojedynczym czynnikiem). Jednak korzystanie z wiadomości SMS jako drugiego czynnika nie jest już zalecane. Istnieje zbyt wiele znanych wektorów ataków dla tego typu implementacji.

[Wskazówki dotyczące NIST](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="configure-mfa-for-administration-pages-using-aspnet-core-opno-locidentity"></a>Konfigurowanie uwierzytelniania wieloskładnikowego dla stron administracyjnych przy użyciu ASP.NET Core Identity

Uwierzytelnianie wieloskładnikowe może być wymuszane dla użytkowników w celu uzyskania dostępu do poufnych stron w aplikacji Identity ASP.NET Core. Może to być przydatne w przypadku aplikacji, w których istnieją różne poziomy dostępu dla różnych tożsamości. Na przykład użytkownicy mogą wyświetlać dane profilu przy użyciu hasła logowania, ale do uzyskiwania dostępu do stron administracyjnych będzie wymagane użycie usługi MFA.

### <a name="extend-the-login-with-an-mfa-claim"></a>Zwiększanie nazwy logowania przy użyciu żądania MFA

Kod demonstracyjny jest skonfigurowany przy użyciu ASP.NET Core z Identity i Razor Pages. Metoda `AddIdentity` jest używana zamiast `AddDefaultIdentity`, więc implementacja `IUserClaimsPrincipalFactory` może zostać użyta do dodania oświadczeń do tożsamości po pomyślnym zalogowaniu.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));
    
    services.AddIdentity<IdentityUser, IdentityRole>(
            options => options.SignIn.RequireConfirmedAccount = false)
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddSingleton<IEmailSender, EmailSender>();
    services.AddScoped<IUserClaimsPrincipalFactory<IdentityUser>, 
        AdditionalUserClaimsPrincipalFactory>();

    services.AddAuthorization(options =>
        options.AddPolicy("TwoFactorEnabled",
            x => x.RequireClaim("amr", "mfa")));

    services.AddRazorPages();
}
```

Klasa `AdditionalUserClaimsPrincipalFactory` dodaje oświadczenie `amr` do oświadczeń użytkownika dopiero po pomyślnym zalogowaniu. Wartość żądania jest odczytywana z bazy danych. W tym miejscu zostanie dodane zgłoszenie, ponieważ użytkownik powinien uzyskać dostęp do wyższego widoku chronionego tylko wtedy, gdy tożsamość została zarejestrowana za pomocą usługi MFA. Jeśli widok bazy danych jest odczytywany z bazy danych bezpośrednio, a nie za pomocą tego żądania, można uzyskać dostęp do widoku bez usługi MFA bezpośrednio po aktywowaniu usługi MFA.

```csharp
using Microsoft.AspNetCore.Identity;
using Microsoft.Extensions.Options;
using System.Collections.Generic;
using System.Security.Claims;
using System.Threading.Tasks;

namespace IdentityStandaloneMfa
{
    public class AdditionalUserClaimsPrincipalFactory : 
        UserClaimsPrincipalFactory<IdentityUser, IdentityRole>
    {
        public AdditionalUserClaimsPrincipalFactory( 
            UserManager<IdentityUser> userManager,
            RoleManager<IdentityRole> roleManager, 
            IOptions<IdentityOptions> optionsAccessor) 
            : base(userManager, roleManager, optionsAccessor)
        {
        }

        public async override Task<ClaimsPrincipal> CreateAsync(IdentityUser user)
        {
            var principal = await base.CreateAsync(user);
            var identity = (ClaimsIdentity)principal.Identity;

            var claims = new List<Claim>();

            if (user.TwoFactorEnabled)
            {
                claims.Add(new Claim("amr", "mfa"));
            }
            else
            {
                claims.Add(new Claim("amr", "pwd"));
            }

            identity.AddClaims(claims);
            return principal;
        }
    }
}
```

Ponieważ konfiguracja usługi Identity została zmieniona w klasie `Startup`, należy zaktualizować układy Identity. Szkieletuje Identity stronach w aplikacji. Zdefiniuj układ w pliku *Identity/Account/Manage/_Layout. cshtml* .

```cshtml
@{
    Layout = "/Pages/Shared/_Layout.cshtml";
}
```

Przypisz również układ dla wszystkich stron zarządzania ze stron Identity:

```cshtml
@{
    Layout = "_Layout.cshtml";
}
```

### <a name="validate-the-mfa-requirement-in-the-administration-page"></a>Sprawdzanie wymagań usługi MFA na stronie Administracja

Na stronie Administracja Razor Sprawdź, czy użytkownik zalogował się przy użyciu usługi MFA. W metodzie `OnGet` tożsamość jest używana w celu uzyskania dostępu do oświadczeń użytkowników. Dla `mfa`wartości jest sprawdzane `amr`. Jeśli w tożsamości brakuje tego żądania lub `false`, Strona przekierowuje do strony Włączanie usługi MFA. Jest to możliwe, ponieważ użytkownik zalogował się już, ale bez usługi MFA.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace IdentityStandaloneMfa
{
    public class AdminModel : PageModel
    {
        public IActionResult OnGet()
        {
            var claimTwoFactorEnabled = 
                User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (claimTwoFactorEnabled != null && 
                "mfa".Equals(claimTwoFactorEnabled.Value))
            {
                // You logged in with MFA, do the administrative stuff
            }
            else
            {
                return Redirect(
                    "/Identity/Account/Manage/TwoFactorAuthentication");
            }

            return Page();
        }
    }
}
```

### <a name="ui-logic-to-toggle-user-login-information"></a>Logika interfejsu użytkownika do przełączania informacji logowania użytkownika

Podczas uruchamiania dodano zasady autoryzacji. Zasady wymagają `mfa``amr` z wartością.

```csharp
services.AddAuthorization(options =>
    options.AddPolicy("TwoFactorEnabled",
        x => x.RequireClaim("amr", "mfa")));
```

Tych zasad można następnie użyć w widoku `_Layout`, aby pokazać lub ukryć menu **administratora** z ostrzeżeniem:

```cshtml
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Identity
@inject SignInManager<IdentityUser> SignInManager
@inject UserManager<IdentityUser> UserManager
@inject IAuthorizationService AuthorizationService
```

Jeśli tożsamość została zarejestrowana przy użyciu usługi MFA, menu **administratora** jest wyświetlane bez ostrzeżenia etykietki narzędzia. Gdy użytkownik zalogował się bez uwierzytelniania wieloskładnikowego, wyświetlane jest menu **administrator (nie włączono)** wraz z etykietą narzędzia, która informuje użytkownika (wyjaśnienie ostrzeżenia).

```cshtml
@if (SignInManager.IsSignedIn(User))
{
    @if ((AuthorizationService.AuthorizeAsync(User, "TwoFactorEnabled")).Result.Succeeded)
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin">Admin</a>
        </li>
    }
    else
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin" 
               id="tooltip-demo"  
               data-toggle="tooltip" 
               data-placement="bottom" 
               title="MFA is NOT enabled. This is required for the Admin Page. If you have activated MFA, then logout, login again.">
                Admin (Not Enabled)
            </a>
        </li>
    }
}
```

Jeśli użytkownik zaloguje się bez uwierzytelniania wieloskładnikowego, wyświetlane jest ostrzeżenie:

![Uwierzytelnianie MFA administratora](mfa/_static/identitystandalonemfa_01.png)

Użytkownik zostanie przekierowany do widoku włączenia usługi MFA po kliknięciu linku **administratora** :

![Administrator aktywuje uwierzytelnianie MFA](mfa/_static/identitystandalonemfa_02.png)

## <a name="send-mfa-sign-in-requirement-to-openid-connect-server"></a>Wyślij wymaganie logowania MFA do serwera OpenID Connect Connect 

Parametru `acr_values` można użyć do przekazania `mfa` wymaganej wartości z klienta do serwera w żądaniu uwierzytelniania.

> [!NOTE]
> Aby ta wartość działała, należy obsłużyć parametr `acr_values`.

### <a name="openid-connect-aspnet-core-client"></a>OpenID Connect Connect ASP.NET Core Client

ASP.NET Core Razor Pages Otwórz aplikację kliencką Connect ID używa metody `AddOpenIdConnect`, aby zalogować się na serwerze programu Open ID Connect. Parametr `acr_values` jest ustawiany z wartością `mfa` i wysyłany z żądaniem uwierzytelnienia. `OpenIdConnectEvents` służy do dodawania tego.

Aby uzyskać zalecane wartości parametrów `acr_values`, zobacz [wartości referencyjne metody uwierzytelniania](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08).

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "<OpenID Connect server URL>";
        options.RequireHttpsMetadata = true;
        options.ClientId = "<OpenID Connect client ID>";
        options.ClientSecret = "<>";
        // Code with PKCE can also be used here
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
        options.Events = new OpenIdConnectEvents
        {
            OnRedirectToIdentityProvider = context =>
            {
                context.ProtocolMessage.SetParameter("acr_values", "mfa");
                return Task.FromResult(0);
            }
        };
    });
```

### <a name="example-openid-connect-identityserver-4-server-with-aspnet-core-opno-locidentity"></a>Przykład OpenID Connect Connect IdentityServer 4 Server z ASP.NET Core Identity

Na serwerze OpenID Connect Connect, który jest implementowany przy użyciu ASP.NET Core Identity z widokami MVC, zostanie utworzony nowy widok o nazwie *ErrorEnable2FA. cshtml* . Widok:

* Wyświetla Jeśli Identity pochodzi z aplikacji, która wymaga uwierzytelniania wieloskładnikowego, ale użytkownik nie został aktywowany w Identity.
* Informuje użytkownika i dodaje link umożliwiający jego aktywowanie.

```cshtml
@{
    ViewData["Title"] = "ErrorEnable2FA";
}

<h1>The client application requires you to have MFA enabled. Enable this, try login again.</h1>

<br />

You can enable MFA to login here:

<br />

<a asp-controller="Manage" asp-action="TwoFactorAuthentication">Enable MFA</a>
```

W metodzie `Login` interfejs `IIdentityServerInteractionService` implementacji `_interaction` jest używany w celu uzyskania dostępu do parametrów otwartego żądania połączenia. Do parametru `acr_values` można uzyskać dostęp za pomocą właściwości `AcrValues`. Gdy klient wysłał ten zestaw `mfa`, można to sprawdzić.

Jeśli wymagana jest usługa MFA, a użytkownik w ASP.NET Core Identity ma włączoną usługę MFA, logowanie będzie kontynuowane. Jeśli użytkownik nie ma włączonej usługi MFA, użytkownik zostanie przekierowany do widoku niestandardowego *ErrorEnable2FA. cshtml*. Następnie ASP.NET Core Identity podpisać użytkownika w programie.

```csharp
//
// POST: /Account/Login
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Login(LoginInputModel model)
{
    var returnUrl = model.ReturnUrl;
    var context = 
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa = 
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    var user = await _userManager.FindByNameAsync(model.Email);
    if (user != null && !user.TwoFactorEnabled && requires2Fa)
    {
        return RedirectToAction(nameof(ErrorEnable2FA));
    }

    // code omitted for brevity
```

Metoda `ExternalLoginCallback` działa jak logowanie lokalne Identity. Dla wartości `mfa` jest sprawdzana Właściwość `AcrValues`. Jeśli wartość `mfa` jest obecna, uwierzytelnianie wieloskładnikowe jest wymuszane przed zakończeniem logowania (na przykład przekierowane do widoku `ErrorEnable2FA`).

```csharp
//
// GET: /Account/ExternalLoginCallback
[HttpGet]
[AllowAnonymous]
public async Task<IActionResult> ExternalLoginCallback(
    string returnUrl = null,
    string remoteError = null)
{
    var context =
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa =
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    if (remoteError != null)
    {
        ModelState.AddModelError(
            string.Empty,
            _sharedLocalizer["EXTERNAL_PROVIDER_ERROR", 
            remoteError]);
        return View(nameof(Login));
    }
    var info = await _signInManager.GetExternalLoginInfoAsync();

    if (info == null)
    {
        return RedirectToAction(nameof(Login));
    }

    var email = info.Principal.FindFirstValue(ClaimTypes.Email);

    if (!string.IsNullOrEmpty(email))
    {
        var user = await _userManager.FindByNameAsync(email);
        if (user != null && !user.TwoFactorEnabled && requires2Fa)
        {
            return RedirectToAction(nameof(ErrorEnable2FA));
        }
    }

    // Sign in the user with this external login provider if the user already has a login.
    var result = await _signInManager
        .ExternalLoginSignInAsync(
            info.LoginProvider, 
            info.ProviderKey, 
            isPersistent: 
            false);

    // code omitted for brevity
```

Jeśli użytkownik jest już zalogowany, aplikacja kliencka:

* Nadal sprawdza poprawność `amr`.
* Usługę MFA można skonfigurować za pomocą linku do widoku Identity ASP.NET Core.

![acr_values-1](mfa/_static/acr_values-1.png)

## <a name="force-aspnet-core-openid-connect-client-to-require-mfa"></a>Wymuś ASP.NET Core OpenID Connect Połącz klienta, aby wymagać uwierzytelniania wieloskładnikowego

Ten przykład pokazuje, w jaki sposób aplikacja ze stroną ASP.NET Core Razor, która używa programu OpenID Connect Connect do logowania, może wymagać, aby użytkownicy zostali uwierzytelnieni przy użyciu usługi MFA.

Aby sprawdzić wymaganie usługi MFA, zostanie utworzone żądanie `IAuthorizationRequirement`. Ta wartość zostanie dodana do stron przy użyciu zasad, które wymagają uwierzytelniania wieloskładnikowego.

```csharp
using Microsoft.AspNetCore.Authorization;
 
namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfa : IAuthorizationRequirement{}
}
```

Zaimplementowano `AuthorizationHandler`, która będzie używać `amr`, i sprawdza, czy `mfa`wartości. `amr` jest zwracana w `id_token` pomyślnego uwierzytelnienia i może mieć wiele różnych wartości, zgodnie z definicją w specyfikacji [wartości odwołania metody uwierzytelniania](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) .

Zwracana wartość zależy od tego, jak tożsamość została uwierzytelniona i w implementacji serwera Connect z połączeniem Open ID.

`AuthorizationHandler` używa wymagań `RequireMfa` i sprawdza poprawność `amr` roszczeń. Serwer OpenID Connect Connect można zaimplementować przy użyciu usługi identityserver4 z ASP.NET Core Identity. Gdy użytkownik loguje się przy użyciu TOTP `amr`, zostanie zwrócona wartość usługi MFA. Jeśli jest używana inna implementacja serwera OpenID Connect Connect lub inny typ MFA, `amr` to lub może mieć inną wartość. Kod musi być rozszerzony, aby można było go również zaakceptować.

```csharp
using Microsoft.AspNetCore.Authorization;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfaHandler : AuthorizationHandler<RequireMfa>
    {
        protected override Task HandleRequirementAsync(
            AuthorizationHandlerContext context, 
            RequireMfa requirement)
        {
            if (context == null)
                throw new ArgumentNullException(nameof(context));
            if (requirement == null)
                throw new ArgumentNullException(nameof(requirement));

            var amrClaim =
                context.User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (amrClaim != null && amrClaim.Value == Amr.Mfa)
            {
                context.Succeed(requirement);
            }

            return Task.CompletedTask;
        }
    }
}
```

W metodzie `Startup.ConfigureServices` Metoda `AddOpenIdConnect` jest używana jako domyślny schemat wyzwania. Procedura obsługi autoryzacji, która jest używana do sprawdzania `amr`, jest dodawana do kontenera kontroli Inversion. Następnie tworzone są zasady, które dodają `RequireMfa` wymaganie.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.ConfigureApplicationCookie(options =>
        options.Cookie.SecurePolicy =
            CookieSecurePolicy.Always);

    services.AddSingleton<IAuthorizationHandler, RequireMfaHandler>();

    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "https://localhost:44352";
        options.RequireHttpsMetadata = true;
        options.ClientId = "AspNetCoreRequireMfaOidc";
        options.ClientSecret = "AspNetCoreRequireMfaOidcSecret";
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
    });

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireMfa", policyIsAdminRequirement =>
        {
            policyIsAdminRequirement.Requirements.Add(new RequireMfa());
        });
    });

    services.AddRazorPages();
}
```

Te zasady są następnie używane na stronie Razor zgodnie z wymaganiami. Zasady mogą być również dodawane globalnie dla całej aplikacji.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;
using Microsoft.Extensions.Logging;

namespace AspNetCoreRequireMfaOidc.Pages
{
    [Authorize(Policy= "RequireMfa")]
    public class IndexModel : PageModel
    {
        private readonly ILogger<IndexModel> _logger;

        public IndexModel(ILogger<IndexModel> logger)
        {
            _logger = logger;
        }

        public void OnGet()
        {
        }
    }
}
```

Jeśli użytkownik uwierzytelnia się bez uwierzytelniania wieloskładnikowego, `amr` będzie prawdopodobnie mieć `pwd` wartość. Żądanie nie zostanie autoryzowane do uzyskania dostępu do strony. Przy użyciu wartości domyślnych użytkownik zostanie przekierowany na stronę *Account/AccessDenied* . Takie zachowanie można zmienić lub wdrożyć własną logikę niestandardową tutaj. W tym przykładzie zostanie dodany link, dzięki czemu prawidłowy użytkownik może skonfigurować uwierzytelnianie wieloskładnikowe dla swojego konta.

```cshtml
@page
@model AspNetCoreRequireMfaOidc.AccessDeniedModel
@{
    ViewData["Title"] = "AccessDenied";
    Layout = "~/Pages/Shared/_Layout.cshtml";
}

<h1>AccessDenied</h1>

You require MFA to login here

<a href="https://localhost:44352/Manage/TwoFactorAuthentication">Enable MFA</a>
```

Teraz tylko użytkownicy, którzy uwierzytelniają się za pomocą usługi MFA, mogą uzyskiwać dostęp do strony lub witryny sieci Web. Jeśli używane są różne typy MFA lub jeśli funkcji 2FA jest poprawny, `amr`go będzie mieć różne wartości i muszą zostać prawidłowo przetworzone. Różne serwery programu Open ID Connect również zwracają różne wartości dla tego żądania i mogą nie być zgodne ze specyfikacją [wartości odwołania metody uwierzytelniania](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08) .

Logowanie bez uwierzytelniania wieloskładnikowego (na przykład przy użyciu hasła):

* `amr` ma `pwd` wartość:

    ![require_mfa_oidc_02. png](mfa/_static/require_mfa_oidc_02.png)

* Odmowa dostępu:

    ![require_mfa_oidc_03. png](mfa/_static/require_mfa_oidc_03.png)

Alternatywnie, logowanie przy użyciu uwierzytelniania OTP z Identity:

![require_mfa_oidc_01. png](mfa/_static/require_mfa_oidc_01.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Włącz generowanie kodu QR dla aplikacji TOTP Authenticator w ASP.NET Core](xref:security/authentication/identity-enable-qrcodes)
* [Opcje uwierzytelniania bezhasło dla Azure Active Directory](/azure/active-directory/authentication/concept-authentication-passwordless)
* [FIDO2 .NET Library for FIDO2/WebAuthn zaświadczanie i potwierdzenie przy użyciu platformy .NET](https://github.com/abergs/fido2-net-lib)
* [WebAuthn awesome](https://github.com/herrjemand/awesome-webauthn)
