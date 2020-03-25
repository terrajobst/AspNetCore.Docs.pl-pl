---
title: Zabezpieczanie ASP.NET Core Blazor autonomicznej aplikacji webassembly przy użyciu Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: e12c38ed42a4e2714d785ef8f03097246c40d36e
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218985"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a><span data-ttu-id="a518b-102">Zabezpieczanie ASP.NET Core Blazor autonomicznej aplikacji webassembly przy użyciu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a518b-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory</span></span>

<span data-ttu-id="a518b-103">Autorzy [Javier Calvarro Nelson](https://github.com/javiercn) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a518b-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="a518b-104">Aby utworzyć Blazor autonomiczną aplikację webassembly, która używa [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) do uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="a518b-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication:</span></span>

<span data-ttu-id="a518b-105">[Utwórz dzierżawę usługi AAD i aplikację sieci Web](/azure/active-directory/develop/v2-overview):</span><span class="sxs-lookup"><span data-stu-id="a518b-105">[Create an AAD tenant and web application](/azure/active-directory/develop/v2-overview):</span></span>

<span data-ttu-id="a518b-106">Zarejestruj aplikację usługi AAD w **Azure Active Directory** > **rejestracje aplikacji** obszarze Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="a518b-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="a518b-107">Podaj **nazwę** aplikacji (na przykład **Blazor klienta AAD**).</span><span class="sxs-lookup"><span data-stu-id="a518b-107">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="a518b-108">Wybierz **obsługiwane typy kont**.</span><span class="sxs-lookup"><span data-stu-id="a518b-108">Choose a **Supported account types**.</span></span> <span data-ttu-id="a518b-109">**W tym katalogu organizacji można wybrać konta tylko** dla tego środowiska.</span><span class="sxs-lookup"><span data-stu-id="a518b-109">You may select **Accounts in this organizational directory only** for this experience.</span></span>
1. <span data-ttu-id="a518b-110">Pozostaw pole listy rozwijanej **Identyfikator URI przekierowania** na wartość **Web**i podaj identyfikator URI przekierowania `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="a518b-110">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="a518b-111">Wyłącz **uprawnienia** > **Przyznaj administratorowi OpenID Connect i offline_access** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="a518b-111">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="a518b-112">Wybierz pozycję **Zarejestruj**.</span><span class="sxs-lookup"><span data-stu-id="a518b-112">Select **Register**.</span></span>

<span data-ttu-id="a518b-113">W obszarze **uwierzytelnianie** > **konfiguracjami platformy** > **sieci Web**:</span><span class="sxs-lookup"><span data-stu-id="a518b-113">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="a518b-114">Upewnij się, że **Identyfikator URI przekierowania** `https://localhost:5001/authentication/login-callback` jest obecny.</span><span class="sxs-lookup"><span data-stu-id="a518b-114">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="a518b-115">W przypadku **niejawnego przydzielenia**zaznacz pola wyboru dla **tokenów dostępu** i **tokenów identyfikatorów**.</span><span class="sxs-lookup"><span data-stu-id="a518b-115">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="a518b-116">Pozostałe wartości domyślne dla aplikacji są dopuszczalne dla tego środowiska.</span><span class="sxs-lookup"><span data-stu-id="a518b-116">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="a518b-117">Wybierz ikonę **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="a518b-117">Select the **Save** button.</span></span>

<span data-ttu-id="a518b-118">Zapisz następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="a518b-118">Record the following information:</span></span>

* <span data-ttu-id="a518b-119">Identyfikator aplikacji (identyfikator klienta) (na przykład `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="a518b-119">Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="a518b-120">Identyfikator katalogu (identyfikator dzierżawy) (na przykład `22222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="a518b-120">Directory ID (Tenant ID) (for example, `22222222-2222-2222-2222-222222222222`)</span></span>

<span data-ttu-id="a518b-121">Zastąp symbole zastępcze w poniższym poleceniu zapisanymi wcześniej informacjami i wykonaj polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a518b-121">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="a518b-122">Aby określić lokalizację wyjściową, która tworzy folder projektu, jeśli nie istnieje, Uwzględnij opcję Output w poleceniu z ścieżką (na przykład `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="a518b-122">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="a518b-123">Nazwa folderu jest również częścią nazwy projektu.</span><span class="sxs-lookup"><span data-stu-id="a518b-123">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="a518b-124">Pakiet uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="a518b-124">Authentication package</span></span>

<span data-ttu-id="a518b-125">Gdy aplikacja zostanie utworzona w celu korzystania z kont służbowych (`SingleOrg`), aplikacja automatycznie otrzymuje odwołanie do pakietu dla [biblioteki uwierzytelniania firmy Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="a518b-125">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="a518b-126">Pakiet zawiera zestaw elementów podstawowych, które ułatwiają aplikacji uwierzytelnianie użytkowników i uzyskiwanie tokenów do wywoływania chronionych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="a518b-126">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="a518b-127">W przypadku dodawania uwierzytelniania do aplikacji ręcznie Dodaj pakiet do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="a518b-127">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="a518b-128">Zastąp `{VERSION}` w powyższym odwołaniu do pakietu w wersji pakietu `Microsoft.AspNetCore.Blazor.Templates` pokazanej w <xref:blazor/get-started> artykule.</span><span class="sxs-lookup"><span data-stu-id="a518b-128">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="a518b-129">Pakiet `Microsoft.Authentication.WebAssembly.Msal` w sposób przechodni dodaje pakiet `Microsoft.AspNetCore.Components.WebAssembly.Authentication` do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a518b-129">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="a518b-130">Obsługa usługi uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="a518b-130">Authentication service support</span></span>

<span data-ttu-id="a518b-131">Obsługa uwierzytelniania użytkowników jest rejestrowana w kontenerze usługi przy użyciu metody rozszerzenia `AddMsalAuthentication` dostarczonej przez pakiet `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="a518b-131">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="a518b-132">Ta metoda umożliwia skonfigurowanie wszystkich usług wymaganych przez aplikację do współpracy z dostawcą tożsamości (IP).</span><span class="sxs-lookup"><span data-stu-id="a518b-132">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="a518b-133">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a518b-133">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="a518b-134">Metoda `AddMsalAuthentication` akceptuje wywołanie zwrotne w celu skonfigurowania parametrów wymaganych do uwierzytelnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a518b-134">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="a518b-135">Wartości wymagane do skonfigurowania aplikacji można uzyskać z konfiguracji usługi AAD w witrynie Azure Portal podczas rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a518b-135">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="a518b-136">Szablon Blazor webassembly nie konfiguruje automatycznie aplikacji do żądania tokenu dostępu dla bezpiecznego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="a518b-136">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="a518b-137">Aby zainicjować obsługę administracyjną tokenu w ramach przepływu logowania, Dodaj zakres do domyślnych zakresów tokenów dostępu `MsalProviderOptions`:</span><span class="sxs-lookup"><span data-stu-id="a518b-137">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="a518b-138">Domyślny zakres tokenu dostępu musi mieć format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (na przykład `11111111-1111-1111-1111-111111111111/API.Access`).</span><span class="sxs-lookup"><span data-stu-id="a518b-138">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="a518b-139">Jeśli schemat lub schemat i Host są dostarczane do ustawienia zakresu (jak pokazano w witrynie Azure Portal), *aplikacja kliencka* zgłasza nieobsłużony wyjątek, gdy odbierze *401 nieautoryzowaną* odpowiedź z *aplikacji interfejsu API serwera*.</span><span class="sxs-lookup"><span data-stu-id="a518b-139">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

## <a name="index-page"></a><span data-ttu-id="a518b-140">Strona indeksu</span><span class="sxs-lookup"><span data-stu-id="a518b-140">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="a518b-141">Składnik aplikacji</span><span class="sxs-lookup"><span data-stu-id="a518b-141">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="a518b-142">Składnik RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="a518b-142">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="a518b-143">Składnik LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="a518b-143">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="a518b-144">Składnik uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="a518b-144">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="a518b-145">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a518b-145">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
