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
ms.openlocfilehash: 76bcac29d86a236e56c0eaea24a694c4845ecbcf
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083749"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a>Zabezpieczanie ASP.NET Core Blazor autonomicznej aplikacji webassembly przy użyciu Azure Active Directory

Autorzy [Javier Calvarro Nelson](https://github.com/javiercn) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Aby utworzyć Blazor autonomiczną aplikację webassembly, która używa [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) do uwierzytelniania:

[Utwórz dzierżawę usługi AAD i aplikację sieci Web](/azure/active-directory/develop/v2-overview):

Zarejestruj aplikację usługi AAD w **Azure Active Directory** > **rejestracje aplikacji** obszarze Azure Portal:

1. Podaj **nazwę** aplikacji (na przykład **Blazor klienta AAD**).
1. Wybierz **obsługiwane typy kont**. **W tym katalogu organizacji można wybrać konta tylko** dla tego środowiska.
1. Pozostaw pole listy rozwijanej **Identyfikator URI przekierowania** na wartość **Web**i podaj identyfikator URI przekierowania `https://localhost:5001/authentication/login-callback`.
1. Wyłącz **uprawnienia** > **Przyznaj administratorowi OpenID Connect i offline_access** pole wyboru.
1. Wybierz pozycję **Zarejestruj**.

W obszarze **uwierzytelnianie** > **konfiguracjami platformy** > **sieci Web**:

1. Upewnij się, że **Identyfikator URI przekierowania** `https://localhost:5001/authentication/login-callback` jest obecny.
1. W przypadku **niejawnego przydzielenia**zaznacz pola wyboru dla **tokenów dostępu** i **tokenów identyfikatorów**.
1. Pozostałe wartości domyślne dla aplikacji są dopuszczalne dla tego środowiska.
1. Wybierz ikonę **Zapisz**.

Zapisz następujące informacje:

* Identyfikator aplikacji (identyfikator klienta) (na przykład `11111111-1111-1111-1111-111111111111`)
* Identyfikator katalogu (identyfikator dzierżawy) (na przykład `22222222-2222-2222-2222-222222222222`)

Zastąp symbole zastępcze w poniższym poleceniu zapisanymi wcześniej informacjami i wykonaj polecenie w powłoce poleceń:

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

Aby określić lokalizację wyjściową, która tworzy folder projektu, jeśli nie istnieje, Uwzględnij opcję Output w poleceniu z ścieżką (na przykład `-o BlazorSample`). Nazwa folderu jest również częścią nazwy projektu.

## <a name="authentication-package"></a>Pakiet uwierzytelniania

Gdy aplikacja zostanie utworzona w celu korzystania z kont służbowych (`SingleOrg`), aplikacja automatycznie otrzymuje odwołanie do pakietu dla [biblioteki uwierzytelniania firmy Microsoft](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). Pakiet zawiera zestaw elementów podstawowych, które ułatwiają aplikacji uwierzytelnianie użytkowników i uzyskiwanie tokenów do wywoływania chronionych interfejsów API.

W przypadku dodawania uwierzytelniania do aplikacji ręcznie Dodaj pakiet do pliku projektu aplikacji:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Zastąp `{VERSION}` w powyższym odwołaniu do pakietu w wersji pakietu `Microsoft.AspNetCore.Blazor.Templates` pokazanej w <xref:blazor/get-started> artykule.

Pakiet `Microsoft.Authentication.WebAssembly.Msal` w sposób przechodni dodaje pakiet `Microsoft.AspNetCore.Components.WebAssembly.Authentication` do aplikacji.

## <a name="authentication-service-support"></a>Obsługa usługi uwierzytelniania

Obsługa uwierzytelniania użytkowników jest rejestrowana w kontenerze usługi przy użyciu metody rozszerzenia `AddMsalAuthentication` dostarczonej przez pakiet `Microsoft.Authentication.WebAssembly.Msal`. Ta metoda umożliwia skonfigurowanie wszystkich usług wymaganych przez aplikację do współpracy z dostawcą tożsamości (IP).

*Program.cs*:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

Metoda `AddMsalAuthentication` akceptuje wywołanie zwrotne w celu skonfigurowania parametrów wymaganych do uwierzytelnienia aplikacji. Wartości wymagane do skonfigurowania aplikacji można uzyskać z konfiguracji usługi AAD w witrynie Azure Portal podczas rejestrowania aplikacji.

Szablon Blazor webassembly nie konfiguruje automatycznie aplikacji do żądania tokenu dostępu dla bezpiecznego interfejsu API. Aby zainicjować obsługę administracyjną tokenu w ramach przepływu logowania, Dodaj zakres do domyślnych zakresów tokenów dostępu `MsalProviderOptions`:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> Domyślny zakres tokenu dostępu musi mieć format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (na przykład `11111111-1111-1111-1111-111111111111/API.Access`). Jeśli schemat lub schemat i Host są dostarczane do ustawienia zakresu (jak pokazano w witrynie Azure Portal), *aplikacja kliencka* zgłasza nieobsłużony wyjątek, gdy odbierze *401 nieautoryzowaną* odpowiedź z *aplikacji interfejsu API serwera*.

## <a name="index-page"></a>Strona indeksu

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a>Składnik aplikacji

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>Składnik RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>Składnik LoginDisplay

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>Składnik uwierzytelniania

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:security/authentication/azure-active-directory/index>
