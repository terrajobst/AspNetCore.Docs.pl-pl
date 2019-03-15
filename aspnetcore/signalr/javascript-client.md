---
title: ASP.NET Core SignalR JavaScript klienta
author: bradygaster
description: Omówienie platformy ASP.NET Core SignalR JavaScript klienta.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 03/14/2019
uid: signalr/javascript-client
ms.openlocfilehash: a0980dca2eb8d483a9d9f1c5667fb74ee06364f0
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978345"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript klienta

Przez [Rachel Appel](http://twitter.com/rachelappel)

Biblioteki klienta platformy ASP.NET Core SignalR JavaScript umożliwia deweloperom wywołanie kodu koncentratora po stronie serwera.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Zainstaluj pakiet klienta SignalR

Biblioteka klienta SignalR JavaScript jest dostarczana jako [npm](https://www.npmjs.com/) pakietu. Jeśli używasz programu Visual Studio, uruchom `npm install` z **Konsola Menedżera pakietów** podczas gdy w folderze głównym. Dla programu Visual Studio Code, uruchom polecenie **zintegrowany Terminal**.

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm instaluje zawartość pakietu w *node_modules\\@aspnet\signalr\dist\browser* folderu. Utwórz nowy folder o nazwie *signalr* w obszarze *wwwroot\\lib* folderu. Kopiuj *signalr.js* plik *wwwroot\lib\signalr* folderu.

## <a name="use-the-signalr-javascript-client"></a>Używanie klienta SignalR JavaScript

Odwoływać się do klienta SignalR JavaScript w `<script>` elementu.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Połączenia z koncentratorem

Poniższy kod tworzy i uruchamia połączenie. Nazwa Centrum jest uwzględniana wielkość liter.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Połączenia między źródłami

Zazwyczaj przeglądarek ładowanie połączeń z tej samej domenie co żądanej strony. Istnieją jednak sytuacje, gdy wymagane jest połączenie do innej domeny.

Aby zapobiec złośliwych witryn odczytywanie poufnych danych z innej lokacji [połączeń cross-origin](xref:security/cors) są domyślnie wyłączone. Aby zezwolić na żądania między źródłami, włącz go w `Startup` klasy.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Wywoływanie metod koncentratora z klienta

Klientów JavaScript wywołania metod publicznych w centrach za pośrednictwem [wywołania](/javascript/api/%40aspnet/signalr/hubconnection#invoke) metody [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). `invoke` Metoda przyjmuje dwa argumenty:

* Nazwa metody koncentratora. W poniższym przykładzie jest nazwa metody koncentratora `SendMessage`.
* Argumenty zdefiniowane w metody koncentratora. W poniższym przykładzie jest nazwa argumentu `message`. Przykładowy kod używa Składnia funkcji strzałki, które jest obsługiwane w aktualnych wersjach wszystkich popularnych przeglądarkach, z wyjątkiem programu Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Jeśli używasz usługi Azure SignalR Service w *trybu bez użycia serwera*, nie można wywołać metody koncentratora klienta. Aby uzyskać więcej informacji, zobacz [dokumentacji usługi SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Wywoływanie metody klienta z Centrum

Aby odbierać komunikaty z Centrum, definiowanie metody przy użyciu [na](/javascript/api/%40aspnet/signalr/hubconnection#on) metody `HubConnection`.

* Nazwa metody klienta JavaScript. W poniższym przykładzie nazwa metody jest `ReceiveMessage`.
* Argumenty Centrum przekazuje do metody. W poniższym przykładzie wartość argumentu jest `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Powyższy kod w `connection.on` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metody.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

Określa SignalR, jakiej metody klienta do wywołania, dopasowując nazwy metody i argumenty zdefiniowane w `SendAsync` i `connection.on`.

> [!NOTE]
> Najlepszym rozwiązaniem, należy wywołać [start](/javascript/api/%40aspnet/signalr/hubconnection#start) metody `HubConnection` po `on`. Daje to pewność, że inne programy obsługi są rejestrowane, zanim jakiekolwiek komunikaty są odbierane.

## <a name="error-handling-and-logging"></a>Rejestrowanie i obsługa błędów

Łańcuch `catch` metody do końca `start` metodę, aby obsługiwać błędy po stronie klienta. Użyj `console.error` błędy dane wyjściowe do konsoli w przeglądarce.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Konfiguracja po stronie klienta dziennika śledzenia, przekazując rejestratora i typu zdarzenia logowania, gdy połączenie zostało nawiązane. Komunikaty są rejestrowane, z poziomu określonego dziennika lub nowszy. Poziomy dziennika dostępne są następujące:

* `signalR.LogLevel.Error` &ndash; Komunikaty o błędach. Dzienniki `Error` tylko komunikaty.
* `signalR.LogLevel.Warning` &ndash; Komunikaty ostrzegawcze o potencjalnych błędów. Dzienniki `Warning`, i `Error` wiadomości.
* `signalR.LogLevel.Information` &ndash; Komunikaty o stanie bez błędów. Dzienniki `Information`, `Warning`, i `Error` wiadomości.
* `signalR.LogLevel.Trace` &ndash; Komunikaty śledzenia. Rejestruje wszystkim, łącznie z danych przesyłanych między Centrum a klientem.

Użyj [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metody [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) skonfigurować poziom dziennika. Komunikaty są rejestrowane w konsoli przeglądarki.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Ponowne łączenie klientów

Automatyczne nie ponowne połączenia klienta JavaScript dla elementu SignalR. Należy napisać kod, który zostanie nawiązana ponownie ręcznie klienta. Poniższy kod przedstawia metody typowego ponowne nawiązanie połączenia:

1. Funkcji (w tym przypadku `start` funkcji) jest tworzony, aby rozpocząć połączenie.
1. Wywołaj `start` funkcji przez połączenie `onclose` programu obsługi zdarzeń.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Implementacja rzeczywistych będzie używać wycofywanie wykładnicze lub spróbuj ponownie określoną liczbę razy, przed rezygnacją. 

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja interfejsów API języka JavaScript](/javascript/api/?view=signalr-js-latest)
* [Samouczek języka JavaScript](xref:tutorials/signalr)
* [Samouczek dotyczący WebPack i TypeScript](xref:tutorials/signalr-typescript-webpack)
* [Centra](xref:signalr/hubs)
* [Klient .NET](xref:signalr/dotnet-client)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
* [Żądań Cross-Origin (CORS)](xref:security/cors)
* [Dokumentacja usługi Azure SignalR Service bez użycia serwera](/azure/azure-signalr/signalr-concept-serverless-development-config)
