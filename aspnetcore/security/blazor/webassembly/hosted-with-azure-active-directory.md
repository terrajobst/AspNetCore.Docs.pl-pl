---
title: Zabezpieczanie aplikacji hostowanej przez ASP.NET Core Blazor webassembly przy użyciu Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 0803e436d66ef7df3c68739e674a8652fde11166
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083847"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a><span data-ttu-id="c537d-102">Zabezpieczanie aplikacji hostowanej przez ASP.NET Core Blazor webassembly przy użyciu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c537d-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory</span></span>

<span data-ttu-id="c537d-103">Autorzy [Javier Calvarro Nelson](https://github.com/javiercn) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c537d-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



<span data-ttu-id="c537d-104">W tym artykule opisano sposób tworzenia [aplikacji hostowanej w programieBlazor webassembly](xref:blazor/hosting-models#blazor-webassembly) , która używa [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) do uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="c537d-104">This article describes how to create a [Blazor WebAssembly hosted app](xref:blazor/hosting-models#blazor-webassembly) that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="c537d-105">Rejestrowanie aplikacji w AAD B2C i tworzenie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="c537d-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="c537d-106">Tworzenie dzierżawy</span><span class="sxs-lookup"><span data-stu-id="c537d-106">Create a tenant</span></span>

<span data-ttu-id="c537d-107">Postępuj zgodnie ze wskazówkami w [przewodniku szybki start: Konfigurowanie dzierżawy](/azure/active-directory/develop/quickstart-create-new-tenant) w celu utworzenia dzierżawy w usłudze AAD.</span><span class="sxs-lookup"><span data-stu-id="c537d-107">Follow the guidance in [Quickstart: Set up a tenant](/azure/active-directory/develop/quickstart-create-new-tenant) to create a tenant in AAD.</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="c537d-108">Rejestrowanie aplikacji interfejsu API serwera</span><span class="sxs-lookup"><span data-stu-id="c537d-108">Register a server API app</span></span>

<span data-ttu-id="c537d-109">Postępuj zgodnie ze wskazówkami w [przewodniku szybki start: Zarejestruj aplikację przy użyciu platformy tożsamości firmy Microsoft](/azure/active-directory/develop/quickstart-register-app) i kolejnych tematów usługi Azure AAD, aby zarejestrować aplikację w usłudze AAD dla *aplikacji interfejsu API serwera* w **Azure Active Directory** > **rejestracje aplikacji** obszarze Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="c537d-109">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="c537d-110">Wybierz pozycję **Nowa rejestracja**.</span><span class="sxs-lookup"><span data-stu-id="c537d-110">Select **New registration**.</span></span>
1. <span data-ttu-id="c537d-111">Podaj **nazwę** aplikacji (na przykład **Blazor Server AAD**).</span><span class="sxs-lookup"><span data-stu-id="c537d-111">Provide a **Name** for the app (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="c537d-112">Wybierz **obsługiwane typy kont**.</span><span class="sxs-lookup"><span data-stu-id="c537d-112">Choose a **Supported account types**.</span></span> <span data-ttu-id="c537d-113">W tym środowisku możesz wybrać **tylko konta w tym katalogu organizacji** (pojedynczy dzierżawca).</span><span class="sxs-lookup"><span data-stu-id="c537d-113">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="c537d-114">*Aplikacja interfejsu API serwera* nie wymaga **identyfikatora URI przekierowania** w tym scenariuszu, więc pozostaw listę rozwijaną w **sieci Web** i nie wprowadzaj identyfikatora URI przekierowania.</span><span class="sxs-lookup"><span data-stu-id="c537d-114">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="c537d-115">Wyłącz **uprawnienia** > **Przyznaj administratorowi OpenID Connect i offline_access** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="c537d-115">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="c537d-116">Wybierz pozycję **Zarejestruj**.</span><span class="sxs-lookup"><span data-stu-id="c537d-116">Select **Register**.</span></span>

<span data-ttu-id="c537d-117">W obszarze **uprawnienia interfejsu API**usuń uprawnienie **Microsoft Graph** > **User. Read** , ponieważ aplikacja nie wymaga dostępu do profilu Logowanie lub UER.</span><span class="sxs-lookup"><span data-stu-id="c537d-117">In **API permissions**, remove the **Microsoft Graph** > **User.Read** permission, as the app doesn't require sign in or uer profile access.</span></span>

<span data-ttu-id="c537d-118">W obszarze **Uwidacznianie interfejsu API**:</span><span class="sxs-lookup"><span data-stu-id="c537d-118">In **Expose an API**:</span></span>

1. <span data-ttu-id="c537d-119">Wybierz polecenie **Dodaj zakres**.</span><span class="sxs-lookup"><span data-stu-id="c537d-119">Select **Add a scope**.</span></span>
1. <span data-ttu-id="c537d-120">Wybierz przycisk **Zapisz i kontynuuj**.</span><span class="sxs-lookup"><span data-stu-id="c537d-120">Select **Save and continue**.</span></span>
1. <span data-ttu-id="c537d-121">Podaj **nazwę zakresu** (na przykład `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="c537d-121">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="c537d-122">Podaj **nazwę wyświetlaną zgody administratora** (na przykład `Access API`).</span><span class="sxs-lookup"><span data-stu-id="c537d-122">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="c537d-123">Podaj **Opis zgody administratora** (na przykład `Allows the app to access server app API endpoints.`).</span><span class="sxs-lookup"><span data-stu-id="c537d-123">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="c537d-124">Upewnij się, że **stan** jest ustawiony na **włączone**.</span><span class="sxs-lookup"><span data-stu-id="c537d-124">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="c537d-125">Wybierz pozycję **Dodaj zakres**.</span><span class="sxs-lookup"><span data-stu-id="c537d-125">Select **Add scope**.</span></span>

<span data-ttu-id="c537d-126">Zapisz następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="c537d-126">Record the following information:</span></span>

* <span data-ttu-id="c537d-127">*Aplikacja interfejsu API serwera* Identyfikator aplikacji (identyfikator klienta) (na przykład `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="c537d-127">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="c537d-128">Identyfikator katalogu (identyfikator dzierżawy) (na przykład `222222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="c537d-128">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="c537d-129">Domena dzierżawy usługi AAD (na przykład `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="c537d-129">AAD Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>
* <span data-ttu-id="c537d-130">Zakres domyślny (na przykład `API.Access`)</span><span class="sxs-lookup"><span data-stu-id="c537d-130">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="c537d-131">Rejestrowanie aplikacji klienckiej</span><span class="sxs-lookup"><span data-stu-id="c537d-131">Register a client app</span></span>

<span data-ttu-id="c537d-132">Postępuj zgodnie ze wskazówkami w [przewodniku szybki start: Zarejestruj aplikację przy użyciu platformy tożsamości firmy Microsoft](/azure/active-directory/develop/quickstart-register-app) i kolejnych tematów usługi Azure AAD, aby zarejestrować aplikację w usłudze AAD dla *aplikacji klienckiej* w obszarze **Azure Active Directory** > **rejestracje aplikacji** Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="c537d-132">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register a AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="c537d-133">Wybierz pozycję **Nowa rejestracja**.</span><span class="sxs-lookup"><span data-stu-id="c537d-133">Select **New registration**.</span></span>
1. <span data-ttu-id="c537d-134">Podaj **nazwę** aplikacji (na przykład **Blazor klienta AAD**).</span><span class="sxs-lookup"><span data-stu-id="c537d-134">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="c537d-135">Wybierz **obsługiwane typy kont**.</span><span class="sxs-lookup"><span data-stu-id="c537d-135">Choose a **Supported account types**.</span></span> <span data-ttu-id="c537d-136">W tym środowisku możesz wybrać **tylko konta w tym katalogu organizacji** (pojedynczy dzierżawca).</span><span class="sxs-lookup"><span data-stu-id="c537d-136">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="c537d-137">Pozostaw pole listy rozwijanej **Identyfikator URI przekierowania** na wartość **Web**i podaj identyfikator URI przekierowania `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="c537d-137">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="c537d-138">Wyłącz **uprawnienia** > **Przyznaj administratorowi OpenID Connect i offline_access** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="c537d-138">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="c537d-139">Wybierz pozycję **Zarejestruj**.</span><span class="sxs-lookup"><span data-stu-id="c537d-139">Select **Register**.</span></span>

<span data-ttu-id="c537d-140">W obszarze **uwierzytelnianie** > **konfiguracjami platformy** > **sieci Web**:</span><span class="sxs-lookup"><span data-stu-id="c537d-140">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="c537d-141">Upewnij się, że **Identyfikator URI przekierowania** `https://localhost:5001/authentication/login-callback` jest obecny.</span><span class="sxs-lookup"><span data-stu-id="c537d-141">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="c537d-142">W przypadku **niejawnego przydzielenia**zaznacz pola wyboru dla **tokenów dostępu** i **tokenów identyfikatorów**.</span><span class="sxs-lookup"><span data-stu-id="c537d-142">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="c537d-143">Pozostałe wartości domyślne dla aplikacji są dopuszczalne dla tego środowiska.</span><span class="sxs-lookup"><span data-stu-id="c537d-143">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="c537d-144">Wybierz ikonę **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="c537d-144">Select the **Save** button.</span></span>

<span data-ttu-id="c537d-145">W **uprawnienia interfejsu API**:</span><span class="sxs-lookup"><span data-stu-id="c537d-145">In **API permissions**:</span></span>

1. <span data-ttu-id="c537d-146">Upewnij się, że aplikacja ma **Microsoft Graph** > uprawnienie **użytkownika. odczyt** .</span><span class="sxs-lookup"><span data-stu-id="c537d-146">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="c537d-147">Wybierz pozycję **Dodaj uprawnienia** , a następnie **Moje interfejsy API**.</span><span class="sxs-lookup"><span data-stu-id="c537d-147">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="c537d-148">Wybierz *aplikację interfejsu API serwera* z kolumny **Nazwa** (na przykład **Blazor Server AAD**).</span><span class="sxs-lookup"><span data-stu-id="c537d-148">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="c537d-149">Otwórz listę **interfejsów API** .</span><span class="sxs-lookup"><span data-stu-id="c537d-149">Open the **API** list.</span></span>
1. <span data-ttu-id="c537d-150">Włącz dostęp do interfejsu API (na przykład `API.Access`).</span><span class="sxs-lookup"><span data-stu-id="c537d-150">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="c537d-151">Wybierz pozycję **Dodaj uprawnienia**.</span><span class="sxs-lookup"><span data-stu-id="c537d-151">Select **Add permissions**.</span></span>
1. <span data-ttu-id="c537d-152">Wybierz przycisk **Udziel zawartości administratora dla {Nazwa dzierżawy}** .</span><span class="sxs-lookup"><span data-stu-id="c537d-152">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="c537d-153">Wybierz pozycję **Tak**, aby potwierdzić.</span><span class="sxs-lookup"><span data-stu-id="c537d-153">Select **Yes** to confirm.</span></span>

<span data-ttu-id="c537d-154">Zapisz identyfikator aplikacji *aplikacji klienta* (identyfikator klienta) (na przykład `33333333-3333-3333-3333-333333333333`).</span><span class="sxs-lookup"><span data-stu-id="c537d-154">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="c537d-155">Tworzymy aplikację.</span><span class="sxs-lookup"><span data-stu-id="c537d-155">Create the app</span></span>

<span data-ttu-id="c537d-156">Zastąp symbole zastępcze w poniższym poleceniu zapisanymi wcześniej informacjami i wykonaj polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="c537d-156">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

<span data-ttu-id="c537d-157">Aby określić lokalizację wyjściową, która tworzy folder projektu, jeśli nie istnieje, Uwzględnij opcję Output w poleceniu z ścieżką (na przykład `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="c537d-157">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="c537d-158">Nazwa folderu jest również częścią nazwy projektu.</span><span class="sxs-lookup"><span data-stu-id="c537d-158">The folder name also becomes part of the project's name.</span></span>

> [!NOTE]
> <span data-ttu-id="c537d-159">Zapoznaj się z sekcją [Obsługa usługi uwierzytelniania](#Authentication service support) , aby uzyskać istotną zmianę konfiguracji w zakresie domyślnego tokenu dostępu.</span><span class="sxs-lookup"><span data-stu-id="c537d-159">See the [Authentication service support](#Authentication service support) section for an important configuration change to the default access token scope.</span></span> <span data-ttu-id="c537d-160">Wartość dostarczona przez szablon Blazor webassembly musi zostać zmieniona ręcznie po utworzeniu *aplikacji klienckiej* na podstawie szablonu.</span><span class="sxs-lookup"><span data-stu-id="c537d-160">The value provided by the Blazor WebAssembly template must be manually changed after the *Client app* is created from the template.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="c537d-161">Konfiguracja aplikacji serwera</span><span class="sxs-lookup"><span data-stu-id="c537d-161">Server app configuration</span></span>

<span data-ttu-id="c537d-162">*Ta sekcja dotyczy aplikacji **serwerowej** rozwiązania.*</span><span class="sxs-lookup"><span data-stu-id="c537d-162">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="c537d-163">Pakiet uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="c537d-163">Authentication package</span></span>

<span data-ttu-id="c537d-164">Obsługa uwierzytelniania i autoryzowania wywołań ASP.NET Core interfejsów API sieci Web jest zapewniana przez `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span><span class="sxs-lookup"><span data-stu-id="c537d-164">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="c537d-165">Obsługa usługi uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="c537d-165">Authentication service support</span></span>

<span data-ttu-id="c537d-166">Metoda `AddAuthentication` konfiguruje usługi uwierzytelniania w ramach aplikacji i konfiguruje procedurę obsługi okaziciela JWT jako domyślną metodę uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="c537d-166">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="c537d-167">Metoda `AddAzureADBearer` konfiguruje określone parametry w obsłudze okaziciela JWT wymagane do weryfikacji tokenów emitowanych przez Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="c537d-167">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="c537d-168">`UseAuthentication` i `UseAuthorization` upewnij się, że:</span><span class="sxs-lookup"><span data-stu-id="c537d-168">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="c537d-169">Aplikacja próbuje analizować tokeny i sprawdzać ich poprawność w żądaniach przychodzących.</span><span class="sxs-lookup"><span data-stu-id="c537d-169">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="c537d-170">Wszystkie żądania próbujące uzyskać dostęp do chronionego zasobu bez poprawnego poświadczenia nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="c537d-170">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="c537d-171">Ustawienia aplikacji</span><span class="sxs-lookup"><span data-stu-id="c537d-171">App settings</span></span>

<span data-ttu-id="c537d-172">Plik *appSettings. JSON* zawiera opcje konfigurowania procedury obsługi okaziciela JWT używanej do sprawdzania poprawności tokenów dostępu.</span><span class="sxs-lookup"><span data-stu-id="c537d-172">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="c537d-173">Kontroler WeatherForecast</span><span class="sxs-lookup"><span data-stu-id="c537d-173">WeatherForecast controller</span></span>

<span data-ttu-id="c537d-174">Kontroler WeatherForecast (*controllers/WeatherForecastController. cs*) ujawnia chroniony interfejs API z atrybutem `[Authorize]` zastosowanym do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c537d-174">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="c537d-175">**Ważne** jest, aby zrozumieć, że:</span><span class="sxs-lookup"><span data-stu-id="c537d-175">It's **important** to understand that:</span></span>

* <span data-ttu-id="c537d-176">Atrybut `[Authorize]` w tym kontrolerze interfejsu API jest jedynym warunkiem, że ten interfejs API jest chroniony przed nieautoryzowanym dostępem.</span><span class="sxs-lookup"><span data-stu-id="c537d-176">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="c537d-177">Atrybut `[Authorize]` używany w aplikacji Blazor webassembly służy tylko jako Wskazówka dla aplikacji, której użytkownik powinien mieć autoryzację, aby aplikacja działała poprawnie.</span><span class="sxs-lookup"><span data-stu-id="c537d-177">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

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

## <a name="client-app-configuration"></a><span data-ttu-id="c537d-178">Konfiguracja aplikacji klienta</span><span class="sxs-lookup"><span data-stu-id="c537d-178">Client app configuration</span></span>

<span data-ttu-id="c537d-179">*Ta sekcja dotyczy aplikacji **klienckiej** rozwiązania.*</span><span class="sxs-lookup"><span data-stu-id="c537d-179">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="c537d-180">Pakiet uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="c537d-180">Authentication package</span></span>

<span data-ttu-id="c537d-181">Gdy aplikacja zostanie utworzona w celu korzystania z kont służbowych (`SingleOrg`), aplikacja automatycznie otrzymuje odwołanie do pakietu dla [biblioteki uwierzytelniania firmy Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="c537d-181">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="c537d-182">Pakiet zawiera zestaw elementów podstawowych, które ułatwiają aplikacji uwierzytelnianie użytkowników i uzyskiwanie tokenów do wywoływania chronionych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="c537d-182">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="c537d-183">W przypadku dodawania uwierzytelniania do aplikacji ręcznie Dodaj pakiet do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c537d-183">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="c537d-184">Zastąp `{VERSION}` w powyższym odwołaniu do pakietu w wersji pakietu `Microsoft.AspNetCore.Blazor.Templates` pokazanej w <xref:blazor/get-started> artykule.</span><span class="sxs-lookup"><span data-stu-id="c537d-184">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="c537d-185">Pakiet `Microsoft.Authentication.WebAssembly.Msal` w sposób przechodni dodaje pakiet `Microsoft.AspNetCore.Components.WebAssembly.Authentication` do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c537d-185">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="c537d-186">Obsługa usługi uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="c537d-186">Authentication service support</span></span>

<span data-ttu-id="c537d-187">Obsługa uwierzytelniania użytkowników jest rejestrowana w kontenerze usługi przy użyciu metody rozszerzenia `AddMsalAuthentication` dostarczonej przez pakiet `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="c537d-187">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="c537d-188">Ta metoda umożliwia skonfigurowanie wszystkich usług wymaganych przez aplikację do współpracy z dostawcą tożsamości (IP).</span><span class="sxs-lookup"><span data-stu-id="c537d-188">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="c537d-189">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c537d-189">*Program.cs*:</span></span>

<span data-ttu-id="c537d-190">Po wygenerowaniu *aplikacji klienckiej* domyślny zakres tokenów dostępu ma format `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span><span class="sxs-lookup"><span data-stu-id="c537d-190">When the *Client app* is generated, the default access token scope is of the format `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span></span> <span data-ttu-id="c537d-191">**Usuń część `api://` wartości zakresu.**</span><span class="sxs-lookup"><span data-stu-id="c537d-191">**Remove the `api://` portion of the scope value.**</span></span> <span data-ttu-id="c537d-192">Ten problem zostanie rozwiązany w przyszłej wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="c537d-192">This issue will be addressed in a future preview release.</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="c537d-193">Domyślny zakres tokenu dostępu musi mieć format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (na przykład `11111111-1111-1111-1111-111111111111/API.Access`).</span><span class="sxs-lookup"><span data-stu-id="c537d-193">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="c537d-194">Jeśli schemat lub schemat i Host są dostarczane do ustawienia zakresu (jak pokazano w witrynie Azure Portal), *aplikacja kliencka* zgłasza nieobsłużony wyjątek, gdy odbierze *401 nieautoryzowaną* odpowiedź z *aplikacji interfejsu API serwera*.</span><span class="sxs-lookup"><span data-stu-id="c537d-194">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

<span data-ttu-id="c537d-195">Metoda `AddMsalAuthentication` akceptuje wywołanie zwrotne w celu skonfigurowania parametrów wymaganych do uwierzytelnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c537d-195">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="c537d-196">Wartości wymagane do skonfigurowania aplikacji można uzyskać z konfiguracji usługi AAD w witrynie Azure Portal podczas rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c537d-196">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="c537d-197">Domyślne zakresy tokenów dostępu reprezentują listę zakresów tokenów dostępu, które są następujące:</span><span class="sxs-lookup"><span data-stu-id="c537d-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="c537d-198">Uwzględnione domyślnie w żądaniu logowania.</span><span class="sxs-lookup"><span data-stu-id="c537d-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="c537d-199">Służy do aprowizacji tokenu dostępu zaraz po uwierzytelnieniu.</span><span class="sxs-lookup"><span data-stu-id="c537d-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="c537d-200">Wszystkie zakresy muszą należeć do tej samej aplikacji na Azure Active Directory reguł.</span><span class="sxs-lookup"><span data-stu-id="c537d-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="c537d-201">Dodatkowe zakresy można dodać do dodatkowych aplikacji interfejsu API w razie potrzeby:</span><span class="sxs-lookup"><span data-stu-id="c537d-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="c537d-202">Strona indeksu</span><span class="sxs-lookup"><span data-stu-id="c537d-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a><span data-ttu-id="c537d-203">Składnik aplikacji</span><span class="sxs-lookup"><span data-stu-id="c537d-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="c537d-204">Składnik RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="c537d-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="c537d-205">Składnik LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="c537d-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="c537d-206">Składnik uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="c537d-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="c537d-207">Składnik FetchData</span><span class="sxs-lookup"><span data-stu-id="c537d-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="c537d-208">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c537d-208">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
