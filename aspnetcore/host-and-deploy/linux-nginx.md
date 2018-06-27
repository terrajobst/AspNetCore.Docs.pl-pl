---
title: Host platformy ASP.NET Core w systemie Linux z Nginx
author: rick-anderson
description: Dowiedz się, jak można skonfigurować jako zwrotny serwer proxy na 16.04 Ubuntu, aby przesyłał dalej ruch HTTP dla aplikacji sieci web platformy ASP.NET Core systemem Kestrel Nginx.
ms.author: riande
ms.custom: mvc
ms.date: 05/22/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 0ccc9e396ffc9f7af93d5601fee0182d9e3471f4
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961493"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="d4d3b-103">Host platformy ASP.NET Core w systemie Linux z Nginx</span><span class="sxs-lookup"><span data-stu-id="d4d3b-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="d4d3b-104">Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="d4d3b-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="d4d3b-105">W tym przewodniku opisano konfigurowanie środowiska ASP.NET Core gotowe do produkcji na serwerze Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="d4d3b-106">Te instrukcje, które mogą działać z nowszymi wersjami systemu Ubuntu, ale nie zostały przetestowane zgodnie z instrukcjami z nowszymi wersjami.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-106">These instructions likely work with newer versions of Ubuntu, but the instructions haven't been tested with newer versions.</span></span>

> [!NOTE]
> <span data-ttu-id="d4d3b-107">Dla Ubuntu 14.04 *supervisord* zaleca się rozwiązanie do monitorowania procesu Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-107">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="d4d3b-108">*systemd* nie jest dostępna w Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-108">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="d4d3b-109">Aby uzyskać instrukcje Ubuntu 14.04, zobacz [poprzednią wersję tego tematu](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="d4d3b-109">For Ubuntu 14.04 instructions, see the [previous version of this topic](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="d4d3b-110">Ten przewodnik:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-110">This guide:</span></span>

* <span data-ttu-id="d4d3b-111">Umieszcza istniejącej aplikacji platformy ASP.NET Core za zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-111">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="d4d3b-112">Konfiguruje zwrotnego serwera proxy do przekazywania żądań do serwera sieci web Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-112">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="d4d3b-113">Gwarantuje, że aplikacja sieci web uruchamiany podczas uruchamiania jako demon.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-113">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="d4d3b-114">Konfiguruje narzędzie do zarządzania procesu, aby ponownie uruchomić aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-114">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4d3b-115">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="d4d3b-115">Prerequisites</span></span>

1. <span data-ttu-id="d4d3b-116">Dostęp do serwera Ubuntu 16.04 przy użyciu konta użytkowników standardowych z uprawnieniem sudo.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-116">Access to an Ubuntu 16.04 server with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="d4d3b-117">Zainstaluj środowisko uruchomieniowe .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-117">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="d4d3b-118">Odwiedź stronę [.NET Core wszystkie pliki do pobrania strony](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="d4d3b-118">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="d4d3b-119">Wybierz z listy w obszarze najnowsze środowisko uruchomieniowe-preview **środowiska uruchomieniowego**.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-119">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="d4d3b-120">Wybierz, a następnie postępuj zgodnie z instrukcjami Ubuntu odpowiadających Ubuntu wersji serwera.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-120">Select and follow the instructions for Ubuntu that match the Ubuntu version of the server.</span></span>
1. <span data-ttu-id="d4d3b-121">Istniejącej aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-121">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="d4d3b-122">Publikowanie i skopiuj przez aplikację</span><span class="sxs-lookup"><span data-stu-id="d4d3b-122">Publish and copy over the app</span></span>

<span data-ttu-id="d4d3b-123">Konfigurowanie aplikacji na potrzeby [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="d4d3b-123">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="d4d3b-124">Uruchom [publikowania dotnet](/dotnet/core/tools/dotnet-publish) z Środowisko deweloperskie do tworzenia pakietów aplikacji w katalogu (na przykład *bin i wersji/&lt;target_framework_moniker&gt;/ publish*) który można Uruchom na serwerze:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-124">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="d4d3b-125">Aplikacja również mogą być publikowane jako [niezależne wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) nie, aby obsługa środowiska uruchomieniowego .NET Core na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-125">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="d4d3b-126">Skopiuj aplikacji platformy ASP.NET Core do serwera przy użyciu narzędzia, która integruje się z przepływu organizacji (na przykład punkt połączenia usługi, SFTP).</span><span class="sxs-lookup"><span data-stu-id="d4d3b-126">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="d4d3b-127">Często można znaleźć aplikacji sieci web w obszarze *var* katalog (na przykład *var/aspnetcore/hellomvc*).</span><span class="sxs-lookup"><span data-stu-id="d4d3b-127">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="d4d3b-128">W przypadku wdrożenia produkcyjnego ciągłej integracji przepływu pracy wykonuje pracę publikowania aplikacji i kopiowanie zasoby na serwerze.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-128">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

<span data-ttu-id="d4d3b-129">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-129">Test the app:</span></span>

1. <span data-ttu-id="d4d3b-130">W wierszu polecenia Uruchom aplikację: `dotnet <app_assembly>.dll`.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-130">From the command line, run the app: `dotnet <app_assembly>.dll`.</span></span>
1. <span data-ttu-id="d4d3b-131">W przeglądarce przejdź do `http://<serveraddress>:<port>` można zweryfikować aplikacja działa w systemie Linux lokalnie.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-131">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux locally.</span></span>

## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="d4d3b-132">Skonfiguruj serwer zwrotnego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="d4d3b-132">Configure a reverse proxy server</span></span>

<span data-ttu-id="d4d3b-133">Zwrotny serwer proxy jest typowe dla obsługi aplikacji sieci web dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-133">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="d4d3b-134">Zwrotny serwer proxy kończy żądanie HTTP i przekazuje je do aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-134">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="d4d3b-135">Albo konfiguracji&mdash;z lub bez zwrotnego serwera proxy&mdash;jest prawidłowa i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core w wersji 2.0 lub nowszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="d4d3b-136">Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="d4d3b-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a><span data-ttu-id="d4d3b-137">Użyj serwera proxy wstecznego</span><span class="sxs-lookup"><span data-stu-id="d4d3b-137">Use a reverse proxy server</span></span>

<span data-ttu-id="d4d3b-138">Kestrel stanowi doskonałe rozwiązanie do obsługi zawartości dynamicznej z platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-138">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="d4d3b-139">Funkcji obsługi sieci web nie są jednak jako funkcja sformatowany jako serwery usług IIS, Apache lub Nginx.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-139">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="d4d3b-140">Zwrotnego serwera proxy można odciążyć pracy, takie jak obsługę zawartości statycznej, buforowanie żądań kompresowania żądań i kończenia żądań SSL z serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-140">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="d4d3b-141">Zwrotnego serwera proxy może znajdować się na dedykowanym komputerze lub mogą można wdrożyć obok serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-141">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="d4d3b-142">Na potrzeby tego przewodnika jest używany przez pojedyncze wystąpienie Nginx.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-142">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="d4d3b-143">Uruchamia go na tym samym serwerze, z serwera HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-143">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="d4d3b-144">Na podstawie wymagań, można wybrać różne ustawienia.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-144">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="d4d3b-145">Ponieważ żądania są przekazywane przez zwrotny serwer proxy, należy użyć [przekazywane oprogramowanie pośredniczące nagłówki](xref:host-and-deploy/proxy-load-balancer) z [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-145">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="d4d3b-146">Aktualizacje oprogramowania pośredniczącego `Request.Scheme`za pomocą `X-Forwarded-Proto` nagłówka, więc poprawne działanie tego przekierowania URI i innymi zasadami zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-146">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="d4d3b-147">Każdego składnika, który jest zależny od systemu, takich jak uwierzytelnianie, generowanie konsolidacji przekierowania i używanie funkcji geolokalizacji, muszą znajdować się po wywołaniu przez oprogramowanie pośredniczące przekazane nagłówków.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-147">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="d4d3b-148">Ogólną zasadą przekazywane oprogramowanie pośredniczące nagłówków należy uruchomić przed innym oprogramowaniu pośredniczącym, z wyjątkiem diagnostyki i obsługi oprogramowania pośredniczącego błędów.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-148">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="d4d3b-149">Ta kolejność zapewnia, że oprogramowanie pośredniczące polegania na informacje przekazywane nagłówków może używać wartości nagłówka do przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-149">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d4d3b-150">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d4d3b-150">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d4d3b-151">Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody w `Startup.Configure` przed wywołaniem [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) lub podobne oprogramowanie pośredniczące schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="d4d3b-152">Konfigurowanie oprogramowania pośredniczącego do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-152">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d4d3b-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d4d3b-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d4d3b-154">Wywołanie [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) metody w `Startup.Configure` przed wywołaniem [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) i [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) lub podobne schemat uwierzytelniania oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-154">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="d4d3b-155">Konfigurowanie oprogramowania pośredniczącego do przekazywania `X-Forwarded-For` i `X-Forwarded-Proto` nagłówki:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-155">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="d4d3b-156">Jeśli nie [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) są określone przez oprogramowanie pośredniczące są domyślne nagłówki do przekazywania `None`.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-156">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="d4d3b-157">Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych serwerów proxy i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-157">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="d4d3b-158">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="d4d3b-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="d4d3b-159">Zainstaluj Nginx</span><span class="sxs-lookup"><span data-stu-id="d4d3b-159">Install Nginx</span></span>

<span data-ttu-id="d4d3b-160">Użyj `apt-get` do zainstalowania Nginx.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-160">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="d4d3b-161">Instalator tworzy *systemd* init skrypt uruchamiany Nginx jako demon na uruchamianie systemu.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-161">The installer creates a *systemd* init script that runs Nginx as daemon on system startup.</span></span> 

```bash
sudo -s
nginx=stable # use nginx=development for latest development version
add-apt-repository ppa:nginx/$nginx
apt-get update
apt-get install nginx
```

<span data-ttu-id="d4d3b-162">Ubuntu osobistych pakietu archiwum (PPA) obsługiwany przez ochotników, nie zostało przekazane przez [nginx.org](https://nginx.org/). Aby uzyskać więcej informacji, zobacz [Nginx: binarny wersjach: pakiety oficjalnego Debian/Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span><span class="sxs-lookup"><span data-stu-id="d4d3b-162">The Ubuntu Personal Package Archive (PPA) is maintained by volunteers and isn't distributed by [nginx.org](https://nginx.org/). For more information, see [Nginx: Binary Releases: Official Debian/Ubuntu packages](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).</span></span>

> [!NOTE]
> <span data-ttu-id="d4d3b-163">Jeśli wymagane są opcjonalne moduły Nginx, może być wymagane tworzenie Nginx ze źródła.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-163">If optional Nginx modules are required, building Nginx from source might be required.</span></span>

<span data-ttu-id="d4d3b-164">Po zainstalowaniu Nginx po raz pierwszy, jawnie uruchomić:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-164">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="d4d3b-165">Sprawdź, czy przeglądarka wyświetla domyślna strona początkowa dla Nginx.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-165">Verify a browser displays the default landing page for Nginx.</span></span> <span data-ttu-id="d4d3b-166">Strona docelowa jest dostępny w `http://<server_IP_address>/index.nginx-debian.html`.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-166">The landing page is reachable at `http://<server_IP_address>/index.nginx-debian.html`.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="d4d3b-167">Skonfiguruj Nginx</span><span class="sxs-lookup"><span data-stu-id="d4d3b-167">Configure Nginx</span></span>

<span data-ttu-id="d4d3b-168">Aby skonfigurować Nginx jako zwrotny serwer proxy do przesyłania żądań do aplikacji platformy ASP.NET Core, zmodyfikuj */etc/nginx/sites-available/default*.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-168">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="d4d3b-169">Otwórz go w edytorze tekstów i Zastąp zawartość z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-169">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="d4d3b-170">Gdy nie `server_name` dopasowania, Nginx korzysta z domyślnego serwera.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-170">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="d4d3b-171">Jeśli żaden serwer domyślny jest zdefiniowany, pierwszy serwer w pliku konfiguracji jest serwerem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-171">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="d4d3b-172">Najlepszym rozwiązaniem jest dodanie określonego domyślnego serwera, która zwraca kod stanu 444 w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-172">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="d4d3b-173">To jest przykład domyślny serwer konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-173">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="d4d3b-174">Z poprzedniego pliku i domyślnego serwera konfiguracji, Nginx akceptuje publicznego ruch na porcie 80 z nagłówkiem hosta `example.com` lub `*.example.com`.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-174">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="d4d3b-175">Żądania niezgodni te hosty nie pobrać przekazywane do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-175">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="d4d3b-176">Nginx przekazuje żądania pasujące do Kestrel na `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-176">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="d4d3b-177">Zobacz [jak nginx przetwarza żądanie](https://nginx.org/docs/http/request_processing.html) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-177">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="d4d3b-178">Błąd w celu określenia odpowiedniego [dyrektywy nazwa_serwera](https://nginx.org/docs/http/server_names.html) przedstawia aplikacji luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-178">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="d4d3b-179">Powiązanie symbolu wieloznacznego domeny podrzędnej (na przykład `*.example.com`) nie stanowić to zagrożenie bezpieczeństwa, jeśli kontrolować domeny nadrzędnej cały (w przeciwieństwie do `*.com`, której występuje).</span><span class="sxs-lookup"><span data-stu-id="d4d3b-179">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="d4d3b-180">Zobacz [rfc7230 sekcji-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-180">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="d4d3b-181">Po ustanowieniu konfiguracji Nginx, uruchom `sudo nginx -t` Aby sprawdzić składnię plików konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-181">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="d4d3b-182">Jeśli test konfiguracji w pliku zakończy się pomyślnie, wymusić Nginx do zastosowania zmian, uruchamiając `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-182">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

<span data-ttu-id="d4d3b-183">Aby bezpośrednio uruchamiać aplikacji na serwerze:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-183">To directly run the app on the server:</span></span>

1. <span data-ttu-id="d4d3b-184">Przejdź do katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-184">Navigate to the app's directory.</span></span>
1. <span data-ttu-id="d4d3b-185">Uruchom plik wykonywalny aplikacji: `./<app_executable>`.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-185">Run the app's executable: `./<app_executable>`.</span></span>

<span data-ttu-id="d4d3b-186">Jeśli wystąpi błąd uprawnień, Zmień uprawnienia:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-186">If a permissions error occurs, change the permissions:</span></span>

```console
chmod u+x <app_executable>
```

<span data-ttu-id="d4d3b-187">Jeśli aplikacja działa na serwerze, ale nie odpowiada za pośrednictwem Internetu, sprawdź ustawienia zapory serwera i upewnij się, że port 80 jest otwarty.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-187">If the app runs on the server but fails to respond over the Internet, check the server's firewall and confirm that port 80 is open.</span></span> <span data-ttu-id="d4d3b-188">Jeśli przy użyciu maszyny Wirtualnej systemu Ubuntu Azure, Dodaj regułę grupy zabezpieczeń sieci (NSG), która umożliwia przychodzący port 80 ruchu.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-188">If using an Azure Ubuntu VM, add a Network Security Group (NSG) rule that enables inbound port 80 traffic.</span></span> <span data-ttu-id="d4d3b-189">Jak włączenie reguły ruchu przychodzącego ruchu wychodzącego jest automatycznie przyznawane jest niepotrzebna można włączyć reguły wychodzące port 80.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-189">There's no need to enable an outbound port 80 rule, as the outbound traffic is automatically granted when the inbound rule is enabled.</span></span>

<span data-ttu-id="d4d3b-190">Po zakończeniu testowania aplikacji, zamknij aplikację z `Ctrl+C` w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-190">When done testing the app, shut the app down with `Ctrl+C` at the command prompt.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="d4d3b-191">Monitorowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d4d3b-191">Monitoring the app</span></span>

<span data-ttu-id="d4d3b-192">Serwer jest skonfigurowany do przekazywania żądań wysyłanych do `http://<serveraddress>:80` się do aplikacji platformy ASP.NET Core systemem Kestrel na `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-192">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="d4d3b-193">Nginx Konfigurowanie nie jest jednak do zarządzania procesem Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-193">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="d4d3b-194">*systemd* może służyć do tworzenia pliku usługi, aby uruchomić i monitorować podstawowej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-194">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="d4d3b-195">*systemd* to system init zapewnia wiele zaawansowanych funkcji uruchamianie, zatrzymywanie oraz procesy zarządzania.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-195">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="d4d3b-196">Tworzenie pliku usługi</span><span class="sxs-lookup"><span data-stu-id="d4d3b-196">Create the service file</span></span>

<span data-ttu-id="d4d3b-197">Tworzenie pliku definicji usługi:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-197">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="d4d3b-198">Poniżej przedstawiono przykładowy plik usługi dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-198">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="d4d3b-199">Jeśli użytkownik *danych www* nie jest używany przez tę konfigurację użytkownika zdefiniowane w tym miejscu musi być najpierw utworzyć i podane odpowiednie własność plików.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-199">If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="d4d3b-200">Linux ma system plików z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-200">Linux has a case-sensitive file system.</span></span> <span data-ttu-id="d4d3b-201">Ustawienie "Production" spowoduje wyszukiwanie w pliku konfiguracyjnym ASPNETCORE_ENVIRONMENT *appsettings. Production.JSON*, a nie *appsettings.production.json*.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-201">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="d4d3b-202">Niektóre wartości (na przykład parametry połączenia SQL), należy użyć znaków ucieczki dla dostawców konfiguracji można odczytać zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-202">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="d4d3b-203">Użyj następującego polecenia, aby wygenerować prawidłowo zmienionym wartość do użycia w pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-203">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="d4d3b-204">Zapisz plik i włączyć usługę.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-204">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="d4d3b-205">Uruchom usługę i sprawdź, czy jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-205">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="d4d3b-206">Zwrotny serwer proxy skonfigurowane i zarządzanych za pomocą systemd Kestrel, aplikacji sieci web jest w pełni skonfigurowane i dostępne w przeglądarce na komputerze lokalnym w `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-206">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="d4d3b-207">Jest również dostępny z komputera zdalnego, blokowanie wszelkich zaporą, która może być blokowane.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-207">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="d4d3b-208">Zapoznanie się nagłówki odpowiedzi `Server` nagłówek zawiera obsługiwanej przez Kestrel aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-208">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="d4d3b-209">Przeglądanie dzienników</span><span class="sxs-lookup"><span data-stu-id="d4d3b-209">Viewing logs</span></span>

<span data-ttu-id="d4d3b-210">Ponieważ aplikacja sieci web przy użyciu Kestrel odbywa się przy użyciu `systemd`, scentralizowane dziennika są rejestrowane wszystkie zdarzenia i procesów.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-210">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="d4d3b-211">Jednak ten dziennik zawiera wszystkie wpisy dla wszystkich usług i procesów zarządza `systemd`.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-211">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="d4d3b-212">Aby wyświetlić `kestrel-hellomvc.service`— określone elementy, użyj następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-212">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="d4d3b-213">Dla dalszego filtrowania, opcje czasu takich jak `--since today`, `--until 1 hour ago` lub kombinacji tych można zmniejszyć liczbę wpisów zwracane.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-213">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="d4d3b-214">Zabezpieczanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d4d3b-214">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="d4d3b-215">Włącz AppArmor</span><span class="sxs-lookup"><span data-stu-id="d4d3b-215">Enable AppArmor</span></span>

<span data-ttu-id="d4d3b-216">Linux zabezpieczeń modułów (LSM) tak, to platforma, która jest częścią jądra systemu Linux od 2.6 systemu Linux.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-216">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="d4d3b-217">LSM obsługuje różne implementacje modułów zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-217">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="d4d3b-218">[AppArmor](https://wiki.ubuntu.com/AppArmor) jest LSM, implementujący systemu obowiązkowe kontroli dostępu, który umożliwia ograniczenie program do określonych zasobów.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-218">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="d4d3b-219">Upewnij się, AppArmor jest włączony i poprawnie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-219">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="d4d3b-220">Konfigurowanie zapory</span><span class="sxs-lookup"><span data-stu-id="d4d3b-220">Configuring the firewall</span></span>

<span data-ttu-id="d4d3b-221">Zamknij Wyłącz wszystkie porty zewnętrznych, które nie są używane.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-221">Close off all external ports that are not in use.</span></span> <span data-ttu-id="d4d3b-222">Zapora prostotę (ufw) zapewnia frontonu dla `iptables` korzystanie z interfejsu wiersza polecenia dla konfiguracji zapory.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-222">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="d4d3b-223">Sprawdź, czy `ufw` zezwalają na ruch na dowolne porty.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-223">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="d4d3b-224">Zabezpieczanie Nginx</span><span class="sxs-lookup"><span data-stu-id="d4d3b-224">Securing Nginx</span></span>

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="d4d3b-225">Zmień nazwę Nginx odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="d4d3b-225">Change the Nginx response name</span></span>

<span data-ttu-id="d4d3b-226">Edit *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-226">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a><span data-ttu-id="d4d3b-227">Skonfiguruj opcje</span><span class="sxs-lookup"><span data-stu-id="d4d3b-227">Configure options</span></span>

<span data-ttu-id="d4d3b-228">Skonfiguruj serwer z dodatkowe wymagane moduły.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-228">Configure the server with additional required modules.</span></span> <span data-ttu-id="d4d3b-229">Należy rozważyć użycie zapory aplikacji sieci web, takich jak [ModSecurity](https://www.modsecurity.org/), w celu ograniczenia funkcjonalności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-229">Consider using a web app firewall, such as [ModSecurity](https://www.modsecurity.org/), to harden the app.</span></span>

#### <a name="configure-ssl"></a><span data-ttu-id="d4d3b-230">Konfigurowanie protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="d4d3b-230">Configure SSL</span></span>

* <span data-ttu-id="d4d3b-231">Konfigurowanie serwera do nasłuchiwania na ruch HTTPS na porcie `443` , określając prawidłowy certyfikat wystawiony przez zaufany urząd certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-231">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="d4d3b-232">Ograniczenia funkcjonalności zabezpieczeń poprzez zastosowanie niektórych rozwiązań przedstawione poniżej */etc/nginx/nginx.conf* pliku.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-232">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="d4d3b-233">Przykłady obejmują wybranie silniejszego szyfrowania i przekierowywania całego ruchu za pośrednictwem protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-233">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="d4d3b-234">Dodawanie `HTTP Strict-Transport-Security` nagłówka (HSTS) zapewnia wszystkie kolejne żądania przez klienta są tylko za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-234">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="d4d3b-235">Nie dodawaj nagłówka TLS Strict lub wybrać odpowiednie `max-age` Jeśli SSL zostanie wyłączone w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-235">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="d4d3b-236">Dodaj */etc/nginx/proxy.conf* pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-236">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="d4d3b-237">Edytuj */etc/nginx/nginx.conf* pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-237">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="d4d3b-238">Przykład zawiera zarówno `http` i `server` sekcji w pliku konfiguracji jednego.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-238">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="d4d3b-239">Bezpieczny Nginx z porywaniu kliknięć</span><span class="sxs-lookup"><span data-stu-id="d4d3b-239">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="d4d3b-240">Porywaniu kliknięć to technika złośliwego zbierać zainfekowane użytkownik klika polecenie.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-240">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="d4d3b-241">Porywaniu kliknięć sztuczki ofiara (użytkownik) do kliknięcia zainfekowane w witrynie.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-241">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="d4d3b-242">Użyj X-FRAME-OPTIONS do zabezpieczenia witryny.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-242">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="d4d3b-243">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-243">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="d4d3b-244">Dodaj wiersz `add_header X-Frame-Options "SAMEORIGIN";` i Zapisz plik, a następnie uruchom ponownie Nginx.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-244">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="d4d3b-245">Wykrywanie typ MIME</span><span class="sxs-lookup"><span data-stu-id="d4d3b-245">MIME-type sniffing</span></span>

<span data-ttu-id="d4d3b-246">Ten nagłówek zapobiega w większości przeglądarek z wykrywanie MIME odpowiedzi od deklarowanego typu zawartości, jak nagłówka powoduje, że przeglądarka nie, aby zastąpić typ zawartości odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-246">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="d4d3b-247">Z `nosniff` opcję, jeśli serwer jest wyświetlany komunikat "text/html" jest zawartość przeglądarki renderuje go jako "text/html".</span><span class="sxs-lookup"><span data-stu-id="d4d3b-247">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="d4d3b-248">Edytuj *nginx.conf* pliku:</span><span class="sxs-lookup"><span data-stu-id="d4d3b-248">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="d4d3b-249">Dodaj wiersz `add_header X-Content-Type-Options "nosniff";` i Zapisz plik, a następnie uruchom ponownie Nginx.</span><span class="sxs-lookup"><span data-stu-id="d4d3b-249">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4d3b-250">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d4d3b-250">Additional resources</span></span>

* [<span data-ttu-id="d4d3b-251">Nginx: Wersje binarne: oficjalny Debian/Ubuntu pakietów</span><span class="sxs-lookup"><span data-stu-id="d4d3b-251">Nginx: Binary Releases: Official Debian/Ubuntu packages</span></span>](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [<span data-ttu-id="d4d3b-252">Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="d4d3b-252">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
* [<span data-ttu-id="d4d3b-253">NGINX: Nagłówek przekazane za pomocą</span><span class="sxs-lookup"><span data-stu-id="d4d3b-253">NGINX: Using the Forwarded header</span></span>](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
