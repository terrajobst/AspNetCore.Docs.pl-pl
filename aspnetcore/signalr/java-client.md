---
title: Klient Java ASP.NET Core SignalR
author: mikaelm12
description: Dowiedz się, jak używać klienta ASP.NET Core SignalR Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/java-client
ms.openlocfilehash: d7143b2c22ecdc4e68f484aa4c244e1c520beae0
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963781"
---
# <a name="aspnet-core-opno-locsignalr-java-client"></a>Klient Java ASP.NET Core SignalR

Autor [Mikael Mengistu](https://twitter.com/MikaelM_12)

Klient Java umożliwia łączenie się z serwerem SignalR ASP.NET Core w kodzie Java, w tym z aplikacjami dla systemu Android. Podobnie jak [klient JavaScript](xref:signalr/javascript-client) i [Klient platformy .NET](xref:signalr/dotnet-client), klient Java umożliwia odbieranie i wysyłanie komunikatów do centrum w czasie rzeczywistym. Klient Java jest dostępny w ASP.NET Core 2,2 i nowszych.

Przykładowa aplikacja konsoli Java, do której odwołuje się ten artykuł, używa klienta Java SignalR.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-java-client-package"></a>Zainstaluj pakiet klienta programu SignalR Java

Plik JAR *-1.0.0* , który umożliwia klientom łączenie się z centrami SignalR. Aby znaleźć najnowszy numer wersji pliku JAR, zobacz [wyniki wyszukiwania Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Jeśli używasz Gradle, Dodaj następujący wiersz do sekcji `dependencies` pliku *Build. Gradle* :

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

Jeśli używasz Maven, Dodaj następujące wiersze wewnątrz elementu `<dependencies>` pliku *pliku pom. XML* :

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Nawiązywanie połączenia z centrum

Aby nawiązać `HubConnection`, należy użyć `HubConnectionBuilder`. Podczas tworzenia połączenia można skonfigurować adres URL i poziom dziennika centrum. Skonfiguruj wszystkie wymagane opcje, wywołując dowolną metodę `HubConnectionBuilder` przed `build`. Rozpocznij połączenie z `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Wywoływanie metod centrów z klienta

Wywołanie `send` wywołuje metodę Hub. Przekaż nazwę metody Hub i wszystkie argumenty zdefiniowane w metodzie Hub, aby `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> Jeśli używasz usługi Azure SignalR w *trybie Bezserwerowym*, nie można wywoływać metod centralnych z poziomu klienta. Aby uzyskać więcej informacji, zobacz [dokumentację usługiSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Wywoływanie metod klienta z centrum

Użyj `hubConnection.on`, aby zdefiniować metody na kliencie, które mogą być wywoływane przez centrum. Zdefiniuj metody po kompilacji, ale przed rozpoczęciem połączenia.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Dodawanie rejestrowania

Klient języka Java SignalR używa biblioteki [SLF4J](https://www.slf4j.org/) do rejestrowania. Jest to interfejs API rejestrowania wysokiego poziomu, który umożliwia użytkownikom biblioteki wybranie własnych implementacji rejestrowania przez nałożenie określonej zależności rejestrowania. Poniższy fragment kodu przedstawia sposób korzystania z `java.util.logging` z klientem SignalR Java.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Jeśli nie skonfigurujesz rejestrowania w zależnościach, SLF4J ładuje domyślny Rejestrator braku operacji z następującym komunikatem ostrzegawczym:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Można je bezpiecznie zignorować.

## <a name="android-development-notes"></a>Uwagi dotyczące programowania dla systemu Android

W odniesieniu do Android SDK zgodności dla funkcji klienta SignalR należy wziąć pod uwagę następujące elementy podczas określania docelowej wersji Android SDK:

* Klient języka Java SignalR będzie uruchamiany na poziomie interfejsu API systemu Android w wersji 16 lub nowszej.
* Połączenie za pośrednictwem usługi Azure SignalR wymaga poziomu interfejsu API systemu Android 20 lub nowszego, ponieważ [usługa SignalR platformy Azure](/azure/azure-signalr/signalr-overview) wymaga protokołu TLS 1,2 i nie obsługuje mechanizmów szyfrowania opartych na algorytmie SHA-1. [Dodano obsługę szyfrowania SHA-256 (i powyżej)](https://developer.android.com/reference/javax/net/ssl/SSLSocket) w systemie Android na poziomie interfejsu API 20.

## <a name="configure-bearer-token-authentication"></a>Konfigurowanie uwierzytelniania tokenów okaziciela

W kliencie SignalR Java można skonfigurować token okaziciela do użycia na potrzeby uwierzytelniania, dostarczając "fabrykę tokenów dostępu" do [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) , aby podać [](https://github.com/ReactiveX/RxJava) [\<pojedynczego ciągu](https://reactivex.io/documentation/single.html). Wywołanie metody [Single. Ustąp](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)umożliwia zapisanie logiki w celu utworzenia tokenów dostępu dla klienta.

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>Znane ograniczenia

::: moniker range=">= aspnetcore-3.0"

* Obsługiwany jest tylko protokół JSON.
* Transport zdarzeń powrotu i przesyłania do serwera nie jest obsługiwany.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Obsługiwany jest tylko protokół JSON.
* Obsługiwany jest tylko transport gniazd WebSockets.
* Przesyłanie strumieniowe nie jest jeszcze obsługiwane.

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja interfejsów API języka Java](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [Dokumentacja bezserwerowa usługi SignalR platformy Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)
