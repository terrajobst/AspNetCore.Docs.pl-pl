---
title: Moduł ASP.NET Core
author: guardrex
description: Dowiedz się, jak modułu ASP.NET Core umożliwia serwer sieci web Kestrel do używania usług IIS lub IIS Express.
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: d3f3a42dd7aebc425905b865376a584bcf0e5153
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861462"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="7748e-103">Moduł ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7748e-103">ASP.NET Core Module</span></span>

<span data-ttu-id="7748e-104">Przez [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), i [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="7748e-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7748e-105">Modułu ASP.NET Core umożliwia platformy ASP.NET Core w aplikacji do uruchamiania w proces roboczy usług IIS (*w trakcie*) lub za usług IIS w konfiguracji zwrotny serwer proxy (*spoza procesu*).</span><span class="sxs-lookup"><span data-stu-id="7748e-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="7748e-106">Program IIS zawiera aplikacji web zaawansowane funkcje zabezpieczeń i możliwości zarządzania.</span><span class="sxs-lookup"><span data-stu-id="7748e-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7748e-107">Modułu ASP.NET Core umożliwia platformy ASP.NET Core uruchamiania za usług IIS w konfiguracji zwrotny serwer proxy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7748e-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="7748e-108">Program IIS zawiera aplikacji web zaawansowane funkcje zabezpieczeń i możliwości zarządzania.</span><span class="sxs-lookup"><span data-stu-id="7748e-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="7748e-109">Obsługiwane wersje Windows:</span><span class="sxs-lookup"><span data-stu-id="7748e-109">Supported Windows versions:</span></span>

* <span data-ttu-id="7748e-110">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="7748e-110">Windows 7 or later</span></span>
* <span data-ttu-id="7748e-111">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="7748e-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7748e-112">W przypadku hostowania w procesie, moduł używa implementację w procesie serwera usług IIS, serwera HTTP usług IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="7748e-112">When hosting in-process, the module uses an IIS in-process server implementation, IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="7748e-113">Gdy w hostingu poza procesem, moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7748e-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="7748e-114">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej noszącą nazwę [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="7748e-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7748e-115">Moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7748e-115">The module only works with Kestrel.</span></span> <span data-ttu-id="7748e-116">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej noszącą nazwę [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="7748e-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="7748e-117">Opis podstawowych modułu platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7748e-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7748e-118">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS albo:</span><span class="sxs-lookup"><span data-stu-id="7748e-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="7748e-119">Hostowanie aplikacji ASP.NET Core wewnątrz proces roboczy usług IIS (`w3wp.exe`), co jest nazywane [modelu hostingu w trakcie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="7748e-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="7748e-120">Przesyła żądania sieci web do uruchamiania aplikacji ASP.NET Core zaplecza [serwera Kestrel](xref:fundamentals/servers/kestrel), co jest nazywane [modelu hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="7748e-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="7748e-121">W trakcie modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="7748e-121">In-process hosting model</span></span>

<span data-ttu-id="7748e-122">Za pomocą hostingu platformy ASP.NET Core w trakcie aplikacja jest uruchamiana w tym samym procesie co jej proces roboczy usług IIS.</span><span class="sxs-lookup"><span data-stu-id="7748e-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="7748e-123">Spowoduje to usunięcie spadek wydajności buforowania żądań za pośrednictwem karty sprzężenia zwrotnego korzystając z modelu hostingu poza procesem.</span><span class="sxs-lookup"><span data-stu-id="7748e-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="7748e-124">Usługi IIS obsługuje zarządzanie procesami przy użyciu [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="7748e-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="7748e-125">Moduł ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7748e-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="7748e-126">Wykonuje inicjowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7748e-126">Performs app initialization.</span></span>
  * <span data-ttu-id="7748e-127">Ładunki [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="7748e-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="7748e-128">Wywołania `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="7748e-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="7748e-129">Obsługuje okres istnienia żądanie natywnych usług IIS.</span><span class="sxs-lookup"><span data-stu-id="7748e-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="7748e-130">Na poniższym diagramie przedstawiono relację między usługami IIS, modułu ASP.NET Core i aplikacji obsługiwanych w procesie:</span><span class="sxs-lookup"><span data-stu-id="7748e-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="7748e-132">Żądanie dociera z sieci web do sterownik HTTP.sys trybu jądra.</span><span class="sxs-lookup"><span data-stu-id="7748e-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="7748e-133">Sterownik kieruje żądanie macierzystego w usługach IIS na porcie skonfigurowanym witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7748e-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="7748e-134">Moduł odbiera żądanie natywnych i przekazuje go do serwera HTTP usług IIS (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="7748e-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="7748e-135">Serwer HTTP usług IIS jest implementacją w procesie serwera usług IIS, który konwertuje żądania z natywnego na zarządzanych.</span><span class="sxs-lookup"><span data-stu-id="7748e-135">IIS HTTP Server is an IIS in-process server implementation that converts the request from native to managed.</span></span>

<span data-ttu-id="7748e-136">Po przetworzeniu żądania przez serwer HTTP usług IIS, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7748e-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="7748e-137">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="7748e-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="7748e-138">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="7748e-138">The app's response is passed back to IIS, which pushes it back out to the client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="7748e-139">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="7748e-139">Out-of-process hosting model</span></span>

<span data-ttu-id="7748e-140">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="7748e-140">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="7748e-141">Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera spowoduje ponowne uruchomienie aplikacji, jeśli kończy pracę, lub ulega awarii.</span><span class="sxs-lookup"><span data-stu-id="7748e-141">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="7748e-142">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje, które są uruchamiane w procesie, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="7748e-142">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="7748e-143">Na poniższym diagramie przedstawiono relację między usługami IIS, modułu ASP.NET Core, a aplikacja hostowana spoza procesu:</span><span class="sxs-lookup"><span data-stu-id="7748e-143">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="7748e-145">Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="7748e-145">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="7748e-146">Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7748e-146">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="7748e-147">Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80/443.</span><span class="sxs-lookup"><span data-stu-id="7748e-147">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="7748e-148">Moduł Określa port, za pośrednictwem zmiennej środowiskowej przy uruchamianiu i oprogramowania pośredniczącego integracji usług IIS umożliwia skonfigurowanie serwera do nasłuchiwania `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="7748e-148">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="7748e-149">Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="7748e-149">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="7748e-150">Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7748e-150">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="7748e-151">Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7748e-151">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="7748e-152">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="7748e-152">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="7748e-153">Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7748e-153">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="7748e-154">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="7748e-154">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7748e-155">Modułu ASP.NET Core jest moduł macierzysty usług IIS, które podłącza się do potoku usług IIS do przekazywania żądań sieci web z zapleczem platformy ASP.NET Core z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7748e-155">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="7748e-156">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje także zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="7748e-156">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="7748e-157">Moduł rozpoczyna się proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie dociera i ponownie uruchamia aplikację w przypadku jej awarii.</span><span class="sxs-lookup"><span data-stu-id="7748e-157">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="7748e-158">Jest to zasadniczo takie samo zachowanie, ponieważ aplikacje ASP.NET 4.x, działających w trakcie w usługach IIS, które są zarządzane przez [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="7748e-158">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="7748e-159">Na poniższym diagramie przedstawiono relację między usług IIS, modułu ASP.NET Core i aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7748e-159">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Moduł ASP.NET Core](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="7748e-161">Żądania pojawić się w sieci Web w trybie jądra sterownik HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="7748e-161">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="7748e-162">Sterownik kieruje żądania do usługi IIS w witrynie sieci Web skonfigurowanego portu, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7748e-162">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="7748e-163">Moduł przekazuje żądania do Kestrel na losowy port aplikacji, która nie jest port 80/443.</span><span class="sxs-lookup"><span data-stu-id="7748e-163">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="7748e-164">Moduł Określa port, za pośrednictwem zmiennej środowiskowej przy uruchamianiu i oprogramowania pośredniczącego integracji usług IIS umożliwia skonfigurowanie serwera do nasłuchiwania `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="7748e-164">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="7748e-165">Wykonywane są dodatkowe czynności kontrolne i żądania, które nie pochodzą z modułu są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="7748e-165">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="7748e-166">Moduł nie obsługuje przekazywanie protokołu HTTPS, dlatego żądania są przekazywane za pośrednictwem protokołu HTTP, nawet wtedy, gdy odbierane przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7748e-166">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="7748e-167">Po Kestrel przejmuje żądania z modułu, żądania są przesyłane do potoku oprogramowania pośredniczącego platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7748e-167">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="7748e-168">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="7748e-168">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="7748e-169">Oprogramowanie pośredniczące dodane przez usługi IIS integracji aktualizuje schemat, zdalny adres IP i pathbase w celu uwzględnienia przekazywania żądania do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7748e-169">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="7748e-170">Odpowiedź aplikacji jest przekazywany z powrotem do usług IIS, wypchnięć, które go wycofać do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="7748e-170">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="7748e-171">Wiele modułów macierzystych, takie jak uwierzytelnianie Windows pozostają aktywne.</span><span class="sxs-lookup"><span data-stu-id="7748e-171">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="7748e-172">Aby dowiedzieć się, jak aktywne moduły usług IIS za pomocą modułu, zobacz <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="7748e-172">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="7748e-173">Modułu ASP.NET Core ma kilka innych funkcji.</span><span class="sxs-lookup"><span data-stu-id="7748e-173">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="7748e-174">Moduł wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7748e-174">The module can:</span></span>

* <span data-ttu-id="7748e-175">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="7748e-175">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="7748e-176">Rejestrowanie strumienia stdout, dane wyjściowe do usługi file storage dotyczące rozwiązywania problemów, uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="7748e-176">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="7748e-177">Przekazuj tokeny uwierzytelniania Windows.</span><span class="sxs-lookup"><span data-stu-id="7748e-177">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="7748e-178">Jak zainstalować i używać modułu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7748e-178">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="7748e-179">Aby uzyskać szczegółowe instrukcje dotyczące sposobu instalowania i używania modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="7748e-179">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="7748e-180">Aby uzyskać informacje na temat konfigurowania modułu, zobacz <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="7748e-180">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7748e-181">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7748e-181">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="7748e-182">Repozytorium GitHub modułów Core ASP.NET (kodu źródłowego)</span><span class="sxs-lookup"><span data-stu-id="7748e-182">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
