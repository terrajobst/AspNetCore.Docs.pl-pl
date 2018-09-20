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
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="ea3fb-103">Klienta SignalR Java platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea3fb-103">ASP.NET Core SignalR Java Client</span></span>

<span data-ttu-id="ea3fb-104">Przez [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="ea3fb-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="ea3fb-105">Klienta Java umożliwia połączenie z serwerem biblioteki SignalR platformy ASP.NET Core z kodu Java, w tym aplikacje dla systemu Android.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="ea3fb-106">Podobnie jak [klienta JavaScript](xref:signalr/javascript-client) i [klient modelu .NET](xref:signalr/dotnet-client), klienta Java umożliwia odbieranie i wysyłanie komunikatów do Centrum w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="ea3fb-107">Klienta Java jest dostępne w programie ASP.NET Core 2.2 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="ea3fb-108">Przykładowa aplikacja konsoli środowiska Java przywołanej w niniejszym artykule używa klienta SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="ea3fb-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ea3fb-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="ea3fb-110">Zainstaluj pakiet klienta SignalR Java</span><span class="sxs-lookup"><span data-stu-id="ea3fb-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="ea3fb-111">*Signalr-0.1.0-preview2-35174* plik JAR zezwala klientom na łączenie się koncentratorów SignalR.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-111">The *signalr-0.1.0-preview2-35174* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="ea3fb-112">Aby znaleźć numer najnowszej wersji pliku JAR, zobacz [wyniki wyszukiwania narzędzia Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span><span class="sxs-lookup"><span data-stu-id="ea3fb-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span></span>

<span data-ttu-id="ea3fb-113">Jeśli używasz narzędzia Gradle, Dodaj następujący wiersz do `dependencies` części Twojej *build.gradle* pliku:</span><span class="sxs-lookup"><span data-stu-id="ea3fb-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

<span data-ttu-id="ea3fb-114">Jeśli przy użyciu narzędzia Maven, Dodaj następujące wiersze wewnątrz `<dependencies>` elementu swoje *pom.xml* pliku:</span><span class="sxs-lookup"><span data-stu-id="ea3fb-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="ea3fb-115">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="ea3fb-115">Connect to a hub</span></span>

<span data-ttu-id="ea3fb-116">Aby ustanowić `HubConnection`, `HubConnectionBuilder` powinny być używane.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="ea3fb-117">Poziom adresu URL i dziennika Centrum można skonfigurować podczas tworzenia połączenia.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="ea3fb-118">Skonfiguruj wymagane opcje, wywołując jedną z `HubConnectionBuilder` przed `build`.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="ea3fb-119">Uruchom połączenie przy użyciu `start`.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="ea3fb-120">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="ea3fb-120">Call hub methods from client</span></span>

<span data-ttu-id="ea3fb-121">Wywołanie `send` wywołuje metodę koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="ea3fb-122">Przekaż nazwę metody koncentratora i argumenty zdefiniowane w metody koncentratora `send`.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="ea3fb-123">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="ea3fb-123">Call client methods from hub</span></span>

<span data-ttu-id="ea3fb-124">Użyj `hubConnection.on` do definiowania metod na komputerze klienckim, który można wywoływać koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="ea3fb-125">Określ metody po kompilacji, ale przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a><span data-ttu-id="ea3fb-126">Znane ograniczenia</span><span class="sxs-lookup"><span data-stu-id="ea3fb-126">Known limitations</span></span>

<span data-ttu-id="ea3fb-127">Jest to wczesna wersja zapoznawcza klienta Java.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-127">This is an early preview release of the Java client.</span></span> <span data-ttu-id="ea3fb-128">Istnieje wiele funkcji, które nie są jeszcze obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-128">There are many features that aren't supported yet.</span></span> <span data-ttu-id="ea3fb-129">Następujące luki pracuje w przyszłych wersjach:</span><span class="sxs-lookup"><span data-stu-id="ea3fb-129">The following gaps are being worked on for future releases:</span></span>

* <span data-ttu-id="ea3fb-130">Tylko typy pierwotne, mogą być akceptowane jako parametrów i zwracanych typów.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-130">Only primitive types can be accepted as parameters and return types.</span></span>
* <span data-ttu-id="ea3fb-131">Interfejsy API są synchroniczne.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-131">The APIs are synchronous.</span></span>
* <span data-ttu-id="ea3fb-132">Typ połączenia "Send" jest obsługiwana w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-132">Only the "Send" call type is supported at this time.</span></span> <span data-ttu-id="ea3fb-133">"Wywołaj" i przesyłania strumieniowego wartości zwracane nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-133">"Invoke" and streaming of return values aren't supported.</span></span>
* <span data-ttu-id="ea3fb-134">Tylko protokół JSON jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-134">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="ea3fb-135">Tylko transport gniazda Websocket jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="ea3fb-135">Only the WebSockets transport is supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea3fb-136">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ea3fb-136">Additional resources</span></span>

* [<span data-ttu-id="ea3fb-137">Dokumentacja interfejsu API języka Java</span><span class="sxs-lookup"><span data-stu-id="ea3fb-137">Java API reference</span></span>](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
