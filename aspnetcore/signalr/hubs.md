---
title: Przy użyciu hubs w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać koncentratory w biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: 91f92e9d6b776457cd319965d548ee401ddc5e0e
ms.sourcegitcommit: 4225e2c49a0081e6ac15acff673587201f54b4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282146"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Na użytek koncentratory w SignalR platformy ASP.NET Core

Przez [Rachel Appel](https://twitter.com/rachelappel) i [Kevin Griffin](https://twitter.com/1kevgriff)

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Co to jest Centrum SignalR

Interfejsu API centrów SignalR umożliwia wywoływanie metod na komputerach klienckich połączonych z serwera. W kodzie serwera, można zdefiniować metody, które są wywoływane przez klienta. Kod klienta służy do definiowania metod, które są wywoływane z serwera. SignalR zajmuje się wszystkiego w tle, które sprawia, że w czasie rzeczywistym komunikacji klient serwer i klient serwera jest możliwe.

## <a name="configure-signalr-hubs"></a>Konfigurowanie centrów SignalR

Oprogramowanie pośredniczące SignalR wymaga pewnych usług, które zostały skonfigurowane przez wywołanie metody `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Podczas dodawania funkcji SignalR do aplikacji ASP.NET Core, należy skonfigurować trasy SignalR, wywołując `app.UseSignalR` w `Startup.Configure` metody.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Tworzenie i używanie koncentratory

Utwórz koncentrator od zadeklarowania klasy, która dziedziczy `Hub`i Dodaj metody publiczne do niego. Klienci mogą wywoływać metody, które są zdefiniowane jako `public`.

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

Można określić zwracany typ i parametry, w tym typy złożone i tablice, tak jak w dowolnej metody języka C#. SignalR obsługi serializacji i deserializacji obiektu złożonego obiekty i tablice parametrów i zwracanych wartości.

> [!NOTE]
> Centra są przejściowe:
> * Nie należy przechowywać stanu właściwością klasy koncentratora. Każde wywołanie metody koncentratora jest wykonywane na nowe wystąpienie koncentratora.  
> * Użyj `await` podczas wywoływania metod asynchronicznych, które są zależne od centrum pozostaje aktywne. Na przykład metoda takich jak `Clients.All.SendAsync(...)` może zakończyć się niepowodzeniem, jeśli jest wywoływana bez `await` i metody koncentratora zakończy się przed `SendAsync` zakończy się.

## <a name="the-context-object"></a>Obiekt kontekstu

`Hub` Klasa ma `Context` właściwość, która zawiera następujące właściwości z informacjami o połączeniu:

| Właściwość | Opis |
| ------ | ----------- |
| `ConnectionId` | Pobiera unikatowy identyfikator połączenia, przypisany przez SignalR. Istnieje jeden identyfikator połączenia dla każdego połączenia.|
| `UserIdentifier` | Pobiera [identyfikator użytkownika](xref:signalr/groups). Domyślnie używa SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzonych z tym połączeniem jako identyfikator użytkownika. |
| `User` | Pobiera `ClaimsPrincipal` skojarzone z bieżącego użytkownika. |
| `Items` | Pobiera kolekcję kluczy/wartości, który może służyć do udostępniania danych w zakresie tego połączenia. Dane mogą być przechowywane w tej kolekcji, a jej będzie zachowywane dla połączenia między różnych metod koncentratorów. |
| `Features` | Pobiera kolekcję funkcji dostępnych w ramach połączenia. Na razie ta kolekcja nie jest potrzebny w większości przypadków, więc nie jest on jeszcze opisane szczegółowo. |
| `ConnectionAborted` | Pobiera `CancellationToken` , otrzyma powiadomienie, gdy połączenie zostało przerwane. |

`Hub.Context` zawiera również następujące metody:

| Metoda | Opis |
| ------ | ----------- |
| `GetHttpContext` | Zwraca `HttpContext` dla połączenia lub `null` Jeśli połączenie nie jest skojarzony z żądaniem HTTP. W przypadku połączeń HTTP można użyć tej metody, aby uzyskać informacje, takie jak nagłówki HTTP i ciągi zapytań. |
| `Abort` | Przerywa połączenie. |

## <a name="the-clients-object"></a>Obiekt klientów

`Hub` Klasa ma `Clients` właściwość, która zawiera następujące właściwości dla komunikacji między serwerem a klientem:

| Właściwość | Opis |
| ------ | ----------- |
| `All` | Wywołuje metodę dla wszystkich podłączonych klientów |
| `Caller` | Wywołuje metodę dla klienta, który wywołał metodę koncentratora |
| `Others` | Wywołuje metodę dla wszyscy połączeni klienci oprócz klienta, który wywołał metodę |

`Hub.Clients` zawiera również następujące metody:

| Metoda | Opis |
| ------ | ----------- |
| `AllExcept` | Wywołuje metodę dla wszystkich podłączonych klientów z wyjątkiem wskazanych połączeń |
| `Client` | Wywołuje metodę dla określonego klienta połączone |
| `Clients` | Wywołuje metodę dla określonych klientów połączonych |
| `Group` | Wywołuje metodę dla wszystkich połączeń w określonej grupie  |
| `GroupExcept` | Wywołuje metodę dla wszystkich połączeń w określonej grupie, z wyjątkiem wskazanych połączeń |
| `Groups` | Wywołuje metodę dla wielu grup połączeń  |
| `OthersInGroup` | Wywołuje metodę dla grupy połączeń z wykluczeniem klienta, który wywołał metodę koncentratora  |
| `User` | Wywołuje metodę dla wszystkich połączeń, skojarzone z określonym użytkownikiem |
| `Users` | Wywołuje metodę dla wszystkich połączeń skojarzonych z określonych użytkowników |

Każda właściwość lub metoda w poprzednich tabelach zwraca obiekt z `SendAsync` metody. `SendAsync` Metoda umożliwia podanie nazwy i parametry metody klienta do wywołania.

## <a name="send-messages-to-clients"></a>Wysyłanie komunikatów do klientów

Aby wykonywać wywołania określonych klientów, należy użyć właściwości `Clients` obiektu. W poniższym przykładzie istnieją trzy metody Centrum:

* `SendMessage` wysyła wiadomość do wszystkich połączonych klientów przy użyciu `Clients.All`.
* `SendMessageToCaller` wysyła komunikat do obiektu wywołującego, za pomocą `Clients.Caller`.
* `SendMessageToGroups` wysyła wiadomość do wszystkich klientów w `SignalR Users` grupy.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a>Koncentratory silnie typizowane

Wadą korzystania z `SendAsync` jest, że opiera się na ciąg magiczny, aby określić metodę klienta do wywołania. Spowoduje to pozostawienie Otwórz kod błędów czasu wykonywania, jeśli nazwa metody została błędnie napisana lub brak od klienta.

Alternatywa dla użycia `SendAsync` polega na wpisaniu silnie `Hub` z <xref:Microsoft.AspNetCore.SignalR.Hub`1>. W poniższym przykładzie `ChatHub` metody klienta zostały wyodrębnione na zewnątrz do interfejs o nazwie `IChatClient`.  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

Ten interfejs może służyć do refaktoryzacji poprzednim `ChatHub` przykład.

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

Za pomocą `Hub<IChatClient>` umożliwia w czasie kompilacji sprawdzania metody klienta. Zapobiega to problem spowodował za pomocą magic ciągów, ponieważ `Hub<T>` tylko można zapewnić dostęp do metody zdefiniowane w interfejsie.

Za pomocą silnie typizowanej `Hub<T>` wyłącza możliwość używania `SendAsync`. Wszelkie metody zdefiniowane w interfejsie nadal można zdefiniować jako asynchroniczne. W rzeczywistości, każda z tych metod zwraca `Task`. Ponieważ jest to interfejs, nie używaj `async` — słowo kluczowe. Na przykład:

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> `Async` Sufiks nie jest usunięte z nazwy metody. Chyba, że metoda klienta jest zdefiniowana z `.on('MyMethodAsync')`, nie używaj `MyMethodAsync` jako nazwę.

## <a name="change-the-name-of-a-hub-method"></a>Zmień nazwę metody koncentratora

Domyślnie nazwa metody koncentratora serwera jest nazwa metody .NET. Można jednak użyć [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) atrybutu, aby zmienić to ustawienie domyślne i ręcznie określić nazwę metody. Klienta należy użyć tej nazwy zamiast nazwy metody .NET, podczas wywoływania metody.

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a>Obsługa zdarzeń dla połączenia

Interfejs API centrów SignalR udostępnia `OnConnectedAsync` i `OnDisconnectedAsync` metod wirtualnych, do zarządzania i śledzenia połączeń. Zastąp `OnConnectedAsync` metody wirtualnej do wykonania akcji, gdy klient nawiąże połączenie z koncentratorem, takie jak dodanie go do grupy.

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

Zastąp `OnDisconnectedAsync` metody wirtualnej do wykonania akcji w przypadku, gdy klient zakończy połączenie. Jeśli klient odłączy się celowo (przez wywołanie metody `connection.stop()`, na przykład), `exception` parametr będzie `null`. Jednakże, jeśli klient został odłączony z powodu błędu (np. awarii sieci), `exception` parametr będzie zawierać wyjątek zawierająca opis błędu.

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a>Obsługa błędów

Wyjątki zgłaszane w Twoich metodach koncentratora są wysyłane do klienta, który wywołał metodę. Na kliencie JavaScript `invoke` metoda zwraca [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Gdy klient odbierze błąd związany z programu obsługi dołączone do przy użyciu promise `catch`, ma ona wywoływana i przekazywane jako plik JavaScript `Error` obiektu.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

Jeśli koncentrator zgłasza wyjątek, połączenia nie są zamknięte. Domyślnie SignalR zwraca ogólny komunikat o błędzie do klienta. Na przykład:

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

Nieoczekiwane wyjątki często zawierają poufne informacje, takie jak nazwa serwera bazy danych, wyjątek wyzwalane, gdy połączenie z bazą danych nie powiedzie się. SignalR nie ujawnia te szczegółowe komunikaty o błędach domyślnie ze względów bezpieczeństwa. Zobacz [artykułu zagadnienia dotyczące zabezpieczeń](xref:signalr/security#exceptions) Aby uzyskać więcej informacji o tym, dlaczego szczegóły wyjątku są pomijane.

Jeśli masz wyjątkowych warunków możesz *czy* zostanie rozpropagowany do klienta, można użyć `HubException` klasy. Jeśli możesz zgłosić `HubException` ze swojej metody koncentratora SignalR **będzie** Wyślij cały komunikat do klienta bez modyfikacji.

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> Wysyła tylko SignalR `Message` właściwości wyjątku do klienta. Ślad stosu i inne właściwości w drodze wyjątku nie są dostępne dla klienta.

## <a name="related-resources"></a>Powiązane zasoby

* [Wprowadzenie do SignalR platformy ASP.NET Core](xref:signalr/introduction)
* [Klient JavaScript](xref:signalr/javascript-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
