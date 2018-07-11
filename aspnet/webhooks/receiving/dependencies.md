---
uid: webhooks/receiving/dependencies
title: Zależności odbiornika elementów Webhook programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Zależności odbiornika i wstrzykiwanie zależności w elementów Webhook programu ASP.NET.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 05dcfac121e7974fd83c5b3736616479574944a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817840"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Zależności odbiornika elementów Webhook programu ASP.NET

Microsoft ASP.NET WebHooks zaprojektowano przy użyciu iniekcji zależności na uwadze. Większość zależności w systemie można zastąpić za pomocą alternatywnych implementacji przy użyciu aparatu iniekcji zależności.

Zobacz [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) listę zależności odbiornika. Jeśli nie zarejestrowano żadnych zależności, używana jest domyślna implementacja. Zobacz [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) listę domyślnej implementacji.
