---
title: Zabezpieczanie ASP.NET Core Blazor autonomicznej aplikacji webassembly z biblioteką uwierzytelniania
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: ea50d94835b044f9c3d6a0561868f081d32cb62a
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219011"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a><span data-ttu-id="107e3-102">Zabezpieczanie ASP.NET Core Blazor autonomicznej aplikacji webassembly z biblioteką uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="107e3-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library</span></span>

<span data-ttu-id="107e3-103">Autorzy [Javier Calvarro Nelson](https://github.com/javiercn) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="107e3-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="107e3-104">*W przypadku Azure Active Directory (AAD) i Azure Active Directory B2C (AAD B2C) nie postępuj zgodnie ze wskazówkami w tym temacie. Zobacz tematy dotyczące usługi AAD i AAD B2C w tym węźle spisu treści.*</span><span class="sxs-lookup"><span data-stu-id="107e3-104">*For Azure Active Directory (AAD) and Azure Active Directory B2C (AAD B2C), don't follow the guidance in this topic. See the AAD and AAD B2C topics in this table of contents node.*</span></span>

<span data-ttu-id="107e3-105">Aby utworzyć Blazor autonomiczną aplikację webassembly, która używa biblioteki `Microsoft.AspNetCore.Components.WebAssembly.Authentication`, wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="107e3-105">To create a Blazor WebAssembly standalone app that uses `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library, execute the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual
```

<span data-ttu-id="107e3-106">Aby określić lokalizację wyjściową, która tworzy folder projektu, jeśli nie istnieje, Uwzględnij opcję Output w poleceniu z ścieżką (na przykład `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="107e3-106">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="107e3-107">Nazwa folderu jest również częścią nazwy projektu.</span><span class="sxs-lookup"><span data-stu-id="107e3-107">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="107e3-108">W programie Visual Studio [Utwórz aplikację Blazor webassembly](xref:blazor/get-started).</span><span class="sxs-lookup"><span data-stu-id="107e3-108">In Visual Studio, [create a Blazor WebAssembly app](xref:blazor/get-started).</span></span> <span data-ttu-id="107e3-109">Skonfiguruj **uwierzytelnianie** na **poszczególnych kontach użytkowników** , korzystając z opcji **Zapisz konta użytkowników w aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="107e3-109">Set **Authentication** to **Individual User Accounts** with the **Store user accounts in-app** option.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="107e3-110">Pakiet uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="107e3-110">Authentication package</span></span>

<span data-ttu-id="107e3-111">Gdy aplikacja zostanie utworzona w celu używania poszczególnych kont użytkowników, aplikacja automatycznie otrzymuje odwołanie do pakietu dla pakietu `Microsoft.AspNetCore.Components.WebAssembly.Authentication` w pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="107e3-111">When an app is created to use Individual User Accounts, the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="107e3-112">Pakiet zawiera zestaw elementów podstawowych, które ułatwiają aplikacji uwierzytelnianie użytkowników i uzyskiwanie tokenów do wywoływania chronionych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="107e3-112">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="107e3-113">W przypadku dodawania uwierzytelniania do aplikacji ręcznie Dodaj pakiet do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="107e3-113">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="107e3-114">Zastąp `{VERSION}` w powyższym odwołaniu do pakietu w wersji pakietu `Microsoft.AspNetCore.Blazor.Templates` pokazanej w <xref:blazor/get-started> artykule.</span><span class="sxs-lookup"><span data-stu-id="107e3-114">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="107e3-115">Obsługa usługi uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="107e3-115">Authentication service support</span></span>

<span data-ttu-id="107e3-116">Obsługa uwierzytelniania użytkowników jest rejestrowana w kontenerze usługi przy użyciu metody rozszerzenia `AddOidcAuthentication` dostarczonej przez pakiet `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="107e3-116">Support for authenticating users is registered in the service container with the `AddOidcAuthentication` extension method provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="107e3-117">Ta metoda umożliwia skonfigurowanie wszystkich usług wymaganych przez aplikację do współpracy z dostawcą tożsamości (IP).</span><span class="sxs-lookup"><span data-stu-id="107e3-117">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="107e3-118">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="107e3-118">*Program.cs*:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="107e3-119">Obsługa uwierzytelniania dla aplikacji autonomicznych jest oferowana przy użyciu polecenia Open ID Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="107e3-119">Authentication support for standalone apps is offered using Open ID Connect (OIDC).</span></span> <span data-ttu-id="107e3-120">Metoda `AddOidcAuthentication` akceptuje wywołanie zwrotne w celu skonfigurowania parametrów wymaganych do uwierzytelniania aplikacji przy użyciu OIDC.</span><span class="sxs-lookup"><span data-stu-id="107e3-120">The `AddOidcAuthentication` method accepts a callback to configure the parameters required to authenticate an app using OIDC.</span></span> <span data-ttu-id="107e3-121">Wartości wymagane do skonfigurowania aplikacji można uzyskać z adresu IP zgodnego z OIDC.</span><span class="sxs-lookup"><span data-stu-id="107e3-121">The values required for configuring the app can be obtained from the OIDC-compliant IP.</span></span> <span data-ttu-id="107e3-122">Uzyskaj wartości podczas rejestrowania aplikacji, która zwykle odbywa się w portalu online.</span><span class="sxs-lookup"><span data-stu-id="107e3-122">Obtain the values when you register the app, which typically occurs in their online portal.</span></span>

## <a name="index-page"></a><span data-ttu-id="107e3-123">Strona indeksu</span><span class="sxs-lookup"><span data-stu-id="107e3-123">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

## <a name="app-component"></a><span data-ttu-id="107e3-124">Składnik aplikacji</span><span class="sxs-lookup"><span data-stu-id="107e3-124">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="107e3-125">Składnik RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="107e3-125">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="107e3-126">Składnik LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="107e3-126">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="107e3-127">Składnik uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="107e3-127">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
