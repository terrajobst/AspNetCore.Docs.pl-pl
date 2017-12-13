---
uid: webhooks/receiving/dependencies
title: "Zależności odbiornika elementów Webhook ASP.NET | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Odbiornik zależności i iniekcja zależności w elementów Webhook ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="22df7-103">Zależności odbiornika elementów Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="22df7-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="22df7-104">Microsoft ASP.NET WebHooks zaprojektowano z iniekcji zależności na uwadze.</span><span class="sxs-lookup"><span data-stu-id="22df7-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="22df7-105">Większość zależności w systemie można zastąpić alternatywnych implementacji przy użyciu aparatu iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="22df7-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="22df7-106">Zobacz [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) listę zależności odbiornika.</span><span class="sxs-lookup"><span data-stu-id="22df7-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="22df7-107">Jeśli nie zarejestrowano żadnych zależności, zostanie użyta domyślna implementacja.</span><span class="sxs-lookup"><span data-stu-id="22df7-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="22df7-108">Zobacz [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) listę domyślnej implementacji.</span><span class="sxs-lookup"><span data-stu-id="22df7-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
