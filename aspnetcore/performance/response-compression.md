---
title: "Oprogramowanie pośredniczące kompresji odpowiedzi dla platformy ASP.NET Core"
author: guardrex
description: "Więcej informacji na temat kompresji odpowiedzi i sposobie używania oprogramowania pośredniczącego kompresji odpowiedzi w aplikacji platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: eae51e74c7f2b2f038638c765d4e833a1d9b1232
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="response-compression-middleware-for-aspnet-core"></a>Oprogramowanie pośredniczące kompresji odpowiedzi dla platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Przepustowość sieci jest ograniczona zasobów. Zmniejszenie rozmiaru odpowiedzi, zazwyczaj często gwałtowny wzrost czas odpowiedzi aplikacji. Jednym ze sposobów zmniejszyć rozmiar ładunku jest kompresji odpowiedzi aplikacji.

## <a name="when-to-use-response-compression-middleware"></a>Kiedy należy używać oprogramowania pośredniczącego kompresji odpowiedzi
Użyj technologii kompresji odpowiedzi na serwerze usług IIS, Apache lub Nginx. Wydajność oprogramowania pośredniczącego prawdopodobnie nie będzie zgodne z modułami serwera. [Serwer HTTP.sys](xref:fundamentals/servers/httpsys) i [Kestrel](xref:fundamentals/servers/kestrel) aktualnie nie oferują obsługę wbudowanych kompresji.

Użyj oprogramowania pośredniczącego kompresji odpowiedzi, gdy jesteś:

* Nie można użyć następujących technologii serwerowych kompresji:
  * [Moduł dynamicznej kompresji usług IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Apache mod_deflate modułu](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Nginx kompresji i dekompresji](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Hosting bezpośrednio na:
  * [Serwer HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanych [WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Kompresja odpowiedzi
Zazwyczaj nie natywnie skompresowane odpowiedzi mogą korzystać z kompresji odpowiedzi. Odpowiedzi nie natywnie skompresowane zwykle obejmują: CSS, JavaScript, HTML, XML i JSON. Nie Kompresuj natywnie skompresowany zasobów, takich jak pliki PNG. Próba dalsze Kompresuj natywnie skompresowane odpowiedzi żadnych dodatkowych małych skrócenie czasu rozmiaru i transmisji prawdopodobnie będzie overshadowed przez czas przetwarzania kompresji. Nie Kompresuj pliki mniejsze niż około 150 – 1000 bajtów (w zależności od zawartości pliku i wydajności kompresji). Koszty kompresowania małych plików może powodować większe niż nieskompresowanego pliku skompresowanego pliku.

W przypadku klienta może przetwarzać skompresowanej zawartości, klient musi powiadomić serwera jej możliwości, wysyłając `Accept-Encoding` nagłówek z żądania. Gdy serwer wysyła skompresowanej treści, musi on zawierać informacje w `Content-Encoding` nagłówka w sposób jest zakodowany skompresowanych odpowiedzi. W poniższej tabeli przedstawiono zawartości kodowania nazw obsługiwanych przez oprogramowanie pośredniczące.

| `Accept-Encoding`wartości nagłówka | Obsługiwane oprogramowanie pośredniczące | Opis                                                 |
| :-----------------------------: | :------------------: | ----------------------------------------------------------- |
| `br`                            | Nie                   | Format skompresowanych danych Brotli                               |
| `compress`                      | Nie                   | Format danych "Kompresuj" systemu UNIX                                 |
| `deflate`                       | Nie                   | "deflate" skompresowane dane w formacie danych "zlib"     |
| `exi`                           | Nie                   | W3C XML wydajne wymiany                               |
| `gzip`                          | Tak (ustawienie domyślne)        | format pliku gzip                                            |
| `identity`                      | Tak                  | Identyfikator "Bez kodowania": nie musi być zakodowany odpowiedzi. |
| `pack200-gzip`                  | Nie                   | Format transferu sieciowego archiwów Java                   |
| `*`                             | Tak                  | Dostępne kodowanie nie jest jawnie żądanej zawartości     |

Aby uzyskać więcej informacji, zobacz [przez organizację IANA oficjalnego zawartości listy kodowania](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

Oprogramowanie pośredniczące pozwala dodać kompresji dodatkowych dostawców na potrzeby niestandardowe `Accept-Encoding` wartości nagłówka. Aby uzyskać więcej informacji, zobacz [dostawców niestandardowych](#custom-providers) poniżej.

Oprogramowanie pośredniczące jest w stanie reagowanie na wartość jakości (qvalue, `q`) wagi po wysłaniu przez klienta priorytety schematów kompresji. Aby uzyskać więcej informacji, zobacz [RFC 7231: Zaakceptuj kodowanie](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Algorytmy kompresji podlegają zależności między szybkość kompresji i skuteczności kompresji. *Skuteczność* w tym kontekście odnosi się do rozmiaru dane wyjściowe po kompresji. Jest to osiągane przez maksymalnie najmniejszy rozmiar *optymalną* kompresji.

Nagłówki zaangażowane w żądanie wysyłania, buforowanie i odbieranie skompresowanej zawartości są opisane w poniższej tabeli.

| nagłówek             | Rola |
| ------------------ | ---- |
| `Accept-Encoding`  | Wysłanych z klienta na serwerze wskazuje zawartość, kodowanie schematy dopuszczalne do klienta. |
| `Content-Encoding` | Wysyłane z serwera do klienta w celu wskazania kodowania zawartości w ładunku. |
| `Content-Length`   | W przypadku kompresji `Content-Length` nagłówka zostanie usunięty, ponieważ zmiany zawartości treści podczas kompresowania odpowiedzi. |
| `Content-MD5`      | W przypadku kompresji `Content-MD5` nagłówka zostanie usunięty, ponieważ zmieniono zawartość treści i skrót nie jest już prawidłowy. |
| `Content-Type`     | Określa typ MIME zawartości. Należy określić co odpowiedzi jego `Content-Type`. Oprogramowanie pośredniczące sprawdza tę wartość, aby określić, jeśli odpowiedź powinna być kompresowana. Oprogramowanie pośredniczące określa zbiór [domyślne typy MIME](#mime-types) go może zakodować, ale można zastąpić, lub dodać typy MIME. |
| `Vary`             | Po wysłaniu przez serwer z wartością `Accept-Encoding` do klientów i serwerów proxy, `Vary` nagłówka wskazuje do klienta lub serwera proxy, który powinien pamięci podręcznej (różne) odpowiedzi na podstawie wartości z `Accept-Encoding` nagłówka żądania. Wynik zwracania zawartości za pomocą `Vary: Accept-Encoding` nagłówka jest zarówno skompresowane i nieskompresowanym odpowiedzi są buforowane oddzielnie. |

Funkcje oprogramowania pośredniczącego kompresji odpowiedzi z można eksplorować [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). Przykładową:
* Kompresja odpowiedzi aplikacji przy użyciu dostawców niestandardowych kompresji i gzip.
* Jak dodać typ MIME do listy domyślnych typów MIME kompresji.

## <a name="package"></a>Package
Aby dołączyć oprogramowanie pośredniczące w projekcie, należy dodać odwołanie do [ `Microsoft.AspNetCore.ResponseCompression` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) pakietu lub użyj [ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) pakietu. Ta funkcja jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 1.1 lub nowszej.

## <a name="configuration"></a>Konfiguracja
Poniższy kod przedstawia sposób włączania oprogramowania pośredniczącego kompresji odpowiedzi z kompresji gzip domyślne i domyślnych typów MIME.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/StartupBasic.cs?name=snippet1&highlight=4,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/StartupBasic.cs?name=snippet1&highlight=3,8)]

---

> [!NOTE]
> Użyj narzędzia, takiego jak [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), lub [Postman](https://www.getpostman.com/) można ustawić `Accept-Encoding` nagłówek żądania i badania nagłówki odpowiedzi, rozmiar i treść.

Prześlij żądanie do przykładowej aplikacji bez `Accept-Encoding` nagłówka i sprawdź, czy odpowiedź jest nieskompresowane. `Content-Encoding` i `Vary` nagłówki nie są dostępne w odpowiedzi.

![Wynik żądania bez nagłówka Accept-Encoding przedstawiający okno fiddler. Odpowiedź nie jest kompresowana.](response-compression/_static/request-uncompressed.png)

Prześlij żądanie do aplikacji przykładowej przy `Accept-Encoding: gzip` nagłówka i sprawdź, czy odpowiedź jest kompresowana. `Content-Encoding` i `Vary` nagłówki są obecne w odpowiedzi.

![Okno narzędzia fiddler przedstawiający wynik żądania z nagłówka Accept-Encoding i wartości gzip. Nagłówki Vary i kodowania zawartości są dodawane do odpowiedzi. Odpowiedź jest kompresowana.](response-compression/_static/request-compressed.png)

## <a name="providers"></a>dostawców
### <a name="gzipcompressionprovider"></a>GzipCompressionProvider
Użyj `GzipCompressionProvider` kompresji odpowiedzi z gzip. To jest dostawcą domyślnym kompresji, jeśli żaden nie jest określony. Można ustawić poziomu z kompresji `GzipCompressionProviderOptions`. 

Domyślnie dostawca kompresji gzip najszybszym poziom kompresji (`CompressionLevel.Fastest`), może nie dawać najbardziej efektywny kompresji. W razie potrzeby najbardziej efektywny kompresji można skonfigurować oprogramowanie pośredniczące optymalnej kompresji.

| Poziom kompresji                | Opis                                                                                                   |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `CompressionLevel.Fastest`       | Kompresja powinno zakończyć tak szybko jak to możliwe, nawet jeśli dane wyjściowe nie jest optymalnie skompresowane. |
| `CompressionLevel.NoCompression` | Kompresja nie powinny być wykonywane.                                                                           |
| `CompressionLevel.Optimal`       | Odpowiedzi powinna być optymalnie kompresowana, nawet jeśli kompresja trwa dłużej.                |


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=3,8-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,10-13)]

---

## <a name="mime-types"></a>MIME, typy
Oprogramowanie pośredniczące Określa domyślny zestaw typy MIME kompresji:
* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

Można zastąpić, lub Dołącz typy MIME opcje oprogramowania pośredniczącego kompresji odpowiedzi. Należy pamiętać, że symbol wieloznaczny MIME typy, takich jak `text/*` nie są obsługiwane. Przykładowa aplikacja dodaje typ MIME dla `image/svg+xml` kompresuje i obsługuje platformy ASP.NET Core obraz transparentu (*banner.svg*).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7)]

---

### <a name="custom-providers"></a>Dostawcy niestandardowi
Można tworzyć niestandardowe kompresji implementacje z `ICompressionProvider`. `EncodingName` Reprezentuje zawartość, kodowanie tego `ICompressionProvider` tworzy. Oprogramowanie pośredniczące używa tych informacji do wybierz dostawcę na podstawie listy określone w `Accept-Encoding` nagłówka żądania.

Przy użyciu aplikacji przykładowej, klient przesyła żądanie z `Accept-Encoding: mycustomcompression` nagłówka. Oprogramowanie pośredniczące używa implementacji niestandardowych kompresji i zwraca odpowiedź z `Content-Encoding: mycustomcompression` nagłówka. Klient musi mieć możliwość dekompresja niestandardowego kodowania w kolejności stosowania niestandardowego kompresji do pracy.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=4)]

[!code-csharp[Main](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[Main](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

Prześlij żądanie do aplikacji przykładowej przy `Accept-Encoding: mycustomcompression` nagłówka i obserwować nagłówków odpowiedzi. `Vary` i `Content-Encoding` nagłówki są obecne w odpowiedzi. Treść odpowiedzi (tego nie pokazano) nie jest skompresowane za próbki. Nie ma implementacja kompresji `CustomCompressionProvider` klasy próbki. Jednak pokazano, gdzie czy wdrożenie jest algorytm kompresji.

![Wynik żądania z nagłówka Accept-Encoding oraz wartość mycustomcompression okno fiddler. Nagłówki Vary i kodowania zawartości są dodawane do odpowiedzi.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a>Kompresja z bezpiecznego protokołu
Skompresowane odpowiedzi za pośrednictwem bezpiecznego połączenia można sterować za pomocą `EnableForHttps` opcja, która jest domyślnie wyłączona. Używanie kompresji na dynamicznie generowanym stronach może prowadzić do problemów z bezpieczeństwem takich jak [ataki CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) i [naruszenia](https://wikipedia.org/wiki/BREACH_(security_exploit)) ataków.

## <a name="adding-the-vary-header"></a>Dodawanie nagłówka Vary
Podczas kompresowania odpowiedzi na podstawie `Accept-Encoding` nagłówka, istnieją potencjalnie wiele wersji skompresowanych odpowiedzi i nieskompresowanym wersji. Aby nakazać klienta i serwera proxy pamięci podręcznej, czy istnieją wiele wersji, a powinien być przechowywany, `Vary` nagłówka zostanie dodany z `Accept-Encoding` wartości. W przypadku platformy ASP.NET Core 1.x, dodawanie `Vary` nagłówka do odpowiedzi odbywa się ręcznie. W przypadku platformy ASP.NET Core dodaje oprogramowanie pośredniczące 2.x, `Vary` nagłówka automatycznie w przypadku skompresowanych odpowiedzi.

**Program ASP.NET Core tylko 1.x**

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet1)]

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Oprogramowanie pośredniczące problem podczas pod zwrotny serwer proxy Nginx
Jeśli żądanie jest przekazywane przez serwer proxy przez Nginx, `Accept-Encoding` nagłówka zostaną usunięte. Zapobiega to oprogramowanie pośredniczące od kompresji odpowiedzi. Aby uzyskać więcej informacji, zobacz [NGINX: kompresji i dekompresji](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Ten problem jest śledzony przez [zorientować się przekazujące kompresja Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Praca z kompresji dynamicznej usług IIS
Jeśli masz aktywnego IIS dynamicznej kompresji modułu skonfigurowane na poziomie serwera, który ma zostać wyłączone dla aplikacji, możesz to zrobić z dodatku programu *web.config* pliku. Aby uzyskać więcej informacji, zobacz [moduły IIS wyłączenie](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Rozwiązywanie problemów
Użyj narzędzia, takiego jak [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), lub [Postman](https://www.getpostman.com/), umożliwiają skonfigurowanie `Accept-Encoding` nagłówek żądania i badania nagłówki odpowiedzi, rozmiar i treść. Oprogramowanie pośredniczące kompresji odpowiedzi kompresuje odpowiedzi, które spełniają poniższe warunki:
* `Accept-Encoding` Nagłówek ma wartość `gzip`, `*`, lub niestandardowy kodowania pasujący dostawcy niestandardowego kompresji, który został określony. Wartość nie może być `identity` ani mieć wartości jakości (qvalue, `q`) ustawienie 0 (zero).
* Typ MIME (`Content-Type`) musi być ustawiona i musi być zgodny z typem MIME skonfigurowane na `ResponseCompressionOptions`.
* Żądanie nie może zawierać `Content-Range` nagłówka.
* Żądanie musi używać protokołu niezabezpieczonego (http), chyba że bezpiecznego protokołu (https) jest skonfigurowana w opcjach oprogramowanie pośredniczące kompresji odpowiedzi. *Należy zwrócić uwagę zagrożenia [opisane powyżej](#compression-with-secure-protocol) po włączaniu bezpiecznych kompresji zawartości.*

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Uruchamianie aplikacji](xref:fundamentals/startup)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Mozilla Developer Network: Zaakceptuj kodowania](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [Sekcji 7231 RFC 3.1.2.1: Zawierać zawartości](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 sekcja 4.2.3: Kodowanie Gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Wersja specyfikacji formatu pliku GZIP 4.3](http://www.ietf.org/rfc/rfc1952.txt)
