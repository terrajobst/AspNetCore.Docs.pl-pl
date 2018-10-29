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
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="229db-103">Klienta programu ASP.NET Core SignalR w języku Java</span><span class="sxs-lookup"><span data-stu-id="229db-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="229db-104">Przez [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="229db-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="229db-105">Klienta Java umożliwia połączenie z serwerem biblioteki SignalR platformy ASP.NET Core z kodu Java, w tym aplikacje dla systemu Android.</span><span class="sxs-lookup"><span data-stu-id="229db-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="229db-106">Podobnie jak [klienta JavaScript](xref:signalr/javascript-client) i [klient modelu .NET](xref:signalr/dotnet-client), klienta Java umożliwia odbieranie i wysyłanie komunikatów do Centrum w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="229db-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="229db-107">Klienta Java jest dostępne w programie ASP.NET Core 2.2 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="229db-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="229db-108">Przykładowa aplikacja konsoli środowiska Java przywołanej w niniejszym artykule używa klienta SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="229db-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="229db-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="229db-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="229db-110">Zainstaluj pakiet klienta SignalR Java</span><span class="sxs-lookup"><span data-stu-id="229db-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="229db-111">*Signalr-1.0.0-preview3-35501* plik JAR zezwala klientom na łączenie się koncentratorów SignalR.</span><span class="sxs-lookup"><span data-stu-id="229db-111">The *signalr-1.0.0-preview3-35501* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="229db-112">Aby znaleźć numer najnowszej wersji pliku JAR, zobacz [wyniki wyszukiwania narzędzia Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="229db-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="229db-113">Jeśli używasz narzędzia Gradle, Dodaj następujący wiersz do `dependencies` części Twojej *build.gradle* pliku:</span><span class="sxs-lookup"><span data-stu-id="229db-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

<span data-ttu-id="229db-114">Jeśli przy użyciu narzędzia Maven, Dodaj następujące wiersze wewnątrz `<dependencies>` elementu swoje *pom.xml* pliku:</span><span class="sxs-lookup"><span data-stu-id="229db-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="229db-115">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="229db-115">Connect to a hub</span></span>

<span data-ttu-id="229db-116">Aby ustanowić `HubConnection`, `HubConnectionBuilder` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="229db-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="229db-117">Poziom adresu URL i dziennika Centrum można skonfigurować podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="229db-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="229db-118">Skonfiguruj wymagane opcje, wywołując jedną z `HubConnectionBuilder` przed `build`.</span><span class="sxs-lookup"><span data-stu-id="229db-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="229db-119">Uruchom połączenie przy użyciu `start`.</span><span class="sxs-lookup"><span data-stu-id="229db-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="229db-120">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="229db-120">Call hub methods from client</span></span>

<span data-ttu-id="229db-121">Wywołanie `send` wywołuje metodę koncentratora.</span><span class="sxs-lookup"><span data-stu-id="229db-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="229db-122">Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `send`.</span><span class="sxs-lookup"><span data-stu-id="229db-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="229db-123">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="229db-123">Call client methods from hub</span></span>

<span data-ttu-id="229db-124">Użyj `hubConnection.on` do definiowania metod na komputerze klienckim, który można wywoływać koncentratora.</span><span class="sxs-lookup"><span data-stu-id="229db-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="229db-125">Określ metody po kompilacji, ale przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="229db-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="229db-126">Dodawanie rejestrowania</span><span class="sxs-lookup"><span data-stu-id="229db-126">Add logging</span></span>

<span data-ttu-id="229db-127">Klient SignalR Java używa [SLF4J](https://www.slf4j.org/) biblioteki do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="229db-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="229db-128">To API wysokiego poziomu rejestrowania, który umożliwia użytkownikom Biblioteka wybrana zapewniali własną implementację określonych rejestrowania, przenosząc powstanie zależności określonych rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="229db-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="229db-129">Poniższy fragment kodu przedstawia sposób użycia `java.util.logging` za pomocą klienta SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="229db-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="229db-130">Jeśli nie skonfigurowano rejestrowanie, w zależności, SLF4J ładuje domyślny Rejestrator nie operacji następujący komunikat ostrzegawczy:</span><span class="sxs-lookup"><span data-stu-id="229db-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="229db-131">To można bezpiecznie zignorować.</span><span class="sxs-lookup"><span data-stu-id="229db-131">This can safely be ignored.</span></span>

## <a name="known-limitations"></a><span data-ttu-id="229db-132">Znane ograniczenia</span><span class="sxs-lookup"><span data-stu-id="229db-132">Known limitations</span></span>

<span data-ttu-id="229db-133">Jest to wersja zapoznawcza klienta Java.</span><span class="sxs-lookup"><span data-stu-id="229db-133">This is a preview release of the Java client.</span></span> <span data-ttu-id="229db-134">Niektóre funkcje nie są obsługiwane:</span><span class="sxs-lookup"><span data-stu-id="229db-134">Some features aren't supported:</span></span>

* <span data-ttu-id="229db-135">Tylko protokół JSON jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="229db-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="229db-136">Tylko transport gniazda Websocket jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="229db-136">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="229db-137">Przesyłanie strumieniowe nie jest jeszcze obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="229db-137">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="229db-138">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="229db-138">Additional resources</span></span>

* [<span data-ttu-id="229db-139">Dokumentacja interfejsów API języka Java</span><span class="sxs-lookup"><span data-stu-id="229db-139">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
