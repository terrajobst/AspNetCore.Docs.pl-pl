---
uid: signalr/overview/security/persistent-connection-authorization
title: Uwierzytelnianie i autoryzacja połączeń trwałych SignalR | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano sposób wymusić autoryzację dla trwałego połączenia. Aby uzyskać ogólne informacje na temat integracji zabezpieczeń do aplikacji SignalR...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: bbbcb5593fb265eca4fb261d378532047d53e674
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287393"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="0151c-104">Uwierzytelnianie i autoryzacja połączeń trwałych usługi SignalR</span><span class="sxs-lookup"><span data-stu-id="0151c-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="0151c-105">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0151c-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="0151c-106">W tym temacie opisano sposób wymusić autoryzację dla trwałego połączenia.</span><span class="sxs-lookup"><span data-stu-id="0151c-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="0151c-107">Aby uzyskać ogólne informacje na temat integracji zabezpieczeń do aplikacji SignalR zobacz [wprowadzenie do zabezpieczeń](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="0151c-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="0151c-108">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="0151c-108">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="0151c-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0151c-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="0151c-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="0151c-110">.NET 4.5</span></span>
> - <span data-ttu-id="0151c-111">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="0151c-111">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="0151c-112">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="0151c-112">Previous versions of this topic</span></span>
>
> <span data-ttu-id="0151c-113">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="0151c-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="0151c-114">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="0151c-114">Questions and comments</span></span>
>
> <span data-ttu-id="0151c-115">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="0151c-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="0151c-116">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="0151c-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="0151c-117">Wymuszanie autoryzacji</span><span class="sxs-lookup"><span data-stu-id="0151c-117">Enforce authorization</span></span>

<span data-ttu-id="0151c-118">Aby wymusić reguł autoryzacji, korzystając z [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) konieczne jest przesłonięcie `AuthorizeRequest` metody.</span><span class="sxs-lookup"><span data-stu-id="0151c-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="0151c-119">Nie można użyć `Authorize` atrybutu z połączeń trwałych.</span><span class="sxs-lookup"><span data-stu-id="0151c-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="0151c-120">`AuthorizeRequest` Metoda jest wywoływana przez SignalR Framework przed każdym żądaniem, aby sprawdzić, czy użytkownik jest autoryzowany do wykonania żądanej akcji.</span><span class="sxs-lookup"><span data-stu-id="0151c-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="0151c-121">`AuthorizeRequest` Metoda nie jest wywoływana przez klienta; zamiast tego należy uwierzytelnić użytkownika za pośrednictwem mechanizmu uwierzytelniania standardowego w Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0151c-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="0151c-122">W poniższym przykładzie pokazano sposób ograniczania żądań do uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="0151c-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="0151c-123">Można dodać wszelka logika autoryzacji dostosowanego w metodzie AuthorizeRequest; takich jak sprawdzanie, czy użytkownik należy do określonej roli.</span><span class="sxs-lookup"><span data-stu-id="0151c-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
