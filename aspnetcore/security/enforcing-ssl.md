---
title: Wymuszanie protokołu HTTPS w platformy ASP.NET Core
author: rick-anderson
description: Pokazuje, jak będą musieli HTTPS/TLS w ASP.NET Core aplikacji sieci web.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: b324dbcd6d28c1a8505f96da333874728e2e6a18
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a>Wymuszanie protokołu HTTPS w platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten dokument zawiera jak:

- Wymagać protokołu HTTPS dla wszystkich żądań.
- Przekieruj żądania HTTP, HTTPS.

> [!WARNING]
> Czy **nie** użyj `RequireHttpsAttribute` na interfejsów API sieci Web, czy odbierać poufne informacje. `RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP, HTTPS. Klientów interfejsu API nie może zrozumieć lub przestrzegać przekierowania z protokołu HTTP, HTTPS. Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP. Interfejsy API sieci Web powinien:
>
>* Nie nasłuchiwania protokołu HTTP.
>* Zamknięcie połączenia z kodem stanu 400 (nieprawidłowe żądanie), a nie obsługiwać żądania.

<a name="require"></a>
## <a name="require-https"></a>Wymagać protokołu HTTPS

::: moniker range=">= aspnetcore-2.1"
Firma Microsoft zaleca wszystkie platformy ASP.NET Core wywołania aplikacji sieci web `UseHttpsRedirection` Aby przekierować wszystkie żądania HTTP, HTTPS. Jeśli `UseHsts` jest wywoływana w aplikacji, musi zostać wywołana przed `UseHttpsRedirection`.

Poniższy kod wywołania `UseHttpsRedirection` w `Startup` klasy:

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]


Następujący kod:

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

* Zestawy `RedirectStatusCode`.
* Ustawia HTTPS port 5001.

::: moniker-end


::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) jest używana, aby wymagać protokołu HTTPS. `[RequireHttpsAttribute]` można dekoracji kontrolerów lub metody lub mogą być stosowane globalnie. Aby zastosować atrybut globalny, Dodaj następujący kod do `ConfigureServices` w `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Poprzedni wyróżniony kod wymaga wszystkie żądania przy użyciu `HTTPS`; w związku z tym żądania HTTP są ignorowane. Następujący wyróżniony kod przekierowuje żądania HTTP, https:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Aby uzyskać więcej informacji, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting).

Globalny wymagających protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa. Stosowanie `[RequireHttps]` atrybut, aby wszystkie kontrolery/Razor strony nie jest uznawane za należycie zabezpieczone globalnie wymagających protokołu HTTPS. Nie można zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Protokół zabezpieczeń Strict transportu HTTP (HSTS)

Na [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [zabezpieczeń transportu HTTP ograniczeniami (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) jest ulepszeniem zabezpieczeń opcjonalnych w określonym przez aplikację sieci web przy użyciu nagłówka odpowiedzi specjalnych. Kiedy obsługiwanej przeglądarki odbiera ten nagłówek przeglądarka uniemożliwi komunikację z są wysyłane za pośrednictwem protokołu HTTP z określoną domeną i zamiast tego wyśle cała komunikacja za pośrednictwem protokołu HTTPS. Uniemożliwia także kliknij HTTPS za pomocą monity w przeglądarkach.

ASP.NET Core 2.1 preview1 lub później implementuje HSTS z `UseHsts` — metoda rozszerzenia. Poniższy kod wywołania `UseHsts` gdy aplikacja nie jest w [tryb programowania](xref:fundamentals/environments):

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` nie jest zalecane w rozwoju ponieważ nagłówek HSTS jest wysoce cachable przez przeglądarki. Domyślnie UseHsts wyklucza adresu lokalnego sprzężenia zwrotnego.

Następujący kod:

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Ustawia parametr wstępnego ładowania nagłówka Strict TLS. Wstępnego ładowania nie jest częścią [specyfikacji RFC HSTS](https://tools.ietf.org/html/rfc6797), ale nie jest obsługiwana przez przeglądarki sieci web wstępnie HSTS witryn na nowej instalacji. Zobacz [ https://hstspreload.org/ ](https://hstspreload.org/) Aby uzyskać więcej informacji.
* Umożliwia [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), która ma zastosowanie zasad HSTS poddomen hosta. 
* Jawnie Ustawia maksymalny wiek parametr nagłówka TLS Strict do 60 dni. Jeśli nie zostanie ustawiona wartość domyślna to 30 dni. Zobacz [dyrektywy maksymalny wiek](https://tools.ietf.org/html/rfc6797#section-6.1.1) Aby uzyskać więcej informacji.
* Dodaje `example.com` do listy hostów, które mają zostać wykluczone.

`UseHsts` nie obejmuje następujące hosty sprzężenie zwrotne:

* `localhost` Adres sprzężenia zwrotnego protokołu IPv4.
* `127.0.0.1` Adres sprzężenia zwrotnego protokołu IPv4.
* `[::1]` Adres sprzężenia zwrotnego protokołu IPv6.

Powyższy przykład przedstawia sposób dodawania dodatkowych hostów.
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Zrezygnować z protokołu HTTPS na tworzenie projektu

Program ASP.NET Core 2.1 i nowsze szablonów aplikacji sieci web (od programu Visual Studio lub wiersza polecenia platformy dotnet) umożliwiają [przekierowania HTTPS](#require) i [HSTS](#hsts). W przypadku wdrożeń, które nie wymagają protokołu HTTPS użytkownik może zrezygnować z protokołu HTTPS. Na przykład niektóre usługi wewnętrznej bazy danych, gdy HTTPS jest obsługiwane zewnętrznie na krawędzi, przy użyciu protokołu HTTPS w każdym węźle nie jest wymagana.

Aby zrezygnować z protokołu HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Usuń zaznaczenie pola wyboru **Konfiguruj na potrzeby protokołu HTTPS** wyboru.

![Diagram jednostek](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

Użyj `--no-https` opcji. Na przykład

```cli
dotnet new razor --no-https
```

------

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Jak skonfigurować certyfikat dewelopera dla Docker

Zobacz [ten problem GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
