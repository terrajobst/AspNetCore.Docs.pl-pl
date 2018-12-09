---
title: Host sieci Web i ogólny hosta w programie ASP.NET Core
author: guardrex
description: Informacje o hosta sieci Web programu ASP.NET Core i .NET ogólnego hosta, które są odpowiedzialni za zarządzanie uruchamiania i czasu życia aplikacji.
ms.author: riande
ms.custom: mvc,seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 3e67d8338aa7ac1b1530d0498ee0126d36a8d72b
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121521"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a>Host sieci Web i ogólny hosta w programie ASP.NET Core

Konfigurowanie aplikacji platformy .NET i uruchamiania *hosta*. Host jest odpowiedzialny za zarządzanie uruchamiania i czasu życia aplikacji. Host dwa interfejsy API są dostępne do użycia:

* [Sieci Web hosta](xref:fundamentals/host/web-host) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web.
* [Ogólny hosta](xref:fundamentals/host/generic-host) (platformy ASP.NET Core 2.1 lub nowszej) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web (na przykład aplikacje, które uruchamianie zadań w tle). W przyszłej wersji ogólnych hosta będzie odpowiedni do hostowania dowolnych aplikacji, w tym aplikacje sieci web. Ogólny hosta ostatecznie spowoduje zastąpienie hosta sieci Web.

Dla hostingu platformy ASP.NET Core *aplikacje sieci web*, programiści powinni używać hosta sieci Web, na podstawie <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>. Do hostowania *-web apps*, deweloperzy należy używać ogólnych hosta, na podstawie <xref:Microsoft.Extensions.Hosting.HostBuilder>.

<xref:fundamentals/host/hosted-services>  
Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.

<xref:fundamentals/configuration/platform-specific-configuration>  
Odkryj, jak i zwiększanie możliwości aplikacji platformy ASP.NET Core z odwołania lub nieużywany zestawu przy użyciu <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementacji.
