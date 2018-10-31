---
title: Przegląd zabezpieczeń platformy ASP.NET Core
author: tdykstra
description: Poznaj podstawowe informacje dotyczące uwierzytelniania, autoryzacji i zabezpieczeń w programie ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: security/index
ms.openlocfilehash: 4277266e20ab1921a2ba24d4500358ba90330370
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50252948"
---
# <a name="overview-of-aspnet-core-security"></a>Przegląd zabezpieczeń platformy ASP.NET Core

Platforma ASP.NET Core umożliwia deweloperom łatwe konfigurowanie i zarządzanie zabezpieczeniami dla swoich aplikacji. Platforma ASP.NET Core zawiera funkcje zarządzania uwierzytelniania, autoryzacji, ochrony danych, Wymuszanie protokołu SSL, wpisy tajne aplikacji, ochrona przed fałszerstwem żądań ochrony i zarządzanie mechanizmu CORS. Te funkcje zabezpieczeń pozwalają na tworzenie niezawodnych, ale zabezpieczanie aplikacji platformy ASP.NET Core.

## <a name="aspnet-core-security-features"></a>Funkcje zabezpieczeń platformy ASP.NET Core

Platforma ASP.NET Core udostępnia wiele narzędzi i bibliotek, aby zabezpieczyć swoje aplikacje, w tym Wbudowani dostawcy tożsamości, ale można użyć 3rd usług tożsamości innych firm, takich jak Facebook, Twitter i LinkedIn. Platforma ASP.NET Core umożliwia łatwe zarządzanie wpisy tajne aplikacji, które służą do przechowywania i korzystania z poufnych informacji bez ujawniania ich w kodzie.

## <a name="authentication-vs-authorization"></a>Uwierzytelnianie programu vs. Autoryzacja

Uwierzytelnianie jest procesem, w którym użytkownik podaje poświadczenia, które następnie są porównywane te, przechowywane w systemie operacyjnym, bazy danych, aplikacji lub zasobów. Jeśli są zgodne, użytkownikom pomyślnie uwierzytelnione i można wykonać akcje, które otrzymali oni autoryzację pozwalającą, podczas procesu autoryzacji. Autoryzacja odnosi się do procesu, który określa, jakie użytkownik może wykonywać.

Innym sposobem, aby traktować uwierzytelniania jest należy wziąć pod uwagę jako sposób wprowadź spację, np. serwera, bazy danych, aplikacji lub zasobów, podczas gdy autoryzacja to akcje, które może wykonywać użytkownik, do których obiektów wewnątrz tego miejsca (serwer, baza danych lub aplikacji).

## <a name="common-vulnerabilities-in-software"></a>Typowe luk w zabezpieczeniach oprogramowania

Platforma ASP.NET Core i programem EF zawiera funkcje, które ułatwiają zabezpieczanie aplikacji i zapobieganie naruszeniom bezpieczeństwa. Poniższa lista łącza spowoduje przejście do dokumentacji techniki takimi szczegółami, jak uniknąć typowych luk w zabezpieczeniach w usłudze web apps:

* [Ataki z użyciem skryptów między witrynami](xref:security/cross-site-scripting)
* [Ataki polegające na iniekcji SQL](/ef/core/querying/raw-sql)
* [Cross-Site Request Forgery (CSRF)](xref:security/anti-request-forgery)
* [Atakom na otwarte przekierowywanie](xref:security/preventing-open-redirects)

Ma więcej luk w zabezpieczeniach, które należy wiedzieć. Aby uzyskać więcej informacji, zobacz sekcję w tym dokumencie na *dokumentację zabezpieczeń platformy ASP.NET Core*.

## <a name="aspnet-core-security-documentation"></a>Dokumentacja zabezpieczeń platformy ASP.NET Core

* Uwierzytelnianie
  * [Wprowadzenie do tożsamości](xref:security/authentication/identity)
  * [Włączanie uwierzytelniania za pomocą usługi Facebook, Google i innych dostawców zewnętrznych](xref:security/authentication/social/index)
  * [Włączanie uwierzytelniania za pomocą protokołu WS-Federation](xref:security/authentication/ws-federation)
  * [Konfigurowanie uwierzytelniania systemu Windows](xref:security/authentication/windowsauth)
  * [Potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm)
  * [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS](xref:security/authentication/2fa)
  * [Używania uwierzytelniania plików cookie bez użycia produktu Identity](xref:security/authentication/cookie)
  * [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
    * [Integrowanie usługi Azure AD w aplikacji sieci web platformy ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
    * [Wywoływanie interfejsu API sieci Web programu ASP.NET Core z aplikacji WPF przy użyciu usługi Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
    * [Wywoływanie interfejsu Web API z aplikacji internetowej platformy ASP.NET Core przy użyciu usługi Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
    * [Aplikację internetową platformy ASP.NET Core przy użyciu usługi Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
  * [Zabezpieczanie aplikacji platformy ASP.NET Core za pomocą usługi IdentityServer4](https://identityserver4.readthedocs.io)
* Autoryzacja
  * [Wprowadzenie](xref:security/authorization/introduction)
  * [Tworzenie aplikacji przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data)
  * [Autoryzacja prosta](xref:security/authorization/simple)
  * [Autoryzacja oparta na rolach](xref:security/authorization/roles)
  * [Autoryzacja oparta na oświadczeniach](xref:security/authorization/claims)
  * [Autoryzacja oparta na zasadach](xref:security/authorization/policies)
  * [Wstrzykiwanie zależności w programach obsługi wymagań](xref:security/authorization/dependencyinjection)
  * [Autoryzacja na podstawie zasobów](xref:security/authorization/resourcebased)
  * [Autoryzacja na podstawie widoku](xref:security/authorization/views)
  * [Ograniczanie tożsamości według schematu](xref:security/authorization/limitingidentitybyscheme)
* Ochrona danych
  * [Wprowadzenie do ochrony danych](xref:security/data-protection/introduction)
  * [Wprowadzenie do interfejsów API ochrony danych](xref:security/data-protection/using-data-protection)
  * Interfejsy API przeznaczone dla klientów
    * [Omówienie interfejsów API przeznaczonych dla klientów](xref:security/data-protection/consumer-apis/overview)
    * [Ciągi celów](xref:security/data-protection/consumer-apis/purpose-strings)
    * [Hierarchia celów i obsługa wielu dzierżawców](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
    * [Wyznaczanie skrótów haseł](xref:security/data-protection/consumer-apis/password-hashing)
    * [Ograniczanie okresu istnienia ładunków chronionych](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
    * [Wyłączanie ochrony ładunków, których klucze zostały odwołane](xref:security/data-protection/consumer-apis/dangerous-unprotect)
  * [Konfiguracja](xref:security/data-protection/configuration/index)
    * [Konfigurowanie ochrony danych](xref:security/data-protection/configuration/overview)
    * [Ustawienia domyślne](xref:security/data-protection/configuration/default-settings)
    * [Zasady dla komputera](xref:security/data-protection/configuration/machine-wide-policy)
    * [Scenariusze bez DI-aware](xref:security/data-protection/configuration/non-di-scenarios)
  * [Interfejsy API rozszerzalności](xref:security/data-protection/extensibility/index)
    * [Rozszerzalność kryptografii Core](xref:security/data-protection/extensibility/core-crypto)
    * [Rozszerzalność zarządzania kluczami](xref:security/data-protection/extensibility/key-management)
    * [Różne interfejsy API](xref:security/data-protection/extensibility/misc-apis)
  * [Implementacja](xref:security/data-protection/implementation/index)
    * [Szczegóły uwierzytelnionego szyfrowania](xref:security/data-protection/implementation/authenticated-encryption-details)
    * [Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie](xref:security/data-protection/implementation/subkeyderivation)
    * [Nagłówki kontekstu](xref:security/data-protection/implementation/context-headers)
    * [Zarządzanie kluczami](xref:security/data-protection/implementation/key-management)
    * [Dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers)
    * [Szyfrowanie kluczy podczas magazynowania](xref:security/data-protection/implementation/key-encryption-at-rest)
    * [Ustawienia i niezmienność klucza](xref:security/data-protection/implementation/key-immutability)
    * [Format magazynu kluczy](xref:security/data-protection/implementation/key-storage-format)
    * [Dostawcy efemerycznej ochrony danych](xref:security/data-protection/implementation/key-storage-ephemeral)
  * [Zgodność](xref:security/data-protection/compatibility/index)
    * [Zastąp \<machineKey > w programie ASP.NET:](xref:security/data-protection/compatibility/replacing-machinekey)
* [Tworzenie aplikacji przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data)
* [Bezpieczne przechowywanie wpisów tajnych aplikacji w czasie projektowania](xref:security/app-secrets)
* [Dostawca konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration)
* [Wymuszanie protokołu SSL](xref:security/enforcing-ssl)
* [Ochrona przed fałszerstwem żądań](xref:security/anti-request-forgery)
* [Zapobieganie atakom na otwarte przekierowywanie](xref:security/preventing-open-redirects)
* [Zapobieganie atakom z użyciem skryptów między witrynami](xref:security/cross-site-scripting)
* [Włączanie żądań Cross-Origin (CORS)](xref:security/cors)
* [Udostępnianie plików cookie między aplikacjami](xref:security/cookie-sharing)
* [Lista bezpiecznych adresów IP](xref:security/ip-safelist)
