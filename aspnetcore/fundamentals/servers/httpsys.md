---
title: Sterownik HTTP.sys implementacja serwera sieci web platformy ASP.NET Core
author: rick-anderson
description: "Wprowadza HTTP.sys, serwer sieci web platformy ASP.NET Core w systemie Windows. W oparciu sterownik trybu jądra Http.Sys, sterownik HTTP.sys stanowi alternatywę dla Kestrel, który może służyć do bezpośredniego połączenia z Internetem bez usług IIS."
keywords: "ASP.NET Core,HttpSys,HTTP.sys,HttpListener,url prefiksy protokołu SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: d3f9eb4943ed62b674d6bb2ab1b275b0a3c02343
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Sterownik HTTP.sys implementacja serwera sieci web platformy ASP.NET Core

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Roaming Krzysztof](https://github.com/Tratcher)

> [!NOTE]
> Ten temat dotyczy tylko programu ASP.NET Core 2.0 i nowszych. We wcześniejszych wersjach programu ASP.NET Core, nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener).

Sterownik HTTP.sys jest [serwera sieci web dla platformy ASP.NET Core](index.md) , na którym działają tylko w systemie Windows. Jest zbudowany na [sterownik trybu jądra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). Sterownik HTTP.sys stanowi alternatywę dla [Kestrel](kestrel.md) oferująca niektórych funkcji, które nie Kestel. **Składnik HTTP.sys nie można używać z usług IIS lub usług IIS Express, ponieważ nie jest zgodna z [platformy ASP.NET Core modułu](aspnet-core-module.md).**

Sterownik HTTP.sys obsługuje następujące funkcje:

- [Uwierzytelnianie systemu Windows](xref:security/authentication/windowsauth)
- Udostępnianie portów
- Protokół HTTPS z SNI
- HTTP/2 za pośrednictwem protokołu TLS (z systemem Windows 10)
- Przekazywanie pliku bezpośredniego
- Buforowanie odpowiedzi
- Protokół WebSockets (z systemem Windows 8)

Obsługiwane wersje systemu Windows:

- Windows 7 i Windows Server 2008 R2 lub nowszy

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Kiedy należy używać HTTP.sys

Sterownik HTTP.sys jest przydatne w przypadku wdrożeń, których należy udostępnić server bezpośrednio do Internetu, nie za pomocą usług IIS.

![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetem](httpsys/_static/httpsys-to-internet.png)

Ponieważ jest zbudowany na Http.Sys, sterownik HTTP.sys nie wymaga zwrotnego serwera proxy dla ochrony przed atakami. Sterownik Http.Sys jest dojrzała technologia, która chroni przed wiele rodzajów ataków i zapewnia niezawodność, zabezpieczeń i skalowalność serwera sieci web kompletne. SAM działa jako odbiornik HTTP u góry pliku Http.Sys. 

Sterownik HTTP.sys jest dobrym rozwiązaniem w przypadku wdrożeń wewnętrznego, gdy będziesz potrzebować funkcja nie jest dostępna w Kestrel, takich jak uwierzytelnianie systemu Windows.

![Sterownik HTTP.sys komunikuje się bezpośrednio z sieci wewnętrznej](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Jak używać HTTP.sys

Poniżej przedstawiono omówienie zadań instalacji systemu operacyjnego hosta i aplikacji platformy ASP.NET Core.

### <a name="configure-windows-server"></a>Konfigurowanie systemu Windows Server

* Zainstaluj wersję platformy .NET, która wymaga aplikacji, takich jak [.NET Core](https://www.microsoft.com/net/download/core) lub [.NET Framework](https://www.microsoft.com/net/download/framework).

* Preregister prefiksów URL do powiązania do pliku HTTP.sys i skonfigurować certyfikaty SSL

   Jeśli nie preregister prefiksy adresów URL w systemie Windows, należy do uruchamiania aplikacji z uprawnieniami administratora. Jedynym wyjątkiem jest powiązaniu localhost przy użyciu protokołu HTTP (a nie HTTPS) z większą niż 1024; numeru portu w takim przypadku uprawnienia administratora nie są wymagane.

   Aby uzyskać więcej informacji, zobacz [preregister prefiksy i konfigurowanie protokołu SSL](#preregister-url-prefixes-and-configure-ssl) dalszej części tego artykułu.

* Otwórz porty zapory, aby zezwolić na ruch do pliku HTTP.sys.

   Można użyć *netsh.exe* lub [poleceń cmdlet programu PowerShell](https://technet.microsoft.com/library/jj554906).

Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Konfigurowanie aplikacji platformy ASP.NET Core używać HTTP.sys

* Żadna instalacja pakietu jest niezbędny w przypadku używasz [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) pakietu znajduje się w metapackage.

* Wywołanie `UseHttpSys` — metoda rozszerzenia na `WebHostBuilder` w Twojej `Main` metoda określania żadnego [opcje HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) potrzebne, jak pokazano w poniższym przykładzie:

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Skonfiguruj opcje HTTP.sys

Poniżej przedstawiono niektóre ustawienia HTTP.sys i ograniczeń, które można skonfigurować.

**Maksymalna liczba połączeń klientów**

Można ustawić maksymalną liczbę równoczesnych otwartych połączeń TCP dla całej aplikacji w następującym kodem *Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

Maksymalna liczba połączeń jest nieograniczony (null) domyślnie.

**Żądanie maksymalny rozmiar treści**

Domyślny rozmiar treści żądania maksymalna jest 30,000,000 bajtów, czyli około 28.6 MB.

Zalecanym sposobem zastąpienie limit w aplikacji platformy ASP.NET Core MVC jest użycie [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atrybutu metody akcji:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Oto przykład, który pokazuje, jak skonfigurować ograniczenia dla całej aplikacji, każde żądanie:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Można zastąpić ustawienie dla określonego żądania w *Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Jeśli próbujesz skonfigurować limit na żądanie, po uruchomieniu aplikacji odczytu żądania, jest zgłaszany wyjątek. Brak `IsReadOnly` właściwość, która informuje, jeśli `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza jest za późno skonfigurować limit.

Aby uzyskać informacje o innych opcjach HTTP.sys, zobacz [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Skonfiguruj adresy URL i portów do nasłuchiwania 

Domyślnie wiąże platformy ASP.NET Core `http://localhost:5000`. Aby skonfigurować prefiksy URL i portów, można użyć `UseUrls` — metoda rozszerzenia, `urls` argumentu wiersza polecenia, zmienną środowiskową ASPNETCORE_URLS lub `UrlPrefixes` właściwość [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). Poniższy przykład kodu wykorzystuje `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Zaletą `UrlPrefixes` jest, że zostanie wyświetlony komunikat o błędzie natychmiast Jeśli próbujesz dodać prefiks, który jest sformatowany problem. Zaletą `UseUrls` (udostępniane `urls` i ASPNETCORE_URLS) to, że można łatwo przełączać się między Kestrel i sterownik HTTP.sys.

Jeśli używasz zarówno `UseUrls` (lub `urls` lub ASPNETCORE_URLS) i `UrlPrefixes`, ustawienia w `UrlPrefixes` przesłonić te w `UseUrls`. Aby uzyskać więcej informacji, zobacz [hostingu](xref:fundamentals/hosting).

Korzysta z pliku HTTP.sys [formaty ciągu UrlPrefix interfejsu API serwera HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Upewnij się, że określono tego samego prefiksu ciągi w `UseUrls` lub `UrlPrefixes` który preregister na serwerze. 

### <a name="dont-use-iis"></a>Nie używaj usług IIS

Upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub usług IIS Express.

W programie Visual Studio domyślnego profilu uruchamiania jest dla usług IIS Express. Aby uruchomić projekt jako aplikacji konsoli, ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu.

![Wybierz profil aplikacji konsoli](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Preregister prefiksy URL i skonfigurować protokół SSL

Zarówno usług IIS, jak i składnik HTTP.sys polegać na podstawowy sterownik trybu jądra Http.Sys do nasłuchiwania żądań i wstępne przetwarzanie. W usługach IIS interfejs użytkownika zarządzania umożliwia stosunkowo łatwa do skonfigurowania wszystko. Jednak należy skonfigurować serwer Http.Sys samodzielnie. Wbudowane narzędzie do wykonywania, czyli *netsh.exe*. 

Z *netsh.exe* można zarezerwować prefiksy URL i przypisać certyfikatów SSL. Narzędzie wymaga uprawnień administracyjnych.

W poniższym przykładzie przedstawiono minimalnej liczbie niezbędnej do zarezerwowania prefiksy URL porty 80 i 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Poniższy przykład przedstawia sposób przypisania certyfikatu SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

W tym miejscu znajduje się dokumentacja odwołanie *netsh.exe*:

* [Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [Ciągi UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Następujące zasoby zawierają szczegółowe instrukcje dotyczące kilka scenariuszy. Artykuły, które odwołują się do HttpListener są stosowane jednakowo do pliku HTTP.sys, zgodnie z obu są oparte na pliku Http.Sys.

* [Porady: Konfigurowanie portu z certyfikatem SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Komunikacja HTTPS - HttpListener na podstawie hostingu oraz certyfikatu klienta](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to jest blogu innych firm i jest dość stary, ale nadal zawiera przydatne informacje.
* [Porady: HttpListener za pomocą wskazówki lub serwer Http niezarządzanych kodu (C++) jako serwera SSL proste](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) starsze blogu zawierający przydatne informacje jest zbyt.

Poniżej przedstawiono niektóre narzędzia innych firm, które mogą być łatwiejsze niż w *netsh.exe* wiersza polecenia. Nie są one udostępniane przez lub podpisane przez firmę Microsoft. Narzędzia Uruchom jako administrator domyślnie, ponieważ *netsh.exe* sam wymaga uprawnień administratora.

* [Sterownik HTTP.sys Menedżera](http://httpsysmanager.codeplex.com/) udostępnia interfejs do listy i konfigurowania certyfikatów SSL i opcji, zastrzeżenia prefiksu i listy zaufanych certyfikatów. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) pozwala wyświetlić lub skonfigurować certyfikaty SSL i prefiksy URL. Interfejs użytkownika jest bardziej precyzyjnych niż http.sys Manager i udostępnia kilka więcej opcji konfiguracji, ale w przeciwnym razie zapewnia funkcje podobne. Nie można utworzyć nową listę zaufania certyfikatów (CTL), ale można przypisać istniejące.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Przykładowa aplikacja dla tego artykułu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [Sterownik HTTP.sys kodu źródłowego](https://github.com/aspnet/HttpSysServer/)
* [Hosting](xref:fundamentals/hosting)
