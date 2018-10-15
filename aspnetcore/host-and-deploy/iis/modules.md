---
title: Moduły usług IIS za pomocą programu ASP.NET Core
author: guardrex
description: Dowiedz się, aktywnych i nieaktywnych moduły IIS dla aplikacji platformy ASP.NET Core oraz jak zarządzać moduły usług IIS.
ms.author: riande
ms.custom: mvc
ms.date: 10/12/2018
uid: host-and-deploy/iis/modules
ms.openlocfilehash: b417d479d0c3f8b3e739d4c72b52247de0e88e56
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325955"
---
# <a name="iis-modules-with-aspnet-core"></a>Moduły usług IIS za pomocą programu ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Niektóre moduły macierzyste usług IIS i wszystkie moduły usług IIS, zarządzane nie może przetworzyć żądania dla aplikacji platformy ASP.NET Core. W wielu przypadkach platformy ASP.NET Core oferuje alternatywa scenariusze obsługiwane przez natywne i zarządzane moduły usług IIS.

## <a name="native-modules"></a>Moduły macierzyste

Tabela zawiera moduły macierzyste usług IIS, które działają na żądania zwrotnego serwera proxy aplikacji platformy ASP.NET Core.

| Moduł | Funkcjonalność aplikacji platformy ASP.NET Core | Opcja platformy ASP.NET Core |
| --- | :---: | --- |
| **Uwierzytelnianie anonimowe**<br>`AnonymousAuthenticationModule`                                  | Tak | |
| **Uwierzytelnianie podstawowe**<br>`BasicAuthenticationModule`                                          | Tak | |
| **Uwierzytelnianie mapowań certyfikacji klientów**<br>`CertificateMappingAuthenticationModule`      | Tak | |
| **CGI**<br>`CgiModule`                                                                           | Nie  | |
| **Weryfikacja konfiguracji**<br>`ConfigurationValidationModule`                                  | Tak | |
| **Błędy HTTP**<br>`CustomErrorModule`                                                           | Nie  | [Oprogramowanie pośredniczące strony kodu stanu](xref:fundamentals/error-handling#configure-status-code-pages) |
| **Funkcja Rejestrowanie niestandardowe**<br>`CustomLoggingModule`                                                      | Tak | |
| **Dokument domyślny**<br>`DefaultDocumentModule`                                                  | Nie  | [Oprogramowanie pośredniczące plików domyślne](xref:fundamentals/static-files#serve-a-default-document) |
| **Uwierzytelnianie szyfrowane**<br>`DigestAuthenticationModule`                                        | Tak | |
| **Przeglądanie katalogów**<br>`DirectoryListingModule`                                               | Nie  | [Oprogramowanie pośredniczące przeglądania w katalogu](xref:fundamentals/static-files#enable-directory-browsing) |
| **Kompresja dynamiczna**<br>`DynamicCompressionModule`                                            | Tak | [Oprogramowanie pośredniczące kompresji odpowiedzi](xref:performance/response-compression) |
| **Śledzenie**<br>`FailedRequestsTracingModule`                                                     | Tak | [Rejestrowanie platformy ASP.NET Core](xref:fundamentals/logging/index#tracesource-provider) |
| **Buforowanie plików**<br>`FileCacheModule`                                                            | Nie  | [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) |
| **Buforowanie HTTP**<br>`HttpCacheModule`                                                            | Nie  | [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) |
| **Rejestrowanie HTTP**<br>`HttpLoggingModule`                                                          | Tak | [Rejestrowanie platformy ASP.NET Core](xref:fundamentals/logging/index) |
| **Przekierowywanie HTTP**<br>`HttpRedirectionModule`                                                  | Tak | [Oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting) |
| **Uwierzytelnianie mapowań certyfikatów klientów internetowych usług informacyjnych**<br>`IISCertificateMappingAuthenticationModule` | Tak | |
| **Ograniczenia adresów IP i domen**<br>`IpRestrictionModule`                                          | Tak | |
| **Filtry ISAPI**<br>`IsapiFilterModule`                                                         | Tak | [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule`                                                                       | Tak | [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index) |
| **Obsługa protokołów**<br>`ProtocolSupportModule`                                                  | Tak | |
| **Filtrowanie żądań**<br>`RequestFilteringModule`                                                | Tak | [Oprogramowanie pośredniczące ponownego zapisywania adresów URL `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Monitor żądań**<br>`RequestMonitorModule`                                                    | Tak | |
| **Ponownego zapisywania adresów URL**&#8224;<br>`RewriteModule`                                                      | Tak | [Oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting) |
| **Server-Side Includes**<br>`ServerSideIncludeModule`                                            | Nie  | |
| **Kompresja statyczna**<br>`StaticCompressionModule`                                              | Nie  | [Oprogramowanie pośredniczące kompresji odpowiedzi](xref:performance/response-compression) |
| **Zawartość statyczna**<br>`StaticFileModule`                                                         | Nie  | [Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) |
| **Buforowanie tokenów**<br>`TokenCacheModule`                                                          | Tak | |
| **Identyfikator URI w pamięci podręcznej**<br>`UriCacheModule`                                                              | Tak | |
| **Autoryzacja adresów URL**<br>`UrlAuthorizationModule`                                                | Tak | [ASP.NET Core Identity](xref:security/authentication/identity) |
| **Uwierzytelnianie Windows**<br>`WindowsAuthenticationModule`                                      | Tak | |

&#8224;Adres URL Nadpisz modułu `isFile` i `isDirectory` typów dopasowania nie działają z aplikacji platformy ASP.NET Core z powodu zmian w [strukturę katalogów](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Moduły zarządzane

Moduły zarządzane są *nie* funkcjonalność hostowanej aplikacji platformy ASP.NET Core, gdy wersja środowiska .NET CLR pula aplikacji jest równa **bez kodu zarządzanego**. Platforma ASP.NET Core oferuje oprogramowanie pośredniczące alternatywy w kilku przypadkach.

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

W przypadku używania Menedżera usług IIS do konfigurowania ustawień, *web.config* zmodyfikowaniu pliku w aplikacji. Jeśli wdrażanie aplikacji, a jeśli tak, to w tym *web.config*, wszelkie zmiany wprowadzone za pomocą Menedżera usług IIS zostaną zastąpione przez wdrożonych *web.config* pliku. Jeśli zmiany zostaną wprowadzone na serwer *web.config* plików, skopiować zaktualizowane *web.config* plików na serwerze, aby od razu lokalnego projektu.

## <a name="disabling-iis-modules"></a>Wyłączenie modułów usług IIS

Jeśli moduł IIS jest skonfigurowany na poziomie serwera, który musi zostać wyłączone dla aplikacji, dodawania do aplikacji *web.config* pliku można wyłączyć modułu. Pozostaw modułu w miejscu i dezaktywować go za pomocą ustawienia konfiguracji (jeśli jest dostępny) lub usunięcie modułu z poziomu aplikacji.

### <a name="module-deactivation"></a>Moduł dezaktywacji

Wiele modułów oferują ustawienia konfiguracji, pozwalające im na wyłączony bez usuwania modułu z poziomu aplikacji. Jest to najprostsza i najszybszym sposobem dezaktywować modułu. Na przykład moduł przekierowania HTTP, można wyłączyć za pomocą `<httpRedirect>` element *web.config*:

```xml
<configuration>
  <system.webServer>
    <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Aby uzyskać więcej informacji o wyłączaniu modułów przy użyciu ustawień konfiguracyjnych, skorzystaj z łączy w *elementy podrzędne* części [IIS \<system.webServer >](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Usuwanie modułu

Jeśli sposób, aby usunąć moduł z ustawieniem w *web.config*, odblokować moduł i odblokować `<modules>` części *web.config* pierwszy:

1. Uzyskaj dostęp do modułu na poziomie serwera. Wybierz serwer usług IIS w Menedżerze usług IIS **połączeń** pasku bocznym. Otwórz **modułów** w **IIS** obszaru. Wybierz moduł z listy. W **akcje** pasku bocznym po prawej stronie, wybierz **Unlock**. Odblokować dowolną liczbę modułów związanych z planowaniem do usunięcia z *web.config* później.

2. Wdrażanie aplikacji bez `<modules>` sekcji *web.config*. Jeśli aplikacja jest wdrożona za pomocą *web.config* zawierający `<modules>` sekcji bez konieczności odblokować sekcji najpierw w Menedżerze programu IIS, programu Configuration Manager zgłasza wyjątek podczas próby odblokowania sekcji. W związku z tym, Wdróż aplikację bez `<modules>` sekcji.

3. Odblokuj `<modules>` części *web.config*. W **połączeń** pasku bocznym, wybierz witrynę internetową w **witryn**. W **zarządzania** obszarze Otwórz **edytora konfiguracji**. Użyj formantów nawigacji, aby wybrać `system.webServer/modules` sekcji. W **akcje** pasku bocznym po prawej stronie, wybierz, aby **Unlock** sekcji.

4. W tym momencie `<modules>` sekcji mogą być dodawane do *web.config* plik z `<remove>` elementu do usunięcia modułu z poziomu aplikacji. Wiele `<remove>` elementy mogą być dodawane do usunąć wiele modułów. Jeśli *web.config* zmian na serwerze, natychmiast wprowadzenie identycznych zmian w projekcie *web.config* plik lokalnie. Usuwanie modułu w ten sposób nie wpływa na modułu z innymi aplikacjami na serwerze.

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

Na przykład usunąć `DynamicCompressionModule` z domyślna witryna sieci Web:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Minimalną konfiguracją modułów

Tylko moduły wymagane do uruchamiania aplikacji ASP.NET Core to moduł uwierzytelniania anonimowego i modułu ASP.NET Core.

Moduł buforowania identyfikatora URI (`UriCacheModule`) umożliwia usługom IIS do pamięci podręcznej konfiguracji witryny sieci Web na poziomie adresu URL. Bez tego modułu IIS musi odczytywać i analizować konfiguracji na każde żądanie, nawet wtedy, gdy wymagane są wielokrotnie tego samego adresu URL. Każde żądanie podczas analizowania konfiguracji powoduje spadek istotnie poprawiającą wydajność. *Mimo że moduł buforowania identyfikatora URI nie jest bezwzględnie konieczne hostowanej aplikacji platformy ASP.NET Core uruchomić, zaleca się włączenie modułu pamięci podręcznej URI we wszystkich wdrożeniach platformy ASP.NET Core.*

Moduł buforowanie HTTP (`HttpCacheModule`) implementuje wyjściowej pamięci podręcznej usług IIS, a także logikę buforowania elementów w pamięci podręcznej HTTP.sys. Bez tego modułu zawartość nie jest buforowany w trybie jądra, a pamięć podręczna profile są ignorowane. Usunięcie modułu buforowanie HTTP, zwykle ma negatywny wpływ na wydajność i użycie zasobów. *Mimo że moduł buforowanie HTTP nie jest bezwzględnie konieczne hostowanej aplikacji platformy ASP.NET Core uruchomić, zaleca się włączenie moduł buforowanie HTTP we wszystkich wdrożeniach platformy ASP.NET Core.*

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/iis/index>
* [Wprowadzenie do architektury usługi IIS: modułów w usługach IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Przegląd moduły usług IIS](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Dostosowywanie usług IIS 7.0 ról i modułów](https://technet.microsoft.com/library/cc627313.aspx)
* [USŁUGI IIS `<system.webServer>`](/iis/configuration/system.webServer/)
