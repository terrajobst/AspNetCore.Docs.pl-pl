---
title: Klienta programu ASP.NET Core SignalR w języku Java
author: mikaelm12
description: Dowiedz się, jak używać klienta platformy ASP.NET Core SignalR w języku Java.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 03/14/2019
uid: signalr/java-client
ms.openlocfilehash: 53055b2642cae7640ae79cb5ae88ad6b2714c689
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209687"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="4d7af-103">Klienta programu ASP.NET Core SignalR w języku Java</span><span class="sxs-lookup"><span data-stu-id="4d7af-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="4d7af-104">Przez [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="4d7af-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="4d7af-105">Klienta Java umożliwia połączenie z serwerem biblioteki SignalR platformy ASP.NET Core z kodu Java, w tym aplikacje dla systemu Android.</span><span class="sxs-lookup"><span data-stu-id="4d7af-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="4d7af-106">Podobnie jak [klienta JavaScript](xref:signalr/javascript-client) i [klient modelu .NET](xref:signalr/dotnet-client), klienta Java umożliwia odbieranie i wysyłanie komunikatów do Centrum w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="4d7af-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="4d7af-107">Klienta Java jest dostępne w programie ASP.NET Core 2.2 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="4d7af-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="4d7af-108">Przykładowa aplikacja konsoli środowiska Java przywołanej w niniejszym artykule używa klienta SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="4d7af-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="4d7af-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4d7af-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="4d7af-110">Zainstaluj pakiet klienta SignalR Java</span><span class="sxs-lookup"><span data-stu-id="4d7af-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="4d7af-111">*Signalr 1.0.0* plik JAR zezwala klientom na łączenie się koncentratorów SignalR.</span><span class="sxs-lookup"><span data-stu-id="4d7af-111">The *signalr-1.0.0* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="4d7af-112">Aby znaleźć numer najnowszej wersji pliku JAR, zobacz [wyniki wyszukiwania narzędzia Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="4d7af-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="4d7af-113">Jeśli używasz narzędzia Gradle, Dodaj następujący wiersz do `dependencies` części Twojej *build.gradle* pliku:</span><span class="sxs-lookup"><span data-stu-id="4d7af-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

<span data-ttu-id="4d7af-114">Jeśli przy użyciu narzędzia Maven, Dodaj następujące wiersze wewnątrz `<dependencies>` elementu swoje *pom.xml* pliku:</span><span class="sxs-lookup"><span data-stu-id="4d7af-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="4d7af-115">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="4d7af-115">Connect to a hub</span></span>

<span data-ttu-id="4d7af-116">Aby ustanowić `HubConnection`, `HubConnectionBuilder` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="4d7af-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="4d7af-117">Poziom adresu URL i dziennika Centrum można skonfigurować podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="4d7af-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="4d7af-118">Skonfiguruj wymagane opcje, wywołując jedną z `HubConnectionBuilder` przed `build`.</span><span class="sxs-lookup"><span data-stu-id="4d7af-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="4d7af-119">Uruchom połączenie przy użyciu `start`.</span><span class="sxs-lookup"><span data-stu-id="4d7af-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="4d7af-120">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="4d7af-120">Call hub methods from client</span></span>

<span data-ttu-id="4d7af-121">Wywołanie `send` wywołuje metodę koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4d7af-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="4d7af-122">Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `send`.</span><span class="sxs-lookup"><span data-stu-id="4d7af-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> <span data-ttu-id="4d7af-123">Jeśli używasz usługi Azure SignalR Service w *trybu bez użycia serwera*, nie można wywołać metody koncentratora klienta.</span><span class="sxs-lookup"><span data-stu-id="4d7af-123">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="4d7af-124">Aby uzyskać więcej informacji, zobacz [dokumentacji usługi SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="4d7af-124">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="4d7af-125">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="4d7af-125">Call client methods from hub</span></span>

<span data-ttu-id="4d7af-126">Użyj `hubConnection.on` do definiowania metod na komputerze klienckim, który można wywoływać koncentratora.</span><span class="sxs-lookup"><span data-stu-id="4d7af-126">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="4d7af-127">Określ metody po kompilacji, ale przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="4d7af-127">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="4d7af-128">Dodawanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="4d7af-128">Add logging</span></span>

<span data-ttu-id="4d7af-129">Klient SignalR Java używa [SLF4J](https://www.slf4j.org/) biblioteki do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="4d7af-129">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="4d7af-130">To API wysokiego poziomu rejestrowania, który umożliwia użytkownikom Biblioteka wybrana zapewniali własną implementację określonych rejestrowania, przenosząc powstanie zależności określonych rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="4d7af-130">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="4d7af-131">Poniższy fragment kodu przedstawia sposób użycia `java.util.logging` za pomocą klienta SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="4d7af-131">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="4d7af-132">Jeśli nie skonfigurowano rejestrowanie, w zależności, SLF4J ładuje domyślny Rejestrator nie operacji następujący komunikat ostrzegawczy:</span><span class="sxs-lookup"><span data-stu-id="4d7af-132">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="4d7af-133">To można bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="4d7af-133">This can safely be ignored.</span></span>

## <a name="android-development-notes"></a><span data-ttu-id="4d7af-134">Uwagi dotyczące opracowywania aplikacji systemu android</span><span class="sxs-lookup"><span data-stu-id="4d7af-134">Android development notes</span></span>

<span data-ttu-id="4d7af-135">W odniesieniu do zestawu SDK systemu Android zgodności dla funkcji klienta SignalR należy wziąć pod uwagę następujące elementy podczas określania wersji zestawu SDK systemu Android docelowej:</span><span class="sxs-lookup"><span data-stu-id="4d7af-135">With regards to Android SDK compatibility for the SignalR client features, consider the following items when specifying your target Android SDK version:</span></span>

* <span data-ttu-id="4d7af-136">Klienta Java SignalR zostanie uruchomiony na Android API 16 poziom lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="4d7af-136">The SignalR Java Client will run on Android API Level 16 and later.</span></span>
* <span data-ttu-id="4d7af-137">Łącząc się za pośrednictwem usługi Azure SignalR Service będzie wymagać systemu Android poziom interfejsu API 20 i nowsze ponieważ [usługi Azure SignalR Service](/azure/azure-signalr/signalr-overview) wymaga protokołu TLS 1.2, a nie obsługuje mechanizmów szyfrowania opartego na SHA-1.</span><span class="sxs-lookup"><span data-stu-id="4d7af-137">Connecting through the Azure SignalR Service will require Android API Level 20 and later because the [Azure SignalR Service](/azure/azure-signalr/signalr-overview) requires TLS 1.2 and doesn't support SHA-1-based cipher suites.</span></span> <span data-ttu-id="4d7af-138">Android [dodano mechanizmy szyfrowania pomocy technicznej dla algorytmu SHA-256 (i nowszych)](https://developer.android.com/reference/javax/net/ssl/SSLSocket) w poziom interfejsu API 20.</span><span class="sxs-lookup"><span data-stu-id="4d7af-138">Android [added support for SHA-256 (and above) cipher suites](https://developer.android.com/reference/javax/net/ssl/SSLSocket) in API Level 20.</span></span>

## <a name="configure-bearer-token-authentication"></a><span data-ttu-id="4d7af-139">Konfigurowanie uwierzytelniania tokenu elementu nośnego</span><span class="sxs-lookup"><span data-stu-id="4d7af-139">Configure bearer token authentication</span></span>

<span data-ttu-id="4d7af-140">W kliencie SignalR Java, można skonfigurować token elementu nośnego do użycia na potrzeby uwierzytelniania, zapewniając "dostęp do tokenu fabryka" w celu [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="4d7af-140">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an "access token factory" to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="4d7af-141">Użyj [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) zapewnienie [RxJava](https://github.com/ReactiveX/RxJava) [pojedynczego\<ciągu >](http://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="4d7af-141">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="4d7af-142">W wyniku wywołania [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), można napisać logikę do tworzenia tokenów dostępu klienta.</span><span class="sxs-lookup"><span data-stu-id="4d7af-142">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a><span data-ttu-id="4d7af-143">Znane ograniczenia</span><span class="sxs-lookup"><span data-stu-id="4d7af-143">Known limitations</span></span>

* <span data-ttu-id="4d7af-144">Tylko protokół JSON jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="4d7af-144">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="4d7af-145">Tylko transport gniazda Websocket jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="4d7af-145">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="4d7af-146">Przesyłanie strumieniowe nie jest jeszcze obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="4d7af-146">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4d7af-147">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="4d7af-147">Additional resources</span></span>

* [<span data-ttu-id="4d7af-148">Dokumentacja interfejsów API języka Java</span><span class="sxs-lookup"><span data-stu-id="4d7af-148">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [<span data-ttu-id="4d7af-149">Dokumentacja usługi Azure SignalR Service bez użycia serwera</span><span class="sxs-lookup"><span data-stu-id="4d7af-149">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
