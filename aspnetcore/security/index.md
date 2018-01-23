---
title: "Przegląd zabezpieczeń platformy ASP.NET Core | Dokumentacja firmy Microsoft"
author: rachelappel
description: "Poznaj podstawowe informacje dotyczące uwierzytelniania, autoryzacji i zabezpieczeń w ASP.NET Core"
ms.author: rachelap
manager: wpickett
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: 3f0f1c402aeac388c2fcabb509aa8aa3a46e95f5
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="aspnet-core-security-overview"></a>Przegląd zabezpieczeń platformy ASP.NET Core

Platformy ASP.NET Core umożliwia deweloperom łatwe konfigurowanie i zarządzanie zabezpieczeniami dla aplikacji. Platformy ASP.NET Core zawiera funkcje związane z zarządzaniem uwierzytelniania, autoryzacji, ochrony danych, Wymuszanie protokołu SSL, klucze tajne aplikacji, ochrony sfałszowaniem żądań oprogramowania i zarządzania CORS. Te funkcje zabezpieczeń umożliwiają tworzenie niezawodnych jeszcze secure aplikacji platformy ASP.NET Core. 

## <a name="aspnet-core-security-features"></a>Funkcje zabezpieczeń platformy ASP.NET Core

Platformy ASP.NET Core udostępnia wiele narzędzi i biblioteki w celu zabezpieczenia aplikacji w tym wbudowanych dostawców tożsamości, ale można użyć 3 usług tożsamości firm, takich jak Facebook, Twitter i LinkedIn. Platformy ASP.NET Core aby łatwo zarządzać klucze tajne aplikacji, które służą do przechowywania i używać informacji poufnych, bez konieczności je ujawnić w kodzie. 

## <a name="authentication-vs-authorization"></a>Uwierzytelnianie programu vs. Autoryzacja

Uwierzytelnianie to proces, w którym użytkownik udostępnia poświadczenia, które następnie są porównywane te są przechowywane w systemie operacyjnym, bazy danych, aplikacji lub zasobów. Jeśli są zgodne, użytkownicy pomyślnie uwierzytelnił i można następnie wykonać akcje, które użytkownik jest uprawniony, podczas procesu autoryzacji. Autoryzacja odwołuje się do proces, który określa, jakie użytkownik może wykonywać. 

Innym sposobem uwierzytelniania należy traktować jest należy wziąć pod uwagę sposób wprowadź spację, np. serwera, bazy danych, aplikacji lub zasobów, gdy autoryzacja jest akcje, które użytkownik może wykonywać na obiekty, które wewnątrz tego miejsca (serwer bazy danych i aplikacji).

## <a name="common-vulnerabilities-in-software"></a>Znanych luk w oprogramowaniu

Platformy ASP.NET Core i EF zawiera funkcje, które ułatwiają zabezpieczenia aplikacji i zapobiec naruszeń zabezpieczeń. Poniższa lista łączy przejście do dokumentacji zwierają technik w celu uniknięcia najczęściej luk w zabezpieczeniach w aplikacji sieci web:

* [Ataki skryptów krzyżowych](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [Ataki](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Fałszowanie żądań między witrynami (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [Otwórz przekierowania ataków](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

Istnieje więcej usterek, które należy zwrócić uwagę. Aby uzyskać więcej informacji, zobacz sekcję w tym dokumencie na *dokumentacji zabezpieczeń ASP.NET*. 

## <a name="aspnet-security-documentation"></a>Dokumentacja zabezpieczeń platformy ASP.NET

*   [Uwierzytelnianie](authentication/index.md)
    *   [Wprowadzenie do tożsamości](authentication/identity.md)
    *   [Włączanie uwierzytelniania za pomocą usługi Facebook, Google i innych dostawców zewnętrznych](authentication/social/index.md)
    * [Konfigurowanie uwierzytelniania systemu Windows](authentication/windowsauth.md)
    *   [Potwierdzenie konta i odzyskiwanie hasła](authentication/accconfirm.md)
    *   [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS](authentication/2fa.md) 
    *   [Użyj plików cookie uwierzytelniania bez tożsamości](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integrowanie usługi Azure AD do aplikacji sieci web platformy ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Wywołanie interfejsu API platformy ASP.NET Core sieci Web z aplikacji WPF przy użyciu usługi Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Wywoływanie interfejsu Web API z aplikacji internetowej platformy ASP.NET Core przy użyciu usługi Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Aplikacji sieci web platformy ASP.NET Core za pomocą usługi Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Zabezpieczanie aplikacji platformy ASP.NET Core za pomocą usługi IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autoryzacja](authorization/index.md)
    *   [Wprowadzenie](authorization/introduction.md)
    *   [Tworzenie aplikacji przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data)
    *   [Autoryzacja prosta](authorization/simple.md)
    *   [Autoryzacja oparta na rolach](authorization/roles.md)
    *   [Autoryzacja oparta na oświadczeniach](authorization/claims.md)
    *   [Autoryzacja oparta na zasadach](authorization/policies.md)
    *   [Wstrzykiwanie zależności w programach obsługi wymagań](authorization/dependencyinjection.md)
    *   [Autoryzacja na podstawie zasobów](authorization/resourcebased.md)
    *   [Autoryzacja na podstawie widoku](authorization/views.md)
    *   [Ograniczanie tożsamości według schematu](authorization/limitingidentitybyscheme.md)
*   [Ochrona danych](data-protection/index.md)
    *   [Wprowadzenie do ochrony danych](data-protection/introduction.md)
    *   [Wprowadzenie do interfejsów API ochrony danych](data-protection/using-data-protection.md)
    *   [Interfejsy API przeznaczone dla klientów](data-protection/consumer-apis/index.md)
        *   [Omówienie interfejsów API przeznaczonych dla klientów](data-protection/consumer-apis/overview.md)
        *   [Ciągi celów](data-protection/consumer-apis/purpose-strings.md)
        *   [Hierarchia celów i obsługa wielu dzierżawców](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Tworzenia skrótów haseł](data-protection/consumer-apis/password-hashing.md)
        *   [Ograniczanie okresu istnienia ładunków chronionych](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Wyłączanie ochrony ładunków, których klucze zostały odwołane](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Konfiguracja](data-protection/configuration/index.md)
        *   [Konfigurowanie ochrony danych](data-protection/configuration/overview.md)
        *   [Ustawienia domyślne](data-protection/configuration/default-settings.md)
        *   [Zasady dla komputera](data-protection/configuration/machine-wide-policy.md)
        *   [Scenariusze z systemem innym niż Podpisane aware](data-protection/configuration/non-di-scenarios.md)
    *   [Interfejsy API rozszerzalności](data-protection/extensibility/index.md)
        *   [Rozszerzalność kryptografii Core](data-protection/extensibility/core-crypto.md)
        *   [Rozszerzalność zarządzania kluczami](data-protection/extensibility/key-management.md)
        *   [Różne interfejsy API](data-protection/extensibility/misc-apis.md)
    *   [Implementacja](data-protection/implementation/index.md)
        *   [Szczegóły uwierzytelnionego szyfrowania](data-protection/implementation/authenticated-encryption-details.md)
        *   [Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie](data-protection/implementation/subkeyderivation.md)
        *   [Nagłówki kontekstu](data-protection/implementation/context-headers.md)
        *   [Zarządzanie kluczami](data-protection/implementation/key-management.md)
        *   [Dostawcy magazynu kluczy](data-protection/implementation/key-storage-providers.md)
        *   [Szyfrowanie kluczy podczas magazynowania](data-protection/implementation/key-encryption-at-rest.md)
        *   [Niezmienność kluczy i zmienianie ustawień](data-protection/implementation/key-immutability.md)
        *   [Format magazynu kluczy](data-protection/implementation/key-storage-format.md)
        *   [Dostawcy efemerycznej ochrony danych](data-protection/implementation/key-storage-ephemeral.md)
    *   [Zgodność](data-protection/compatibility/index.md)
        *   [Udostępnianie plików cookie między aplikacjami](data-protection/compatibility/cookie-sharing.md)
        *   [Zamienianie elementu <machineKey> na platformie ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Tworzenie aplikacji przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data)
*   [Bezpieczne przechowywanie kluczy tajnych aplikacji w czasie projektowania](app-secrets.md)
*   [Dostawca konfiguracji usługi Azure Key Vault](key-vault-configuration.md)
*   [Wymuszanie protokołu SSL](enforcing-ssl.md)
*   [Ochrona przed fałszerstwem żądań](anti-request-forgery.md)
*   [Zapobieganie atakom na otwarte przekierowywanie](preventing-open-redirects.md)
*   [Zapobieganie atakom z użyciem skryptów między witrynami](cross-site-scripting.md)
*   [Włączanie żądań Cross-Origin (CORS)](cors.md)
