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
# <a name="aspnet-webhooks-receiver-dependencies"></a>Zależności odbiornika elementów Webhook programu ASP.NET

Microsoft ASP.NET WebHooks zaprojektowano przy użyciu iniekcji zależności na uwadze. Większość zależności w systemie można zastąpić za pomocą alternatywnych implementacji przy użyciu aparatu iniekcji zależności.

Zobacz [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) listę zależności odbiornika. Jeśli nie zarejestrowano żadnych zależności, używana jest domyślna implementacja. Zobacz [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) listę domyślnej implementacji.
