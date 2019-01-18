---
ms.openlocfilehash: a7be92adffc06ac0f25b84ea15d1a8c4896f4f9d
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54397079"
---
<!--Don't update this for 2.2, use the 2.2 version --> Wywołanie [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity) umożliwia skonfigurowanie domyślnych ustawień systemu. [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) przeciążenia zestawy [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) właściwości. [AddAuthentication (Akcja&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) przeciążenie umożliwia konfigurowanie opcji uwierzytelniania, które mogą być używane do definiowania schematów uwierzytelniania domyślny dla różnych celów. Kolejne wywołania `AddAuthentication` zastąpienie wcześniej skonfigurowane [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) właściwości.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) metody rozszerzenia, które rejestrują program obsługi uwierzytelniania mogą lze volat pouze jednou dla schematu uwierzytelniania. Istnieją przeciążenia, które umożliwia konfigurowanie właściwości schematu, nazwa schematu i nazwę wyświetlaną.
