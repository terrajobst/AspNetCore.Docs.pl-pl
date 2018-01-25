---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: "Uwierzytelnianie i autoryzacja połączenia trwałego SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft"
author: pfletcher
description: "W tym temacie opisano wymuszanie autoryzacji dla trwałego połączenia. Aby uzyskać ogólne informacje o integracji zabezpieczeń aplikacji SignalR..."
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
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Uwierzytelnianie i autoryzacja połączenia trwałego SignalR (SignalR 1.x)
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym temacie opisano wymuszanie autoryzacji dla trwałego połączenia. Aby uzyskać ogólne informacje na temat integracji zabezpieczeń w aplikacji SignalR, zobacz [wprowadzenie do zabezpieczeń](index.md).


## <a name="enforce-authorization"></a>Wymuszanie autoryzacji

Aby wymusić reguł autoryzacji, korzystając z [klasy PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) konieczne jest przesłonięcie `AuthorizeRequest` metody. Nie można użyć `Authorize` atrybut z połączeń trwałych. `AuthorizeRequest` Metoda jest wywoływana przez platformę SignalR przed każdym żądaniem, aby sprawdzić, czy użytkownik jest autoryzowany do wykonania żądanej akcji. `AuthorizeRequest` Metoda nie jest wywoływana z klienta; zamiast tego należy uwierzytelnić użytkownika za pomocą mechanizmu uwierzytelniania standardowych aplikacji.

W poniższym przykładzie przedstawiono sposób ogranicza liczby żądań, uwierzytelnionym użytkownikom.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Można dodać wszelka logika dostosowane autoryzacji w metodzie AuthorizeRequest; takich jak sprawdzanie, czy użytkownik należy do określonej roli.
