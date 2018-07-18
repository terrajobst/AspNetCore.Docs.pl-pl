---
title: Przy użyciu hubs w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać koncentratory w biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: be39666373e2b099054bb71f4a7fcf17aeb9a01c
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095284"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Na użytek koncentratory w SignalR platformy ASP.NET Core

Przez [Rachel Appel](https://twitter.com/rachelappel) i [Kevin Griffin](https://twitter.com/1kevgriff)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(jak pobrać)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Co to jest Centrum SignalR

Interfejsu API centrów SignalR umożliwia wywoływanie metod na komputerach klienckich połączonych z serwera. W kodzie serwera, można zdefiniować metody, które są wywoływane przez klienta. Kod klienta służy do definiowania metod, które są wywoływane z serwera. SignalR zajmuje się wszystkiego w tle, które sprawia, że w czasie rzeczywistym komunikacji klient serwer i klient serwera jest możliwe.

## <a name="configure-signalr-hubs"></a>Konfigurowanie centrów SignalR

Oprogramowanie pośredniczące SignalR wymaga pewnych usług, które zostały skonfigurowane przez wywołanie metody `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Podczas dodawania funkcji SignalR do aplikacji ASP.NET Core, należy skonfigurować trasy SignalR, wywołując `app.UseSignalR` w `Startup.Configure` metody.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Tworzenie i używanie koncentratory

Utwórz koncentrator od zadeklarowania klasy, która dziedziczy `Hub`i Dodaj metody publiczne do niego. Klienci mogą wywoływać metody, które są zdefiniowane jako `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Można określić zwracany typ i parametry, w tym typy złożone i tablice, tak jak w dowolnej metody języka C#. SignalR obsługi serializacji i deserializacji obiektu złożonego obiekty i tablice parametrów i zwracanych wartości.

## <a name="the-clients-object"></a>Obiekt klientów

Każde wystąpienie `Hub` klasa ma właściwość o nazwie `Clients` zawiera następujące elementy członkowskie na potrzeby komunikacji między serwerem a klientem:

| Właściwość | Opis |
| ------ | ----------- |
| `All` | Wywołuje metodę dla wszystkich podłączonych klientów |
| `Caller` | Wywołuje metodę dla klienta, który wywołał metodę koncentratora |
| `Others` | Wywołuje metodę dla wszyscy połączeni klienci oprócz klienta, który wywołał metodę |


Ponadto `Hub.Clients` zawiera następujące metody:

| Metoda | Opis |
| ------ | ----------- |
| `AllExcept` | Wywołuje metodę dla wszystkich podłączonych klientów z wyjątkiem wskazanych połączeń |
| `Client` | Wywołuje metodę dla określonego klienta połączone |
| `Clients` | Wywołuje metodę dla określonych klientów połączonych |
| `Group` | Wywołuje metodę do wszystkich połączeń w określonej grupie  |
| `GroupExcept` | Wywołuje metodę do wszystkich połączeń w określonej grupie, z wyjątkiem wskazanych połączeń |
| `Groups` | Wywołuje metodę do wielu grup połączeń  |
| `OthersInGroup` | Wywołuje metodę do grupy połączeń z wykluczeniem klienta, który wywołał metodę koncentratora  |
| `User` | Wywołuje metodę, aby wszystkie połączenia skojarzone z określonym użytkownikiem |
| `Users` | Wywołuje metodę, aby wszystkie połączenia skojarzone z określonych użytkowników |

Każda właściwość lub metoda w poprzednich tabelach zwraca obiekt z `SendAsync` metody. `SendAsync` Metoda umożliwia podanie nazwy i parametry metody klienta do wywołania.

## <a name="send-messages-to-clients"></a>Wysyłanie komunikatów do klientów

Aby wykonywać wywołania określonych klientów, należy użyć właściwości `Clients` obiektu. W poniższym przykładzie `SendMessageToCaller` metoda pokazuje wysyłania komunikatu do połączenia, który wywołał metodę koncentratora. `SendMessageToGroups` Metoda wysyła komunikat do grup, przechowywane w `List` o nazwie `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Obsługa zdarzeń dla połączenia

Interfejs API centrów SignalR udostępnia `OnConnectedAsync` i `OnDisconnectedAsync` metod wirtualnych, do zarządzania i śledzenia połączeń. Zastąp `OnConnectedAsync` metody wirtualnej do wykonania akcji, gdy klient nawiąże połączenie z koncentratorem, takie jak dodanie go do grupy.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Obsługa błędów

Wyjątki zgłaszane w Twoich metodach koncentratora są wysyłane do klienta, który wywołał metodę. Na kliencie JavaScript `invoke` metoda zwraca [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Gdy klient odbierze błąd związany z programu obsługi dołączone do przy użyciu promise `catch`, ma ona wywoływana i przekazywane jako plik JavaScript `Error` obiektu.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Powiązane zasoby

* [Wprowadzenie do SignalR platformy ASP.NET Core](xref:signalr/introduction)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
