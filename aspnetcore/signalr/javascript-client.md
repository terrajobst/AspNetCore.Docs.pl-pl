---
title: Program ASP.NET Core SignalR JavaScript klienta
author: rachelappel
description: Przegląd klienta platformy ASP.NET Core SignalR JavaScript.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: 6ff888d3337bb53d435744009f4cc24b327ebcda
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341941"
---
# <a name="aspnet-core-signalr-javascript-client"></a>Program ASP.NET Core SignalR JavaScript klienta

Przez [Rachel Appel](http://twitter.com/rachelappel)

Biblioteka klienta platformy ASP.NET Core SignalR JavaScript umożliwia deweloperom wywołanie kodu koncentratora po stronie serwera.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Zainstaluj pakiet klienta SignalR

Biblioteka klienta SignalR JavaScript jest dostarczana jako [npm](https://www.npmjs.com/) pakietu. Jeśli używasz programu Visual Studio, uruchomić `npm install` z **Konsola Menedżera pakietów** while w folderze głównym. Dla programu Visual Studio Code, uruchom polecenie z **zintegrowane Terminal**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Npm instaluje zawartość pakietu w *node_modules\\ @aspnet\signalr\dist\browser*  folderu. Utwórz nowy folder o nazwie *signalr* w obszarze *wwwroot\\lib* folderu. Kopiuj *signalr.js* pliku *wwwroot\lib\signalr* folderu.

## <a name="use-the-signalr-javascript-client"></a>Korzystanie z klienta SignalR JavaScript

Odwołanie klienta SignalR JavaScript w `<script>` elementu.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Połączenia z koncentratorem

Poniższy kod tworzy i uruchamia połączenie. Nazwa koncentratora jest uwzględniana wielkość liter.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Połączenia między źródłami

Zazwyczaj przeglądarki załadować połączeń z tej samej domenie co żądanej strony. Istnieją jednak sytuacji, gdy wymagane jest połączenie z innej domeny.

Aby zapobiec złośliwa witryna odczytywanie danych poufnych z innej lokacji, [połączeń między źródłami](xref:security/cors) są domyślnie wyłączone. Aby zezwolić na żądanie między źródłami, włączyć go w `Startup` klasy.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Wywoływanie metod koncentratora od klienta

Klientów języka JavaScript wywołanie metody publiczne na koncentratorów przez przy użyciu `connection.invoke`. `invoke` Metoda przyjmuje dwa argumenty:

* Nazwa metody koncentratora. W poniższym przykładzie nazwa Centrum jest `SendMessage`.
* Żadnych argumentów w metodzie koncentratora. W poniższym przykładzie jest nazwa argumentu `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Wywoływanie metody klienta z Centrum

Aby odbierać komunikaty z koncentratora, zdefiniować przy użyciu metody `connection.on` metody.

* Nazwa metody JavaScript klienta. W poniższym przykładzie nazwa metody jest `ReceiveMessage`.
* Argumenty koncentratora przekazuje do metody. W poniższym przykładzie jest wartość argumentu `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Poprzedni kod w `connection.on` jest uruchamiany, gdy kod po stronie serwera wywołuje za pomocą `SendAsync` metody.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

Określa, która metoda klienta do wywołania przez dopasowanie nazwy metody SignalR i argumenty zdefiniowanych w `SendAsync` i `connection.on`.

> [!NOTE]
> Jako najlepsze rozwiązanie należy wywołać `connection.start` po `connection.on` tak programu obsługi są zarejestrowane przed komunikaty są odbierane.

## <a name="error-handling-and-logging"></a>Obsługa błędów i rejestrowanie

Łańcuch `catch` metody na końcu `connection.start` sposób obsługi błędów po stronie klienta. Użyj `console.error` błędy dane wyjściowe do konsoli przeglądarki.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Konfiguracja po stronie klienta dziennika śledzenia przez przekazanie rejestratora i typ zdarzenia logowania po nawiązaniu połączenia. Komunikaty są rejestrowane na poziomie określonego dziennika i wyższych. Poziomy rejestrowania dostępne są następujące:

* `signalR.LogLevel.Error` : Komunikaty wystąpił błąd. Dzienniki `Error` tylko komunikaty.
* `signalR.LogLevel.Warning` : Ostrzeżenie komunikaty o potencjalnych błędów. Dzienniki `Warning`, i `Error` wiadomości.
* `signalR.LogLevel.Information` : Komunikaty o stanie bez błędów. Dzienniki `Information`, `Warning`, i `Error` wiadomości.
* `signalR.LogLevel.Trace` : Śledzenia wiadomości. Rejestruje wszystkim łącznie z danych przesyłanych między klientem koncentratora.

Użyj `configureLogging` metoda `HubConnectionBuilder` skonfigurować poziom dziennika. Komunikaty są rejestrowane w konsoli przeglądarki.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>Zasoby pokrewne

* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
* [Włącz żądania między źródłami (CORS) w platformy ASP.NET Core](xref:security/cors)