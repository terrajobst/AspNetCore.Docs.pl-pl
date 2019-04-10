---
title: Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak skonfigurować uwierzytelnianie Windows w programie ASP.NET Core, za pomocą usług IIS Express, usługi IIS i sterownik HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 04/03/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: bd4ffa79c4d1e0070c820fa9c06b0a84c3aaae74
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468656"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core

Przez [Scott Addie](https://twitter.com/Scott_Addie) i [Luke Latham](https://github.com/guardrex)

[Uwierzytelnianie Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) można skonfigurować dla aplikacji platformy ASP.NET Core z [IIS](xref:host-and-deploy/iis/index) lub [HTTP.sys](xref:fundamentals/servers/httpsys).

Uwierzytelnianie Windows opiera się uwierzytelniać użytkowników aplikacji platformy ASP.NET Core w systemie operacyjnym. Możesz użyć uwierzytelniania Windows, gdy serwer działa w sieci firmowej przy użyciu tożsamości domeny usługi Active Directory lub konta Windows do identyfikacji użytkowników. Uwierzytelnianie Windows najlepiej nadaje się do środowisk intranetowych, w którym użytkownicy, aplikacje klienckie i serwery sieci web należą do tej samej domeny Windows.

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Włączanie uwierzytelniania Windows w aplikacji ASP.NET Core

**Aplikacji sieci Web** szablonu dostępnego za pośrednictwem programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core może być skonfigurowane do obsługi uwierzytelniania Windows.

# [<a name="visual-studio"></a>Visual Studio](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a>Korzystanie z szablonu aplikacji uwierzytelniania Windows dla nowego projektu

W programie Visual Studio:

1. Utwórz nowy projekt.
1. Wybierz **aplikacji sieci Web platformy ASP.NET Core**. Wybierz opcję **Dalej**.
1. Podaj nazwę w **Nazwa projektu** pola. Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu. Wybierz pozycję **Utwórz**.
1. Wybierz **zmiany** w obszarze **uwierzytelniania**.
1. W **Zmień uwierzytelnianie** wybierz **uwierzytelniania Windows**. Kliknij przycisk **OK**.
1. Wybierz **aplikacji sieci Web**.
1. Wybierz pozycję **Utwórz**.

Uruchom aplikację. Nazwa użytkownika pojawia się w interfejsie użytkownika aplikacji renderowany.

### <a name="manual-configuration-for-an-existing-project"></a>Ręcznej konfiguracji dla istniejącego projektu

Właściwości projektu umożliwiają uwierzytelnianie Windows włączyć i wyłączyć uwierzytelnianie anonimowe:

1. Kliknij prawym przyciskiem myszy projekt w programie Visual Studio **Eksploratora rozwiązań** i wybierz **właściwości**.
1. Wybierz **debugowania** kartę.
1. Usuń zaznaczenie pola wyboru dla **Włącz uwierzytelnianie anonimowe**.
1. Zaznacz pole wyboru dla **Włącz uwierzytelnianie Windows**.

Alternatywnie, można skonfigurować właściwości w `iisSettings` węźle *launchSettings.json* pliku:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# [<a name="net-core-cli"></a>.NET core interfejsu wiersza polecenia](#tab/netcore-cli)

Użyj **uwierzytelniania Windows** szablonu aplikacji.

Wykonaj [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia `webapp` argumentu (aplikację sieci Web platformy ASP.NET Core) i `--auth Windows` przełącznika:

```console
dotnet new webapp --auth Windows
```

---

Podczas modyfikowania istniejącego projektu, upewnij się, że plik projektu zawiera odwołania do pakietu dla [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **lub** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) pakietu NuGet.

## <a name="enable-windows-authentication-with-iis"></a>Włączanie uwierzytelniania Windows za pomocą usług IIS

Usługi IIS używają [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) hostująca aplikacje platformy ASP.NET Core. Uwierzytelnianie Windows jest skonfigurowany dla usług IIS za pomocą *web.config* pliku. Następujące sekcje show jak:

* Zapewnia lokalny *web.config* pliku, który aktywuje uwierzytelniania Windows na serwerze, gdy aplikacja jest wdrożona.
* Konfigurowanie za pomocą Menedżera usług IIS *web.config* pliku aplikacji platformy ASP.NET Core, która została już wdrożona na serwerze.

### <a name="iis-configuration"></a>Konfiguracja programu IIS

Jeśli jeszcze tego nie zrobiono, Włącz usługi IIS do hostowania aplikacji platformy ASP.NET Core. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/iis/index>.

Włączyć usługi roli usług IIS na potrzeby uwierzytelniania Windows. Aby uzyskać więcej informacji, zobacz [Włącz uwierzytelnianie Windows usługami roli usług IIS (zobacz krok 2)](xref:host-and-deploy/iis/index#iis-configuration).

Oprogramowania pośredniczącego integracji usługi IIS jest domyślnie skonfigurowana do automatycznego uwierzytelniania żądań. Aby uzyskać więcej informacji, zobacz [hosta ASP.NET Core na Windows za pomocą programu IIS: Opcje usług IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

Modułu ASP.NET Core jest domyślnie skonfigurowana do przekazywania tokenu uwierzytelniania Windows do aplikacji. Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core: Atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Utwórz nową witrynę IIS

Określ nazwę i folder, a następnie zezwala utworzyć nową pulę aplikacji.

### <a name="enable-windows-authentication-for-the-app-in-iis"></a>Włącz uwierzytelnianie Windows dla aplikacji w usługach IIS

Użyj **albo** z następujących metod:

* [Programowanie — konfiguracja po stronie przed opublikowaniem aplikacji](#development-side-configuration-with-a-local-webconfig-file) (*zalecane*)
* [Konfiguracja po stronie serwera po opublikowaniu aplikacji](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a>Programowanie — konfiguracja po stronie przy użyciu pliku lokalnego pliku web.config

Wykonaj poniższe kroki **przed** możesz [publikowanie i wdrażanie projektu](#publish-and-deploy-your-project-to-the-iis-site-folder).

Dodaj następujący kod *web.config* plik do katalogu głównego projektu:

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

Gdy projekt zostanie opublikowany przez zestaw SDK (bez `<IsTransformWebConfigDisabled>` właściwością `true` w pliku projektu), opublikowanego *web.config* plik zawiera `<location><system.webServer><security><authentication>` sekcji. Aby uzyskać więcej informacji na temat `<IsTransformWebConfigDisabled>` właściwości, zobacz <xref:host-and-deploy/iis/index#webconfig-file>.

#### <a name="server-side-configuration-with-the-iis-manager"></a>Konfiguracja po stronie serwera za pomocą Menedżera usług IIS

Wykonaj poniższe kroki **po** możesz [publikowanie i wdrażanie projektu](#publish-and-deploy-your-project-to-the-iis-site-folder).

1. W Menedżerze usług IIS wybierz witrynę IIS, w obszarze **witryn** węźle **połączeń** pasku bocznym.
1. Kliknij dwukrotnie **uwierzytelniania** w **IIS** obszaru.
1. Wybierz **uwierzytelnianie anonimowe**. Wybierz **wyłączyć** w **akcje** pasku bocznym.
1. Wybierz **uwierzytelniania Windows**. Wybierz **Włącz** w **akcje** pasku bocznym.

Kiedy te akcje są wykonywane, Menedżera usług IIS modyfikuje aplikacji *web.config* pliku. A `<system.webServer><security><authentication>` węzeł zostanie dodany ze zaktualizowanymi ustawieniami dla `anonymousAuthentication` i `windowsAuthentication`:

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

`<system.webServer>` Dodany do sekcji *web.config* pliku przez Menedżera usług IIS znajduje się poza jej `<location>` sekcji dodawane przez program .NET Core SDK po opublikowaniu aplikacji. Ponieważ sekcji zostanie dodany poza `<location>` węzła, ustawienia są dziedziczone przez żaden [aplikacji podrzędnych](xref:host-and-deploy/iis/index#sub-applications) do bieżącej aplikacji. Aby zapobiec dziedziczenia, Przenieś dodany `<security>` sekcji wewnątrz `<location><system.webServer>` sekcji podanym w zestawie SDK.

W przypadku Menedżera usług IIS można dodać konfiguracji usług IIS dotyczy tylko aplikacji *web.config* pliku na serwerze. Kolejne wdrożenie aplikacji może zastąpić ustawienia na serwerze, jeśli kopię serwera *web.config* zastępuje projektu *web.config* pliku. Użyj **albo** z następujących metod do zarządzania ustawieniami:

* Użyj Menedżera usług IIS, aby zresetować ustawienia w *web.config* pliku po plik jest zastępowany we wdrożeniu.
* Dodaj *pliku web.config* aplikacji lokalnie przy użyciu ustawień. Aby uzyskać więcej informacji, zobacz [rozwoju — konfiguracja po stronie](#development-side-configuration-with-a-local-webconfig-file) sekcji.

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a>Publikowanie i wdrażanie projektu do folderu witryny usług IIS

Za pomocą programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core, publikowanie i wdrażanie aplikacji do folderu docelowego.

Aby uzyskać więcej informacji dotyczących obsługi za pomocą programu IIS publikowania i wdrażania, zobacz następujące tematy:

* [Publikowanie DotNet](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

Uruchom aplikację, aby sprawdzić, czy działa uwierzytelniania Windows.

## <a name="enable-windows-authentication-with-httpsys"></a>Włącz uwierzytelnianie Windows w pliku HTTP.sys

Chociaż Kestrel nie obsługuje uwierzytelniania Windows, możesz użyć [HTTP.sys](xref:fundamentals/servers/httpsys) do obsługi scenariuszy samodzielnie hostowanego na Windows. Poniższy przykład umożliwia skonfigurowanie aplikacji hosta sieci web HTTP.sys za pomocą uwierzytelniania Windows:

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> Sterownik HTTP.sys delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos. Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i sterownik HTTP.sys. Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika. Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.

> [!NOTE]
> Sterownik HTTP.sys nie jest obsługiwana na serwerze Nano Server w wersji 1709 lub nowszej. Aby użyć uwierzytelniania Windows i sterownik HTTP.sys systemu Nano Server, użyj [Server Core (microsoft/windowsservercore) kontenera](https://hub.docker.com/r/microsoft/windowsservercore/). Aby uzyskać więcej informacji na temat Server Core, zobacz [co to jest opcja instalacji Server Core w systemie Windows Server?](/windows-server/administration/server-core/what-is-server-core).

## <a name="work-with-windows-authentication"></a>Praca z uwierzytelnianiem Windows

Stan konfiguracji dostępu anonimowego określa sposób, w którym `[Authorize]` i `[AllowAnonymous]` atrybuty są używane w aplikacji. W poniższych sekcjach dwóch opisano sposób obsługi konfiguracji niedozwolonych i dozwolone stany dostęp anonimowy.

### <a name="disallow-anonymous-access"></a>Nie zezwalaj na dostęp anonimowy

Gdy jest włączone uwierzytelnianie Windows i dostęp anonimowy jest wyłączony, `[Authorize]` i `[AllowAnonymous]` atrybuty mają żadnego skutku. Jeśli nie zezwala na dostęp anonimowy skonfigurowano witryny usług IIS (lub sterownik HTTP.sys), żądanie nigdy nie osiągnie Twojej aplikacji. Z tego powodu `[AllowAnonymous]` atrybut nie ma zastosowania.

### <a name="allow-anonymous-access"></a>Zezwalaj na dostęp anonimowy

Po włączeniu uwierzytelniania Windows i dostęp anonimowy używać `[Authorize]` i `[AllowAnonymous]` atrybutów. `[Authorize]` Atrybut umożliwia zabezpieczenie rodzajów aplikacji, które naprawdę wymagają uwierzytelniania Windows. `[AllowAnonymous]` Atrybutu zastąpienia `[Authorize]` atrybutu do użycia w aplikacjach, które zezwala na dostęp anonimowy. Zobacz [autoryzacja prosta](xref:security/authorization/simple) szczegóły użycia atrybutu.

W programie ASP.NET Core 2.x, `[Authorize]` atrybut wymaga dodatkowej konfiguracji w *Startup.cs* zażąda anonimowe żądania uwierzytelniania Windows. Zalecana konfiguracja zależy od nieco używany serwer sieci web.

> [!NOTE]
> Domyślnie użytkownicy, którzy nie mają autoryzacji do dostępu do strony są prezentowane z pustą odpowiedź HTTP 403. [Oprogramowania pośredniczącego StatusCodePages](xref:fundamentals/error-handling#usestatuscodepages) można skonfigurować, aby zapewnić użytkownikom lepsze środowisko "Odmowa dostępu".

#### <a name="iis"></a>IIS

Jeśli korzystanie z usług IIS, należy dodać następujące polecenie, aby `ConfigureServices` metody:

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Jeśli używasz HTTP.sys, Dodaj następujące polecenie, aby `ConfigureServices` metody:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Personifikacja

Platforma ASP.NET Core nie implementuje personifikacji. Aplikacje są uruchamiane przy użyciu tożsamości przez aplikację dla wszystkich żądań, przy użyciu tożsamości puli lub procesu aplikacji. Jeśli musisz jawnie wykonaj akcję w imieniu użytkownika, należy użyć [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) w [oprogramowania pośredniczącego terminalu wbudowane](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) w `Startup.Configure`. Uruchomić jedną akcję w tym kontekście, a następnie zamknij kontekstu.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` nie obsługuje operacji asynchronicznych i nie powinny być używane w przypadku złożonych scenariuszy. Na przykład zawijania całego żądania lub łańcuchów oprogramowanie pośredniczące nie jest obsługiwany lub zalecane.

### <a name="claims-transformations"></a>Przekształcenia oświadczeń

W przypadku hostowania w trybie usług IIS w trakcie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wewnętrznie wywoływana w celu zainicjowania przez użytkownika. W związku z tym <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacji używanego do przekształcania oświadczeń, po każdym uwierzytelniania nie jest aktywowana domyślnie. Aby uzyskać więcej informacji i przykładowy kod, który aktywuje przekształcenia oświadczeń w przypadku hostowania w procesie, zobacz <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.
