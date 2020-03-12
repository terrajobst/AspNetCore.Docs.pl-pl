---
title: Zabezpieczanie aplikacji hostowanej przez ASP.NET Core Blazor webassembly przy użyciu Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: 232a4247f8bea23eec3dc35cba4659c88887124d
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083882"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a><span data-ttu-id="1121c-102">Zabezpieczanie aplikacji hostowanej przez ASP.NET Core Blazor webassembly przy użyciu Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="1121c-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory B2C</span></span>

<span data-ttu-id="1121c-103">Autorzy [Javier Calvarro Nelson](https://github.com/javiercn) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1121c-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="1121c-104">W tym artykule opisano sposób tworzenia aplikacji autonomicznej Blazor webassembly, która używa [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) na potrzeby uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1121c-104">This article describes how to create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="1121c-105">Rejestrowanie aplikacji w AAD B2C i tworzenie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="1121c-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="1121c-106">Tworzenie dzierżawy</span><span class="sxs-lookup"><span data-stu-id="1121c-106">Create a tenant</span></span>

<span data-ttu-id="1121c-107">Postępuj zgodnie ze wskazówkami w [samouczku: Utwórz dzierżawcę Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-create-tenant) , aby utworzyć dzierżawę AAD B2C i zarejestrować następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="1121c-107">Follow the guidance in [Tutorial: Create an Azure Active Directory B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) to create an AAD B2C tenant and record the following information:</span></span>

* <span data-ttu-id="1121c-108">Wystąpienie AAD B2C (na przykład `https://contoso.b2clogin.com/`, które obejmuje końcowy ukośnik)</span><span class="sxs-lookup"><span data-stu-id="1121c-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span>
* <span data-ttu-id="1121c-109">AAD B2C domenę dzierżawy (na przykład `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="1121c-109">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="1121c-110">Rejestrowanie aplikacji interfejsu API serwera</span><span class="sxs-lookup"><span data-stu-id="1121c-110">Register a server API app</span></span>

<span data-ttu-id="1121c-111">Postępuj zgodnie ze wskazówkami w [samouczku: Zarejestruj aplikację w Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) , aby zarejestrować aplikację usługi AAD dla *aplikacji interfejsu API serwera* w obszarze **Azure Active Directory** > **rejestracje aplikacji** Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="1121c-111">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="1121c-112">Wybierz pozycję **Nowa rejestracja**.</span><span class="sxs-lookup"><span data-stu-id="1121c-112">Select **New registration**.</span></span>
1. <span data-ttu-id="1121c-113">Podaj **nazwę** aplikacji (na przykład **Blazor Server AAD B2C**).</span><span class="sxs-lookup"><span data-stu-id="1121c-113">Provide a **Name** for the app (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="1121c-114">W przypadku **obsługiwanych typów kont**wybierz pozycję **konta w dowolnym katalogu organizacyjnym lub dowolnym dostawcy tożsamości. Do uwierzytelniania użytkowników za pomocą Azure AD B2C.**</span><span class="sxs-lookup"><span data-stu-id="1121c-114">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="1121c-115">(wiele dzierżawców) dla tego środowiska.</span><span class="sxs-lookup"><span data-stu-id="1121c-115">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="1121c-116">*Aplikacja interfejsu API serwera* nie wymaga **identyfikatora URI przekierowania** w tym scenariuszu, więc pozostaw listę rozwijaną w **sieci Web** i nie wprowadzaj identyfikatora URI przekierowania.</span><span class="sxs-lookup"><span data-stu-id="1121c-116">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="1121c-117">Upewnij się, że **uprawnienia** > **Przyznaj administratorowi wartość OpenID Connect, a uprawnienia offline_access** są włączone.</span><span class="sxs-lookup"><span data-stu-id="1121c-117">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="1121c-118">Wybierz pozycję **Zarejestruj**.</span><span class="sxs-lookup"><span data-stu-id="1121c-118">Select **Register**.</span></span>

<span data-ttu-id="1121c-119">W obszarze **Uwidacznianie interfejsu API**:</span><span class="sxs-lookup"><span data-stu-id="1121c-119">In **Expose an API**:</span></span>

1. <span data-ttu-id="1121c-120">Wybierz polecenie **Dodaj zakres**.</span><span class="sxs-lookup"><span data-stu-id="1121c-120">Select **Add a scope**.</span></span>
1. <span data-ttu-id="1121c-121">Wybierz przycisk **Zapisz i kontynuuj**.</span><span class="sxs-lookup"><span data-stu-id="1121c-121">Select **Save and continue**.</span></span>
1. <span data-ttu-id="1121c-122">Podaj **nazwę zakresu** (na przykład `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="1121c-122">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="1121c-123">Podaj **nazwę wyświetlaną zgody administratora** (na przykład `Access API`).</span><span class="sxs-lookup"><span data-stu-id="1121c-123">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="1121c-124">Podaj **Opis zgody administratora** (na przykład `Allows the app to access server app API endpoints.`).</span><span class="sxs-lookup"><span data-stu-id="1121c-124">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="1121c-125">Upewnij się, że **stan** jest ustawiony na **włączone**.</span><span class="sxs-lookup"><span data-stu-id="1121c-125">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="1121c-126">Wybierz pozycję **Dodaj zakres**.</span><span class="sxs-lookup"><span data-stu-id="1121c-126">Select **Add scope**.</span></span>

<span data-ttu-id="1121c-127">Zapisz następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="1121c-127">Record the following information:</span></span>

* <span data-ttu-id="1121c-128">*Aplikacja interfejsu API serwera* Identyfikator aplikacji (identyfikator klienta) (na przykład `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="1121c-128">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="1121c-129">Identyfikator katalogu (identyfikator dzierżawy) (na przykład `222222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="1121c-129">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="1121c-130">*Aplikacja interfejsu API serwera* Identyfikator URI identyfikatora aplikacji (na przykład `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, Azure Portal może być wartością domyślną identyfikatora klienta).</span><span class="sxs-lookup"><span data-stu-id="1121c-130">*Server API app* App ID URI (for example, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, the Azure portal might default the value to the Client ID)</span></span>
* <span data-ttu-id="1121c-131">Zakres domyślny (na przykład `API.Access`)</span><span class="sxs-lookup"><span data-stu-id="1121c-131">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="1121c-132">Rejestrowanie aplikacji klienckiej</span><span class="sxs-lookup"><span data-stu-id="1121c-132">Register a client app</span></span>

<span data-ttu-id="1121c-133">Postępuj zgodnie ze wskazówkami w [samouczku: Zarejestruj aplikację w Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) ponownie, aby zarejestrować aplikację usługi AAD dla *aplikacji klienckiej* w obszarze **Azure Active Directory** > **rejestracje aplikacji** Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="1121c-133">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) again to register an AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="1121c-134">Wybierz pozycję **Nowa rejestracja**.</span><span class="sxs-lookup"><span data-stu-id="1121c-134">Select **New registration**.</span></span>
1. <span data-ttu-id="1121c-135">Podaj **nazwę** aplikacji (na przykład **Blazor AAD B2C klienta**).</span><span class="sxs-lookup"><span data-stu-id="1121c-135">Provide a **Name** for the app (for example, **Blazor Client AAD B2C**).</span></span>
1. <span data-ttu-id="1121c-136">W przypadku **obsługiwanych typów kont**wybierz pozycję **konta w dowolnym katalogu organizacyjnym lub dowolnym dostawcy tożsamości. Do uwierzytelniania użytkowników za pomocą Azure AD B2C.**</span><span class="sxs-lookup"><span data-stu-id="1121c-136">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="1121c-137">(wiele dzierżawców) dla tego środowiska.</span><span class="sxs-lookup"><span data-stu-id="1121c-137">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="1121c-138">Pozostaw pole listy rozwijanej **Identyfikator URI przekierowania** na wartość **Web**i podaj identyfikator URI przekierowania `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="1121c-138">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="1121c-139">Upewnij się, że **uprawnienia** > **Przyznaj administratorowi wartość OpenID Connect, a uprawnienia offline_access** są włączone.</span><span class="sxs-lookup"><span data-stu-id="1121c-139">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="1121c-140">Wybierz pozycję **Zarejestruj**.</span><span class="sxs-lookup"><span data-stu-id="1121c-140">Select **Register**.</span></span>

<span data-ttu-id="1121c-141">W obszarze **uwierzytelnianie** > **konfiguracjami platformy** > **sieci Web**:</span><span class="sxs-lookup"><span data-stu-id="1121c-141">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="1121c-142">Upewnij się, że **Identyfikator URI przekierowania** `https://localhost:5001/authentication/login-callback` jest obecny.</span><span class="sxs-lookup"><span data-stu-id="1121c-142">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="1121c-143">W przypadku **niejawnego przydzielenia**zaznacz pola wyboru dla **tokenów dostępu** i **tokenów identyfikatorów**.</span><span class="sxs-lookup"><span data-stu-id="1121c-143">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="1121c-144">Pozostałe wartości domyślne dla aplikacji są dopuszczalne dla tego środowiska.</span><span class="sxs-lookup"><span data-stu-id="1121c-144">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="1121c-145">Wybierz ikonę **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="1121c-145">Select the **Save** button.</span></span>

<span data-ttu-id="1121c-146">W **uprawnienia interfejsu API**:</span><span class="sxs-lookup"><span data-stu-id="1121c-146">In **API permissions**:</span></span>

1. <span data-ttu-id="1121c-147">Upewnij się, że aplikacja ma **Microsoft Graph** > uprawnienie **użytkownika. odczyt** .</span><span class="sxs-lookup"><span data-stu-id="1121c-147">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="1121c-148">Wybierz pozycję **Dodaj uprawnienia** , a następnie **Moje interfejsy API**.</span><span class="sxs-lookup"><span data-stu-id="1121c-148">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="1121c-149">Wybierz *aplikację interfejsu API serwera* z kolumny **Nazwa** (na przykład **Blazor Server AAD B2C**).</span><span class="sxs-lookup"><span data-stu-id="1121c-149">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="1121c-150">Otwórz listę **interfejsów API** .</span><span class="sxs-lookup"><span data-stu-id="1121c-150">Open the **API** list.</span></span>
1. <span data-ttu-id="1121c-151">Włącz dostęp do interfejsu API (na przykład `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="1121c-151">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="1121c-152">Wybierz pozycję **Dodaj uprawnienia**.</span><span class="sxs-lookup"><span data-stu-id="1121c-152">Select **Add permissions**.</span></span>
1. <span data-ttu-id="1121c-153">Wybierz przycisk **Udziel zawartości administratora dla {Nazwa dzierżawy}** .</span><span class="sxs-lookup"><span data-stu-id="1121c-153">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="1121c-154">Wybierz pozycję **Tak**, aby potwierdzić.</span><span class="sxs-lookup"><span data-stu-id="1121c-154">Select **Yes** to confirm.</span></span>

<span data-ttu-id="1121c-155">W > domowej **Azure AD B2C** > **przepływów użytkownika**:</span><span class="sxs-lookup"><span data-stu-id="1121c-155">In **Home** > **Azure AD B2C** > **User flows**:</span></span>

[<span data-ttu-id="1121c-156">Tworzenie przepływu użytkownika dotyczącego rejestracji i logowania</span><span class="sxs-lookup"><span data-stu-id="1121c-156">Create a sign-up and sign-in user flow</span></span>](/azure/active-directory-b2c/tutorial-create-user-flows)

<span data-ttu-id="1121c-157">Zapisz następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="1121c-157">Record the following information:</span></span>

* <span data-ttu-id="1121c-158">Zapisz identyfikator aplikacji *aplikacji klienta* (identyfikator klienta) (na przykład `33333333-3333-3333-3333-333333333333`).</span><span class="sxs-lookup"><span data-stu-id="1121c-158">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>
* <span data-ttu-id="1121c-159">Zapisz nazwę przepływu użytkownika tworzenia konta i logowania utworzonego dla aplikacji (na przykład `B2C_1_signupsignin`).</span><span class="sxs-lookup"><span data-stu-id="1121c-159">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="1121c-160">Tworzymy aplikację.</span><span class="sxs-lookup"><span data-stu-id="1121c-160">Create the app</span></span>

<span data-ttu-id="1121c-161">Zastąp symbole zastępcze w poniższym poleceniu zapisanymi wcześniej informacjami i wykonaj polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="1121c-161">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho -ssp "{SIGN UP OR SIGN IN POLICY}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="1121c-162">Aby określić lokalizację wyjściową, która tworzy folder projektu, jeśli nie istnieje, Uwzględnij opcję Output w poleceniu z ścieżką (na przykład `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="1121c-162">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="1121c-163">Nazwa folderu jest również częścią nazwy projektu.</span><span class="sxs-lookup"><span data-stu-id="1121c-163">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="1121c-164">Konfiguracja aplikacji serwera</span><span class="sxs-lookup"><span data-stu-id="1121c-164">Server app configuration</span></span>

<span data-ttu-id="1121c-165">*Ta sekcja dotyczy aplikacji **serwerowej** rozwiązania.*</span><span class="sxs-lookup"><span data-stu-id="1121c-165">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="1121c-166">Pakiet uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="1121c-166">Authentication package</span></span>

<span data-ttu-id="1121c-167">Obsługa uwierzytelniania i autoryzowania wywołań ASP.NET Core interfejsów API sieci Web jest zapewniana przez `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span><span class="sxs-lookup"><span data-stu-id="1121c-167">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="1121c-168">Obsługa usługi uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="1121c-168">Authentication service support</span></span>

<span data-ttu-id="1121c-169">Metoda `AddAuthentication` konfiguruje usługi uwierzytelniania w ramach aplikacji i konfiguruje procedurę obsługi okaziciela JWT jako domyślną metodę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1121c-169">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="1121c-170">Metoda `AddAzureADBearer` konfiguruje określone parametry w obsłudze okaziciela JWT wymagane do weryfikacji tokenów emitowanych przez Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="1121c-170">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="1121c-171">`UseAuthentication` i `UseAuthorization` upewnij się, że:</span><span class="sxs-lookup"><span data-stu-id="1121c-171">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="1121c-172">Aplikacja próbuje analizować tokeny i sprawdzać ich poprawność w żądaniach przychodzących.</span><span class="sxs-lookup"><span data-stu-id="1121c-172">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="1121c-173">Wszystkie żądania próbujące uzyskać dostęp do chronionego zasobu bez poprawnego poświadczenia nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="1121c-173">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="1121c-174">Ustawienia aplikacji</span><span class="sxs-lookup"><span data-stu-id="1121c-174">App settings</span></span>

<span data-ttu-id="1121c-175">Plik *appSettings. JSON* zawiera opcje konfigurowania procedury obsługi okaziciela JWT używanej do sprawdzania poprawności tokenów dostępu.</span><span class="sxs-lookup"><span data-stu-id="1121c-175">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "{API CLIENT ID}",
    "Domain": "{DOMAIN}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="1121c-176">Kontroler WeatherForecast</span><span class="sxs-lookup"><span data-stu-id="1121c-176">WeatherForecast controller</span></span>

<span data-ttu-id="1121c-177">Kontroler WeatherForecast (*controllers/WeatherForecastController. cs*) ujawnia chroniony interfejs API z atrybutem `[Authorize]` zastosowanym do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1121c-177">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="1121c-178">**Ważne** jest, aby zrozumieć, że:</span><span class="sxs-lookup"><span data-stu-id="1121c-178">It's **important** to understand that:</span></span>

* <span data-ttu-id="1121c-179">Atrybut `[Authorize]` w tym kontrolerze interfejsu API jest jedynym warunkiem, że ten interfejs API jest chroniony przed nieautoryzowanym dostępem.</span><span class="sxs-lookup"><span data-stu-id="1121c-179">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="1121c-180">Atrybut `[Authorize]` używany w aplikacji Blazor webassembly służy tylko jako Wskazówka dla aplikacji, której użytkownik powinien mieć autoryzację, aby aplikacja działała poprawnie.</span><span class="sxs-lookup"><span data-stu-id="1121c-180">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="1121c-181">Konfiguracja aplikacji klienta</span><span class="sxs-lookup"><span data-stu-id="1121c-181">Client app configuration</span></span>

<span data-ttu-id="1121c-182">*Ta sekcja dotyczy aplikacji **klienckiej** rozwiązania.*</span><span class="sxs-lookup"><span data-stu-id="1121c-182">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="1121c-183">Pakiet uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="1121c-183">Authentication package</span></span>

<span data-ttu-id="1121c-184">Gdy aplikacja zostanie utworzona w celu korzystania z pojedynczego konta B2C (`IndividualB2C`), aplikacja automatycznie otrzymuje odwołanie do pakietu dla [biblioteki uwierzytelniania firmy Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="1121c-184">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="1121c-185">Pakiet zawiera zestaw elementów podstawowych, które ułatwiają aplikacji uwierzytelnianie użytkowników i uzyskiwanie tokenów do wywoływania chronionych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="1121c-185">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="1121c-186">W przypadku dodawania uwierzytelniania do aplikacji ręcznie Dodaj pakiet do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1121c-186">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="1121c-187">Zastąp `{VERSION}` w powyższym odwołaniu do pakietu w wersji pakietu `Microsoft.AspNetCore.Blazor.Templates` pokazanej w <xref:blazor/get-started> artykule.</span><span class="sxs-lookup"><span data-stu-id="1121c-187">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="1121c-188">Pakiet `Microsoft.Authentication.WebAssembly.Msal` w sposób przechodni dodaje pakiet `Microsoft.AspNetCore.Components.WebAssembly.Authentication` do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1121c-188">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="1121c-189">Obsługa usługi uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="1121c-189">Authentication service support</span></span>

<span data-ttu-id="1121c-190">Obsługa uwierzytelniania użytkowników jest rejestrowana w kontenerze usługi przy użyciu metody rozszerzenia `AddMsalAuthentication` dostarczonej przez pakiet `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="1121c-190">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="1121c-191">Ta metoda umożliwia skonfigurowanie wszystkich usług wymaganych przez aplikację do współpracy z dostawcą tożsamości (IP).</span><span class="sxs-lookup"><span data-stu-id="1121c-191">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="1121c-192">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1121c-192">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{DEFAULT SCOPE}");
});
```

<span data-ttu-id="1121c-193">Metoda `AddMsalAuthentication` akceptuje wywołanie zwrotne w celu skonfigurowania parametrów wymaganych do uwierzytelnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1121c-193">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="1121c-194">Wartości wymagane do skonfigurowania aplikacji można uzyskać z konfiguracji usługi AAD w witrynie Azure Portal podczas rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1121c-194">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="1121c-195">Szablon Blazor webassembly automatycznie skonfiguruje aplikację do żądania tokenu dostępu dla bezpiecznego interfejsu API dla zakresu domyślnego dostarczonego do polecenia `dotnet new` (`{APP ID URI}/{DEFAULT SCOPE}`).</span><span class="sxs-lookup"><span data-stu-id="1121c-195">The Blazor WebAssembly template automatically configures the app to request an access token for a secure API for the default scope provided to the `dotnet new` command (`{APP ID URI}/{DEFAULT SCOPE}`).</span></span>

<span data-ttu-id="1121c-196">Domyślne zakresy tokenów dostępu reprezentują listę zakresów tokenów dostępu, które są następujące:</span><span class="sxs-lookup"><span data-stu-id="1121c-196">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="1121c-197">Uwzględnione domyślnie w żądaniu logowania.</span><span class="sxs-lookup"><span data-stu-id="1121c-197">Included by default in the sign in request.</span></span>
* <span data-ttu-id="1121c-198">Służy do aprowizacji tokenu dostępu zaraz po uwierzytelnieniu.</span><span class="sxs-lookup"><span data-stu-id="1121c-198">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="1121c-199">Wszystkie zakresy muszą należeć do tej samej aplikacji na Azure Active Directory reguł.</span><span class="sxs-lookup"><span data-stu-id="1121c-199">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="1121c-200">Dodatkowe zakresy można dodać do dodatkowych aplikacji interfejsu API w razie potrzeby:</span><span class="sxs-lookup"><span data-stu-id="1121c-200">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="1121c-201">Strona indeksu</span><span class="sxs-lookup"><span data-stu-id="1121c-201">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a><span data-ttu-id="1121c-202">Składnik aplikacji</span><span class="sxs-lookup"><span data-stu-id="1121c-202">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="1121c-203">Składnik RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="1121c-203">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="1121c-204">Składnik LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="1121c-204">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="1121c-205">Składnik uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="1121c-205">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="1121c-206">Składnik FetchData</span><span class="sxs-lookup"><span data-stu-id="1121c-206">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="1121c-207">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1121c-207">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="1121c-208">Samouczek: tworzenie dzierżawy usługi Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="1121c-208">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
