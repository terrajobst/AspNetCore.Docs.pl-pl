---
title: Implementacja serwera sieci web WebListener w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat WebListener, serwer sieci web platformy ASP.NET Core na Windows, który może służyć do bezpośredniego połączenia z Internetu bez usług IIS.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 5d72672cc48243f8ee17df615e3379143ed868f6
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206445"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>Implementacja serwera sieci web WebListener w programie ASP.NET Core

Przez [Tom Dykstra](https://github.com/tdykstra) i [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Ten temat dotyczy tylko programu ASP.NET Core 1.x. W programie ASP.NET Core 2.0 nosi nazwę WebListener [HTTP.sys](httpsys.md).

Jest WebListener [serwera sieci web dla platformy ASP.NET Core](index.md) , które jest uruchamiane tylko na Windows. Jest on oparty na [sterownik trybu jądra Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener stanowi alternatywę [Kestrel](kestrel.md) mogą służyć do bezpośredniego połączenia z Internetem bez polegania na serwerze IIS jako serwera zwrotnego serwera proxy. W rzeczywistości **WebListener nie można używać z usług IIS Express, lub usług IIS, ponieważ nie jest zgodny z [modułu ASP.NET Core](aspnet-core-module.md).**

Mimo że WebListener została opracowana dla platformy ASP.NET Core, można bezpośrednio w dowolnej aplikacji platformy .NET Core lub .NET Framework za pomocą [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pakietu NuGet.

WebListener obsługuje następujące funkcje:

- [Uwierzytelnianie Windows](xref:security/authentication/windowsauth)
- Współużytkowanie portów
- Protokołu HTTPS z SNI
- Protokołu HTTP/2 za pośrednictwem protokołu TLS (system Windows 10)
- Przekazywanie pliku bezpośrednie
- Buforowanie odpowiedzi
- Gniazda Websocket (system Windows 8)

Obsługiwane wersje Windows:

- Windows 7 i Windows Server 2008 R2 lub nowszy

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Kiedy należy używać WebListener

WebListener jest przydatne w przypadku wdrożeń, w których należy udostępnić server bezpośrednio do Internetu, nie za pomocą usług IIS.

![Weblistener komunikuje się bezpośrednio z Internetu](weblistener/_static/weblistener-to-internet.png)

Ponieważ jest on oparty na Http.Sys, WebListener nie wymaga zwrotnego serwera proxy w celu ochrony przed atakami. Sterownik Http.Sys to dojrzała technologia, która chroni przed wiele rodzajów ataków i udostępnia niezawodności, bezpieczeństwa i skalowalności serwera sieci web w pełni funkcjonalne. SAM działa jako odbiornik HTTP w pliku HTTP.sys.

WebListener jest również dobrym wyborem dla wewnętrznego wdrożeń, gdy będą potrzebne funkcje, które zapewnia, że nie można dostać się przy użyciu Kestrel.

![Weblistener komunikuje się bezpośrednio z sieci wewnętrznej](weblistener/_static/weblistener-to-internal.png)

## <a name="kernel-mode-authentication-with-kerberos"></a>Uwierzytelnianie trybu jądra przy użyciu protokołu Kerberos

WebListener delegatów, aby uwierzytelnianie trybu jądra za pomocą protokołu uwierzytelniania Kerberos. Uwierzytelnianie w trybie użytkownika nie jest obsługiwana przy użyciu protokołu Kerberos i WebListener. Konto komputera należy używany do odszyfrowywania tokenu/biletu Kerberos uzyskany z usługi Active Directory i przesyłany dalej przez klienta do serwera w celu uwierzytelnienia użytkownika. Rejestrowanie głównej nazwy usługi (SPN) dla hosta, a nie użytkownika aplikacji.

## <a name="how-to-use-weblistener"></a>Jak używać WebListener

Poniżej przedstawiono omówienie zadań instalacji systemu operacyjnego hosta i aplikacji ASP.NET Core.

### <a name="configure-windows-server"></a>Konfigurowanie systemu Windows Server

* Zainstaluj wersję platformy .NET, wymaganych przez aplikację, takie jak [platformy .NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) lub .NET Framework 4.5.1.

* Preregister przedrostki adresów URL, aby powiązać WebListener i konfigurowanie certyfikatów protokołu SSL

   Nie preregister przedrostki adresów URL w Windows, musisz uruchomić aplikację z uprawnieniami administratora. Jedynym wyjątkiem jest, jeżeli powiążesz do hosta lokalnego przy użyciu protokołu HTTP (a nie HTTPS), numer portu jest większa niż 1024; w takim przypadku uprawnienia administratora nie są wymagane.

   Aby uzyskać więcej informacji, zobacz [preregister prefiksy i konfigurowanie protokołu SSL](#preregister-url-prefixes-and-configure-ssl) w dalszej części tego artykułu.

* Otwórz porty zapory muszą zezwalać na ruch do osiągnięcia WebListener.

   Możesz użyć netsh.exe lub [poleceń cmdlet programu PowerShell](https://technet.microsoft.com/library/jj554906).

Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Konfigurowanie aplikacji platformy ASP.NET Core

* Zainstaluj pakiet NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Spowoduje to również zainstalowanie [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) jako zależność.

* Wywołanie `UseWebListener` metody rozszerzenia w [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) w Twojej `Main` metody, określając wszystkie WebListener [opcje](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) i [ustawienia](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) potrzebne , jak pokazano w poniższym przykładzie:

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Konfigurowanie adresów URL i portów do nasłuchiwania 

  Domyślnie platforma ASP.NET Core wiąże `http://localhost:5000`. Aby skonfigurować przedrostki adresów URL i portów, można użyć `UseURLs` metody rozszerzenia `urls` argument wiersza polecenia lub systemu konfiguracji platformy ASP.NET Core. Aby uzyskać więcej informacji zobacz [hosta w ASP.NET Core(xref:fundamentals/host/index).

  Sieci Web używa odbiornika [formaty ciągu prefiks Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). Nie ma żadnych wymagań formatu ciągu prefiksu, które są specyficzne dla WebListener.

  > [!WARNING]
  > Powiązania najwyższego poziomu symbolu wieloznacznego (`http://*:80/` i `http://+:80`) powinien **nie** można użyć. Powiązania najwyższego poziomu symboli wieloznacznych otworzyć aplikację w celu luk w zabezpieczeniach. Dotyczy to silnych i słabych symboli wieloznacznych. Użyj nazwy hostów jawne, a nie symboli wieloznacznych. Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.mysub.com`) nie ma to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony). Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.

  > [!NOTE]
  > Upewnij się, że ten sam prefiks ciągi w `UseUrls` , preregister na serwerze. 

* Upewnij się, że aplikacja nie jest skonfigurowana do uruchamiania usług IIS lub IIS Express.

  W programie Visual Studio domyślnym profilem uruchamiania jest dla usług IIS Express.  Aby uruchomić projekt jako aplikację konsolową w języku należy ręcznie zmienić wybranego profilu, jak pokazano na poniższym zrzucie ekranu.

  ![Wybierz profil aplikacji konsoli](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Jak używać WebListener poza platformy ASP.NET Core

* Zainstaluj [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) pakietu NuGet.

* [Przedrostki adresów URL, aby powiązać WebListener i skonfigurować certyfikaty SSL preregister](#preregister-url-prefixes-and-configure-ssl) podobnie jak w przypadku użycia w programie ASP.NET Core.

Dostępne są także [ustawienia rejestru Http.Sys](https://support.microsoft.com/kb/820129).


Poniżej przedstawiono przykładowy kod, który demonstruje użycie WebListener poza platformy ASP.NET Core:

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Preregister przedrostki adresów URL i konfigurowanie protokołu SSL

Usługi IIS i WebListener polegają na podstawowy sterownik trybu jądra Http.Sys do nasłuchiwania żądań i wstępne przetwarzanie. W usługach IIS interfejs użytkownika zarządzania zapewnia stosunkowo prosty sposób skonfigurować wszystko. Jednak jeśli używasz WebListener należy skonfigurować serwer HTTP.sys tak, samodzielnie. Wbudowane narzędzie robić, który jest netsh.exe. 

Najbardziej typowych zadań, należy użyć netsh.exe dla są rezerwowanie prefiksów adresu URL i przypisywania certyfikatów SSL.

NetSh.exe nie jest to proste narzędzie do użycia dla początkujących. Poniższy przykład przedstawia bez minimalnej liczbie niezbędnej do zarezerwowania przedrostki adresów URL dla portów 80 i 443:

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

Poniższy przykład pokazuje, jak przypisać certyfikat SSL:

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

Oto oficjalna dokumentacja:

* [Polecenia Netsh dla Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [Ciągi UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Następujące zasoby zawierają szczegółowe instrukcje dla kilku scenariuszy. Artykuły, które odwołują się do `HttpListener` jednakowo do zastosowania `WebListener`, ponieważ także są oparte na Http.Sys.

* [Instrukcje: konfigurowanie portu z certyfikatem SSL](/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [Komunikacja HTTPS - HttpListener i opartych na Hosting certyfikatu klienta](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) to jest blogu innych firm i jest dość stara, ale nadal zawiera przydatne informacje.
* [Instrukcje: HttpListener za pomocą wskazówki lub serwer Http niezarządzanego kodu (C++) jako serwera prostego protokołu SSL](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) to zbyt blogu starszy przydatnymi informacjami.
* [Jak skonfigurować WebListener podstawowych platformy .NET, przy użyciu protokołu SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

Poniżej przedstawiono niektóre narzędzia innych producentów, które mogą być łatwiejszy w obsłudze niż netsh.exe wiersza polecenia. Nie są one udostępniane przez lub zatwierdzone przez firmę Microsoft. Narzędzia Uruchom jako administrator domyślnie, ponieważ netsh.exe sam wymaga uprawnień administratora.

* [Sterownik HTTP.sys Menedżera](http://httpsysmanager.codeplex.com/) udostępnia interfejs wielokrotnego użytku listy i konfigurowania certyfikatów SSL i opcji, rezerwacje prefiksu i listy zaufanych certyfikatów. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) pozwala wyświetlić lub skonfigurować certyfikaty SSL i przedrostki adresów URL. Interfejs użytkownika jest bardziej precyzyjnych niż http.sys Manager i udostępnia kilka więcej opcji konfiguracji, ale w przeciwnym razie zapewnia podobne funkcje. Nie można utworzyć nowej listy zaufania certyfikatów (CTL), ale można przypisać już istniejące.

Podczas generowania certyfikatów SSL z podpisem własnym, firma Microsoft udostępnia narzędzia wiersza polecenia: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) i polecenia cmdlet programu PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Dostępne są również narzędzia interfejsu użytkownika innych firm, które ułatwiają wygenerować certyfikaty SSL z podpisem własnym:

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Interfejs użytkownika narzędzia MakeCert](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Przykładowa aplikacja w tym artykule](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [Kod źródłowy WebListener](https://github.com/aspnet/HttpSysServer/)
* [Hosting](xref:fundamentals/host/index)
