---
title: Moduły usług IIS z platformy ASP.NET Core
author: guardrex
description: Wykryj aktywną i nieaktywną modułów usług IIS dla aplikacji platformy ASP.NET Core i zarządzanie modułów usług IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: e88526d997618658f58488adb37ae1e519ea3f59
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="iis-modules-with-aspnet-core"></a>Moduły usług IIS z platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Aplikacje platformy ASP.NET Core są obsługiwane przez usługi IIS w konfiguracji zwrotnego serwera proxy. Niektóre moduły macierzyste usług IIS i wszystkie moduły usług IIS zarządzane nie są dostępne do przetwarzania żądań dla aplikacji platformy ASP.NET Core. W wielu przypadkach platformy ASP.NET Core oferuje alternatywę do funkcji natywnych i zarządzanych modułów usług IIS.

## <a name="native-modules"></a>Moduły macierzyste

Tabela wskazuje modułów macierzystych usług IIS, które działają na zwrotny serwer proxy żądań do aplikacji platformy ASP.NET Core.

| Moduł | Funkcjonalności przy użyciu aplikacji platformy ASP.NET Core | Opcja platformy ASP.NET Core |
| ------ | :-------------------------------: | ------------------- |
| **Uwierzytelnianie anonimowe**<br>`AnonymousAuthenticationModule` | Tak | |
| **Uwierzytelnianie podstawowe**<br>`BasicAuthenticationModule` | Tak | |
| **Uwierzytelnianie mapowań certyfikacji klientów**<br>`CertificateMappingAuthenticationModule` | Tak | |
| **CGI**<br>`CgiModule` | Nie | |
| **Sprawdzanie poprawności konfiguracji**<br>`ConfigurationValidationModule` | Tak | |
| **Błędy HTTP**<br>`CustomErrorModule` | Nie | [Oprogramowanie pośredniczące strony kod stanu](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **Rejestrowanie niestandardowe**<br>`CustomLoggingModule` | Tak | |
| **Dokument domyślny**<br>`DefaultDocumentModule` | Nie | [Oprogramowanie pośredniczące plików domyślne](xref:fundamentals/static-files#serve-a-default-document) |
| **Uwierzytelnianie szyfrowane**<br>`DigestAuthenticationModule` | Tak | |
| **Przeglądanie katalogów**<br>`DirectoryListingModule` | Nie | [Oprogramowanie pośredniczące przeglądania w katalogu](xref:fundamentals/static-files#enable-directory-browsing) |
| **Kompresja dynamiczna**<br>`DynamicCompressionModule` | Tak | [Oprogramowanie pośredniczące kompresji odpowiedzi](xref:performance/response-compression) |
| **Śledzenie**<br>`FailedRequestsTracingModule` | Tak | [Rejestrowanie platformy ASP.NET Core](xref:fundamentals/logging/index#the-tracesource-provider) |
| **Buforowanie plików**<br>`FileCacheModule` | Nie | [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) |
| **Buforowanie HTTP**<br>`HttpCacheModule` | Nie | [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) |
| **Rejestrowanie HTTP**<br>`HttpLoggingModule` | Tak | [Rejestrowanie platformy ASP.NET Core](xref:fundamentals/logging/index)<br>Implementacje: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **Przekierowywanie HTTP**<br>`HttpRedirectionModule` | Tak | [Oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting) |
| **Uwierzytelnianie mapowań certyfikatów klientów usług IIS**<br>`IISCertificateMappingAuthenticationModule` | Tak | |
| **Ograniczenia adresów IP i domen**<br>`IpRestrictionModule` | Tak | |
| **Filtry ISAPI**<br>`IsapiFilterModule` | Tak | [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | Tak | [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index) |
| **Obsługa protokołu**<br>`ProtocolSupportModule` | Tak | |
| **Filtrowanie żądań**<br>`RequestFilteringModule` | Tak | [Ponowne zapisywanie adresów URL w oprogramowania pośredniczącego `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Monitor żądań**<br>`RequestMonitorModule` | Tak | |
| **Ponowne zapisywanie adresów URL**<br>`RewriteModule` | Yes&#8224; | [Oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting) |
| **Server-Side Includes**<br>`ServerSideIncludeModule` | Nie | |
| **Kompresja statyczna**<br>`StaticCompressionModule` | Nie | [Oprogramowanie pośredniczące kompresji odpowiedzi](xref:performance/response-compression) |
| **Zawartość statyczna**<br>`StaticFileModule` | Nie | [Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) |
| **Buforowanie tokenu**<br>`TokenCacheModule` | Tak | |
| **Identyfikator URI w pamięci podręcznej**<br>`UriCacheModule` | Tak | |
| **Autoryzacja adresów URL**<br>`UrlAuthorizationModule` | Tak | [ASP.NET Core Identity](xref:security/authentication/identity) |
| **Uwierzytelnianie systemu Windows**<br>`WindowsAuthenticationModule` | Tak | |

&#8224;Adres URL edycji modułu `isFile` i `isDirectory` zgodny z typami nie działają w aplikacjach ASP.NET Core z powodu zmian w [struktury katalogów](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Modułów zarządzanych

Moduły zarządzane są *nie* funkcjonalności z hostowanej aplikacji platformy ASP.NET Core, gdy wersja środowiska .NET CLR puli aplikacji ustawiono **bez kodu zarządzanego**. Platformy ASP.NET Core oferuje alternatywne oprogramowanie pośredniczące w kilku przypadkach.

| Moduł                  | Opcja platformy ASP.NET Core |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| Uwierzytelniania formularzy     | [Oprogramowanie pośredniczące uwierzytelniania plików cookie](xref:security/authentication/cookie) |
| OutputCache             | [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) |
| Profil                 | |
| RoleManager             | |
| ScriptModule 4.0        | |
| Sesja                 | [Oprogramowanie pośredniczące sesji](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [Oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | [ASP.NET Core Identity](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>Zmiany aplikacji Menedżera usług IIS

Jeśli za pomocą Menedżera usług IIS, aby skonfigurować ustawienia, *web.config* pliku aplikacji zostanie zmieniona. Wdrażania aplikacji i tym *web.config*, wszystkie zmiany wprowadzone z Menedżera usług IIS są zastępowane przez wdrożone *web.config* pliku. Jeśli zmiany zostały wprowadzone do serwera *web.config* plików, skopiować zaktualizowane *web.config* pliku na serwerze lokalnym projektu bezpośrednio.

## <a name="disabling-iis-modules"></a>Wyłączanie modułów usług IIS

Jeśli moduł usług IIS jest skonfigurowany na poziomie serwera, który musi zostać wyłączone dla aplikacji, dodanie do wybranej aplikacji *web.config* pliku można wyłączyć moduł. Pozostaw modułu w miejscu i zdezaktywować przy użyciu ustawienia konfiguracji (jeśli jest dostępny) lub Usuń moduł z aplikacji.

### <a name="module-deactivation"></a>Dezaktywacja modułu

Wiele modułów zapewniają ustawienia konfiguracji, która pozwala na wyłączony bez usuwania modułu z poziomu aplikacji. Jest to najprostsza i najszybszym sposobem dezaktywować modułu. Na przykład można wyłączyć moduł przekierowania HTTP z  **\<httpRedirect >** element *web.config*:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Aby uzyskać więcej informacji o wyłączaniu modułów przy użyciu ustawień konfiguracyjnych, skorzystaj z łączy w *elementy podrzędne* sekcji [IIS \<system.webServer >](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Usunięcie modułu

Jeśli Aby usunąć moduł z ustawieniem w *web.config*, odblokowanie modułem i odblokować  **\<modułów >** sekcji *web.config* pierwszy:

1. Odblokowanie modułem na poziomie serwera. Wybierz serwer usług IIS w Menedżerze usług IIS **połączeń** paska bocznego. Otwórz **modułów** w **IIS** obszaru. Wybierz moduł z listy. W **akcje** po prawej, wybierz **Unlock**. Odblokowanie jak wiele modułów podczas planowania do usunięcia z *web.config* później.

2. Wdrażanie aplikacji bez  **\<modułów >** sekcji *web.config*. Jeśli aplikacja została wdrożona z *web.config* zawierający  **\<modułów >** sekcji bez konieczności odblokowane sekcji najpierw w Menedżerze usług IIS, programu Configuration Manager zgłasza wyjątek Podczas próby odblokowania sekcji. W związku z tym wdrażania aplikacji bez  **\<modułów >** sekcji.

3. Odblokuj  **\<modułów >** sekcji *web.config*. W **połączeń** paska bocznego, wybierz witrynę sieci Web w **witryny**. W **zarządzania** obszaru, otwórz **edytora konfiguracji**. Użyj formantów nawigacji, aby wybrać `system.webServer/modules` sekcji. W **akcje** po prawej, wybierz, aby **Unlock** sekcji.

4. W tym momencie  **\<modułów >** sekcji można dodać do *web.config* pliku z  **\<Usuń >** elementu do usunięcia z modułu aplikacja. Wiele  **\<Usuń >** elementy mogą zostać dodane, aby usunąć wiele modułów. Jeśli *web.config* zmian na serwerze, natychmiast wprowadzenie identycznych zmian w projekcie *web.config* plików lokalnie. Usunięcie modułu w ten sposób nie mają wpływu na modułu z innych aplikacji na serwerze.

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

Moduł usług IIS może zostać także usunięty z *Appcmd.exe*. Podaj `MODULE_NAME` i `APPLICATION_NAME` w poleceniu:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Na przykład usunąć `DynamicCompressionModule` z domyślnej witryny sieci Web:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Minimalną konfiguracją modułów

Tylko modułami wymaganymi do uruchomienia aplikacji platformy ASP.NET Core to modułu uwierzytelniania anonimowego i moduł platformy ASP.NET Core.

![Otwórz Menedżera usług IIS, aby moduły z minimalną konfiguracją modułów pokazano](modules/_static/modules.png)

Moduł pamięci podręcznej URI (`UriCacheModule`) zezwala usługom IIS do pamięci podręcznej konfiguracji witryny sieci Web na poziomie adresu URL. Bez tego modułu IIS należy odczytać i przeanalizować konfiguracji na każde żądanie, nawet wtedy, gdy wymagany jest wielokrotnie ten sam adres URL. Każde żądanie podczas analizowania konfiguracji powoduje zmniejszenie wydajności znaczące. *Mimo że moduł buforowania identyfikator URI nie jest ściśle wymagane do uruchamiania hostowanej aplikacji platformy ASP.NET Core, zaleca się włączenia modułu pamięci podręcznej URI dla wszystkich wdrożeń platformy ASP.NET Core.*

Moduł buforowania HTTP (`HttpCacheModule`) implementuje wyjściową pamięć podręczną programu IIS, a także logikę buforowanie elementów w pamięci podręcznej z pliku HTTP.sys. Bez tego modułu zawartości jest już w pamięci podręcznej w trybie jądra, a pamięć podręczna profile są ignorowane. Usunięcie modułu buforowanie HTTP zazwyczaj ma negatywny wpływ na wydajność i zużycie zasobów. *Mimo że moduł buforowania HTTP nie jest ściśle wymagane do uruchamiania hostowanej aplikacji platformy ASP.NET Core, zalecamy włączenia moduł buforowania HTTP dla wszystkich wdrożeń platformy ASP.NET Core.*

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Hosting w systemie Windows z usługami IIS](xref:host-and-deploy/iis/index)
* [Wprowadzenie do architektury usług IIS: modułów w usługach IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Omówienie moduły IIS](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Dostosowywanie usług IIS 7.0 ról i modułów](https://technet.microsoft.com/library/cc627313.aspx)
* [USŁUGI IIS `<system.webServer>`](/iis/configuration/system.webServer/)
