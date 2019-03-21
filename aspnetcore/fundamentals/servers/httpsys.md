---
title: Implementacja serwera sieci web HTTP.sys, w programie ASP.NET Core
author: guardrex
description: Więcej informacji na temat HTTP.sys, serwer sieci web platformy ASP.NET Core na Windows. Oparta na sterownik trybu jądra HTTP.sys, sterownik HTTP.sys stanowi alternatywę Kestrel, który może służyć do bezpośredniego połączenia z Internetu bez usług IIS.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/21/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 6ec9b7bf3da0015b8ac3918a4d47644fffc14cdb
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209177"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementacja serwera sieci web HTTP.sys, w programie ASP.NET Core

Przez [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), i [Luke Latham](https://github.com/guardrex)

[Sterownik HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) jest [serwera sieci web dla platformy ASP.NET Core](xref:fundamentals/servers/index) uruchomioną w Windows. Sterownik HTTP.sys stanowi alternatywę [Kestrel](xref:fundamentals/servers/kestrel) serwera i ofert, niektóre funkcje, że nie zapewnia Kestrel.

> [!IMPORTANT]
> Sterownik HTTP.sys nie jest zgodny z [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) i nie można używać z usług IIS lub IIS Express.

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

2. Wywołaj <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> — metoda rozszerzenia podczas tworzenia hosta sieci Web, określając wymagane <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>:

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   Dodatkowa konfiguracja HTTP.sys odbywa się za pośrednictwem [ustawień rejestru](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).

   **Opcje HTTP.sys**

   | Właściwość | Opis | Domyślny |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | Kontrolowanie, czy synchroniczna wejścia/wyjścia jest dozwolone dla `HttpContext.Request.Body` i `HttpContext.Response.Body`. | `true` |
   | [Authentication.AllowAnonymous](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | Zezwalaj na anonimowe żądania. | `true` |
   | [Authentication.Schemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | Umożliwia określenie schematów uwierzytelniania dozwolone. Może być modyfikowany w dowolnym momencie przed disposing odbiornika. Wartości są dostarczane przez [wyliczenia AuthenticationSchemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes): `Basic`, `Kerberos`, `Negotiate`, `None`, i `NTLM`. | `None` |
   | [EnableResponseCaching](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | Próba [trybu jądra](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) buforowanie odpowiedzi z kwalifikujących się nagłówków. Odpowiedź nie może zawierać `Set-Cookie`, `Vary`, lub `Pragma` nagłówków. Musi on zawierać `Cache-Control` nagłówka to `public` i `shared-max-age` lub `max-age` wartość lub `Expires` nagłówka. | `true` |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | Maksymalna liczba współbieżnych akceptuje. | 5 &times; [środowiska.<br> ProcessorCount](xref:System.Environment.ProcessorCount) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | Maksymalna liczba jednoczesnych połączeń, aby zaakceptować. Użyj `-1` = nieskończoność. Użyj `null` używać ustawienia komputera. | `null`<br>(unlimited) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | Zobacz <a href="#maxrequestbodysize">MaxRequestBodySize</a> sekcji. | Bajty 30000000<br>(~28.6 MB) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | Maksymalna liczba żądań, które można umieścić w kolejce. | 1000 |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | Wskazuje, jeśli operacji zapisu treści odpowiedzi, które rozłączy się niepowodzeniem z powodu klienta należy zgłaszać wyjątki lub zakończone normalnie. | `false`<br>(zazwyczaj zakończenie) |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | Udostępnianie HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> konfiguracji, która może być również konfigurowane w rejestrze. Skorzystaj z linków interfejsu API, aby dowiedzieć się więcej na temat każdego ustawienia, w tym wartości domyślne:<ul><li>[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; czasu dozwolony dla interfejsu API serwera HTTP opróżnić treści jednostki połączenia Keep-Alive.</li><li>[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; czasu dozwolony dla treści jednostki żądania dostarczenie.</li><li>[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; czasu dozwolony dla interfejsu API serwera HTTP, można przeanalizować żądanego nagłówka.</li><li>[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; czasu dozwolony dla bezczynnego połączenia.</li><li>[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; minimum Wyślij szybkości odpowiedzi na.</li><li>[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; czas przeznaczony na żądanie pozostawać w kolejce żądań, zanim go przejmuje przez aplikację.</li></ul> |  |
   | <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | Określ <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> do rejestrowania w pliku HTTP.sys. Najbardziej przydatna jest [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*), która umożliwia Dodaj prefiks do kolekcji. Te mogą zostać zmienione w dowolnym momencie przed disposing odbiornika. |  |

   <a name="maxrequestbodysize"></a>

   **MaxRequestBodySize**

   Maksymalny dozwolony rozmiar wszelkie treści żądania, w bajtach. Po ustawieniu `null`, żądanie maksymalny rozmiar treści jest nieograniczona. To ograniczenie nie ma wpływu na uaktualnionym połączeń, które zawsze są nieograniczone.

   Zalecaną metodą do zastąpienia limitu w aplikacji ASP.NET Core MVC dla pojedynczego `IActionResult` jest użycie <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> atrybutu metody akcji:

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   Wyjątek jest generowany, jeśli aplikacja próbuje skonfigurować limit na żądanie, po uruchomieniu aplikacji odczytu żądania. `IsReadOnly` Właściwość może służyć do wskazania, jeśli `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza, jest za późno, aby skonfigurować limit.

   Jeśli aplikacja powinien przesłonić <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> żądań, należy użyć <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>:

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. Jeśli używasz programu Visual Studio, upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub IIS Express.

   W programie Visual Studio domyślnym profilem uruchamiania jest dla usług IIS Express. Aby uruchomić projekt jako aplikację konsolową, ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu:

   ![Wybierz profil aplikacji konsoli](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Konfigurowanie systemu Windows Server

1. Określenia portów, aby otworzyć aplikację i używania [zapory Windows](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule) lub [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) polecenia cmdlet programu PowerShell, aby otworzyć porty zapory muszą zezwalać na ruch do osiągnięcia HTTP.sys. W poniższych poleceń i konfiguracji aplikacji używany jest port 443.

1. Podczas wdrażania aplikacji na Maszynie wirtualnej platformy Azure, należy otworzyć ruch na portach [sieciowej grupy zabezpieczeń](/azure/virtual-machines/windows/nsg-quickstart-portal). W poniższych poleceń i konfiguracji aplikacji używany jest port 443.

1. Uzyskaj i zainstaluj certyfikaty X.509, jeśli jest to wymagane.

   W Windows, tworzenie certyfikatów z podpisem własnym za pomocą [polecenia cmdlet programu PowerShell New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate). Na przykład nieobsługiwany zobacz [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).

   Instalowanie certyfikatów z podpisem własnym albo podpisanych przez urząd certyfikacji na serwerze **komputera lokalnego** > **osobistych** przechowywania.

1. Jeśli aplikacja jest [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd), zainstalowania platformy .NET Core i .NET Framework (Jeśli aplikacja jest aplikacją platformy .NET Core przeznaczonych dla platformy .NET Framework).

   * **.NET core** &ndash; Jeśli aplikacja wymaga platformy .NET Core, uzyskanie i uruchomienie **środowisko uruchomieniowe programu .NET Core** Instalator [pobierania programu .NET Core](https://dotnet.microsoft.com/download). Nie należy instalować pełnego zestawu SDK na serwerze.
   * **.NET framework** &ndash; Jeśli aplikacja wymaga programu .NET Framework, zobacz [Przewodnik instalacji .NET Framework](/dotnet/framework/install/). Zainstaluj wymagane .NET Framework. Instalator dla najnowszej wersji .NET Framework jest dostępna z [pobierania programu .NET Core](https://dotnet.microsoft.com/download) strony.

   Jeśli aplikacja jest [niezależna wdrożenia](/dotnet/core/deploying/#framework-dependent-deployments-scd), aplikacja zawiera środowisko uruchomieniowe w jej wdrożenia. Instalacja framework nie jest wymagane na serwerze.

1. Konfigurowanie adresów URL i portów w aplikacji.

   Domyślnie platforma ASP.NET Core wiąże `http://localhost:5000`. Aby skonfigurować przedrostki adresów URL i portów, dostępne są następujące opcje:

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * `urls` argument wiersza polecenia
   * `ASPNETCORE_URLS` Zmienna środowiskowa
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   Poniższy przykład kodu pokazuje sposób użycia <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> z serwera lokalnego adresu IP `10.0.0.4` na porcie 443:

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=11)]

   Zaletą `UrlPrefixes` jest natychmiast wygenerowany komunikat o błędzie w przypadku prefiksów niewłaściwie sformatowany.

   Ustawienia w `UrlPrefixes` zastąpienia `UseUrls` / `urls` / `ASPNETCORE_URLS` ustawienia. W związku z tym, zaletą `UseUrls`, `urls`i `ASPNETCORE_URLS` zmienna środowiskowa jest łatwiejsze przełączanie między Kestrel i sterownik HTTP.sys. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.

   Używa HTTP.sys [UrlPrefix interfejsu API serwera HTTP, formaty ciągu](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > Powiązania najwyższego poziomu symbolu wieloznacznego (`http://*:80/` i `http://+:80`) powinien **nie** można użyć. Symbol wieloznaczny najwyższego poziomu powiązania Utwórz aplikację luk w zabezpieczeniach. Dotyczy to silnych i słabych symboli wieloznacznych. Użyj hosta jawne nazwy lub adresy IP, a nie symboli wieloznacznych. Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.mysub.com`) nie jest to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony). Aby uzyskać więcej informacji, zobacz [RFC 7230: Ppkt 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).

1. Preregister przedrostki adresów URL na serwerze.

   Wbudowane narzędzie do konfigurowania HTTP.sys *netsh.exe*. *netsh.exe* służy do zarezerwowania przedrostki adresów URL i przypisywać certyfikaty X.509. Narzędzie wymaga uprawnień administratora.

   Użyj *netsh.exe* narzędzie, aby zarejestrować adresy URL aplikacji:

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * `<URL>` &ndash; W pełni kwalifikowaną URL Uniform Resource Locator (). Nie używaj wiązania symboli wieloznacznych. Za pomocą prawidłową nazwę hosta lokalnego adresu IP. *Adres URL musi zawierać końcowego ukośnika.*
   * `<USER>` &ndash; Określa nazwę użytkownika lub grupy użytkowników.

   W poniższym przykładzie jest lokalny adres IP serwera `10.0.0.4`:

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   Po zarejestrowaniu się adres URL, narzędzie odpowiada za pomocą `URL reservation successfully added`.

   Aby usunąć zarejestrowanego adresu URL, użyj `delete urlacl` polecenia:

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. Zarejestruj certyfikaty X.509 na serwerze.

   Użyj *netsh.exe* narzędzia, aby zarejestrować certyfikat dla aplikacji:

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * `<IP>` &ndash; Określa lokalny adres IP dla wiązania. Nie używaj wiązania symboli wieloznacznych. Użyj prawidłowego adresu IP.
   * `<PORT>` &ndash; Określa port dla wiązania.
   * `<THUMBPRINT>` &ndash; Odcisk palca certyfikatu X.509.
   * `<GUID>` &ndash; Wygenerowany przez projektanta identyfikator GUID do reprezentowania aplikacji do celów informacyjnych.

   Do celów referencyjnych zapisywania identyfikatora GUID w aplikacji jako tag pakietu:

   * W programie Visual Studio:
     * Otwórz właściwości projektu aplikacji klikając prawym przyciskiem myszy w aplikacji w **Eksploratora rozwiązań** i wybierając polecenie **właściwości**.
     * Wybierz **pakietu** kartę.
     * Wprowadź identyfikator GUID, który został utworzony w **tagi** pola.
   * Bez korzystania z programu Visual Studio:
     * Otwórz plik projektu aplikacji.
     * Dodaj `<PackageTags>` właściwości na nową lub istniejącą `<PropertyGroup>` z identyfikatorem GUID, który został utworzony:

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   W poniższym przykładzie:

   * Lokalny adres IP serwera jest `10.0.0.4`.
   * Zapewnia online losowe generator GUID `appid` wartość.

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   Po zarejestrowaniu certyfikatu, narzędzie odpowiada za pomocą `SSL Certificate successfully added`.

   Aby usunąć rejestracji certyfikatu, użyj `delete sslcert` polecenia:

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   Dokumentacji dla *netsh.exe*:

   * [Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
   * [Ciągi UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. Uruchom aplikację.

   Uprawnienia administratora nie są wymagane do uruchomienia aplikacji, podczas tworzenia wiązania do hosta lokalnego przy użyciu protokołu HTTP (a nie HTTPS), numer portu jest większa niż 1024. W przypadku innych konfiguracji (na przykład przy użyciu lokalnego adresu IP lub powiązania z portem 443) należy uruchomić tę aplikację z uprawnieniami administratora.

   Aplikacja reaguje na publiczny adres IP serwera. W tym przykładzie jest połączyć się z serwerem z sieci Internet, publiczny adres IP `104.214.79.47`.

   Certyfikatu deweloperskiego jest używany w tym przykładzie. Strona ładuje się bezpiecznie po pomijanie ostrzeżeń niezaufanego certyfikatu w przeglądarce.

   ![Załadowano indeksu aplikacji wyświetlana w oknie przeglądarki](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Dla aplikacji hostowanych przez rozszerzenie HTTP.sys, które współdziałają z żądaniami z Internetu lub sieci firmowej dodatkowa konfiguracja może być wymagane w przypadku hostowania za serwery proxy i moduły równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Włącz uwierzytelnianie Windows w pliku HTTP.sys](xref:security/authentication/windowsauth#enable-windows-authentication-with-httpsys)
* [Serwer HTTP interfejsu API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [repozytorium GitHub ASPNET/HttpSysServer (kodu źródłowego)](https://github.com/aspnet/HttpSysServer/)
* [Host](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
