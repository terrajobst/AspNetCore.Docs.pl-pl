---
title: Host platformy ASP.NET Core w usłudze Azure App Service
author: guardrex
description: Dowiedz się, jak hostować aplikacje platformy ASP.NET Core w usłudze Azure App Service wraz z łączami do przydatnych zasobów.
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 9a7d20378cac597b748d8a60eb0f0bf17c9ba082
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/20/2018
ms.locfileid: "41756217"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="5d386-103">Host platformy ASP.NET Core w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5d386-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="5d386-104">[Usługa Azure App Service](https://azure.microsoft.com/services/app-service/) jest [chmury obliczeniowej platformy usługi firmy Microsoft](https://azure.microsoft.com/) do hostowania aplikacji sieci web, łącznie z platformą ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d386-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="5d386-105">Przydatne zasoby</span><span class="sxs-lookup"><span data-stu-id="5d386-105">Useful resources</span></span>

<span data-ttu-id="5d386-106">Azure [dokumentację usługi Web Apps](/azure/app-service/) to miejsce, dokumentacja, samouczki, przykłady, przewodniki z instrukcjami i inne zasoby aplikacji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="5d386-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="5d386-107">Są dwa istotne samouczków, które odnoszą się do hostowania aplikacji platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5d386-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="5d386-108">Szybki Start: Tworzenie aplikacji internetowej platformy ASP.NET Core na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="5d386-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="5d386-109">Visual Studio umożliwia tworzenie i wdrażanie aplikacji sieci web ASP.NET Core w usłudze Azure App Service w Windows.</span><span class="sxs-lookup"><span data-stu-id="5d386-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="5d386-110">Szybki Start: Tworzenie aplikacji sieci web platformy .NET Core w usłudze App Service w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="5d386-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="5d386-111">Tworzenie i wdrażanie aplikacji sieci web ASP.NET Core w usłudze Azure App Service w systemie Linux, należy użyć wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="5d386-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="5d386-112">Następujące artykuły są dostępne w dokumentacji platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5d386-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="5d386-113">Publikowanie na platformie Azure przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5d386-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="5d386-114">Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5d386-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="5d386-115">Publikowanie na platformie Azure przy użyciu narzędzi interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="5d386-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="5d386-116">Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service za pomocą klienta wiersza polecenia usługi Git.</span><span class="sxs-lookup"><span data-stu-id="5d386-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="5d386-117">Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i rozwiązania Git</span><span class="sxs-lookup"><span data-stu-id="5d386-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="5d386-118">Dowiedz się, jak utworzyć aplikację internetową platformy ASP.NET Core przy użyciu programu Visual Studio, a następnie wdrożyć ją w usłudze Azure App Service przy użyciu narzędzia Git do ciągłego wdrażania.</span><span class="sxs-lookup"><span data-stu-id="5d386-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="5d386-119">Ciągłe wdrażanie na platformie Azure za pomocą usług VSTS</span><span class="sxs-lookup"><span data-stu-id="5d386-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="5d386-120">Skonfiguruj kompilację o ciągłej integracji dla aplikacji ASP.NET Core, a następnie utworzyć wydanie ciągłe wdrażanie w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5d386-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="5d386-121">Usługa Azure piaskownica aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="5d386-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="5d386-122">Dowiedz się, wykonywanie ograniczenia środowiska uruchomieniowego usługi Azure App Service, wymuszane przez platformę Azure Apps.</span><span class="sxs-lookup"><span data-stu-id="5d386-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="5d386-123">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="5d386-123">Application configuration</span></span>

<span data-ttu-id="5d386-124">W programie ASP.NET Core 2.0 lub nowszej następujące pakiety NuGet zapewniają funkcji automatycznego rejestrowania w przypadku aplikacji wdrożonych w usłudze Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="5d386-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="5d386-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) używa [interfejsu IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) nad umożliwieniem integracji światła w górę platformy ASP.NET Core w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5d386-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="5d386-126">Funkcje rejestrowania dodano są dostarczane przez `Microsoft.AspNetCore.AzureAppServicesIntegration` pakietu.</span><span class="sxs-lookup"><span data-stu-id="5d386-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="5d386-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) wykonuje [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) dodać dostawców rejestrowania diagnostycznego usługi Azure App Service w `Microsoft.Extensions.Logging.AzureAppServices` pakietu.</span><span class="sxs-lookup"><span data-stu-id="5d386-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="5d386-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) zawiera implementacje rejestratora do obsługi dzienników diagnostycznych usługi Azure App Service i przesyłanie strumieniowe funkcji dzienników.</span><span class="sxs-lookup"><span data-stu-id="5d386-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="5d386-129">Jeśli przeznaczonych dla platformy .NET Core i odwoływanie się do [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage), pakiety są już uwzględniane.</span><span class="sxs-lookup"><span data-stu-id="5d386-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="5d386-130">Pakiety nie są spełnione z nowszego [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="5d386-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="5d386-131">Jeśli przeznaczonych dla platformy .NET Framework lub odwołuje się do `Microsoft.AspNetCore.App` meta Microsoft.aspnetcore.all, odwołania się do pakietów indywidualne rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="5d386-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="5d386-132">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="5d386-132">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="5d386-133">Usługi IIS oprogramowania pośredniczącego integracji, który konfiguruje przekazany oprogramowania pośredniczącego nagłówków i modułu ASP.NET Core są skonfigurowane do przekazywania schemat (HTTP/HTTPS) i zdalny adres IP, skąd pochodzi żądanie.</span><span class="sxs-lookup"><span data-stu-id="5d386-133">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="5d386-134">Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych za serwery proxy dodatkowe i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="5d386-134">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="5d386-135">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core pracować z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="5d386-135">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="5d386-136">Monitorowanie i rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="5d386-136">Monitoring and logging</span></span>

<span data-ttu-id="5d386-137">Monitorowanie, rejestrowanie i informacje dotyczące rozwiązywania problemów zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="5d386-137">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="5d386-138">Porady: monitorowanie aplikacji w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5d386-138">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="5d386-139">Dowiedz się, jak przeglądać limity przydziału i metryki, aby aplikacje i plany usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="5d386-139">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="5d386-140">Włączanie rejestrowania diagnostycznego dla aplikacji sieci web w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5d386-140">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="5d386-141">Dowiedz się, jak włączyć i dostęp do rejestrowania diagnostycznego dla kodów stanu HTTP, żądań zakończonych niepowodzeniem i aktywności serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="5d386-141">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="5d386-142">Wprowadzenie do obsługi błędów w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5d386-142">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="5d386-143">Poznaj typowe metody obsługi błędów w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d386-143">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="5d386-144">Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5d386-144">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="5d386-145">Dowiedz się, jak diagnozować problemy z wdrożeniami w usłudze Azure App Service za pomocą aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d386-145">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="5d386-146">Dokumentacja typowych błędów dla usługi Azure App Service i IIS za pomocą programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5d386-146">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="5d386-147">Zobacz typowych błędów konfiguracji wdrażania dla aplikacji hostowanych przez Azure App Service/IIS za pomocą poradę dotyczącą rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="5d386-147">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="5d386-148">Pierścień klucz ochrony danych i miejsca wdrożenia</span><span class="sxs-lookup"><span data-stu-id="5d386-148">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="5d386-149">[Klucze ochrony danych](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) zostaną utrwalone w *%HOME%\ASP.NET\DataProtection-Keys* folderu.</span><span class="sxs-lookup"><span data-stu-id="5d386-149">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="5d386-150">Ten folder jest wspierana przez sieć, Magazyn, które jest synchronizowane na wszystkich maszynach hostingu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5d386-150">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="5d386-151">Klucze nie są chronione w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="5d386-151">Keys aren't protected at rest.</span></span> <span data-ttu-id="5d386-152">Ten folder zawiera pierścień klucza do wszystkich wystąpień aplikacji w gnieździe pojedynczego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="5d386-152">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="5d386-153">Gniazda wdrażane pojedynczo, takich jak przejściowe i produkcyjne, nie udostępniaj klucza pierścień.</span><span class="sxs-lookup"><span data-stu-id="5d386-153">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="5d386-154">Po zamianie między miejscami wdrożenia, każdy system przy użyciu ochrony danych będzie możliwe do odszyfrowywania danych przechowywanych w poprzednim miejsca przy użyciu pierścień klucza.</span><span class="sxs-lookup"><span data-stu-id="5d386-154">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="5d386-155">Oprogramowaniu pośredniczącym pliku Cookie ASP.NET używa ochrony danych, aby chronić swoje pliki cookie.</span><span class="sxs-lookup"><span data-stu-id="5d386-155">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="5d386-156">Prowadzi to do użytkowników, trwa wylogowanie z aplikacji, która używa standardowych oprogramowaniu pośredniczącym pliku Cookie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5d386-156">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="5d386-157">Jako rozwiązanie pierścień klucz niezależnie od miejsca należy użyć dostawcy zewnętrznego pierścienia klucz takiego jak:</span><span class="sxs-lookup"><span data-stu-id="5d386-157">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="5d386-158">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="5d386-158">Azure Blob Storage</span></span>
* <span data-ttu-id="5d386-159">Usługa Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5d386-159">Azure Key Vault</span></span>
* <span data-ttu-id="5d386-160">Magazyn SQL</span><span class="sxs-lookup"><span data-stu-id="5d386-160">SQL store</span></span>
* <span data-ttu-id="5d386-161">Pamięć podręczna redis</span><span class="sxs-lookup"><span data-stu-id="5d386-161">Redis cache</span></span>

<span data-ttu-id="5d386-162">Aby uzyskać więcej informacji, zobacz [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="5d386-162">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="5d386-163">Wdrażanie platformy ASP.NET Core w wersji zapoznawczej w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5d386-163">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="5d386-164">Platforma ASP.NET Core w wersji zapoznawczej aplikacji można wdrożyć w usłudze Azure App Service przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="5d386-164">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="5d386-165">[Zainstalować rozszerzenie witryny usługi (wersja zapoznawcza)](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="5d386-165">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="5d386-166">Używać platformy Docker z funkcją Web Apps for containers</span><span class="sxs-lookup"><span data-stu-id="5d386-166">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

<span data-ttu-id="5d386-167">Jeśli wystąpi problem, za pomocą rozszerzenia witryny (wersja zapoznawcza), otwórz problem w [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="5d386-167">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="5d386-168">Zainstalować rozszerzenie witryny usługi (wersja zapoznawcza)</span><span class="sxs-lookup"><span data-stu-id="5d386-168">Install the preview site extension</span></span>

1. <span data-ttu-id="5d386-169">W witrynie Azure portal przejdź do bloku usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="5d386-169">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="5d386-170">Wybierz aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="5d386-170">Select the web app.</span></span>
1. <span data-ttu-id="5d386-171">W polu wyszukiwania wprowadź "ex" lub przewiń w dół listy okienka zarządzania **narzędzia PROGRAMISTYCZNE**.</span><span class="sxs-lookup"><span data-stu-id="5d386-171">Enter "ex" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="5d386-172">Wybierz **narzędzia PROGRAMISTYCZNE** > **rozszerzenia**.</span><span class="sxs-lookup"><span data-stu-id="5d386-172">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="5d386-173">Wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5d386-173">Select **Add**.</span></span>

   ![Usługa Azure blok aplikacji z poprzednich kroków](index/_static/x1.png)

1. <span data-ttu-id="5d386-175">Wybierz **rozszerzenia platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="5d386-175">Select **ASP.NET Core Extensions**.</span></span>
1. <span data-ttu-id="5d386-176">Wybierz **OK** aby zaakceptować warunki prawne.</span><span class="sxs-lookup"><span data-stu-id="5d386-176">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="5d386-177">Wybierz **OK** można zainstalować rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="5d386-177">Select **OK** to install the extension.</span></span>

<span data-ttu-id="5d386-178">Po ukończeniu operacji dodawania, jest zainstalowana najnowsza wersja zapoznawcza platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d386-178">When the add operations complete, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="5d386-179">Weryfikowanie instalacji uruchamiając `dotnet --info` w konsoli.</span><span class="sxs-lookup"><span data-stu-id="5d386-179">Verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="5d386-180">Z **usługi App Service** bloku:</span><span class="sxs-lookup"><span data-stu-id="5d386-180">From the **App Service** blade:</span></span>

1. <span data-ttu-id="5d386-181">Wprowadź "con" w polu wyszukiwania lub przewiń w dół na liście zarządzania okienka do **narzędzia PROGRAMISTYCZNE**.</span><span class="sxs-lookup"><span data-stu-id="5d386-181">Enter "con" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="5d386-182">Wybierz **narzędzia PROGRAMISTYCZNE** > **konsoli**.</span><span class="sxs-lookup"><span data-stu-id="5d386-182">Select **DEVELOPMENT TOOLS** > **Console**.</span></span>
1. <span data-ttu-id="5d386-183">Wprowadź `dotnet --info` w konsoli.</span><span class="sxs-lookup"><span data-stu-id="5d386-183">Enter `dotnet --info` in the console.</span></span>

<span data-ttu-id="5d386-184">Jeśli wersja `2.1.300-preview1-008174` jest najnowsza wersja zapoznawcza, uzyskuje się następujące dane wyjściowe, uruchamiając `dotnet --info` w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="5d386-184">If version `2.1.300-preview1-008174` is the latest preview release, the following output is obtained by running `dotnet --info` at the command prompt:</span></span>

![Usługa Azure blok aplikacji z poprzednich kroków](index/_static/cons.png)

<span data-ttu-id="5d386-186">Wersja platformy ASP.NET Core na poprzednim obrazie `2.1.300-preview1-008174`, znajduje się przykład.</span><span class="sxs-lookup"><span data-stu-id="5d386-186">The version of ASP.NET Core shown in the preceding image, `2.1.300-preview1-008174`, is an example.</span></span> <span data-ttu-id="5d386-187">Od najnowszej wersji wstępnej programu ASP.NET Core w momencie skonfigurowano rozszerzenie witryny pojawia się podczas wykonywania `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="5d386-187">The latest preview version of ASP.NET Core at the time the site extension is configured appears when you execute `dotnet --info`.</span></span>

<span data-ttu-id="5d386-188">`dotnet --info` Wyświetla ścieżkę do rozszerzenia witryny, w którym zainstalowano wersję zapoznawczą.</span><span class="sxs-lookup"><span data-stu-id="5d386-188">The `dotnet --info` displays the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="5d386-189">Pokazuje, aplikacja zostanie uruchomiona z poziomu rozszerzenia lokacji zamiast z domyślnej *ProgramFiles* lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="5d386-189">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="5d386-190">Jeśli widzisz *ProgramFiles*, uruchom ponownie lokację i uruchom `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="5d386-190">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

<span data-ttu-id="5d386-191">**Rozszerzenie witryny (wersja zapoznawcza) za pomocą szablonu usługi ARM**</span><span class="sxs-lookup"><span data-stu-id="5d386-191">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="5d386-192">Jeśli szablon ARM jest używany do tworzenia i wdrażania aplikacji, `siteextensions` typu zasobu może służyć do dodawania rozszerzenia witryny aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="5d386-192">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="5d386-193">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5d386-193">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="5d386-194">Używać platformy Docker z funkcją Web Apps for containers</span><span class="sxs-lookup"><span data-stu-id="5d386-194">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="5d386-195">[Usługi Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) zawiera najnowsze obrazy platformy Docker w wersji zapoznawczej.</span><span class="sxs-lookup"><span data-stu-id="5d386-195">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="5d386-196">Obrazy może służyć jako obrazu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="5d386-196">The images can be used as a base image.</span></span> <span data-ttu-id="5d386-197">Przy użyciu obrazu i wdrażanie w usłudze Web Apps for Containers normalnie.</span><span class="sxs-lookup"><span data-stu-id="5d386-197">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5d386-198">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5d386-198">Additional resources</span></span>

* [<span data-ttu-id="5d386-199">Przegląd usługi Web Apps (5-minutowy klip wideo z omówieniem)</span><span class="sxs-lookup"><span data-stu-id="5d386-199">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="5d386-200">Usługa Azure App Service: Najlepsze miejsce do hostowania aplikacji .NET (55-minutowy klip wideo z omówieniem)</span><span class="sxs-lookup"><span data-stu-id="5d386-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="5d386-201">Azure Friday: Diagnostyki usługi aplikacji Azure i środowisko rozwiązywania problemów (12-minutowy klip wideo)</span><span class="sxs-lookup"><span data-stu-id="5d386-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="5d386-202">Omówienie diagnostyki w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5d386-202">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="5d386-203">Usługa Azure App Service w systemie Windows Server [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="5d386-203">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="5d386-204">Poniższe tematy odnoszą się do podstawowych technologii usług IIS:</span><span class="sxs-lookup"><span data-stu-id="5d386-204">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="5d386-205">Biblioteki Microsoft TechNet: Systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="5d386-205">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
