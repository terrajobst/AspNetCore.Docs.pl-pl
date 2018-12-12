---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Uwierzytelnianie i autoryzacja połączeń trwałych SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano sposób wymusić autoryzację dla trwałego połączenia. Aby uzyskać ogólne informacje na temat integracji zabezpieczeń do aplikacji SignalR...
ms.author: riande
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 4973cb9bd03088106fe9502e0e706c6c28d2d46d
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53286860"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Uwierzytelnianie i autoryzacja połączeń trwałych usługi SignalR (SignalR 1.x)
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym temacie opisano sposób wymusić autoryzację dla trwałego połączenia. Aby uzyskać ogólne informacje na temat integracji zabezpieczeń do aplikacji SignalR zobacz [wprowadzenie do zabezpieczeń](index.md).


## <a name="enforce-authorization"></a>Wymuszanie autoryzacji

Aby wymusić reguł autoryzacji, korzystając z [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) konieczne jest przesłonięcie `AuthorizeRequest` metody. Nie można użyć `Authorize` atrybutu z połączeń trwałych. `AuthorizeRequest` Metoda jest wywoływana przez SignalR Framework przed każdym żądaniem, aby sprawdzić, czy użytkownik jest autoryzowany do wykonania żądanej akcji. `AuthorizeRequest` Metoda nie jest wywoływana przez klienta; zamiast tego należy uwierzytelnić użytkownika za pośrednictwem mechanizmu uwierzytelniania standardowego w Twojej aplikacji.

W poniższym przykładzie pokazano sposób ograniczania żądań do uwierzytelnionych użytkowników.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Można dodać wszelka logika autoryzacji dostosowanego w metodzie AuthorizeRequest; takich jak sprawdzanie, czy użytkownik należy do określonej roli.
