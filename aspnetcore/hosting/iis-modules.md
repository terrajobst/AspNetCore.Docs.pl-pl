---
title: "Używanie modułów usług IIS z platformy ASP.NET Core"
author: guardrex
description: "Odwołanie opisujący aktywną i nieaktywną modułów usług IIS dla aplikacji platformy ASP.NET Core."
keywords: "Platformy ASP.NET Core, usługi iis, moduł, wstecznego serwera proxy"
ms.author: riande
manager: wpickett
ms.date: 03/08/2017
ms.topic: article
ms.assetid: 492b3a7e-04c5-461b-b96a-38ecee5c64bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/iis-modules
ms.openlocfilehash: fee8e830ab43f731de9c90fad06b577662760f87
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="using-iis-modules-with-aspnet-core"></a>Używanie modułów usług IIS z platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Aplikacje platformy ASP.NET Core są obsługiwane przez usługi IIS w konfiguracji serwera proxy odwrotnie. Niektóre moduły macierzyste usług IIS i wszystkie moduły usług IIS zarządzane nie są dostępne do przetwarzania żądań dla aplikacji platformy ASP.NET Core. W wielu przypadkach platformy ASP.NET Core oferuje alternatywę do funkcji natywnych i zarządzanych modułów usług IIS.

## <a name="native-modules"></a>Moduły macierzyste
Moduł | .NET core aktywny | Opcja platformy ASP.NET Core
--- | :---: | ---
**Uwierzytelnianie anonimowe**<br>`AnonymousAuthenticationModule` | Tak | 
**Uwierzytelnianie podstawowe**<br>`BasicAuthenticationModule` | Tak | 
**Uwierzytelnianie mapowań certyfikacji klientów**<br>`CertificateMappingAuthenticationModule` | Tak | 
**CGI**<br>`CgiModule` | Nie | 
**Sprawdzanie poprawności konfiguracji**<br>`ConfigurationValidationModule` | Tak | 
**Błędy HTTP**<br>`CustomErrorModule` | Nie | [Oprogramowanie pośredniczące strony kod stanu](xref:fundamentals/error-handling#configuring-status-code-pages)
**Rejestrowanie niestandardowe**<br>`CustomLoggingModule` | Tak | 
**Dokument domyślny**<br>`DefaultDocumentModule` | Nie | [Oprogramowanie pośredniczące plików domyślne](xref:fundamentals/static-files#serving-a-default-document)
**Uwierzytelnianie szyfrowane**<br>`DigestAuthenticationModule` | Tak | 
**Przeglądanie katalogów**<br>`DirectoryListingModule` | Nie | [Oprogramowanie pośredniczące przeglądania w katalogu](xref:fundamentals/static-files#enabling-directory-browsing)
**Kompresja dynamiczna**<br>`DynamicCompressionModule` | Tak | [Oprogramowanie pośredniczące kompresji odpowiedzi](xref:performance/response-compression)
**Śledzenie**<br>`FailedRequestsTracingModule` | Tak | [Rejestrowanie platformy ASP.NET Core](xref:fundamentals/logging/index#the-tracesource-provider)
**Buforowanie plików**<br>`FileCacheModule` | Nie | [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware)
**Buforowanie HTTP**<br>`HttpCacheModule` | Nie | [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware)
**Rejestrowanie HTTP**<br>`HttpLoggingModule` | Tak | [Rejestrowanie platformy ASP.NET Core](xref:fundamentals/logging/index)<br>Implementacje: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
**Przekierowywanie HTTP**<br>`HttpRedirectionModule` | Tak | [Ponowne zapisywanie adresów URL w oprogramowania pośredniczącego](xref:fundamentals/url-rewriting)
**Uwierzytelnianie mapowań certyfikatów klientów usług IIS**<br>`IISCertificateMappingAuthenticationModule` | Tak | 
**Ograniczenia adresów IP i domen**<br>`IpRestrictionModule` | Tak | 
**Filtry ISAPI**<br>`IsapiFilterModule` | Tak | [Oprogramowanie pośredniczące](xref:fundamentals/middleware)
**INTERFEJS ISAPI**<br>`IsapiModule` | Tak | [Oprogramowanie pośredniczące](xref:fundamentals/middleware)
**Obsługa protokołu**<br>`ProtocolSupportModule` | Tak | 
**Filtrowanie żądań**<br>`RequestFilteringModule` | Tak | [Ponowne zapisywanie adresów URL w oprogramowania pośredniczącego`IRule`](xref:fundamentals/url-rewriting#irule-based-rule)
**Monitor żądań**<br>`RequestMonitorModule` | Tak | 
**Ponowne zapisywanie adresów URL**<br>`RewriteModule` | Yes† | [Ponowne zapisywanie adresów URL w oprogramowania pośredniczącego](xref:fundamentals/url-rewriting)
**Strona serwera obejmuje**<br>`ServerSideIncludeModule` | Nie | 
**Kompresja statyczna**<br>`StaticCompressionModule` | Nie | [Oprogramowanie pośredniczące kompresji odpowiedzi](xref:performance/response-compression)
**Zawartość statyczna**<br>`StaticFileModule` | Nie | [Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files)
**Buforowanie tokenu**<br>`TokenCacheModule` | Tak | 
**Identyfikator URI w pamięci podręcznej**<br>`UriCacheModule` | Tak | 
**Autoryzacja adresów URL**<br>`UrlAuthorizationModule` | Tak | [Tożsamość platformy ASP.NET Core](xref:security/authentication/identity)
**Uwierzytelnianie systemu Windows**<br>`WindowsAuthenticationModule` | Tak | 

†The moduł ponowne zapisywanie adresów URL `isFile` i `isDirectory` nie współpracujesz z aplikacji platformy ASP.NET Core z powodu zmian w [struktury katalogów](xref:hosting/directory-structure).

## <a name="managed-modules"></a>Modułów zarządzanych
Moduł | .NET core aktywny | Opcja platformy ASP.NET Core
--- | :---: | ---
AnonymousIdentification | Nie | 
DefaultAuthentication | Nie | 
FileAuthorization | Nie | 
Uwierzytelniania formularzy | Nie | [Oprogramowanie pośredniczące uwierzytelniania plików cookie](xref:security/authentication/cookie)
OutputCache | Nie | [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware)
Profil | Nie | 
RoleManager | Nie | 
ScriptModule 4.0 | Nie | 
Sesja | Nie | [Oprogramowanie pośredniczące sesji](xref:fundamentals/app-state)
UrlAuthorization | Nie | 
UrlMappingsModule | Nie | [Ponowne zapisywanie adresów URL w oprogramowania pośredniczącego](xref:fundamentals/url-rewriting)
UrlRoutingModule 4.0 | Nie | [Tożsamość platformy ASP.NET Core](xref:security/authentication/identity)
WindowsAuthentication | Nie | 

## <a name="iis-manager-application-changes"></a>Zmiany aplikacji Menedżera usług IIS
Podczas konfigurowania ustawień za pomocą Menedżera usług IIS bezpośrednio zmieniasz *web.config* pliku aplikacji. Jeśli po wdrożeniu aplikacji obejmują *web.config*, wszystkie zmiany dokonane z Menedżera usług IIS zostaną zastąpione przez wdrożone *pliku web.config*. W związku z tym zmiany wprowadzone do serwera *web.config* plików, skopiować zaktualizowane *web.config* plik do lokalnej projektu bezpośrednio.

## <a name="disabling-iis-modules"></a>Wyłączanie modułów usług IIS
Jeśli moduł IIS skonfigurowanych na poziomie serwera, który ma zostać wyłączone dla aplikacji, możesz to zrobić z dodatku programu *web.config* pliku. Pozostaw modułu w miejscu i zdezaktywować przy użyciu ustawienia konfiguracji (jeśli jest dostępny) lub Usuń moduł z aplikacji.

### <a name="module-deactivation"></a>Dezaktywacja modułu
Wiele modułów zapewniają ustawienia konfiguracji, który będzie można je wyłączyć bez usuwania ich z aplikacji. Jest to najprostsza i najszybszym sposobem dezaktywować modułu. Na przykład, jeśli chcesz wyłączyć moduł ponowne zapisywanie adresów URL usług IIS, należy użyć `<httpRedirect>` element, jak pokazano poniżej. Aby uzyskać więcej informacji o wyłączaniu modułów przy użyciu ustawień konfiguracyjnych, skorzystaj z łączy w *elementy podrzędne* sekcji [IIS `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/).

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>Usunięcie modułu
Jeśli możesz zdecydować się na usunięcie modułu z ustawieniem w *web.config*, należy odblokować modułu i odblokować `<modules>` sekcji *web.config* pierwszy. Poniżej opisano kroki:

1. Odblokowanie modułem na poziomie serwera. Kliknij na serwerze usług IIS w Menedżerze usług IIS **połączeń** paska bocznego. Otwórz **modułów** w **IIS** obszaru. Polecenie moduł na liście. W **akcje** po prawej, kliknij przycisk **Unlock**. Odblokuj jak wiele modułów podczas planowania do usunięcia z *web.config* później.

2. Wdrażanie aplikacji bez `<modules>` sekcji *web.config*. W przypadku wdrożenia aplikacji z *web.config* zawierający `<modules>` sekcji bez konieczności odblokowane sekcji najpierw w Menedżerze usług IIS, programu Configuration Manager spowoduje zgłoszenie wyjątku podczas próby odblokowania sekcji. W związku z tym wdrażania aplikacji bez `<modules>` sekcji.

3. Odblokuj `<modules>` sekcji *web.config*. W **połączeń** paska bocznego, kliknij przycisk witryny sieci Web w **witryny**. W **zarządzania** obszaru, otwórz **edytora konfiguracji**. Użyj formantów nawigacji, aby wybrać `system.webServer/modules` sekcji. W **akcje** po prawej, kliknij, aby **Unlock** sekcji.

4. W tym momencie można dodać `<modules>` sekcji do Twojej *web.config* pliku z `<remove>` elementu do usunięcia modułu z aplikacji. Można dodawać wiele `<remove>` elementy, aby usunąć wiele modułów. Nie zapomnij który wprowadzeniu *web.config* zmiany na serwerze, aby były natychmiast w projekcie lokalnie. Usunięcie modułu w ten sposób nie mają wpływu na korzystanie z modułu w innych aplikacjach na serwerze.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Dla instalacji usług IIS z modułami domyślne zainstalowana, można użyć następujących `<module>` sekcji, aby usunąć domyślne moduły.

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

Można również usunąć moduł usług IIS o *Appcmd.exe*. Podaj `MODULE_NAME` i `APPLICATION_NAME` w poleceniu pokazano poniżej:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Oto jak usunąć `DynamicCompressionModule` z domyślnej witryny sieci Web:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimal-module-configuration"></a>Konfiguracja modułu minimalnego
Tylko moduły wymagane do uruchomienia aplikacji platformy ASP.NET Core to modułu uwierzytelniania anonimowego i moduł platformy ASP.NET Core.

![Otwórz Menedżera usług IIS, aby moduły z minimalną konfiguracją modułów pokazano](iis-modules/_static/modules.png)

## <a name="resources"></a>Resources
* [Publikowanie w usługach IIS](xref:publishing/iis)
* [Omówienie moduły IIS](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Dostosowywanie usług IIS 7.0 ról i modułów](https://technet.microsoft.com/library/cc627313.aspx)
* [USŁUGI IIS`<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)
