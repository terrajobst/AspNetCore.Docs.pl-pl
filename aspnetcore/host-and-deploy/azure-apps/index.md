---
title: Host platformy ASP.NET Core w usłudze aplikacji Azure
author: guardrex
description: Dowiedzieć się, jak udostępniać aplikacje platformy ASP.NET Core w usłudze Azure App Service z łączami do przydatnych zasobów.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="db8c6-103">Host platformy ASP.NET Core w usłudze aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="db8c6-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="db8c6-104">[Usługa aplikacji Azure](https://azure.microsoft.com/services/app-service/) jest [przetwarzania platformy usług w chmurze firmy Microsoft](https://azure.microsoft.com/) do obsługi aplikacji sieci web, łącznie z platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db8c6-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="db8c6-105">Przydatne zasoby</span><span class="sxs-lookup"><span data-stu-id="db8c6-105">Useful resources</span></span>

<span data-ttu-id="db8c6-106">Azure [dokumentacji aplikacji sieci Web](/azure/app-service/) jest stroną główną dla dokumentacja, samouczki, przykłady, przewodników instruktażowych dot i innych zasobów aplikacji Azure.</span><span class="sxs-lookup"><span data-stu-id="db8c6-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="db8c6-107">Są dwa samouczki zauważalne, które odnoszą się do obsługi aplikacji platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="db8c6-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="db8c6-108">Szybki Start: Tworzenie aplikacji sieci web platformy ASP.NET Core na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="db8c6-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="db8c6-109">Tworzenie i wdrażanie aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service w systemie Windows za pomocą programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db8c6-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="db8c6-110">Szybki Start: Tworzenie aplikacji sieci web platformy .NET Core w usłudze App Service w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="db8c6-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="db8c6-111">Użyj wiersza polecenia do tworzenia i wdrażania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service w systemie Linux.</span><span class="sxs-lookup"><span data-stu-id="db8c6-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="db8c6-112">Są dostępne w dokumentacji platformy ASP.NET Core następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="db8c6-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="db8c6-113">Publikowanie na platformie Azure przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="db8c6-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="db8c6-114">Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db8c6-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="db8c6-115">Publikowanie na platformie Azure przy użyciu narzędzi interfejsu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="db8c6-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="db8c6-116">Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service przy użyciu klienta wiersza polecenia Git.</span><span class="sxs-lookup"><span data-stu-id="db8c6-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="db8c6-117">Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i rozwiązania Git</span><span class="sxs-lookup"><span data-stu-id="db8c6-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="db8c6-118">Informacje o sposobie tworzenia aplikacji sieci web platformy ASP.NET Core za pomocą programu Visual Studio i wdrożyć ją w usłudze Azure App Service przy użyciu usługi Git do ciągłego wdrażania.</span><span class="sxs-lookup"><span data-stu-id="db8c6-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="db8c6-119">Ciągłe wdrażanie na platformie Azure za pomocą usług VSTS</span><span class="sxs-lookup"><span data-stu-id="db8c6-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="db8c6-120">Konfigurowanie kompilacji elementu konfiguracji dla aplikacji platformy ASP.NET Core, a następnie utworzyć wersję ciągłe wdrażanie w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="db8c6-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="db8c6-121">Piaskownica aplikacji sieci Web platformy Azure</span><span class="sxs-lookup"><span data-stu-id="db8c6-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="db8c6-122">Wykryj ograniczenia wykonywania środowiska uruchomieniowego usługi Azure App Service wymuszane przez platformę aplikacji Azure.</span><span class="sxs-lookup"><span data-stu-id="db8c6-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="db8c6-123">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="db8c6-123">Application configuration</span></span>

<span data-ttu-id="db8c6-124">Z platformy ASP.NET Core 2.0 lub nowszego oraz trzy pakiety w programie [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) udostępnia funkcje automatycznego rejestrowania w przypadku aplikacji wdrożonych w usłudze Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="db8c6-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="db8c6-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) używa [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) do zapewniania platformy ASP.NET Core lightup integracji z usługi aplikacji Azure.</span><span class="sxs-lookup"><span data-stu-id="db8c6-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="db8c6-126">Funkcje rejestrowania dodano są dostarczane przez `Microsoft.AspNetCore.AzureAppServicesIntegration` pakietu.</span><span class="sxs-lookup"><span data-stu-id="db8c6-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="db8c6-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) wykonuje [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) dodawać dostawców rejestrowania diagnostyki Azure App Service w `Microsoft.Extensions.Logging.AzureAppServices` pakietu.</span><span class="sxs-lookup"><span data-stu-id="db8c6-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="db8c6-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) zawiera implementacje rejestratora do obsługi dzienników diagnostyki Azure App Service i przesyłania strumieniowego funkcje dziennika.</span><span class="sxs-lookup"><span data-stu-id="db8c6-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="db8c6-129">Serwer proxy i scenariuszy usługi równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="db8c6-129">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="db8c6-130">IIS integracji oprogramowania pośredniczącego, który konfiguruje przekazywane oprogramowanie pośredniczące nagłówki i moduł platformy ASP.NET Core są skonfigurowane do przekazywania schemat (HTTP/HTTPS) oraz adres IP zdalnego, którego pochodzi żądanie.</span><span class="sxs-lookup"><span data-stu-id="db8c6-130">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="db8c6-131">Dodatkowa konfiguracja może być wymagane dla aplikacji hostowanych serwerów proxy dodatkowe i moduły równoważenia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="db8c6-131">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="db8c6-132">Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="db8c6-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="db8c6-133">Monitorowanie i rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="db8c6-133">Monitoring and logging</span></span>

<span data-ttu-id="db8c6-134">Monitorowanie, rejestrowanie i informacje dotyczące rozwiązywania problemów zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="db8c6-134">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="db8c6-135">Porady: monitorować aplikacje w usłudze aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="db8c6-135">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="db8c6-136">Dowiedz się, jak Przejrzyj przydziałów i metryki dla aplikacji i planów usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8c6-136">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="db8c6-137">Włączanie rejestrowania diagnostyki dla aplikacji sieci web w usłudze aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="db8c6-137">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="db8c6-138">Dowiedzieć się, jak włączyć i dostępu do diagnostyki rejestrowanie aktywności serwera sieci web, nieudanych żądań i kodów stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="db8c6-138">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="db8c6-139">Wprowadzenie do obsługi błędów w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db8c6-139">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="db8c6-140">Dowiedz się, typowe appoaches do obsługi błędów w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db8c6-140">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="db8c6-141">Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="db8c6-141">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="db8c6-142">Dowiedz się, jak diagnozować problemy z wdrożenia usługi Azure App Service przy użyciu aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db8c6-142">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="db8c6-143">Typowe błędy odwołania dla usługi Azure App Service i IIS z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db8c6-143">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="db8c6-144">Zobacz typowych błędów konfiguracji wdrożenia dla aplikacji hostowanych przez Azure App Service/IIS z porady dotyczące rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="db8c6-144">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="db8c6-145">Pierścień klucza ochrony danych i miejsca wdrożenia</span><span class="sxs-lookup"><span data-stu-id="db8c6-145">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="db8c6-146">[Klucze ochrony danych](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) zostały utrwalone *%HOME%\ASP.NET\DataProtection-Keys* folderu.</span><span class="sxs-lookup"><span data-stu-id="db8c6-146">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="db8c6-147">Ten folder nie jest obsługiwana przez sieć magazynu i jest synchronizowane na wszystkich komputerach hosting aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8c6-147">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="db8c6-148">Klucze chronione nie są w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="db8c6-148">Keys aren't protected at rest.</span></span> <span data-ttu-id="db8c6-149">Ten folder zawiera pierścień klucza do wszystkich wystąpień aplikacji w miejscu pojedynczego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="db8c6-149">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="db8c6-150">Miejsc oddzielne wdrożenia, takich jak przemieszczania i produkcji, nie należy współużytkować pierścień klucza.</span><span class="sxs-lookup"><span data-stu-id="db8c6-150">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="db8c6-151">Podczas wymiany między miejscami wdrożenia, każdy system przy użyciu ochrony danych będzie niemożliwe do odszyfrowywania danych przechowywanych w poprzednim miejsca przy użyciu pierścień klucza.</span><span class="sxs-lookup"><span data-stu-id="db8c6-151">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="db8c6-152">Oprogramowanie pośredniczące plików Cookie ASP.NET używa ochrony danych do ochrony jego plików cookie.</span><span class="sxs-lookup"><span data-stu-id="db8c6-152">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="db8c6-153">Prowadzi to do użytkowników podpisywany z aplikacji, która używa standardowych oprogramowaniu pośredniczącym pliku Cookie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="db8c6-153">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="db8c6-154">Jako rozwiązanie pierścień klucza niezależny od miejsca używa dostawcy zewnętrznego pierścień klucza, takich jak:</span><span class="sxs-lookup"><span data-stu-id="db8c6-154">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="db8c6-155">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="db8c6-155">Azure Blob Storage</span></span>
* <span data-ttu-id="db8c6-156">Usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="db8c6-156">Azure Key Vault</span></span>
* <span data-ttu-id="db8c6-157">Magazynu SQL</span><span class="sxs-lookup"><span data-stu-id="db8c6-157">SQL store</span></span>
* <span data-ttu-id="db8c6-158">Pamięci podręcznej redis</span><span class="sxs-lookup"><span data-stu-id="db8c6-158">Redis cache</span></span>

<span data-ttu-id="db8c6-159">Aby uzyskać więcej informacji, zobacz [dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="db8c6-159">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="db8c6-160">Wdrażanie wersji zapoznawczej platformy ASP.NET Core w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="db8c6-160">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="db8c6-161">Platformy ASP.NET Core Podgląd aplikacje można wdrożyć w usłudze Azure App Service z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="db8c6-161">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* [<span data-ttu-id="db8c6-162">Zainstaluj rozszerzenie lokacji wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="db8c6-162">Install the preview site extention</span></span>](#site-x)
* [<span data-ttu-id="db8c6-163">Wdróż aplikację samodzielnie zawarte</span><span class="sxs-lookup"><span data-stu-id="db8c6-163">Deploy the app self contained</span></span>](#self)
* [<span data-ttu-id="db8c6-164">Użyj Docker z aplikacjami sieci Web dla kontenerów</span><span class="sxs-lookup"><span data-stu-id="db8c6-164">Use Docker with Web Apps for containers</span></span>](#docker)

<span data-ttu-id="db8c6-165">Jeśli masz problem przy użyciu rozszerzenia lokacji wersji zapoznawczej, otwórz problemu na [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="db8c6-165">If you have a problem using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a><span data-ttu-id="db8c6-166">Zainstaluj rozszerzenie lokacji wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="db8c6-166">Install the preview site extention</span></span>

* <span data-ttu-id="db8c6-167">W portalu Azure przejdź do bloku usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8c6-167">From the Azure portal, navigate to the App Service blade.</span></span>
* <span data-ttu-id="db8c6-168">Wprowadź "ex" w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="db8c6-168">Enter "ex" in the search box.</span></span>
* <span data-ttu-id="db8c6-169">Wybierz **rozszerzenia**.</span><span class="sxs-lookup"><span data-stu-id="db8c6-169">Select **Extensions**.</span></span>
* <span data-ttu-id="db8c6-170">Wybierz opcję "Dodaj".</span><span class="sxs-lookup"><span data-stu-id="db8c6-170">Select "Add".</span></span>

![Azure bloku aplikacji z poprzednich krokach](index/_static/x1.png)

* <span data-ttu-id="db8c6-172">Wybierz **rozszerzenia środowiska uruchomieniowego platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="db8c6-172">Select **ASP.NET Core Runtime Extensions**.</span></span>
* <span data-ttu-id="db8c6-173">Wybierz **OK** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="db8c6-173">Select **OK** > **OK**.</span></span>

<span data-ttu-id="db8c6-174">Po ukończeniu operacji dodawania zainstalowano najnowszej wersji zapoznawczej .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="db8c6-174">When the add operations completes, the latest .NET Core 2.1 preview is installed.</span></span> <span data-ttu-id="db8c6-175">Instalację można zweryfikować, uruchamiając `dotnet --info` w konsoli.</span><span class="sxs-lookup"><span data-stu-id="db8c6-175">You can verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="db8c6-176">W bloku usługi aplikacji:</span><span class="sxs-lookup"><span data-stu-id="db8c6-176">From the App Service blade:</span></span>

* <span data-ttu-id="db8c6-177">Wprowadź "con", w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="db8c6-177">Enter "con" in the search box.</span></span>
* <span data-ttu-id="db8c6-178">Wybierz **konsoli**.</span><span class="sxs-lookup"><span data-stu-id="db8c6-178">Select **Console**.</span></span>
* <span data-ttu-id="db8c6-179">Wprowadź `dotnet --info` w konsoli.</span><span class="sxs-lookup"><span data-stu-id="db8c6-179">Enter `dotnet --info` in the console.</span></span>

![Azure bloku aplikacji z poprzednich krokach](index/_static/cons.png)

<span data-ttu-id="db8c6-181">Obraz poprzedniego były aktualne w momencie to zostało zapisane.</span><span class="sxs-lookup"><span data-stu-id="db8c6-181">The preceding image was current at the time this was written.</span></span> <span data-ttu-id="db8c6-182">Może pojawić się innej wersji.</span><span class="sxs-lookup"><span data-stu-id="db8c6-182">You may see a different version.</span></span>

<span data-ttu-id="db8c6-183">`dotnet --info` Wyświetla ścieżkę do rozszerzenia lokacji, w których została zainstalowana wersja zapoznawcza.</span><span class="sxs-lookup"><span data-stu-id="db8c6-183">The `dotnet --info` displays the the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="db8c6-184">Pokazuje, aplikacja zostanie uruchomiona z rozszerzenia lokacji zamiast z domyślnej *ProgramFiles* lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="db8c6-184">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="db8c6-185">Jeśli widzisz *ProgramFiles*, należy ponownie uruchomić witrynę i uruchomić `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="db8c6-185">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a><span data-ttu-id="db8c6-186">Rozszerzenie lokacji wersji zapoznawczej za pomocą szablonu usługi ARM</span><span class="sxs-lookup"><span data-stu-id="db8c6-186">Use the preview site extention with an ARM template</span></span>

<span data-ttu-id="db8c6-187">Jeśli używasz szablonu usługi ARM do tworzenia i wdrażania aplikacji można użyć `siteextensions` typu zasobów, aby dodać do lokacji rozszerzenie aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="db8c6-187">If you are using an ARM template to create and deploy applications you can use the `siteextensions` resource type to add the site extension to a Web App.</span></span> <span data-ttu-id="db8c6-188">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="db8c6-188">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="db8c6-189">Wdróż aplikację samodzielnie zawarte</span><span class="sxs-lookup"><span data-stu-id="db8c6-189">Deploy the app self contained</span></span>

<span data-ttu-id="db8c6-190">Można wdrożyć [niezależne aplikacji](/dotnet/core/deploying/#self-contained-deployments-scd) która prowadzi środowiska wykonawczego w wersji zapoznawczej wraz z nim podczas wdrażania.</span><span class="sxs-lookup"><span data-stu-id="db8c6-190">You can deploy a [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) that carries the preview runtime with it when being deployed.</span></span> <span data-ttu-id="db8c6-191">W przypadku wdrażania aplikacji samodzielną:</span><span class="sxs-lookup"><span data-stu-id="db8c6-191">When deploying a self contained app:</span></span>

* <span data-ttu-id="db8c6-192">Nie trzeba przygotować witryny.</span><span class="sxs-lookup"><span data-stu-id="db8c6-192">You don’t need to prepare your site.</span></span>
* <span data-ttu-id="db8c6-193">Należy opublikować aplikację inaczej niż w przypadku wdrażania aplikacji, gdy zestaw SDK jest zainstalowany na serwerze.</span><span class="sxs-lookup"><span data-stu-id="db8c6-193">Requires you to publish your application differently than you would when deploying an app once the SDK is installed on the server.</span></span>

<span data-ttu-id="db8c6-194">Samodzielne aplikacje są opcję dla wszystkich aplikacji .NET Core.</span><span class="sxs-lookup"><span data-stu-id="db8c6-194">Self-contained apps are an option for all .NET Core applications.</span></span>

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="db8c6-195">Użyj Docker z aplikacjami sieci Web dla kontenerów</span><span class="sxs-lookup"><span data-stu-id="db8c6-195">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="db8c6-196">[Centrum Docker](https://hub.docker.com/r/microsoft/aspnetcore/) zawiera najnowsze obrazy Docker 2.1 podglądu.</span><span class="sxs-lookup"><span data-stu-id="db8c6-196">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest 2.1 preview Docker images.</span></span> <span data-ttu-id="db8c6-197">Można ich używać jako obrazu podstawowego i wdrażania aplikacji sieci Web dla kontenerów w zwykły sposób.</span><span class="sxs-lookup"><span data-stu-id="db8c6-197">You can use them as your base image and deploy to Web Apps for Containers as you normally would.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db8c6-198">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="db8c6-198">Additional resources</span></span>

* [<span data-ttu-id="db8c6-199">Przegląd usługi Web Apps (5-minutowy klip wideo z omówieniem)</span><span class="sxs-lookup"><span data-stu-id="db8c6-199">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="db8c6-200">Usługa aplikacji Azure: Najlepiej umieścić na hoście aplikacji .NET (55-minutowy klip wideo z omówieniem)</span><span class="sxs-lookup"><span data-stu-id="db8c6-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="db8c6-201">Piątek z Azure: Diagnostycznych usługi aplikacji Azure i rozwiązywanie problemów z doświadczenia (12-minutowy film wideo)</span><span class="sxs-lookup"><span data-stu-id="db8c6-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="db8c6-202">Omówienie diagnostyki usługi aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="db8c6-202">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="db8c6-203">Usługa aplikacji Azure w systemie Windows Server używa [Internet Information Services (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="db8c6-203">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="db8c6-204">Poniższe tematy odnoszą się do podstawowych technologii usług IIS:</span><span class="sxs-lookup"><span data-stu-id="db8c6-204">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="db8c6-205">Host platformy ASP.NET Core w systemie Windows z programem IIS</span><span class="sxs-lookup"><span data-stu-id="db8c6-205">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="db8c6-206">Wprowadzenie do platformy ASP.NET Core modułu</span><span class="sxs-lookup"><span data-stu-id="db8c6-206">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="db8c6-207">Odwołania do konfiguracji modułu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db8c6-207">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="db8c6-208">Moduły usług IIS z platformą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db8c6-208">IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="db8c6-209">Bibliotece Microsoft TechNet: Serwer systemu Windows</span><span class="sxs-lookup"><span data-stu-id="db8c6-209">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
