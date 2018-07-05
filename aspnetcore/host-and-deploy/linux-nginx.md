---
title: Host platformy ASP.NET Core w systemie Linux przy użyciu serwera Nginx
author: rick-anderson
description: Dowiedz się, jak skonfigurować serwer Nginx jako zwrotny serwer proxy w systemie Ubuntu 16.04 do przesyłania ruchu HTTP do aplikacji sieci web ASP.NET Core uruchomionych na Kestrel.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 840a9f98b3409f74b9a41ee24ff7bcb33a875470
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433938"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="4f2c4-103">Host platformy ASP.NET Core w systemie Linux przy użyciu serwera Nginx</span><span class="sxs-lookup"><span data-stu-id="4f2c4-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="4f2c4-104">Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="4f2c4-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="4f2c4-105">W tym przewodniku wyjaśniono Konfigurowanie środowiska ASP.NET Core gotowe do produkcji na serwerze z systemem Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="4f2c4-106">Te instrukcje, które mogą działać z nowszymi wersjami systemu Ubuntu, ale instrukcje nie zostały przetestowane z nowszymi wersjami.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

> [!NOTE]
> <span data-ttu-id="4f2c4-107">Aby uzyskać Ubuntu 14.04 *supervisord* jest zalecane jako rozwiązanie do monitorowania procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-107">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="4f2c4-108">*systemd* nie jest dostępna w systemie Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-108">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="4f2c4-109">Ubuntu 14.04 instrukcje można znaleźć [poprzednią wersję tego tematu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="4f2c4-109">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="4f2c4-110">Ten przewodnik:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-110">This guide:</span></span>

* <span data-ttu-id="4f2c4-111">Umieszcza istniejącej aplikacji platformy ASP.NET Core za serwerem proxy odwrotnej.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-111">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="4f2c4-112">Konfiguruje zwrotnego serwera proxy do przekazywania żądań do serwera sieci web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-112">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="4f2c4-113">Gwarantuje, że aplikacja sieci web jest uruchamiana podczas uruchamiania jako demon.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-113">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="4f2c4-114">Konfiguruje narzędzia do zarządzania procesami, aby pomóc, uruchom ponownie aplikację internetową.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-114">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f2c4-115">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="4f2c4-115">Prerequisites</span></span>

1. <span data-ttu-id="4f2c4-116">Dostęp do serwera Ubuntu 16.04 przy użyciu konta użytkownika standardowego przy użyciu uprawnień "sudo".</span><span class="sxs-lookup"><span data-stu-id="4f2c4-116">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="4f2c4-117">Na serwerze, należy zainstalować środowisko uruchomieniowe platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-117">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="4f2c4-118">Odwiedź stronę [.NET Core wszystkie strony plików do pobrania](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="4f2c4-118">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="4f2c4-119">Z listy w obszarze wybierz najnowsze środowisko uruchomieniowe — wersja zapoznawcza **środowiska uruchomieniowego**.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-119">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="4f2c4-120">Wybierz, a następnie postępuj zgodnie z instrukcjami dotyczącymi Ubuntu, które są zgodne z wersją systemu Ubuntu server.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-120">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="4f2c4-121">Istniejącą aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-121">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="4f2c4-122">Publikowanie i skopiuj aplikacji</span><span class="sxs-lookup"><span data-stu-id="4f2c4-122">Publish and copy over the app</span></span>

<span data-ttu-id="4f2c4-123">Konfigurowanie aplikacji na potrzeby [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="4f2c4-123">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="4f2c4-124">Uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) ze środowiska projektowego, aby utworzyć pakiet aplikacji do katalogu (na przykład *bin/wydawania/&lt;target_framework_moniker&gt;/ publish*), można Uruchom na serwerze:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-124">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="4f2c4-125">Aplikację można także publikować jako [niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) Jeśli wolisz nie zachować środowisko uruchomieniowe platformy .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-125">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="4f2c4-126">Skopiuj aplikacji ASP.NET Core na serwer przy użyciu narzędzia, która integruje się z przepływu pracy w organizacji, (na przykład punkt połączenia usługi, SFTP).</span><span class="sxs-lookup"><span data-stu-id="4f2c4-126">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="4f2c4-127">Często do lokalizowania aplikacji sieci web w obszarze *var* katalog (na przykład *aspnetcore/var/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="4f2c4-127">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="4f2c4-128">W przypadku wdrożenia produkcyjnego przepływu pracy ciągłej integracji działa publikowania aplikacji i kopiowanie zasobów do serwera.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-128">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="4f2c4-129">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-129">Test the app:</span></span>

1. <span data-ttu-id="4f2c4-130">W wierszu polecenia Uruchom aplikację: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-130">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="4f2c4-131">W przeglądarce przejdź do `http://<serveraddress>:<port>` Aby sprawdzić, aplikacja działa lokalnie w systemie Linux.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-131">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="4f2c4-132">Konfigurowanie zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="4f2c4-132">Configure a reverse proxy server</span></span>

<span data-ttu-id="4f2c4-133">Zwrotny serwer proxy jest wspólne dla aplikacji sieci web dynamicznego obsługująca.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-133">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="4f2c4-134">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-134">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="4f2c4-135">Każda konfiguracja&mdash;z lub bez serwera proxy odwrotnej&mdash;jest prawidłowy i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core 2.0 lub nowszej aplikacje.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="4f2c4-136">Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="4f2c4-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="4f2c4-137">Użyj serwera proxy odwrotnej</span><span class="sxs-lookup"><span data-stu-id="4f2c4-137">Use a reverse proxy server</span></span>

<span data-ttu-id="4f2c4-138">Kestrel nadaje się doskonale dla obsługujących zawartość dynamiczną z platformą ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-138">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="4f2c4-139">Jednak możliwości usług sieci web nie są jako wyposażonym jako serwerów, takich jak usługi IIS, Apache i Nginx.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-139">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="4f2c4-140">Serwer proxy odwrotnej można odciążyć pracy, takich jak obsługująca zawartość statyczną, buforowanie żądań kompresowania żądań i kończenie żądań SSL z serwerem HTTP.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-140">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="4f2c4-141">Zwrotnego serwera proxy mogą znajdować się na dedykowanym komputerze lub można wdrażać wraz z serwerem HTTP.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-141">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="4f2c4-142">Na potrzeby tego przewodnika pojedyncze wystąpienie serwera Nginx jest używany.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-142">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="4f2c4-143">Działa na tym samym serwerze, wraz z serwerem HTTP.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-143">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="4f2c4-144">Na podstawie wymagań, można wybrać różne ustawienia.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-144">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="4f2c4-145">Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy użyć [przekazywane oprogramowania pośredniczącego nagłówki](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-145">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="4f2c4-146">Aktualizacje oprogramowania pośredniczącego `Request.Scheme`przy użyciu `X-Forwarded-Proto` nagłówka, więc działanie tego identyfikatory URI przekierowań i innych zasad zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-146">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="4f2c4-147">Dowolny składnik, który jest zależny od systemu, takie jak uwierzytelnianie, generowanie konsolidacji, przekierowań i geolokalizacja, muszą być umieszczone po wywołaniu oprogramowanie pośredniczące przekazane nagłówków.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-147">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="4f2c4-148">Zgodnie z ogólną zasadą przekazywane oprogramowania pośredniczącego nagłówki należy uruchomić przed innym oprogramowaniu pośredniczącym, z wyjątkiem diagnostyki i obsługi oprogramowania pośredniczącego błędów.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-148">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="4f2c4-149">Ta kolejność gwarantuje, że oprogramowanie pośredniczące, opierając się na nagłówki przekazywane informacje mogą wykorzystywać wartości nagłówka do przetworzenia.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-149">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4f2c4-150">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f2c4-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4f2c4-151">Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) lub podobne oprogramowanie pośredniczące schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="4f2c4-152">Konfigurowanie oprogramowania pośredniczącego, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4f2c4-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4f2c4-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4f2c4-154">Wywoływanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in Class metoda `Startup.Configure` przed wywołaniem [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) i [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) lub podobne schematu uwierzytelniania oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-154">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="4f2c4-155">Konfigurowanie oprogramowania pośredniczącego, aby przekazywać `X-Forwarded-For` i `X-Forwarded-Proto` nagłówków:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-155">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="4f2c4-156">Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone oprogramowanie pośredniczące, są domyślne nagłówki do przekazywania `None`.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-156">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="4f2c4-157">Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-157">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="4f2c4-158">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="4f2c4-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="4f2c4-159">Zainstalować rozwiązanie Nginx</span><span class="sxs-lookup"><span data-stu-id="4f2c4-159">Install Nginx</span></span>

<span data-ttu-id="4f2c4-160">Użyj `apt-get` do zainstalowania serwera Nginx.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-160">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="4f2c4-161">Instalator tworzy *systemd* skryptu init, który uruchamia serwer Nginx jako demon przy uruchamianiu systemu.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-161">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="4f2c4-162">Ubuntu osobistych pakiet archiwum (osobistych archiwów pakietów innych) obsługiwany przez wolontariuszy, nie zostało przekazane przez [nginx.org](https://nginx.org/). Aby uzyskać więcej informacji, zobacz [Nginx: binarny wydań: pakiety oficjalne Debian/Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="4f2c4-162">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="4f2c4-163">Jeśli wymagane są opcjonalne modułów serwera Nginx, może być wymagane tworzenia Nginx ze źródła.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-163">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="4f2c4-164">Po zainstalowaniu serwera Nginx po raz pierwszy, jawnie uruchom ją, uruchamiając:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-164">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="4f2c4-165">Sprawdź, czy przeglądarka wyświetla wartość domyślna strona docelowa dla kontenera Nginx.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-165">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="4f2c4-166">Strona docelowa jest dostępny na `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-166">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="4f2c4-167">Konfigurowanie serwera Nginx</span><span class="sxs-lookup"><span data-stu-id="4f2c4-167">Configure Nginx</span></span>

<span data-ttu-id="4f2c4-168">Aby skonfigurować serwer Nginx jako zwrotny serwer proxy, aby przekazywać żądania do aplikacji platformy ASP.NET Core, zmodyfikuj */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-168">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="4f2c4-169">Otwórz go w edytorze tekstów i Zastąp zawartość następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-169">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

<span data-ttu-id="4f2c4-170">Gdy nie `server_name` dopasowania, Nginx korzysta z domyślnego serwera.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-170">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="4f2c4-171">Jeśli żaden serwer domyślny jest zdefiniowany, pierwszy serwer w pliku konfiguracji jest domyślny serwer.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-171">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="4f2c4-172">Najlepszym rozwiązaniem jest dodanie określonych domyślnego serwera, która zwraca kod stanu 444 w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-172">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="4f2c4-173">Przedstawiono przykładową konfigurację serwera domyślnego:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-173">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="4f2c4-174">Za pomocą poprzedniego pliku i domyślnego serwera konfiguracji Nginx akceptuje publicznych ruch na porcie 80 z nagłówkiem hosta `example.com` lub `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-174">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="4f2c4-175">Żądania, które nie pasują te hosty nie uzyskać przekazywane do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-175">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="4f2c4-176">Serwer Nginx przekazuje żądania pasujących do Kestrel w `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-176">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="4f2c4-177">Zobacz [przetwarzaniu żądania przez serwer nginx](https://nginx.org/docs/http/request_processing.html) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-177">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span> <span data-ttu-id="4f2c4-178">Aby zmienić port adresu IP firmy Kestrel, zobacz [Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="4f2c4-178">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="4f2c4-179">Nie można określić poprawną [dyrektywy nazwa_serwera](https://nginx.org/docs/http/server_names.html) ujawnia luki w zabezpieczeniach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-179">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="4f2c4-180">Powiązanie symbol wieloznaczny domeny podrzędnej (na przykład `*.example.com`) nie stanowić to zagrożenie bezpieczeństwa, jeśli możesz kontrolować domenę nadrzędną całego (w przeciwieństwie do `*.com`, który jest narażony).</span><span class="sxs-lookup"><span data-stu-id="4f2c4-180">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="4f2c4-181">Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-181">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="4f2c4-182">Po ustanowieniu konfigurację serwera Nginx, uruchom `sudo nginx -t` Aby sprawdzić składnię plików konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-182">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="4f2c4-183">Jeśli test konfiguracji w pliku zakończy się pomyślnie, wymusić Nginx, aby wczytać zmiany, uruchamiając `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-183">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="4f2c4-184">Aby bezpośrednio uruchamiać aplikację na serwerze:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-184">To directly run the app on the server:</span></span>

1. <span data-ttu-id="4f2c4-185">Przejdź do katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-185">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="4f2c4-186">Uruchom plik wykonywalny aplikacji: `./<app_executable>`.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-186">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="4f2c4-187">Jeśli wystąpi błąd uprawnień, Zmień uprawnienia:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-187">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="4f2c4-188">Jeśli aplikacja działa na serwerze, ale nie odpowiada za pośrednictwem Internetu, sprawdź ustawienia zapory serwera i upewnij się, że port 80 jest otwarty.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-188">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="4f2c4-189">Jeśli używasz systemu Ubuntu Maszynie wirtualnej platformy Azure, Dodaj regułę sieciowej grupy zabezpieczeń (NSG), która włącza port przychodzący ruch 80.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-189">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="4f2c4-190">Nie ma potrzeby można włączyć reguły ruchu wychodzącego portu 80, jak ruch wychodzący jest udzielany automatycznie po włączeniu reguły dla ruchu przychodzącego.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-190">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="4f2c4-191">Po zakończeniu testowania aplikacji, zamknij aplikację za pomocą `Ctrl+C` w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-191">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="4f2c4-192">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4f2c4-192">Monitoring the app</span></span>

<span data-ttu-id="4f2c4-193">Serwer jest skonfigurowany do przekazywania żądań kierowanych do `http://<serveraddress>:80` się do aplikacji platformy ASP.NET Core uruchomionych na Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-193">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="4f2c4-194">Jednak serwer Nginx nie jest skonfigurować do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-194">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="4f2c4-195">*systemd* może służyć do tworzenia pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-195">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="4f2c4-196">*systemd* to system init, który zapewnia wiele funkcji zaawansowanych uruchamianie, zatrzymywanie oraz zarządzanie procesami.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-196">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="4f2c4-197">Utwórz plik usługi</span><span class="sxs-lookup"><span data-stu-id="4f2c4-197">Create the service file</span></span>

<span data-ttu-id="4f2c4-198">Tworzenie pliku definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-198">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="4f2c4-199">Poniżej przedstawiono przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-199">The following is an example service file for the app:</span></span>

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="4f2c4-200">Jeśli użytkownik *danych www* nie jest używany przez tę konfigurację użytkownika zdefiniowane w tym miejscu należy najpierw utworzyć i podane odpowiednie prawa własności plików.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-200">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="4f2c4-201">System plików rozróżniana wielkość liter jest systemu Linux.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-201">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="4f2c4-202">Ustawienie ASPNETCORE_ENVIRONMENT "Produkcyjne" wyniki wyszukiwania dla pliku konfiguracji w *appsettings. Production.JSON*, a nie *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-202">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="4f2c4-203">Niektóre wartości (na przykład parametry połączenia SQL), należy użyć znaków ucieczki dla dostawców konfiguracji można odczytać zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-203">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="4f2c4-204">Użyj następującego polecenia do generowania prawidłowo o zmienionym znaczeniu wartości do użycia w pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-204">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="4f2c4-205">Zapisz plik i włączyć usługę.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-205">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="4f2c4-206">Uruchom usługę i sprawdź, czy jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-206">Start the service and verify that it's running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="4f2c4-207">Zwrotny serwer proxy, skonfigurowane i Kestrel zarządzane za pośrednictwem systemd, aplikacji sieci web jest w pełni skonfigurowane i dostępne za pomocą przeglądarki na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-207">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="4f2c4-208">Jest także dostępny z komputera zdalnego, wyłączając wszelkie zapory, która może blokować.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-208">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="4f2c4-209">Inspekcja nagłówki odpowiedzi `Server` nagłówka przedstawiono aplikację ASP.NET Core, obsługiwanej przez Kestrel.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-209">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="4f2c4-210">Wyświetlanie dzienników</span><span class="sxs-lookup"><span data-stu-id="4f2c4-210">Viewing logs</span></span>

<span data-ttu-id="4f2c4-211">Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu `systemd`, scentralizowane dziennika są rejestrowane wszystkie zdarzenia i procesów.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-211">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="4f2c4-212">Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów, które zarządza `systemd`.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-212">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="4f2c4-213">Aby wyświetlić `kestrel-hellomvc.service`— określone elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-213">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="4f2c4-214">Do dalszego filtrowania, opcje czasu takie jak `--since today`, `--until 1 hour ago` lub kombinację tych można zmniejszyć liczbę zwróconych pozycji.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-214">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="4f2c4-215">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4f2c4-215">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="4f2c4-216">Włącz AppArmor</span><span class="sxs-lookup"><span data-stu-id="4f2c4-216">Enable AppArmor</span></span>

<span data-ttu-id="4f2c4-217">Linux zabezpieczeń modułów (LSM) tak, to struktura, która jest częścią jądra systemu Linux od systemu Linux w wersji 2.6.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-217">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="4f2c4-218">LSM obsługuje różne implementacje modułach zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-218">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="4f2c4-219">[AppArmor](https://wiki.ubuntu.com/AppArmor) jest LSM, implementujący systemu obowiązkowe kontroli dostępu, który umożliwia ograniczenie program ma ograniczony zestaw zasobów.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-219">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="4f2c4-220">Upewnij się, AppArmor jest włączone i skonfigurowane prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-220">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="4f2c4-221">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="4f2c4-221">Configuring the firewall</span></span>

<span data-ttu-id="4f2c4-222">Zamknij wyłączanie wszystkich portów zewnętrznych, które nie są używane.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-222">Close off all external ports that are not in use.</span></span> <span data-ttu-id="4f2c4-223">Zapora prostotę (ufw) zawiera frontonu na potrzeby `iptables` , zapewniając interfejs wiersza polecenia dla konfiguracji zapory.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-223">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="4f2c4-224">Upewnij się, że `ufw` zezwalają na ruch na portach, wszelkie potrzebne.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-224">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="4f2c4-225">Zabezpieczanie serwera Nginx</span><span class="sxs-lookup"><span data-stu-id="4f2c4-225">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="4f2c4-226">Zmień nazwę odpowiedzi serwera Nginx</span><span class="sxs-lookup"><span data-stu-id="4f2c4-226">Change the Nginx response name</span></span>

<span data-ttu-id="4f2c4-227">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-227">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="4f2c4-228">Skonfiguruj opcje</span><span class="sxs-lookup"><span data-stu-id="4f2c4-228">Configure options</span></span>

<span data-ttu-id="4f2c4-229">Konfigurowanie serwera przy użyciu dodatkowych wymaganych modułów.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-229">Configure the server with additional required modules.</span></span> <span data-ttu-id="4f2c4-230">Należy wziąć pod uwagę przy użyciu zapory aplikacji sieci web, takich jak [zapory ModSecurity](https://www.modsecurity.org/), w celu ograniczenia funkcjonalności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-230">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="4f2c4-231">Konfigurowanie certyfikatu SSL</span><span class="sxs-lookup"><span data-stu-id="4f2c4-231">Configure SSL</span></span>

* <span data-ttu-id="4f2c4-232">Konfigurowanie serwera do nasłuchiwania ruchu HTTPS na porcie `443` , określając prawidłowy certyfikat wystawiony przez zaufany urząd certyfikacji (CA).</span><span class="sxs-lookup"><span data-stu-id="4f2c4-232">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="4f2c4-233">Wzmocnić zabezpieczenia przez wprowadzenie niektórych rozwiązań przedstawiony w następującym */etc/nginx/nginx.conf* pliku.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-233">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="4f2c4-234">Przykłady obejmują wybranie silniejszego szyfrowania i przekierowania całego ruchu za pośrednictwem protokołu HTTP do HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-234">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="4f2c4-235">Dodawanie `HTTP Strict-Transport-Security` nagłówka (HSTS) zapewnia wszystkie kolejne żądania wysłane przez klienta są tylko za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-235">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="4f2c4-236">Nie dodawaj nagłówka zabezpieczeń w przypadku transportu Strict, lub wybrać odpowiednią `max-age` Jeśli SSL zostanie wyłączona w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-236">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="4f2c4-237">Dodaj */etc/nginx/proxy.conf* pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-237">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="4f2c4-238">Edytuj */etc/nginx/nginx.conf* pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-238">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="4f2c4-239">Przykład zawiera zarówno `http` i `server` sekcji w pliku w jednej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-239">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="4f2c4-240">Zabezpieczenia serwera Nginx z porywaniu kliknięć</span><span class="sxs-lookup"><span data-stu-id="4f2c4-240">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="4f2c4-241">Porywaniu kliknięć jest techniki złośliwego Zbieraj zainfekowane użytkownik klika polecenie.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-241">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="4f2c4-242">Porywaniu kliknięć wskazówki ofiarą (gości) do kliknięcia w witrynie zainfekowane.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-242">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="4f2c4-243">Użyj X-FRAME-OPTIONS na zabezpieczenie witryny.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-243">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="4f2c4-244">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-244">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="4f2c4-245">Dodaj wiersz `add_header X-Frame-Options "SAMEORIGIN";` i Zapisz plik, a następnie uruchom ponownie serwer Nginx.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-245">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="4f2c4-246">Wykrywanie typu MIME</span><span class="sxs-lookup"><span data-stu-id="4f2c4-246">MIME-type sniffing</span></span>

<span data-ttu-id="4f2c4-247">Tego pliku nagłówkowego zapobiega w większości przeglądarek z wykrywanie MIME odpowiedzi od deklarowanej typu zawartości jako nagłówek powoduje, że przeglądarka nie, aby zastąpić typ zawartości odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-247">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="4f2c4-248">Za pomocą `nosniff` opcji, jeśli serwer jest w stanie "text/html" jest zawartość, Przeglądarka renderuje je jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="4f2c4-248">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="4f2c4-249">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="4f2c4-249">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="4f2c4-250">Dodaj wiersz `add_header X-Content-Type-Options "nosniff";` i Zapisz plik, a następnie uruchom ponownie serwer Nginx.</span><span class="sxs-lookup"><span data-stu-id="4f2c4-250">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4f2c4-251">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4f2c4-251">Additional resources</span></span>

* [<span data-ttu-id="4f2c4-252">Nginx: Wersje binarne: pakiety oficjalne Debian/Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4f2c4-252">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [<span data-ttu-id="4f2c4-253">Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="4f2c4-253">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="4f2c4-254">Serwer NGINX: Przy użyciu nagłówka przekazane</span><span class="sxs-lookup"><span data-stu-id="4f2c4-254">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
