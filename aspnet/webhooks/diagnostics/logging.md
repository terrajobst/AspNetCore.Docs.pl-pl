---
uid: webhooks/diagnostics/logging
title: ASP.NET elementów Webhook rejestrowania | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jak przeprowadzić logowanie elementów Webhook ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044828"
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="31f65-103">ASP.NET elementów Webhook rejestrowania</span><span class="sxs-lookup"><span data-stu-id="31f65-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="31f65-104">Microsoft ASP.NET WebHooks używa rejestrowania sposób zgłaszania problemów i problemy.</span><span class="sxs-lookup"><span data-stu-id="31f65-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="31f65-105">Domyślnie dzienniki są zapisywane przy użyciu [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) gdzie mogą być zarządzanych za pomocą [obiektów nasłuchujących śledzenia](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) innego strumienia dziennika, takich jak.</span><span class="sxs-lookup"><span data-stu-id="31f65-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="31f65-106">W przypadku wdrażania aplikacji sieci Web jako aplikacji sieci Web platformy Azure, dzienniki są automatycznie pobierane i mogą być zarządzane wraz z wszystkimi innymi [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="31f65-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="31f65-107">Aby uzyskać więcej informacji, zobacz [Włączanie rejestrowania diagnostyki dla aplikacji sieci web w usłudze aplikacji Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="31f65-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="31f65-108">Dodatkowo dzienniki można uzyskać bezpośrednio w programie Visual Studio, zgodnie z opisem w [Rozwiązywanie problemów aplikacji sieci web w usłudze Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="31f65-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="31f65-109">Przekierowywanie dzienników</span><span class="sxs-lookup"><span data-stu-id="31f65-109">Redirecting Logs</span></span>

<span data-ttu-id="31f65-110">Zamiast zapisywania dzienników [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), istnieje możliwość zapewniać implementację alternatywny rejestrowania, który można rejestrować bezpośrednio do Menedżera dziennika, takich jak [Log4Net](http://logging.apache.org/log4net/) i [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="31f65-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="31f65-111">Wystarczy podać implementacja [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) i zarejestrowanie go za pomocą aparatu iniekcji zależności, wybranym i jej będzie pobrać pobrana przez Microsoft ASP.NET WebHooks.</span><span class="sxs-lookup"><span data-stu-id="31f65-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="31f65-112">Zobacz [iniekcji zależności w ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="31f65-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
