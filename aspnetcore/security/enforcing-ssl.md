---
title: Wymuszanie protokołu HTTPS w programie ASP.NET Core
author: rick-anderson
description: Pokazuje, jak HTTPS/TLS w programie ASP.NET Core wymagają aplikacji sieci web.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 838cd00545f36736461616f806942249aaf6eee0
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893181"
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

<a name="require"></a>
## <a name="require-https"></a>Wymaganie protokołu HTTPS

::: moniker range=">= aspnetcore-2.1"

Firma Microsoft zaleca wszystkich produkcyjnym platformy ASP.NET Core wywołanie aplikacji sieci web:

* Oprogramowanie pośredniczące przekierowania protokołu HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) aby przekierować wszystkie żądania HTTP do HTTPS.
* [UseHsts](#hsts), protokół zabezpieczeń Strict transportu HTTP (HSTS).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

Poniższy kod wywoła `UseHttpsRedirection` w `Startup` klasy:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Wyróżniony kod:

* Używa domyślnej [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).
* Używa domyślnej [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), chyba że zostaną zastąpione `ASPNETCORE_HTTPS_PORT` zmiennej środowiskowej lub [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

> [!WARNING] 
>Port musi być dostępny dla oprogramowania pośredniczącego do przekierowania protokołu HTTPS. Jeśli port nie jest dostępny, przekierowania protokołu HTTPS nie występuje. HTTPS port można określić przez jedną z następujących ustawień:
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* `ASPNETCORE_HTTPS_PORT` Zmiennej środowiskowej. 
>* Na etapie opracowywania adresu url HTTPS w *launchsettings.json*. 
>* Adres url HTTPS, skonfigurować bezpośrednio na Kestrel lub słowa kluczowego HttpSys. 

Następujący wyróżniony kod wywołuje [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) skonfigurować opcje oprogramowania pośredniczącego:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Wywoływanie `AddHttpsRedirection` tylko jest konieczna zmiana wartości ` HttpsPort` lub ` RedirectStatusCode`;

Wyróżniony kod:

* Zestawy [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) do `Status307TemporaryRedirect`, która jest wartością domyślną.
* Ustawia HTTPS port 5001. Wartość domyślna to 443.

Automatycznie Ustaw port przez następujących mechanizmów:

* Oprogramowanie pośredniczące może odnajdować portów za pomocą [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) gdy są spełnione następujące warunki:

   * Kestrel lub sterownik HTTP.sys jest używana bezpośrednio z punktów końcowych HTTPS (dotyczy także uruchamianie aplikacji za pomocą debugera programu Visual Studio Code firmy).
   * Tylko **jeden port HTTPS** jest używany przez aplikację.

* Program Visual Studio jest używany:
   * Usługi IIS Express ma obsługujące protokół HTTPS.
   * *launchSettings.json* ustawia `sslPort` dla usług IIS Express.

> [!NOTE]
> Gdy aplikacja jest uruchamiana za zwrotny serwer proxy (na przykład, IIS, usługi IIS Express), `IServerAddressesFeature` jest niedostępna. Numer portu musi być skonfigurowany ręcznie. Jeśli port nie jest ustawiony, przekierowanie nie nastąpi żądań.

Można skonfigurować port, ustawiając [ustawienia konfiguracji hosta sieci Web https_port](xref:fundamentals/host/web-host#https-port):

**Klucz**: https_port  
**Typ**: *ciągu*  
**Domyślne**: nie ustawiono wartość domyślną.  
**Można ustawić przy użyciu**: `UseSetting`  
**Zmienna środowiskowa**: `<PREFIX_>HTTPS_PORT` (prefiks jest `ASPNETCORE_` przy użyciu hosta sieci Web.)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> Port można skonfigurować pośrednio przez ustawienie adresu URL z `ASPNETCORE_URLS` zmiennej środowiskowej. Zmienna środowiskowa konfiguruje serwer, a następnie oprogramowanie pośredniczące pośrednio odnajduje portu HTTPS za pośrednictwem `IServerAddressesFeature`.

Jeśli port nie jest ustawiony:

* Żądania nie są przekierowywane.
* Oprogramowanie pośredniczące rejestruje ostrzeżenie "Nie można określić port https dla przekierowania."

> [!NOTE]
> Alternatywa dla użycia oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) jest użycie oprogramowanie pośredniczące ponownego zapisywania adresów URL (`AddRedirectToHttps`). `AddRedirectToHttps` Podczas wykonywania przekierowania, można również ustawić kod stanu i port. Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting).
>
> Podczas przekierowywania HTTPS bez potrzeby przekierowania dodatkowe reguły, firma Microsoft zaleca używanie oprogramowania pośredniczącego przekierowania protokołu HTTPS (`UseHttpsRedirection`) opisane w tym temacie.

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

<a name="hsts"></a>
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

W środowiskach produkcyjnych, implementacja protokołu HTTPS, po raz pierwszy należy ustawić wartość początkową HSTS małej wartości. Ustaw wartość od kilku godzin do nie więcej niż jednego dziennie w przypadku konieczności przywrócenia infrastruktury protokołu HTTPS do protokołu HTTP. Po masz pewność, że trwałości Konfiguracja protokołu HTTPS, należy zwiększyć wartość max-age HSTS; często używane wartości jest jeden rok. 

Poniższy kod:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Ustawia wstępnego ładowania parametr nagłówka zabezpieczeń w przypadku transportu Strict. Wstępnego ładowania nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale jest obsługiwane przez przeglądarki sieci web do wstępnego witryn HSTS na zainstalować od nowa. Zobacz [ https://hstspreload.org/ ](https://hstspreload.org/) Aby uzyskać więcej informacji.
* Włącza [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), co ma zastosowanie zasad HSTS poddomen hosta. 
* Jawnie Ustawia parametr max-age nagłówka zabezpieczeń w przypadku transportu Strict do 60 dni. Jeśli nie zostanie ustawiona wartość domyślna to 30 dni. Zobacz [dyrektywy max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) Aby uzyskać więcej informacji.
* Dodaje `example.com` do listy hostów, które mają zostać wykluczone.

`UseHsts` nie obejmuje następujące hosty sprzężenia zwrotnego:

* `localhost` Adres sprzężenia zwrotnego protokołu IPv4.
* `127.0.0.1` Adres sprzężenia zwrotnego protokołu IPv4.
* `[::1]` Adres sprzężenia zwrotnego protokołu IPv6.

Poprzedni przykład pokazuje, jak dodać kolejne hosty.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Zrezygnować z protokołu HTTPS przy tworzeniu projektu

Włącz szablony ASP.NET Core 2.1 lub nowszej sieci web aplikacji (z programu Visual Studio lub wiersza polecenia dotnet) [przekierowania protokołu HTTPS](#require) i [HSTS](#hsts). W przypadku wdrożeń, które nie wymagają protokołu HTTPS użytkownik może zrezygnować protokołu HTTPS. Na przykład niektóre usługi zaplecza, gdzie HTTPS jest obsługiwane zewnętrznie na urządzeniach brzegowych, przy użyciu protokołu HTTPS w każdym węźle nie jest wymagane.

Aby zrezygnować z protokołu HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Usuń zaznaczenie pola wyboru **Konfigurowanie protokołu HTTPS** pola wyboru.

![Diagram jednostek](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

Użyj `--no-https` opcji. Na przykład

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Jak skonfigurować certyfikat dewelopera dla programu Docker

Zobacz [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end

## <a name="additional-information"></a>Dodatkowe informacje

* [Obsługa przeglądarek OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
