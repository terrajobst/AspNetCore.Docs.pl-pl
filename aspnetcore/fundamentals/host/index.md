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
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a>Host platformy ASP.NET Core

Aplikacje .NET skonfigurować i uruchomić *hosta*. Host jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji. Host dwa interfejsy API są dostępne do użycia:

* [Sieci Web hosta](xref:fundamentals/host/web-host) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web.
* [Ogólny hosta](xref:fundamentals/host/generic-host) (platformy ASP.NET Core 2.1 lub nowszej) &ndash; odpowiednia na potrzeby hostowania aplikacji sieci web (na przykład aplikacji uruchamianych zadania w tle). W przyszłym wydaniu rodzajowego hosta będzie odpowiednie do obsługi dowolnego rodzaju aplikacji, w tym aplikacji sieci web. Ogólny hosta ostatecznie spowoduje zastąpienie hosta sieci Web.

W tej chwili Programiści powinni używać [hosta sieci Web](xref:fundamentals/host/web-host) na podstawie [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) do hostowania aplikacji platformy ASP.NET Core.
