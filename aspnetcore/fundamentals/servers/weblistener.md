---
title: WebListener implementacja serwera sieci web platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat WebListener, serwer sieci web platformy ASP.NET Core w systemie Windows, który może służyć do bezpośredniego połączenia z Internetem bez usług IIS.
ms.author: riande
ms.date: 03/13/2018
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 68aea99d6ce6af12655ef5fdb13130e9279e448a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274872"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>WebListener implementacja serwera sieci web platformy ASP.NET Core

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Roaming Krzysztof](https://github.com/Tratcher)

> [!NOTE]
> Ten temat dotyczy tylko platformy ASP.NET Core 1.x. W programie ASP.NET 2.0 Core, nosi nazwę WebListener [HTTP.sys](httpsys.md).

Jest WebListener [serwera sieci web dla platformy ASP.NET Core](index.md) , na którym działają tylko w systemie Windows. Jest zbudowany na [sterownik trybu jądra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener stanowi alternatywę dla [Kestrel](kestrel.md) który może służyć do bezpośredniego połączenia z Internetem bez polegania na serwerze IIS jako serwera zwrotnego serwera proxy. W rzeczywistości **WebListener nie można używać z usług IIS lub usług IIS Express, ponieważ nie jest zgodna z [platformy ASP.NET Core modułu](aspnet-core-module.md).**

Mimo że WebListener został opracowany dla platformy ASP.NET Core, można bezpośrednio w dowolnej aplikacji .NET Core lub .NET Framework za pomocą [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pakietu NuGet.

WebListener obsługuje następujące funkcje:

- [Uwierzytelnianie systemu Windows](xref:security/authentication/windowsauth)
- Udostępnianie portów
- Protokół HTTPS z SNI
- HTTP/2 za pośrednictwem protokołu TLS (z systemem Windows 10)
- Przekazywanie pliku bezpośredniego
- Buforowanie odpowiedzi
- Protokół WebSockets (z systemem Windows 8)

Obsługiwane wersje systemu Windows:

- Windows 7 i Windows Server 2008 R2 lub nowszy

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Kiedy należy używać WebListener

WebListener jest przydatne w przypadku wdrożeń, których należy udostępnić server bezpośrednio do Internetu, nie za pomocą usług IIS.

![Weblistener komunikuje się bezpośrednio z Internetem](weblistener/_static/weblistener-to-internet.png)

Ponieważ jest zbudowany na Http.Sys, WebListener nie wymaga zwrotnego serwera proxy dla ochrony przed atakami. Sterownik Http.Sys jest dojrzała technologia, która chroni przed wiele rodzajów ataków i zapewnia niezawodność, zabezpieczeń i skalowalność serwera sieci web kompletne. SAM działa jako odbiornik HTTP u góry pliku Http.Sys. 

WebListener jest również dobrym rozwiązaniem w przypadku wdrożeń wewnętrznego, gdy będziesz potrzebować funkcje, które zapewnia, że nie można pobrać przy użyciu Kestrel.

![Weblistener komunikuje się bezpośrednio z sieci wewnętrznej](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Jak używać WebListener

Poniżej przedstawiono omówienie zadań instalacji systemu operacyjnego hosta i aplikacji platformy ASP.NET Core.

### <a name="configure-windows-server"></a>Konfigurowanie systemu Windows Server

* Zainstaluj wersję platformy .NET, która wymaga aplikacji, takich jak [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) lub .NET Framework 4.5.1.

* Preregister prefiksy URL, aby powiązać WebListener i skonfigurować certyfikaty SSL

   Jeśli nie preregister prefiksy adresów URL w systemie Windows, należy do uruchamiania aplikacji z uprawnieniami administratora. Jedynym wyjątkiem jest powiązaniu localhost przy użyciu protokołu HTTP (a nie HTTPS) z większą niż 1024; numeru portu w takim przypadku uprawnienia administratora nie są wymagane.

   Aby uzyskać więcej informacji, zobacz [preregister prefiksy i konfigurowanie protokołu SSL](#preregister-url-prefixes-and-configure-ssl) dalszej części tego artykułu.

* Otwórz porty zapory, aby zezwolić na ruch do osiągnięcia WebListener.

   Można użyć netsh.exe lub [poleceń cmdlet programu PowerShell](https://technet.microsoft.com/library/jj554906).

Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Konfigurowanie aplikacji platformy ASP.NET Core

* Zainstaluj pakiet NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Spowoduje to również zainstalowanie [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) jako zależność.

* Wywołanie `UseWebListener` — metoda rozszerzenia na [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) w Twojej `Main` metody, określając żadnych WebListener [opcje](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) i [ustawienia](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) potrzebne , jak pokazano w poniższym przykładzie:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Skonfiguruj adresy URL i portów do nasłuchiwania 

  Domyślnie program ASP.NET Core wiąże `http://localhost:5000`. Aby skonfigurować prefiksy URL i portów, można użyć `UseURLs` — metoda rozszerzenia, `urls` argumentu wiersza polecenia lub systemu konfiguracji platformy ASP.NET Core. Aby uzyskać więcej informacji zobacz [hosta w ASP.NET Core(xref:fundamentals/host/index).

  Sieci Web używa odbiornika [formaty ciągu prefiksu Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). Nie ma żadnych wymagań formatu ciągu prefiksu, które są specyficzne dla WebListener.

  > [!WARNING]
  > Powiązania najwyższego poziomu symbolu wieloznacznego (`http://*:80/` i `http://+:80`) powinien **nie** można użyć. Powiązania wieloznaczny najwyższego poziomu można otwarcie luk w zabezpieczeniach aplikacji. Dotyczy to zarówno silne i słabe symboli wieloznacznych. Użyj nazwy hostów jawne zamiast symboli wieloznacznych. Powiązanie symbolu wieloznacznego domeny podrzędnej (na przykład `*.mysub.com`) nie ma to zagrożenie bezpieczeństwa, jeśli kontrolować domeny nadrzędnej całego (w przeciwieństwie do `*.com`, której występuje). Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.

  > [!NOTE]
  > Upewnij się, że określono tego samego prefiksu ciągi w `UseUrls` który preregister na serwerze. 

* Upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub usług IIS Express.

  W programie Visual Studio domyślnego profilu uruchamiania jest dla usług IIS Express.  Aby uruchomić projekt jako aplikacji konsoli należy ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu.

  ![Wybierz profil aplikacji konsoli](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Jak używać WebListener poza platformy ASP.NET Core

* Zainstaluj [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pakietu NuGet.

* [Preregister prefiksy URL, aby powiązać WebListener i skonfigurować certyfikaty SSL](#preregister-url-prefixes-and-configure-ssl) jak w przypadku użycia w ASP.NET Core.

Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).


Oto przykład kodu, który demonstruje używać WebListener poza platformy ASP.NET Core:

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Preregister prefiksy URL i skonfigurować protokół SSL

Zarówno usług IIS, jak i WebListener polegać na podstawowy sterownik trybu jądra Http.Sys do nasłuchiwania żądań i wstępne przetwarzanie. W usługach IIS interfejs użytkownika zarządzania umożliwia stosunkowo łatwa do skonfigurowania wszystko. Jednak jeśli używasz WebListener należy skonfigurować serwer Http.Sys samodzielnie. Wbudowane narzędzie robić, który jest netsh.exe. 

Najbardziej typowych zadań, należy użyć netsh.exe dla są rezerwowania prefiksy URL i przypisywanie certyfikatów SSL.

NetSh.exe nie jest łatwo narzędzia do użycia dla początkujących użytkowników. W poniższym przykładzie przedstawiono podstawowe czynności potrzebne do zarezerwowania prefiksy URL porty 80 i 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Poniższy przykład przedstawia sposób przypisania certyfikatu SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

Oto oficjalnego dokumentacji:

* [Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [Ciągi UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Następujące zasoby zawierają szczegółowe instrukcje dotyczące kilka scenariuszy. Artykuły, które odwołują się do `HttpListener` stosowane jednakowo do `WebListener`, ponieważ zarówno są oparte na pliku Http.Sys.

* [Instrukcje: konfigurowanie portu z certyfikatem SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Komunikacja HTTPS - HttpListener na podstawie hostingu oraz certyfikatu klienta](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to jest blogu innych firm i jest dość stary, ale nadal zawiera przydatne informacje.
* [Porady: HttpListener za pomocą wskazówki lub serwer Http niezarządzanych kodu (C++) jako serwera SSL proste](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) starsze blogu zawierający przydatne informacje jest zbyt.
* [Jak skonfigurować WebListener Core .NET, przy użyciu protokołu SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Poniżej przedstawiono niektóre narzędzia innych firm, które mogą być łatwiejsze do użycia niż netsh.exe wiersza polecenia. Nie są one udostępniane przez lub podpisane przez firmę Microsoft. Narzędzia Uruchom jako administrator domyślnie, ponieważ netsh.exe sam wymaga uprawnień administratora.

* [Sterownik HTTP.sys Menedżera](http://httpsysmanager.codeplex.com/) udostępnia interfejs do listy i konfigurowania certyfikatów SSL i opcji, zastrzeżenia prefiksu i listy zaufanych certyfikatów. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) pozwala wyświetlić lub skonfigurować certyfikaty SSL i prefiksy URL. Interfejs użytkownika jest bardziej precyzyjnych niż http.sys Manager i udostępnia kilka więcej opcji konfiguracji, ale w przeciwnym razie zapewnia funkcje podobne. Nie można utworzyć nową listę zaufania certyfikatów (CTL), ale można przypisać istniejące.

Podczas generowania certyfikatów SSL z podpisem własnym, firma Microsoft udostępnia narzędzia wiersza polecenia: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) i polecenia cmdlet programu PowerShell [SelfSignedCertificate nowy](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Dostępne są również narzędzia interfejsu użytkownika innych firm, które ułatwiają wygenerować certyfikaty SSL z podpisem własnym:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [MakeCert interfejsu użytkownika](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Przykładowa aplikacja dla tego artykułu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [Kod źródłowy WebListener](https://github.com/aspnet/HttpSysServer/)
* [Hosting](xref:fundamentals/host/index)
