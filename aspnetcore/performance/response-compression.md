---
title: Kompresja odpowiedzi w programie ASP.NET Core
author: guardrex
description: Informacje o kompresji odpowiedzi i sposobie używania oprogramowanie pośredniczące kompresji odpowiedzi w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: performance/response-compression
ms.openlocfilehash: d5d2da3dc0a8a452de97d98161d429389d2f7638
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815614"
---
# <a name="response-compression-in-aspnet-core"></a>Kompresja odpowiedzi w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przepustowość sieci jest ograniczona zasobów. Zazwyczaj zmniejszenie rozmiaru odpowiedzi często znacznie zwiększa szybkość reakcji aplikacji. Jednym ze sposobów, aby zmniejszyć rozmiar ładunku jest kompresji odpowiedzi aplikacji.

## <a name="when-to-use-response-compression-middleware"></a>Kiedy należy używać oprogramowanie pośredniczące kompresji odpowiedzi

Użyj technologii kompresji odpowiedzi na serwerze usług IIS, Apache i Nginx. Wydajność oprogramowania pośredniczącego prawdopodobnie nie będzie zgodne z modułów serwera. [Serwer HTTP.sys](xref:fundamentals/servers/httpsys) serwera i [Kestrel](xref:fundamentals/servers/kestrel) server obecnie nie oferują obsługę wbudowanych kompresji.

Oprogramowanie pośredniczące kompresji odpowiedzi należy użyć, jeśli:

* Nie można użyć następujących technologii serwerowych kompresji:
  * [Moduł dynamicznej kompresji usług IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Moduł mod_deflate Apache](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Serwer Nginx kompresja i Dekompresja](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Hosting bezpośrednio na:
  * [Serwer HTTP.sys](xref:fundamentals/servers/httpsys) (dawniej nazywanych WebListener)
  * [Kestrel serwera](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Kompresja odpowiedzi

Zazwyczaj nie natywnie skompresowane odpowiedzi mogą korzystać z kompresji odpowiedzi. Odpowiedzi natywnie skompresowane zwykle obejmują: CSS, JavaScript, HTML, XML i JSON. Nie należy kompresować natywnie skompresowany zasobów, takich jak pliki PNG. Jeśli użytkownik podejmie próbę dalszego skompresować natywnie skompresowane odpowiedzi, to wszystkie małe dodatkowe skrócenie czasu rozmiar i przekazywania będzie prawdopodobnie overshadowed przez czas potrzebny do przetworzenia kompresji. Nie Kompresuj pliki mniejsze niż około 150 – 1000 bajtów (w zależności od zawartości pliku i wydajność kompresji). Obciążenie kompresowanie małych plików może powodować większe niż nieskompresowanego pliku skompresowanego pliku.

W przypadku klienta może przetwarzać skompresowanej treści, klient musi powiadomić serwera jego możliwości, wysyłając `Accept-Encoding` nagłówek z żądania. Gdy serwer wysyła skompresowanej treści, musi on zawierać informacje zawarte w `Content-Encoding` nagłówek jak skompresowane odpowiedzi jest zaszyfrowana. Zawartość nazw kodowania, obsługiwane przez oprogramowanie pośredniczące są wyświetlane w poniższej tabeli.

::: moniker range=">= aspnetcore-2.2"

| `Accept-Encoding` wartości nagłówka | Obsługiwane oprogramowanie pośredniczące | Opis |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Tak (ustawienie domyślne)        | [Format skompresowanych danych Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | Nie                   | [Format skompresowanych danych DEFLATE](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | Nie                   | [Wymiana wydajne XML W3C](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Yes                  | [Format pliku gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Yes                  | Identyfikator "Bez kodowania": Odpowiedź nie musi być zakodowany. |
| `pack200-gzip`                  | Nie                   | [Format Transfer sieci archiwa Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Yes                  | Kodowanie nie jest jawnie żądanej zawartości dostępne |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| `Accept-Encoding` wartości nagłówka | Obsługiwane oprogramowanie pośredniczące | Opis |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Nie                   | [Format skompresowanych danych Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | Nie                   | [Format skompresowanych danych DEFLATE](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | Nie                   | [Wymiana wydajne XML W3C](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Tak (ustawienie domyślne)        | [Format pliku gzip](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Tak                  | Identyfikator "Bez kodowania": Odpowiedź nie musi być zakodowany. |
| `pack200-gzip`                  | Nie                   | [Format Transfer sieci archiwa Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Tak                  | Kodowanie nie jest jawnie żądanej zawartości dostępne |

::: moniker-end

Aby uzyskać więcej informacji, zobacz [IANA oficjalne kodowania listy zawartości](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

Oprogramowanie pośredniczące pozwala na dodawanie kompresji dodatkowych dostawców na potrzeby niestandardowych `Accept-Encoding` wartości nagłówka. Aby uzyskać więcej informacji, zobacz [niestandardowi](#custom-providers) poniżej.

Oprogramowanie pośredniczące jest w stanie reagowanie na wartości jakości (qvalue, `q`) wagi, gdy wysłane przez klienta w celu określenia priorytetów schematów kompresji. Aby uzyskać więcej informacji, zobacz [RFC 7231: Zaakceptuj kodowanie](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Algorytmy kompresji podlegają zależność między szybkości kompresji i efektywność kompresji. *Skuteczność* w tym kontekście odnosi się do rozmiaru danych wyjściowych po kompresji. Najmniejszy rozmiar odbywa się przez większość *optymalne* kompresji.

Zaangażowane w żądanie nagłówki wysyłanie, buforowanie i odbieranie skompresowanej treści są opisane w poniższej tabeli.

| nagłówek             | Role |
| ------------------ | ---- |
| `Accept-Encoding`  | Wysłanych z klienta do serwera w celu wskazania kodowania schematy dopuszczalne klientowi zawartości. |
| `Content-Encoding` | Wysyłane z serwera do klienta w celu wskazania kodowania zawartości w ładunku. |
| `Content-Length`   | W przypadku kompresji `Content-Length` nagłówka zostanie usunięty, ponieważ zmiany zawartości treści, gdy odpowiedź jest skompresowany. |
| `Content-MD5`      | W przypadku kompresji `Content-MD5` nagłówka zostanie usunięty, ponieważ zmieniono treść i skrót nie jest już prawidłowy. |
| `Content-Type`     | Określa typ MIME zawartości. Należy określić w każdej odpowiedzi jego `Content-Type`. Oprogramowanie pośredniczące sprawdza tę wartość, aby określić, jeśli odpowiedź powinien być skompresowany. Oprogramowanie pośredniczące określa zestaw [domyślne typy MIME](#mime-types) można kodować, ale można zastąpić, lub dodać typy MIME. |
| `Vary`             | Gdy wysyłane przez serwer o wartości `Accept-Encoding` do klientów i serwerów proxy, `Vary` nagłówek wskazuje do klienta lub serwera proxy, który powinien pamięci podręcznej (różne) odpowiedzi na podstawie wartości z `Accept-Encoding` nagłówku żądania. Wynik zwracania zawartości przy użyciu `Vary: Accept-Encoding` nagłówek jest zarówno skompresowane i bez kompresji odpowiedzi są buforowane osobno. |

Zapoznaj się z funkcjami oprogramowanie pośredniczące kompresji odpowiedzi z [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples). Ilustruje przykład:

* Kompresja odpowiedzi aplikacji za pomocą Gzip i dostawców niestandardowych kompresji.
* Jak dodać typ MIME do domyślnej listy typów MIME dla kompresji.

## <a name="package"></a>Package

::: moniker range=">= aspnetcore-2.1"

Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), która obejmuje [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pakietu.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage), która obejmuje [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pakietu.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pakietu.

::: moniker-end

## <a name="configuration"></a>Konfiguracja

::: moniker range=">= aspnetcore-2.2"

Poniższy kod przedstawia sposób włączania oprogramowanie pośredniczące kompresji odpowiedzi dla domyślnych typów MIME i dostawców kompresji ([Brotli](#brotli-compression-provider) i [Gzip](#gzip-compression-provider)):

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Poniższy kod przedstawia sposób włączania oprogramowanie pośredniczące kompresji odpowiedzi na domyślnej stronie typy MIME i [dostawcy kompresji Gzip](#gzip-compression-provider):

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

Uwagi:

* `app.UseResponseCompression` musi zostać wywołana przed `app.UseMvc`.
* Użyj narzędzia takiego jak [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), lub [Postman](https://www.getpostman.com/) można ustawić `Accept-Encoding` nagłówek żądania i badanie nagłówki odpowiedzi, rozmiar i treść.

Prześlij żądanie do przykładowej aplikacji bez `Accept-Encoding` nagłówka i sprawdź, czy odpowiedź jest bez kompresji. `Content-Encoding` i `Vary` nagłówki nie są obecne w odpowiedzi.

![Wynik żądania bez nagłówka Accept-Encoding wyświetlana w oknie programu fiddler. Odpowiedź nie jest skompresowany.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

Prześlij żądanie do przykładowej aplikacji przy użyciu `Accept-Encoding: br` nagłówka (Brotli kompresji) i sprawdź, czy odpowiedź jest skompresowany. `Content-Encoding` i `Vary` nagłówki są obecne w odpowiedzi.

![Wynik żądania przy użyciu nagłówka Accept-Encoding i wartość br okno programu fiddler. Nagłówki zależne i kodowania zawartości są dodawane do odpowiedzi. Odpowiedź jest skompresowany.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Prześlij żądanie do przykładowej aplikacji przy użyciu `Accept-Encoding: gzip` nagłówka i sprawdź, czy odpowiedź jest skompresowany. `Content-Encoding` i `Vary` nagłówki są obecne w odpowiedzi.

![Okno narzędzia fiddler wynik żądania przy użyciu nagłówka Accept-Encoding i wartość gzip. Nagłówki zależne i kodowania zawartości są dodawane do odpowiedzi. Odpowiedź jest skompresowany.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a>dostawcy

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a>Dostawca kompresji Brotli

Użyj <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> kompresji odpowiedzi z [format skompresowanych danych Brotli](https://tools.ietf.org/html/rfc7932).

Jeśli żaden dostawca kompresji są jawnie dodawane do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* Dostawca kompresji Brotli jest domyślnie dodawany do tablicy dostawców kompresji wraz z [dostawcy kompresji Gzip](#gzip-compression-provider).
* Kompresja domyślnie kompresji Brotli Brotli format skompresowanych danych jest obsługiwany przez klienta. Jeśli Brotli nie jest obsługiwane przez klienta, kompresji domyślnie Gzip, gdy klient obsługuje kompresję Gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Dostawca kompresji Brotoli muszą być dodane po dodaniu jawnie żadnego dostawcy kompresji:

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

Ustaw poziom dzięki kompresji <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>. Dostawca kompresji Brotli wartość domyślna to najszybszy poziom kompresji ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), może nie dawać kompresji najbardziej efektywny sposób. W razie potrzeby kompresji najbardziej efektywny sposób konfiguracji oprogramowania pośredniczącego optymalnej kompresji.

| Poziom kompresji | Opis |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | Kompresja powinno zająć tak szybko, jak to możliwe, nawet jeśli dane wyjściowe nie są optymalnie skompresowane. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | Kompresja nie powinny być wykonywane. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Odpowiedzi powinny zostać skompresowane optymalnie, nawet jeśli kompresja zajmuje więcej czasu na ukończenie. |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a>Dostawca kompresji gzip

Użyj <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> kompresji odpowiedzi z [format pliku Gzip](https://tools.ietf.org/html/rfc1952).

Jeśli żaden dostawca kompresji są jawnie dodawane do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

::: moniker range=">= aspnetcore-2.2"

* Dostawca kompresji Gzip jest domyślnie dodawany do tablicy dostawców kompresji wraz z [dostawcy kompresji Brotli](#brotli-compression-provider).
* Kompresja domyślnie kompresji Brotli Brotli format skompresowanych danych jest obsługiwany przez klienta. Jeśli Brotli nie jest obsługiwane przez klienta, kompresji domyślnie Gzip, gdy klient obsługuje kompresję Gzip.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* Dostawca kompresji Gzip jest domyślnie dodawany do tablicy dostawców kompresji.
* Domyślnie kompresji Gzip, gdy klient obsługuje kompresję Gzip.

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Dostawca kompresji Gzip muszą być dodane po dodaniu jawnie żadnego dostawcy kompresji:

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

Ustaw poziom dzięki kompresji <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>. Dostawca kompresji Gzip, wartość domyślna to najszybszy poziom kompresji ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), może nie dawać kompresji najbardziej efektywny sposób. W razie potrzeby kompresji najbardziej efektywny sposób konfiguracji oprogramowania pośredniczącego optymalnej kompresji.

| Poziom kompresji | Opis |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | Kompresja powinno zająć tak szybko, jak to możliwe, nawet jeśli dane wyjściowe nie są optymalnie skompresowane. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | Kompresja nie powinny być wykonywane. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Odpowiedzi powinny zostać skompresowane optymalnie, nawet jeśli kompresja zajmuje więcej czasu na ukończenie. |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a>Dostawcy niestandardowi

Tworzenie implementacji niestandardowych kompresji z <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>. <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Reprezentuje zawartość, kodowanie, że `ICompressionProvider` tworzy. Oprogramowanie pośredniczące używa tych informacji do wybierz dostawcę oparte na listę określoną w `Accept-Encoding` nagłówku żądania.

Korzystając z przykładowej aplikacji, klient przesyła żądanie z `Accept-Encoding: mycustomcompression` nagłówka. Oprogramowanie pośredniczące używa implementacji niestandardowych kompresji i zwraca odpowiedź z `Content-Encoding: mycustomcompression` nagłówka. Klient musi mieć możliwość Dekompresuj niestandardowego kodowania w kolejności dla implementacji niestandardowych kompresji do pracy.

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

Prześlij żądanie do przykładowej aplikacji przy użyciu `Accept-Encoding: mycustomcompression` nagłówka i sprawdź, czy nagłówki odpowiedzi. `Vary` i `Content-Encoding` nagłówki są obecne w odpowiedzi. Treść odpowiedzi (niewyświetlany) nie jest skompresowany za próbki. Nie ma implementacji kompresji `CustomCompressionProvider` klasy próbki. Jednak przykład pokazuje, gdzie będzie implementować algorytm kompresji.

![Wynik żądania przy użyciu nagłówka Accept-Encoding i wartość mycustomcompression okno programu fiddler. Nagłówki zależne i kodowania zawartości są dodawane do odpowiedzi.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a>MIME, typy

Oprogramowanie pośredniczące Określa domyślny zestaw typów MIME dla kompresji:

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

Zastąp lub Dołącz typy MIME przy użyciu opcji oprogramowanie pośredniczące kompresji odpowiedzi. Należy pamiętać, że symbol wieloznaczny MIME typów, takich jak `text/*` nie są obsługiwane. Przykładowa aplikacja dodaje typ MIME dla `image/svg+xml` kompresuje i służy transparent obrazu platformy ASP.NET Core (*banner.svg*).

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a>Kompresja z bezpiecznego protokołu

Skompresowane odpowiedzi za pośrednictwem bezpiecznego połączenia, mogą być kontrolowane za pomocą `EnableForHttps` opcja, która jest domyślnie wyłączona. Przy użyciu kompresji, za pomocą stron dynamicznie generowanym może prowadzić do problemów zabezpieczeń takich jak [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszenia](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataków.

## <a name="adding-the-vary-header"></a>Dodawanie nagłówka zależne

::: moniker range=">= aspnetcore-2.0"

Podczas kompresowania odpowiedzi na podstawie `Accept-Encoding` nagłówka, występują potencjalnie wiele wersji skompresowane odpowiedzi i wersji bez kompresji. Aby można było wydać polecenie pamięci podręcznej klienta i serwera proxy, że wiele wersji istnieją i powinny być przechowywane, `Vary` nagłówek jest dodawana z `Accept-Encoding` wartości. W programie ASP.NET Core 2.0 lub nowszej, dodaje oprogramowanie pośredniczące `Vary` nagłówka automatycznie, gdy odpowiedź jest skompresowany.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Podczas kompresowania odpowiedzi na podstawie `Accept-Encoding` nagłówka, występują potencjalnie wiele wersji skompresowane odpowiedzi i wersji bez kompresji. Aby można było wydać polecenie pamięci podręcznej klienta i serwera proxy, że wiele wersji istnieją i powinny być przechowywane, `Vary` nagłówek jest dodawana z `Accept-Encoding` wartości. W programie ASP.NET Core 1.x, dodając `Vary` nagłówek odpowiedzi odbywa się ręcznie:

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Oprogramowanie pośredniczące problem za zaporą zwrotny serwer proxy Nginx

Gdy żądanie jest przekazywane przez serwer Nginx, `Accept-Encoding` nagłówka zostanie usunięty. Usuwanie `Accept-Encoding` nagłówka zapobiega oprogramowanie pośredniczące kompresji odpowiedzi. Aby uzyskać więcej informacji, zobacz [NGINX: Kompresja i Dekompresja](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Ten problem jest śledzona przez [ustalić przekazywanego kompresji dla kontenera Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Praca z kompresji dynamicznej usług IIS

Jeśli masz aktywne dynamicznej kompresji Moduł IIS konfigurowane na poziomie serwera, który chcesz wyłączyć dla aplikacji, Wyłącz moduł za pomocą dodatku *web.config* pliku. Aby uzyskać więcej informacji, zobacz [moduły IIS wyłączenie](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Użyj narzędzia, takiego jak [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), lub [Postman](https://www.getpostman.com/), umożliwiają ustawianie `Accept-Encoding` nagłówek żądania i badanie nagłówki odpowiedzi, rozmiar i treść. Domyślnie oprogramowanie pośredniczące kompresji odpowiedzi kompresuje odpowiedzi, które spełniają poniższe warunki:

::: moniker range=">= aspnetcore-2.2"

* `Accept-Encoding` Występuje nagłówek, wartością `br`, `gzip`, `*`, lub niestandardowego kodowania, które odpowiada dostawcy niestandardowego kompresji, który został określony. Wartość nie może być `identity` ani mieć wartości jakości (qvalue, `q`) ustawienie 0 (zero).
* Typ MIME (`Content-Type`) musi być ustawiona, a musi być zgodny typ MIME skonfigurowane na <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* Żądanie nie może zawierać `Content-Range` nagłówka.
* Żądanie musi używać protokołu niezabezpieczonego (http), chyba, że bezpiecznego protokołu (https) jest skonfigurowana w opcjach oprogramowanie pośredniczące kompresji odpowiedzi. *Należy pamiętać, niebezpieczeństwo [opisanych powyżej](#compression-with-secure-protocol) podczas włączania bezpiecznego kompresję zawartości.*

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* `Accept-Encoding` Występuje nagłówek, wartością `gzip`, `*`, lub niestandardowego kodowania, które odpowiada dostawcy niestandardowego kompresji, który został określony. Wartość nie może być `identity` ani mieć wartości jakości (qvalue, `q`) ustawienie 0 (zero).
* Typ MIME (`Content-Type`) musi być ustawiona, a musi być zgodny typ MIME skonfigurowane na <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* Żądanie nie może zawierać `Content-Range` nagłówka.
* Żądanie musi używać protokołu niezabezpieczonego (http), chyba, że bezpiecznego protokołu (https) jest skonfigurowana w opcjach oprogramowanie pośredniczące kompresji odpowiedzi. *Należy pamiętać, niebezpieczeństwo [opisanych powyżej](#compression-with-secure-protocol) podczas włączania bezpiecznego kompresję zawartości.*

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Mozilla Developer Network: Accept-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 Section 3.1.2.1: Zawierać zawartości](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 sekcja 4.2.3: Kodowanie w formacie gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Wersja specyfikacji formatu pliku GZIP 4.3](https://www.ietf.org/rfc/rfc1952.txt)
