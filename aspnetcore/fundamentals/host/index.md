---
title: Hosta w programie ASP.NET Core
author: guardrex
description: Informacje o hosta sieci Web programu ASP.NET Core i .NET ogólnego hosta, które są odpowiedzialni za zarządzanie uruchamiania i czasu życia aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 9927722b5080beb94e5628d9e7b54e6d50a5bff8
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336053"
---
# <a name="host-in-aspnet-core"></a>Hosta w programie ASP.NET Core

Konfigurowanie aplikacji platformy .NET i uruchamiania *hosta*. Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji. Host dwa interfejsy API są dostępne do użycia:

* [Sieci Web hosta](xref:fundamentals/host/web-host) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web.
* [Ogólny hosta](xref:fundamentals/host/generic-host) (platformy ASP.NET Core 2.1 lub nowszej) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web (na przykład aplikacje, które uruchamianie zadań w tle). W przyszłej wersji ogólnych hosta będzie odpowiedni do hostowania dowolnych aplikacji, w tym aplikacje sieci web. Ogólny hosta ostatecznie spowoduje zastąpienie hosta sieci Web.

Dla hostingu platformy ASP.NET Core *aplikacje sieci web*, programiści powinni używać hosta sieci Web, na podstawie <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>. Do hostowania *-web apps*, deweloperzy należy używać ogólnych hosta, na podstawie <xref:Microsoft.Extensions.Hosting.HostBuilder>.

<xref:fundamentals/host/hosted-services>  
Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.

<xref:fundamentals/configuration/platform-specific-configuration>  
Odkryj, jak i zwiększanie możliwości aplikacji platformy ASP.NET Core z odwołania lub nieużywany zestawu przy użyciu <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementacji.
