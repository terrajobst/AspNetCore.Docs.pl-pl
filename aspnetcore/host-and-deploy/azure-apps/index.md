---
title: Host platformy ASP.NET Core w usłudze Azure App Service
author: guardrex
description: Dowiedz się, jak hostować aplikacje platformy ASP.NET Core w usłudze Azure App Service wraz z łączami do przydatnych zasobów.
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 23289823e154d93e4bedd23a1efae0e58c71eae0
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340189"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="3f74d-103">Host platformy ASP.NET Core w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3f74d-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="3f74d-104">[Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) jest [chmury obliczeniowej platformy usługi firmy Microsoft](https://azure.microsoft.com/) do hostowania aplikacji sieci web, łącznie z platformą ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f74d-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="3f74d-105">Przydatne zasoby</span><span class="sxs-lookup"><span data-stu-id="3f74d-105">Useful resources</span></span>

<span data-ttu-id="3f74d-106">Azure [dokumentację usługi Web Apps](/azure/app-service/) to miejsce, dokumentacja, samouczki, przykłady, przewodniki z instrukcjami i inne zasoby aplikacji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="3f74d-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="3f74d-107">Są dwa istotne samouczków, które odnoszą się do hostowania aplikacji platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3f74d-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="3f74d-108">Szybki Start: Tworzenie aplikacji internetowej platformy ASP.NET Core na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="3f74d-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="3f74d-109">Visual Studio umożliwia tworzenie i wdrażanie aplikacji sieci web ASP.NET Core w usłudze Azure App Service w Windows.</span><span class="sxs-lookup"><span data-stu-id="3f74d-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="3f74d-110">Szybki Start: Tworzenie aplikacji sieci web platformy .NET Core w usłudze App Service w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="3f74d-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="3f74d-111">Tworzenie i wdrażanie aplikacji sieci web ASP.NET Core w usłudze Azure App Service w systemie Linux, należy użyć wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="3f74d-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="3f74d-112">Następujące artykuły są dostępne w dokumentacji platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3f74d-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="3f74d-113">Publikowanie na platformie Azure przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f74d-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="3f74d-114">Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f74d-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="3f74d-115">Publikowanie na platformie Azure przy użyciu narzędzi interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="3f74d-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="3f74d-116">Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service za pomocą klienta wiersza polecenia usługi Git.</span><span class="sxs-lookup"><span data-stu-id="3f74d-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="3f74d-117">Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i rozwiązania Git</span><span class="sxs-lookup"><span data-stu-id="3f74d-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="3f74d-118">Dowiedz się, jak utworzyć aplikację internetową platformy ASP.NET Core przy użyciu programu Visual Studio, a następnie wdrożyć ją w usłudze Azure App Service przy użyciu narzędzia Git do ciągłego wdrażania.</span><span class="sxs-lookup"><span data-stu-id="3f74d-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="3f74d-119">Tworzenie pierwszego potoku za pomocą potoków usługi Azure</span><span class="sxs-lookup"><span data-stu-id="3f74d-119">Create your first pipeline with Azure Pipelines</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="3f74d-120">Skonfiguruj kompilację o ciągłej integracji dla aplikacji ASP.NET Core, a następnie utworzyć wydanie ciągłe wdrażanie w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3f74d-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="3f74d-121">Usługa Azure piaskownica aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="3f74d-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="3f74d-122">Dowiedz się, wykonywanie ograniczenia środowiska uruchomieniowego usługi Azure App Service, wymuszane przez platformę Azure Apps.</span><span class="sxs-lookup"><span data-stu-id="3f74d-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="3f74d-123">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="3f74d-123">Application configuration</span></span>

<span data-ttu-id="3f74d-124">W programie ASP.NET Core 2.0 lub nowszej następujące pakiety NuGet zapewniają funkcji automatycznego rejestrowania w przypadku aplikacji wdrożonych w usłudze Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="3f74d-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="3f74d-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) używa [interfejsu IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) nad umożliwieniem integracji światła w górę platformy ASP.NET Core w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3f74d-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="3f74d-126">Funkcje rejestrowania dodano są dostarczane przez `Microsoft.AspNetCore.AzureAppServicesIntegration` pakietu.</span><span class="sxs-lookup"><span data-stu-id="3f74d-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="3f74d-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) wykonuje [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) dodać dostawców rejestrowania diagnostycznego usługi Azure App Service w `Microsoft.Extensions.Logging.AzureAppServices` pakietu.</span><span class="sxs-lookup"><span data-stu-id="3f74d-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="3f74d-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) zawiera implementacje rejestratora do obsługi dzienników diagnostycznych usługi Azure App Service i przesyłanie strumieniowe funkcji dzienników.</span><span class="sxs-lookup"><span data-stu-id="3f74d-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="3f74d-129">Jeśli przeznaczonych dla platformy .NET Core i odwoływanie się do [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage), pakiety są już uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="3f74d-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="3f74d-130">Pakiety nie są spełnione z nowszego [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3f74d-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="3f74d-131">Jeśli przeznaczonych dla platformy .NET Framework lub odwołuje się do `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all, odwołania się do pakietów indywidualne rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="3f74d-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="3f74d-132">Zastąpienie konfiguracji aplikacji przy użyciu witryny Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3f74d-132">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="3f74d-133">**Ustawienia aplikacji** obszaru **ustawienia aplikacji** bloku pozwala na Ustawianie zmiennych środowiskowych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3f74d-133">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="3f74d-134">Zmienne środowiskowe mogą być używane przez [dostawcę konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="3f74d-134">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="3f74d-135">Jeśli aplikacja używa [hosta sieci Web](xref:fundamentals/host/web-host) i kompilacje hosta, za pomocą [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), użyj zmiennych środowiskowych, które skonfigurować hosta `ASPNETCORE_` prefiks.</span><span class="sxs-lookup"><span data-stu-id="3f74d-135">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="3f74d-136">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host> i [dostawcę konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="3f74d-136">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="3f74d-137">Jeśli aplikacja używa [ogólnego hosta](xref:fundamentals/host/generic-host), zmienne środowiskowe nie są domyślnie ładowany do konfiguracji aplikacji i dostawcy konfiguracji muszą zostać dodane przez dewelopera.</span><span class="sxs-lookup"><span data-stu-id="3f74d-137">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="3f74d-138">Deweloper Określa prefiks zmiennej środowiska, po dodaniu dostawcy konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="3f74d-138">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="3f74d-139">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host> i [dostawcę konfiguracji zmiennych środowiskowych](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="3f74d-139">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="3f74d-140">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="3f74d-140">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="3f74d-141">Usługi IIS oprogramowania pośredniczącego integracji, który konfiguruje przekazany oprogramowania pośredniczącego nagłówków i modułu ASP.NET Core są skonfigurowane do przekazywania schemat (HTTP/HTTPS) i zdalny adres IP, skąd pochodzi żądanie.</span><span class="sxs-lookup"><span data-stu-id="3f74d-141">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="3f74d-142">Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy dodatkowe i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="3f74d-142">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="3f74d-143">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="3f74d-143">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="3f74d-144">Monitorowanie i rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="3f74d-144">Monitoring and logging</span></span>

<span data-ttu-id="3f74d-145">Monitorowanie, rejestrowanie i informacje dotyczące rozwiązywania problemów zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="3f74d-145">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="3f74d-146">Porady: monitorowanie aplikacji w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3f74d-146">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="3f74d-147">Dowiedz się, jak przeglądać limity przydziału i metryki, aby aplikacje i plany usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="3f74d-147">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="3f74d-148">Włączanie rejestrowania diagnostycznego dla aplikacji sieci web w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3f74d-148">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="3f74d-149">Dowiedz się, jak włączyć i dostęp do rejestrowania diagnostycznego dla kodów stanu HTTP, żądań zakończonych niepowodzeniem i aktywności serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="3f74d-149">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="3f74d-150">Wprowadzenie do obsługi błędów w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f74d-150">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="3f74d-151">Poznaj typowe metody obsługi błędów w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f74d-151">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="3f74d-152">Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3f74d-152">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="3f74d-153">Dowiedz się, jak diagnozować problemy z wdrożeniami w usłudze Azure App Service za pomocą aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f74d-153">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="3f74d-154">Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f74d-154">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="3f74d-155">Zobacz typowych błędów konfiguracji wdrażania dla aplikacji hostowanych przez Azure App Service/IIS za pomocą poradę dotyczącą rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="3f74d-155">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="3f74d-156">Pierścień klucz ochrony danych i miejsca wdrożenia</span><span class="sxs-lookup"><span data-stu-id="3f74d-156">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="3f74d-157">[Klucze ochrony danych](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) zostaną utrwalone w *%HOME%\ASP.NET\DataProtection-Keys* folderu.</span><span class="sxs-lookup"><span data-stu-id="3f74d-157">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="3f74d-158">Ten folder jest wspierana przez sieć, Magazyn, które jest synchronizowane na wszystkich maszynach hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3f74d-158">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="3f74d-159">Klucze nie są chronione w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="3f74d-159">Keys aren't protected at rest.</span></span> <span data-ttu-id="3f74d-160">Ten folder zawiera pierścień klucza do wszystkich wystąpień aplikacji w gnieździe pojedynczego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="3f74d-160">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="3f74d-161">Gniazda wdrażane pojedynczo, takich jak przejściowe i produkcyjne, nie udostępniaj klucza pierścień.</span><span class="sxs-lookup"><span data-stu-id="3f74d-161">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="3f74d-162">Po zamianie między miejscami wdrożenia, każdy system przy użyciu ochrony danych będzie możliwe do odszyfrowywania danych przechowywanych w poprzednim miejsca przy użyciu pierścień klucza.</span><span class="sxs-lookup"><span data-stu-id="3f74d-162">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="3f74d-163">Oprogramowaniu pośredniczącym pliku Cookie ASP.NET używa ochrony danych, aby chronić swoje pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="3f74d-163">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="3f74d-164">Prowadzi to do użytkowników, trwa wylogowanie z aplikacji, która używa standardowych oprogramowaniu pośredniczącym pliku Cookie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3f74d-164">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="3f74d-165">Jako rozwiązanie pierścień klucz niezależnie od miejsca należy użyć dostawcy zewnętrznego pierścienia klucz takiego jak:</span><span class="sxs-lookup"><span data-stu-id="3f74d-165">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="3f74d-166">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3f74d-166">Azure Blob Storage</span></span>
* <span data-ttu-id="3f74d-167">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="3f74d-167">Azure Key Vault</span></span>
* <span data-ttu-id="3f74d-168">Magazyn SQL</span><span class="sxs-lookup"><span data-stu-id="3f74d-168">SQL store</span></span>
* <span data-ttu-id="3f74d-169">Pamięć podręczna redis</span><span class="sxs-lookup"><span data-stu-id="3f74d-169">Redis cache</span></span>

<span data-ttu-id="3f74d-170">Aby uzyskać więcej informacji, zobacz [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="3f74d-170">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="3f74d-171">Wdrażanie platformy ASP.NET Core w wersji zapoznawczej w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3f74d-171">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="3f74d-172">Platforma ASP.NET Core w wersji zapoznawczej aplikacji można wdrożyć w usłudze Azure App Service przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="3f74d-172">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="3f74d-173">[Zainstalować rozszerzenie witryny usługi (wersja zapoznawcza)](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="3f74d-173">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="3f74d-174">Używać platformy Docker z funkcją Web Apps for containers</span><span class="sxs-lookup"><span data-stu-id="3f74d-174">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="3f74d-175">Zainstalować rozszerzenie witryny usługi (wersja zapoznawcza)</span><span class="sxs-lookup"><span data-stu-id="3f74d-175">Install the preview site extension</span></span>

<span data-ttu-id="3f74d-176">Jeśli wystąpi problem, za pomocą rozszerzenia witryny (wersja zapoznawcza), otwórz problem w [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="3f74d-176">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="3f74d-177">W witrynie Azure portal przejdź do bloku usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="3f74d-177">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="3f74d-178">Wybierz aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="3f74d-178">Select the web app.</span></span>
1. <span data-ttu-id="3f74d-179">Typ "ex" w polu wyszukiwania lub przewiń w dół na liście zarządzania sekcji, aby **narzędzia PROGRAMISTYCZNE**.</span><span class="sxs-lookup"><span data-stu-id="3f74d-179">Type "ex" in the search box or scroll down the list of management sections to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="3f74d-180">Wybierz **narzędzia PROGRAMISTYCZNE** > **rozszerzenia**.</span><span class="sxs-lookup"><span data-stu-id="3f74d-180">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="3f74d-181">Wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3f74d-181">Select **Add**.</span></span>
1. <span data-ttu-id="3f74d-182">Wybierz **platformy ASP.NET Core &lt;x.y&gt; (x86) środowiska uruchomieniowego** rozszerzenia z listy, gdzie `<x.y>` jest w wersji zapoznawczej platformy ASP.NET Core (na przykład **środowiska uruchomieniowego platformy ASP.NET Core 2.2 (x86)**).</span><span class="sxs-lookup"><span data-stu-id="3f74d-182">Select the **ASP.NET Core &lt;x.y&gt; (x86) Runtime** extension from the list, where `<x.y>` is the ASP.NET Core preview version (for example, **ASP.NET Core 2.2 (x86) Runtime**).</span></span> <span data-ttu-id="3f74d-183">X86 środowiska uruchomieniowego jest odpowiednia dla [wdrożeń zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd) opierają się na spoza procesu hostingu przez modułu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f74d-183">The x86 runtime is appropriate for [framework-dependent deployments](/dotnet/core/deploying/#framework-dependent-deployments-fdd) that rely on out-of-process hosting by the ASP.NET Core Module.</span></span>
1. <span data-ttu-id="3f74d-184">Wybierz **OK** aby zaakceptować warunki prawne.</span><span class="sxs-lookup"><span data-stu-id="3f74d-184">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="3f74d-185">Wybierz **OK** można zainstalować rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="3f74d-185">Select **OK** to install the extension.</span></span>

<span data-ttu-id="3f74d-186">Po zakończeniu tej operacji jest zainstalowana najnowsza wersja zapoznawcza platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f74d-186">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="3f74d-187">Zweryfikuj instalację:</span><span class="sxs-lookup"><span data-stu-id="3f74d-187">Verify the installation:</span></span>

1. <span data-ttu-id="3f74d-188">Wybierz **zaawansowane narzędzia** w obszarze **narzędzia PROGRAMISTYCZNE**.</span><span class="sxs-lookup"><span data-stu-id="3f74d-188">Select **Advanced Tools** under **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="3f74d-189">Wybierz **Przejdź** na **Narzędzia zaawansowane** bloku.</span><span class="sxs-lookup"><span data-stu-id="3f74d-189">Select **Go** on the **Advanced Tools** blade.</span></span>
1. <span data-ttu-id="3f74d-190">Wybierz **konsoli debugowania** > **PowerShell** elementu menu.</span><span class="sxs-lookup"><span data-stu-id="3f74d-190">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="3f74d-191">W wierszu polecenia programu PowerShell uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="3f74d-191">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="3f74d-192">Zastąpiono wersję środowiska uruchomieniowego platformy ASP.NET Core dla `<x.y>` w poleceniu:</span><span class="sxs-lookup"><span data-stu-id="3f74d-192">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   <span data-ttu-id="3f74d-193">Jeśli jest zainstalowany w wersji zapoznawczej środowiska uruchomieniowego dla platformy ASP.NET Core 2.2, polecenie to:</span><span class="sxs-lookup"><span data-stu-id="3f74d-193">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   <span data-ttu-id="3f74d-194">Polecenie zwraca `True` podczas x64 czas wykonywania (wersja zapoznawcza) jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="3f74d-194">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="3f74d-195">Architektura platformy — x86/x64 64 aplikację usługi App Services znajduje się w **ustawienia aplikacji** bloku w obszarze **ustawienia ogólne** warstwy hostowanym w obliczeniowej serii A i lepsze hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3f74d-195">The platform architecture (x86/x64) of an App Services app is set in the **Application Settings** blade under **General Settings** for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="3f74d-196">Jeśli aplikacja jest uruchamiana w trybie w procesie, architektura platformy jest skonfigurowany dla 64-bitowych (x64) modułu ASP.NET Core używa środowiska uruchomieniowego 64-bitowych (wersja zapoznawcza), jeśli jest obecny.</span><span class="sxs-lookup"><span data-stu-id="3f74d-196">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="3f74d-197">Zainstaluj **platformy ASP.NET Core &lt;x.y&gt; (x64) środowiska uruchomieniowego** rozszerzenia (na przykład **środowiska uruchomieniowego platformy ASP.NET Core 2.2 (x64)**).</span><span class="sxs-lookup"><span data-stu-id="3f74d-197">Install the **ASP.NET Core &lt;x.y&gt; (x64) Runtime** extension (for example, **ASP.NET Core 2.2 (x64) Runtime**).</span></span>
>
> <span data-ttu-id="3f74d-198">Po zainstalowaniu x64 podglądu środowiska uruchomieniowego, uruchom następujące polecenie w oknie wiersza polecenia programu PowerShell programu Kudu, aby zweryfikować instalację.</span><span class="sxs-lookup"><span data-stu-id="3f74d-198">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="3f74d-199">Zastąpiono wersję środowiska uruchomieniowego platformy ASP.NET Core dla `<x.y>` w poleceniu:</span><span class="sxs-lookup"><span data-stu-id="3f74d-199">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> <span data-ttu-id="3f74d-200">Jeśli jest zainstalowany w wersji zapoznawczej środowiska uruchomieniowego dla platformy ASP.NET Core 2.2, polecenie to:</span><span class="sxs-lookup"><span data-stu-id="3f74d-200">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> <span data-ttu-id="3f74d-201">Polecenie zwraca `True` podczas x64 czas wykonywania (wersja zapoznawcza) jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="3f74d-201">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="3f74d-202">**ASP.NET Core rozszerzenia** zapewnia dodatkowe funkcje dla platformy ASP.NET Core w usłudze Azure App Services, takich jak włączenie rejestrowania platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="3f74d-202">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="3f74d-203">Rozszerzenie jest instalowana automatycznie podczas wdrażania w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f74d-203">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="3f74d-204">Jeśli rozszerzenie nie jest zainstalowany, zainstaluj go dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3f74d-204">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="3f74d-205">**Rozszerzenie witryny (wersja zapoznawcza) za pomocą szablonu usługi ARM**</span><span class="sxs-lookup"><span data-stu-id="3f74d-205">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="3f74d-206">Jeśli szablon ARM jest używany do tworzenia i wdrażania aplikacji, `siteextensions` typu zasobu może służyć do dodawania rozszerzenia witryny aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="3f74d-206">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="3f74d-207">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3f74d-207">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="3f74d-208">Używać platformy Docker z funkcją Web Apps for containers</span><span class="sxs-lookup"><span data-stu-id="3f74d-208">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="3f74d-209">[Usługi Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) zawiera najnowsze obrazy platformy Docker w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="3f74d-209">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="3f74d-210">Obrazy może służyć jako obrazu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="3f74d-210">The images can be used as a base image.</span></span> <span data-ttu-id="3f74d-211">Przy użyciu obrazu i wdrażanie w usłudze Web Apps for Containers normalnie.</span><span class="sxs-lookup"><span data-stu-id="3f74d-211">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f74d-212">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3f74d-212">Additional resources</span></span>

* [<span data-ttu-id="3f74d-213">Przegląd usługi Web Apps (5-minutowy klip wideo z omówieniem)</span><span class="sxs-lookup"><span data-stu-id="3f74d-213">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="3f74d-214">Usługa Azure App Service: Najlepsze miejsce do hostowania aplikacji .NET (55-minutowy klip wideo z omówieniem)</span><span class="sxs-lookup"><span data-stu-id="3f74d-214">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="3f74d-215">Azure Friday: Diagnostyki usługi aplikacji Azure i środowisko rozwiązywania problemów (12-minutowy klip wideo)</span><span class="sxs-lookup"><span data-stu-id="3f74d-215">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="3f74d-216">Omówienie diagnostyki w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3f74d-216">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="3f74d-217">Usługa Azure App Service w systemie Windows Server [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="3f74d-217">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="3f74d-218">Poniższe tematy odnoszą się do podstawowych technologii usług IIS:</span><span class="sxs-lookup"><span data-stu-id="3f74d-218">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="3f74d-219">Biblioteki Microsoft TechNet: Systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="3f74d-219">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
