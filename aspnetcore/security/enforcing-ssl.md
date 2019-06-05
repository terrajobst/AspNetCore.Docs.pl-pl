---
title: Wymuszanie protokołu HTTPS w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wymaganie protokołu HTTPS/TLS w aplikacji sieci web platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 8d48877153d6d75348e29299c669125904236de8
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692593"
---
# <a name="enforce-https-in-aspnet-core"></a>Wymuszanie protokołu HTTPS w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tym dokumencie przedstawiono sposób:

* Wymaganie protokołu HTTPS dla wszystkich żądań.
* Przekieruj wszystkie żądania HTTP do HTTPS.

Nie interfejsu API mogą uniemożliwić wysyłanie danych poufnych na pierwsze żądanie klienta.

> [!WARNING]
> Czy **nie** użyj [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) na interfejsy API sieci Web, która odbierać poufne informacje. `RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP do HTTPS. Klienci interfejsu API może zrozumieć lub nie przestrzegają przekierowuje z protokołu HTTP do HTTPS. Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP. Interfejsy API sieci Web powinien:
>
> * Nasłuchuj od protokołu HTTP.
> * Zamknij połączenie z kodem stanu 400 (złe żądanie), a nie obsłużyć żądania.

## <a name="require-https"></a>Wymaganie protokołu HTTPS

::: moniker range=">= aspnetcore-2.1"

Firma Microsoft zaleca tej platformy ASP.NET Core w środowisku produkcyjnym wywołanie aplikacji sieci web:

* Oprogramowanie pośredniczące przekierowania protokołu HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) do przekierowywania żądań HTTP do HTTPS.
* Oprogramowanie pośredniczące HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) do wysyłania nagłówków interfejsów HTTP Strict transportu zabezpieczeń protokołu (HSTS) do klientów.

> [!NOTE]
> Aplikacje wdrożone w konfiguracji zwrotny serwer proxy zezwala na serwer proxy, aby obsłużyć zabezpieczenia połączeń (HTTPS). Jeśli serwer proxy obsługuje także przekierowania protokołu HTTPS, nie ma potrzeby używania oprogramowania pośredniczącego przekierowania protokołu HTTPS. Jeśli serwer proxy obsługuje także zapisywanie nagłówki HSTS (na przykład [HSTS natywnej obsługi w programie IIS 10.0 (1709) lub nowszym](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), oprogramowanie pośredniczące HSTS nie jest wymagane przez aplikację. Aby uzyskać więcej informacji, zobacz [rezygnacji z protokołu HTTPS/HSTS przy tworzeniu projektu](#opt-out-of-httpshsts-on-project-creation).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

Poniższy kod wywoła `UseHttpsRedirection` w `Startup` klasy:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Wyróżniony kod:

* Używa domyślnej [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Używa domyślnej [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), chyba że zostaną zastąpione `ASPNETCORE_HTTPS_PORT` zmiennej środowiskowej lub [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

Zalecamy używanie przekierowania tymczasowego, a nie stałych przekierowania. Buforowanie linków może spowodować niestabilność zachowanie w środowiskach programistycznych. Jeśli wolisz wysłać kod stanu trwałe przekierowanie, gdy aplikacja jest w środowisku deweloperskim bez, zobacz [Konfigurowanie przekierowania stałe w środowisku produkcyjnym](#configure-permanent-redirects-in-production) sekcji. Firma Microsoft zaleca używanie [HSTS](#http-strict-transport-security-protocol-hsts) celu sygnalizowania, że dla klientów, którzy tylko zabezpieczyć zasób w aplikacji (tylko w środowisku produkcyjnym) powinny być wysyłane żądania.

### <a name="port-configuration"></a>Konfiguracja portu

Port musi być dostępny dla oprogramowania pośredniczącego do przekierowania niezabezpieczone żądania HTTPS. Jeśli port nie jest dostępna:

* Przekierowania protokołu HTTPS nie zachodzi.
* Oprogramowanie pośredniczące rejestruje ostrzeżenie "Nie można określić port https dla przekierowania."

Określ port HTTPS przy użyciu dowolnej z następujących metod:

* Ustaw [HttpsRedirectionOptions.HttpsPort](#options).
* Ustaw `ASPNETCORE_HTTPS_PORT` zmiennej środowiskowej lub [ustawienia konfiguracji hosta sieci Web https_port](xref:fundamentals/host/web-host#https-port):

  **Klucz**: `https_port`  
  **Typ**: *ciągu*  
  **Domyślne**: Nie ustawiono wartość domyślną.  
  **Można ustawić przy użyciu**: `UseSetting`  
  **Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT` (Prefiks jest `ASPNETCORE_` przy użyciu [hosta sieci Web](xref:fundamentals/host/web-host).)

  Podczas konfigurowania <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> w `Program`:

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* Wskazać port z bezpiecznego schematu przy użyciu `ASPNETCORE_URLS` zmiennej środowiskowej. Zmienna środowiskowa konfiguruje serwer. Oprogramowanie pośredniczące pośrednio odnajduje portu HTTPS za pośrednictwem <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>. To podejście nie działa w przypadku wdrożeń zwrotnego serwera proxy.
* W trakcie opracowywania, należy ustawić adres URL protokołu HTTPS *launchsettings.json*. Włączanie obsługi protokołu HTTPS, gdy usługi IIS Express jest używana.
* Skonfiguruj punkt końcowy HTTPS URL we wdrożeniu usługi edge publicznego [Kestrel](xref:fundamentals/servers/kestrel) serwera lub [HTTP.sys](xref:fundamentals/servers/httpsys) serwera. Tylko **jeden port HTTPS** jest używany przez aplikację. Oprogramowanie pośredniczące odnajduje portów za pomocą <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.

> [!NOTE]
> Po uruchomieniu aplikacji w ramach konfiguracji zwrotny serwer proxy <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> jest niedostępna. Ustaw port przy użyciu jednej z innych metod opisanych w tej sekcji.

Podczas Kestrel lub sterownik HTTP.sys jest używany jako serwer graniczny publicznymi, Kestrel lub sterownik HTTP.sys muszą być skonfigurowane do nasłuchiwania na obu:

* Bezpieczny port, na którym klient zostanie przekierowany (zazwyczaj port 443 w środowisku produkcyjnym i 5001 w trakcie programowania).
* Port niezabezpieczone (zwykle 80 w środowisku produkcyjnym) do 5000 w trakcie opracowywania.

Niebezpieczne port musi być dostępny dla klientów w kolejności dla aplikacji do odbierania niezabezpieczone żądania i przekierowywania klientów do bezpiecznego portu.

Aby uzyskać więcej informacji, zobacz [konfiguracji punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) lub <xref:fundamentals/servers/httpsys>.

### <a name="deployment-scenarios"></a>Scenariusze wdrażania

Wszystkie zapory między klientem i serwerem musi mieć również komunikacji otwarte porty niezbędne dla ruchu.

Jeśli żądania są przekazywane w konfiguracji zwrotny serwer proxy, należy użyć [przekazywane oprogramowania pośredniczącego nagłówki](xref:host-and-deploy/proxy-load-balancer) przed wywołaniem oprogramowania pośredniczącego przekierowania protokołu HTTPS. Aktualizacje oprogramowania pośredniczącego nagłówki przekazywane `Request.Scheme`przy użyciu `X-Forwarded-Proto` nagłówka. Zezwala oprogramowania pośredniczącego przekierowania URI i innych zasad zabezpieczeń, do prawidłowego działania. Po przekazywane oprogramowania pośredniczącego nagłówków nie jest używany, aplikacji wewnętrznej bazy danych może być wyświetlony prawidłowy schemat i nie pozostać w pętli przekierowania. Typowe komunikat o błędzie użytkownika końcowego jest wystąpiły za dużo przekierowań.

W przypadku wdrażania w usłudze Azure App Service, postępuj zgodnie ze wskazówkami w [samouczka: Powiązania istniejącego niestandardowego certyfikatu SSL w usłudze Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).

### <a name="options"></a>Opcje

Następujący wyróżniony kod wywołuje [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) skonfigurować opcje oprogramowania pośredniczącego:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Wywoływanie `AddHttpsRedirection` tylko jest konieczna zmiana wartości `HttpsPort` lub `RedirectStatusCode`.

Wyróżniony kod:

* Zestawy [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) do <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, która jest wartością domyślną. Użyj pola <xref:Microsoft.AspNetCore.Http.StatusCodes> klasy do przypisania do `RedirectStatusCode`.
* Ustawia HTTPS port 5001. Wartość domyślna to 443.

#### <a name="configure-permanent-redirects-in-production"></a>Konfigurowanie przekierowania stałe w środowisku produkcyjnym

Oprogramowanie pośredniczące, wartość domyślna to wysyłanie [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) przy użyciu wszystkich przekierowań. Jeśli wolisz wysłać kod stanu trwałe przekierowanie, gdy aplikacja jest w środowisku deweloperskim bez zabalit warunkowego Wyszukaj środowisko — opcje konfiguracji oprogramowania pośredniczącego.

Podczas konfigurowania `IWebHostBuilder` w *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a>Informacje o innym podejściu oprogramowania pośredniczącego przekierowania protokołu HTTPS

Alternatywa dla użycia oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) jest użycie oprogramowanie pośredniczące ponownego zapisywania adresów URL (`AddRedirectToHttps`). `AddRedirectToHttps` Podczas wykonywania przekierowania, można również ustawić kod stanu i port. Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).

Podczas przekierowywania HTTPS bez potrzeby przekierowania dodatkowe reguły, firma Microsoft zaleca używanie oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) opisane w tym temacie.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) służy do wymagania protokołu HTTPS. `[RequireHttpsAttribute]` można dekoracji kontrolery lub metody lub mogą być stosowane globalnie. Aby zastosować atrybut globalnie, Dodaj następujący kod do `ConfigureServices` w `Startup`:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Powyższy kod wyróżniony wymaga, wszystkie żądania przy użyciu `HTTPS`; dlatego żądania HTTP są ignorowane. Następujący wyróżniony kod przekierowuje żądania HTTP do HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting). Oprogramowanie pośredniczące pozwala również aplikację, aby ustawić kod stanu lub kod stanu i port, podczas wykonywania przekierowania.

Globalnie wymagania protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa. Stosowanie `[RequireHttps]` atrybutu, aby wszystkie kontrolery/Razor strony nie jest uważany za bezpieczny globalnie wymagania protokołu HTTPS. Nie może zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a>Protokół zabezpieczeń Strict transportu HTTP (HSTS)

Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [zabezpieczeń transportu HTTP ograniczeniami (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) jest ulepszeniem zabezpieczeń zgłoszenie zgody na uczestnictwo w określonym przez aplikację sieci web przy użyciu nagłówka odpowiedzi. Gdy [przeglądarki obsługującej HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) odbiera tego nagłówka:

* Przeglądarka przechowuje konfigurację dla domeny, która uniemożliwia wysłanie Każde powiadomienie za pośrednictwem protokołu HTTP. Przeglądarka wymusza całą komunikację za pośrednictwem protokołu HTTPS.
* Przeglądarka uniemożliwia użytkownikowi za pomocą certyfikatów niezaufanych lub jest nieprawidłowy. Przeglądarka wyłącza monity, które umożliwiają użytkownikowi tymczasowo ufać takiego certyfikatu.

Ponieważ HSTS jest wymuszana przez klienta ma pewne ograniczenia:

* Klient musi obsługiwać HSTS.
* HSTS wymaga co najmniej jeden pomyślne żądanie HTTPS do ustanowienia zasad HSTS.
* Aplikacja musi sprawdzić każdego żądania HTTP i przekierowania lub odrzucanie żądania HTTP.

Platforma ASP.NET Core 2.1 lub nowszej implementuje HSTS z `UseHsts` — metoda rozszerzenia. Poniższy kod wywoła `UseHsts` gdy aplikacja nie jest [trybu opracowywania](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` nie jest zalecane w rozwoju, ponieważ ustawienia HSTS są wysoce podlega buforowaniu przez przeglądarki. Domyślnie `UseHsts` nie obejmuje adresu lokalnego sprzężenia zwrotnego.

W środowiskach produkcyjnych implementacji protokołu HTTPS, po raz pierwszy, ustaw początkowy [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) małej wartości przy użyciu jednej z <xref:System.TimeSpan> metody. Ustaw wartość od kilku godzin do nie więcej niż jednego dziennie w przypadku konieczności przywrócenia infrastruktury protokołu HTTPS do protokołu HTTP. Po masz pewność, że trwałości Konfiguracja protokołu HTTPS, należy zwiększyć wartość max-age HSTS; często używane wartości jest jeden rok.

Poniższy kod:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Ustawia wstępnego ładowania parametr nagłówka zabezpieczeń w przypadku transportu Strict. Wstępnego ładowania nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale jest obsługiwane przez przeglądarki sieci web do wstępnego witryn HSTS na zainstalować od nowa. Zobacz [ https://hstspreload.org/ ](https://hstspreload.org/) Aby uzyskać więcej informacji.
* Włącza [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), co ma zastosowanie zasad HSTS poddomen hosta.
* Jawnie Ustawia parametr max-age nagłówka zabezpieczeń w przypadku transportu Strict do 60 dni. Jeśli nie zostanie ustawiona wartość domyślna to 30 dni. Zobacz [dyrektywy max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) Aby uzyskać więcej informacji.
* Dodaje `example.com` do listy hostów, które mają zostać wykluczone.

`UseHsts` nie obejmuje następujące hosty sprzężenia zwrotnego:

* `localhost` : Adres IPv4 sprzężenia zwrotnego.
* `127.0.0.1` : Adres IPv4 sprzężenia zwrotnego.
* `[::1]` : Adres sprzężenia zwrotnego protokołu IPv6.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a>Zrezygnować z protokołu HTTPS/HSTS przy tworzeniu projektu

W niektórych scenariuszach usługi zaplecza gdzie zabezpieczenia połączeń jest obsługiwane na urządzeniach brzegowych publicznych sieci konfigurowanie zabezpieczenia połączeń w każdym węźle nie jest wymagana. Wygenerowany na podstawie szablonów w programie Visual Studio lub z aplikacji sieci Web [dotnet nowe](/dotnet/core/tools/dotnet-new) Włącz polecenie [przekierowania protokołu HTTPS](#require-https) i [HSTS](#http-strict-transport-security-protocol-hsts). W przypadku wdrożeń, które nie wymagają tych scenariuszy użytkownik może zrezygnować z HTTPS/HSTS po utworzeniu aplikacji z szablonu.

Aby zrezygnować z protokołu HTTPS/HSTS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Usuń zaznaczenie pola wyboru **Konfigurowanie protokołu HTTPS** pole wyboru.

![Nowe aplikacje sieci Web programu ASP.NET Core Wyświetla okno dialogowe Konfigurowanie usunięto zaznaczenie pola wyboru protokołu HTTPS.](enforcing-ssl/_static/out.png)

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

Użyj `--no-https` opcji. Na przykład

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a>Certyfikat jako zaufany platformy ASP.NET Core HTTPS programowanie na Windows i macOS

Zestaw .NET core SDK zawiera certyfikatu deweloperskiego protokołu HTTPS. Certyfikat jest instalowany jako część pierwszego uruchomienia komputera. Na przykład `dotnet --info` generuje dane wyjściowe podobne do następującego:

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

Instalowanie programu .NET Core SDK instaluje certyfikatu deweloperskiego platformy ASP.NET Core HTTPS do magazynu certyfikatów użytkownika lokalnego. Certyfikat został zainstalowany, ale nie jest zaufany. Aby zaufania certyfikatu wykonaj jednorazowy krok, aby uruchomić program dotnet `dev-certs` narzędzie:

```console
dotnet dev-certs https --trust
```

Następujące polecenie zapewnia pomoc w `dev-certs` narzędzie:

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Jak skonfigurować certyfikat dewelopera dla programu Docker

Zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6199).

::: moniker-end

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a>Ufać certyfikatowi protokołu HTTPS z podsystemu Windows dla systemu Linux

Podsystem Windows dla systemu Linux (WSL) generuje certyfikatu z podpisem własnym protokołu HTTPS. Aby skonfigurować Magazyn certyfikatów Windows ufać certyfikatowi WSL:

* Uruchom następujące polecenie, aby wyeksportować certyfikat wygenerowany WSL: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`
* W oknie WSL uruchom następujące polecenie: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`

  Poprzednie polecenie ustawia zmienne środowiskowe, więc zaufanego certyfikatu Windows korzysta z systemu Linux.

## <a name="additional-information"></a>Dodatkowe informacje

* <xref:host-and-deploy/proxy-load-balancer>
* [Host platformy ASP.NET Core w systemie Linux z Apache: Konfiguracja protokołu HTTPS](xref:host-and-deploy/linux-apache#https-configuration)
* [Host platformy ASP.NET Core w systemie Linux przy użyciu serwera Nginx: Konfiguracja protokołu HTTPS](xref:host-and-deploy/linux-nginx#https-configuration)
* [Jak skonfigurować protokół SSL na serwerze IIS](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [Obsługa przeglądarek OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
