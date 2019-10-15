---
title: Implementacja serwera sieci Web HTTP. sys w ASP.NET Core
author: guardrex
description: Informacje o pliku HTTP. sys, serwerze sieci Web dla ASP.NET Core w systemie Windows. W oparciu o sterownik trybu jądra HTTP. sys, HTTP. sys jest alternatywą dla Kestrel, która może być używana do bezpośredniego połączenia z Internetem bez usług IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f9e564119604e13bdc48a6c36de7d283c56f68f0
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333816"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementacja serwera sieci Web HTTP. sys w ASP.NET Core

Autorzy [Dykstra](https://github.com/tdykstra), [Krzysztof Ross](https://github.com/Tratcher)i [Luke Latham](https://github.com/guardrex)

[Http. sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) to [serwer sieci Web dla ASP.NET Core](xref:fundamentals/servers/index) , który działa tylko w systemie Windows. HTTP. sys jest alternatywą dla [Kestrel](xref:fundamentals/servers/kestrel) Server i oferuje pewne funkcje, które nie są Kestrel.

> [!IMPORTANT]
> Protokół HTTP. sys nie jest zgodny z [modułem ASP.NET Core](xref:host-and-deploy/aspnet-core-module) i nie można go używać z usługami IIS ani IIS Express.

W przypadku protokołu HTTP. sys obsługiwane są następujące funkcje:

* [Uwierzytelnianie systemu Windows](xref:security/authentication/windowsauth)
* Udostępnianie portów
* HTTPS z SNI
* HTTP/2 za pośrednictwem protokołu TLS (system Windows 10 lub nowszy)
* Bezpośrednia transmisja plików
* Buforowanie odpowiedzi
* Obiekty WebSockets (system Windows 8 lub nowszy)

Obsługiwane wersje systemu Windows:

* System Windows 7 lub nowszy
* System Windows Server 2008 R2 lub nowszy

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Kiedy używać protokołu HTTP. sys

Metoda HTTP. sys jest przydatna w przypadku wdrożeń, w których:

* Istnieje potrzeba bezpośredniego udostępnienia serwera w Internecie bez korzystania z usług IIS.

  ![Protokół HTTP. sys komunikuje się bezpośrednio z Internetem](httpsys/_static/httpsys-to-internet.png)

* Wdrożenie wewnętrzne wymaga, aby funkcja była niedostępna w Kestrel, taka jak [uwierzytelnianie systemu Windows](xref:security/authentication/windowsauth).

  ![Protokół HTTP. sys komunikuje się bezpośrednio z siecią wewnętrzną](httpsys/_static/httpsys-to-internal.png)

HTTP. sys jest doskonałym technologią chroniącą przed wieloma typami ataków i zapewnia niezawodność, bezpieczeństwo i skalowalność w pełni funkcjonalny serwer sieci Web. Usługi IIS działają jako odbiornik HTTP na serwerze HTTP. sys.

## <a name="http2-support"></a>Obsługa protokołu HTTP/2

[Protokół HTTP/2](https://httpwg.org/specs/rfc7540.html) jest włączony dla aplikacji ASP.NET Core, jeśli spełnione są następujące wymagania podstawowe:

* Windows Server 2016/Windows 10 lub nowszy
* Połączenie [negocjowania protokołu warstwy aplikacji (ClientHello alpn)](https://tools.ietf.org/html/rfc7301#section-3)
* Połączenie TLS 1,2 lub nowsze

::: moniker range=">= aspnetcore-2.2"

W przypadku nawiązania połączenia HTTP/2 raporty [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) `HTTP/2`.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

W przypadku nawiązania połączenia HTTP/2 raporty [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) `HTTP/1.1`.

::: moniker-end

Protokół HTTP/2 jest domyślnie włączony. Jeśli połączenie HTTP/2 nie zostało ustanowione, połączenie powraca do protokołu HTTP/1.1. W przyszłych wydaniach systemu Windows są dostępne flagi konfiguracji protokołu HTTP/2, w tym możliwość wyłączenia protokołu HTTP/2 przy użyciu protokołu HTTP. sys.

## <a name="kernel-mode-authentication-with-kerberos"></a>Uwierzytelnianie w trybie jądra przy użyciu protokołu Kerberos

Serwer HTTP. sys deleguje do uwierzytelniania w trybie jądra przy użyciu protokołu uwierzytelniania Kerberos. Uwierzytelnianie w trybie użytkownika nie jest obsługiwane w przypadku protokołów Kerberos i HTTP. sys. Konto komputera musi służyć do odszyfrowywania tokenu lub biletu Kerberos uzyskanych z Active Directory i przesłanych przez klienta na serwer w celu uwierzytelnienia użytkownika. Zarejestruj główną nazwę usługi (SPN) dla hosta, a nie użytkownika aplikacji.

## <a name="how-to-use-httpsys"></a>Jak używać protokołu HTTP. sys

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>Skonfiguruj aplikację ASP.NET Core do korzystania z protokołu HTTP. sys

::: moniker range="< aspnetcore-3.0"

Odwołanie do pakietu w pliku projektu nie jest wymagane w przypadku korzystania z [pakietu Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) ([NuGet.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)). Gdy nie korzystasz z pakietu "`Microsoft.AspNetCore.App`", Dodaj odwołanie do pakietu do [Microsoft. AspNetCore. Server. HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).

::: moniker-end

Wywołaj metodę rozszerzenia <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> podczas kompilowania hosta, określając wszystkie wymagane <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>. Poniższy przykład ustawia wartości domyślne dla opcji:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](httpsys/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](httpsys/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=4-12)]

::: moniker-end

Dodatkowa konfiguracja protokołu HTTP. sys jest obsługiwana za pośrednictwem [ustawień rejestru](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).

   **Opcje HTTP. sys**

::: moniker range=">= aspnetcore-3.1"

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [AllowSynchronousIO](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | Określ, czy synchroniczna operacja wejścia/wyjścia jest dozwolona dla `HttpContext.Request.Body` i `HttpContext.Response.Body`. | `false` |
| [Authentication. AllowAnonymous](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | Zezwalaj na żądania anonimowe. | `true` |
| [Uwierzytelnianie. schematy](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | Określ dozwolone schematy uwierzytelniania. Można je zmodyfikować w dowolnym momencie przed wyjęciem odbiornika. Wartości są dostarczane przez [Wyliczenie AuthenticationSchemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None` i `NTLM`. | `None` |
| [EnableResponseCaching](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | Próba buforowania [trybu jądra](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) dla odpowiedzi z uprawnionymi nagłówkami. Odpowiedź nie może zawierać nagłówków `Set-Cookie`, `Vary` lub `Pragma`. Musi zawierać `Cache-Control` nagłówka, który jest `public` i `shared-max-age` lub `max-age` wartość lub nagłówek `Expires`. | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | Maksymalna liczba współbieżnych akceptacji. | 5 &times; [środowisko. <br>ProcessorCount](xref:System.Environment.ProcessorCount) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | Maksymalna liczba jednoczesnych połączeń do zaakceptowania. Użyj `-1` do nieskończoności. Użyj `null`, aby użyć ustawienia dla całego komputera w rejestrze. | `null`<br>(cały komputer<br>konfigurowania |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | Zobacz sekcję <a href="#maxrequestbodysize">MaxRequestBodySize</a> . | 30000000 bajtów<br>(~ 28,6 MB) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | Maksymalna liczba żądań, które można umieścić w kolejce. | 1000 |
| `RequestQueueMode` | Wskazuje, czy serwer jest odpowiedzialny za tworzenie i Konfigurowanie kolejki żądań, czy też ma zostać dołączony do istniejącej kolejki.<br>W przypadku dołączania do istniejącej kolejki nie mają zastosowania większość istniejących opcji konfiguracji. | `RequestQueueMode.Create` |
| `RequestQueueName` | Nazwa kolejki żądań HTTP. sys. | `null` (Kolejka anonimowa) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | Wskaż, czy zapisy treści odpowiedzi nie powiodą się, ponieważ rozłączenia klienta nie powiedzie się, jeśli wyjątki lub są normalnie kompletne. | `false`<br>(normalne zakończenie) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | Uwidocznij konfigurację pliku HTTP. sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager>, która może być również skonfigurowana w rejestrze. Postępuj zgodnie z linkami interfejsu API, aby dowiedzieć się więcej na temat każdego ustawienia, w tym wartości domyślnych:<ul><li>[TimeoutManager. DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; czas dozwolony dla interfejsu API serwera HTTP do opróżniania treści jednostki w ramach połączenia Keep-Alive.</li><li>[TimeoutManager. EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; czas dozwolony dla treści jednostki żądania.</li><li>[TimeoutManager. HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; czas dozwolony dla interfejsu API serwera http, aby przeanalizować nagłówek żądania.</li><li>[TimeoutManager. IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; czas dozwolony dla połączenia bezczynnego.</li><li>[TimeoutManager. MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; Minimalna szybkość wysyłania odpowiedzi.</li><li>[TimeoutManager. RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; czas, aby żądanie pozostało w kolejce żądań przed jego usunięciem.</li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | Określ <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection>, aby zarejestrować się w pliku HTTP. sys. Najbardziej przydatne jest [UrlPrefixCollection. Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), który służy do dodawania prefiksu do kolekcji. Można je zmodyfikować w dowolnym momencie przed wyjęciem odbiornika. |  |

::: moniker-end

::: moniker range="= aspnetcore-3.0"

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [AllowSynchronousIO](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | Określ, czy synchroniczna operacja wejścia/wyjścia jest dozwolona dla `HttpContext.Request.Body` i `HttpContext.Response.Body`. | `false` |
| [Authentication. AllowAnonymous](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | Zezwalaj na żądania anonimowe. | `true` |
| [Uwierzytelnianie. schematy](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | Określ dozwolone schematy uwierzytelniania. Można je zmodyfikować w dowolnym momencie przed wyjęciem odbiornika. Wartości są dostarczane przez [Wyliczenie AuthenticationSchemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None` i `NTLM`. | `None` |
| [EnableResponseCaching](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | Próba buforowania [trybu jądra](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) dla odpowiedzi z uprawnionymi nagłówkami. Odpowiedź nie może zawierać nagłówków `Set-Cookie`, `Vary` lub `Pragma`. Musi zawierać `Cache-Control` nagłówka, który jest `public` i `shared-max-age` lub `max-age` wartość lub nagłówek `Expires`. | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | Maksymalna liczba współbieżnych akceptacji. | 5 &times; [środowisko. <br>ProcessorCount](xref:System.Environment.ProcessorCount) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | Maksymalna liczba jednoczesnych połączeń do zaakceptowania. Użyj `-1` do nieskończoności. Użyj `null`, aby użyć ustawienia dla całego komputera w rejestrze. | `null`<br>(cały komputer<br>konfigurowania |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | Zobacz sekcję <a href="#maxrequestbodysize">MaxRequestBodySize</a> . | 30000000 bajtów<br>(~ 28,6 MB) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | Maksymalna liczba żądań, które można umieścić w kolejce. | 1000 |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | Wskaż, czy zapisy treści odpowiedzi nie powiodą się, ponieważ rozłączenia klienta nie powiedzie się, jeśli wyjątki lub są normalnie kompletne. | `false`<br>(normalne zakończenie) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | Uwidocznij konfigurację pliku HTTP. sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager>, która może być również skonfigurowana w rejestrze. Postępuj zgodnie z linkami interfejsu API, aby dowiedzieć się więcej na temat każdego ustawienia, w tym wartości domyślnych:<ul><li>[TimeoutManager. DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; czas dozwolony dla interfejsu API serwera HTTP do opróżniania treści jednostki w ramach połączenia Keep-Alive.</li><li>[TimeoutManager. EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; czas dozwolony dla treści jednostki żądania.</li><li>[TimeoutManager. HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; czas dozwolony dla interfejsu API serwera http, aby przeanalizować nagłówek żądania.</li><li>[TimeoutManager. IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; czas dozwolony dla połączenia bezczynnego.</li><li>[TimeoutManager. MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; Minimalna szybkość wysyłania odpowiedzi.</li><li>[TimeoutManager. RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; czas, aby żądanie pozostało w kolejce żądań przed jego usunięciem.</li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | Określ <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection>, aby zarejestrować się w pliku HTTP. sys. Najbardziej przydatne jest [UrlPrefixCollection. Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), który służy do dodawania prefiksu do kolekcji. Można je zmodyfikować w dowolnym momencie przed wyjęciem odbiornika. |  |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| Właściwość | Opis | Domyślny |
| -------- | ----------- | :-----: |
| [AllowSynchronousIO](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | Określ, czy synchroniczna operacja wejścia/wyjścia jest dozwolona dla `HttpContext.Request.Body` i `HttpContext.Response.Body`. | `true` |
| [Authentication. AllowAnonymous](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | Zezwalaj na żądania anonimowe. | `true` |
| [Uwierzytelnianie. schematy](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | Określ dozwolone schematy uwierzytelniania. Można je zmodyfikować w dowolnym momencie przed wyjęciem odbiornika. Wartości są dostarczane przez [Wyliczenie AuthenticationSchemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None` i `NTLM`. | `None` |
| [EnableResponseCaching](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | Próba buforowania [trybu jądra](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) dla odpowiedzi z uprawnionymi nagłówkami. Odpowiedź nie może zawierać nagłówków `Set-Cookie`, `Vary` lub `Pragma`. Musi zawierać `Cache-Control` nagłówka, który jest `public` i `shared-max-age` lub `max-age` wartość lub nagłówek `Expires`. | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | Maksymalna liczba współbieżnych akceptacji. | 5 &times; [środowisko. <br>ProcessorCount](xref:System.Environment.ProcessorCount) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | Maksymalna liczba jednoczesnych połączeń do zaakceptowania. Użyj `-1` do nieskończoności. Użyj `null`, aby użyć ustawienia dla całego komputera w rejestrze. | `null`<br>(cały komputer<br>konfigurowania |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | Zobacz sekcję <a href="#maxrequestbodysize">MaxRequestBodySize</a> . | 30000000 bajtów<br>(~ 28,6 MB) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | Maksymalna liczba żądań, które można umieścić w kolejce. | 1000 |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | Wskaż, czy zapisy treści odpowiedzi nie powiodą się, ponieważ rozłączenia klienta nie powiedzie się, jeśli wyjątki lub są normalnie kompletne. | `false`<br>(normalne zakończenie) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | Uwidocznij konfigurację pliku HTTP. sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager>, która może być również skonfigurowana w rejestrze. Postępuj zgodnie z linkami interfejsu API, aby dowiedzieć się więcej na temat każdego ustawienia, w tym wartości domyślnych:<ul><li>[TimeoutManager. DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; czas dozwolony dla interfejsu API serwera HTTP do opróżniania treści jednostki w ramach połączenia Keep-Alive.</li><li>[TimeoutManager. EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; czas dozwolony dla treści jednostki żądania.</li><li>[TimeoutManager. HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; czas dozwolony dla interfejsu API serwera http, aby przeanalizować nagłówek żądania.</li><li>[TimeoutManager. IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; czas dozwolony dla połączenia bezczynnego.</li><li>[TimeoutManager. MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; Minimalna szybkość wysyłania odpowiedzi.</li><li>[TimeoutManager. RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; czas, aby żądanie pozostało w kolejce żądań przed jego usunięciem.</li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | Określ <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection>, aby zarejestrować się w pliku HTTP. sys. Najbardziej przydatne jest [UrlPrefixCollection. Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), który służy do dodawania prefiksu do kolekcji. Można je zmodyfikować w dowolnym momencie przed wyjęciem odbiornika. |  |

::: moniker-end

<a name="maxrequestbodysize"></a>

**MaxRequestBodySize**

Maksymalny dozwolony rozmiar dowolnej treści żądania w bajtach. W przypadku ustawienia wartości `null` maksymalny rozmiar treści żądania jest nieograniczony. Ten limit nie ma wpływu na uaktualnione połączenia, które są zawsze nieograniczone.

Zalecaną metodą przesłonięcia limitu w aplikacji ASP.NET Core MVC dla pojedynczego `IActionResult` jest użycie atrybutu <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> na metodzie akcji:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Wyjątek jest zgłaszany, jeśli aplikacja próbuje skonfigurować limit żądania po rozpoczęciu odczytywania żądania przez aplikację. Właściwość `IsReadOnly` może służyć do wskazywania, czy właściwość `MaxRequestBodySize` jest w stanie tylko do odczytu, co oznacza, że jest zbyt późno, aby skonfigurować limit.

Jeśli aplikacja powinna zastąpić <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> na żądanie, użyj <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](httpsys/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](httpsys/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

::: moniker-end

Jeśli używasz programu Visual Studio, upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS ani IIS Express.

W programie Visual Studio domyślny profil uruchamiania jest przeznaczony dla IIS Express. Aby uruchomić projekt jako aplikację konsolową, należy ręcznie zmienić wybrany profil, jak pokazano na poniższym zrzucie ekranu:

![Wybierz profil aplikacji konsoli](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Konfigurowanie systemu Windows Server

1. Określ porty do otwarcia dla aplikacji i Użyj [zapory systemu Windows](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) lub polecenia cmdlet [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) programu PowerShell, aby otworzyć porty zapory w celu zezwolenia na ruch do pliku http. sys. W poniższych poleceniach i konfiguracji aplikacji jest używany port 443.

1. Podczas wdrażania na maszynie wirtualnej platformy Azure Otwórz porty w [sieciowej grupie zabezpieczeń](/azure/virtual-machines/windows/nsg-quickstart-portal). W poniższych poleceniach i konfiguracji aplikacji jest używany port 443.

1. Uzyskaj i zainstaluj certyfikaty X. 509, jeśli jest to wymagane.

   W systemie Windows utwórz certyfikaty z podpisem własnym za pomocą [polecenia cmdlet New-SelfSignedCertificate programu PowerShell](/powershell/module/pkiclient/new-selfsignedcertificate). Aby zapoznać się z nieobsługiwanym przykładem, zobacz [UpdateIISExpressSSLForChrome. ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).

   Zainstaluj certyfikaty z podpisem własnym lub certyfikat podpisany przez urząd certyfikacji na **lokalnym komputerze**serwera  > **osobistym** magazynie.

1. Jeśli aplikacja jest [wdrożeniem zależnym od platformy](/dotnet/core/deploying/#framework-dependent-deployments-fdd), zainstaluj platformę .net core, .NET Framework lub obie (Jeśli aplikacja jest aplikacją platformy .NET Core przeznaczoną dla .NET Framework).

   * **.Net core** &ndash; Jeśli aplikacja wymaga platformy .NET Core, uzyskaj i uruchom Instalatora **środowiska uruchomieniowego platformy .NET Core** z [programu .NET Core downloads](https://dotnet.microsoft.com/download). Nie instaluj pełnego zestawu SDK na serwerze.
   * **.NET Framework** &ndash;, jeśli aplikacja wymaga .NET Framework, zapoznaj się z [podręcznikiem instalacji .NET Framework](/dotnet/framework/install/). Zainstaluj wymagane .NET Framework. Instalator dla najnowszej .NET Framework jest dostępny na stronie [plików do pobrania w programie .NET Core](https://dotnet.microsoft.com/download) .

   Jeśli aplikacja jest [wdrożeniem](/dotnet/core/deploying/#self-contained-deployments-scd)niezależnym, aplikacja zawiera środowisko uruchomieniowe w ramach wdrożenia. Na serwerze nie jest wymagana instalacja platformy.

1. Skonfiguruj adresy URL i porty w aplikacji.

   Domyślnie ASP.NET Core wiąże się z `http://localhost:5000`. Aby skonfigurować prefiksy i porty adresów URL, dostępne są następujące opcje:

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * `urls` argument wiersza polecenia
   * Zmienna środowiskowa `ASPNETCORE_URLS`
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   Poniższy przykład kodu pokazuje, jak używać <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> z lokalnym adresem IP serwera `10.0.0.4` na porcie 443:

::: moniker range=">= aspnetcore-3.0"

   [!code-csharp[](httpsys/samples_snapshot/3.x/Program.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

   [!code-csharp[](httpsys/samples_snapshot/2.x/Program.cs?highlight=6)]

::: moniker-end

   Zaletą `UrlPrefixes` jest wygenerowanie komunikatu o błędzie natychmiast dla nieprawidłowo sformatowanych prefiksów.

   Ustawienia w `UrlPrefixes` przesłaniają `UseUrls` @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5. Z tego względu zaletą `UseUrls`, `urls` i zmiennej środowiskowej `ASPNETCORE_URLS` jest łatwiejsze przełączanie między Kestrel i HTTP. sys.

   HTTP. sys używa [formatów ciągu UrlPrefix interfejsu API serwera http](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > **Nie** należy używać powiązań z symbolami wieloznacznymi najwyższego poziomu (`http://*:80/` i `http://+:80`). Powiązania z symbolami wieloznacznymi najwyższego poziomu tworzy luki w zabezpieczeniach aplikacji. Dotyczy to zarówno silnych, jak i słabych symboli wieloznacznych. Używaj jawnych nazw hostów lub adresów IP, a nie symboli wieloznacznych. Powiązanie symboli wieloznacznych subdomen (na przykład `*.mysub.com`) nie jest zagrożeniem bezpieczeństwa, jeśli kontrolujesz całą domenę nadrzędną (w przeciwieństwie do `*.com`, który jest narażony na ataki). Aby uzyskać więcej informacji, zobacz [RFC 7230: sekcja 5,4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).

1. Przedrejestruj prefiksy adresów URL na serwerze.

   Wbudowane narzędzie do konfigurowania protokołu HTTP. sys to *netsh. exe*. *netsh. exe* służy do zastrzegania PREFIKSÓW adresów URL i przypisywania certyfikatów X. 509. Narzędzie wymaga uprawnień administratora.

   Użyj narzędzia *netsh. exe* , aby zarejestrować adresy URL dla aplikacji:

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * `<URL>` &ndash; w pełni kwalifikowany adres URL (Uniform Resource Locator). Nie używaj powiązania z symbolami wieloznacznymi. Użyj prawidłowej nazwy hosta lub lokalnego adresu IP. *Adres URL musi zawierać końcowy ukośnik.*
   * `<USER>` &ndash; określa nazwę użytkownika lub grupy użytkowników.

   W poniższym przykładzie lokalny adres IP serwera to `10.0.0.4`:

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   Po zarejestrowaniu adresu URL narzędzie reaguje na `URL reservation successfully added`.

   Aby usunąć zarejestrowany adres URL, użyj polecenia `delete urlacl`:

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. Zarejestruj certyfikaty X. 509 na serwerze.

   Użyj narzędzia *netsh. exe* do rejestrowania certyfikatów dla aplikacji:

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * `<IP>` &ndash; określa lokalny adres IP dla powiązania. Nie używaj powiązania z symbolami wieloznacznymi. Użyj prawidłowego adresu IP.
   * `<PORT>` &ndash; określa port dla powiązania.
   * `<THUMBPRINT>` &ndash; odcisk palca certyfikatu X. 509.
   * `<GUID>` &ndash; identyfikator GUID generowany przez dewelopera do reprezentowania aplikacji do celów informacyjnych.

   W celach referencyjnych Zapisz identyfikator GUID w aplikacji jako tag pakietu:

   * W programie Visual Studio:
     * Otwórz właściwości projektu aplikacji, klikając prawym przyciskiem myszy aplikację w **Eksplorator rozwiązań** i wybierając pozycję **Właściwości**.
     * Wybierz kartę **pakiet** .
     * Wprowadź identyfikator GUID, który został utworzony w polu **Tagi** .
   * Gdy nie korzystasz z programu Visual Studio:
     * Otwórz plik projektu aplikacji.
     * Dodaj `<PackageTags>` właściwość do nowej lub istniejącej `<PropertyGroup>` z identyfikatorem GUID, który został utworzony:

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   W poniższym przykładzie:

   * Lokalny adres IP serwera to `10.0.0.4`.
   * Generator losowy identyfikator GUID w trybie online zapewnia wartość `appid`.

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   Po zarejestrowaniu certyfikatu narzędzie reaguje na `SSL Certificate successfully added`.

   Aby usunąć rejestrację certyfikatu, użyj `delete sslcert` polecenia:

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   Dokumentacja referencyjna dla programu *netsh. exe*:

   * [Polecenia netsh dla protokołu HTTP (Hypertext Transfer Protocol)](https://technet.microsoft.com/library/cc725882.aspx)
   * [UrlPrefix ciągi](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. Uruchom aplikację.

   Uprawnienia administratora nie są wymagane do uruchomienia aplikacji w przypadku powiązania z hostem localhost przy użyciu protokołu HTTP (nie HTTPS) z numerem portu większym niż 1024. W przypadku innych konfiguracji (na przykład przy użyciu lokalnego adresu IP lub powiązania z portem 443) Uruchom aplikację z uprawnieniami administratora.

   Aplikacja odpowiada na publiczny adres IP serwera. W tym przykładzie serwer jest osiągalny z Internetu przy użyciu publicznego adresu IP `104.214.79.47`.

   W tym przykładzie jest używany certyfikat programistyczny. Strona ładuje się bezpiecznie po pominięciu ostrzeżenia niezaufanego certyfikatu w przeglądarce.

   ![Okno przeglądarki pokazujące załadowana stronę indeksu aplikacji](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scenariusze serwera proxy i modułu równoważenia obciążenia

W przypadku aplikacji hostowanych przez protokół HTTP. sys, które współdziałają z żądaniami z Internetu lub sieci firmowej, może być wymagana dodatkowa konfiguracja w przypadku hostowania za serwerami proxy i modułami równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [konfigurowanie ASP.NET Core do pracy z serwerami proxy i usługami równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Włącz uwierzytelnianie systemu Windows przy użyciu protokołu HTTP. sys](xref:security/authentication/windowsauth#httpsys)
* [Interfejs API serwera HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [HttpSysServer lub repozytorium GitHub (kod źródłowy)](https://github.com/aspnet/HttpSysServer/)
* [Host](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
