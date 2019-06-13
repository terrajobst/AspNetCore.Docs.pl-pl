---
title: Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak skonfigurować uwierzytelnianie Windows w programie ASP.NET Core dla usług IIS i sterownik HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/12/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 93f833adff95f25d570947cd1a9035d652f522c2
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034955"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Konfigurowanie uwierzytelniania Windows w programie ASP.NET Core

Przez [Scott Addie](https://twitter.com/Scott_Addie) i [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

Windows Authentication (nazywanego też uwierzytelnianiem Negotiate, Kerberos lub NTLM) można skonfigurować dla aplikacji platformy ASP.NET Core z [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), lub [HTTP.sys](xref:fundamentals/servers/httpsys) .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Windows Authentication (nazywanego też uwierzytelnianiem Negotiate, Kerberos lub NTLM) można skonfigurować dla aplikacji platformy ASP.NET Core z [IIS](xref:host-and-deploy/iis/index) lub [HTTP.sys](xref:fundamentals/servers/httpsys).

::: moniker-end

Uwierzytelnianie Windows opiera się uwierzytelniać użytkowników aplikacji platformy ASP.NET Core w systemie operacyjnym. Możesz użyć uwierzytelniania Windows, gdy serwer działa w sieci firmowej przy użyciu tożsamości domeny usługi Active Directory lub konta Windows do identyfikacji użytkowników. Uwierzytelnianie Windows najlepiej nadaje się do środowisk intranetowych, w którym użytkownicy, aplikacje klienckie i serwery sieci web należą do tej samej domeny Windows.

> [!NOTE]
> Uwierzytelnianie Windows nie jest obsługiwane przy użyciu protokołu HTTP/2. Mogą być wysyłane wezwań do uwierzytelnienia w odpowiedzi HTTP/2, ale klient musi obniżyć wersję usługi do protokołu HTTP/1.1 przed uwierzytelnieniem.

## <a name="iisiis-express"></a>Usługi IIS/IIS Express

Dodawanie usług uwierzytelniania za pomocą wywołania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> przestrzeni nazw) w `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a>Ustawienia (debuger) uruchamiania

Konfiguracja ustawień uruchamiania ma wpływ tylko na *Properties/launchSettings.json* plików dla usług IIS Express i nie skonfigurować uwierzytelnianie usług IIS dla Windows. Konfiguracja serwera została wyjaśniona w [IIS](#iis) sekcji.

**Aplikacji sieci Web** szablonu dostępnego za pośrednictwem programu Visual Studio lub interfejsu wiersza polecenia platformy .NET Core, można skonfigurować do obsługi uwierzytelniania Windows, która aktualizuje *Properties/launchSettings.json* pliku automatycznie.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

**Nowy projekt**

1. Utwórz nowy projekt.
1. Wybierz **aplikacji sieci Web platformy ASP.NET Core**. Wybierz opcję **Dalej**.
1. Podaj nazwę w **Nazwa projektu** pola. Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu. Wybierz pozycję **Utwórz**.
1. Wybierz **zmiany** w obszarze **uwierzytelniania**.
1. W **Zmień uwierzytelnianie** wybierz **uwierzytelniania Windows**. Kliknij przycisk **OK**.
1. Wybierz **aplikacji sieci Web**.
1. Wybierz pozycję **Utwórz**.

Uruchom aplikację. Nazwa użytkownika pojawia się w interfejsie użytkownika aplikacji renderowany.

**Istniejący projekt**

Właściwości projektu Windows uwierzytelnianie włączyć i wyłączyć uwierzytelnianie anonimowe:

1. Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** i wybierz **właściwości**.
1. Wybierz **debugowania** kartę.
1. Usuń zaznaczenie pola wyboru dla **Włącz uwierzytelnianie anonimowe**.
1. Zaznacz pole wyboru dla **Włącz uwierzytelnianie Windows**.
1. Zapisz i zamknij stronę właściwości.

Alternatywnie, można skonfigurować właściwości w `iisSettings` węźle *launchSettings.json* pliku:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Program Visual Studio Code / .NET Core interfejsu wiersza polecenia](#tab/visual-studio-code+netcore-cli)

**Nowy projekt**

Wykonaj [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia `webapp` argumentu (aplikację sieci Web platformy ASP.NET Core) i `--auth Windows` przełącznika:

```console
dotnet new webapp --auth Windows
```

**Istniejący projekt**

Aktualizacja `iisSettings` węźle *launchSettings.json* pliku:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

Podczas modyfikowania istniejącego projektu, upewnij się, że plik projektu zawiera odwołania do pakietu dla [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **lub** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) pakietu NuGet.

### <a name="iis"></a>IIS

Usługi IIS używają [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) hostująca aplikacje platformy ASP.NET Core. Uwierzytelnianie Windows jest skonfigurowany dla usług IIS za pomocą *web.config* pliku. Następujące sekcje show jak:

* Zapewnia lokalny *web.config* pliku, który aktywuje uwierzytelniania Windows na serwerze, gdy aplikacja jest wdrożona.
* Konfigurowanie za pomocą Menedżera usług IIS *web.config* pliku aplikacji platformy ASP.NET Core, która została już wdrożona na serwerze.

Jeśli jeszcze tego nie zrobiono, Włącz usługi IIS do hostowania aplikacji platformy ASP.NET Core. Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/iis/index>.

Włączyć usługi roli usług IIS na potrzeby uwierzytelniania Windows. Aby uzyskać więcej informacji, zobacz [Włącz uwierzytelnianie Windows usługami roli usług IIS (zobacz krok 2)](xref:host-and-deploy/iis/index#iis-configuration).

[Oprogramowania pośredniczącego integracji usługi IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) jest domyślnie skonfigurowana do automatycznego uwierzytelniania żądań. Aby uzyskać więcej informacji, zobacz [hosta ASP.NET Core na Windows za pomocą programu IIS: Opcje usług IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

Modułu ASP.NET Core jest domyślnie skonfigurowana do przekazywania tokenu uwierzytelniania Windows do aplikacji. Aby uzyskać więcej informacji, zobacz [informacje o konfiguracji modułu ASP.NET Core: Atrybuty elementu aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

Użyj **albo** z następujących metod:

* **Przed opublikowaniem i wdrażania projektu,** Dodaj następujący kod *web.config* plik do katalogu głównego projektu:

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  Gdy projekt zostanie opublikowany przez zestaw SDK platformy .NET Core (bez `<IsTransformWebConfigDisabled>` właściwością `true` w pliku projektu), opublikowanego *web.config* plik zawiera `<location><system.webServer><security><authentication>` sekcji. Aby uzyskać więcej informacji na temat `<IsTransformWebConfigDisabled>` właściwości, zobacz <xref:host-and-deploy/iis/index#webconfig-file>.

* **Po publikowania i wdrażania projektu,** przeprowadzenia konfiguracji po stronie serwera za pomocą Menedżera usług IIS:

  1. W Menedżerze usług IIS wybierz witrynę IIS, w obszarze **witryn** węźle **połączeń** pasku bocznym.
  1. Kliknij dwukrotnie **uwierzytelniania** w **IIS** obszaru.
  1. Wybierz **uwierzytelnianie anonimowe**. Wybierz **wyłączyć** w **akcje** pasku bocznym.
  1. Wybierz **uwierzytelniania Windows**. Wybierz **Włącz** w **akcje** pasku bocznym.

  Kiedy te akcje są wykonywane, Menedżera usług IIS modyfikuje aplikacji *web.config* pliku. A `<system.webServer><security><authentication>` węzeł zostanie dodany ze zaktualizowanymi ustawieniami dla `anonymousAuthentication` i `windowsAuthentication`:

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  `<system.webServer>` Dodany do sekcji *web.config* pliku przez Menedżera usług IIS znajduje się poza jej `<location>` sekcji dodawane przez program .NET Core SDK po opublikowaniu aplikacji. Ponieważ sekcji zostanie dodany poza `<location>` węzła, ustawienia są dziedziczone przez żaden [aplikacji podrzędnych](xref:host-and-deploy/iis/index#sub-applications) do bieżącej aplikacji. Aby zapobiec dziedziczenia, Przenieś dodany `<security>` sekcji wewnątrz `<location><system.webServer>` sekcji podanym .NET Core SDK.

  W przypadku Menedżera usług IIS można dodać konfiguracji usług IIS dotyczy tylko aplikacji *web.config* pliku na serwerze. Kolejne wdrożenie aplikacji może zastąpić ustawienia na serwerze, jeśli kopię serwera *web.config* zastępuje projektu *web.config* pliku. Użyj **albo** z następujących metod do zarządzania ustawieniami:

  * Użyj Menedżera usług IIS, aby zresetować ustawienia w *web.config* pliku po plik jest zastępowany we wdrożeniu.
  * Dodaj *pliku web.config* aplikacji lokalnie przy użyciu ustawień.

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a>Kestrel

 [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) pakietu NuGet może być używany z [Kestrel](xref:fundamentals/servers/kestrel) do obsługi uwierzytelniania Windows przy użyciu Negotiate, Kerberos i NTLM w Windows, Linux i macOS.

> [!WARNING]
> Poświadczenia mogą zostać utrwalone na żądań połączenia. *Negocjowania uwierzytelniania nie może być używany z serwerami proxy, chyba że serwer proxy przechowuje koligacji połączenia 1:1 (trwałe połączenie), za pomocą Kestrel.* Oznacza to, że uwierzytelniania Negotiate nie mogą być używane z Kestrel za IIS [poza procesem programu ASP.NET Core modułu (ANCM)](xref:host-and-deploy/iis/index#out-of-process-hosting-model).

 Dodawanie usług uwierzytelniania za pomocą wywołania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` przestrzeni nazw) i `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` przestrzeni nazw) w `Startup.ConfigureServices`:

 ```csharp
services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

Dodaj oprogramowanie pośredniczące uwierzytelniania przez wywołanie metody <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> w `Startup.Configure`:

 ```csharp
app.UseAuthentication();

app.UseMvc();
```

Aby uzyskać więcej informacji na temat oprogramowania pośredniczącego, zobacz <xref:fundamentals/middleware/index>.

Anonimowe żądania są dozwolone. Użyj [autoryzacja w programie ASP.NET Core](xref:security/authorization/introduction) zażąda anonimowe żądania uwierzytelniania.

### <a name="windows-environment-configuration"></a>Konfiguracja środowiska Windows

[Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) składnik przeprowadza uwierzytelnianie w trybie użytkownika. Główne nazwy usług (SPN), należy dodać do konta użytkownika uruchamiającego usługi, a nie na koncie komputera. Wykonaj `setspn -S HTTP/mysrevername.mydomain.com myuser` w powłoce poleceń administracyjnych.

### <a name="linux-and-macos-environment-configuration"></a>Konfiguracja środowiska systemu Linux i macOS

Instrukcje dotyczące przyłączania komputera z systemem Linux lub macOS do domeny Windows są dostępne w [Połącz Studio danych platformy Azure z serwerem SQL przy użyciu uwierzytelniania Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) artykułu. Instrukcje Utwórz konto komputera dla maszyny z systemem Linux w domenie. Nazwy SPN należy dodać do tego konta maszyny.

> [!NOTE]
> Gdy postępując zgodnie ze wskazówkami w [Połącz Studio danych platformy Azure z serwerem SQL przy użyciu uwierzytelniania Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) artykuł, Zastąp `python-software-properties` z `python3-software-properties` w razie potrzeby.

Po w systemie Linux lub macOS komputer jest przyłączony do domeny, są wymagane dodatkowe kroki w celu zapewnienia [pliku keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) za pomocą nazw SPN:

* Na kontrolerze domeny Dodaj nową usługę sieci web nazwy SPN konta na komputerze:
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* Użyj [ktpass](/windows-server/administration/windows-commands/ktpass) do generowania pliku keytab:
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * Niektóre pola musi być określona w wielkie wskazane.
* Skopiuj plik keytab do komputera w systemie Linux lub macOS.
* Wybierz plik keytab za pośrednictwem zmiennej środowiskowej: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`
* Wywoływanie `klist` aby pokazać nazwy SPN aktualnie dostępne do użycia.

> [!NOTE]
> Plik keytab zawiera poświadczenia dostępu do domeny i muszą być odpowiednio chronione.

::: moniker-end

## <a name="httpsys"></a>HTTP.sys

[Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) obsługuje Windows uwierzytelnianie trybu jądra, przy użyciu Negotiate, NTLM lub uwierzytelniania podstawowego.

Dodawanie usług uwierzytelniania za pomocą wywołania <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> przestrzeni nazw) w `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

Konfigurowanie aplikacji sieci web hosta HTTP.sys za pomocą uwierzytelniania Windows (*Program.cs*). <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> Trwa <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> przestrzeni nazw.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> Sterownik HTTP.sys delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos. Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i sterownik HTTP.sys. Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika. Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.

> [!NOTE]
> Sterownik HTTP.sys nie jest obsługiwana na serwerze Nano Server w wersji 1709 lub nowszej. Aby użyć uwierzytelniania Windows i sterownik HTTP.sys systemu Nano Server, użyj [Server Core (microsoft/windowsservercore) kontenera](https://hub.docker.com/r/microsoft/windowsservercore/). Aby uzyskać więcej informacji na temat Server Core, zobacz [co to jest opcja instalacji Server Core w systemie Windows Server?](/windows-server/administration/server-core/what-is-server-core).

## <a name="authorize-users"></a>Autoryzowanie użytkowników

Stan konfiguracji dostępu anonimowego określa sposób, w którym `[Authorize]` i `[AllowAnonymous]` atrybuty są używane w aplikacji. W poniższych sekcjach dwóch opisano sposób obsługi konfiguracji niedozwolonych i dozwolone stany dostęp anonimowy.

### <a name="disallow-anonymous-access"></a>Nie zezwalaj na dostęp anonimowy

Gdy jest włączone uwierzytelnianie Windows i dostęp anonimowy jest wyłączony, `[Authorize]` i `[AllowAnonymous]` atrybuty mają żadnego skutku. Jeśli witryna usług IIS jest skonfigurowany do nie zezwalaj na dostęp anonimowy, żądanie nigdy nie osiągnie aplikacji. Z tego powodu `[AllowAnonymous]` atrybut nie ma zastosowania.

### <a name="allow-anonymous-access"></a>Zezwalaj na dostęp anonimowy

Po włączeniu uwierzytelniania Windows i dostęp anonimowy używać `[Authorize]` i `[AllowAnonymous]` atrybutów. `[Authorize]` Atrybut umożliwia bezpieczne punkty końcowe aplikacji, które wymagają uwierzytelniania. `[AllowAnonymous]` Atrybutu zastąpienia `[Authorize]` atrybutów w aplikacjach, które zezwala na dostęp anonimowy. Atrybut szczegóły użycia można znaleźć <xref:security/authorization/simple>.

> [!NOTE]
> Domyślnie użytkownicy, którzy nie mają autoryzacji do dostępu do strony są prezentowane z pustą odpowiedź HTTP 403. [Oprogramowania pośredniczącego StatusCodePages](xref:fundamentals/error-handling#usestatuscodepages) można skonfigurować, aby zapewnić użytkownikom lepsze środowisko "Odmowa dostępu".

## <a name="impersonation"></a>Personifikacja

Platforma ASP.NET Core nie implementuje personifikacji. Aplikacje są uruchamiane przy użyciu tożsamości przez aplikację dla wszystkich żądań, przy użyciu tożsamości puli lub procesu aplikacji. Jeśli aplikacja powinna wykonać akcję w imieniu użytkownika, należy użyć [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) w [oprogramowania pośredniczącego terminalu wbudowane](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) w `Startup.Configure`. Uruchomić jedną akcję w tym kontekście, a następnie zamknij kontekstu.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` nie obsługuje operacji asynchronicznych i nie powinny być używane w przypadku złożonych scenariuszy. Na przykład zawijania całego żądania lub łańcuchów oprogramowanie pośredniczące nie jest obsługiwany lub zalecane.

::: moniker range=">= aspnetcore-3.0"

Gdy [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) pakiet umożliwia uwierzytelnianie na Windows, Linux i macOS, personifikacja jest obsługiwana tylko na Windows.

::: moniker-end

## <a name="claims-transformations"></a>Przekształcenia oświadczeń

W przypadku hostowania w trybie usług IIS w trakcie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wewnętrznie wywoływana w celu zainicjowania przez użytkownika. W związku z tym <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacji używanego do przekształcania oświadczeń, po każdym uwierzytelniania nie jest aktywowana domyślnie. Aby uzyskać więcej informacji i przykładowy kod, który aktywuje przekształcenia oświadczeń w przypadku hostowania w procesie, zobacz <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
