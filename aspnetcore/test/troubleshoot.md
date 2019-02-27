---
title: Rozwiązywanie problemów z projektami ASP.NET Core
author: Rick-Anderson
description: Omówienie i rozwiązywanie problemów, ostrzeżenia i błędy w projektach programu ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: test/troubleshoot
ms.openlocfilehash: c8b34f51fd329eb9a7c34f7be93bd7f2aa054283
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899295"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Rozwiązywanie problemów z projektami ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Poniższe łącza zapewniają wskazówki dotyczące rozwiązywania problemów:

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Ndc Developers Conference (2018 r. Londyn;): Diagnozowanie problemów w aplikacji programu ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET Blog: Rozwiązywanie problemów z wydajnością programu ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Ostrzeżenia dotyczące zestawu .NET core SDK

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>32- i 64-bitowe wersje programu .NET Core SDK są instalowane.

W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:

> Zarówno 32- i 64 bitowych wersjach programu .NET Core SDK są instalowane. Tylko szablony z wersji 64-bitowych zainstalowanej w lokalizacji "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/both32and64bit.png)

To ostrzeżenie jest wyświetlane, gdy zarówno wersji 64-bitowych (x 64), jak i 32-bitowych (x86) [zestawu .NET Core SDK](https://www.microsoft.com/net/download/all) są zainstalowane. Typowe przyczyny, które można zainstalować obie wersje obejmują:

* Pierwotnie pobrany przy użyciu komputera z 32-bitowy Instalator zestawu .NET Core SDK, ale następnie skopiować go na i zainstalować je na komputerze 64-bitowym.
* 32-bitowych .NET Core SDK został zainstalowany przez inną aplikację.
* Niewłaściwa wersja został pobrany i zainstalowany.

Odinstaluj 32-bitowych .NET Core SDK, aby uniknąć tego ostrzeżenia. Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**. Jeśli zrozumiesz, dlaczego występuje ostrzeżenie i jego skutków, możesz zignorować to ostrzeżenie.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK jest zainstalowany w wielu lokalizacjach

W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:

> .NET Core SDK jest zainstalowany w wielu lokalizacjach. Tylko szablony z zestawów SDK zainstalowanych na "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/multiplelocations.png)

Ten komunikat jest wyświetlany, gdy masz co najmniej jedna instalacja zestawu SDK programu .NET Core w katalogu, poza *C:\\Program Files\\dotnet\\sdk\\*. Zwykle dzieje się tak w przypadku zestawu .NET Core SDK został wdrożony na maszynie za pomocą kopiowania/wklejania zamiast Instalatora MSI.

Odinstaluj 32-bitowych .NET Core SDK, aby uniknąć tego ostrzeżenia. Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**. Jeśli zrozumiesz, dlaczego występuje ostrzeżenie i jego skutków, możesz zignorować to ostrzeżenie.

### <a name="no-net-core-sdks-were-detected"></a>Nie wykryto żadnych zestawów .NET Core SDK

W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:

> Nie wykryto żadnych zestawów .NET Core SDK, upewnij się, że są one uwzględnione w zmiennej środowiskowej "PATH".

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/NoNetCore.png)

To ostrzeżenie jest wyświetlane, gdy zmienna środowiskowa `PATH` nie wskazuje na żadnych zestawów .NET Core SDK na komputerze (na przykład `C:\Program Files\dotnet\` i `C:\Program Files (x86)\dotnet\`). Aby rozwiązać ten problem:

* Zainstaluj lub sprawdzić, czy jest zainstalowany zestaw .NET Core SDK. Uzyskaj najnowszą wersję Instalatora z [pobiera .NET](https://dotnet.microsoft.com/download). 
* Upewnij się, że `PATH` zmienna środowiskowa wskazuje lokalizację, w którym jest zainstalowany zestaw SDK. Instalator zwykle ustawia `PATH`.

## <a name="obtain-data-from-an-app"></a>Uzyskiwanie danych z aplikacji

Jeśli aplikacja jest w stanie odpowiadać na żądania, możesz uzyskać następujące dane z aplikacji za pomocą oprogramowania pośredniczącego:

* Żądanie &ndash; metody schematu, hosta, pathbase, ścieżki, ciąg, nagłówki zapytania
* Połączenie &ndash; zdalny adres IP, port zdalny, lokalny adres IP, port lokalny, certyfikat klienta
* Tożsamość &ndash; nazwę, nazwę wyświetlaną
* Ustawienia konfiguracji
* Zmienne środowiskowe

Umieść następujące wpisy [oprogramowania pośredniczącego](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) kod na początku `Startup.Configure` potoku przetwarzania żądań metody. Środowisko jest sprawdzany przed uruchomieniem oprogramowania pośredniczącego, aby upewnić się, że kod jest wykonywane tylko w środowisku programistycznym.

Aby uzyskać środowisko, użyj jednej z następujących metod:

* Wstrzykiwanie `IHostingEnvironment` do `Startup.Configure` metody i Sprawdź środowisko o zmiennej lokalnej. Następujący przykładowy kod pokazuje tego podejścia.

* Przypisz środowiska do właściwości w `Startup` klasy. Sprawdź środowisko przy użyciu właściwości (na przykład `if (Environment.IsDevelopment())`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, 
    IConfiguration config)
{
    if (env.IsDevelopment())
    {
        app.Run(async (context) =>
        {
            var sb = new StringBuilder();
            var nl = System.Environment.NewLine;
            var rule = string.Concat(nl, new string('-', 40), nl);
            var authSchemeProvider = app.ApplicationServices
                .GetRequiredService<IAuthenticationSchemeProvider>();

            sb.Append($"Request{rule}");
            sb.Append($"{DateTimeOffset.Now}{nl}");
            sb.Append($"{context.Request.Method} {context.Request.Path}{nl}");
            sb.Append($"Scheme: {context.Request.Scheme}{nl}");
            sb.Append($"Host: {context.Request.Headers["Host"]}{nl}");
            sb.Append($"PathBase: {context.Request.PathBase.Value}{nl}");
            sb.Append($"Path: {context.Request.Path.Value}{nl}");
            sb.Append($"Query: {context.Request.QueryString.Value}{nl}{nl}");

            sb.Append($"Connection{rule}");
            sb.Append($"RemoteIp: {context.Connection.RemoteIpAddress}{nl}");
            sb.Append($"RemotePort: {context.Connection.RemotePort}{nl}");
            sb.Append($"LocalIp: {context.Connection.LocalIpAddress}{nl}");
            sb.Append($"LocalPort: {context.Connection.LocalPort}{nl}");
            sb.Append($"ClientCert: {context.Connection.ClientCertificate}{nl}{nl}");

            sb.Append($"Identity{rule}");
            sb.Append($"User: {context.User.Identity.Name}{nl}");
            var scheme = await authSchemeProvider
                .GetSchemeAsync(IISDefaults.AuthenticationScheme);
            sb.Append($"DisplayName: {scheme?.DisplayName}{nl}{nl}");

            sb.Append($"Headers{rule}");
            foreach (var header in context.Request.Headers)
            {
                sb.Append($"{header.Key}: {header.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Websockets{rule}");
            if (context.Features.Get<IHttpUpgradeFeature>() != null)
            {
                sb.Append($"Status: Enabled{nl}{nl}");
            }
            else
            {
                sb.Append($"Status: Disabled{nl}{nl}");
            }

            sb.Append($"Configuration{rule}");
            foreach (var pair in config.AsEnumerable())
            {
                sb.Append($"{pair.Path}: {pair.Value}{nl}");
            }
            sb.Append(nl);

            sb.Append($"Environment Variables{rule}");
            var vars = System.Environment.GetEnvironmentVariables();
            foreach (var key in vars.Keys.Cast<string>().OrderBy(key => key, 
                StringComparer.OrdinalIgnoreCase))
            {
                var value = vars[key];
                sb.Append($"{key}: {value}{nl}");
            }

            context.Response.ContentType = "text/plain";
            await context.Response.WriteAsync(sb.ToString());
        });
    }
}
```
