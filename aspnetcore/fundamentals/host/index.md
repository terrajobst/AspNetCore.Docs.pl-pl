---
title: Host platformy ASP.NET Core
author: guardrex
description: Informacje o hosta sieci Web platformy ASP.NET Core i .NET rodzajowego hosta, które są odpowiedzialni za zarządzanie uruchamiania i okresem istnienia aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276620"
---
# <a name="host-in-aspnet-core"></a>Host platformy ASP.NET Core

Aplikacje .NET skonfigurować i uruchomić *hosta*. Host jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji. Host dwa interfejsy API są dostępne do użycia:

* [Sieci Web hosta](xref:fundamentals/host/web-host) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web.
* [Ogólny hosta](xref:fundamentals/host/generic-host) (platformy ASP.NET Core 2.1 lub nowszej) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web (na przykład aplikacji uruchamianych zadania w tle). W przyszłym wydaniu rodzajowego hosta będzie odpowiednie do obsługi dowolnego rodzaju aplikacji, w tym aplikacji sieci web. Ogólny hosta ostatecznie spowoduje zastąpienie hosta sieci Web.

Dla hostingu ASP.NET Core *sieci web apps*, programiści powinni używać hosta sieci Web na podstawie [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Dla hostingu *-web apps*, programiści powinni używać rodzajowego hosta, na podstawie [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).
