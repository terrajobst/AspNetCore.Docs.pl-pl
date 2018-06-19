---
uid: webhooks/receiving/dependencies
title: Zależności odbiornika elementów Webhook ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Odbiornik zależności i iniekcja zależności w elementów Webhook ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573278"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Zależności odbiornika elementów Webhook ASP.NET

Microsoft ASP.NET WebHooks zaprojektowano z iniekcji zależności na uwadze. Większość zależności w systemie można zastąpić alternatywnych implementacji przy użyciu aparatu iniekcji zależności.

Zobacz [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) listę zależności odbiornika. Jeśli nie zarejestrowano żadnych zależności, zostanie użyta domyślna implementacja. Zobacz [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) listę domyślnej implementacji.
