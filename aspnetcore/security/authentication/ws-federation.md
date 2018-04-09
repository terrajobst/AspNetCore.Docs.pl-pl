---
title: Uwierzytelniać użytkowników za pomocą protokołu WS-Federation w ASP.NET Core
author: chlowell
description: W tym samouczku przedstawiono sposób użycia protokołu WS-Federation w aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: d4621c7b97678903b9f2562e353da3883334b599
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="71a98-103">Uwierzytelniać użytkowników za pomocą protokołu WS-Federation w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71a98-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="71a98-104">Ten samouczek pokazuje, jak umożliwić użytkownikom logowanie za pomocą dostawcy uwierzytelniania protokołu WS-Federation jak Active Directory Federation Services (ADFS) lub [usługi Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="71a98-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="71a98-105">Używa Przykładowa aplikacja platformy ASP.NET Core 2.0 opisanego w [Facebook, Google i zewnętrznego dostawcy uwierzytelniania](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="71a98-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="71a98-106">Dla aplikacji programu ASP.NET 2.0 Core Obsługa protokołu WS-Federation jest zapewniana przez [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="71a98-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="71a98-107">Ten składnik jest przenoszone z [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) i udostępnia wiele mechanika tego składnika.</span><span class="sxs-lookup"><span data-stu-id="71a98-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="71a98-108">Jednak składniki różnią się na kilka sposobów.</span><span class="sxs-lookup"><span data-stu-id="71a98-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="71a98-109">Domyślnie nowe oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="71a98-109">By default, the new middleware:</span></span>

* <span data-ttu-id="71a98-110">Nie zezwalaj na niechciane logowania.</span><span class="sxs-lookup"><span data-stu-id="71a98-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="71a98-111">Ta funkcja protokołu WS-Federation jest narażony na ataki XSRF.</span><span class="sxs-lookup"><span data-stu-id="71a98-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="71a98-112">Jednak można ją włączyć za `AllowUnsolicitedLogins` opcji.</span><span class="sxs-lookup"><span data-stu-id="71a98-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="71a98-113">Nie sprawdza co post formularza dla logowania w wiadomości.</span><span class="sxs-lookup"><span data-stu-id="71a98-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="71a98-114">Tylko żądania do `CallbackPath` są sprawdzane pod kątem logowania ubezp `CallbackPath` domyślnie `/signin-wsfed` , ale można zmienić.</span><span class="sxs-lookup"><span data-stu-id="71a98-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed.</span></span> <span data-ttu-id="71a98-115">Ta ścieżka może być udostępniane innym innych dostawców uwierzytelniania, włączając `SkipUnrecognizedRequests` opcji.</span><span class="sxs-lookup"><span data-stu-id="71a98-115">This path can be shared with other authentication providers by enabling the `SkipUnrecognizedRequests` option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="71a98-116">Rejestrowanie aplikacji w usłudze Active Directory</span><span class="sxs-lookup"><span data-stu-id="71a98-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="71a98-117">Usługi federacyjne Active Directory</span><span class="sxs-lookup"><span data-stu-id="71a98-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="71a98-118">Otwórz okno serwera **jednostki uzależnionej strony Kreatora dodawania relacji zaufania** z poziomu konsoli zarządzania usług AD FS:</span><span class="sxs-lookup"><span data-stu-id="71a98-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Dodaj Kreator zaufania uzależniona:-Zapraszamy!](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="71a98-120">Wybierz ręcznie wprowadź dane:</span><span class="sxs-lookup"><span data-stu-id="71a98-120">Choose to enter data manually:</span></span>

![Dodaj kreatora zaufania jednostki uzależnionej: Wybierz źródło danych](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="71a98-122">Wprowadź nazwę wyświetlaną dla jednostki uzależnionej.</span><span class="sxs-lookup"><span data-stu-id="71a98-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="71a98-123">Nazwa nie jest istotne dla aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="71a98-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="71a98-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) Brak obsługi szyfrowania tokenu, dlatego nie skonfigurować certyfikat szyfrowania tokenu:</span><span class="sxs-lookup"><span data-stu-id="71a98-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Dodaj kreatora zaufania jednostki uzależnionej: Konfigurowanie certyfikatu](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="71a98-126">Włącz obsługę protokołu WS-Federation Passive przy użyciu adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71a98-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="71a98-127">Sprawdź, czy port jest poprawna dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="71a98-127">Verify the port is correct for the app:</span></span>

![Dodaj kreatora zaufania jednostki uzależnionej: Skonfiguruj adresy URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="71a98-129">Musi to być adres URL HTTPS.</span><span class="sxs-lookup"><span data-stu-id="71a98-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="71a98-130">Usługi IIS Express zapewniają certyfikatu z podpisem własnym, odnośnie do hostowania aplikacji podczas tworzenia.</span><span class="sxs-lookup"><span data-stu-id="71a98-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="71a98-131">Kestrel wymaga certyfikatu ręcznej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="71a98-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="71a98-132">Zobacz [Kestrel dokumentacji](xref:fundamentals/servers/kestrel) więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="71a98-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="71a98-133">Kliknij przycisk **dalej** za pośrednictwem pozostałe kroki kreatora i **Zamknij** na końcu.</span><span class="sxs-lookup"><span data-stu-id="71a98-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="71a98-134">ASP.NET Core Identity wymaga **Identyfikatora nazwy** oświadczeń.</span><span class="sxs-lookup"><span data-stu-id="71a98-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="71a98-135">Dodaj jeden z **Edycja reguł oświadczeń** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="71a98-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Edycja reguł oświadczeń](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="71a98-137">W **przekształcenie Kreatorze dodawania reguły oświadczenia**, pozostaw wartość domyślną **wysyłanie atrybutów LDAP jako oświadczeń** wybrany szablon, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="71a98-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="71a98-138">Dodaj mapowanie reguły **nazwy konta SAM** atrybutu LDAP **Identyfikatora nazwy** oświadczeń wychodzących:</span><span class="sxs-lookup"><span data-stu-id="71a98-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Dodaj kreatora reguły przekształcania oświadczeń: Konfiguruj regułę dotyczącą oświadczeń](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="71a98-140">Kliknij przycisk **Zakończ** > **OK** w **Edycja reguł oświadczeń** okna.</span><span class="sxs-lookup"><span data-stu-id="71a98-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="71a98-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="71a98-141">Azure Active Directory</span></span>

* <span data-ttu-id="71a98-142">Przejdź do bloku rejestracji aplikacji dzierżawę usługi AAD.</span><span class="sxs-lookup"><span data-stu-id="71a98-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="71a98-143">Kliknij przycisk **nowej rejestracji aplikacji**:</span><span class="sxs-lookup"><span data-stu-id="71a98-143">Click **New application registration**:</span></span>

![Usługi Azure Active Directory: Rejestracje aplikacji](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="71a98-145">Wprowadź nazwę dla rejestracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71a98-145">Enter a name for the app registration.</span></span> <span data-ttu-id="71a98-146">Nie jest to ważne w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="71a98-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="71a98-147">Wprowadź adres URL aplikacji nasłuchuje jako **adres URL logowania**:</span><span class="sxs-lookup"><span data-stu-id="71a98-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: Tworzenie rejestracji aplikacji](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="71a98-149">Kliknij przycisk **punkty końcowe** i zanotuj **dokument metadanych usług federacyjnych** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="71a98-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="71a98-150">Jest to oprogramowanie pośredniczące WS-Federation `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="71a98-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Usługa Azure Active Directory: punkty końcowe](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="71a98-152">Przejdź do nowej rejestracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71a98-152">Navigate to the new app registration.</span></span> <span data-ttu-id="71a98-153">Kliknij przycisk **ustawienia** > **właściwości** i zanotuj **identyfikator URI aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="71a98-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="71a98-154">Jest to oprogramowanie pośredniczące WS-Federation `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="71a98-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Usługi Azure Active Directory: Właściwości rejestracji aplikacji](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="71a98-156">Dodaj WS-Federation jako dostawcy logowania zewnętrznego dla ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="71a98-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="71a98-157">Dodawanie zależności na [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) do projektu.</span><span class="sxs-lookup"><span data-stu-id="71a98-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="71a98-158">Dodaj WS-Federation do `Configure` metody w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="71a98-158">Add WS-Federation to the `Configure` method in *Startup.cs*:</span></span>

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="71a98-159">Zaloguj się za pomocą protokołu WS-Federation</span><span class="sxs-lookup"><span data-stu-id="71a98-159">Log in with WS-Federation</span></span>

<span data-ttu-id="71a98-160">Przejdź do aplikacji i kliknij przycisk **Zaloguj** znajdujące się w nagłówku nawigacji.</span><span class="sxs-lookup"><span data-stu-id="71a98-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="71a98-161">Istnieje możliwość logowania za pomocą WsFederation: ![strony logowania](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="71a98-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="71a98-162">Z usług AD FS jako dostawcę, przycisk przekierowuje do strony logowania usług AD FS: ![strony logowania usług AD FS](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="71a98-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="71a98-163">Usłudze Azure Active Directory jako dostawcy, przycisk przekierowuje do strony logowania usługi AAD: ![strony logowania usługi AAD](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="71a98-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="71a98-164">Pomyślne logowanie dla nowego użytkownika przekierowuje do strony rejestracji aplikacji: ![strony rejestru](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="71a98-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="71a98-165">Użyj protokołu WS-Federation bez tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71a98-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="71a98-166">Oprogramowania pośredniczącego protokołu WS-Federation może być używany bez tożsamości.</span><span class="sxs-lookup"><span data-stu-id="71a98-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="71a98-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="71a98-167">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
