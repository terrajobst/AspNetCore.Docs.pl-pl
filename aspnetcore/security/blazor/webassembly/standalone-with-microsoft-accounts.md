---
title: Zabezpieczanie ASP.NET Core Blazor autonomicznej aplikacji webassembly przy użyciu kont Microsoft
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: be73bec971f96bd64afc735a1ea750d47c7bc383
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219262"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a><span data-ttu-id="64a2d-102">Zabezpieczanie ASP.NET Core Blazor autonomicznej aplikacji webassembly przy użyciu kont Microsoft</span><span class="sxs-lookup"><span data-stu-id="64a2d-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Microsoft Accounts</span></span>

<span data-ttu-id="64a2d-103">Autorzy [Javier Calvarro Nelson](https://github.com/javiercn) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="64a2d-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="64a2d-104">Aby utworzyć Blazor autonomiczną aplikację webassembly, która korzysta [z kont Microsoft z usługą Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) do uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="64a2d-104">To create a Blazor WebAssembly standalone app that uses [Microsoft Accounts with Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) for authentication:</span></span>

1. [<span data-ttu-id="64a2d-105">Tworzenie dzierżawy usługi AAD i aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="64a2d-105">Create an AAD tenant and web application</span></span>](/azure/active-directory/develop/v2-overview)

   <span data-ttu-id="64a2d-106">Zarejestruj aplikację usługi AAD w **Azure Active Directory** > **rejestracje aplikacji** obszarze Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="64a2d-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

   <span data-ttu-id="64a2d-107">1\.</span><span class="sxs-lookup"><span data-stu-id="64a2d-107">1\.</span></span> <span data-ttu-id="64a2d-108">Podaj **nazwę** aplikacji (na przykład **Blazor klienta AAD**).</span><span class="sxs-lookup"><span data-stu-id="64a2d-108">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span><br>
   <span data-ttu-id="64a2d-109">2\.</span><span class="sxs-lookup"><span data-stu-id="64a2d-109">2\.</span></span> <span data-ttu-id="64a2d-110">W obszarze **obsługiwane typy kont**wybierz pozycję **konta w dowolnym katalogu organizacyjnym**.</span><span class="sxs-lookup"><span data-stu-id="64a2d-110">In **Supported account types**, select **Accounts in any organizational directory**.</span></span><br>
   <span data-ttu-id="64a2d-111">3 \.</span><span class="sxs-lookup"><span data-stu-id="64a2d-111">3\.</span></span> <span data-ttu-id="64a2d-112">Pozostaw pole listy rozwijanej **Identyfikator URI przekierowania** na wartość **Web**i podaj identyfikator URI przekierowania `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="64a2d-112">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span><br>
   <span data-ttu-id="64a2d-113">4\.</span><span class="sxs-lookup"><span data-stu-id="64a2d-113">4\.</span></span> <span data-ttu-id="64a2d-114">Wyłącz **uprawnienia** > **Przyznaj administratorowi OpenID Connect i offline_access** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="64a2d-114">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span><br>
   <span data-ttu-id="64a2d-115">5 \.</span><span class="sxs-lookup"><span data-stu-id="64a2d-115">5\.</span></span> <span data-ttu-id="64a2d-116">Wybierz pozycję **Zarejestruj**.</span><span class="sxs-lookup"><span data-stu-id="64a2d-116">Select **Register**.</span></span>

   <span data-ttu-id="64a2d-117">W obszarze **uwierzytelnianie** > **konfiguracjami platformy** > **sieci Web**:</span><span class="sxs-lookup"><span data-stu-id="64a2d-117">In **Authentication** > **Platform configurations** > **Web**:</span></span>

   <span data-ttu-id="64a2d-118">1\.</span><span class="sxs-lookup"><span data-stu-id="64a2d-118">1\.</span></span> <span data-ttu-id="64a2d-119">Upewnij się, że **Identyfikator URI przekierowania** `https://localhost:5001/authentication/login-callback` jest obecny.</span><span class="sxs-lookup"><span data-stu-id="64a2d-119">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span><br>
   <span data-ttu-id="64a2d-120">2\.</span><span class="sxs-lookup"><span data-stu-id="64a2d-120">2\.</span></span> <span data-ttu-id="64a2d-121">W przypadku **niejawnego przydzielenia**zaznacz pola wyboru dla **tokenów dostępu** i **tokenów identyfikatorów**.</span><span class="sxs-lookup"><span data-stu-id="64a2d-121">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span><br>
   <span data-ttu-id="64a2d-122">3 \.</span><span class="sxs-lookup"><span data-stu-id="64a2d-122">3\.</span></span> <span data-ttu-id="64a2d-123">Pozostałe wartości domyślne dla aplikacji są dopuszczalne dla tego środowiska.</span><span class="sxs-lookup"><span data-stu-id="64a2d-123">The remaining defaults for the app are acceptable for this experience.</span></span><br>
   <span data-ttu-id="64a2d-124">4\.</span><span class="sxs-lookup"><span data-stu-id="64a2d-124">4\.</span></span> <span data-ttu-id="64a2d-125">Wybierz ikonę **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="64a2d-125">Select the **Save** button.</span></span>

   <span data-ttu-id="64a2d-126">Zapisz identyfikator aplikacji (identyfikator klienta) (na przykład `11111111-1111-1111-1111-111111111111`).</span><span class="sxs-lookup"><span data-stu-id="64a2d-126">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

1. <span data-ttu-id="64a2d-127">Zastąp symbole zastępcze w poniższym poleceniu zapisanymi wcześniej informacjami i wykonaj polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="64a2d-127">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   <span data-ttu-id="64a2d-128">Aby określić lokalizację wyjściową, która tworzy folder projektu, jeśli nie istnieje, Uwzględnij opcję Output w poleceniu z ścieżką (na przykład `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="64a2d-128">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="64a2d-129">Nazwa folderu jest również częścią nazwy projektu.</span><span class="sxs-lookup"><span data-stu-id="64a2d-129">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="64a2d-130">Po utworzeniu aplikacji powinno być możliwe:</span><span class="sxs-lookup"><span data-stu-id="64a2d-130">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="64a2d-131">Zaloguj się do aplikacji przy użyciu konta Microsoft.</span><span class="sxs-lookup"><span data-stu-id="64a2d-131">Log into the app using a Microsoft Account.</span></span>
* <span data-ttu-id="64a2d-132">Zażądaj tokenów dostępu dla interfejsów API firmy Microsoft, wykorzystując takie samo podejście jak w przypadku autonomicznych aplikacji Blazor pod warunkiem, że aplikacja została prawidłowo skonfigurowana.</span><span class="sxs-lookup"><span data-stu-id="64a2d-132">Request access tokens for Microsoft APIs using the same approach as for standalone Blazor apps provided that you have configured the app correctly.</span></span> <span data-ttu-id="64a2d-133">Aby uzyskać więcej informacji, zobacz [Szybki Start: Konfigurowanie aplikacji do udostępniania interfejsów API sieci Web](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span><span class="sxs-lookup"><span data-stu-id="64a2d-133">For more information, see [Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="64a2d-134">Pakiet uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="64a2d-134">Authentication package</span></span>

<span data-ttu-id="64a2d-135">Gdy aplikacja zostanie utworzona w celu korzystania z kont służbowych (`SingleOrg`), aplikacja automatycznie otrzymuje odwołanie do pakietu dla [biblioteki uwierzytelniania firmy Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="64a2d-135">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="64a2d-136">Pakiet zawiera zestaw elementów podstawowych, które ułatwiają aplikacji uwierzytelnianie użytkowników i uzyskiwanie tokenów do wywoływania chronionych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="64a2d-136">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="64a2d-137">W przypadku dodawania uwierzytelniania do aplikacji ręcznie Dodaj pakiet do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="64a2d-137">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="64a2d-138">Zastąp `{VERSION}` w powyższym odwołaniu do pakietu w wersji pakietu `Microsoft.AspNetCore.Blazor.Templates` pokazanej w <xref:blazor/get-started> artykule.</span><span class="sxs-lookup"><span data-stu-id="64a2d-138">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="64a2d-139">Pakiet `Microsoft.Authentication.WebAssembly.Msal` w sposób przechodni dodaje pakiet `Microsoft.AspNetCore.Components.WebAssembly.Authentication` do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64a2d-139">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="64a2d-140">Obsługa usługi uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="64a2d-140">Authentication service support</span></span>

<span data-ttu-id="64a2d-141">Obsługa uwierzytelniania użytkowników jest rejestrowana w kontenerze usługi przy użyciu metody rozszerzenia `AddMsalAuthentication` dostarczonej przez pakiet `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="64a2d-141">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="64a2d-142">Ta metoda umożliwia skonfigurowanie wszystkich usług wymaganych przez aplikację do współpracy z dostawcą tożsamości (IP).</span><span class="sxs-lookup"><span data-stu-id="64a2d-142">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="64a2d-143">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="64a2d-143">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="64a2d-144">Metoda `AddMsalAuthentication` akceptuje wywołanie zwrotne w celu skonfigurowania parametrów wymaganych do uwierzytelnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64a2d-144">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="64a2d-145">Wartości wymagane do skonfigurowania aplikacji można uzyskać z konfiguracji kont Microsoft podczas rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64a2d-145">The values required for configuring the app can be obtained from the Microsoft Accounts configuration when you register the app.</span></span>

## <a name="index-page"></a><span data-ttu-id="64a2d-146">Strona indeksu</span><span class="sxs-lookup"><span data-stu-id="64a2d-146">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="64a2d-147">Składnik aplikacji</span><span class="sxs-lookup"><span data-stu-id="64a2d-147">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="64a2d-148">Składnik RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="64a2d-148">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="64a2d-149">Składnik LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="64a2d-149">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="64a2d-150">Składnik uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="64a2d-150">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="64a2d-151">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="64a2d-151">Additional resources</span></span>

* [<span data-ttu-id="64a2d-152">Szybki Start: rejestrowanie aplikacji na platformie tożsamości firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="64a2d-152">Quickstart: Register an application with the Microsoft identity platform</span></span>](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [<span data-ttu-id="64a2d-153">Szybki Start: Konfigurowanie aplikacji do udostępniania interfejsów API sieci Web</span><span class="sxs-lookup"><span data-stu-id="64a2d-153">Quickstart: Configure an application to expose web APIs</span></span>](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
