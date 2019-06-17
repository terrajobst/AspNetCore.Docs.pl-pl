---
title: Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service
author: guardrex
description: Ten artykuł zawiera linki do hosta platformy Azure i wdrażanie zasobów.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/28/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 5daefde13310ebeb232ef4c8886b12ad78182e50
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048240"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="692a8-103">Wdrażanie aplikacji platformy ASP.NET Core w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="692a8-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="692a8-104">[Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) jest [chmury obliczeniowej platformy usługi firmy Microsoft](https://azure.microsoft.com/) do hostowania aplikacji sieci web, łącznie z platformą ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="692a8-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="692a8-105">Przydatne zasoby</span><span class="sxs-lookup"><span data-stu-id="692a8-105">Useful resources</span></span>

<span data-ttu-id="692a8-106">[Dokumentacja usługi App Service](/azure/app-service/) to miejsce, dokumentacja, samouczki, przykłady, przewodniki z instrukcjami i inne zasoby aplikacji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="692a8-106">[App Service Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="692a8-107">Są dwa istotne samouczków, które odnoszą się do hostowania aplikacji platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="692a8-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="692a8-108">Tworzenie aplikacji internetowej platformy ASP.NET Core na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="692a8-108">Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="692a8-109">Visual Studio umożliwia tworzenie i wdrażanie aplikacji sieci web ASP.NET Core w usłudze Azure App Service w Windows.</span><span class="sxs-lookup"><span data-stu-id="692a8-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="692a8-110">Tworzenie aplikacji platformy ASP.NET Core w usłudze App Service w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="692a8-110">Create an ASP.NET Core app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="692a8-111">Tworzenie i wdrażanie aplikacji sieci web ASP.NET Core w usłudze Azure App Service w systemie Linux, należy użyć wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="692a8-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="692a8-112">Następujące artykuły są dostępne w dokumentacji platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="692a8-112">The following articles are available in ASP.NET Core documentation:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="692a8-113">Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="692a8-113">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
<span data-ttu-id="692a8-114">Dowiedz się, jak utworzyć aplikację internetową platformy ASP.NET Core przy użyciu programu Visual Studio, a następnie wdrożyć ją w usłudze Azure App Service przy użyciu narzędzia Git do ciągłego wdrażania.</span><span class="sxs-lookup"><span data-stu-id="692a8-114">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="692a8-115">Tworzenie pierwszego potoku</span><span class="sxs-lookup"><span data-stu-id="692a8-115">Create your first pipeline</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="692a8-116">Skonfiguruj kompilację o ciągłej integracji dla aplikacji ASP.NET Core, a następnie utworzyć wydanie ciągłe wdrażanie w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="692a8-116">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="692a8-117">Usługa Azure piaskownica aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="692a8-117">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="692a8-118">Dowiedz się, wykonywanie ograniczenia środowiska uruchomieniowego usługi Azure App Service, wymuszane przez platformę Azure Apps.</span><span class="sxs-lookup"><span data-stu-id="692a8-118">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="692a8-119">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="692a8-119">Application configuration</span></span>

### <a name="platform"></a><span data-ttu-id="692a8-120">Platforma</span><span class="sxs-lookup"><span data-stu-id="692a8-120">Platform</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="692a8-121">Środowiska uruchomieniowe dla aplikacji 32-bitowych (x 86) i (x64) 64-bitowe znajdują się w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="692a8-121">Runtimes for 64-bit (x64) and 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="692a8-122">[Zestawu .NET Core SDK](/dotnet/core/sdk) dostępne w usłudze App Service jest 32-bitowych, ale możesz wdrażać aplikacje 64-bitowego za pomocą [Kudu](https://github.com/projectkudu/kudu/wiki) konsoli lub za pośrednictwem [publikowania MSDeploy z programem Visual Studio, profilu lub interfejsu wiersza polecenia](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="692a8-122">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit, but you can deploy 64-bit apps using the [Kudu](https://github.com/projectkudu/kudu/wiki) console or via [MSDeploy with a Visual Studio publish profile or CLI command](xref:host-and-deploy/visual-studio-publish-profiles).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="692a8-123">W przypadku aplikacji natywnych zależności środowiska uruchomieniowe dla aplikacji 32-bitowych (x 86) znajdują się w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="692a8-123">For apps with native dependencies, runtimes for 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="692a8-124">[Zestawu .NET Core SDK](/dotnet/core/sdk) dostępne w usłudze App Service jest 32-bitowy.</span><span class="sxs-lookup"><span data-stu-id="692a8-124">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit.</span></span>

::: moniker-end

### <a name="packages"></a><span data-ttu-id="692a8-125">Pakiety</span><span class="sxs-lookup"><span data-stu-id="692a8-125">Packages</span></span>

<span data-ttu-id="692a8-126">Należy uwzględnić następujące pakiety NuGet w celu zapewnienia funkcji automatycznego rejestrowania w przypadku aplikacji wdrożonych w usłudze Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="692a8-126">Include the following NuGet packages to provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="692a8-127">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) używa [interfejsu IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) nad umożliwieniem integracji światła w górę platformy ASP.NET Core w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="692a8-127">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="692a8-128">Funkcje rejestrowania dodano są dostarczane przez `Microsoft.AspNetCore.AzureAppServicesIntegration` pakietu.</span><span class="sxs-lookup"><span data-stu-id="692a8-128">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="692a8-129">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) wykonuje [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) dodać dostawców rejestrowania diagnostycznego usługi Azure App Service w `Microsoft.Extensions.Logging.AzureAppServices` pakietu.</span><span class="sxs-lookup"><span data-stu-id="692a8-129">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="692a8-130">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) zawiera implementacje rejestratora do obsługi dzienników diagnostycznych usługi Azure App Service i przesyłanie strumieniowe funkcji dzienników.</span><span class="sxs-lookup"><span data-stu-id="692a8-130">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="692a8-131">Poprzedni pakiety nie są dostępne z [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="692a8-131">The preceding packages aren't available from the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="692a8-132">Aplikacje, których platformą docelową jest program .NET Framework lub odwołanie `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all jawnie musi odwoływać się do poszczególnych pakietów w pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="692a8-132">Apps that target .NET Framework or reference the `Microsoft.AspNetCore.App` metapackage must explicitly reference the individual packages in the app's project file.</span></span>

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="692a8-133">Zastąpienie konfiguracji aplikacji przy użyciu witryny Azure Portal</span><span class="sxs-lookup"><span data-stu-id="692a8-133">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="692a8-134">Ustawienia aplikacji w witrynie Azure Portal pozwala ustawić zmienne środowiskowe dla optymalizacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="692a8-134">App settings in the Azure Portal permit you to set environment variables for the app.</span></span> <span data-ttu-id="692a8-135">Zmienne środowiskowe mogą być używane przez [dostawcę konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="692a8-135">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="692a8-136">Po utworzeniu lub zmodyfikowaniu w witrynie Azure Portal ustawienia aplikacji i **Zapisz** przycisk jest zaznaczony, ponownym uruchomieniu aplikacji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="692a8-136">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="692a8-137">Zmienna środowiskowa jest dostępne dla aplikacji, po ponownym uruchomieniu usługi.</span><span class="sxs-lookup"><span data-stu-id="692a8-137">The environment variable is available to the app after the service restarts.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="692a8-138">Jeśli aplikacja używa [ogólnego hosta](xref:fundamentals/host/generic-host), zmienne środowiskowe nie są domyślnie ładowany do konfiguracji aplikacji i dostawcy konfiguracji muszą zostać dodane przez dewelopera.</span><span class="sxs-lookup"><span data-stu-id="692a8-138">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="692a8-139">Deweloper Określa prefiks zmiennej środowiska, po dodaniu dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="692a8-139">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="692a8-140">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host> i [dostawcę konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="692a8-140">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="692a8-141">Kiedy aplikacja kompilacje hosta, za pomocą [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), użyj zmiennych środowiskowych, które skonfigurować hosta `ASPNETCORE_` prefiks.</span><span class="sxs-lookup"><span data-stu-id="692a8-141">When an app builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="692a8-142">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host> i [dostawcę konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="692a8-142">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="692a8-143">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="692a8-143">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="692a8-144">[Oprogramowania pośredniczącego integracji usługi IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), który konfiguruje przekazany oprogramowania pośredniczącego nagłówki odnośnie do hostowania [spoza procesu](xref:host-and-deploy/iis/index#out-of-process-hosting-model), i modułu ASP.NET Core są skonfigurowane do przekazywania schemat (HTTP/HTTPS) oraz zdalny adres IP, w której pochodzi żądanie.</span><span class="sxs-lookup"><span data-stu-id="692a8-144">The [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), which configures Forwarded Headers Middleware when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="692a8-145">Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy dodatkowe i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="692a8-145">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="692a8-146">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="692a8-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="692a8-147">Monitorowanie i rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="692a8-147">Monitoring and logging</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="692a8-148">Aplikacje platformy ASP.NET Core, wdrożyć w usłudze App Service automatycznie otrzymywać rozszerzenie usługi App Service **rejestrowania integracji programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="692a8-148">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Integration**.</span></span> <span data-ttu-id="692a8-149">Rozszerzenia umożliwiają integrację rejestrowanie dla aplikacji platformy ASP.NET Core w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="692a8-149">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="692a8-150">Aplikacje platformy ASP.NET Core, wdrożyć w usłudze App Service automatycznie otrzymywać rozszerzenie usługi App Service **platformy ASP.NET Core rejestrowania rozszerzenia**.</span><span class="sxs-lookup"><span data-stu-id="692a8-150">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Extensions**.</span></span> <span data-ttu-id="692a8-151">Rozszerzenia umożliwiają integrację rejestrowanie dla aplikacji platformy ASP.NET Core w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="692a8-151">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

<span data-ttu-id="692a8-152">Monitorowanie, rejestrowanie i informacje dotyczące rozwiązywania problemów zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="692a8-152">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="692a8-153">Monitorowanie aplikacji w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="692a8-153">Monitor apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="692a8-154">Dowiedz się, jak przeglądać limity przydziału i metryki, aby aplikacje i plany usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="692a8-154">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="692a8-155">Włączanie rejestrowania diagnostycznego dla aplikacji w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="692a8-155">Enable diagnostics logging for apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="692a8-156">Dowiedz się, jak włączyć i dostęp do rejestrowania diagnostycznego dla kodów stanu HTTP, żądań zakończonych niepowodzeniem i aktywności serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="692a8-156">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="692a8-157">Poznaj typowe metody obsługi błędów w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="692a8-157">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-apps/troubleshoot>  
<span data-ttu-id="692a8-158">Dowiedz się, jak diagnozować problemy z wdrożeniami w usłudze Azure App Service za pomocą aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="692a8-158">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-iis-errors-reference>  
<span data-ttu-id="692a8-159">Zobacz typowych błędów konfiguracji wdrażania dla aplikacji hostowanych przez Azure App Service/IIS za pomocą poradę dotyczącą rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="692a8-159">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="692a8-160">Pierścień klucz ochrony danych i miejsca wdrożenia</span><span class="sxs-lookup"><span data-stu-id="692a8-160">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="692a8-161">[Klucze ochrony danych](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) zostaną utrwalone w *%HOME%\ASP.NET\DataProtection-Keys* folderu.</span><span class="sxs-lookup"><span data-stu-id="692a8-161">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="692a8-162">Ten folder jest wspierana przez sieć, Magazyn, które jest synchronizowane na wszystkich maszynach hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="692a8-162">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="692a8-163">Klucze nie są chronione w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="692a8-163">Keys aren't protected at rest.</span></span> <span data-ttu-id="692a8-164">Ten folder zawiera pierścień klucza do wszystkich wystąpień aplikacji w gnieździe pojedynczego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="692a8-164">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="692a8-165">Gniazda wdrażane pojedynczo, takich jak przejściowe i produkcyjne, nie udostępniaj klucza pierścień.</span><span class="sxs-lookup"><span data-stu-id="692a8-165">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="692a8-166">Po zamianie między miejscami wdrożenia, każdy system przy użyciu ochrony danych będzie możliwe do odszyfrowywania danych przechowywanych w poprzednim miejsca przy użyciu pierścień klucza.</span><span class="sxs-lookup"><span data-stu-id="692a8-166">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="692a8-167">Oprogramowaniu pośredniczącym pliku Cookie ASP.NET używa ochrony danych, aby chronić swoje pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="692a8-167">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="692a8-168">Prowadzi to do użytkowników, trwa wylogowanie z aplikacji, która używa standardowych oprogramowaniu pośredniczącym pliku Cookie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="692a8-168">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="692a8-169">Jako rozwiązanie pierścień klucz niezależnie od miejsca należy użyć dostawcy zewnętrznego pierścienia klucz takiego jak:</span><span class="sxs-lookup"><span data-stu-id="692a8-169">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="692a8-170">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="692a8-170">Azure Blob Storage</span></span>
* <span data-ttu-id="692a8-171">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="692a8-171">Azure Key Vault</span></span>
* <span data-ttu-id="692a8-172">Magazyn SQL</span><span class="sxs-lookup"><span data-stu-id="692a8-172">SQL store</span></span>
* <span data-ttu-id="692a8-173">Pamięć podręczna redis</span><span class="sxs-lookup"><span data-stu-id="692a8-173">Redis cache</span></span>

<span data-ttu-id="692a8-174">Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/implementation/key-storage-providers>.</span><span class="sxs-lookup"><span data-stu-id="692a8-174">For more information, see <xref:security/data-protection/implementation/key-storage-providers>.</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="692a8-175">Wdrażanie platformy ASP.NET Core w wersji zapoznawczej w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="692a8-175">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="692a8-176">Użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="692a8-176">Use one of the following approaches:</span></span>

* <span data-ttu-id="692a8-177">[Zainstalować rozszerzenie witryny usługi (wersja zapoznawcza)](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="692a8-177">[Install the preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="692a8-178">[Wdróż aplikację, która jest niezależna](#deploy-the-app-self-contained).</span><span class="sxs-lookup"><span data-stu-id="692a8-178">[Deploy the app self-contained](#deploy-the-app-self-contained).</span></span>
* <span data-ttu-id="692a8-179">[Używać platformy Docker z funkcją Web Apps for containers](#use-docker-with-web-apps-for-containers).</span><span class="sxs-lookup"><span data-stu-id="692a8-179">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="692a8-180">Zainstalować rozszerzenie witryny usługi (wersja zapoznawcza)</span><span class="sxs-lookup"><span data-stu-id="692a8-180">Install the preview site extension</span></span>

<span data-ttu-id="692a8-181">Jeśli wystąpi problem, za pomocą rozszerzenia witryny (wersja zapoznawcza), otworzyć [problem aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/issues).</span><span class="sxs-lookup"><span data-stu-id="692a8-181">If a problem occurs using the preview site extension, open an [aspnet/AspNetCore issue](https://github.com/aspnet/AspNetCore/issues).</span></span>

1. <span data-ttu-id="692a8-182">W witrynie Azure Portal przejdź do usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="692a8-182">From the Azure Portal, navigate to the App Service.</span></span>
1. <span data-ttu-id="692a8-183">Wybierz aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="692a8-183">Select the web app.</span></span>
1. <span data-ttu-id="692a8-184">Typ "ex" w polu wyszukiwania, aby filtrować "Rozszerzenia" lub przewiń w dół na liście narzędzi do zarządzania.</span><span class="sxs-lookup"><span data-stu-id="692a8-184">Type "ex" in the search box to filter for "Extensions" or scroll down the list of management tools.</span></span>
1. <span data-ttu-id="692a8-185">Wybierz **rozszerzenia**.</span><span class="sxs-lookup"><span data-stu-id="692a8-185">Select **Extensions**.</span></span>
1. <span data-ttu-id="692a8-186">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="692a8-186">Select **Add**.</span></span>
1. <span data-ttu-id="692a8-187">Wybierz **platformy ASP.NET Core {X.Y} ({x64 | x86}) środowiska uruchomieniowego** rozszerzenia z listy, gdzie `{X.Y}` jest w wersji zapoznawczej platformy ASP.NET Core i `{x64|x86}` Określa platformę.</span><span class="sxs-lookup"><span data-stu-id="692a8-187">Select the **ASP.NET Core {X.Y} ({x64|x86}) Runtime** extension from the list, where `{X.Y}` is the ASP.NET Core preview version and `{x64|x86}` specifies the platform.</span></span>
1. <span data-ttu-id="692a8-188">Wybierz **OK** aby zaakceptować warunki prawne.</span><span class="sxs-lookup"><span data-stu-id="692a8-188">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="692a8-189">Wybierz **OK** można zainstalować rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="692a8-189">Select **OK** to install the extension.</span></span>

<span data-ttu-id="692a8-190">Po zakończeniu tej operacji jest zainstalowana najnowsza wersja zapoznawcza platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="692a8-190">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="692a8-191">Zweryfikuj instalację:</span><span class="sxs-lookup"><span data-stu-id="692a8-191">Verify the installation:</span></span>

1. <span data-ttu-id="692a8-192">Wybierz **zaawansowane narzędzia**.</span><span class="sxs-lookup"><span data-stu-id="692a8-192">Select **Advanced Tools**.</span></span>
1. <span data-ttu-id="692a8-193">Wybierz **Przejdź** w **zaawansowane narzędzia**.</span><span class="sxs-lookup"><span data-stu-id="692a8-193">Select **Go** in **Advanced Tools**.</span></span>
1. <span data-ttu-id="692a8-194">Wybierz **konsoli debugowania** > **PowerShell** elementu menu.</span><span class="sxs-lookup"><span data-stu-id="692a8-194">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="692a8-195">W wierszu polecenia programu PowerShell uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="692a8-195">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="692a8-196">Zastąpiono wersję środowiska uruchomieniowego platformy ASP.NET Core dla `{X.Y}` i platforma dla `{PLATFORM}` w poleceniu:</span><span class="sxs-lookup"><span data-stu-id="692a8-196">Substitute the ASP.NET Core runtime version for `{X.Y}` and the platform for `{PLATFORM}` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   <span data-ttu-id="692a8-197">Polecenie zwraca `True` podczas x64 czas wykonywania (wersja zapoznawcza) jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="692a8-197">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="692a8-198">Architektura platformy — x86/x64 64 aplikację usługi App Services jest ustawiona w ustawieniach aplikacji w witrynie Azure Portal dla hostowanym w obliczeniowej serii A i lepsze, hostowanie warstwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="692a8-198">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="692a8-199">Jeśli aplikacja jest uruchamiana w trybie w procesie, architektura platformy jest skonfigurowany dla 64-bitowych (x64) modułu ASP.NET Core używa środowiska uruchomieniowego 64-bitowych (wersja zapoznawcza), jeśli jest obecny.</span><span class="sxs-lookup"><span data-stu-id="692a8-199">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="692a8-200">Zainstaluj **platformy ASP.NET Core {X.Y} (x64) środowiska uruchomieniowego** rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="692a8-200">Install the **ASP.NET Core {X.Y} (x64) Runtime** extension.</span></span>
>
> <span data-ttu-id="692a8-201">Po zainstalowaniu x64 podglądu środowiska uruchomieniowego, uruchom następujące polecenie w oknie wiersza polecenia programu PowerShell programu Kudu, aby zweryfikować instalację.</span><span class="sxs-lookup"><span data-stu-id="692a8-201">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="692a8-202">Zastąpiono wersję środowiska uruchomieniowego platformy ASP.NET Core dla `{X.Y}` w poleceniu:</span><span class="sxs-lookup"><span data-stu-id="692a8-202">Substitute the ASP.NET Core runtime version for `{X.Y}` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> <span data-ttu-id="692a8-203">Polecenie zwraca `True` podczas x64 czas wykonywania (wersja zapoznawcza) jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="692a8-203">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="692a8-204">**ASP.NET Core rozszerzenia** zapewnia dodatkowe funkcje dla platformy ASP.NET Core w usłudze Azure App Services, takich jak włączenie rejestrowania platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="692a8-204">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="692a8-205">Rozszerzenie jest instalowana automatycznie podczas wdrażania w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="692a8-205">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="692a8-206">Jeśli rozszerzenie nie jest zainstalowany, zainstaluj go dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="692a8-206">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="692a8-207">**Rozszerzenie witryny (wersja zapoznawcza) za pomocą szablonu usługi ARM**</span><span class="sxs-lookup"><span data-stu-id="692a8-207">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="692a8-208">Jeśli szablon ARM jest używany do tworzenia i wdrażania aplikacji, `siteextensions` typu zasobu może służyć do dodawania rozszerzenia witryny aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="692a8-208">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="692a8-209">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="692a8-209">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="692a8-210">Wdróż aplikację, która jest niezależna</span><span class="sxs-lookup"><span data-stu-id="692a8-210">Deploy the app self-contained</span></span>

<span data-ttu-id="692a8-211">A [niezależna wdrożenia (— SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) który jest przeznaczony dla wersji zapoznawczej środowiska uruchomieniowego niesie ze sobą w środowisku uruchomieniowym w wersji zapoznawczej we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="692a8-211">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="692a8-212">W przypadku wdrażania aplikacja samodzielna:</span><span class="sxs-lookup"><span data-stu-id="692a8-212">When deploying a self-contained app:</span></span>

* <span data-ttu-id="692a8-213">Witryny w usłudze Azure App Service nie wymaga [rozszerzenie witryny w wersji zapoznawczej](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="692a8-213">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="692a8-214">Aplikacja musi zostać opublikowany, zgodnie z innego podejścia niż podczas publikowania dla [zależny od struktury wdrożenia (stacje)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="692a8-214">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

#### <a name="publish-from-visual-studio"></a><span data-ttu-id="692a8-215">Publikowanie z programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="692a8-215">Publish from Visual Studio</span></span>

1. <span data-ttu-id="692a8-216">Wybierz **kompilacji** > **publikowania {Nazwa aplikacji}** na pasku narzędzi programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="692a8-216">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar.</span></span>
1. <span data-ttu-id="692a8-217">W **wybierz lokalizację docelową publikowania** okna dialogowego, upewnij się, że **usługi App Service** jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="692a8-217">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="692a8-218">Wybierz **zaawansowane**.</span><span class="sxs-lookup"><span data-stu-id="692a8-218">Select **Advanced**.</span></span> <span data-ttu-id="692a8-219">**Publikuj** zostanie otwarte okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="692a8-219">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="692a8-220">W **Publikuj** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="692a8-220">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="692a8-221">Upewnij się, że **wersji** wybrać konfigurację.</span><span class="sxs-lookup"><span data-stu-id="692a8-221">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="692a8-222">Otwórz **tryb wdrożenia** listy rozwijanej i wybierz pozycję **niezależna**.</span><span class="sxs-lookup"><span data-stu-id="692a8-222">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="692a8-223">Wybierz docelowe środowisko uruchomieniowe z **docelowe środowisko uruchomieniowe** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="692a8-223">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="692a8-224">Wartość domyślna to `win-x86`.</span><span class="sxs-lookup"><span data-stu-id="692a8-224">The default is `win-x86`.</span></span>
   * <span data-ttu-id="692a8-225">Jeśli potrzebujesz usunięcie dodatkowych plików po wdrożeniu, otwórz **opcji publikowania pliku** i zaznacz pole wyboru, aby usunąć dodatkowe pliki w lokalizacji docelowej.</span><span class="sxs-lookup"><span data-stu-id="692a8-225">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="692a8-226">Wybierz pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="692a8-226">Select **Save**.</span></span>
1. <span data-ttu-id="692a8-227">Utwórz nową witrynę, lub zaktualizuj istniejącą lokację, postępując zgodnie z pozostałymi instrukcjami w Kreatorze publikacji.</span><span class="sxs-lookup"><span data-stu-id="692a8-227">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

#### <a name="publish-using-command-line-interface-cli-tools"></a><span data-ttu-id="692a8-228">Publikowanie za pomocą narzędzia interfejsu wiersza polecenia (CLI)</span><span class="sxs-lookup"><span data-stu-id="692a8-228">Publish using command-line interface (CLI) tools</span></span>

1. <span data-ttu-id="692a8-229">W pliku projektu, należy określić co najmniej jedną [identyfikatorów środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="692a8-229">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="692a8-230">Użyj `<RuntimeIdentifier>` (pojedynczą) dla jednego identyfikatorów RID lub użyj `<RuntimeIdentifiers>` (liczba mnoga) można podać rozdzieloną średnikami listę identyfikatorów RID.</span><span class="sxs-lookup"><span data-stu-id="692a8-230">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="692a8-231">W poniższym przykładzie `win-x86` określono identyfikatorów RID:</span><span class="sxs-lookup"><span data-stu-id="692a8-231">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>netcoreapp2.1</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. <span data-ttu-id="692a8-232">Z powłoki poleceń, Opublikuj aplikację w konfiguracji wydania dla aparatu plików wykonywalnych hosta [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenia.</span><span class="sxs-lookup"><span data-stu-id="692a8-232">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="692a8-233">W poniższym przykładzie aplikacja została opublikowana na potrzeby `win-x86` identyfikatorów RID.</span><span class="sxs-lookup"><span data-stu-id="692a8-233">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="692a8-234">RID dostarczane do `--runtime` w musi być podana opcja `<RuntimeIdentifier>` (lub `<RuntimeIdentifiers>`) właściwość w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="692a8-234">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```

1. <span data-ttu-id="692a8-235">Przenieś zawartość *wersji/bin / {w TARGET FRAMEWORK} / {identyfikator środowiska URUCHOMIENIOWEGO} / publish* katalogu do lokacji w usłudze App Service.</span><span class="sxs-lookup"><span data-stu-id="692a8-235">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="692a8-236">Używać platformy Docker z funkcją Web Apps for containers</span><span class="sxs-lookup"><span data-stu-id="692a8-236">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="692a8-237">[Usługi Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) zawiera najnowsze obrazy platformy Docker w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="692a8-237">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="692a8-238">Obrazy może służyć jako obrazu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="692a8-238">The images can be used as a base image.</span></span> <span data-ttu-id="692a8-239">Przy użyciu obrazu i wdrażanie w usłudze Web Apps for Containers normalnie.</span><span class="sxs-lookup"><span data-stu-id="692a8-239">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="protocol-settings-https"></a><span data-ttu-id="692a8-240">Ustawienia protokołu (HTTPS)</span><span class="sxs-lookup"><span data-stu-id="692a8-240">Protocol settings (HTTPS)</span></span>

<span data-ttu-id="692a8-241">Powiązania bezpiecznego protokołu zezwalania Określ certyfikat do użycia podczas odpowiadania na żądania za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="692a8-241">Secure protocol bindings allow you specify a certificate to use when responding to requests over HTTPS.</span></span> <span data-ttu-id="692a8-242">Powiązanie wymaga ważnego certyfikatu prywatnego (*PFX*) dla określonej nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="692a8-242">Binding requires a valid private certificate (*.pfx*) issued for the specific hostname.</span></span> <span data-ttu-id="692a8-243">Aby uzyskać więcej informacji, zobacz [samouczka: Powiązywanie istniejącego niestandardowego certyfikatu SSL w usłudze Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="692a8-243">For more information, see [Tutorial: Bind an existing custom SSL certificate to Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

## <a name="transform-webconfig"></a><span data-ttu-id="692a8-244">Przekształcanie pliku web.config</span><span class="sxs-lookup"><span data-stu-id="692a8-244">Transform web.config</span></span>

<span data-ttu-id="692a8-245">Jeśli potrzebujesz do przekształcania *web.config* przy publikowaniu (na przykład ustawienie zmiennych środowiskowych na podstawie konfiguracji, profilu lub środowiska), zobacz <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="692a8-245">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="692a8-246">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="692a8-246">Additional resources</span></span>

* [<span data-ttu-id="692a8-247">Omówienie usługi App Service</span><span class="sxs-lookup"><span data-stu-id="692a8-247">App Service overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="692a8-248">Azure App Service: Najlepiej umieścić na hoście aplikacji .NET (55-minutowy klip wideo z omówieniem)</span><span class="sxs-lookup"><span data-stu-id="692a8-248">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="692a8-249">Azure Friday: Azure diagnostyki usługi aplikacji i rozwiązywanie problemów z doświadczenia (12-minutowy klip wideo)</span><span class="sxs-lookup"><span data-stu-id="692a8-249">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="692a8-250">Omówienie diagnostyki w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="692a8-250">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="692a8-251">Usługa Azure App Service w systemie Windows Server [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="692a8-251">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="692a8-252">Poniższe tematy odnoszą się do podstawowych technologii usług IIS:</span><span class="sxs-lookup"><span data-stu-id="692a8-252">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="692a8-253">System Windows Server — zawartość administratora IT dla bieżących i wcześniejszych wersji</span><span class="sxs-lookup"><span data-stu-id="692a8-253">Windows Server - IT administrator content for current and previous releases</span></span>](/windows-server/windows-server-versions)
