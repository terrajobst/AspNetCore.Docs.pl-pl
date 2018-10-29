---
title: Klienta programu ASP.NET Core SignalR w języku Java
author: mikaelm12
description: Dowiedz się, jak używać klienta platformy ASP.NET Core SignalR w języku Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 646118c78d5d38b44b89d399cd06a5332a11d064
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207774"
---
# <a name="aspnet-core-signalr-java-client"></a>Klienta programu ASP.NET Core SignalR w języku Java

Przez [Mikael Mengistu](https://twitter.com/MikaelM_12)

Klienta Java umożliwia połączenie z serwerem biblioteki SignalR platformy ASP.NET Core z kodu Java, w tym aplikacje dla systemu Android. Podobnie jak [klienta JavaScript](xref:signalr/javascript-client) i [klient modelu .NET](xref:signalr/dotnet-client), klienta Java umożliwia odbieranie i wysyłanie komunikatów do Centrum w czasie rzeczywistym. Klienta Java jest dostępne w programie ASP.NET Core 2.2 lub nowszej.

Przykładowa aplikacja konsoli środowiska Java przywołanej w niniejszym artykule używa klienta SignalR Java.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Zainstaluj pakiet klienta SignalR Java

*Signalr-1.0.0-preview3-35501* plik JAR zezwala klientom na łączenie się koncentratorów SignalR. Aby znaleźć numer najnowszej wersji pliku JAR, zobacz [wyniki wyszukiwania narzędzia Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Jeśli używasz narzędzia Gradle, Dodaj następujący wiersz do `dependencies` części Twojej *build.gradle* pliku:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

Jeśli przy użyciu narzędzia Maven, Dodaj następujące wiersze wewnątrz `<dependencies>` elementu swoje *pom.xml* pliku:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Połączenia z koncentratorem

Aby ustanowić `HubConnection`, `HubConnectionBuilder` powinny być używane. Poziom adresu URL i dziennika Centrum można skonfigurować podczas tworzenia połączenia. Skonfiguruj wymagane opcje, wywołując jedną z `HubConnectionBuilder` przed `build`. Uruchom połączenie przy użyciu `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Wywoływanie metod koncentratora z klienta

Wywołanie `send` wywołuje metodę koncentratora. Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a>Wywoływanie metody klienta z Centrum

Użyj `hubConnection.on` do definiowania metod na komputerze klienckim, który można wywoływać koncentratora. Określ metody po kompilacji, ale przed rozpoczęciem połączenia.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Dodawanie rejestrowania

Klient SignalR Java używa [SLF4J](https://www.slf4j.org/) biblioteki do rejestrowania. To API wysokiego poziomu rejestrowania, który umożliwia użytkownikom Biblioteka wybrana zapewniali własną implementację określonych rejestrowania, przenosząc powstanie zależności określonych rejestrowania. Poniższy fragment kodu przedstawia sposób użycia `java.util.logging` za pomocą klienta SignalR Java.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Jeśli nie skonfigurowano rejestrowanie, w zależności, SLF4J ładuje domyślny Rejestrator nie operacji następujący komunikat ostrzegawczy:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

To można bezpiecznie zignorować.

## <a name="known-limitations"></a>Znane ograniczenia

Jest to wersja zapoznawcza klienta Java. Niektóre funkcje nie są obsługiwane:

* Tylko protokół JSON jest obsługiwany.
* Tylko transport gniazda Websocket jest obsługiwany.
* Przesyłanie strumieniowe nie jest jeszcze obsługiwane.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja interfejsów API języka Java](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
