---
title: Wdróż aplikacje ASP.NET Core w Azure App Service
author: guardrex
description: Ten artykuł zawiera linki do hosta platformy Azure i wdrażania zasobów.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/28/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 4dc150ff4534e42e1995a185f650cea9df70ccc4
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187047"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="b1fc3-103">Wdróż aplikacje ASP.NET Core w Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b1fc3-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="b1fc3-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) to [usługa platformy obliczeniowej w chmurze firmy Microsoft](https://azure.microsoft.com/) do hostowania aplikacji sieci web, w tym ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="b1fc3-105">Przydatne zasoby</span><span class="sxs-lookup"><span data-stu-id="b1fc3-105">Useful resources</span></span>

<span data-ttu-id="b1fc3-106">[Dokumentacja App Service](/azure/app-service/) jest domem dla dokumentacji usługi Azure Apps, samouczków, przykładów, przewodników i innych zasobów.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-106">[App Service Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="b1fc3-107">Dwa istotne samouczki odnoszące się do hostingu aplikacji ASP.NET Core są następujące:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="b1fc3-108">Tworzenie aplikacji sieci Web ASP.NET Core na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="b1fc3-108">Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="b1fc3-109">Użyj programu Visual Studio, aby utworzyć i wdrożyć aplikację sieci Web ASP.NET Core do Azure App Service w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="b1fc3-110">Tworzenie aplikacji ASP.NET Core w usłudze App Service w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="b1fc3-110">Create an ASP.NET Core app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="b1fc3-111">Użyj wiersza polecenia, aby utworzyć i wdrożyć aplikację sieci Web ASP.NET Core do Azure App Service w systemie Linux.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="b1fc3-112">Następujące artykuły są dostępne w dokumentacji ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-112">The following articles are available in ASP.NET Core documentation:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="b1fc3-113">Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-113">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
<span data-ttu-id="b1fc3-114">Dowiedz się, jak utworzyć aplikację internetową ASP.NET Core przy użyciu programu Visual Studio i wdrożyć ją do Azure App Service przy użyciu narzędzia Git do ciągłego wdrażania.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-114">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="b1fc3-115">Tworzenie pierwszego potoku</span><span class="sxs-lookup"><span data-stu-id="b1fc3-115">Create your first pipeline</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="b1fc3-116">Skonfiguruj kompilację CI dla aplikacji ASP.NET Core, a następnie Utwórz wersję ciągłego wdrażania do Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-116">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="b1fc3-117">Piaskownica aplikacji sieci Web platformy Azure</span><span class="sxs-lookup"><span data-stu-id="b1fc3-117">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="b1fc3-118">Odkryj ograniczenia wykonywania w środowisku uruchomieniowym Azure App Service wymuszane przez platformę Azure Apps.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-118">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

<xref:test/troubleshoot>  
<span data-ttu-id="b1fc3-119">Zrozumienie i rozwiązywanie problemów z ostrzeżeniami i błędami w projektach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-119">Understand and troubleshoot warnings and errors with ASP.NET Core projects.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="b1fc3-120">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="b1fc3-120">Application configuration</span></span>

### <a name="platform"></a><span data-ttu-id="b1fc3-121">Platforma</span><span class="sxs-lookup"><span data-stu-id="b1fc3-121">Platform</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b1fc3-122">W Azure App Service są obecne środowiska uruchomieniowe dla aplikacji 64-bitowych (x64) i 32-bitowych (x86).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-122">Runtimes for 64-bit (x64) and 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="b1fc3-123">[Zestaw .NET Core SDK](/dotnet/core/sdk) dostępne w App Service to 32-bitowe, ale można wdrożyć aplikacje 64-bitowe utworzone lokalnie za pomocą konsoli [kudu](https://github.com/projectkudu/kudu/wiki) lub procesu publikowania w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-123">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit, but you can deploy 64-bit apps built locally using the [Kudu](https://github.com/projectkudu/kudu/wiki) console or the publish process in Visual Studio.</span></span> <span data-ttu-id="b1fc3-124">Aby uzyskać więcej informacji, zobacz sekcję [Publikowanie i wdrażanie aplikacji](#publish-and-deploy-the-app) .</span><span class="sxs-lookup"><span data-stu-id="b1fc3-124">For more information, see the [Publish and deploy the app](#publish-and-deploy-the-app) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b1fc3-125">W przypadku aplikacji z natywnymi zależnościami w Azure App Service są obecne środowiska uruchomieniowe dla 32-bitowych (x86) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-125">For apps with native dependencies, runtimes for 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="b1fc3-126">[Zestaw .NET Core SDK](/dotnet/core/sdk) dostępne w App Service to 32-bitowe.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-126">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit.</span></span>

::: moniker-end

<span data-ttu-id="b1fc3-127">Aby uzyskać więcej informacji na temat składników .NET Core i metod dystrybucji, takich jak informacje o środowisku uruchomieniowym .NET Core i zestaw .NET Core SDK, [Zobacz informacje o programie .NET Core: Kompozycja](/dotnet/core/about#composition).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-127">For more information on .NET Core framework components and distribution methods, such as information on the .NET Core runtime and the .NET Core SDK, see [About .NET Core: Composition](/dotnet/core/about#composition).</span></span>

### <a name="packages"></a><span data-ttu-id="b1fc3-128">Pakiety</span><span class="sxs-lookup"><span data-stu-id="b1fc3-128">Packages</span></span>

<span data-ttu-id="b1fc3-129">Dołącz następujące pakiety NuGet, aby udostępnić funkcje automatycznego rejestrowania dla aplikacji wdrożonych w Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-129">Include the following NuGet packages to provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="b1fc3-130">[Microsoft. AspNetCore. AzureAppServices. HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) używa [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) , aby zapewnić ASP.NET Coreą integrację z Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-130">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="b1fc3-131">Dodane funkcje rejestrowania są udostępniane przez `Microsoft.AspNetCore.AzureAppServicesIntegration` pakiet.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-131">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="b1fc3-132">[Microsoft. AspNetCore. AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) wykonuje [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) , aby dodać Azure App Service `Microsoft.Extensions.Logging.AzureAppServices` dostawców rejestrowania diagnostyki w pakiecie.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-132">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="b1fc3-133">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) udostępnia implementacje rejestratorów umożliwiające obsługę dzienników diagnostyki Azure App Service i funkcji przesyłania strumieniowego dzienników.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-133">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="b1fc3-134">Poprzednie pakiety nie są dostępne w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-134">The preceding packages aren't available from the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="b1fc3-135">Aplikacje, które są przeznaczone dla elementu `Microsoft.AspNetCore.App` .NET Framework lub odwołują się do pakietu, muszą jawnie odwoływać się do poszczególnych pakietów w pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-135">Apps that target .NET Framework or reference the `Microsoft.AspNetCore.App` metapackage must explicitly reference the individual packages in the app's project file.</span></span>

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="b1fc3-136">Przesłoń konfigurację aplikacji przy użyciu witryny Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b1fc3-136">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="b1fc3-137">Ustawienia aplikacji w witrynie Azure Portal umożliwiają ustawianie zmiennych środowiskowych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-137">App settings in the Azure Portal permit you to set environment variables for the app.</span></span> <span data-ttu-id="b1fc3-138">Zmienne środowiskowe mogą być używane przez [dostawcę konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-138">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="b1fc3-139">Po utworzeniu lub zmodyfikowaniu ustawienia aplikacji w witrynie Azure Portal i wybraniu przycisku **Zapisz** aplikacja platformy Azure zostanie uruchomiona ponownie.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-139">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="b1fc3-140">Zmienna środowiskowa jest dostępna dla aplikacji po ponownym uruchomieniu usługi.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-140">The environment variable is available to the app after the service restarts.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b1fc3-141">Gdy aplikacja korzysta z [hosta ogólnego](xref:fundamentals/host/generic-host), zmienne środowiskowe nie są domyślnie ładowane do konfiguracji aplikacji, a dostawca konfiguracji musi zostać dodany przez dewelopera.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-141">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="b1fc3-142">Deweloper Określa prefiks zmiennej środowiskowej podczas dodawania dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-142">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="b1fc3-143">Aby uzyskać więcej informacji, <xref:fundamentals/host/generic-host> Zobacz i [dostawca konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-143">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b1fc3-144">Gdy aplikacja kompiluje hosta za pomocą elementu [webhost. CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), zmienne środowiskowe, które konfigurują `ASPNETCORE_` hosta, używają prefiksu.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-144">When an app builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="b1fc3-145">Aby uzyskać więcej informacji, <xref:fundamentals/host/web-host> Zobacz i [dostawca konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-145">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="b1fc3-146">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="b1fc3-146">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="b1fc3-147">[Oprogramowanie pośredniczące integracji usług IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), które konfiguruje przekazane nagłówki oprogramowania pośredniczącego podczas hostingu [poza procesem](xref:host-and-deploy/iis/index#out-of-process-hosting-model), a moduł ASP.NET Core jest skonfigurowany do przesyłania dalej schematu (http/https) i zdalnego adresu IP, z którego pochodzi żądanie .</span><span class="sxs-lookup"><span data-stu-id="b1fc3-147">The [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), which configures Forwarded Headers Middleware when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="b1fc3-148">Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy dodatkowe i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-148">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="b1fc3-149">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="b1fc3-150">Monitorowanie i rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="b1fc3-150">Monitoring and logging</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b1fc3-151">ASP.NET Core aplikacje wdrożone do App Service automatycznie otrzymują rozszerzenie App Service **ASP.NET Core integrację rejestrowania**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-151">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Integration**.</span></span> <span data-ttu-id="b1fc3-152">Rozszerzenie włącza integrację rejestrowania dla aplikacji ASP.NET Core w Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-152">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b1fc3-153">ASP.NET Core aplikacje wdrożone do App Service automatycznie otrzymują rozszerzenia App Service **ASP.NET Core rejestrowania rozszerzeń**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-153">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Extensions**.</span></span> <span data-ttu-id="b1fc3-154">Rozszerzenie włącza integrację rejestrowania dla aplikacji ASP.NET Core w Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-154">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

<span data-ttu-id="b1fc3-155">Informacje o monitorowaniu, rejestrowaniu i rozwiązywaniu problemów znajdują się w następujących artykułach:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-155">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="b1fc3-156">Monitorowanie aplikacji w Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b1fc3-156">Monitor apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="b1fc3-157">Dowiedz się, jak przeglądać limity przydziału i metryki dla aplikacji i planów App Service.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-157">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="b1fc3-158">Włączanie rejestrowania diagnostycznego dla aplikacji w Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b1fc3-158">Enable diagnostics logging for apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="b1fc3-159">Odkryj, jak włączyć i uzyskać dostęp do rejestrowania diagnostycznego dla kodów stanu HTTP, żądań zakończonych niepowodzeniem i aktywności serwera sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-159">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="b1fc3-160">Poznaj typowe podejścia do obsługi błędów w aplikacjach ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-160">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

<xref:test/troubleshoot-azure-iis>  
<span data-ttu-id="b1fc3-161">Dowiedz się, jak zdiagnozować problemy z wdrożeniami Azure App Service przy użyciu aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-161">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-iis-errors-reference>  
<span data-ttu-id="b1fc3-162">Zapoznaj się z informacjami o typowych problemach z konfiguracją wdrażania dla aplikacji hostowanych przez Azure App Service/usług IIS.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-162">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="b1fc3-163">Pierścień i miejsca wdrożenia klucza ochrony danych</span><span class="sxs-lookup"><span data-stu-id="b1fc3-163">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="b1fc3-164">[Klucze ochrony danych](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) są utrwalane w folderze *%Home%\ASP.NET\DataProtection-Keys* .</span><span class="sxs-lookup"><span data-stu-id="b1fc3-164">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="b1fc3-165">Ten folder jest obsługiwany przez magazyn sieciowy i synchronizowany na wszystkich komputerach obsługujących aplikację.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-165">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="b1fc3-166">Klucze nie są chronione w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-166">Keys aren't protected at rest.</span></span> <span data-ttu-id="b1fc3-167">Ten folder dostarcza pierścień kluczy do wszystkich wystąpień aplikacji w jednym miejscu wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-167">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="b1fc3-168">Oddzielne miejsca wdrożenia, takie jak przygotowanie i środowisko produkcyjne, nie dzielą się z kluczem.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-168">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="b1fc3-169">W przypadku wymiany między miejscami wdrożenia każdy system korzystający z ochrony danych nie będzie w stanie odszyfrować przechowywanych danych przy użyciu dzwonka klucza w poprzednim gnieździe.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-169">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="b1fc3-170">Oprogramowanie pośredniczące ASP.NET plików cookie używa ochrony danych do ochrony plików cookie.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-170">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="b1fc3-171">Prowadzi to do użytkowników wylogowanych z aplikacji korzystającej ze standardowego oprogramowania ASP.NET cookie.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-171">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="b1fc3-172">W przypadku rozwiązania z niezależnym dzwonkiem, należy użyć zewnętrznego dostawcy usługi Key dystynktywnego, takiego jak:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-172">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="b1fc3-173">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="b1fc3-173">Azure Blob Storage</span></span>
* <span data-ttu-id="b1fc3-174">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="b1fc3-174">Azure Key Vault</span></span>
* <span data-ttu-id="b1fc3-175">Sklep SQL</span><span class="sxs-lookup"><span data-stu-id="b1fc3-175">SQL store</span></span>
* <span data-ttu-id="b1fc3-176">Pamięć podręczna Redis</span><span class="sxs-lookup"><span data-stu-id="b1fc3-176">Redis cache</span></span>

<span data-ttu-id="b1fc3-177">Aby uzyskać więcej informacji, zobacz <xref:security/data-protection/implementation/key-storage-providers>.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-177">For more information, see <xref:security/data-protection/implementation/key-storage-providers>.</span></span>
<a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>
<!-- revert this after 3.0 supported
## Deploy ASP.NET Core preview release to Azure App Service

Use one of the following approaches if the app relies on a preview release of .NET Core:

* [Install the preview site extension](#install-the-preview-site-extension).
* [Deploy a self-contained preview app](#deploy-a-self-contained-preview-app).
* [Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).
-->
## <a name="deploy-aspnet-core-30-to-azure-app-service"></a><span data-ttu-id="b1fc3-178">Wdróż ASP.NET Core 3,0 do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b1fc3-178">Deploy ASP.NET Core 3.0 to Azure App Service</span></span>

<span data-ttu-id="b1fc3-179">Mamy nadzieję, że na Azure App Service wkrótce będzie dostępna ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-179">We hope to have ASP.NET Core 3.0 available on Azure App Service soon.</span></span>

<span data-ttu-id="b1fc3-180">Jeśli aplikacja korzysta z platformy .NET Core 3,0, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-180">Use one of the following approaches if the app relies on .NET Core 3.0:</span></span>

* <span data-ttu-id="b1fc3-181">[Zainstaluj rozszerzenie witryny w wersji](#install-the-preview-site-extension)zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-181">[Install the preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="b1fc3-182">[Wdróż samodzielną aplikację w wersji](#deploy-a-self-contained-preview-app)zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-182">[Deploy a self-contained preview app](#deploy-a-self-contained-preview-app).</span></span>
* <span data-ttu-id="b1fc3-183">[Użyj platformy Docker z Web Apps dla kontenerów](#use-docker-with-web-apps-for-containers).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-183">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="b1fc3-184">Zainstaluj rozszerzenie witryny w wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="b1fc3-184">Install the preview site extension</span></span>

<span data-ttu-id="b1fc3-185">Jeśli wystąpi problem przy użyciu rozszerzenia witryny w wersji zapoznawczej, Otwórz [problem ASPNET/AspNetCore](https://github.com/aspnet/AspNetCore/issues).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-185">If a problem occurs using the preview site extension, open an [aspnet/AspNetCore issue](https://github.com/aspnet/AspNetCore/issues).</span></span>

1. <span data-ttu-id="b1fc3-186">W witrynie Azure Portal przejdź do App Service.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-186">From the Azure Portal, navigate to the App Service.</span></span>
1. <span data-ttu-id="b1fc3-187">Wybierz aplikację sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-187">Select the web app.</span></span>
1. <span data-ttu-id="b1fc3-188">Wpisz "ex" w polu wyszukiwania, aby odfiltrować "rozszerzenia", lub przewiń w dół listy narzędzi do zarządzania.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-188">Type "ex" in the search box to filter for "Extensions" or scroll down the list of management tools.</span></span>
1. <span data-ttu-id="b1fc3-189">Wybierz pozycję **rozszerzenia**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-189">Select **Extensions**.</span></span>
1. <span data-ttu-id="b1fc3-190">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-190">Select **Add**.</span></span>
1. <span data-ttu-id="b1fc3-191">ASP.NET Core wybierz z listy rozszerzenie **środowiska uruchomieniowego {X. Y} ({x64 | x86})** , na `{X.Y}` którym jest ASP.NET Core wersja zapoznawcza i `{x64|x86}` Określa platformę.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-191">Select the **ASP.NET Core {X.Y} ({x64|x86}) Runtime** extension from the list, where `{X.Y}` is the ASP.NET Core preview version and `{x64|x86}` specifies the platform.</span></span>
1. <span data-ttu-id="b1fc3-192">Wybierz **przycisk OK** , aby zaakceptować postanowienia prawne.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-192">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="b1fc3-193">Wybierz **przycisk OK** , aby zainstalować rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-193">Select **OK** to install the extension.</span></span>

<span data-ttu-id="b1fc3-194">Po zakończeniu operacji zostanie zainstalowana najnowsza wersja programu .NET Core Preview.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-194">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="b1fc3-195">Sprawdź instalację:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-195">Verify the installation:</span></span>

1. <span data-ttu-id="b1fc3-196">Wybierz pozycję **Narzędzia zaawansowane**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-196">Select **Advanced Tools**.</span></span>
1. <span data-ttu-id="b1fc3-197">Wybierz pozycję **Przejdź** do **zaawansowanych narzędzi**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-197">Select **Go** in **Advanced Tools**.</span></span>
1. <span data-ttu-id="b1fc3-198">Wybierz element menu**programu PowerShell** **konsoli** > debugowania.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-198">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="b1fc3-199">W wierszu polecenia programu PowerShell wykonaj następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-199">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="b1fc3-200">Zastąp wersję środowiska uruchomieniowego `{X.Y}` ASP.NET Core dla programu i `{PLATFORM}` platformy dla programu w ramach polecenia:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-200">Substitute the ASP.NET Core runtime version for `{X.Y}` and the platform for `{PLATFORM}` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   <span data-ttu-id="b1fc3-201">Polecenie zwraca wartość `True` , gdy jest zainstalowane środowisko uruchomieniowe x64 w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-201">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="b1fc3-202">Architektura platformy (x86/x64) aplikacji App Services jest ustawiana w ustawieniach aplikacji w witrynie Azure Portal dla aplikacji hostowanych w ramach obliczeń serii A lub lepszej warstwy hostingu.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-202">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="b1fc3-203">Jeśli aplikacja jest uruchamiana w trybie w procesie i architektura platformy jest skonfigurowana pod kątem 64-bitowej (x64), moduł ASP.NET Core używa 64-bitowego środowiska uruchomieniowego w wersji zapoznawczej, jeśli jest obecny.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-203">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="b1fc3-204">Zainstaluj rozszerzenie **środowiska uruchomieniowego ASP.NET Core {X. Y} (x64)** .</span><span class="sxs-lookup"><span data-stu-id="b1fc3-204">Install the **ASP.NET Core {X.Y} (x64) Runtime** extension.</span></span>
>
> <span data-ttu-id="b1fc3-205">Po zainstalowaniu środowiska uruchomieniowego w wersji zapoznawczej x64 Uruchom następujące polecenie w oknie poleceń programu PowerShell kudu, aby zweryfikować instalację.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-205">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="b1fc3-206">Zastąp wersję środowiska uruchomieniowego `{X.Y}` ASP.NET Core dla polecenia:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-206">Substitute the ASP.NET Core runtime version for `{X.Y}` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> <span data-ttu-id="b1fc3-207">Polecenie zwraca wartość `True` , gdy jest zainstalowane środowisko uruchomieniowe x64 w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-207">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="b1fc3-208">**Rozszerzenia ASP.NET Core** umożliwiają korzystanie z dodatkowych funkcji ASP.NET Core na platformie Azure App Services, takich jak Włączanie rejestrowania na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-208">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="b1fc3-209">Rozszerzenie jest instalowane automatycznie podczas wdrażania z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-209">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="b1fc3-210">Jeśli rozszerzenie nie jest zainstalowane, zainstaluj je dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-210">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="b1fc3-211">**Używanie rozszerzenia witryny w wersji zapoznawczej z szablonem ARM**</span><span class="sxs-lookup"><span data-stu-id="b1fc3-211">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="b1fc3-212">Jeśli szablon ARM jest używany do tworzenia i wdrażania aplikacji, `siteextensions` typ zasobu może służyć do dodawania rozszerzenia witryny do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-212">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="b1fc3-213">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-213">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-a-self-contained-preview-app"></a><span data-ttu-id="b1fc3-214">Wdrażanie samodzielnej aplikacji w wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="b1fc3-214">Deploy a self-contained preview app</span></span>

<span data-ttu-id="b1fc3-215">[Wdrożenie samodzielne (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) , które jest przeznaczone dla środowiska uruchomieniowego w wersji zapoznawczej, przenosi środowisko uruchomieniowe w wersji zapoznawczej w ramach wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-215">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="b1fc3-216">Podczas wdrażania aplikacji samodzielnej:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-216">When deploying a self-contained app:</span></span>

* <span data-ttu-id="b1fc3-217">Lokacja w Azure App Service nie wymaga [rozszerzenia witryny w wersji](#install-the-preview-site-extension)zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-217">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="b1fc3-218">Aplikacja musi zostać opublikowana przy użyciu innego podejścia niż w przypadku publikowania dla [wdrożenia zależnego od platformy (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-218">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="b1fc3-219">Postępuj zgodnie ze wskazówkami zawartymi w sekcji [Wdróż aplikację](#deploy-the-app-self-contained) samodzielną.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-219">Follow the guidance in the [Deploy the app self-contained](#deploy-the-app-self-contained) section.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="b1fc3-220">Korzystanie z platformy Docker z Web Apps dla kontenerów</span><span class="sxs-lookup"><span data-stu-id="b1fc3-220">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="b1fc3-221">[Centrum platformy Docker](https://hub.docker.com/r/microsoft/aspnetcore/) zawiera najnowsze obrazy platformy Docker w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-221">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="b1fc3-222">Obrazy mogą być używane jako obraz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-222">The images can be used as a base image.</span></span> <span data-ttu-id="b1fc3-223">Użyj obrazu i Wdróż go w celu zapewnienia normalnej Web Apps kontenerów.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-223">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="b1fc3-224">Publikowanie i wdrażanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="b1fc3-224">Publish and deploy the app</span></span>

### <a name="deploy-the-app-framework-dependent"></a><span data-ttu-id="b1fc3-225">Wdróż aplikację zależną od platformy</span><span class="sxs-lookup"><span data-stu-id="b1fc3-225">Deploy the app framework-dependent</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b1fc3-226">W przypadku wdrożenia 64-bitowego [zależnego od platformy](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="b1fc3-226">For a 64-bit [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

* <span data-ttu-id="b1fc3-227">Aby utworzyć aplikację 64-bitową, użyj 64-bitowej zestaw .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-227">Use a 64-bit .NET Core SDK to build a 64-bit app.</span></span>
* <span data-ttu-id="b1fc3-228">Ustaw **platformę** na **64 bit** w**ustawieniach ogólnych** **konfiguracji** > App Service.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-228">Set the **Platform** to **64 Bit** in the App Service's **Configuration** > **General settings**.</span></span> <span data-ttu-id="b1fc3-229">Aby umożliwić wybór liczby bitów platformy, aplikacja musi korzystać z planu usługi w warstwie Podstawowa lub wyższa.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-229">The app must use a Basic or higher service plan to enable the choice of platform bitness.</span></span>

::: moniker-end

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1fc3-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1fc3-230">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="b1fc3-231">Wybierz pozycję **kompilacja** > **Opublikuj {nazwa aplikacji}** na pasku narzędzi programu Visual Studio lub kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-231">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="b1fc3-232">W oknie dialogowym **Wybieranie elementu docelowego publikowania** upewnij się, że **App Service** jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-232">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="b1fc3-233">Wybierz pozycję **Zaawansowane**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-233">Select **Advanced**.</span></span> <span data-ttu-id="b1fc3-234">Zostanie otwarte okno dialogowe **Publikowanie** .</span><span class="sxs-lookup"><span data-stu-id="b1fc3-234">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="b1fc3-235">W **Publikuj** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-235">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="b1fc3-236">Upewnij się, że wybrano konfigurację **wydania** .</span><span class="sxs-lookup"><span data-stu-id="b1fc3-236">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="b1fc3-237">Otwórz listę rozwijaną **tryb wdrażania** i wybierz pozycję **zależne od struktury**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-237">Open the **Deployment Mode** drop-down list and select **Framework-Dependent**.</span></span>
   * <span data-ttu-id="b1fc3-238">Wybierz **przenośne** jako **docelowe środowisko uruchomieniowe**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-238">Select **Portable** as the **Target Runtime**.</span></span>
   * <span data-ttu-id="b1fc3-239">Jeśli konieczne jest usunięcie dodatkowych plików po wdrożeniu, Otwórz **Opcje publikowania plików** i zaznacz pole wyboru w celu usunięcia dodatkowych plików w miejscu docelowym.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-239">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="b1fc3-240">Wybierz pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-240">Select **Save**.</span></span>
1. <span data-ttu-id="b1fc3-241">Utwórz nową lokację lub zaktualizuj istniejącą witrynę, postępując zgodnie z pozostałymi instrukcjami Kreatora publikacji.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-241">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b1fc3-242">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b1fc3-242">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="b1fc3-243">W pliku projektu nie określaj [identyfikatora środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-243">In the project file, don't specify a [Runtime Identifier (RID)](/dotnet/core/rid-catalog).</span></span>

1. <span data-ttu-id="b1fc3-244">W powłoce poleceń Opublikuj aplikację w konfiguracji wydania przy użyciu polecenia [dotnet Publish](/dotnet/core/tools/dotnet-publish) .</span><span class="sxs-lookup"><span data-stu-id="b1fc3-244">From a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="b1fc3-245">W poniższym przykładzie aplikacja jest publikowana jako aplikacja zależna od platformy:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-245">In the following example, the app is published as a framework-dependent app:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="b1fc3-246">Przenieś zawartość katalogu *bin/Release/{Target Framework}/Publish* do lokacji programu w App Service.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-246">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="b1fc3-247">Jeśli przeciągasz zawartość folderu *publikowania* z lokalnego dysku twardego lub udziału sieciowego bezpośrednio do App Service w konsoli [kudu](https://github.com/projectkudu/kudu/wiki) , przeciągnij `D:\home\site\wwwroot` pliki do folderu w konsoli kudu.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-247">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the [Kudu](https://github.com/projectkudu/kudu/wiki) console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="b1fc3-248">Wdróż aplikację samodzielną</span><span class="sxs-lookup"><span data-stu-id="b1fc3-248">Deploy the app self-contained</span></span>

<span data-ttu-id="b1fc3-249">Użyj programu Visual Studio lub narzędzi interfejsu wiersza polecenia (CLI) dla samodzielnego [wdrożenia (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-249">Use Visual Studio or the command-line interface (CLI) tools for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b1fc3-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1fc3-250">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="b1fc3-251">Wybierz pozycję **kompilacja** > **Opublikuj {nazwa aplikacji}** na pasku narzędzi programu Visual Studio lub kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-251">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="b1fc3-252">W oknie dialogowym **Wybieranie elementu docelowego publikowania** upewnij się, że **App Service** jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-252">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="b1fc3-253">Wybierz pozycję **Zaawansowane**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-253">Select **Advanced**.</span></span> <span data-ttu-id="b1fc3-254">Zostanie otwarte okno dialogowe **Publikowanie** .</span><span class="sxs-lookup"><span data-stu-id="b1fc3-254">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="b1fc3-255">W **Publikuj** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-255">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="b1fc3-256">Upewnij się, że wybrano konfigurację **wydania** .</span><span class="sxs-lookup"><span data-stu-id="b1fc3-256">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="b1fc3-257">Otwórz listę rozwijaną **tryb wdrażania** i wybierz pozycję samodzielny.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-257">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="b1fc3-258">Wybierz docelowe środowisko uruchomieniowe z listy rozwijanej **docelowy środowisko uruchomieniowe** .</span><span class="sxs-lookup"><span data-stu-id="b1fc3-258">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="b1fc3-259">Wartość domyślna to `win-x86`.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-259">The default is `win-x86`.</span></span>
   * <span data-ttu-id="b1fc3-260">Jeśli konieczne jest usunięcie dodatkowych plików po wdrożeniu, Otwórz **Opcje publikowania plików** i zaznacz pole wyboru w celu usunięcia dodatkowych plików w miejscu docelowym.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-260">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="b1fc3-261">Wybierz pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-261">Select **Save**.</span></span>
1. <span data-ttu-id="b1fc3-262">Utwórz nową lokację lub zaktualizuj istniejącą witrynę, postępując zgodnie z pozostałymi instrukcjami Kreatora publikacji.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-262">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b1fc3-263">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b1fc3-263">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="b1fc3-264">W pliku projektu Określ jeden lub więcej [identyfikatorów środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-264">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="b1fc3-265">Użyj `<RuntimeIdentifier>` (pojedyncze) dla pojedynczego identyfikatora RID lub Użyj `<RuntimeIdentifiers>` (plural), aby podać listę identyfikatorów RID rozdzielonych średnikami.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-265">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="b1fc3-266">W poniższym przykładzie `win-x86` określono identyfikator RID:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-266">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>{TARGET FRAMEWORK}</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. <span data-ttu-id="b1fc3-267">W powłoce poleceń Opublikuj aplikację w konfiguracji wydania dla środowiska uruchomieniowego hosta za pomocą polecenia [dotnet Publish](/dotnet/core/tools/dotnet-publish) .</span><span class="sxs-lookup"><span data-stu-id="b1fc3-267">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="b1fc3-268">W poniższym przykładzie aplikacja jest publikowana dla `win-x86` identyfikatora RID.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-268">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="b1fc3-269">Identyfikator RID dostarczony do `--runtime` opcji musi być podany `<RuntimeIdentifier>` we właściwości (lub `<RuntimeIdentifiers>`) w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-269">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```

1. <span data-ttu-id="b1fc3-270">Przenieś zawartość katalogu *bin/Release/{Target Framework}/{Runtime identyfikator}/Publish* do lokacji w App Service.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-270">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="b1fc3-271">Jeśli przeciągasz zawartość folderu *publikowania* z lokalnego dysku twardego lub udziału sieciowego bezpośrednio do App Service w konsoli kudu, przeciągnij pliki do `D:\home\site\wwwroot` folderu w konsoli kudu.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-271">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the Kudu console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

## <a name="protocol-settings-https"></a><span data-ttu-id="b1fc3-272">Ustawienia protokołu (HTTPS)</span><span class="sxs-lookup"><span data-stu-id="b1fc3-272">Protocol settings (HTTPS)</span></span>

<span data-ttu-id="b1fc3-273">Bezpieczne powiązania protokołów pozwalają określić certyfikat, który ma być używany podczas odpowiadania na żądania za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-273">Secure protocol bindings allow you specify a certificate to use when responding to requests over HTTPS.</span></span> <span data-ttu-id="b1fc3-274">Powiązanie wymaga prawidłowego certyfikatu prywatnego (*PFX*) dla określonej nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-274">Binding requires a valid private certificate (*.pfx*) issued for the specific hostname.</span></span> <span data-ttu-id="b1fc3-275">Aby uzyskać więcej informacji, [zobacz Samouczek: Powiąż istniejący niestandardowy certyfikat SSL z Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-275">For more information, see [Tutorial: Bind an existing custom SSL certificate to Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

## <a name="transform-webconfig"></a><span data-ttu-id="b1fc3-276">Przekształcanie pliku web.config</span><span class="sxs-lookup"><span data-stu-id="b1fc3-276">Transform web.config</span></span>

<span data-ttu-id="b1fc3-277">Jeśli musisz przekształcić *plik Web. config* przy publikowaniu (na przykład ustawić zmienne środowiskowe na podstawie konfiguracji, profilu lub środowiska), zobacz <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="b1fc3-277">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1fc3-278">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b1fc3-278">Additional resources</span></span>

* [<span data-ttu-id="b1fc3-279">Przegląd App Service</span><span class="sxs-lookup"><span data-stu-id="b1fc3-279">App Service overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="b1fc3-280">Azure App Service: Najlepsze miejsce do hostowania aplikacji .NET (wideo z omówieniem 55 minut)</span><span class="sxs-lookup"><span data-stu-id="b1fc3-280">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="b1fc3-281">Piątek Azure: Azure App Service środowisko diagnostyczne i rozwiązywania problemów (wideo 12-minutowy)</span><span class="sxs-lookup"><span data-stu-id="b1fc3-281">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="b1fc3-282">Omówienie diagnostyki Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b1fc3-282">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="b1fc3-283">Azure App Service w systemie Windows Server używa [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="b1fc3-283">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="b1fc3-284">Poniższe tematy dotyczą podstawowej technologii usług IIS:</span><span class="sxs-lookup"><span data-stu-id="b1fc3-284">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="b1fc3-285">System Windows Server — zawartość administratora IT dla bieżących i poprzednich wersji</span><span class="sxs-lookup"><span data-stu-id="b1fc3-285">Windows Server - IT administrator content for current and previous releases</span></span>](/windows-server/windows-server-versions)
