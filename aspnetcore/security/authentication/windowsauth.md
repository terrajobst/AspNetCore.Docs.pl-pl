---
title: Skonfiguruj uwierzytelnianie systemu Windows w ASP.NET Core
author: ardalis
description: "W tym artykule opisano sposób konfigurowania uwierzytelniania systemu Windows w ASP.NET, za pomocą usług IIS Express, usługi IIS, sterownik HTTP.sys i WebListener Core."
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: c229537e7f533eea2173dbc51b8d0d0e097d434a
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a>Skonfiguruj uwierzytelnianie systemu Windows w aplikacji platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com) i [Scott Addie](https://twitter.com/Scott_Addie)

Uwierzytelnianie systemu Windows dla aplikacji platformy ASP.NET Core hostowanych w przypadku usług IIS można skonfigurować [HTTP.sys](xref:fundamentals/servers/httpsys), lub [WebListener](xref:fundamentals/servers/weblistener).

## <a name="what-is-windows-authentication"></a>Co to jest uwierzytelnianie systemu Windows?

Uwierzytelnianie systemu Windows, zależy od systemu operacyjnego, aby uwierzytelniać użytkowników aplikacji platformy ASP.NET Core. Można użyć uwierzytelniania systemu Windows, gdy serwer działa w sieci firmowej przy użyciu tożsamości domeny usługi Active Directory lub inne konta systemu Windows do identyfikowania użytkowników. Uwierzytelnianie systemu Windows jest najodpowiedniejsze do środowisk intranetowych, w których użytkownicy, aplikacje klienckie i serwery sieci web należą do tej samej domeny systemu Windows.

[Więcej informacji na temat uwierzytelniania systemu Windows i instalując je dla usług IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Włącz uwierzytelnianie systemu Windows w aplikacji platformy ASP.NET Core

Szablonu aplikacji sieci Web w usłudze Visual Studio może być skonfigurowana do obsługi uwierzytelniania systemu Windows.

### <a name="use-the-windows-authentication-app-template"></a>Szablon aplikacji uwierzytelniania systemu Windows

W programie Visual Studio:
1. Utwórz nową aplikację sieci Web platformy ASP.NET Core. 
1. Wybierz aplikację sieci Web z listy szablonów.
1. Wybierz **Zmień uwierzytelnianie** i wybrać **uwierzytelniania systemu Windows**. 

Uruchom aplikację. Nazwa użytkownika jest wyświetlany w górnym rogu aplikacji.

![Zrzut ekranu przeglądarki uwierzytelniania systemu Windows](windowsauth/_static/browser-screenshot.png)

W przypadku projektach przy użyciu usług IIS Express szablonu zapewnia konfiguracji koniecznych do używania uwierzytelniania systemu Windows. Poniższej sekcji pokazano, jak ręcznie skonfigurować aplikację ASP.NET Core dla uwierzytelniania systemu Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Ustawienia programu Visual Studio dla systemu Windows i uwierzytelnianie anonimowe

Projekt programu Visual Studio **właściwości** strony **debugowania** karta zawiera pola wyboru dla uwierzytelniania anonimowego i uwierzytelniania systemu Windows.

![Zrzut ekranu przeglądarki uwierzytelniania systemu Windows](windowsauth/_static/vs-auth-property-menu.png)

Alternatywnie można skonfigurować te dwie właściwości w *launchSettings.json* pliku:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Włącz uwierzytelnianie systemu Windows z programem IIS

Usługi IIS używają [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) (ANCM) na potrzeby hostowania aplikacji platformy ASP.NET Core. ANCM przepływów uwierzytelniania systemu Windows do usług IIS domyślnie. Konfiguracja uwierzytelniania systemu Windows jest wykonywana w ramach usług IIS, a nie projekt aplikacji. Poniższe sekcje pokazują, jak skonfigurować aplikację ASP.NET Core uwierzytelniania systemu Windows za pomocą Menedżera usług IIS.

### <a name="create-a-new-iis-site"></a>Utwórz nową witrynę IIS

Określ nazwę i folder i zezwolenie na tworzenie nowej puli aplikacji.

### <a name="customize-authentication"></a>Dostosowywanie uwierzytelniania

Otwórz menu uwierzytelniania dla tej lokacji.

![Menu uwierzytelniania usług IIS](windowsauth/_static/iis-authentication-menu.png)

Należy wyłączyć uwierzytelnianie anonimowe, a następnie włączyć uwierzytelnianie systemu Windows.

![Ustawienia uwierzytelniania usług IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Publikowanie projektu do folderu witryny usług IIS

Przy użyciu programu Visual Studio lub .NET Core interfejsu wiersza polecenia, Opublikuj aplikację do folderu docelowego.

![Okno dialogowe publikowanie programu Visual Studio](windowsauth/_static/vs-publish-app.png)

Dowiedz się więcej o [publikowania w usługach IIS](xref:host-and-deploy/iis/index).

Uruchom aplikację w celu zweryfikowania, czy działa uwierzytelnianie systemu Windows.

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a>Włącz uwierzytelnianie systemu Windows z pliku HTTP.sys lub WebListener

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Chociaż Kestrel nie obsługuje uwierzytelniania systemu Windows, można użyć [HTTP.sys](xref:fundamentals/servers/httpsys) na potrzeby obsługi scenariuszy własnym hostowanej w systemie Windows. Poniższy przykład umożliwia skonfigurowanie aplikacji sieci web hosta do pliku HTTP.sys za pomocą uwierzytelniania systemu Windows:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Chociaż Kestrel nie obsługuje uwierzytelniania systemu Windows, można użyć [WebListener](xref:fundamentals/servers/weblistener) na potrzeby obsługi scenariuszy własnym hostowanej w systemie Windows. Poniższy przykład umożliwia skonfigurowanie aplikacji sieci web hosta do WebListener za pomocą uwierzytelniania systemu Windows:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a>Praca z uwierzytelnianiem systemu Windows

Stan konfiguracji dostęp anonimowy Określa sposób, w którym `[Authorize]` i `[AllowAnonymous]` atrybuty są używane w aplikacji. W poniższych sekcjach dwóch opisano sposób obsługi konfiguracji niedozwolonych i dozwolone stany dostęp anonimowy.

### <a name="disallow-anonymous-access"></a>Nie zezwalaj na dostęp anonimowy

Jeśli jest włączone uwierzytelnianie systemu Windows i dostęp anonimowy jest wyłączony, `[Authorize]` i `[AllowAnonymous]` atrybutów nie obowiązują. Jeśli witryna usług IIS (lub serwera HTTP.sys lub WebListener) jest skonfigurowany do nie zezwalaj na dostęp anonimowy, żądania nigdy nie osiągnie aplikacji. Z tego powodu `[AllowAnonymous]` atrybut nie jest stosowana.

### <a name="allow-anonymous-access"></a>Zezwalaj na dostęp anonimowy

Gdy zarówno uwierzytelnianie systemu Windows i dostęp anonimowy jest włączona, należy użyć `[Authorize]` i `[AllowAnonymous]` atrybutów. `[Authorize]` Atrybutu umożliwia zabezpieczenie częściach aplikacji, które są rzeczywiście wymagają uwierzytelniania systemu Windows. `[AllowAnonymous]` Atrybutu zastąpienia `[Authorize]` atrybutu użycia w aplikacjach, które zezwala na dostęp anonimowy. Zobacz [proste autoryzacji](xref:security/authorization/simple) szczegóły użycia atrybutu.

W przypadku platformy ASP.NET Core 2.x, `[Authorize]` atrybut wymaga dodatkowych czynności konfiguracyjnych *Startup.cs* zażąda anonimowe żądania uwierzytelniania systemu Windows. Zalecana konfiguracja zależy od nieco używany serwer sieci web.

> [!NOTE]
> Domyślnie użytkownicy, którzy nie mają zezwolenia na dostęp do strony są prezentowane pustego dokumentu. [Oprogramowanie pośredniczące StatusCodePages](xref:fundamentals/error-handling#configuring-status-code-pages) można skonfigurować, aby zapewnić użytkownikom lepsze środowisko "Odmowa dostępu".

#### <a name="iis"></a>IIS

Jeśli za pomocą usług IIS, Dodaj następujący kod do `ConfigureServices` metody: 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Jeśli z pliku HTTP.sys, Dodaj następujący kod do `ConfigureServices` metody:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Personifikacja

Nie zawiera implementacji personifikacji platformy ASP.NET Core. Aplikacje są uruchamiane z tożsamości aplikacji dla wszystkich żądań przy użyciu tożsamości puli lub procesu aplikacji. Jeśli musisz jawnie wykonania akcji w imieniu użytkownika, użyj `WindowsIdentity.RunImpersonated`. Uruchom jednego działania w tym kontekście, a następnie zamknij kontekstu.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Należy pamiętać, że `RunImpersonated` nie obsługuje operacji asynchronicznych i nie powinny być używane do złożonych scenariuszy. Na przykład zawijania całego żądania lub łańcuchów oprogramowanie pośredniczące nie jest obsługiwany lub zalecane.

---
