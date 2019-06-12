---
title: Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia
author: guardrex
description: Więcej informacji o konfiguracji dla aplikacji hostowanych za serwery proxy i moduły równoważenia obciążenia, które często zasłaniać żądanie ważnych informacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2019
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: bcafc33b8faf81912d536d3df8941d196685ecad
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837361"
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia

Przez [Luke Latham](https://github.com/guardrex) i [Chris Ross](https://github.com/Tratcher)

W zalecanej konfiguracji dla platformy ASP.NET Core aplikacja jest hostowana przy użyciu modułu Core IIS/ASP.NET, Nginx lub Apache. Serwery proxy, moduły równoważenia obciążenia i innych urządzeniach sieciowych często zasłaniać informacje o żądaniu przed osiągnięciem przez nią aplikacji:

* Gdy żądania HTTPS są przekazywane za pośrednictwem protokołu HTTP, oryginalnym schematem (HTTPS) zostaną utracone i muszą być przekazywane w nagłówku.
* Ponieważ aplikacja odbiera żądanie z serwera proxy i jej wartość true, źródła w Internecie lub w sieci firmowej, źródłowy adres IP klienta, również muszą być przekazywane w nagłówku.

Te informacje mogą być ważne przetwarzanie żądań, na przykład w przekierowania, uwierzytelnianie, generowanie konsolidacji, ocena zasad i geolokalizacja klienta.

## <a name="forwarded-headers"></a>Nagłówki przekazywane

Zgodnie z Konwencją serwery proxy przesyłania dalej informacji w nagłówkach HTTP.

| nagłówek | Opis |
| ------ | ----------- |
| X-Forwarded-For | Przechowuje informacje dotyczące klienta, który zainicjował żądań i kolejne serwery proxy w łańcuchu serwery proxy. Ten parametr może zawierać adresów IP adresy (i, opcjonalnie, numery portów). Łańcuch serwerów proxy pierwszy parametr wskazuje klienta, w którym żądanie zostało wystosowane najpierw. Postępuj zgodnie z identyfikatorami kolejnych serwera proxy. Ostatni serwer proxy w łańcuchu nie ma na liście parametrów. Adres IP ostatniego serwera proxy i opcjonalnie numeru portu są dostępne jako zdalny adres IP w warstwie transportowej. |
| X-Forwarded-Proto | Wartość źródłowy schemat (HTTP/HTTPS). Wartość może być też listy programów, jeśli żądanie jest przesunięta wielu serwerów proxy. |
| X-Forwarded-Host | Oryginalna wartość pola nagłówka hosta. Zazwyczaj serwery proxy nie należy modyfikować nagłówka hosta. Zobacz [Microsoft Security doradztwa technicznego dotyczącego CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) uzyskać informacji na temat luk w zabezpieczeniach typu "podniesienia uprawnień", która dotyczy systemów, w którym serwer proxy nie sprawdzania poprawności, lub ograniczyć nagłówki hosta do znane dobre wartości. |

Pośredniczącym nagłówki przekazywane z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu, odczytuje tych nagłówków i wypełnia skojarzonymi polami na <xref:Microsoft.AspNetCore.Http.HttpContext>.

Aktualizacje oprogramowania pośredniczącego:

* [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress) &ndash; można ustawić przy użyciu `X-Forwarded-For` wartość nagłówka. Dodatkowe ustawienia mają wpływ na sposób ustawiania oprogramowanie pośredniczące `RemoteIpAddress`. Aby uzyskać więcej informacji, zobacz [opcje przekazywane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](xref:Microsoft.AspNetCore.Http.HttpRequest.Scheme) &ndash; można ustawić przy użyciu `X-Forwarded-Proto` wartość nagłówka.
* [HttpContext.Request.Host](xref:Microsoft.AspNetCore.Http.HttpRequest.Host) &ndash; można ustawić przy użyciu `X-Forwarded-Host` wartość nagłówka.

Oprogramowanie pośredniczące nagłówki przekazywane [domyślne ustawienia](#forwarded-headers-middleware-options) można skonfigurować. Ustawienia domyślne są:

* Jest dostępny tylko *jeden serwer proxy* między aplikacją a źródło żądania.
* Tylko adresy sprzężenia zwrotnego są skonfigurowane dla znanych serwerów proxy i znane sieci.
* Nagłówki przekazywane są nazywane `X-Forwarded-For` i `X-Forwarded-Proto`.

Nie wszystkie urządzenia sieciowe, Dodaj `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki bez dodatkowej konfiguracji. Sprawdź wskazówki z producentem urządzenia, jeśli żądań kierowanych na serwer proxy nie zawierają tych nagłówków, gdy osiągną oni aplikację. Jeśli urządzenie korzysta z nazw nagłówka innego niż `X-Forwarded-For` i `X-Forwarded-Proto`ustaw <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> i <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> opcje można dopasować nazw nagłówka używany przez urządzenie. Aby uzyskać więcej informacji, zobacz [opcje przekazywane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options) i [konfigurację serwera proxy, który korzysta z nazw inny nagłówek](#configuration-for-a-proxy-that-uses-different-header-names).

## <a name="iisiis-express-and-aspnet-core-module"></a>Usługi IIS/IIS Express i modułów platformy ASP.NET Core

Przekazane nagłówki oprogramowanie pośredniczące jest włączona domyślnie [oprogramowania pośredniczącego integracji usługi IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) gdy aplikacja jest hostowana [spoza procesu](xref:fundamentals/servers/index#out-of-process-hosting-model) usług IIS oraz modułu ASP.NET Core. Przekazane nagłówki oprogramowanie pośredniczące jest aktywowana ma być uruchomiony w potoku oprogramowania pośredniczącego przy użyciu określonej konfiguracji ograniczone do modułu ASP.NET Core ze względu na wątpliwości relacji zaufania z nagłówkami przekazywane (na przykład [fałszowanie adresów IP](https://www.iplocation.net/ip-spoofing)). Oprogramowanie pośredniczące jest skonfigurowana do przesyłania dalej `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków i jest ograniczona do jednego hosta lokalnego serwera proxy. Jeśli wymagana jest dodatkowa konfiguracja, zobacz [opcje przekazywane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Inne serwera proxy i scenariuszy usługi równoważenia obciążenia

Poza przy użyciu [integracji usług IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) hostując [spoza procesu](xref:fundamentals/servers/index#out-of-process-hosting-model), przekazywane oprogramowania pośredniczącego nagłówków nie jest włączona domyślnie. Przekazane nagłówki oprogramowania pośredniczącego musi być włączona dla aplikacji do nagłówków procesów przekazywanych za pomocą <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>. Po włączeniu oprogramowanie pośredniczące, jeśli nie <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> określonych do oprogramowania pośredniczącego, domyślnie [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) są [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders).

Konfigurowanie oprogramowania pośredniczącego z <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków w `Startup.ConfigureServices`. Wywoływanie <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> method in Class metoda `Startup.Configure` przed wywołaniem innym oprogramowaniu pośredniczącym:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> Jeśli nie <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> są określone w `Startup.ConfigureServices` lub bezpośrednio do metody rozszerzenia za pomocą <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*>, są domyślne nagłówki do przekazywania [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders). <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> Właściwość musi być skonfigurowany przy użyciu nagłówków do przesyłania dalej.

## <a name="nginx-configuration"></a>Konfigurację serwera Nginx

Do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków, zobacz <xref:host-and-deploy/linux-nginx#configure-nginx>. Aby uzyskać więcej informacji, zobacz [NGINX: Przy użyciu nagłówka przesłanym](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).

## <a name="apache-configuration"></a>Konfiguracja Apache

`X-Forwarded-For` zostanie dodany automatycznie (patrz [mod_proxy Apache modułu: Reverse Proxy Request Headers](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)). Aby uzyskać informacje na temat sposobu przekazywania `X-Forwarded-Proto` nagłówka, zobacz <xref:host-and-deploy/linux-apache#configure-apache>.

## <a name="forwarded-headers-middleware-options"></a>Przekazane opcje oprogramowania pośredniczącego nagłówki

<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> Sterowanie zachowaniem oprogramowania pośredniczącego nagłówki przekazywane. Poniższy przykład umożliwia zmianę wartości domyślne:

* Ogranicz liczbę wpisów w nagłówki przekazywane do `2`.
* Dodaj adres serwera proxy znanych `127.0.10.1`.
* Zmień nazwę nagłówka przekazywane z domyślnego `X-Forwarded-For` do `X-Forwarded-For-My-Custom-Header-Name`.

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-My-Custom-Header-Name";
});
```

| Opcja | Opis |
| ------ | ----------- |
| AllowedHosts | Ogranicza hosty `X-Forwarded-Host` nagłówka do podanych wartości.<ul><li>Wartości są porównywane, liczba porządkowa Ignoruj-wielkie i małe litery.</li><li>Numery portów muszą zostać wykluczone.</li><li>Jeśli lista jest pusta, wszystkie hosty są dozwolone.</li><li>Symbol wieloznaczny najwyższego poziomu `*` zezwala na wszystkich hostach niepusta.</li><li>Poddomena symbole wieloznaczne są dozwolone, ale nie są zgodne z domeny katalogu głównego. Na przykład `*.contoso.com` odpowiada poddomeny `foo.contoso.com` , ale nie domeny katalogu głównego `contoso.com`.</li><li>Nazwy hostów Unicode są dozwolone, ale są konwertowane na [Punycode](https://tools.ietf.org/html/rfc3492) do dopasowania.</li><li>[Adresy IPv6](https://tools.ietf.org/html/rfc4291) musi zawierać blokujących nawiasy kwadratowe i musi być w [konwencjonalne formularza](https://tools.ietf.org/html/rfc4291#section-2.2) (na przykład `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). Adresy IPv6 nie są specjalne — z uwzględnieniem wielkości liter do sprawdzenia pod kątem równości logicznych między różnych formatów, a nie kanoniczną jest wykonywane.</li><li>Nie można ograniczyć dozwolone hosty mogą zezwolić osobie atakującej podszywały się pod łącza wygenerowanej przez usługę.</li></ul>Wartość domyślna to pusta `IList<string>`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedForHeaderName). Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-For` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.<br><br>Wartość domyślna to `X-Forwarded-For`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders> | Określa, które usług przesyłania dalej powinny zostać przetworzone. Zobacz [wyliczenia ForwardedHeaders](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders) dla listy pól, które są stosowane. Typowe wartości przypisane do tej właściwości to "ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto`.<br><br>Wartość domyślna to [ForwardedHeaders.None](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeaders). |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHostHeaderName> | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedHostHeaderName). Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-Host` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.<br><br>Wartość domyślna to `X-Forwarded-Host`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XForwardedProtoHeaderName). Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-Proto` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.<br><br>Wartość domyślna to `X-Forwarded-Proto`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardLimit> | Ogranicza liczbę wpisów w nagłówki, które są przetwarzane. Ustaw `null` wyłączyć limit, ale powinno mieć miejsce tylko jeśli `KnownProxies` lub `KnownNetworks` są skonfigurowane.<br><br>Domyślnym ustawieniem jest 1. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks> | Zakresy znanych sieci w celu akceptowania nagłówki przekazywane z adresów. Podaj zakresów adresów IP przy użyciu notacji Bezklasowego routingu międzydomenowego (CIDR).<br><br>Jeśli serwer używa podwójnych gniazd, adresy IPv4, są dostarczane w formacie protokołu IPv6 (na przykład `10.0.0.1` w protokole IPv4 reprezentowane w protokole IPv6 jako `::ffff:10.0.0.1`). See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Określić, jeśli ten format jest wymagany, analizując [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Aby uzyskać więcej informacji, zobacz [konfigurację dla adresu IPv4 reprezentowane jako adres IPv6](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) sekcji.<br><br>Wartość domyślna to `IList` \< <xref:Microsoft.AspNetCore.HttpOverrides.IPNetwork>> zawierający pojedynczy wpis dla `IPAddress.Loopback`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies> | Adresy znanych serwerów proxy, aby zaakceptować nagłówki przekazywane z. Użyj `KnownProxies` określić dokładny adres IP jest zgodny.<br><br>Jeśli serwer używa podwójnych gniazd, adresy IPv4, są dostarczane w formacie protokołu IPv6 (na przykład `10.0.0.1` w protokole IPv4 reprezentowane w protokole IPv6 jako `::ffff:10.0.0.1`). See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Określić, jeśli ten format jest wymagany, analizując [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Aby uzyskać więcej informacji, zobacz [konfigurację dla adresu IPv4 reprezentowane jako adres IPv6](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) sekcji.<br><br>Wartość domyślna to `IList` \< <xref:System.Net.IPAddress>> zawierający pojedynczy wpis dla `IPAddress.IPv6Loopback`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalForHeaderName> | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalForHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalForHeaderName).<br><br>Wartość domyślna to `X-Original-For`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalHostHeaderName> | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalHostHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalHostHeaderName).<br><br>Wartość domyślna to `X-Original-Host`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.OriginalProtoHeaderName> | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalProtoHeaderName](xref:Microsoft.AspNetCore.HttpOverrides.ForwardedHeadersDefaults.XOriginalProtoHeaderName).<br><br>Wartość domyślna to `X-Original-Proto`. |
| <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.RequireHeaderSymmetry> | Wymagana jest liczba wartości nagłówka, aby był zsynchronizowany między [ForwardedHeadersOptions.ForwardedHeaders](xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedHeaders) przetwarzany.<br><br>Wartość domyślna w programie ASP.NET Core 1.x jest `true`. Domyślnie w programie ASP.NET Core 2.0 lub nowszym `false`. |

## <a name="scenarios-and-use-cases"></a>Scenariuszy i przypadków użycia

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Nie można dodać przekazywane nagłówki oraz wszystkie żądania są bezpieczne

W niektórych przypadkach może być nie można dodać nagłówki przekazywane do żądania, które są przekazywane do aplikacji. Jeśli serwer proxy jest wymuszania, czy wszystkie publiczne zewnętrznych żądania HTTPS, schemat można ręcznie ustawić `Startup.Configure` przed użyciem dowolnego typu oprogramowania pośredniczącego:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Ten kod można wyłączyć za pomocą zmiennej środowiskowej lub inne ustawienia konfiguracji w rozwoju lub w środowisku przejściowym.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Podstawa ścieżki i serwery proxy, które zmieniają ścieżki żądania

Niektóre serwery proxy Przekaż ścieżkę bez zmian, ale z aplikacją ścieżki podstawowej, które powinno zostać usunięte, tak aby routing działa poprawnie. [UsePathBaseExtensions.UsePathBase](xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*) oprogramowania pośredniczącego dzieli ścieżkę do [HttpRequest.Path](xref:Microsoft.AspNetCore.Http.HttpRequest.Path) i ścieżki podstawowej aplikacji do [HttpRequest.PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase).

Jeśli `/foo` jest ścieżką serwera proxy są przekazywane jako ścieżki podstawowej aplikacji `/foo/api/1`, zestawy oprogramowania pośredniczącego `Request.PathBase` do `/foo` i `Request.Path` do `/api/1` za pomocą następującego polecenia:

```csharp
app.UsePathBase("/foo");
```

Oryginalna ścieżka i podstawa ścieżki zostaną ponownie zastosowane, gdy oprogramowanie pośredniczące wywoływana jest ponownie w odwrotnej kolejności. Aby uzyskać więcej informacji na temat przetwarzania zamówień oprogramowanie pośredniczące, zobacz <xref:fundamentals/middleware/index>.

Jeśli serwer proxy usuwa ścieżki (na przykład przekazywania `/foo/api/1` do `/api/1`), poprawka przekierowuje i łączy, ustawiając żądania [PathBase](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) właściwości:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Jeśli serwer proxy dodaje dane ścieżki, odrzucić część ścieżki rozwiązać za pomocą przekierowań i łącza <xref:Microsoft.AspNetCore.Http.PathString.StartsWithSegments*> i przypisywanie do <xref:Microsoft.AspNetCore.Http.HttpRequest.Path> właściwości:

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

### <a name="configuration-for-a-proxy-that-uses-different-header-names"></a>Konfiguracja serwera proxy, który korzysta z nazw nagłówka

Jeśli serwer proxy nie używa nagłówków o nazwie `X-Forwarded-For` i `X-Forwarded-Proto` do przekazywania adresów/port serwera proxy i pochodzącymi informacji schematu, ustaw <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedForHeaderName> i <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.ForwardedProtoHeaderName> opcje można dopasować nazw nagłówka używany przez serwer proxy:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

### <a name="configuration-for-an-ipv4-address-represented-as-an-ipv6-address"></a>Konfiguracja dla adresu IPv4, reprezentowane jako adres IPv6

Jeśli serwer używa podwójnych gniazd, adresy IPv4, są dostarczane w formacie protokołu IPv6 (na przykład `10.0.0.1` w protokole IPv4 reprezentowane w protokole IPv6 jako `::ffff:10.0.0.1` lub `::ffff:a00:1`). See [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Określić, jeśli ten format jest wymagany, analizując [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).

W poniższym przykładzie adresu sieciowego, który dostarcza nagłówki przekazywane jest dodawany do `KnownNetworks` listy w formacie IPv6.

Adres IPv4: `10.11.12.1/8`

Przekonwertowana adres IPv6: `::ffff:10.11.12.1`  
Długość prefiksu przekonwertowana: 104

Można też podać adres w formacie szesnastkowym (`10.11.12.1` reprezentowane w protokole IPv6 jako `::ffff:0a0b:0c01`). Podczas konwertowania adres IPv4, IPv6, Dodaj 96 do długości prefiksów CIDR (`8` w przykładzie), aby uwzględnić dodatkowe `::ffff:` prefiks IPv6 (8 + 96 = 104). 

```csharp
// To access IPNetwork and IPAddress, add the following namespaces:
// using using System.Net;
// using Microsoft.AspNetCore.HttpOverrides;
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedHeaders =
        ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    options.KnownNetworks.Add(new IPNetwork(
        IPAddress.Parse("::ffff:10.11.12.1"), 104));
});
```

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

## <a name="forward-the-scheme-for-linux-and-non-iis-reverse-proxies"></a>Do przodu schematu dla systemów Linux i innej niż IIS zwrotnych serwerów proxy

Wywołanie szablonów platformy .NET core <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*> i <xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>. Te metody umieść lokacji wejścia w nieskończoną pętlę Jeśli wdrożony do usługi aplikacji systemu Linux platformy Azure, Azure Linux maszyny wirtualnej (VM), lub za wszystkie pozostałe zwrotny serwer proxy oprócz usług IIS. Protokół TLS jest zakończony przez zwrotny serwer proxy i Kestrel nie jest powiadomieni schematu właściwe żądanie. OAuth i OIDC również zakończyć się niepowodzeniem w tej konfiguracji ponieważ generują niepoprawne przekierowania. <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> dodaje i konfiguruje przekazany oprogramowania pośredniczącego nagłówków, gdy działające poza usługą IIS, ale brak pasującego automatycznej konfiguracji dla systemu Linux (Integracja Apache i Nginx).

Aby przesłać dalej schemat z serwera proxy w scenariuszach innej niż IIS, należy dodać i skonfigurować przekazywane oprogramowania pośredniczącego nagłówków. W `Startup.ConfigureServices`, użyj następującego kodu:

```csharp
// using Microsoft.AspNetCore.HttpOverrides;

if (string.Equals(
    Environment.GetEnvironmentVariable("ASPNETCORE_FORWARDEDHEADERS_ENABLED"), 
    "true", StringComparison.OrdinalIgnoreCase))
{
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = ForwardedHeaders.XForwardedFor | 
            ForwardedHeaders.XForwardedProto;
        // Only loopback proxies are allowed by default.
        // Clear that restriction because forwarders are enabled by explicit 
        // configuration.
        options.KnownNetworks.Clear();
        options.KnownProxies.Clear();
    });
}
```

Dopóki nowe obrazy kontenera znajdują się na platformie Azure, musisz utworzyć ustawienie aplikacji (zmienną środowiskową) dla `ASPNETCORE_FORWARDEDHEADERS_ENABLED` równa `true`. Aby uzyskać więcej informacji, zobacz [szablony nie działają w systemie Linux dostawcy Antares ze względu na Brak schematu usług przesyłania dalej (aspnet/AspNetCore #4135)](https://github.com/aspnet/AspNetCore/issues/4135).

::: moniker-end

## <a name="troubleshoot"></a>Rozwiązywanie problemów

Gdy nagłówki nie są przekazywane, zgodnie z oczekiwaniami, Włącz [rejestrowania](xref:fundamentals/logging/index). Jeśli dzienniki nie zapewniają wystarczające informacje, aby rozwiązać problem, wyliczanie nagłówków żądania odebranego przez serwer. Oprogramowanie pośredniczące wbudowane umożliwia zapisania nagłówki żądania odpowiedzi aplikacji lub dziennika nagłówków. 

Do zapisania nagłówki odpowiedzi aplikacji, należy umieścić następujące oprogramowanie pośredniczące terminalu wbudowane natychmiast po wywołaniu <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> w `Startup.Configure`:

```csharp
app.Run(async (context) =>
{
    context.Response.ContentType = "text/plain";

    // Request method, scheme, and path
    await context.Response.WriteAsync(
        $"Request Method: {context.Request.Method}{Environment.NewLine}");
    await context.Response.WriteAsync(
        $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
    await context.Response.WriteAsync(
        $"Request Path: {context.Request.Path}{Environment.NewLine}");

    // Headers
    await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

    foreach (var header in context.Request.Headers)
    {
        await context.Response.WriteAsync($"{header.Key}: " +
            $"{header.Value}{Environment.NewLine}");
    }

    await context.Response.WriteAsync(Environment.NewLine);

    // Connection: RemoteIp
    await context.Response.WriteAsync(
        $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
});
```

Możesz zapisywać dzienniki zamiast treść odpowiedzi. Zapisywanie dzienników zazwyczaj umożliwia witryny funkcji podczas debugowania.

Zapisywanie dzienników, a nie treści odpowiedzi:

* Wstrzykiwanie `ILogger<Startup>` do `Startup` klasy zgodnie z opisem w [twórz dzienniki w uruchamiania](xref:fundamentals/logging/index#create-logs-in-startup).
* Umieść następujące oprogramowanie pośredniczące wbudowane natychmiast po wywołaniu <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersExtensions.UseForwardedHeaders*> w `Startup.Configure`.

```csharp
app.Use(async (context, next) =>
{
    // Request method, scheme, and path
    _logger.LogDebug("Request Method: {METHOD}", context.Request.Method);
    _logger.LogDebug("Request Scheme: {SCHEME}", context.Request.Scheme);
    _logger.LogDebug("Request Path: {PATH}", context.Request.Path);

    // Headers
    foreach (var header in context.Request.Headers)
    {
        _logger.LogDebug("Header: {KEY}: {VALUE}", header.Key, header.Value);
    }

    // Connection: RemoteIp
    _logger.LogDebug("Request RemoteIp: {REMOTE_IP_ADDRESS}", 
        context.Connection.RemoteIpAddress);

    await next();
});
```

Podczas przetwarzania `X-Forwarded-{For|Proto|Host}` wartości są przenoszone do `X-Original-{For|Proto|Host}`. Jeśli dany nagłówek jest wiele wartości, zanotuj nagłówki procesów przekazywanych oprogramowania pośredniczącego nagłówków w odwrotnej kolejności od prawej do lewej. Wartość domyślna `ForwardLimit` jest 1 (jeden), dlatego wartość prawej z nagłówków jest przetwarzany, chyba że wartość `ForwardLimit` wzrasta.

Oryginalny zdalny adres IP żądania musi pasować do wpisu w `KnownProxies` lub `KnownNetworks` Wyświetla przed nagłówki przekazywane są przetwarzane. Ogranicza to nagłówek fałszowania przez nie akceptuje usług przesyłania dalej z niezaufanych serwerów proxy. Po wykryciu nieznany proxy rejestrowania wskazuje adres serwera proxy:

```console
September 20th 2018, 15:49:44.168 Unknown proxy: 10.0.0.100:54321
```

W powyższym przykładzie 10.0.0.100 jest serwer proxy. Jeśli serwer jest zaufany serwer proxy, należy dodać adres IP serwera do `KnownProxies` (lub zaufanej sieci, aby dodać `KnownNetworks`) w `Startup.ConfigureServices`. Aby uzyskać więcej informacji, zobacz [opcje przekazywane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options) sekcji.

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

> [!IMPORTANT]
> Zezwalaj tylko zaufanych serwerów proxy i sieci do przekazywania nagłówków. W przeciwnym razie [fałszowanie adresów IP](https://www.iplocation.net/ip-spoofing) ataki są możliwe.

## <a name="certificate-forwarding"></a>Przekazywanie certyfikatu 

### <a name="on-azure"></a>Na platformie Azure

Zobacz [dokumentacji platformy Azure](/azure/app-service/app-service-web-configure-tls-mutual-auth) do konfigurowania usługi Azure Web Apps. W swojej aplikacji `Startup.Configure` metody, Dodaj następujący kod przed wywołaniem do `app.UseAuthentication();`:

```csharp
app.UseCertificateForwarding();
```

Należy również skonfigurować oprogramowanie pośredniczące przekazywania certyfikatu, aby określić nazwę nagłówka, która korzysta z platformy Azure. W swojej aplikacji `Startup.ConfigureServices` metody, Dodaj następujący kod, aby skonfigurować nagłówek, w którym oprogramowanie pośredniczące tworzy certyfikat:

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "X-ARR-ClientCert");
```

### <a name="with-other-web-proxies"></a>Za pomocą innych serwerów proxy sieci web

Jeśli używasz serwera proxy, który nie jest lub platformy Azure Web Apps Routing żądań aplikacji IIS, należy skonfigurować serwer proxy do przekazywania certyfikatu otrzymanego w nagłówku HTTP. W swojej aplikacji `Startup.Configure` metody, Dodaj następujący kod przed wywołaniem do `app.UseAuthentication();`:

```csharp
app.UseCertificateForwarding();
```

Należy również skonfigurować oprogramowanie pośredniczące przekazywania certyfikatu, aby określić nazwę nagłówka. W swojej aplikacji `Startup.ConfigureServices` metody, Dodaj następujący kod, aby skonfigurować nagłówek, w którym oprogramowanie pośredniczące tworzy certyfikat:

```csharp
services.AddCertificateForwarding(options =>
    options.CertificateHeader = "YOUR_CERTIFICATE_HEADER_NAME");
```

Ponadto jeśli serwer proxy jest coś innego niż base64 kodowanie certyfikatu (tak jak w przypadku przy użyciu serwera Nginx) ustaw `HeaderConverter` opcji. Rozważmy następujący przykład w `Startup.ConfigureServices`:

```csharp
services.AddCertificateForwarding(options =>
{
    options.CertificateHeader = "YOUR_CUSTOM_HEADER_NAME";
    options.HeaderConverter = (headerValue) => 
    {
        var clientCertificate = 
           /* some conversion logic to create an X509Certificate2 */
        return clientCertificate;
    }
});
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/web-farm>
* [Microsoft Security Advisory CVE-2018-0787: Platforma ASP.NET Core podniesienie uprawnień](https://github.com/aspnet/Announcements/issues/295)
