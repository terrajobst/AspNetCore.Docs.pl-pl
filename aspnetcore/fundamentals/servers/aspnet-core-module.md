---
title: Moduł ASP.NET Core
author: guardrex
description: Dowiedz się, jak modułu ASP.NET Core zezwala na serwerze sieci web Kestrel do używania usług IIS lub IIS Express jako serwera zwrotny serwer proxy.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 39c1b364f9dab635c79e00561d212c858c0c4395
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191259"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="d4775-103">Moduł ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4775-103">ASP.NET Core Module</span></span>

<span data-ttu-id="d4775-104">Przez [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), i [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="d4775-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d4775-105">Modułu ASP.NET Core umożliwia platformy ASP.NET Core w aplikacji do uruchamiania w proces roboczy usług IIS (*w trakcie*) lub za usług IIS w konfiguracji zwrotny serwer proxy (*spoza procesu*).</span><span class="sxs-lookup"><span data-stu-id="d4775-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="d4775-106">Program IIS zawiera aplikacji web zaawansowane funkcje zabezpieczeń i możliwości zarządzania.</span><span class="sxs-lookup"><span data-stu-id="d4775-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d4775-107">Modułu ASP.NET Core umożliwia platformy ASP.NET Core uruchamiania za usług IIS w konfiguracji zwrotny serwer proxy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d4775-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="d4775-108">Program IIS zawiera aplikacji web zaawansowane funkcje zabezpieczeń i możliwości zarządzania.</span><span class="sxs-lookup"><span data-stu-id="d4775-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="d4775-109">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="d4775-109">Supported Windows versions:</span></span>

* <span data-ttu-id="d4775-110">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="d4775-110">Windows 7 or later</span></span>
* <span data-ttu-id="d4775-111">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="d4775-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d4775-112">W przypadku hostowania w procesie, moduł ma własną implementację serwera `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="d4775-112">When hosting in-process, the module has its own server implementation, `IISHttpServer`.</span></span>

<span data-ttu-id="d4775-113">Gdy w hostingu poza procesem, moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4775-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="d4775-114">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej noszącą nazwę [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="d4775-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d4775-115">Moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4775-115">The module only works with Kestrel.</span></span> <span data-ttu-id="d4775-116">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej noszącą nazwę [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="d4775-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="d4775-117">Opis podstawowych modułu platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d4775-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d4775-118">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS albo:</span><span class="sxs-lookup"><span data-stu-id="d4775-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="d4775-119">Hostowanie aplikacji ASP.NET Core wewnątrz proces roboczy usług IIS (`w3wp.exe`), co jest nazywane [modelu hostingu w trakcie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="d4775-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="d4775-120">Przesyła żądania sieci web do uruchamiania aplikacji ASP.NET Core zaplecza [serwera Kestrel](xref:fundamentals/servers/kestrel), co jest nazywane [modelu hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="d4775-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="d4775-121">W trakcie modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="d4775-121">In-process hosting model</span></span>

<span data-ttu-id="d4775-122">Za pomocą hostingu platformy ASP.NET Core w trakcie aplikacja jest uruchamiana w tym samym procesie co jej proces roboczy usług IIS.</span><span class="sxs-lookup"><span data-stu-id="d4775-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="d4775-123">Spowoduje to usunięcie spadek wydajności buforowania żądań za pośrednictwem karty sprzężenia zwrotnego korzystając z modelu hostingu poza procesem.</span><span class="sxs-lookup"><span data-stu-id="d4775-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="d4775-124">Usługi IIS obsługuje zarządzanie procesami przy użyciu [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="d4775-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="d4775-125">Moduł ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="d4775-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="d4775-126">Wykonuje inicjowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d4775-126">Performs app initialization.</span></span>
  * <span data-ttu-id="d4775-127">Ładunki [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="d4775-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="d4775-128">Wywołania `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="d4775-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="d4775-129">Obsługuje okres istnienia żądanie natywnych usług IIS.</span><span class="sxs-lookup"><span data-stu-id="d4775-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="d4775-130">Na poniższym diagramie przedstawiono relację między usługami IIS, modułu ASP.NET Core i aplikacji obsługiwanych w procesie:</span><span class="sxs-lookup"><span data-stu-id="d4775-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="d4775-132">Żądanie dociera z sieci web do sterownik HTTP.sys trybu jądra.</span><span class="sxs-lookup"><span data-stu-id="d4775-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="d4775-133">Sterownik kieruje żądanie macierzystego w usługach IIS na porcie skonfigurowanym witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d4775-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="d4775-134">Moduł odbiera żądanie natywnych i przekazuje sterowanie do `IISHttpServer`, czyli co konwertuje żądanie z natywnego na zarządzane.</span><span class="sxs-lookup"><span data-stu-id="d4775-134">The module receives the native request and passes control to `IISHttpServer`, which is what converts the request from native to managed.</span></span>

<span data-ttu-id="d4775-135">Po `IISHttpServer` przejmuje żądania, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4775-135">After `IISHttpServer` picks up the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="d4775-136">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="d4775-136">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="d4775-137">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="d4775-137">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="d4775-138">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="d4775-138">Out-of-process hosting model</span></span>

<span data-ttu-id="d4775-139">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="d4775-139">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="d4775-140">Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera spowoduje ponowne uruchomienie aplikacji, jeśli kończy pracę, lub ulega awarii.</span><span class="sxs-lookup"><span data-stu-id="d4775-140">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="d4775-141">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje, które są uruchamiane w procesie, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="d4775-141">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="d4775-142">Na poniższym diagramie przedstawiono relację między usługami IIS, modułu ASP.NET Core, a aplikacja hostowana spoza procesu:</span><span class="sxs-lookup"><span data-stu-id="d4775-142">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="d4775-144">Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="d4775-144">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="d4775-145">Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d4775-145">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="d4775-146">Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80/443.</span><span class="sxs-lookup"><span data-stu-id="d4775-146">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="d4775-147">Moduł Określa port, za pośrednictwem zmiennej środowiskowej przy uruchamianiu i oprogramowania pośredniczącego integracji usług IIS umożliwia skonfigurowanie serwera do nasłuchiwania `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="d4775-147">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="d4775-148">Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="d4775-148">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="d4775-149">Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d4775-149">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="d4775-150">Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4775-150">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="d4775-151">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="d4775-151">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="d4775-152">Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4775-152">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="d4775-153">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="d4775-153">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d4775-154">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS do przekazywania żądań sieci web z zapleczem platformy ASP.NET Core z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d4775-154">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="d4775-155">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje także zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="d4775-155">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="d4775-156">Moduł rozpoczyna się proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera i ponownie uruchamia aplikację w przypadku jej awarii.</span><span class="sxs-lookup"><span data-stu-id="d4775-156">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="d4775-157">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje ASP.NET 4.x, działających w trakcie w usługach IIS, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="d4775-157">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="d4775-158">Na poniższym diagramie przedstawiono relację między usług IIS, modułu ASP.NET Core i aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d4775-158">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="d4775-160">Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="d4775-160">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="d4775-161">Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d4775-161">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="d4775-162">Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80/443.</span><span class="sxs-lookup"><span data-stu-id="d4775-162">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="d4775-163">Moduł Określa port, za pośrednictwem zmiennej środowiskowej przy uruchamianiu i oprogramowania pośredniczącego integracji usług IIS umożliwia skonfigurowanie serwera do nasłuchiwania `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="d4775-163">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="d4775-164">Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="d4775-164">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="d4775-165">Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d4775-165">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="d4775-166">Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4775-166">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="d4775-167">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="d4775-167">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="d4775-168">Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d4775-168">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="d4775-169">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="d4775-169">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="d4775-170">Wiele modułów macierzystych, takie jak uwierzytelnianie Windows pozostają aktywne.</span><span class="sxs-lookup"><span data-stu-id="d4775-170">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="d4775-171">Aby dowiedzieć się, jak aktywne moduły usług IIS za pomocą modułu, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="d4775-171">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="d4775-172">Modułu ASP.NET Core ma kilka innych funkcji.</span><span class="sxs-lookup"><span data-stu-id="d4775-172">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="d4775-173">Moduł wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="d4775-173">The module can:</span></span>

* <span data-ttu-id="d4775-174">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="d4775-174">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="d4775-175">Rejestrowanie strumienia stdout, dane wyjściowe do usługi file storage dotyczące rozwiązywania problemów, uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="d4775-175">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="d4775-176">Przekazuj tokeny uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="d4775-176">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="d4775-177">Jak zainstalować i używać modułu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4775-177">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="d4775-178">Aby uzyskać szczegółowe instrukcje dotyczące sposobu instalowania i używania modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="d4775-178">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="d4775-179">Aby uzyskać informacje na temat konfigurowania modułu, zobacz <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="d4775-179">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4775-180">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="d4775-180">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="d4775-181">Repozytorium GitHub modułów Core ASP.NET (kodu źródłowego)</span><span class="sxs-lookup"><span data-stu-id="d4775-181">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
