---
title: Zabezpieczanie ASP.NET Core Blazor autonomicznej aplikacji webassembly z biblioteką uwierzytelniania
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: f9cc2884dcd94c729c45a056ae4327a2c75d34be
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083756"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a><span data-ttu-id="0a85d-102">Zabezpieczanie ASP.NET Core Blazor autonomicznej aplikacji webassembly z biblioteką uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="0a85d-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library</span></span>

<span data-ttu-id="0a85d-103">Autorzy [Javier Calvarro Nelson](https://github.com/javiercn) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0a85d-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="0a85d-104">Aby utworzyć Blazor autonomiczną aplikację webassembly, która używa biblioteki `Microsoft.AspNetCore.Components.WebAssembly.Authentication`, wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="0a85d-104">To create a Blazor WebAssembly standalone app that uses `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library, execute the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual
```

<span data-ttu-id="0a85d-105">Aby określić lokalizację wyjściową, która tworzy folder projektu, jeśli nie istnieje, Uwzględnij opcję Output w poleceniu z ścieżką (na przykład `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="0a85d-105">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="0a85d-106">Nazwa folderu jest również częścią nazwy projektu.</span><span class="sxs-lookup"><span data-stu-id="0a85d-106">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="0a85d-107">W programie Visual Studio [Utwórz aplikację Blazor webassembly](xref:blazor/get-started).</span><span class="sxs-lookup"><span data-stu-id="0a85d-107">In Visual Studio, [create a Blazor WebAssembly app](xref:blazor/get-started).</span></span> <span data-ttu-id="0a85d-108">Skonfiguruj **uwierzytelnianie** na **poszczególnych kontach użytkowników** , korzystając z opcji **Zapisz konta użytkowników w aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="0a85d-108">Set **Authentication** to **Individual User Accounts** with the **Store user accounts in-app** option.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="0a85d-109">Pakiet uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="0a85d-109">Authentication package</span></span>

<span data-ttu-id="0a85d-110">Gdy aplikacja zostanie utworzona w celu używania poszczególnych kont użytkowników, aplikacja automatycznie otrzymuje odwołanie do pakietu dla pakietu `Microsoft.AspNetCore.Components.WebAssembly.Authentication` w pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0a85d-110">When an app is created to use Individual User Accounts, the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="0a85d-111">Pakiet zawiera zestaw elementów podstawowych, które ułatwiają aplikacji uwierzytelnianie użytkowników i uzyskiwanie tokenów do wywoływania chronionych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="0a85d-111">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="0a85d-112">W przypadku dodawania uwierzytelniania do aplikacji ręcznie Dodaj pakiet do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="0a85d-112">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="0a85d-113">Zastąp `{VERSION}` w powyższym odwołaniu do pakietu w wersji pakietu `Microsoft.AspNetCore.Blazor.Templates` pokazanej w <xref:blazor/get-started> artykule.</span><span class="sxs-lookup"><span data-stu-id="0a85d-113">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="0a85d-114">Obsługa usługi uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="0a85d-114">Authentication service support</span></span>

<span data-ttu-id="0a85d-115">Obsługa uwierzytelniania użytkowników jest rejestrowana w kontenerze usługi przy użyciu metody rozszerzenia `AddOidcAuthentication` dostarczonej przez pakiet `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="0a85d-115">Support for authenticating users is registered in the service container with the `AddOidcAuthentication` extension method provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="0a85d-116">Ta metoda umożliwia skonfigurowanie wszystkich usług wymaganych przez aplikację do współpracy z dostawcą tożsamości (IP).</span><span class="sxs-lookup"><span data-stu-id="0a85d-116">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="0a85d-117">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="0a85d-117">*Program.cs*:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

<span data-ttu-id="0a85d-118">Obsługa uwierzytelniania dla aplikacji autonomicznych jest oferowana przy użyciu polecenia Open ID Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="0a85d-118">Authentication support for standalone apps is offered using Open ID Connect (OIDC).</span></span> <span data-ttu-id="0a85d-119">Metoda `AddOidcAuthentication` akceptuje wywołanie zwrotne w celu skonfigurowania parametrów wymaganych do uwierzytelniania aplikacji przy użyciu OIDC.</span><span class="sxs-lookup"><span data-stu-id="0a85d-119">The `AddOidcAuthentication` method accepts a callback to configure the parameters required to authenticate an app using OIDC.</span></span> <span data-ttu-id="0a85d-120">Wartości wymagane do skonfigurowania aplikacji można uzyskać z adresu IP, takiego jak Google, Microsoft lub innego dostawcy zgodnego z OIDC.</span><span class="sxs-lookup"><span data-stu-id="0a85d-120">The values required for configuring the app can be obtained from the IP, such as Google, Microsoft, or other OIDC-compliant provider.</span></span> <span data-ttu-id="0a85d-121">Uzyskaj wartości podczas rejestrowania aplikacji, która zwykle odbywa się w portalu online.</span><span class="sxs-lookup"><span data-stu-id="0a85d-121">Obtain the values when you register the app, which typically occurs in their online portal.</span></span>

## <a name="index-page"></a><span data-ttu-id="0a85d-122">Strona indeksu</span><span class="sxs-lookup"><span data-stu-id="0a85d-122">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a><span data-ttu-id="0a85d-123">Składnik aplikacji</span><span class="sxs-lookup"><span data-stu-id="0a85d-123">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="0a85d-124">Składnik RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="0a85d-124">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="0a85d-125">Składnik LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="0a85d-125">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="0a85d-126">Składnik uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="0a85d-126">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
