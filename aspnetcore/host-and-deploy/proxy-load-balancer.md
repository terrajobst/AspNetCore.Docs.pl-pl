---
title: Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia
author: guardrex
description: Więcej informacji o konfiguracji dla aplikacji hostowanych za serwery proxy i moduły równoważenia obciążenia, które często zasłaniać żądanie ważnych informacji.
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2018
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: 255baf5570fc5127718aafcb3170bc3d00f00c91
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040072"
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
| X-Forwarded dla | Przechowuje informacje dotyczące klienta, który zainicjował żądań i kolejne serwery proxy w łańcuchu serwery proxy. Ten parametr może zawierać adresów IP adresy (i, opcjonalnie, numery portów). Łańcuch serwerów proxy pierwszy parametr wskazuje klienta, w którym żądanie zostało wystosowane najpierw. Postępuj zgodnie z identyfikatorami kolejnych serwera proxy. Ostatni serwer proxy w łańcuchu nie ma na liście parametrów. Adres IP ostatniego serwera proxy i opcjonalnie numeru portu są dostępne jako zdalny adres IP w warstwie transportowej. |
| X-Forwarded-Proto | Wartość źródłowy schemat (HTTP/HTTPS). Wartość może być też listy programów, jeśli żądanie jest przesunięta wielu serwerów proxy. |
| X-Forwarded-Host | Oryginalna wartość pola nagłówka hosta. Zazwyczaj serwery proxy nie należy modyfikować nagłówka hosta. Zobacz [Microsoft Security doradztwa technicznego dotyczącego CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) uzyskać informacji na temat luk w zabezpieczeniach typu "podniesienia uprawnień", który wpływa na systemach, w którym serwer proxy nie weryfikuje lub nagłówki hosta restict znane dobre wartości. |

Pośredniczącym nagłówki przekazywane z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu, odczytuje tych nagłówków i wypełnia skojarzonymi polami na [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

Aktualizacje oprogramowania pośredniczącego:

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; można ustawić przy użyciu `X-Forwarded-For` wartość nagłówka. Dodatkowe ustawienia mają wpływ na sposób ustawiania oprogramowanie pośredniczące `RemoteIpAddress`. Aby uzyskać więcej informacji, zobacz [opcje przekazywane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; można ustawić przy użyciu `X-Forwarded-Proto` wartość nagłówka.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; można ustawić przy użyciu `X-Forwarded-Host` wartość nagłówka.

Oprogramowanie pośredniczące nagłówki przekazywane [domyślne ustawienia](#forwarded-headers-middleware-options) można skonfigurować. Ustawienia domyślne są:

* Jest dostępny tylko *jeden serwer proxy* między aplikacją a źródło żądania.
* Tylko adresy sprzężenia zwrotnego są skonfigurowane dla znanych serwerów proxy i znane sieci.
* Nagłówki przekazywane są nazywane `X-Forwarded-For` i `X-Forwarded-Proto`.

Nie wszystkie urządzenia sieciowe, Dodaj `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki bez dodatkowej konfiguracji. Sprawdź wskazówki z producentem urządzenia, jeśli żądań kierowanych na serwer proxy nie zawierają tych nagłówków, gdy osiągną oni aplikację. Jeśli urządzenie korzysta z nazw nagłówka innego niż `X-Forwarded-For` i `X-Forwarded-Proto`ustaw [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) i [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) opcje można dopasować nazw nagłówka używany przez urządzenie. Aby uzyskać więcej informacji, zobacz [opcje przekazywane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options) i [konfigurację serwera proxy, który korzysta z nazw inny nagłówek](#configuration-for-a-proxy-that-uses-different-header-names).

## <a name="iisiis-express-and-aspnet-core-module"></a>Usługi IIS/IIS Express i modułów platformy ASP.NET Core

Przekazane nagłówki oprogramowanie pośredniczące jest włączona domyślnie oprogramowania pośredniczącego integracji usługi IIS, po uruchomieniu aplikacji usług IIS oraz modułu ASP.NET Core. Przekazane nagłówki oprogramowanie pośredniczące jest aktywowana ma być uruchomiony w potoku oprogramowania pośredniczącego przy użyciu określonej konfiguracji ograniczone do modułu ASP.NET Core ze względu na wątpliwości relacji zaufania z nagłówkami przekazywane (na przykład [fałszowanie adresów IP](https://www.iplocation.net/ip-spoofing)). Oprogramowanie pośredniczące jest skonfigurowana do przesyłania dalej `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków i jest ograniczona do jednego hosta lokalnego serwera proxy. Jeśli wymagana jest dodatkowa konfiguracja, zobacz [opcje przekazywane oprogramowania pośredniczącego nagłówki](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Inne serwera proxy i scenariuszy usługi równoważenia obciążenia

Poza za pomocą oprogramowania pośredniczącego integracji usługi IIS, przekazywane oprogramowania pośredniczącego nagłówków nie jest domyślnie włączone. Przekazane nagłówki oprogramowania pośredniczącego musi być włączona dla aplikacji do nagłówków procesów przekazywanych za pomocą [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Po włączeniu oprogramowanie pośredniczące, jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) określonych do oprogramowania pośredniczącego, domyślnie [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) są [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Konfigurowanie oprogramowania pośredniczącego z [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków w `Startup.ConfigureServices`. Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem innym oprogramowaniu pośredniczącym:

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
> Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone w `Startup.ConfigureServices` lub bezpośrednio do metody rozszerzenia za pomocą [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), wartość domyślna nagłówki do przesyłania dalej są [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) właściwość musi być skonfigurowany przy użyciu nagłówków do przesyłania dalej.

## <a name="nginx-configuration"></a>Konfigurację serwera Nginx

Do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków, zobacz <xref:host-and-deploy/linux-nginx#configure-nginx>. Aby uzyskać więcej informacji, zobacz [NGINX: przy użyciu nagłówka przesłanym](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/).

## <a name="apache-configuration"></a>Konfiguracja Apache

`X-Forwarded-For` jest automatycznie dodawany (zobacz [mod_proxy modułu Apache: Reverse Proxy nagłówkami żądań](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)). Aby uzyskać informacje na temat sposobu przekazywania `X-Forwarded-Proto` nagłówka, zobacz <xref:host-and-deploy/linux-apache#configure-apache>.

## <a name="forwarded-headers-middleware-options"></a>Przekazane opcje oprogramowania pośredniczącego nagłówki

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) sterowania zachowaniem oprogramowania pośredniczącego nagłówki przekazywane. Poniższy przykład umożliwia zmianę wartości domyślne:

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

::: moniker range=">= aspnetcore-2.1"
| Opcja | Opis |
| ------ | ----------- |
| AllowedHosts | Ogranicza hosty `X-Forwarded-Host` nagłówka do podanych wartości.<ul><li>Wartości są porównywane, liczba porządkowa Ignoruj-wielkie i małe litery.</li><li>Numery portów muszą zostać wykluczone.</li><li>Jeśli lista jest pusta, wszystkie hosty są dozwolone.</li><li>Symbol wieloznaczny najwyższego poziomu `*` zezwala na wszystkich hostach niepusta.</li><li>Poddomena symbole wieloznaczne są dozwolone, ale nie są zgodne z domeny katalogu głównego. Na przykład `*.contoso.com` odpowiada poddomeny `foo.contoso.com` , ale nie domeny katalogu głównego `contoso.com`.</li><li>Nazwy hostów Unicode są dozwolone, ale są konwertowane na [Punycode](https://tools.ietf.org/html/rfc3492) do dopasowania.</li><li>[Adresy IPv6](https://tools.ietf.org/html/rfc4291) musi zawierać blokujących nawiasy kwadratowe i musi być w [konwencjonalne formularza](https://tools.ietf.org/html/rfc4291#section-2.2) (na przykład `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). Adresy IPv6 nie są specjalne — z uwzględnieniem wielkości liter do sprawdzenia pod kątem równości logicznych między różnych formatów, a nie kanoniczną jest wykonywane.</li><li>Nie można ograniczyć dozwolone hosty mogą zezwolić osobie atakującej podszywały się pod łącza wygenerowanej przez usługę.</li></ul>Wartość domyślna to pusta [IList\<ciągu >](/dotnet/api/system.collections.generic.ilist-1). |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername). Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-For` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.<br><br>Wartość domyślna to `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Określa, które usług przesyłania dalej powinny zostać przetworzone. Zobacz [wyliczenia ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) dla listy pól, które są stosowane. Typowe wartości przypisane do tej właściwości to <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Wartość domyślna to [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername). Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-Host` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.<br><br>Wartość domyślna to `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername). Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-Proto` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.<br><br>Wartość domyślna to `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Ogranicza liczbę wpisów w nagłówki, które są przetwarzane. Ustaw `null` wyłączyć limit, ale powinno mieć miejsce tylko jeśli `KnownProxies` lub `KnownNetworks` są skonfigurowane.<br><br>Domyślnym ustawieniem jest 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Zakresy znanych sieci w celu akceptowania nagłówki przekazywane z adresów. Podaj zakresów adresów IP przy użyciu notacji Bezklasowego routingu międzydomenowego (CIDR).<br><br>Jeśli serwer używa podwójnych gniazd, adresy IPv4, są dostarczane w formacie protokołu IPv6 (na przykład `10.0.0.1` w protokole IPv4 reprezentowane w protokole IPv6 jako `::ffff:10.0.0.1`). Zobacz [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Określić, jeśli ten format jest wymagany, analizując [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Aby uzyskać więcej informacji, zobacz [konfigurację dla adresu IPv4 reprezentowane jako adres IPv6](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) sekcji.<br><br>Wartość domyślna to [IList](/dotnet/api/system.collections.generic.ilist-1)\<[wspólny](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> zawierający pojedynczy wpis dla `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Adresy znanych serwerów proxy, aby zaakceptować nagłówki przekazywane z. Użyj `KnownProxies` określić dokładny adres IP jest zgodny.<br><br>Jeśli serwer używa podwójnych gniazd, adresy IPv4, są dostarczane w formacie protokołu IPv6 (na przykład `10.0.0.1` w protokole IPv4 reprezentowane w protokole IPv6 jako `::ffff:10.0.0.1`). Zobacz [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Określić, jeśli ten format jest wymagany, analizując [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Aby uzyskać więcej informacji, zobacz [konfigurację dla adresu IPv4 reprezentowane jako adres IPv6](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) sekcji.<br><br>Wartość domyślna to [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> zawierający pojedynczy wpis dla `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Wartość domyślna to `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Wartość domyślna to `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Wartość domyślna to `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Wymagana jest liczba wartości nagłówka, aby był zsynchronizowany między [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) przetwarzany.<br><br>Wartość domyślna w programie ASP.NET Core 1.x jest `true`. Domyślnie w programie ASP.NET Core 2.0 lub nowszym `false`. |
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
| Opcja | Opis |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername). Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-For` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.<br><br>Wartość domyślna to `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Określa, które usług przesyłania dalej powinny zostać przetworzone. Zobacz [wyliczenia ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) dla listy pól, które są stosowane. Typowe wartości przypisane do tej właściwości to <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>Wartość domyślna to [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername). Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-Host` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.<br><br>Wartość domyślna to `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername). Ta opcja jest używana, gdy nie korzysta z serwera proxy/usługi przesyłania dalej `X-Forwarded-Proto` nagłówka, ale używa niektórych innych nagłówka do przekazywania informacji.<br><br>Wartość domyślna to `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Ogranicza liczbę wpisów w nagłówki, które są przetwarzane. Ustaw `null` wyłączyć limit, ale powinno mieć miejsce tylko jeśli `KnownProxies` lub `KnownNetworks` są skonfigurowane.<br><br>Domyślnym ustawieniem jest 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Zakresy znanych sieci w celu akceptowania nagłówki przekazywane z adresów. Podaj zakresów adresów IP przy użyciu notacji Bezklasowego routingu międzydomenowego (CIDR).<br><br>Jeśli serwer używa podwójnych gniazd, adresy IPv4, są dostarczane w formacie protokołu IPv6 (na przykład `10.0.0.1` w protokole IPv4 reprezentowane w protokole IPv6 jako `::ffff:10.0.0.1`). Zobacz [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Określić, jeśli ten format jest wymagany, analizując [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Aby uzyskać więcej informacji, zobacz [konfigurację dla adresu IPv4 reprezentowane jako adres IPv6](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) sekcji.<br><br>Wartość domyślna to [IList](/dotnet/api/system.collections.generic.ilist-1)\<[wspólny](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> zawierający pojedynczy wpis dla `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Adresy znanych serwerów proxy, aby zaakceptować nagłówki przekazywane z. Użyj `KnownProxies` określić dokładny adres IP jest zgodny.<br><br>Jeśli serwer używa podwójnych gniazd, adresy IPv4, są dostarczane w formacie protokołu IPv6 (na przykład `10.0.0.1` w protokole IPv4 reprezentowane w protokole IPv6 jako `::ffff:10.0.0.1`). Zobacz [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Określić, jeśli ten format jest wymagany, analizując [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*). Aby uzyskać więcej informacji, zobacz [konfigurację dla adresu IPv4 reprezentowane jako adres IPv6](#configuration-for-an-ipv4-address-represented-as-an-ipv6-address) sekcji.<br><br>Wartość domyślna to [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> zawierający pojedynczy wpis dla `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>Wartość domyślna to `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>Wartość domyślna to `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Użyj wartości header określonej przez tę właściwość, zamiast określonego przez [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>Wartość domyślna to `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Wymagana jest liczba wartości nagłówka, aby był zsynchronizowany między [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) przetwarzany.<br><br>Wartość domyślna w programie ASP.NET Core 1.x jest `true`. Domyślnie w programie ASP.NET Core 2.0 lub nowszym `false`. |
::: moniker-end

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

Niektóre serwery proxy Przekaż ścieżkę bez zmian, ale z aplikacją ścieżki podstawowej, które powinno zostać usunięte, tak aby routing działa poprawnie. [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) oprogramowania pośredniczącego dzieli ścieżkę do [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) i ścieżki podstawowej aplikacji do [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Jeśli `/foo` jest ścieżką serwera proxy są przekazywane jako ścieżki podstawowej aplikacji `/foo/api/1`, zestawy oprogramowania pośredniczącego `Request.PathBase` do `/foo` i `Request.Path` do `/api/1` za pomocą następującego polecenia:

```csharp
app.UsePathBase("/foo");
```

Oryginalna ścieżka i podstawa ścieżki zostaną ponownie zastosowane, gdy oprogramowanie pośredniczące wywoływana jest ponownie w odwrotnej kolejności. Aby uzyskać więcej informacji na temat przetwarzania zamówień oprogramowanie pośredniczące, zobacz <xref:fundamentals/middleware/index>.

Jeśli serwer proxy usuwa ścieżki (na przykład przekazywania `/foo/api/1` do `/api/1`), poprawka przekierowuje i łączy, ustawiając żądania [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) właściwości:

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Jeśli serwer proxy dodaje dane ścieżki, odrzucić część ścieżki rozwiązać za pomocą przekierowań i łącza [StartsWithSegments (ciąg PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) i przypisywanie do [ścieżki](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) właściwości:

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

Jeśli serwer proxy nie używa nagłówków o nazwie `X-Forwarded-For` i `X-Forwarded-Proto` do przekazywania adresów/port serwera proxy i pochodzącymi informacji schematu, ustaw [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) i [ForwardedProtoHeaderName ](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) opcje można dopasować nazw nagłówka używany przez serwer proxy:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedForHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-For_Header";
    options.ForwardedProtoHeaderName = "Header_Name_Used_By_Proxy_For_X-Forwarded-Proto_Header";
});
```

### <a name="configuration-for-an-ipv4-address-represented-as-an-ipv6-address"></a>Konfiguracja dla adresu IPv4, reprezentowane jako adres IPv6

Jeśli serwer używa podwójnych gniazd, adresy IPv4, są dostarczane w formacie protokołu IPv6 (na przykład `10.0.0.1` w protokole IPv4 reprezentowane w protokole IPv6 jako `::ffff:10.0.0.1` lub `::ffff:a00:1`). Zobacz [IPAddress.MapToIPv6](xref:System.Net.IPAddress.MapToIPv6*). Określić, jeśli ten format jest wymagany, analizując [HttpContext.Connection.RemoteIpAddress](xref:Microsoft.AspNetCore.Http.ConnectionInfo.RemoteIpAddress*).

W poniższym przykładzie adresu sieciowego, który dostarcza nagłówki przekazywane jest dodawany do `KnownNetworks` listy w formacie IPv6:

Krótki format IPv6 `10.11.12.1/8`:

* `::ffff:0a0b:0c01`
* Długość prefiksu: 104 (8 + 96&dagger;)

```csharp
// To access IPNetwork and IPAddress, add the following namespaces:
// using using System.Net;
// using Microsoft.AspNetCore.HttpOverrides;
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardedHeaders =
        ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    options.KnownNetworks.Add(new IPNetwork(
        IPAddress.Parse("::ffff:0a0b:0c01"), 104));
});
```

&dagger;Podczas konwertowania adres IPv4, IPv6, należy dodać 96 do długości prefiksu CIDR w celu uwzględnienia dodatkowych `::ffff:` prefiks IPv6.

## <a name="troubleshoot"></a>Rozwiązywanie problemów

Gdy nagłówki nie są przekazywane, zgodnie z oczekiwaniami, Włącz [rejestrowania](xref:fundamentals/logging/index). Jeśli dzienniki nie zapewniają wystarczające informacje, aby rozwiązać problem, wyliczanie nagłówków żądania odebranego przez serwer. Może być zapisany nagłówki odpowiedzi aplikacji za pomocą oprogramowania pośredniczącego wbudowany:

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
    });
}
```

Upewnij się, że X - Forwarded-* nagłówki są odbierane przez serwer z oczekiwanych wartości. Jeśli dany nagłówek jest wiele wartości, zanotuj nagłówki procesów przekazywanych oprogramowania pośredniczącego nagłówków w odwrotnej kolejności od prawej do lewej.

Oryginalny zdalny adres IP żądania musi pasować do wpisu w `KnownProxies` lub `KnownNetworks` Wyświetla przed `X-Forwarded-For` jest przetwarzany. Ogranicza to nagłówek fałszowania przez nie akceptuje usług przesyłania dalej z niezaufanych serwerów proxy.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/web-farm>
* [Microsoft Security Advisory CVE-2018-0787: Platforma ASP.NET Core podniesienie uprawnień](https://github.com/aspnet/Announcements/issues/295)
