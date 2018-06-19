---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Uwierzytelnianie i autoryzacja połączenia trwałego SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano wymuszanie autoryzacji dla trwałego połączenia. Aby uzyskać ogólne informacje o integracji zabezpieczeń aplikacji SignalR...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 2e97dfd03c61b110325c41a992b4af490fcd17de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036105"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="9065a-104">Uwierzytelnianie i autoryzacja połączenia trwałego SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="9065a-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="9065a-105">przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9065a-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9065a-106">W tym temacie opisano wymuszanie autoryzacji dla trwałego połączenia.</span><span class="sxs-lookup"><span data-stu-id="9065a-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="9065a-107">Aby uzyskać ogólne informacje na temat integracji zabezpieczeń w aplikacji SignalR, zobacz [wprowadzenie do zabezpieczeń](index.md).</span><span class="sxs-lookup"><span data-stu-id="9065a-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="9065a-108">Wymuszanie autoryzacji</span><span class="sxs-lookup"><span data-stu-id="9065a-108">Enforce authorization</span></span>

<span data-ttu-id="9065a-109">Aby wymusić reguł autoryzacji, korzystając z [klasy PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) konieczne jest przesłonięcie `AuthorizeRequest` metody.</span><span class="sxs-lookup"><span data-stu-id="9065a-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="9065a-110">Nie można użyć `Authorize` atrybut z połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="9065a-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="9065a-111">`AuthorizeRequest` Metoda jest wywoływana przez platformę SignalR przed każdym żądaniem, aby sprawdzić, czy użytkownik jest autoryzowany do wykonania żądanej akcji.</span><span class="sxs-lookup"><span data-stu-id="9065a-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="9065a-112">`AuthorizeRequest` Metoda nie jest wywoływana z klienta; zamiast tego należy uwierzytelnić użytkownika za pomocą mechanizmu uwierzytelniania standardowych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9065a-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="9065a-113">W poniższym przykładzie przedstawiono sposób ogranicza liczby żądań, uwierzytelnionym użytkownikom.</span><span class="sxs-lookup"><span data-stu-id="9065a-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="9065a-114">Można dodać wszelka logika dostosowane autoryzacji w metodzie AuthorizeRequest; takich jak sprawdzanie, czy użytkownik należy do określonej roli.</span><span class="sxs-lookup"><span data-stu-id="9065a-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
