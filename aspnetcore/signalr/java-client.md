---
title: Klienta programu ASP.NET Core SignalR w języku Java
author: mikaelm12
description: Dowiedz się, jak używać klienta platformy ASP.NET Core SignalR w języku Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 06/27/2019
uid: signalr/java-client
ms.openlocfilehash: eea1dfb7d8afcd34c0dacd8315ad196d7235c9f7
ms.sourcegitcommit: 6d9cf728465cdb0de1037633a8b7df9a8989cccb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67463270"
---
# <a name="aspnet-core-signalr-java-client"></a>Klienta programu ASP.NET Core SignalR w języku Java

Przez [Mikael Mengistu](https://twitter.com/MikaelM_12)

Klienta Java umożliwia połączenie z serwerem biblioteki SignalR platformy ASP.NET Core z kodu Java, w tym aplikacje dla systemu Android. Podobnie jak [klienta JavaScript](xref:signalr/javascript-client) i [klient modelu .NET](xref:signalr/dotnet-client), klienta Java umożliwia odbieranie i wysyłanie komunikatów do Centrum w czasie rzeczywistym. Klienta Java jest dostępne w programie ASP.NET Core 2.2 lub nowszej.

Przykładowa aplikacja konsoli środowiska Java przywołanej w niniejszym artykule używa klienta SignalR Java.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Zainstaluj pakiet klienta SignalR Java

*Signalr 1.0.0* plik JAR zezwala klientom na łączenie się koncentratorów SignalR. Aby znaleźć numer najnowszej wersji pliku JAR, zobacz [wyniki wyszukiwania narzędzia Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Jeśli używasz narzędzia Gradle, Dodaj następujący wiersz do `dependencies` części Twojej *build.gradle* pliku:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

Jeśli przy użyciu narzędzia Maven, Dodaj następujące wiersze wewnątrz `<dependencies>` elementu swoje *pom.xml* pliku:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Połączenia z koncentratorem

Aby ustanowić `HubConnection`, `HubConnectionBuilder` powinny być używane. Poziom adresu URL i dziennika Centrum można skonfigurować podczas tworzenia połączenia. Skonfiguruj wymagane opcje, wywołując jedną z `HubConnectionBuilder` przed `build`. Uruchom połączenie przy użyciu `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Wywoływanie metod koncentratora z klienta

Wywołanie `send` wywołuje metodę koncentratora. Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> Jeśli używasz usługi Azure SignalR Service w *trybu bez użycia serwera*, nie można wywołać metody koncentratora klienta. Aby uzyskać więcej informacji, zobacz [dokumentacji usługi SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

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

## <a name="android-development-notes"></a>Uwagi dotyczące opracowywania aplikacji systemu android

W odniesieniu do zestawu SDK systemu Android zgodności dla funkcji klienta SignalR należy wziąć pod uwagę następujące elementy podczas określania wersji zestawu SDK systemu Android docelowej:

* Klienta Java SignalR zostanie uruchomiony na Android API 16 poziom lub nowszy.
* Łącząc się za pośrednictwem usługi Azure SignalR Service będzie wymagać systemu Android poziom interfejsu API 20 i nowsze ponieważ [usługi Azure SignalR Service](/azure/azure-signalr/signalr-overview) wymaga protokołu TLS 1.2, a nie obsługuje mechanizmów szyfrowania opartego na SHA-1. Android [dodano mechanizmy szyfrowania pomocy technicznej dla algorytmu SHA-256 (i nowszych)](https://developer.android.com/reference/javax/net/ssl/SSLSocket) w poziom interfejsu API 20.

## <a name="configure-bearer-token-authentication"></a>Konfigurowanie uwierzytelniania tokenu elementu nośnego

W kliencie SignalR Java, można skonfigurować token elementu nośnego do użycia na potrzeby uwierzytelniania, zapewniając "dostęp do tokenu fabryka" w celu [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) zapewnienie [RxJava](https://github.com/ReactiveX/RxJava) [pojedynczego\<ciągu >](http://reactivex.io/documentation/single.html). W wyniku wywołania [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), można napisać logikę do tworzenia tokenów dostępu klienta.

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>Znane ograniczenia

::: moniker range=">= aspnetcore-3.0"

* Tylko protokół JSON jest obsługiwany.
* Transportu rezerwowy oraz transportu zdarzenia wysyłane serwera nie są obsługiwane.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Tylko protokół JSON jest obsługiwany.
* Tylko transport gniazda Websocket jest obsługiwany.
* Przesyłanie strumieniowe nie jest jeszcze obsługiwane.

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja interfejsów API języka Java](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [Dokumentacja usługi Azure SignalR Service bez użycia serwera](/azure/azure-signalr/signalr-concept-serverless-development-config)
