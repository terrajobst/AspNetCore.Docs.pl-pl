---
title: Przegląd zabezpieczeń platformy ASP.NET Core
author: rachelappel
description: Poznaj podstawowe informacje dotyczące uwierzytelniania, autoryzacji i zabezpieczeń w ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.date: 11/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/index
ms.openlocfilehash: da3829b2d5ae5db1861c7423da5afc7acbee6697
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="overview-of-aspnet-core-security"></a>Przegląd zabezpieczeń platformy ASP.NET Core

Platformy ASP.NET Core umożliwia deweloperom łatwe konfigurowanie i zarządzanie zabezpieczeniami dla aplikacji. Platformy ASP.NET Core zawiera funkcje związane z zarządzaniem uwierzytelniania, autoryzacji, ochrony danych, Wymuszanie protokołu SSL, klucze tajne aplikacji, ochrony sfałszowaniem żądań oprogramowania i zarządzania CORS. Te funkcje zabezpieczeń umożliwiają tworzenie niezawodnych jeszcze secure aplikacji platformy ASP.NET Core.

## <a name="aspnet-core-security-features"></a>Funkcje zabezpieczeń platformy ASP.NET Core

Platformy ASP.NET Core udostępnia wiele narzędzi i biblioteki w celu zabezpieczenia aplikacji w tym wbudowanych dostawców tożsamości, ale można użyć 3 usług tożsamości firm, takich jak Facebook, Twitter i LinkedIn. Platformy ASP.NET Core aby łatwo zarządzać klucze tajne aplikacji, które służą do przechowywania i używać informacji poufnych, bez konieczności je ujawnić w kodzie.

## <a name="authentication-vs-authorization"></a>Uwierzytelnianie programu vs. Autoryzacja

Uwierzytelnianie to proces, w którym użytkownik udostępnia poświadczenia, które następnie są porównywane te są przechowywane w systemie operacyjnym, bazy danych, aplikacji lub zasobów. Jeśli są zgodne, użytkownicy pomyślnie uwierzytelnił i można następnie wykonać akcje, które są uprawnieni, podczas procesu autoryzacji. Autoryzacja odwołuje się do proces, który określa, jakie użytkownik może wykonywać.

Innym sposobem uwierzytelniania należy traktować jest należy wziąć pod uwagę sposób wprowadź spację, np. serwera, bazy danych, aplikacji lub zasobów, gdy autoryzacja jest akcje, które użytkownik może wykonywać na obiekty, które wewnątrz tego miejsca (serwer bazy danych i aplikacji).

## <a name="common-vulnerabilities-in-software"></a>Znanych luk w oprogramowaniu

Platformy ASP.NET Core i EF zawiera funkcje, które ułatwiają zabezpieczenia aplikacji i zapobiec naruszeń zabezpieczeń. Poniższa lista łączy przejście do dokumentacji zwierają technik w celu uniknięcia najczęściej luk w zabezpieczeniach w aplikacji sieci web:

* [Ataki skryptów krzyżowych](xref:security/cross-site-scripting)
* [Ataki](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Fałszowanie żądań między witrynami (CSRF)](xref:security/anti-request-forgery)
* [Otwórz przekierowania ataków](xref:security/preventing-open-redirects)

Istnieje więcej usterek, które należy zwrócić uwagę. Aby uzyskać więcej informacji, zobacz sekcję w tym dokumencie na *dokumentacji zabezpieczeń ASP.NET*.

## <a name="aspnet-security-documentation"></a>Dokumentacja zabezpieczeń platformy ASP.NET

*   [Uwierzytelnianie](xref:security/authentication/index)
    *   [Wprowadzenie do tożsamości](xref:security/authentication/identity)
    *   [Włączanie uwierzytelniania za pomocą usługi Facebook, Google i innych dostawców zewnętrznych](xref:security/authentication/social/index)
    *   [Włącz uwierzytelnianie za pomocą protokołu WS-Federation](xref:security/authentication/ws-federation)
    * [Konfigurowanie uwierzytelniania systemu Windows](xref:security/authentication/windowsauth)
    *   [Potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm)
    *   [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS](xref:security/authentication/2fa)
    *   [Użyj plików cookie uwierzytelniania bez tożsamości](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Integrowanie usługi Azure AD do aplikacji sieci web platformy ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Wywołanie interfejsu API platformy ASP.NET Core sieci Web z aplikacji WPF przy użyciu usługi Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Wywoływanie interfejsu Web API z aplikacji internetowej platformy ASP.NET Core przy użyciu usługi Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Aplikacji sieci web platformy ASP.NET Core za pomocą usługi Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Zabezpieczanie aplikacji platformy ASP.NET Core za pomocą usługi IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autoryzacja](xref:security/authorization/index)
    *   [Wprowadzenie](xref:security/authorization/introduction)
    *   [Tworzenie aplikacji przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data)
    *   [Autoryzacja prosta](xref:security/authorization/simple)
    *   [Autoryzacja oparta na rolach](xref:security/authorization/roles)
    *   [Autoryzacja oparta na oświadczeniach](xref:security/authorization/claims)
    *   [Autoryzacja oparta na zasadach](xref:security/authorization/policies)
    *   [Wstrzykiwanie zależności w programach obsługi wymagań](xref:security/authorization/dependencyinjection)
    *   [Autoryzacja na podstawie zasobów](xref:security/authorization/resourcebased)
    *   [Autoryzacja na podstawie widoku](xref:security/authorization/views)
    *   [Ograniczanie tożsamości według schematu](xref:security/authorization/limitingidentitybyscheme)
*   [Ochrona danych](xref:security/data-protection/index)
    *   [Wprowadzenie do ochrony danych](xref:security/data-protection/introduction)
    *   [Wprowadzenie do interfejsów API ochrony danych](xref:security/data-protection/using-data-protection)
    *   [Interfejsy API przeznaczone dla klientów](xref:security/data-protection/consumer-apis/index)
        *   [Omówienie interfejsów API przeznaczonych dla klientów](xref:security/data-protection/consumer-apis/overview)
        *   [Ciągi celów](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [Hierarchia celów i obsługa wielu dzierżawców](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [Skrót hasła](xref:security/data-protection/consumer-apis/password-hashing)
        *   [Ograniczanie okresu istnienia ładunków chronionych](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [Wyłączanie ochrony ładunków, których klucze zostały odwołane](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [Konfiguracja](xref:security/data-protection/configuration/index)
        *   [Konfigurowanie ochrony danych](xref:security/data-protection/configuration/overview)
        *   [Ustawienia domyślne](xref:security/data-protection/configuration/default-settings)
        *   [Zasady dla komputera](xref:security/data-protection/configuration/machine-wide-policy)
        *   [Scenariusze z systemem innym niż Podpisane aware](xref:security/data-protection/configuration/non-di-scenarios)
    *   [Interfejsy API rozszerzalności](xref:security/data-protection/extensibility/index)
        *   [Rozszerzalność kryptografii Core](xref:security/data-protection/extensibility/core-crypto)
        *   [Rozszerzalność zarządzania kluczami](xref:security/data-protection/extensibility/key-management)
        *   [Różne interfejsy API](xref:security/data-protection/extensibility/misc-apis)
    *   [Implementacja](xref:security/data-protection/implementation/index)
        *   [Szczegóły uwierzytelnionego szyfrowania](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie](xref:security/data-protection/implementation/subkeyderivation)
        *   [Nagłówki kontekstu](xref:security/data-protection/implementation/context-headers)
        *   [Zarządzanie kluczami](xref:security/data-protection/implementation/key-management)
        *   [Dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers)
        *   [Szyfrowanie kluczy podczas magazynowania](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [Immutability klucza i ustawienia](xref:security/data-protection/implementation/key-immutability)
        *   [Format magazynu kluczy](xref:security/data-protection/implementation/key-storage-format)
        *   [Dostawcy efemerycznej ochrony danych](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [Zgodność](xref:security/data-protection/compatibility/index)
        *   [Zamienianie elementu <machineKey> na platformie ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
*   [Tworzenie aplikacji przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data)
*   [Bezpieczne przechowywanie klucze tajne aplikacji do rozwoju](xref:security/app-secrets)
*   [Dostawca konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration)
*   [Wymuszanie protokołu SSL](xref:security/enforcing-ssl)
*   [Ochrona przed fałszerstwem żądań](xref:security/anti-request-forgery)
*   [Zapobieganie atakom na otwarte przekierowywanie](xref:security/preventing-open-redirects)
*   [Zapobieganie atakom z użyciem skryptów między witrynami](xref:security/cross-site-scripting)
*   [Włączanie żądań Cross-Origin (CORS)](xref:security/cors)
*   [Udostępnianie plików cookie między aplikacjami](xref:security/cookie-sharing)
