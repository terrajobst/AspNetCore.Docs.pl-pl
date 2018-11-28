---
title: Rozwiązywanie problemów z projektami ASP.NET Core
author: Rick-Anderson
description: Omówienie i rozwiązywanie problemów, ostrzeżenia i błędy w projektach programu ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2018
uid: test/troubleshoot
ms.openlocfilehash: 7a3361970bde2b8761c76884fc1905957d075c5c
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450778"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="19568-103">Rozwiązywanie problemów z projektami ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19568-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="19568-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="19568-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="19568-105">Poniższe łącza zapewniają wskazówki dotyczące rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="19568-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="19568-106">Ndc Developers Conference (2018 r. Londyn;): Diagnozowanie problemów w aplikacji programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19568-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="19568-107">Blog platformy ASP.NET: Rozwiązywanie problemów z problemów z wydajnością programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19568-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="19568-108">Ostrzeżenia dotyczące zestawu .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="19568-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="19568-109">32- i 64-bitowe wersje programu .NET Core SDK są instalowane.</span><span class="sxs-lookup"><span data-stu-id="19568-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="19568-110">W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="19568-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="19568-111">Zarówno 32- i 64 bitowych wersjach programu .NET Core SDK są instalowane.</span><span class="sxs-lookup"><span data-stu-id="19568-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="19568-112">Tylko szablony z wersji 64-bitowych zainstalowanej w lokalizacji "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="19568-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="19568-114">To ostrzeżenie jest wyświetlane, gdy zarówno wersji 64-bitowych (x 64), jak i 32-bitowych (x86) [zestawu .NET Core SDK](https://www.microsoft.com/net/download/all) są zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="19568-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="19568-115">Typowe przyczyny, które można zainstalować obie wersje obejmują:</span><span class="sxs-lookup"><span data-stu-id="19568-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="19568-116">Pierwotnie pobrany przy użyciu komputera z 32-bitowy Instalator zestawu .NET Core SDK, ale następnie skopiować go na i zainstalować je na komputerze 64-bitowym.</span><span class="sxs-lookup"><span data-stu-id="19568-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="19568-117">32-bitowych .NET Core SDK został zainstalowany przez inną aplikację.</span><span class="sxs-lookup"><span data-stu-id="19568-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="19568-118">Niewłaściwa wersja został pobrany i zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="19568-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="19568-119">Odinstaluj 32-bitowych .NET Core SDK, aby uniknąć tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="19568-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="19568-120">Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**.</span><span class="sxs-lookup"><span data-stu-id="19568-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="19568-121">Jeśli zrozumiesz, dlaczego występuje ostrzeżenie i jego skutków, możesz zignorować to ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="19568-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="19568-122">.NET Core SDK jest zainstalowany w wielu lokalizacjach</span><span class="sxs-lookup"><span data-stu-id="19568-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="19568-123">W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="19568-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="19568-124">.NET Core SDK jest zainstalowany w wielu lokalizacjach.</span><span class="sxs-lookup"><span data-stu-id="19568-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="19568-125">Tylko szablony z zestawów SDK zainstalowanych na "C:\\Program Files\\dotnet\\sdk\\" będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="19568-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="19568-127">Ten komunikat jest wyświetlany, gdy masz co najmniej jedna instalacja zestawu SDK programu .NET Core w katalogu, poza *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="19568-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="19568-128">Zwykle dzieje się tak w przypadku zestawu .NET Core SDK został wdrożony na maszynie za pomocą kopiowania/wklejania zamiast Instalatora MSI.</span><span class="sxs-lookup"><span data-stu-id="19568-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="19568-129">Odinstaluj 32-bitowych .NET Core SDK, aby uniknąć tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="19568-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="19568-130">Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**.</span><span class="sxs-lookup"><span data-stu-id="19568-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="19568-131">Jeśli zrozumiesz, dlaczego występuje ostrzeżenie i jego skutków, możesz zignorować to ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="19568-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="19568-132">Nie wykryto żadnych zestawów .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="19568-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="19568-133">W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="19568-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="19568-134">Nie wykryto żadnych zestawów .NET Core SDK, upewnij się, że są one uwzględnione w zmiennej środowiskowej "PATH".</span><span class="sxs-lookup"><span data-stu-id="19568-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Zrzut ekranu okna dialogowego OneASP.NET przedstawiający komunikat ostrzegawczy](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="19568-136">To ostrzeżenie jest wyświetlane, gdy zmienna środowiskowa `PATH` nie wskazuje na żadnych zestawów .NET Core SDK na maszynie.</span><span class="sxs-lookup"><span data-stu-id="19568-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="19568-137">Aby rozwiązać ten problem:</span><span class="sxs-lookup"><span data-stu-id="19568-137">To resolve this problem:</span></span>

* <span data-ttu-id="19568-138">Zainstaluj lub sprawdzić, czy jest zainstalowany zestaw .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="19568-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="19568-139">Upewnij się, że `PATH` zmienna środowiskowa wskazuje lokalizację, w którym jest zainstalowany zestaw SDK.</span><span class="sxs-lookup"><span data-stu-id="19568-139">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="19568-140">Instalator zwykle ustawia `PATH`.</span><span class="sxs-lookup"><span data-stu-id="19568-140">The installer normally sets the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="19568-141">Uzyskiwanie danych z aplikacji</span><span class="sxs-lookup"><span data-stu-id="19568-141">Obtain data from an app</span></span>

<span data-ttu-id="19568-142">Jeśli aplikacja jest w stanie odpowiadać na żądania, możesz uzyskać następujące dane z aplikacji za pomocą oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="19568-142">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="19568-143">Żądanie &ndash; metody schematu, hosta, pathbase, ścieżki, ciąg, nagłówki zapytania</span><span class="sxs-lookup"><span data-stu-id="19568-143">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="19568-144">Połączenie &ndash; zdalny adres IP, port zdalny, lokalny adres IP, port lokalny, certyfikat klienta</span><span class="sxs-lookup"><span data-stu-id="19568-144">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="19568-145">Tożsamość &ndash; nazwę, nazwę wyświetlaną</span><span class="sxs-lookup"><span data-stu-id="19568-145">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="19568-146">Ustawienia konfiguracji</span><span class="sxs-lookup"><span data-stu-id="19568-146">Configuration settings</span></span>
* <span data-ttu-id="19568-147">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="19568-147">Environment variables</span></span>

<span data-ttu-id="19568-148">Umieść następujące wpisy [oprogramowania pośredniczącego](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) kod na początku `Startup.Configure` potoku przetwarzania żądań metody.</span><span class="sxs-lookup"><span data-stu-id="19568-148">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="19568-149">Środowisko jest sprawdzany przed uruchomieniem oprogramowania pośredniczącego, aby upewnić się, że kod jest wykonywane tylko w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="19568-149">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="19568-150">Aby uzyskać środowisko, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="19568-150">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="19568-151">Wstrzykiwanie `IHostingEnvironment` do `Startup.Configure` metody i Sprawdź środowisko o zmiennej lokalnej.</span><span class="sxs-lookup"><span data-stu-id="19568-151">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="19568-152">Następujący przykładowy kod pokazuje tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="19568-152">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="19568-153">Przypisz środowiska do właściwości w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="19568-153">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="19568-154">Sprawdź środowisko przy użyciu właściwości (na przykład `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="19568-154">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
