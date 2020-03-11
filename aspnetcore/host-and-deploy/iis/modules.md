---
title: Moduły usług IIS z ASP.NET Core
author: rick-anderson
description: Dowiedz się, aktywnych i nieaktywnych moduły IIS dla aplikacji platformy ASP.NET Core oraz jak zarządzać moduły usług IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 0f13ef3eb1da03960ef1fa54d33532b6ebbdc128
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657907"
---
# <a name="iis-modules-with-aspnet-core"></a>Moduły usług IIS z ASP.NET Core

Niektóre z natywnych modułów usług IIS i wszystkich modułów zarządzanych przez usługi IIS nie mogą przetwarzać żądań dla ASP.NET Core aplikacji. W wielu przypadkach ASP.NET Core oferuje alternatywę dla scenariuszy rozłożonych przez moduły natywne i zarządzane usług IIS.

## <a name="native-modules"></a>Moduły macierzyste

Tabela wskazuje natywne moduły usług IIS, które działają w aplikacjach ASP.NET Core i ASP.NET Core Module.

| Moduł | Funkcjonalne z ASP.NET Core aplikacjami | Opcja ASP.NET Core |
| --- | :---: | --- |
| **Uwierzytelnianie anonimowe**<br>`AnonymousAuthenticationModule`                                  | Yes | |
| **Uwierzytelnianie podstawowe**<br>`BasicAuthenticationModule`                                          | Yes | |
| **Uwierzytelnianie mapowań certyfikatów klientów**<br>`CertificateMappingAuthenticationModule`      | Yes | |
| **Aplikacja**<br>`CgiModule`                                                                           | Nie  | |
| **Weryfikacja konfiguracji**<br>`ConfigurationValidationModule`                                  | Yes | |
| **Błędy HTTP**<br>`CustomErrorModule`                                                           | Nie  | [Oprogramowanie pośredniczące stron kodu stanu](xref:fundamentals/error-handling#usestatuscodepages) |
| **Rejestrowanie niestandardowe**<br>`CustomLoggingModule`                                                      | Yes | |
| **Dokument domyślny**<br>`DefaultDocumentModule`                                                  | Nie  | [Pliki domyślne oprogramowania pośredniczącego](xref:fundamentals/static-files#serve-a-default-document) |
| **Uwierzytelnianie szyfrowane**<br>`DigestAuthenticationModule`                                        | Yes | |
| **Przeglądanie katalogów**<br>`DirectoryListingModule`                                               | Nie  | [Oprogramowanie pośredniczące przeglądania katalogów](xref:fundamentals/static-files#enable-directory-browsing) |
| **Kompresja dynamiczna**<br>`DynamicCompressionModule`                                            | Yes | [Oprogramowanie pośredniczące kompresji odpowiedzi](xref:performance/response-compression) |
| **Śledzenie nieudanych żądań**<br>`FailedRequestsTracingModule`                                     | Yes | [Rejestrowanie ASP.NET Core](xref:fundamentals/logging/index#tracesource-provider) |
| **Buforowanie plików**<br>`FileCacheModule`                                                            | Nie  | [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) |
| **Buforowanie HTTP**<br>`HttpCacheModule`                                                            | Nie  | [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) |
| **Rejestrowanie HTTP**<br>`HttpLoggingModule`                                                          | Yes | [Rejestrowanie ASP.NET Core](xref:fundamentals/logging/index) |
| **Przekierowywanie HTTP**<br>`HttpRedirectionModule`                                                  | Yes | [Oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting) |
| **Śledzenie HTTP**<br>`TracingModule`                                                              | Yes | |
| **Uwierzytelnianie mapowania certyfikatu klienta usług IIS**<br>`IISCertificateMappingAuthenticationModule` | Yes | |
| **Ograniczenia adresów IP i domen**<br>`IpRestrictionModule`                                          | Yes | |
| **Filtry ISAPI**<br>`IsapiFilterModule`                                                         | Yes | [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index) |
| **INTERCEPTOR**<br>`IsapiModule`                                                                       | Yes | [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index) |
| **Obsługa protokołu**<br>`ProtocolSupportModule`                                                  | Yes | |
| **Filtrowanie żądań**<br>`RequestFilteringModule`                                                | Yes | [Ponowne zapisywanie adresu URL `IRule` oprogramowania pośredniczącego](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Monitor żądań**<br>`RequestMonitorModule`                                                    | Yes | |
| **Ponowne zapisywanie adresów URL**&#8224;<br>`RewriteModule`                                                      | Yes | [Oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting) |
| **Zawiera po stronie serwera**<br>`ServerSideIncludeModule`                                            | Nie  | |
| **Kompresja statyczna**<br>`StaticCompressionModule`                                              | Nie  | [Oprogramowanie pośredniczące kompresji odpowiedzi](xref:performance/response-compression) |
| **Zawartość statyczna**<br>`StaticFileModule`                                                         | Nie  | [Oprogramowanie pośredniczące plików statycznych](xref:fundamentals/static-files) |
| **Buforowanie tokenów**<br>`TokenCacheModule`                                                          | Yes | |
| **Buforowanie URI**<br>`UriCacheModule`                                                              | Yes | |
| **Autoryzacja adresów URL**<br>`UrlAuthorizationModule`                                                | Yes | [ASP.NET Core tożsamość](xref:security/authentication/identity) |
| **Uwierzytelnianie systemu Windows**<br>`WindowsAuthenticationModule`                                      | Yes | |

&#8224;Typ `isFile` i `isDirectory` dopasowania modułu ponownego zapisywania adresu URL nie działają z aplikacjami ASP.NET Core ze względu na zmiany w [strukturze katalogów](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Moduły zarządzane

Moduły zarządzane *nie* działają w przypadku hostowanych aplikacji ASP.NET Core, gdy wersja środowiska .NET CLR puli aplikacji jest ustawiona na **Brak kodu zarządzanego**. ASP.NET Core oferuje alternatywy dla oprogramowania pośredniczącego w kilku przypadkach.

| Moduł                  | Opcja ASP.NET Core |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Oprogramowanie pośredniczące uwierzytelniania plików cookie](xref:security/authentication/cookie) |
| OutputCache             | [Oprogramowanie pośredniczące buforowania odpowiedzi](xref:performance/caching/middleware) |
| Profil                 | |
| RoleManager             | |
| ScriptModule-4,0        | |
| Sesja                 | [Oprogramowanie pośredniczące sesji](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [Oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | [ASP.NET Core tożsamość](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>Zmiany aplikacji Menedżera usług IIS

W przypadku konfigurowania ustawień przy użyciu Menedżera usług IIS zostanie zmieniony plik *Web. config* aplikacji. W przypadku wdrażania aplikacji, w tym pliku *Web. config*, wszelkie zmiany wprowadzone za pomocą Menedżera usług IIS są zastępowane przez wdrożony plik *Web. config* . Jeśli w pliku *Web. config* serwera wprowadzono zmiany, skopiuj zaktualizowany plik *Web. config* na serwerze bezpośrednio do lokalnego projektu.

## <a name="disabling-iis-modules"></a>Wyłączanie modułów usług IIS

Jeśli moduł IIS jest skonfigurowany na poziomie serwera, który musi być wyłączony dla aplikacji, dodanie do pliku *Web. config* aplikacji może spowodować wyłączenie modułu. Pozostaw moduł w miejscu i Dezaktywuj go przy użyciu ustawienia konfiguracji (jeśli jest dostępne) lub usuń moduł z aplikacji.

### <a name="module-deactivation"></a>Dezaktywacja modułu

Wiele modułów oferuje ustawienie konfiguracji umożliwiające wyłączenie go bez usuwania modułu z aplikacji. Jest to najprostszy i najszybszy sposób dezaktywowania modułu. Na przykład moduł przekierowania HTTP można wyłączyć za pomocą elementu `<httpRedirect>` w *pliku Web. config*:

```xml
<configuration>
  <system.webServer>
    <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Aby uzyskać więcej informacji na temat wyłączania modułów z ustawieniami konfiguracji, postępuj zgodnie z linkami w sekcji *elementy podrzędne* [usług IIS \<system. WebServer >](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Usuwanie modułu

Jeśli nie chcesz usunąć modułu z ustawieniem w *pliku Web. config*, Odblokuj moduł i odblokuj `<modules>` sekcji *Web. config* w pierwszej kolejności:

1. Odblokuj moduł na poziomie serwera. Na pasku bocznym **połączenia** Menedżera usług IIS wybierz serwer IIS. Otwórz **moduły** w obszarze **IIS** . Wybierz moduł z listy. Na pasku bocznym **Akcje** po prawej stronie wybierz pozycję **Odblokuj**. Jeśli wpis akcji dla modułu pojawia się jako **Blokada**, moduł jest już odblokowany i nie jest wymagana żadna akcja. Odblokuj tyle modułów, ile planujesz usunąć z *pliku Web. config* później.

2. Wdróż aplikację bez sekcji `<modules>` w *pliku Web. config*. Jeśli aplikacja jest wdrażana za pomocą *pliku Web. config* zawierającego `<modules>` sekcji bez uprzedniego odblokowywania sekcji w Menedżerze usług IIS, Configuration Manager zgłasza wyjątek podczas próby odblokowania sekcji. W związku z tym Wdróż aplikację bez sekcji `<modules>`.

3. Odblokuj sekcję `<modules>` *pliku Web. config*. Na pasku bocznym **połączenia** wybierz witrynę sieci Web w obszarze **Lokacje**. W obszarze **Zarządzanie** Otwórz **Edytor konfiguracji**. Użyj kontrolek nawigacji, aby wybrać sekcję `system.webServer/modules`. Na pasku bocznym **Akcje** po prawej stronie wybierz opcję **odblokowania** sekcji. Jeśli wpis akcji dla sekcji modułu jest wyświetlany jako **sekcja blokady**, sekcja modułu jest już odblokowana i nie jest wymagana żadna akcja.

4. Dodaj sekcję `<modules>` do lokalnego pliku *Web. config* aplikacji z elementem `<remove>`, aby usunąć moduł z aplikacji. Dodaj wiele elementów `<remove>`, aby usunąć wiele modułów. Jeśli na serwerze wprowadzono zmiany w pliku *Web. config* , należy natychmiast wprowadzić te same zmiany w plikach *Web. config* projektu lokalnie. Usunięcie modułu korzystającego z tego podejścia nie ma wpływu na używanie modułu z innymi aplikacjami na serwerze.

   ```xml
   <configuration>
    <system.webServer>
      <modules>
        <remove name="MODULE_NAME" />
      </modules>
    </system.webServer>
   </configuration>
   ```

Aby dodać lub usunąć moduły IIS Express przy użyciu *pliku Web. config*, zmodyfikuj *plik ApplicationHost. config* w celu odblokowania sekcji `<modules>`:

1. Otwórz *aplikację {Application root}\\. vs\config\applicationhost.config*.

1. Znajdź `<section>` element dla modułów usług IIS i Zmień `overrideModeDefault` `Deny` na `Allow`:

   ```xml
   <section name="modules"
            allowDefinition="MachineToApplication"
            overrideModeDefault="Allow" />
   ```

1. Znajdź sekcję `<location path="" overrideMode="Allow"><system.webServer><modules>`. Dla wszystkich modułów, które chcesz usunąć, ustaw `lockItem` z `true` na `false`. W poniższym przykładzie moduł CGI jest odblokowany:

   ```xml
   <add name="CgiModule" lockItem="false" />
   ```

1. Po odblokowaniu sekcji `<modules>` i poszczególnych modułów można dodać lub usunąć moduły usług IIS przy użyciu pliku *Web. config* aplikacji na potrzeby uruchamiania aplikacji na IIS Express.

Moduł IIS można także usunąć za pomocą narzędzia *Appcmd. exe*. Podaj `MODULE_NAME` i `APPLICATION_NAME` w poleceniu:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Na przykład Usuń `DynamicCompressionModule` z domyślnej witryny sieci Web:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Minimalna konfiguracja modułu

Jedyne moduły wymagane do uruchomienia aplikacji ASP.NET Core są modułem uwierzytelniania anonimowego i modułem ASP.NET Core.

Moduł buforowania URI (`UriCacheModule`) umożliwia usługom IIS buforowanie konfiguracji witryny sieci Web na poziomie adresu URL. Bez tego modułu usługi IIS muszą odczytywać i analizować konfigurację dla każdego żądania, nawet jeśli ten sam adres URL jest wielokrotnie żądany. Analizowanie konfiguracji każde żądanie skutkuje znaczącą wydajnością. *Chociaż moduł buforowania URI nie jest ściśle wymagany do uruchomienia hostowanej aplikacji ASP.NET Core, zalecamy włączenie modułu buforowania URI dla wszystkich wdrożeń ASP.NET Core.*

Moduł buforowania HTTP (`HttpCacheModule`) implementuje wyjściową pamięć podręczną programu IIS, a także logikę dla elementów buforowania w pamięci podręcznej HTTP. sys. Bez tego modułu zawartość nie jest już buforowana w trybie jądra, a profile pamięci podręcznej są ignorowane. Usunięcie modułu buforowania HTTP zwykle ma niekorzystne skutki dla wydajności i użycia zasobów. *Chociaż moduł buforowania HTTP nie jest ściśle wymagany do uruchomienia hostowanej aplikacji ASP.NET Core, zalecamy włączenie modułu buforowania HTTP we wszystkich wdrożeniach ASP.NET Core.*

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do architektury usług IIS: moduły w usługach IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Przegląd modułów usług IIS](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Dostosowywanie ról i modułów usług IIS 7,0](https://technet.microsoft.com/library/cc627313.aspx)
* [Program IIS \<system. WebServer >](/iis/configuration/system.webServer/)
