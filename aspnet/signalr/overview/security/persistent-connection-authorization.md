---
uid: signalr/overview/security/persistent-connection-authorization
title: Uwierzytelnianie i autoryzacja połączenia trwałego SignalR | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano wymuszanie autoryzacji dla trwałego połączenia. Aby uzyskać ogólne informacje o integracji zabezpieczeń aplikacji SignalR...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: d559cfa21f6444b2361fd003b9ce3d2c9c6c57a4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042202"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Uwierzytelnianie i autoryzacja połączenia trwałego SignalR
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym temacie opisano wymuszanie autoryzacji dla trwałego połączenia. Aby uzyskać ogólne informacje na temat integracji zabezpieczeń w aplikacji SignalR, zobacz [wprowadzenie do zabezpieczeń](introduction-to-security.md). 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR w wersji 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
> 
> Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


## <a name="enforce-authorization"></a>Wymuszanie autoryzacji

Aby wymusić reguł autoryzacji, korzystając z [klasy PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) konieczne jest przesłonięcie `AuthorizeRequest` metody. Nie można użyć `Authorize` atrybut z połączeń trwałych. `AuthorizeRequest` Metoda jest wywoływana przez platformę SignalR przed każdym żądaniem, aby sprawdzić, czy użytkownik jest autoryzowany do wykonania żądanej akcji. `AuthorizeRequest` Metoda nie jest wywoływana z klienta; zamiast tego należy uwierzytelnić użytkownika za pomocą mechanizmu uwierzytelniania standardowych aplikacji.

W poniższym przykładzie przedstawiono sposób ogranicza liczby żądań, uwierzytelnionym użytkownikom.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Można dodać wszelka logika dostosowane autoryzacji w metodzie AuthorizeRequest; takich jak sprawdzanie, czy użytkownik należy do określonej roli.
