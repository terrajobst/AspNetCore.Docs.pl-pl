---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Uwierzytelnianie i autoryzacja połączeń trwałych SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano sposób wymusić autoryzację dla trwałego połączenia. Aby uzyskać ogólne informacje na temat integracji zabezpieczeń do aplikacji SignalR...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 471bce03a37a401c2afe3735afef6f838555cc26
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365113"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="cc125-104">Uwierzytelnianie i autoryzacja połączeń trwałych SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="cc125-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="cc125-105">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cc125-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cc125-106">W tym temacie opisano sposób wymusić autoryzację dla trwałego połączenia.</span><span class="sxs-lookup"><span data-stu-id="cc125-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="cc125-107">Aby uzyskać ogólne informacje na temat integracji zabezpieczeń do aplikacji SignalR zobacz [wprowadzenie do zabezpieczeń](index.md).</span><span class="sxs-lookup"><span data-stu-id="cc125-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="cc125-108">Wymuszanie autoryzacji</span><span class="sxs-lookup"><span data-stu-id="cc125-108">Enforce authorization</span></span>

<span data-ttu-id="cc125-109">Aby wymusić reguł autoryzacji, korzystając z [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) konieczne jest przesłonięcie `AuthorizeRequest` metody.</span><span class="sxs-lookup"><span data-stu-id="cc125-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="cc125-110">Nie można użyć `Authorize` atrybutu z połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="cc125-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="cc125-111">`AuthorizeRequest` Metoda jest wywoływana przez SignalR Framework przed każdym żądaniem, aby sprawdzić, czy użytkownik jest autoryzowany do wykonania żądanej akcji.</span><span class="sxs-lookup"><span data-stu-id="cc125-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="cc125-112">`AuthorizeRequest` Metoda nie jest wywoływana przez klienta; zamiast tego należy uwierzytelnić użytkownika za pośrednictwem mechanizmu uwierzytelniania standardowego w Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc125-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="cc125-113">W poniższym przykładzie pokazano sposób ograniczania żądań do uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="cc125-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="cc125-114">Można dodać wszelka logika autoryzacji dostosowanego w metodzie AuthorizeRequest; takich jak sprawdzanie, czy użytkownik należy do określonej roli.</span><span class="sxs-lookup"><span data-stu-id="cc125-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
