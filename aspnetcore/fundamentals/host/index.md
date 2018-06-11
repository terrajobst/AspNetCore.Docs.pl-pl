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
# <a name="host-in-aspnet-core"></a>Host platformy ASP.NET Core

Aplikacje .NET skonfigurować i uruchomić *hosta*. Host jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji. Host dwa interfejsy API są dostępne do użycia:

* [Sieci Web hosta](xref:fundamentals/host/web-host) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web.
* [Ogólny hosta](xref:fundamentals/host/generic-host) (platformy ASP.NET Core 2.1 lub nowszej) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web (na przykład aplikacji uruchamianych zadania w tle). W przyszłym wydaniu rodzajowego hosta będzie odpowiednie do obsługi dowolnego rodzaju aplikacji, w tym aplikacji sieci web. Ogólny hosta ostatecznie spowoduje zastąpienie hosta sieci Web.

Dla hostingu ASP.NET Core *sieci web apps*, programiści powinni używać hosta sieci Web na podstawie [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Dla hostingu *-web apps*, programiści powinni używać rodzajowego hosta, na podstawie [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).
