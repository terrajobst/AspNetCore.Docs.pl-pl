---
uid: webhooks/receiving/dependencies
title: Zależności odbiornika elementów Webhook programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Zależności odbiornika i wstrzykiwanie zależności w elementów Webhook programu ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.openlocfilehash: cf45e3d2e45e4b7882b42d9aa0931e18c08f76b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401189"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="7a403-103">Zależności odbiornika elementów Webhook programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7a403-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="7a403-104">Microsoft ASP.NET WebHooks zaprojektowano przy użyciu iniekcji zależności na uwadze.</span><span class="sxs-lookup"><span data-stu-id="7a403-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="7a403-105">Większość zależności w systemie można zastąpić za pomocą alternatywnych implementacji przy użyciu aparatu iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="7a403-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="7a403-106">Zobacz [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) listę zależności odbiornika.</span><span class="sxs-lookup"><span data-stu-id="7a403-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="7a403-107">Jeśli nie zarejestrowano żadnych zależności, używana jest domyślna implementacja.</span><span class="sxs-lookup"><span data-stu-id="7a403-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="7a403-108">Zobacz [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) listę domyślnej implementacji.</span><span class="sxs-lookup"><span data-stu-id="7a403-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
