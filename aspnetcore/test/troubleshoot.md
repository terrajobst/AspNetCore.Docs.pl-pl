---
title: Rozwiązywanie problemów z projektami ASP.NET Core
author: Rick-Anderson
description: Omówienie i rozwiązywanie problemów, ostrzeżenia i błędy w projektach programu ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: test/troubleshoot
ms.openlocfilehash: bcec8a55a5111e1f3acf53ae2f57b45e6e609d25
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313676"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="a4df2-103">Rozwiązywanie problemów z projektami ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4df2-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="a4df2-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a4df2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a4df2-105">Poniższe łącza zapewniają wskazówki dotyczące rozwiązywania problemów:</span><span class="sxs-lookup"><span data-stu-id="a4df2-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="a4df2-106">Ndc Developers Conference (2018 r. Londyn;): Diagnozowanie problemów w aplikacji programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4df2-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="a4df2-107">ASP.NET Blog: Rozwiązywanie problemów z wydajnością programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4df2-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="a4df2-108">Ostrzeżenia dotyczące zestawu .NET core SDK</span><span class="sxs-lookup"><span data-stu-id="a4df2-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="a4df2-109">32-bitowych i 64-bitowe wersje programu .NET Core SDK są instalowane.</span><span class="sxs-lookup"><span data-stu-id="a4df2-109">Both the 32-bit and 64-bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="a4df2-110">W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="a4df2-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="a4df2-111">Zarówno 32-bitowych i 64-bitowe wersje programu .NET Core SDK są instalowane.</span><span class="sxs-lookup"><span data-stu-id="a4df2-111">Both 32-bit and 64-bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="a4df2-112">Tylko szablony z 64-bitowe wersje zainstalowane na "C:\\Program Files\\dotnet\\sdk\\" są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="a4df2-112">Only templates from the 64-bit versions installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="a4df2-113">To ostrzeżenie jest wyświetlane, gdy zarówno wersji 64-bitowych (x 64), jak i 32-bitowych (x86) [zestawu .NET Core SDK](https://www.microsoft.com/net/download/all) są zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="a4df2-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="a4df2-114">Typowe przyczyny, które można zainstalować obie wersje obejmują:</span><span class="sxs-lookup"><span data-stu-id="a4df2-114">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="a4df2-115">Pierwotnie pobrany przy użyciu komputera z 32-bitowy Instalator zestawu .NET Core SDK, ale następnie skopiować go na i zainstalować je na komputerze 64-bitowym.</span><span class="sxs-lookup"><span data-stu-id="a4df2-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="a4df2-116">32-bitowych .NET Core SDK został zainstalowany przez inną aplikację.</span><span class="sxs-lookup"><span data-stu-id="a4df2-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="a4df2-117">Niewłaściwa wersja został pobrany i zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="a4df2-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="a4df2-118">Odinstaluj 32-bitowych .NET Core SDK, aby uniknąć tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="a4df2-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="a4df2-119">Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**.</span><span class="sxs-lookup"><span data-stu-id="a4df2-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="a4df2-120">Jeśli zrozumiesz, dlaczego występuje ostrzeżenie i jego skutków, możesz zignorować to ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="a4df2-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="a4df2-121">.NET Core SDK jest zainstalowany w wielu lokalizacjach</span><span class="sxs-lookup"><span data-stu-id="a4df2-121">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="a4df2-122">W **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="a4df2-122">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="a4df2-123">.NET Core SDK jest zainstalowany w wielu lokalizacjach.</span><span class="sxs-lookup"><span data-stu-id="a4df2-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="a4df2-124">Tylko szablony z zestawów SDK zainstalowany w "C:\\Program Files\\dotnet\\sdk\\" są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="a4df2-124">Only templates from the SDKs installed at 'C:\\Program Files\\dotnet\\sdk\\' are displayed.</span></span>

<span data-ttu-id="a4df2-125">Ten komunikat jest wyświetlany, gdy masz co najmniej jedna instalacja zestawu SDK programu .NET Core w katalogu, poza *C:\\Program Files\\dotnet\\sdk\\* .</span><span class="sxs-lookup"><span data-stu-id="a4df2-125">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="a4df2-126">Zwykle dzieje się tak w przypadku zestawu .NET Core SDK został wdrożony na maszynie za pomocą kopiowania/wklejania zamiast Instalatora MSI.</span><span class="sxs-lookup"><span data-stu-id="a4df2-126">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="a4df2-127">Odinstaluj wszystkie 32-bitowych zestawów .NET Core SDK i środowiska uruchomieniowe Aby uniknąć tego ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="a4df2-127">Uninstall all 32-bit .NET Core SDKs and runtimes to prevent this warning.</span></span> <span data-ttu-id="a4df2-128">Odinstaluj z **Panelu sterowania** > **programy i funkcje** > **Odinstaluj lub zmień program**.</span><span class="sxs-lookup"><span data-stu-id="a4df2-128">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="a4df2-129">Jeśli zrozumiesz, dlaczego występuje ostrzeżenie i jego skutków, możesz zignorować to ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="a4df2-129">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="a4df2-130">Nie wykryto żadnych zestawów .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="a4df2-130">No .NET Core SDKs were detected</span></span>

* <span data-ttu-id="a4df2-131">W programie Visual Studio **nowy projekt** okno dla platformy ASP.NET Core, może zostać wyświetlony następujące ostrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="a4df2-131">In the Visual Studio **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

  > <span data-ttu-id="a4df2-132">Nie wykryto żadnych zestawów .NET Core SDK, upewnij się, że są one uwzględnione w zmiennej środowiskowej `PATH`.</span><span class="sxs-lookup"><span data-stu-id="a4df2-132">No .NET Core SDKs were detected, ensure they are included in the environment variable `PATH`.</span></span>

* <span data-ttu-id="a4df2-133">Podczas wykonywania `dotnet` polecenia ostrzeżenie jest wyświetlane jako:</span><span class="sxs-lookup"><span data-stu-id="a4df2-133">When executing a `dotnet` command, the warning appears as:</span></span>

  > <span data-ttu-id="a4df2-134">Nie można odnaleźć żadnych dotnet zainstalowanych zestawów SDK.</span><span class="sxs-lookup"><span data-stu-id="a4df2-134">It was not possible to find any installed dotnet SDKs.</span></span>

<span data-ttu-id="a4df2-135">Ostrzeżenia te są wyświetlane, gdy zmienna środowiskowa `PATH` nie wskazuje na żadnych zestawów .NET Core SDK na maszynie.</span><span class="sxs-lookup"><span data-stu-id="a4df2-135">These warnings appear when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="a4df2-136">Aby rozwiązać ten problem:</span><span class="sxs-lookup"><span data-stu-id="a4df2-136">To resolve this problem:</span></span>

* <span data-ttu-id="a4df2-137">Instalowanie programu .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="a4df2-137">Install the .NET Core SDK.</span></span> <span data-ttu-id="a4df2-138">Uzyskaj najnowszą wersję Instalatora z [pobiera .NET](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="a4df2-138">Obtain the latest installer from [.NET Downloads](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="a4df2-139">Upewnij się, że `PATH` zmienna środowiskowa wskazuje lokalizację, w którym jest zainstalowany zestaw SDK (`C:\Program Files\dotnet\` 64-bitowy/x64 lub `C:\Program Files (x86)\dotnet\` dla 32-bitowy/x86).</span><span class="sxs-lookup"><span data-stu-id="a4df2-139">Verify that the `PATH` environment variable points to the location where the SDK is installed (`C:\Program Files\dotnet\` for 64-bit/x64 or `C:\Program Files (x86)\dotnet\` for 32-bit/x86).</span></span> <span data-ttu-id="a4df2-140">Instalator zestawu SDK zwykle ustawia `PATH`.</span><span class="sxs-lookup"><span data-stu-id="a4df2-140">The SDK installer normally sets the `PATH`.</span></span> <span data-ttu-id="a4df2-141">Zawsze instaluj tej samej bitowości zestawy SDK i środowiska uruchomieniowe na tym samym komputerze.</span><span class="sxs-lookup"><span data-stu-id="a4df2-141">Always install the same bitness SDKs and runtimes on the same machine.</span></span>

### <a name="missing-sdk-after-installing-the-net-core-hosting-bundle"></a><span data-ttu-id="a4df2-142">Brak zestawu SDK po zainstalowaniu pakietu hostingu platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="a4df2-142">Missing SDK after installing the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="a4df2-143">Instalowanie [hostingu pakietu programu .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modyfikuje `PATH` podczas instalacji środowiska uruchomieniowego platformy .NET Core, aby wskazywał platformy .NET Core w wersji 32-bitowych (x 86) (`C:\Program Files (x86)\dotnet\`).</span><span class="sxs-lookup"><span data-stu-id="a4df2-143">Installing the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) modifies the `PATH` when it installs the .NET Core runtime to point to the 32-bit (x86) version of .NET Core (`C:\Program Files (x86)\dotnet\`).</span></span> <span data-ttu-id="a4df2-144">Może to spowodować brak zestawów SDK po 32-bitowych (x 86) platformy .NET Core `dotnet` jest używane polecenie ([zestawów .NET Core SDK nie zostały wykryte](#no-net-core-sdks-were-detected)).</span><span class="sxs-lookup"><span data-stu-id="a4df2-144">This can result in missing SDKs when the 32-bit (x86) .NET Core `dotnet` command is used ([No .NET Core SDKs were detected](#no-net-core-sdks-were-detected)).</span></span> <span data-ttu-id="a4df2-145">Aby rozwiązać ten problem, Przenieś `C:\Program Files\dotnet\` do pozycji przed `C:\Program Files (x86)\dotnet\` na `PATH`.</span><span class="sxs-lookup"><span data-stu-id="a4df2-145">To resolve this problem, move `C:\Program Files\dotnet\` to a position before `C:\Program Files (x86)\dotnet\` on the `PATH`.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="a4df2-146">Uzyskiwanie danych z aplikacji</span><span class="sxs-lookup"><span data-stu-id="a4df2-146">Obtain data from an app</span></span>

<span data-ttu-id="a4df2-147">Jeśli aplikacja jest w stanie odpowiadać na żądania, możesz uzyskać następujące dane z aplikacji za pomocą oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="a4df2-147">If an app is capable of responding to requests, you can obtain the following data from the app using middleware:</span></span>

* <span data-ttu-id="a4df2-148">Żądanie &ndash; metody schematu, hosta, pathbase, ścieżki, ciąg, nagłówki zapytania</span><span class="sxs-lookup"><span data-stu-id="a4df2-148">Request &ndash; Method, scheme, host, pathbase, path, query string, headers</span></span>
* <span data-ttu-id="a4df2-149">Połączenie &ndash; zdalny adres IP, port zdalny, lokalny adres IP, port lokalny, certyfikat klienta</span><span class="sxs-lookup"><span data-stu-id="a4df2-149">Connection &ndash; Remote IP address, remote port, local IP address, local port, client certificate</span></span>
* <span data-ttu-id="a4df2-150">Tożsamość &ndash; nazwę, nazwę wyświetlaną</span><span class="sxs-lookup"><span data-stu-id="a4df2-150">Identity &ndash; Name, display name</span></span>
* <span data-ttu-id="a4df2-151">Ustawienia konfiguracji</span><span class="sxs-lookup"><span data-stu-id="a4df2-151">Configuration settings</span></span>
* <span data-ttu-id="a4df2-152">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="a4df2-152">Environment variables</span></span>

<span data-ttu-id="a4df2-153">Umieść następujące wpisy [oprogramowania pośredniczącego](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) kod na początku `Startup.Configure` potoku przetwarzania żądań metody.</span><span class="sxs-lookup"><span data-stu-id="a4df2-153">Place the following [middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) code at the beginning of the `Startup.Configure` method's request processing pipeline.</span></span> <span data-ttu-id="a4df2-154">Środowisko jest sprawdzany przed uruchomieniem oprogramowania pośredniczącego, aby upewnić się, że kod jest wykonywane tylko w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="a4df2-154">The environment is checked before the middleware is run to ensure that the code is only executed in the Development environment.</span></span>

<span data-ttu-id="a4df2-155">Aby uzyskać środowisko, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="a4df2-155">To obtain the environment, use either of the following approaches:</span></span>

* <span data-ttu-id="a4df2-156">Wstrzykiwanie `IHostingEnvironment` do `Startup.Configure` metody i Sprawdź środowisko o zmiennej lokalnej.</span><span class="sxs-lookup"><span data-stu-id="a4df2-156">Inject the `IHostingEnvironment` into the `Startup.Configure` method and check the environment with the local variable.</span></span> <span data-ttu-id="a4df2-157">Następujący przykładowy kod pokazuje tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="a4df2-157">The following sample code demonstrates this approach.</span></span>

* <span data-ttu-id="a4df2-158">Przypisz środowiska do właściwości w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="a4df2-158">Assign the environment to a property in the `Startup` class.</span></span> <span data-ttu-id="a4df2-159">Sprawdź środowisko przy użyciu właściwości (na przykład `if (Environment.IsDevelopment())`).</span><span class="sxs-lookup"><span data-stu-id="a4df2-159">Check the environment using the property (for example, `if (Environment.IsDevelopment())`).</span></span>

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
