---
title: Moduł platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak moduł platformy ASP.NET Core umożliwia Kestrel serwer sieci web dla usług IIS lub usług IIS Express jako serwera zwrotnego serwera proxy.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 789b0b2e74405256bb0e247ae5ea9697e3cb28e5
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-module"></a><span data-ttu-id="a57e0-103">Moduł platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a57e0-103">ASP.NET Core Module</span></span>

<span data-ttu-id="a57e0-104">Przez [Dykstra Tomasz](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), i [Roaming Krzysztof](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="a57e0-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="a57e0-105">Moduł platformy ASP.NET Core umożliwia platformy ASP.NET Core uruchamiania za usług IIS w konfiguracji zwrotny serwer proxy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a57e0-105">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="a57e0-106">Usługi IIS oferują aplikacji web zaawansowane funkcje zabezpieczeń i możliwości zarządzania.</span><span class="sxs-lookup"><span data-stu-id="a57e0-106">IIS provides advanced web app security and manageability features.</span></span>

<span data-ttu-id="a57e0-107">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="a57e0-107">Supported Windows versions:</span></span>

* <span data-ttu-id="a57e0-108">Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a57e0-108">Windows 7 or later</span></span>
* <span data-ttu-id="a57e0-109">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a57e0-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="a57e0-110">Moduł platformy ASP.NET Core działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a57e0-110">The ASP.NET Core Module only works with Kestrel.</span></span> <span data-ttu-id="a57e0-111">Moduł nie jest zgodna z [HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanych [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="a57e0-111">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

## <a name="aspnet-core-module-description"></a><span data-ttu-id="a57e0-112">Opis podstawowych modułu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a57e0-112">ASP.NET Core Module description</span></span>

<span data-ttu-id="a57e0-113">Moduł platformy ASP.NET Core jest macierzysty moduł usług IIS, które podłącza się do potoku usług IIS do przekierowywania żądań sieci web do zaplecza aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a57e0-113">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to redirect web requests to backend ASP.NET Core apps.</span></span> <span data-ttu-id="a57e0-114">Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostają aktywne.</span><span class="sxs-lookup"><span data-stu-id="a57e0-114">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="a57e0-115">Aby dowiedzieć się, jak aktywne moduły usług IIS w module, zobacz [moduły IIS](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="a57e0-115">To learn more about IIS modules active with the module, see [IIS modules](xref:host-and-deploy/iis/modules).</span></span>

<span data-ttu-id="a57e0-116">Ponieważ aplikacje platformy ASP.NET Core, uruchom w procesie oddzielić od proces roboczy usług IIS, moduł obsługuje także zarządzanie procesem.</span><span class="sxs-lookup"><span data-stu-id="a57e0-116">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="a57e0-117">Moduł uruchamia proces dla aplikacji platformy ASP.NET Core po pierwsze żądanie dociera i ponowne uruchomienie aplikacji, jeśli uległa awarii.</span><span class="sxs-lookup"><span data-stu-id="a57e0-117">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="a57e0-118">Jest to zasadniczo takie samo zachowanie jako ASP.NET 4.x aplikacje, które są uruchamiane w procesie w usługach IIS, które są zarządzane przez [usługi aktywacji procesów systemu Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="a57e0-118">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="a57e0-119">Na poniższym diagramie przedstawiono relację między usługami IIS, platformy ASP.NET Core modułu a aplikacje platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a57e0-119">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and ASP.NET Core apps:</span></span>

![Moduł platformy ASP.NET Core](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="a57e0-121">Żądania odbierania z sieci web do sterownik HTTP.sys trybu jądra.</span><span class="sxs-lookup"><span data-stu-id="a57e0-121">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="a57e0-122">Sterownik kieruje żądania do usług IIS na skonfigurowanym porcie witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="a57e0-122">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="a57e0-123">Moduł przekazuje żądania do Kestrel na losowy port dla aplikacji, która nie jest port 80/443.</span><span class="sxs-lookup"><span data-stu-id="a57e0-123">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="a57e0-124">Moduł Określa port, za pomocą zmiennej środowiskowej podczas uruchamiania i oprogramowanie pośredniczące integracji usług IIS umożliwia skonfigurowanie serwera do nasłuchiwania `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="a57e0-124">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="a57e0-125">Dodatkowe testy są wykonywane, i zostały odrzucone żądania, które nie pochodzą z modułu.</span><span class="sxs-lookup"><span data-stu-id="a57e0-125">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="a57e0-126">Moduł nie obsługuje przekazywanie protokołu HTTPS, więc żądania są przekazywane za pośrednictwem protokołu HTTP, nawet jeśli odebranych przez usługi IIS przy użyciu protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a57e0-126">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="a57e0-127">Po Kestrel przejmuje żądania z modułu, żądanie zostanie przypisany do potoku oprogramowanie pośredniczące platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a57e0-127">After Kestrel picks up a request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a57e0-128">Potoku oprogramowania pośredniczącego obsługuje żądanie i przekazuje ją jako `HttpContext` wystąpienie aplikacji logiki.</span><span class="sxs-lookup"><span data-stu-id="a57e0-128">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="a57e0-129">Odpowiedzi aplikacji jest przekazywane z powrotem do usług IIS, które umieszcza on Wycofaj do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="a57e0-129">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="a57e0-130">Moduł platformy ASP.NET Core ma kilka innych funkcji.</span><span class="sxs-lookup"><span data-stu-id="a57e0-130">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="a57e0-131">Moduł może:</span><span class="sxs-lookup"><span data-stu-id="a57e0-131">The module can:</span></span>

* <span data-ttu-id="a57e0-132">Ustawianie zmiennych środowiskowych dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="a57e0-132">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="a57e0-133">Zaloguj się stdout dane wyjściowe do przechowywania plików podczas rozwiązywania problemów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="a57e0-133">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="a57e0-134">Tokeny uwierzytelniania systemu Windows do przodu.</span><span class="sxs-lookup"><span data-stu-id="a57e0-134">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="a57e0-135">Jak zainstalować i używać modułu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a57e0-135">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="a57e0-136">Aby uzyskać szczegółowe instrukcje na temat instalowania i używania moduł platformy ASP.NET Core, zobacz [hosta w systemie Windows z programem IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="a57e0-136">For detailed instructions on how to install and use the ASP.NET Core Module, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="a57e0-137">Aby uzyskać informacje na temat konfigurowania modułu, zobacz [odwołania konfiguracji platformy ASP.NET Core modułu](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="a57e0-137">For information on configuring the module, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a57e0-138">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a57e0-138">Additional resources</span></span>

* [<span data-ttu-id="a57e0-139">Hosting w systemie Windows z usługami IIS</span><span class="sxs-lookup"><span data-stu-id="a57e0-139">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="a57e0-140">Odwołania do konfiguracji modułu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a57e0-140">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="a57e0-141">Repozytorium GitHub modułu podstawowej platformy ASP.NET (kodu źródłowego)</span><span class="sxs-lookup"><span data-stu-id="a57e0-141">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
