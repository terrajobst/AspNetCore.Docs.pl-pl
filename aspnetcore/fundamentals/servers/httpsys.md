---
title: Sterownik HTTP.sys implementacja serwera sieci web platformy ASP.NET Core
author: tdykstra
description: "Więcej informacji na temat HTTP.sys, serwer sieci web platformy ASP.NET Core w systemie Windows. W oparciu sterownik trybu jądra HTTP.sys, sterownik HTTP.sys stanowi alternatywę dla Kestrel, który może służyć do bezpośredniego połączenia z Internetem bez usług IIS."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/28/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 730ecf12f718f6bbbdefb7cdc561481b126c995b
ms.sourcegitcommit: c5ecda3c5b1674b62294cfddcb104e7f0b9ce465
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Sterownik HTTP.sys implementacja serwera sieci web platformy ASP.NET Core

Przez [Dykstra Tomasz](https://github.com/tdykstra), [Roaming Krzysztof](https://github.com/Tratcher), i [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> Ten temat dotyczy do platformy ASP.NET Core 2.0 lub nowszej. We wcześniejszych wersjach programu ASP.NET Core, nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener).

[Sterownik HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) jest [serwera sieci web dla platformy ASP.NET Core](xref:fundamentals/servers/index) systemem Windows. Sterownik HTTP.sys stanowi alternatywę dla [Kestrel](xref:fundamentals/servers/kestrel) i oferuje funkcje, które nie zapewnia Kestrel.

> [!IMPORTANT]
> Sterownik HTTP.sys jest niezgodny z [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) i nie można używać z usług IIS lub usług IIS Express.

Sterownik HTTP.sys obsługuje następujące funkcje:

* [Uwierzytelnianie systemu Windows](xref:security/authentication/windowsauth)
* Udostępnianie portów
* Protokół HTTPS z SNI
* HTTP/2 za pośrednictwem protokołu TLS (system Windows 10 lub nowszy)
* Przekazywanie pliku bezpośredniego
* Buforowanie odpowiedzi
* Protokół WebSockets (z systemem Windows 8 lub nowszy)

Obsługiwane wersje systemu Windows:

* Windows 7 lub nowszy
* Windows Server 2008 R2 lub nowszy

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Kiedy należy używać HTTP.sys

Sterownik HTTP.sys jest przydatne w przypadku wdrożeń gdzie:

* Brak konieczności udostępnić server bezpośrednio do Internetu, nie za pomocą usług IIS.

  ![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetem](httpsys/_static/httpsys-to-internet.png)

* Wewnętrznego wdrażania wymaga funkcji nie jest dostępna w Kestrel, takich jak [uwierzytelniania systemu Windows](xref:security/authentication/windowsauth).

  ![Sterownik HTTP.sys komunikuje się bezpośrednio z siecią wewnętrzną](httpsys/_static/httpsys-to-internal.png)

Sterownik HTTP.sys jest dojrzała technologia, która chroni przed wiele rodzajów ataków i zapewnia niezawodność, zabezpieczeń i skalowalność serwera sieci web kompletne. SAM działa jako odbiornik HTTP u góry pliku HTTP.sys. 

## <a name="how-to-use-httpsys"></a>Jak używać HTTP.sys

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>Konfigurowanie aplikacji platformy ASP.NET Core korzystanie z pliku HTTP.sys

1. Odwołanie pakietu w pliku projektu nie jest wymagane, korzystając z [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)) (platformy ASP.NET Core w wersji 2.0 lub nowszej). Korzystając z nie `Microsoft.AspNetCore.All` metapackage, Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).

1. Wywołanie [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) — metoda rozszerzenia podczas kompilowania hosta sieci web, określając każdego wymaganego [opcje HTTP.sys](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   Obsługiwane przez sterownik HTTP.sys dodatkowe czynności konfiguracyjne [ustawień rejestru](https://support.microsoft.com/kb/820129).

   **Opcje HTTP.sys**

   | Właściwość | Opis | Domyślny |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | Kontrolowanie, czy synchroniczne we/wy jest dozwolone dla `HttpContext.Request.Body` i `HttpContext.Response.Body`. | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | Zezwalaj na anonimowe żądania. | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | Określ schematy uwierzytelniania dozwolonych. Może być modyfikowany w dowolnym momencie przed disposing odbiornika. Wartości są dostarczane przez [wyliczenia AuthenticationSchemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None`, i `NTLM`. | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | Próba [trybu jądra](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) buforowanie odpowiedzi z kwalifikujących się nagłówków. Odpowiedź nie może zawierać `Set-Cookie`, `Vary`, lub `Pragma` nagłówków. Musi on zawierać `Cache-Control` nagłówka to jest `public` i `shared-max-age` lub `max-age` wartość, lub `Expires` nagłówka. | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | Maksymalna liczba równoczesnych akceptuje. | 5 &times; [środowiska.<br> ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [maxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | Maksymalna liczba jednoczesnych połączeń, aby zaakceptować. Użyj `-1` = nieskończoność. Użyj `null` do używania ustawienia komputera. | `null`<br>(unlimited) |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | Zobacz <a href="#maxrequestbodysize">MaxRequestBodySize</a> sekcji. | 30000000 bajtów<br>(~28.6 MB) |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | Maksymalna liczba żądań, które mogą być umieszczone w kolejce. | 1000 |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | Wskazać, czy operacji zapisu treści odpowiedzi, które zakończy się niepowodzeniem z powodu klienta połączenie powinno zgłaszają wyjątki, czy zakończone normalnie. | `false`<br>(zazwyczaj ukończenia) |
   | [Limity czasu](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | Udostępnianie pliku HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) konfigurację, która może być również skonfigurowane w rejestrze. Skorzystaj z łączy interfejsu API, aby dowiedzieć się więcej na temat każdego ustawienia, łącznie z wartościami domyślnymi:<ul><li>[Timeouts.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.drainentitybody) &ndash; czas dozwolony dla interfejsu API serwera HTTP w celu opróżnienia treści jednostki podtrzymania połączenia.</li><li>[Timeouts.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.entitybody) &ndash; czas dozwolony dla treści jednostki żądania do celu.</li><li>[Timeouts.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.headerwait) &ndash; czas dozwolony dla interfejsu API serwera HTTP przeanalizować nagłówka żądania.</li><li>[Timeouts.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.idleconnection) &ndash; czas dozwolony dla bezczynnego połączenia.</li><li>[Timeouts.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.minsendbytespersecond) &ndash; minimalna szybkość odpowiedzi wysyłania.</li><li>[Timeouts.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts.requestqueue) &ndash; czas przeznaczony na żądanie pozostawać w kolejce żądań, zanim go przejmuje przez aplikację.</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | Określ [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) można zarejestrować przy użyciu składnika HTTP.sys. Najbardziej przydatne jest [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), który służy do dodania do kolekcji prefiksu. Te mogą zostać zmodyfikowane w dowolnym momencie przed disposing odbiornika. |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   Maksymalny dozwolony rozmiar żadnych treści żądania w bajtach. Jeśli wartość `null`, żądanie maksymalny rozmiar treści jest nieograniczony. To ograniczenie nie ma wpływu na uaktualnionym połączeń, które są zawsze nieograniczone.

   Zalecaną metodą do zastąpienia limitu w aplikacji ASP.NET Core MVC z jednym `IActionResult` jest użycie [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) atrybutu metody akcji:
   
   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   Jeśli aplikacja próbuje skonfigurować limit na żądanie, po uruchomieniu aplikacji odczytu żądania, jest zgłaszany wyjątek. `IsReadOnly` Właściwości można wskazać, czy `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza jest za późno skonfigurować limit.

   Jeśli aplikacja powinny zastępować [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) na żądanie, użyj [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

1. Jeśli używasz programu Visual Studio, upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub usług IIS Express.

   W programie Visual Studio domyślnego profilu uruchamiania jest dla usług IIS Express. Aby uruchomić projekt jako aplikację konsoli, ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu:

   ![Wybierz profil aplikacji konsoli](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Konfigurowanie systemu Windows Server

1. Jeśli aplikacja jest [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), zainstaluj oprogramowanie .NET Core i .NET Framework (jeśli jest to aplikacja .NET Core docelowy program .NET Framework).

   * **Oprogramowanie .NET core** &ndash; Jeśli aplikacja wymaga platformy .NET Core, Uzyskaj i uruchom Instalatora programu .NET Core z [pobiera .NET](https://www.microsoft.com/net/download/windows).
   * **.NET framework** &ndash; Jeśli aplikacja wymaga programu .NET Framework, zobacz [.NET Framework: Przewodnik instalacji](/dotnet/framework/install/) można znaleźć instrukcje dotyczące instalacji. Zainstaluj wymagane .NET Framework. Instalator programu .NET Framework najnowsze można znaleźć w folderze [pobiera .NET](https://www.microsoft.com/net/download/windows).

1. Skonfiguruj adresy URL i portów dla aplikacji.

   Domyślnie program ASP.NET Core wiąże `http://localhost:5000`. Aby skonfigurować prefiksy URL i portów, są następujące opcje przy użyciu:

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * `urls` Argument wiersza polecenia
   * `ASPNETCORE_URLS` Zmienna środowiskowa
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   Poniższy przykładowy kod przedstawia sposób użycia [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes):

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   Zaletą `UrlPrefixes` jest natychmiast wygenerowany komunikat o błędzie dla prefiksów niewłaściwie sformatowany.

   Ustawienia w `UrlPrefixes` zastąpienia `UseUrls` / `urls` / `ASPNETCORE_URLS` ustawienia. W związku z tym zaletą `UseUrls`, `urls`i `ASPNETCORE_URLS` zmiennej środowiskowej, jest łatwiejsze w celu przełączania się między Kestrel i sterownik HTTP.sys. Aby uzyskać więcej informacji na temat `UseUrls`, `urls`, i `ASPNETCORE_URLS`, zobacz [hostingu](xref:fundamentals/hosting).

   Korzysta z pliku HTTP.sys [formaty ciągu UrlPrefix interfejsu API serwera HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

1. Preregister prefiksów URL do powiązania do pliku HTTP.sys i konfigurowanie certyfikatów x.509.

   Jeśli adres URL prefiksy nie są preregistered w systemie Windows, uruchom aplikację z uprawnieniami administratora. Jedynym wyjątkiem jest podczas wiązania z hostem lokalnym przy użyciu protokołu HTTP (a nie HTTPS) z większą niż 1024 numeru portu. W takim przypadku uprawnienia administratora nie są wymagane.

   1. Wbudowane narzędzie do konfigurowania HTTP.sys jest *netsh.exe*. *netsh.exe* służy do zarezerwowania prefiksy URL i przypisać certyfikatów X.509. Narzędzie wymaga uprawnień administratora.

      W poniższym przykładzie przedstawiono poleceń do zarezerwowania prefiksy URL porty 80 i 443:

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      Poniższy przykład pokazuje, jak można przypisać certyfikat X.509:

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
      ```

      Odwołanie dokumentacji *netsh.exe*:

      * [Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
      * [Ciągi UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

   1. Tworzenie certyfikatów X.509 z podpisem własnym, jeśli jest to wymagane.

     [!INCLUDE[How to make an X.509 cert](../../includes/make-x509-cert.md)]

1. Otwórz porty zapory, aby zezwolić na ruch do pliku HTTP.sys. Użyj *netsh.exe* lub [poleceń cmdlet programu PowerShell](https://technet.microsoft.com/library/jj554906).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Interfejsu API serwera HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [repozytorium GitHub ASPNET/HttpSysServer (kodu źródłowego)](https://github.com/aspnet/HttpSysServer/)
* [Hosting](xref:fundamentals/hosting)
