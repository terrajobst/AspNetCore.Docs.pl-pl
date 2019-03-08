---
ms.openlocfilehash: 2fe12027e7a5233cf01e6c412f7ee536d479facd
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665786"
---
<!--Don't update this for 2.2, use the 2.2 version --> Wywołanie [AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity) umożliwia skonfigurowanie domyślnych ustawień systemu. [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) przeciążenia zestawy [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) właściwości. [AddAuthentication (Akcja&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) przeciążenie umożliwia konfigurowanie opcji uwierzytelniania, które mogą być używane do definiowania schematów uwierzytelniania domyślny dla różnych celów. Kolejne wywołania `AddAuthentication` zastąpienie wcześniej skonfigurowane [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) właściwości.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) metody rozszerzenia, które rejestrują program obsługi uwierzytelniania mogą lze volat pouze jednou dla schematu uwierzytelniania. Istnieją przeciążenia, które umożliwia konfigurowanie właściwości schematu, nazwa schematu i nazwę wyświetlaną.
