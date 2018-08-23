---
uid: webhooks/receiving/dependencies
title: Zależności odbiornika elementów Webhook programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Zależności odbiornika i wstrzykiwanie zależności w elementów Webhook programu ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755047"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="2fcbc-103">Zależności odbiornika elementów Webhook programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2fcbc-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="2fcbc-104">Microsoft ASP.NET WebHooks zaprojektowano przy użyciu iniekcji zależności na uwadze.</span><span class="sxs-lookup"><span data-stu-id="2fcbc-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="2fcbc-105">Większość zależności w systemie można zastąpić za pomocą alternatywnych implementacji przy użyciu aparatu iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="2fcbc-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="2fcbc-106">Zobacz [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) listę zależności odbiornika.</span><span class="sxs-lookup"><span data-stu-id="2fcbc-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="2fcbc-107">Jeśli nie zarejestrowano żadnych zależności, używana jest domyślna implementacja.</span><span class="sxs-lookup"><span data-stu-id="2fcbc-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="2fcbc-108">Zobacz [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) listę domyślnej implementacji.</span><span class="sxs-lookup"><span data-stu-id="2fcbc-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
