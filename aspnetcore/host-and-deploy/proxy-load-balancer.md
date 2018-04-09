---
title: Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia
author: guardrex
description: Informacje o konfiguracji dla aplikacji hostowanych serwerów proxy i równoważenia obciążenia, które często przesłaniać żądanie ważnych informacji.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia

Przez [Luke Latham](https://github.com/guardrex) i [Roaming Krzysztof](https://github.com/Tratcher)

W zalecanej konfiguracji dla platformy ASP.NET Core aplikacja jest hostowana przy użyciu modułu Core IIS/ASP.NET, Nginx lub Apache. Serwery proxy, usługi równoważenia obciążenia i innych urządzeniach sieciowych często przesłaniać informacje o żądaniu przed jego aplikacji:

* Gdy żądań HTTPS są przekazywane przez serwer proxy za pośrednictwem protokołu HTTP, oryginalny schemat (HTTPS) zostaną utracone i muszą być przekazywane w nagłówku.
* Ponieważ aplikacja odbiera żądanie z serwera proxy i jego wartość true źródła w Internecie lub sieci firmowej, źródłowy adres IP klienta, również muszą być przekazywane w nagłówku.

Te informacje mogą być ważne przetwarzania żądania, na przykład w przekierowania uwierzytelniania, generowania łącza, oceny zasad i geoloation klienta.

## <a name="forwarded-headers"></a>Nagłówki przekazany dalej

Konwencja serwery proxy przesyła informacje w nagłówkach HTTP.

| nagłówek | Opis |
| ------ | ----------- |
| X-Forwarded-For | Przechowuje informacje dotyczące klienta, który zainicjował żądanie i kolejne serwery proxy w łańcuchu serwerów proxy. Ten parametr może zawierać adresów IP adresy (i, opcjonalnie, numery portów). Łańcuch serwerów proxy pierwszy parametr wskazuje klienta, w którym najpierw żądanie. Wykonaj kolejne proxy identyfikatorów. Ostatni serwer proxy w łańcuchu nie ma na liście parametrów. Adres IP ostatniego serwera proxy i opcjonalnie numeru portu są dostępne jako zdalny adres IP w przypadku warstwy transportu. |
| X-Forwarded-Proto | Wartość źródłowego schemat (HTTP/HTTPS). Wartość może być również listy programów, jeśli żądanie ma przechodzić wielu serwerów proxy. |
| X-Forwarded-Host | Oryginalnej wartości pola nagłówka hosta. Zwykle nie należy modyfikować proxy nagłówka hosta. Zobacz [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) uzyskać informacji dotyczących usterki "podniesienia uprawnień", która dotyczy systemów, gdy serwer proxy nie sprawdzania poprawności lub nagłówków hosta restict znanej dobrej wartości. |

Przekazywane nagłówki oprogramowanie pośredniczące, z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu, odczytuje te nagłówki i wypełnia skojarzonymi polami na [element HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext). 

Aktualizacje oprogramowania pośredniczącego:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; ustawić za pomocą `X-Forwarded-For` wartość nagłówka. Dodatkowe ustawienia wpływu, jak oprogramowanie pośredniczące ustawia `RemoteIpAddress`. Aby uzyskać więcej informacji, zobacz [opcje przekazane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; ustawić za pomocą `X-Forwarded-Proto` wartość nagłówka.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; ustawić za pomocą `X-Forwarded-Host` wartość nagłówka.

Należy pamiętać, że nie wszystkie urządzenia sieciowe dodać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki bez dodatkowej konfiguracji. Jeśli żądań kierowanych na serwer proxy nie zawierają tych nagłówków, po upływie aplikacji, zapoznaj się wskazówek producenta urządzenia.

Nagłówki oprogramowanie pośredniczące przekazane [ustawienia domyślne](#forwarded-headers-middleware-options) można skonfigurować. Ustawienia domyślne są:

* Istnieje tylko *jeden serwer proxy* między źródła żądania i aplikacji.
* Tylko adresy sprzężenia zwrotnego są konfigurowane dla znanych proxy i znane sieci.

## <a name="iisiis-express-and-aspnet-core-module"></a>Usługi IIS/IIS Express i modułów platformy ASP.NET Core

Przekazany dalej nagłówki oprogramowanie pośredniczące jest domyślnie przez oprogramowanie pośredniczące integracji usług IIS, gdy aplikacja jest uruchamiana za usług IIS i moduł platformy ASP.NET Core. Przekazany dalej nagłówki oprogramowanie pośredniczące został aktywowany do uruchomienia pierwszy w potoku oprogramowania pośredniczącego z ograniczeniami konfiguracją określonego modułu platformy ASP.NET Core ze względu na problemy zaufania z nagłówkami przekazany dalej (na przykład [fałszowanie adresu IP](https://www.iplocation.net/ip-spoofing)). Oprogramowanie pośredniczące jest skonfigurowany do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki i jest ograniczone do jednego hosta lokalnego serwera proxy. Jeśli jest wymagana dodatkowa konfiguracja, zobacz [opcje przekazane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Inne serwera proxy i scenariuszy usługi równoważenia obciążenia

Poza za pomocą oprogramowania pośredniczącego integracji usług IIS, przekazywane oprogramowanie pośredniczące nagłówki nie jest domyślnie włączone. Przekazany dalej nagłówki oprogramowania pośredniczącego musi być włączony dla aplikacji do procesu przekazywane nagłówków z [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Po włączeniu oprogramowanie pośredniczące, jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) podano do oprogramowania pośredniczącego, domyślnie [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) są [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Konfigurowanie oprogramowania pośredniczącego z [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków w `Startup.ConfigureServices`. Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody `Startup.Configure` przed wywołaniem innych oprogramowania pośredniczącego:

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
> Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone w `Startup.ConfigureServices` lub bezpośrednio do metody rozszerzenia [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), wartość domyślna nagłówki do przekazywania są [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) właściwości musi być skonfigurowany z nagłówkami do przesyłania dalej.

## <a name="forwarded-headers-middleware-options"></a>Opcje oprogramowania pośredniczącego nagłówki przekazany dalej

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) sterowania zachowaniem oprogramowania pośredniczącego nagłówki przekazywane:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| Opcja | Opis |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>Wartość domyślna to `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Określa, które usługi przesyłania dalej powinna zostać przetworzona. Zobacz [ForwardedHeaders wyliczenia](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) dla listy pól, które są stosowane. Typowe wartości przypisane do tej właściwości to <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Wartość domyślna to [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>Wartość domyślna to `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>Wartość domyślna to `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Ogranicza liczbę wpisów w nagłówkach, które są przetwarzane. Ustaw `null` Aby wyłączyć limit, ale powinna być wykonywana tylko jeśli `KnownProxies` lub `KnownNetworks` są skonfigurowane.<br><br>Domyślnym ustawieniem jest 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Zakresy znanych serwerów proxy, aby zaakceptować przekazywane nagłówków z adresów. Podaj zakresów adresów IP za pomocą notacji międzydomenowego routingu CIDR (Classless).<br><br>Wartość domyślna to [IList](/dotnet/api/system.collections.generic.ilist-1)\<[wspólny](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> zawierający pojedynczy wpis dla `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Adresy znanych serwerów proxy, aby zaakceptować przekazywane nagłówków z. Użyj `KnownProxies` do określenia dokładny adres IP jest zgodny.<br><br>Wartość domyślna to [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> zawierający pojedynczy wpis dla `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Wartość domyślna to `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Wartość domyślna to `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określony [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Wartość domyślna to `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Wymagana liczba wartości nagłówka za synchronizację między [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) przetwarzane.<br><br>Domyślnie platformy ASP.NET Core 1.x jest `true`. Domyślnie w programie ASP.NET Core 2.0 lub nowszej jest `false`. |

## <a name="scenarios-and-use-cases"></a>Scenariusze i przypadki użycia

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>W przypadku nie można dodać przekazywane nagłówki i wszystkich żądań jest bezpieczne

W niektórych przypadkach może być nie można dodać nagłówków przekazywane na żądanie przekazywane przez serwer proxy aplikacji. Jeśli serwer proxy jest wymuszania, czy wszystkie publicznego zewnętrznego żądania HTTPS, schemat można ręcznie ustawić w `Startup.Configure` przed użyciem dowolnego typu oprogramowania pośredniczącego:

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Ten kod można wyłączyć z inne ustawienia konfiguracji w środowisku przemieszczania lub rozwoju lub zmiennej środowiskowej.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Postępowania w przypadku ścieżki podstawowej i serwerów proxy, które ścieżka żądania zmiany

Niektóre serwery proxy przekazać ścieżka niezmienione, ale z aplikacją ścieżki podstawowej, która powinna zostać usunięta, dzięki czemu routing działa poprawnie. [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) oprogramowanie pośredniczące dzieli ścieżka [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) i ścieżki podstawowej aplikacji do [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Jeśli `/foo` jest ścieżki podstawowej aplikacji dla ścieżki serwera proxy przekazanego jako `/foo/api/1`, ustawia oprogramowanie pośredniczące `Request.PathBase` do `/foo` i `Request.Path` do `/api/1` przy użyciu następującego polecenia:

```csharp
app.UsePathBase("/foo");
```

Oryginalna ścieżka i podstawa ścieżki zostaną ponownie zastosowane, gdy oprogramowanie pośredniczące nie zostanie ponownie wywołany odwrotnie. Aby uzyskać więcej informacji na temat oprogramowania pośredniczącego kolejność przetwarzania zobacz [oprogramowanie pośredniczące](xref:fundamentals/middleware/index).

Jeśli serwer proxy przycina ścieżki (na przykład przekazywania `/foo/api/1` do `/api/1`), poprawka przekierowuje i łączy przez ustawienie żądania [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) właściwości:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Jeśli ścieżka danych polega na dodaniu serwera proxy, pozbyć się części ścieżki ustalenie przekierowania i łącza za pomocą [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) i przypisywanie do [ścieżki](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) właściwości:

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

## <a name="troubleshoot"></a>Rozwiązywanie problemów

Gdy nagłówki nie są przekazywane, zgodnie z oczekiwaniami, należy włączyć [rejestrowanie](xref:fundamentals/logging/index). Jeśli dzienniki nie zawierają wystarczającej ilości informacji do rozwiązania problemu, wyliczanie nagłówków żądania otrzymane przez serwer. Można zapisać nagłówków odpowiedzi aplikacji za pomocą oprogramowania pośredniczącego wbudowany:

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
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
    }
}
```

Upewnij się, że X - przekazywane-* nagłówki są odbierane przez serwer z oczekiwanych wartości. Jeśli podany nagłówek jest wiele wartości, weź pod uwagę nagłówków procesów przekazywane oprogramowanie pośredniczące nagłówków w odwrotnej kolejności od prawej do lewej.

Żądania oryginalnego zdalny adres IP musi być zgodna wpis w `KnownProxies` lub `KnownNetworks` wymieniono przed X przekazywane do przetwarzania. To ogranicza nagłówka fałszowania przez nie akceptuje z niezaufanych serwerów proxy usługi przesyłania dalej.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Microsoft Security Advisory CVE-2018-0787: Platformy ASP.NET Core zagrożenie podniesieniem poziomu uprawnień](https://github.com/aspnet/Announcements/issues/295)
