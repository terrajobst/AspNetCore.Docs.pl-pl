---
title: Uwierzytelnianie użytkowników za pomocą protokołu WS-Federation w programie ASP.NET Core
author: chlowell
description: Ten samouczek pokazuje, jak używać protokołu WS-Federation w aplikacji ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: 6b568928aaf6c958d66279af9fef80ac0c968c8b
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396158"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="8a190-103">Uwierzytelnianie użytkowników za pomocą protokołu WS-Federation w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a190-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="8a190-104">W tym samouczku pokazano, jak umożliwić użytkownikom logowanie za pomocą dostawcy uwierzytelniania protokołu WS-Federation takich jak Active Directory Federation Services (ADFS) lub [usługi Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="8a190-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="8a190-105">Używa ona przykładową aplikację platformy ASP.NET Core 2.0 opisano w [Facebook, Google i uwierzytelniania zewnętrznego dostawcy](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8a190-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="8a190-106">W przypadku aplikacji ASP.NET Core 2.0, WS-Federation pomoc techniczną świadczy [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="8a190-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="8a190-107">Ten składnik jest przenoszone z [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) i udostępnia wiele mechanics tego składnika.</span><span class="sxs-lookup"><span data-stu-id="8a190-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="8a190-108">Jednak składniki różnią się na kilka sposobów.</span><span class="sxs-lookup"><span data-stu-id="8a190-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="8a190-109">Domyślnie nowe oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="8a190-109">By default, the new middleware:</span></span>

* <span data-ttu-id="8a190-110">Nie zezwalaj na niechciane logowania.</span><span class="sxs-lookup"><span data-stu-id="8a190-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="8a190-111">Ta funkcja protokołu WS-Federation jest narażony na ataki XSRF.</span><span class="sxs-lookup"><span data-stu-id="8a190-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="8a190-112">Jednak można ją włączyć za pomocą `AllowUnsolicitedLogins` opcji.</span><span class="sxs-lookup"><span data-stu-id="8a190-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="8a190-113">Nie sprawdza co post formularza logowania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="8a190-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="8a190-114">Tylko żądania `CallbackPath` są sprawdzane pod kątem logowania ubezp `CallbackPath` wartość domyślna to `/signin-wsfed` , ale można zmienić za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [ WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) klasy.</span><span class="sxs-lookup"><span data-stu-id="8a190-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) class.</span></span> <span data-ttu-id="8a190-115">Tej ścieżki mogą być współużytkowane z innymi dostawcami uwierzytelniania przez włączenie [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) opcji.</span><span class="sxs-lookup"><span data-stu-id="8a190-115">This path can be shared with other authentication providers by enabling the [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="8a190-116">Rejestrowanie aplikacji w usłudze Active Directory</span><span class="sxs-lookup"><span data-stu-id="8a190-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="8a190-117">Active Directory Federation Services</span><span class="sxs-lookup"><span data-stu-id="8a190-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="8a190-118">Otwórz okno serwera **jednostki uzależnionej strony zaufania Kreatora dodawania** z poziomu konsoli zarządzania usług AD FS:</span><span class="sxs-lookup"><span data-stu-id="8a190-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Dodaj jednostki uzależnionej zaufania kreatora: Witaj](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="8a190-120">Wybierz ręcznie wprowadź dane jednostki:</span><span class="sxs-lookup"><span data-stu-id="8a190-120">Choose to enter data manually:</span></span>

![Dodaj jednostki uzależnionej zaufania kreatora: Wybierz źródło danych](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="8a190-122">Wprowadź nazwę wyświetlaną dla jednostki uzależnionej.</span><span class="sxs-lookup"><span data-stu-id="8a190-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="8a190-123">Nazwa nie jest istotne dla aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8a190-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="8a190-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) brakuje obsługi szyfrowania tokenu, dlatego nie należy konfigurować certyfikat szyfrowania tokenu:</span><span class="sxs-lookup"><span data-stu-id="8a190-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Dodaj jednostki uzależnionej zaufania kreatora: Konfigurowanie certyfikatu](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="8a190-126">Włącz obsługę protokołu WS-Federation Passive przy użyciu adresu URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8a190-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="8a190-127">Sprawdź, czy port jest poprawna dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8a190-127">Verify the port is correct for the app:</span></span>

![Dodaj jednostki uzależnionej zaufania kreatora: Skonfiguruj adres URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="8a190-129">Musi to być adres URL HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8a190-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="8a190-130">Usługi IIS Express może zapewnić certyfikat z podpisem własnym, odnośnie do hostowania aplikacji podczas programowania.</span><span class="sxs-lookup"><span data-stu-id="8a190-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="8a190-131">Kestrel wymaga certyfikatu ręcznej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8a190-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="8a190-132">Zobacz [dokumentacji Kestrel](xref:fundamentals/servers/kestrel) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="8a190-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="8a190-133">Kliknij przycisk **dalej** przez pozostałą część kreatora i **Zamknij** na końcu.</span><span class="sxs-lookup"><span data-stu-id="8a190-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="8a190-134">Tożsamości platformy ASP.NET Core wymaga **identyfikator nazwy** oświadczenia.</span><span class="sxs-lookup"><span data-stu-id="8a190-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="8a190-135">Dodaj jeden z **Edycja reguł oświadczeń** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="8a190-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Edytuj reguły oświadczeń](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="8a190-137">W **oświadczenia Kreatorze dodawania reguły przekształcania**, pozostaw wartość domyślną **wysyłać atrybuty LDAP jako oświadczenia** wybrany szablon, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="8a190-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="8a190-138">Dodaj mapowanie reguły **nazwy konta SAM** atrybutu LDAP, aby **identyfikator nazwy** oświadczenie wychodzące:</span><span class="sxs-lookup"><span data-stu-id="8a190-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Dodaj kreatora reguły oświadczeń przekształcenia: Konfiguruj regułę dotyczącą oświadczeń](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="8a190-140">Kliknij przycisk **Zakończ** > **OK** w **Edycja reguł oświadczeń** okna.</span><span class="sxs-lookup"><span data-stu-id="8a190-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="8a190-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8a190-141">Azure Active Directory</span></span>

* <span data-ttu-id="8a190-142">Przejdź do bloku rejestracje aplikacji dzierżawy usługi AAD.</span><span class="sxs-lookup"><span data-stu-id="8a190-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="8a190-143">Kliknij przycisk **rejestrowanie nowej aplikacji**:</span><span class="sxs-lookup"><span data-stu-id="8a190-143">Click **New application registration**:</span></span>

![Azure Active Directory: Rejestracje aplikacji](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="8a190-145">Wprowadź nazwę dla rejestracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8a190-145">Enter a name for the app registration.</span></span> <span data-ttu-id="8a190-146">Nie jest to istotne dla aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8a190-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="8a190-147">Wprowadź adres URL aplikacji nasłuchuje jako **adres URL logowania**:</span><span class="sxs-lookup"><span data-stu-id="8a190-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: Tworzenie rejestracji aplikacji](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="8a190-149">Kliknij przycisk **punktów końcowych** i zanotuj **dokument metadanych Federacji** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8a190-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="8a190-150">To jest oprogramowanie pośredniczące WS-Federation `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="8a190-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory: Punkty końcowe](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="8a190-152">Przejdź do nowej rejestracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8a190-152">Navigate to the new app registration.</span></span> <span data-ttu-id="8a190-153">Kliknij przycisk **ustawienia** > **właściwości** i zanotuj **identyfikator URI Identyfikatora aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="8a190-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="8a190-154">To jest oprogramowanie pośredniczące WS-Federation `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="8a190-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory: Właściwości rejestracji aplikacji](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="8a190-156">Dodawanie usługi WS-Federation jako dostawcy logowania zewnętrznego dla tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a190-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="8a190-157">Dodać zależność od [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) do projektu.</span><span class="sxs-lookup"><span data-stu-id="8a190-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="8a190-158">Dodaj WS-Federation na `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8a190-158">Add WS-Federation to `Startup.ConfigureServices`:</span></span>

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

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="8a190-159">Zaloguj się przy użyciu protokołu WS-Federation</span><span class="sxs-lookup"><span data-stu-id="8a190-159">Log in with WS-Federation</span></span>

<span data-ttu-id="8a190-160">Przejdź do aplikacji i kliknij pozycję **Zaloguj** łącze w nagłówku nawigacji.</span><span class="sxs-lookup"><span data-stu-id="8a190-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="8a190-161">Istnieje możliwość Zaloguj się przy użyciu WsFederation: ![Zaloguj się na stronie](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="8a190-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="8a190-162">Za pomocą usług AD FS jako dostawcy przycisk wykonuje przekierowanie do strony logowania usług AD FS: ![Strony logowania usług AD FS](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="8a190-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="8a190-163">Za pomocą usługi Azure Active Directory jako dostawcy przycisk wykonuje przekierowanie do strony logowania usługi AAD: ![Strona logowania usługi AAD](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="8a190-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="8a190-164">Pomyślne logowanie dla nowego użytkownika przekierowuje do strony rejestracji aplikacji: ![Strona rejestracji](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="8a190-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="8a190-165">Użyj protokołu WS-Federation, bez użycia produktu ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="8a190-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="8a190-166">Oprogramowanie pośredniczące WS-Federation, można bez użycia produktu Identity.</span><span class="sxs-lookup"><span data-stu-id="8a190-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="8a190-167">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8a190-167">For example:</span></span>

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
