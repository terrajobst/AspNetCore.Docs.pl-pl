---
title: Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core
author: ardalis
description: W tym artykule opisano sposób konfigurowania uwierzytelniania Windows w programie ASP.NET Core, za pomocą usług IIS Express, usługi IIS, sterownik HTTP.sys i WebListener.
ms.author: riande
ms.date: 08/18/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: a8066d248c0d4db1d1f61b2a14bdb4656a2f4265
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312415"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com) i [Scott Addie](https://twitter.com/Scott_Addie)

Dla aplikacji platformy ASP.NET Core, obsługiwane w przypadku usług IIS można skonfigurować uwierzytelniania Windows [HTTP.sys](xref:fundamentals/servers/httpsys), lub [WebListener](xref:fundamentals/servers/weblistener).

## <a name="windows-authentication"></a>Uwierzytelnianie systemu Windows

Uwierzytelnianie Windows opiera się uwierzytelniać użytkowników aplikacji platformy ASP.NET Core w systemie operacyjnym. Możesz użyć uwierzytelniania Windows, gdy serwer działa w sieci firmowej przy użyciu tożsamości domeny usługi Active Directory lub innym kontom Windows do identyfikacji użytkowników. Uwierzytelnianie Windows najlepiej nadaje się do środowisk intranetowych, w których użytkownicy, aplikacje klienckie i serwery sieci web należą do tej samej domeny Windows.

[Dowiedz się więcej o uwierzytelnianiu Windows i instalując je dla usług IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Włączanie uwierzytelniania Windows w aplikacji ASP.NET Core

Szablon aplikacji sieci Web w usłudze Visual Studio można skonfigurować tak, aby zapewnić obsługę uwierzytelniania Windows.

### <a name="use-the-windows-authentication-app-template"></a>Korzystanie z szablonu aplikacji uwierzytelnianie Windows

W programie Visual Studio:

1. Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.
1. Wybierz aplikację sieci Web z listy szablonów.
1. Wybierz **Zmień uwierzytelnianie** i wybrać **uwierzytelniania Windows**.

Uruchom aplikację. Nazwa użytkownika jest wyświetlana w prawym górnym rogu aplikacji.

![Zrzut ekranu przeglądarki uwierzytelnianie Windows](windowsauth/_static/browser-screenshot.png)

Prace deweloperskie za pomocą usług IIS Express szablon zawiera wszystkie konfiguracje niezbędne do korzystania z uwierzytelniania Windows. W poniższej sekcji pokazano, jak ręcznie skonfigurować aplikację ASP.NET Core dla uwierzytelniania Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Ustawienia programu Visual Studio for Windows i uwierzytelnianie anonimowe

Projekt programu Visual Studio **właściwości** strony **debugowania** karta zawiera pola wyboru dla uwierzytelniania Windows i uwierzytelnianie anonimowe.

![Zrzut ekranu przeglądarki uwierzytelnianie Windows](windowsauth/_static/vs-auth-property-menu.png)

Alternatywnie, można skonfigurować te dwie właściwości w *launchSettings.json* pliku:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Włączanie uwierzytelniania Windows za pomocą usług IIS

Usługi IIS używają [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) hostująca aplikacje platformy ASP.NET Core. Uwierzytelnianie Windows jest skonfigurowany w usługach IIS, a nie aplikacji. Poniższe sekcje pokazują, jak skonfigurować aplikację ASP.NET Core, aby korzystać z uwierzytelniania Windows za pomocą Menedżera usług IIS.

### <a name="iis-configuration"></a>Konfiguracja programu IIS

Włączyć usługi roli usług IIS na potrzeby uwierzytelniania Windows. Aby uzyskać więcej informacji, zobacz [Włącz uwierzytelnianie Windows usługami roli usług IIS (zobacz krok 2)](xref:host-and-deploy/iis/index#iis-configuration).

Oprogramowania pośredniczącego integracji usługi IIS jest domyślnie skonfigurowana do automatycznego uwierzytelniania żądań. Aby uzyskać więcej informacji, zobacz [hosta ASP.NET Core na Windows za pomocą programu IIS: IIS opcje (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

Modułu ASP.NET Core jest domyślnie skonfigurowana do przekazywania tokenu uwierzytelniania Windows do aplikacji. Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core: atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Utwórz nową witrynę IIS

Określ nazwę i folder, a następnie zezwala utworzyć nową pulę aplikacji.

### <a name="customize-authentication"></a>Dostosowywanie uwierzytelniania

Otwórz funkcje uwierzytelniania dla tej witryny.

![Menu uwierzytelniania usług IIS](windowsauth/_static/iis-authentication-menu.png)

Wyłączyć uwierzytelnianie anonimowe, a następnie włączyć uwierzytelnianie Windows.

![Ustawienia uwierzytelniania usług IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Publikowanie projektu do folderu witryny usług IIS

Za pomocą programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core, Opublikuj aplikację w folderze docelowym.

![Okno dialogowe publikowanie w programie Visual Studio](windowsauth/_static/vs-publish-app.png)

Dowiedz się więcej o [publikowania w usługach IIS](xref:host-and-deploy/iis/index).

Uruchom aplikację, aby sprawdzić, czy działa uwierzytelniania Windows.

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a>Włącz uwierzytelnianie Windows w pliku HTTP.sys

Chociaż Kestrel nie obsługuje uwierzytelniania Windows, możesz użyć [HTTP.sys](xref:fundamentals/servers/httpsys) do obsługi scenariuszy samodzielnie hostowanego na Windows. Poniższy przykład umożliwia skonfigurowanie aplikacji hosta sieci web HTTP.sys za pomocą uwierzytelniania Windows:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> Sterownik HTTP.sys delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos. Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i sterownik HTTP.sys. Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika. Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a>Włączanie uwierzytelniania Windows za pomocą WebListener

Chociaż Kestrel nie obsługuje uwierzytelniania Windows, możesz użyć [WebListener](xref:fundamentals/servers/weblistener) do obsługi scenariuszy samodzielnie hostowanego na Windows. Poniższy przykład umożliwia skonfigurowanie aplikacji sieci web hosta WebListener za pomocą uwierzytelniania Windows:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> WebListener delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos. Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i WebListener. Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika. Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.

::: moniker-end

## <a name="work-with-windows-authentication"></a>Praca z uwierzytelnianiem Windows

Stan konfiguracji dostępu anonimowego określa sposób, w którym `[Authorize]` i `[AllowAnonymous]` atrybuty są używane w aplikacji. W poniższych sekcjach dwóch opisano sposób obsługi konfiguracji niedozwolonych i dozwolone stany dostęp anonimowy.

### <a name="disallow-anonymous-access"></a>Nie zezwalaj na dostęp anonimowy

Gdy jest włączone uwierzytelnianie Windows i dostęp anonimowy jest wyłączony, `[Authorize]` i `[AllowAnonymous]` atrybuty mają żadnego skutku. Jeśli witryna usług IIS (lub serwera HTTP.sys lub WebListener) jest skonfigurowany do nie zezwalaj na dostęp anonimowy, żądanie nigdy nie osiągnie Twojej aplikacji. Z tego powodu `[AllowAnonymous]` atrybut nie ma zastosowania.

### <a name="allow-anonymous-access"></a>Zezwalaj na dostęp anonimowy

Po włączeniu uwierzytelniania Windows i dostęp anonimowy używać `[Authorize]` i `[AllowAnonymous]` atrybutów. `[Authorize]` Atrybut umożliwia zabezpieczenie rodzajów aplikacji, które naprawdę wymagają uwierzytelniania Windows. `[AllowAnonymous]` Atrybutu zastąpienia `[Authorize]` atrybutu do użycia w aplikacjach, które zezwala na dostęp anonimowy. Zobacz [autoryzacja prosta](xref:security/authorization/simple) szczegóły użycia atrybutu.

W programie ASP.NET Core 2.x, `[Authorize]` atrybut wymaga dodatkowej konfiguracji w *Startup.cs* zażąda anonimowe żądania uwierzytelniania Windows. Zalecana konfiguracja zależy od nieco używany serwer sieci web.

> [!NOTE]
> Domyślnie użytkownicy, którzy nie mają autoryzacji do dostępu do strony są prezentowane z pustą odpowiedź HTTP 403. [Oprogramowania pośredniczącego StatusCodePages](xref:fundamentals/error-handling#configure-status-code-pages) można skonfigurować, aby zapewnić użytkownikom lepsze środowisko "Odmowa dostępu".

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

Platforma ASP.NET Core nie implementuje personifikacji. Aplikacje są uruchamiane przy użyciu tożsamości aplikacji dla wszystkich żądań, przy użyciu tożsamości puli lub procesu aplikacji. Jeśli musisz jawnie wykonaj akcję w imieniu użytkownika, należy użyć `WindowsIdentity.RunImpersonated`. Uruchomić jedną akcję w tym kontekście, a następnie zamknij kontekstu.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Należy pamiętać, że `RunImpersonated` nie obsługuje operacji asynchronicznych i nie powinny być używane w przypadku złożonych scenariuszy. Na przykład zawijania całego żądania lub łańcuchów oprogramowanie pośredniczące nie jest obsługiwany lub zalecane.
