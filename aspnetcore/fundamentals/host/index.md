---
title: Host platformy ASP.NET Core
author: guardrex
description: Informacje o hosta sieci Web platformy ASP.NET Core i .NET rodzajowego hosta, które są odpowiedzialni za zarządzanie uruchamiania i okresem istnienia aplikacji.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7f8ccff7e3da93d6e617505ac93fafc3a82ed880
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252012"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="f4b78-103">Host platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4b78-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="f4b78-104">Aplikacje .NET skonfigurować i uruchomić *hosta*.</span><span class="sxs-lookup"><span data-stu-id="f4b78-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="f4b78-105">Host jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f4b78-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="f4b78-106">Host dwa interfejsy API są dostępne do użycia:</span><span class="sxs-lookup"><span data-stu-id="f4b78-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="f4b78-107">[Sieci Web hosta](xref:fundamentals/host/web-host) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="f4b78-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="f4b78-108">[Ogólny hosta](xref:fundamentals/host/generic-host) (platformy ASP.NET Core 2.1 lub nowszej) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web (na przykład aplikacji uruchamianych zadania w tle).</span><span class="sxs-lookup"><span data-stu-id="f4b78-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="f4b78-109">W przyszłym wydaniu rodzajowego hosta będzie odpowiednie do obsługi dowolnego rodzaju aplikacji, w tym aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="f4b78-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="f4b78-110">Ogólny hosta ostatecznie spowoduje zastąpienie hosta sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f4b78-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="f4b78-111">Dla hostingu ASP.NET Core *sieci web apps*, programiści powinni używać hosta sieci Web na podstawie [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="f4b78-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="f4b78-112">Dla hostingu *-web apps*, programiści powinni używać rodzajowego hosta, na podstawie [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span><span class="sxs-lookup"><span data-stu-id="f4b78-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
