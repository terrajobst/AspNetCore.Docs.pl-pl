---
title: Implementacja serwera sieci web HTTP.sys, w programie ASP.NET Core
author: guardrex
description: Więcej informacji na temat HTTP.sys, serwer sieci web platformy ASP.NET Core na Windows. Oparta na sterownik trybu jądra HTTP.sys, sterownik HTTP.sys stanowi alternatywę Kestrel, który może służyć do bezpośredniego połączenia z Internetu bez usług IIS.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/26/2018
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f5ab1a3cbd1020a5ab2bd64a81b5782fd116f069
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450648"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementacja serwera sieci web HTTP.sys, w programie ASP.NET Core

Przez [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), i [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> W tym temacie mają zastosowanie do platformy ASP.NET Core 2.0 lub nowszej. We wcześniejszych wersjach programu ASP.NET Core, nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener).

[Sterownik HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) jest [serwera sieci web dla platformy ASP.NET Core](xref:fundamentals/servers/index) uruchomioną w Windows. Sterownik HTTP.sys stanowi alternatywę [Kestrel](xref:fundamentals/servers/kestrel) i oferuje pewne funkcje, które nie zapewnia Kestrel.

> [!IMPORTANT]
> Sterownik HTTP.sys jest niezgodna z [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) i nie można używać z usług IIS lub IIS Express.

Sterownik HTTP.sys obsługuje następujące funkcje:

* [Uwierzytelnianie Windows](xref:security/authentication/windowsauth)
* Współużytkowanie portów
* Protokołu HTTPS z SNI
* Protokołu HTTP/2 za pośrednictwem protokołu TLS (system Windows 10 lub nowsza wersja)
* Przekazywanie pliku bezpośrednie
* Buforowanie odpowiedzi
* Gniazda Websocket (system Windows 8 lub nowszy)

Obsługiwane wersje Windows:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Kiedy należy używać HTTP.sys

Sterownik HTTP.sys jest przydatne w przypadku wdrożeń gdzie:

* Istnieje potrzeba, aby uwidocznić server bezpośrednio do Internetu bez korzystania z usług IIS.

  ![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetu](httpsys/_static/httpsys-to-internet.png)

* Wewnętrznego wdrażania wymaga funkcji nie jest dostępna w Kestrel, takich jak [uwierzytelniania Windows](xref:security/authentication/windowsauth).

  ![Sterownik HTTP.sys komunikuje się bezpośrednio z siecią wewnętrzną](httpsys/_static/httpsys-to-internal.png)

Sterownik HTTP.sys to dojrzała technologia, która chroni przed wiele rodzajów ataków i udostępnia niezawodności, bezpieczeństwa i skalowalności serwera sieci web w pełni funkcjonalne. SAM działa jako odbiornik HTTP w pliku HTTP.sys.

## <a name="http2-support"></a>Obsługa protokołu HTTP/2

[Protokołu HTTP/2](https://httpwg.org/specs/rfc7540.html) jest włączone dla aplikacji platformy ASP.NET Core, jeśli następujące podstawowa wymagania zostały spełnione:

* Windows Server 2016 i Windows 10 lub nowszym
* [Negocjowania protokołu warstwy aplikacji (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) połączenia
* Protokół TLS 1.2 lub nowszej połączenia

::: moniker range=">= aspnetcore-2.2"

Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/2`.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Jeśli zostanie nawiązane połączenie HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) raporty `HTTP/1.1`.

::: moniker-end

Protokołu HTTP/2 jest domyślnie włączona. Jeśli nie jest nawiązane połączenie HTTP/2, połączenie jest powraca do protokołu HTTP/1.1. W przyszłej wersji systemu Windows flagi konfiguracji protokołu HTTP/2 będą dostępne, w tym możliwość wyłączenia protokołu HTTP/2 w pliku HTTP.sys.

## <a name="kernel-mode-authentication-with-kerberos"></a>Uwierzytelnianie trybu jądra przy użyciu protokołu Kerberos

Sterownik HTTP.sys delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos. Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i sterownik HTTP.sys. Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika. Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.

## <a name="how-to-use-httpsys"></a>Jak używać HTTP.sys

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>Konfigurowanie aplikacji platformy ASP.NET Core do użycia w pliku HTTP.sys

1. Odwołania do pakietu w pliku projektu nie jest wymagana, gdy za pomocą [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (platformy ASP.NET Core 2.1 lub nowszej). Bez korzystania z `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all, Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).

2. Wywołaj [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) — metoda rozszerzenia podczas tworzenia hosta sieci web, określając wymagane [opcje HTTP.sys](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   Dodatkowa konfiguracja HTTP.sys odbywa się za pośrednictwem [ustawień rejestru](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).

   **Opcje HTTP.sys**

   | Właściwość | Opis | Domyślny |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | Kontrolowanie, czy synchroniczna wejścia/wyjścia jest dozwolone dla `HttpContext.Request.Body` i `HttpContext.Response.Body`. | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | Zezwalaj na anonimowe żądania. | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | Umożliwia określenie schematów uwierzytelniania dozwolone. Może być modyfikowany w dowolnym momencie przed disposing odbiornika. Wartości są dostarczane przez [wyliczenia AuthenticationSchemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None`, i `NTLM`. | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | Próba [trybu jądra](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) buforowanie odpowiedzi z kwalifikujących się nagłówków. Odpowiedź nie może zawierać `Set-Cookie`, `Vary`, lub `Pragma` nagłówków. Musi on zawierać `Cache-Control` nagłówka to `public` i `shared-max-age` lub `max-age` wartość lub `Expires` nagłówka. | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | Maksymalna liczba współbieżnych akceptuje. | 5 &times; [środowiska.<br> ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [MaxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | Maksymalna liczba jednoczesnych połączeń, aby zaakceptować. Użyj `-1` = nieskończoność. Użyj `null` używać ustawienia komputera. | `null`<br>(unlimited) |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | Zobacz <a href="#maxrequestbodysize">MaxRequestBodySize</a> sekcji. | Bajty 30000000<br>(~28.6 MB) |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | Maksymalna liczba żądań, które można umieścić w kolejce. | 1000 |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | Wskazuje, jeśli operacji zapisu treści odpowiedzi, które rozłączy się niepowodzeniem z powodu klienta należy zgłaszać wyjątki lub zakończone normalnie. | `false`<br>(zazwyczaj zakończenie) |
   | [Limity czasu](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | Udostępnianie HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) konfiguracji, która może być również konfigurowane w rejestrze. Skorzystaj z linków interfejsu API, aby dowiedzieć się więcej na temat każdego ustawienia, w tym wartości domyślne:<ul><li>[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; czasu dozwolony dla interfejsu API serwera HTTP opróżnić treści jednostki połączenia Keep-Alive.</li><li>[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; czasu dozwolony dla treści jednostki żądania dostarczenie.</li><li>[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; czasu dozwolony dla interfejsu API serwera HTTP, można przeanalizować żądanego nagłówka.</li><li>[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; czasu dozwolony dla bezczynnego połączenia.</li><li>[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; minimum Wyślij szybkości odpowiedzi na.</li><li>[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; czas przeznaczony na żądanie pozostawać w kolejce żądań, zanim go przejmuje przez aplikację.</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | Określ [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) do rejestrowania w pliku HTTP.sys. Najbardziej przydatna jest [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), która umożliwia Dodaj prefiks do kolekcji. Te mogą zostać zmienione w dowolnym momencie przed disposing odbiornika. |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   Maksymalny dozwolony rozmiar wszelkie treści żądania, w bajtach. Po ustawieniu `null`, żądanie maksymalny rozmiar treści jest nieograniczona. To ograniczenie nie ma wpływu na uaktualnionym połączeń, które zawsze są nieograniczone.

   Zalecaną metodą do zastąpienia limitu w aplikacji ASP.NET Core MVC dla pojedynczego `IActionResult` jest użycie [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) atrybutu metody akcji:

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   Wyjątek jest generowany, jeśli aplikacja próbuje skonfigurować limit na żądanie, po uruchomieniu aplikacji odczytu żądania. `IsReadOnly` Właściwość może służyć do wskazania, jeśli `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza, jest za późno, aby skonfigurować limit.

   Jeśli aplikacja powinien przesłonić [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) żądań, należy użyć [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. Jeśli używasz programu Visual Studio, upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub IIS Express.

   W programie Visual Studio domyślnym profilem uruchamiania jest dla usług IIS Express. Aby uruchomić projekt jako aplikację konsolową, ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu:

   ![Wybierz profil aplikacji konsoli](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Konfigurowanie systemu Windows Server

1. Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd), zainstalowania platformy .NET Core i .NET Framework (Jeśli aplikacja jest aplikacją platformy .NET Core przeznaczonych dla platformy .NET Framework).

   * **.NET core** &ndash; Jeśli aplikacja wymaga platformy .NET Core, Uzyskaj i uruchom Instalatora programu .NET Core z [pobiera wszystkie .NET](https://www.microsoft.com/net/download/all).
   * **.NET framework** &ndash; Jeśli aplikacja wymaga programu .NET Framework, zobacz [.NET Framework: Przewodnik instalacji](/dotnet/framework/install/) można znaleźć instrukcje dotyczące instalacji. Zainstaluj wymagane .NET Framework. Instalator dla najnowszej wersji .NET Framework, można znaleźć w folderze [pobiera wszystkie .NET](https://www.microsoft.com/net/download/all).

2. Konfigurowanie adresów URL i portów dla aplikacji.

   Domyślnie platforma ASP.NET Core wiąże `http://localhost:5000`. Aby skonfigurować przedrostki adresów URL i portów, są następujące opcje przy użyciu:

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * `urls` argument wiersza polecenia
   * `ASPNETCORE_URLS` Zmienna środowiskowa
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   Poniższy przykład kodu pokazuje sposób użycia [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   Zaletą `UrlPrefixes` jest natychmiast wygenerowany komunikat o błędzie w przypadku prefiksów niewłaściwie sformatowany.

   Ustawienia w `UrlPrefixes` zastąpienia `UseUrls` / `urls` / `ASPNETCORE_URLS` ustawienia. W związku z tym, zaletą `UseUrls`, `urls`i `ASPNETCORE_URLS` zmienna środowiskowa jest łatwiejsze przełączanie między Kestrel i sterownik HTTP.sys. Aby uzyskać więcej informacji na temat `UseUrls`, `urls`, i `ASPNETCORE_URLS`, zobacz [hostów w programie ASP.NET Core](xref:fundamentals/host/index) tematu.

   Używa HTTP.sys [UrlPrefix interfejsu API serwera HTTP, formaty ciągu](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > Powiązania najwyższego poziomu symbolu wieloznacznego (`http://*:80/` i `http://+:80`) powinien **nie** można użyć. Powiązania najwyższego poziomu symboli wieloznacznych otworzyć aplikację w celu luk w zabezpieczeniach. Dotyczy to silnych i słabych symboli wieloznacznych. Użyj nazwy hostów jawne, a nie symboli wieloznacznych. Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.mysub.com`) nie ma to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony). Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.

3. Preregister przedrostki adresów URL, aby powiązać sterownik HTTP.sys i konfigurowanie certyfikatów x.509.

   Jeśli jest wstępnie przedrostki adresów URL nie są zarejestrowane w Windows, należy uruchomić tę aplikację z uprawnieniami administratora. Jedynym wyjątkiem jest, podczas tworzenia wiązania do hosta lokalnego przy użyciu protokołu HTTP (a nie HTTPS), numer portu jest większa niż 1024. W takim przypadku uprawnienia administratora nie są wymagane.

   1. Wbudowane narzędzie do konfigurowania HTTP.sys *netsh.exe*. *netsh.exe* służy do zarezerwowania przedrostki adresów URL i przypisywać certyfikaty X.509. Narzędzie wymaga uprawnień administratora.

      Poniższy przykład przedstawia te polecenia, aby zarezerwować przedrostki adresów URL dla portów 80 i 443:

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      Poniższy przykład pokazuje, jak przypisać certyfikat X.509:

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}"
      ```

      Dokumentacji dla *netsh.exe*:

      * [Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
      * [Ciągi UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

   2. Tworzenie certyfikatów z podpisem własnym X.509, jeśli jest to wymagane.

      [!INCLUDE [How to make an X.509 cert](../../includes/make-x509-cert.md)]

4. Otwórz porty zapory muszą zezwalać na ruch do osiągnięcia HTTP.sys. Użyj *netsh.exe* lub [poleceń cmdlet programu PowerShell](https://technet.microsoft.com/library/jj554906).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Dla aplikacji hostowanych przez rozszerzenie HTTP.sys, które współdziałają z żądaniami z Internetu lub sieci firmowej dodatkowa konfiguracja może być wymagane w przypadku hostowania za serwery proxy i moduły równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Serwer HTTP interfejsu API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [repozytorium GitHub ASPNET/HttpSysServer (kodu źródłowego)](https://github.com/aspnet/HttpSysServer/)
* <xref:fundamentals/host/index>
* <xref:test/troubleshoot>
