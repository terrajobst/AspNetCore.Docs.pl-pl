---
title: Klienta programu ASP.NET Core SignalR w języku Java
author: mikaelm12
description: Dowiedz się, jak używać klienta platformy ASP.NET Core SignalR w języku Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: 0eba59a05ea6fd3fed46fcab86ac20caf40ebb65
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482921"
---
# <a name="aspnet-core-signalr-java-client"></a>Klienta SignalR Java platformy ASP.NET Core

Przez [Mikael Mengistu](https://twitter.com/MikaelM_12)

Klienta Java umożliwia połączenie z serwerem biblioteki SignalR platformy ASP.NET Core z kodu Java, w tym aplikacje dla systemu Android. Podobnie jak [klienta JavaScript](xref:signalr/javascript-client) i [klient modelu .NET](xref:signalr/dotnet-client), klienta Java umożliwia odbieranie i wysyłanie komunikatów do Centrum w czasie rzeczywistym. Klienta Java jest dostępne w programie ASP.NET Core 2.2 lub nowszej.

Przykładowa aplikacja konsoli środowiska Java przywołanej w niniejszym artykule używa klienta SignalR Java.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Zainstaluj pakiet klienta SignalR Java

*Signalr-0.1.0-preview2-35174* plik JAR zezwala klientom na łączenie się koncentratorów SignalR. Aby znaleźć numer najnowszej wersji pliku JAR, zobacz [wyniki wyszukiwania narzędzia Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).

Jeśli używasz narzędzia Gradle, Dodaj następujący wiersz do `dependencies` części Twojej *build.gradle* pliku:

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

Jeśli przy użyciu narzędzia Maven, Dodaj następujące wiersze wewnątrz `<dependencies>` elementu swoje *pom.xml* pliku:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Połączenia z koncentratorem

Aby ustanowić `HubConnection`, `HubConnectionBuilder` powinny być używane. Poziom adresu URL i dziennika Centrum można skonfigurować podczas tworzenia połączenia. Skonfiguruj wymagane opcje, wywołując jedną z `HubConnectionBuilder` przed `build`. Uruchom połączenie przy użyciu `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a>Wywoływanie metod koncentratora z klienta

Wywołanie `send` wywołuje metodę koncentratora. Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a>Wywoływanie metody klienta z Centrum

Użyj `hubConnection.on` do definiowania metod na komputerze klienckim, który można wywoływać koncentratora. Określ metody po kompilacji, ale przed rozpoczęciem połączenia.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a>Znane ograniczenia

Jest to wczesna wersja zapoznawcza klienta Java. Istnieje wiele funkcji, które nie są jeszcze obsługiwane. Następujące luki pracuje w przyszłych wersjach:

* Tylko typy pierwotne, mogą być akceptowane jako parametrów i zwracanych typów.
* Interfejsy API są synchroniczne.
* Typ połączenia "Send" jest obsługiwana w tej chwili. "Wywołaj" i przesyłania strumieniowego wartości zwracane nie są obsługiwane.
* Tylko protokół JSON jest obsługiwany.
* Tylko transport gniazda Websocket jest obsługiwany.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja interfejsu API języka Java](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
