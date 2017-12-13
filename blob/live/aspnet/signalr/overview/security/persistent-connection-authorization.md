---
uid: signalr/overview/security/persistent-connection-authorization
title: "Uwierzytelnianie i autoryzacja połączenia trwałego SignalR | Dokumentacja firmy Microsoft"
author: pfletcher
description: "W tym temacie opisano wymuszanie autoryzacji dla trwałego połączenia. Aby uzyskać ogólne informacje o integracji zabezpieczeń aplikacji SignalR..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9c6fff86ae6b1b65e6ba9922b6b8448643ef1f15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="99af5-104">Uwierzytelnianie i autoryzacja połączenia trwałego SignalR</span><span class="sxs-lookup"><span data-stu-id="99af5-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="99af5-105">przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="99af5-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="99af5-106">W tym temacie opisano wymuszanie autoryzacji dla trwałego połączenia.</span><span class="sxs-lookup"><span data-stu-id="99af5-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="99af5-107">Aby uzyskać ogólne informacje na temat integracji zabezpieczeń w aplikacji SignalR, zobacz [wprowadzenie do zabezpieczeń](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="99af5-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="99af5-108">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="99af5-108">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="99af5-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="99af5-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="99af5-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="99af5-110">.NET 4.5</span></span>
> - <span data-ttu-id="99af5-111">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="99af5-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="99af5-112">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="99af5-112">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="99af5-113">Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="99af5-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="99af5-114">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="99af5-114">Questions and comments</span></span>
> 
> <span data-ttu-id="99af5-115">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="99af5-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="99af5-116">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="99af5-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="99af5-117">Wymuszanie autoryzacji</span><span class="sxs-lookup"><span data-stu-id="99af5-117">Enforce authorization</span></span>

<span data-ttu-id="99af5-118">Aby wymusić reguł autoryzacji, korzystając z [klasy PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) konieczne jest przesłonięcie `AuthorizeRequest` metody.</span><span class="sxs-lookup"><span data-stu-id="99af5-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="99af5-119">Nie można użyć `Authorize` atrybut z połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="99af5-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="99af5-120">`AuthorizeRequest` Metoda jest wywoływana przez platformę SignalR przed każdym żądaniem, aby sprawdzić, czy użytkownik jest autoryzowany do wykonania żądanej akcji.</span><span class="sxs-lookup"><span data-stu-id="99af5-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="99af5-121">`AuthorizeRequest` Metoda nie jest wywoływana z klienta; zamiast tego należy uwierzytelnić użytkownika za pomocą mechanizmu uwierzytelniania standardowych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="99af5-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="99af5-122">W poniższym przykładzie przedstawiono sposób ogranicza liczby żądań, uwierzytelnionym użytkownikom.</span><span class="sxs-lookup"><span data-stu-id="99af5-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="99af5-123">Można dodać wszelka logika dostosowane autoryzacji w metodzie AuthorizeRequest; takich jak sprawdzanie, czy użytkownik należy do określonej roli.</span><span class="sxs-lookup"><span data-stu-id="99af5-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
