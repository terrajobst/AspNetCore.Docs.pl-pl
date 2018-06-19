---
uid: signalr/overview/older-versions/troubleshooting
title: Rozwiązywanie problemów z SignalR (SignalR 1.x) | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym artykule opisano często występujące problemy z opracowywanie aplikacji SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2013
ms.topic: article
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: eb739f806eb57d09008fa0bf04b5a40721dab73d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566051"
---
<a name="signalr-troubleshooting-signalr-1x"></a><span data-ttu-id="14c6a-103">Rozwiązywanie problemów z SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="14c6a-103">SignalR Troubleshooting (SignalR 1.x)</span></span>
====================
<span data-ttu-id="14c6a-104">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="14c6a-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="14c6a-105">W tym dokumencie opisano typowe problemy dotyczące rozwiązywania problemów z SignalR.</span><span class="sxs-lookup"><span data-stu-id="14c6a-105">This document describes common troubleshooting issues with SignalR.</span></span>


<span data-ttu-id="14c6a-106">Ten dokument zawiera następujące sekcje.</span><span class="sxs-lookup"><span data-stu-id="14c6a-106">This document contains the following sections.</span></span>

- [<span data-ttu-id="14c6a-107">Wywoływanie metody między klientem i serwerem dyskretnie kończy się niepowodzeniem</span><span class="sxs-lookup"><span data-stu-id="14c6a-107">Calling methods between the client and server silently fails</span></span>](#connection)
- [<span data-ttu-id="14c6a-108">Inne problemy z połączeniem</span><span class="sxs-lookup"><span data-stu-id="14c6a-108">Other connection issues</span></span>](#other)
- [<span data-ttu-id="14c6a-109">Błędy kompilacji i po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="14c6a-109">Compilation and server-side errors</span></span>](#server)
- [<span data-ttu-id="14c6a-110">Problemy z usługą Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14c6a-110">Visual Studio issues</span></span>](#vs)
- [<span data-ttu-id="14c6a-111">Problemy z internetowych usług informacyjnych</span><span class="sxs-lookup"><span data-stu-id="14c6a-111">Internet Information Services issues</span></span>](#iis)
- [<span data-ttu-id="14c6a-112">Problemy z platformy Azure</span><span class="sxs-lookup"><span data-stu-id="14c6a-112">Azure issues</span></span>](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a><span data-ttu-id="14c6a-113">Wywoływanie metody między klientem i serwerem dyskretnie kończy się niepowodzeniem</span><span class="sxs-lookup"><span data-stu-id="14c6a-113">Calling methods between the client and server silently fails</span></span>

<span data-ttu-id="14c6a-114">W tej sekcji opisano możliwe przyczyny wywołania metody między klientem i serwerem niepowodzenie bez komunikat o błędzie łatwy do rozpoznania.</span><span class="sxs-lookup"><span data-stu-id="14c6a-114">This section describes possible causes for a method call between client and server to fail without a meaningful error message.</span></span> <span data-ttu-id="14c6a-115">W aplikacji SignalR serwer nie ma informacji o metod, które implementuje w kliencie; gdy serwer wywołuje metodę klienta, dane nazwy i parametru metody są wysyłane do klienta, a metoda jest wykonywana tylko wtedy, gdy istnieje w formacie, który określony serwer.</span><span class="sxs-lookup"><span data-stu-id="14c6a-115">In a SignalR application, the server has no information about the methods that the client implements; when the server invokes a client method, the method name and parameter data are sent to the client, and the method is executed only if it exists in the format that the server specified.</span></span> <span data-ttu-id="14c6a-116">Jeśli odpowiadającej metody znajduje się na kliencie, nic się nie dzieje i żaden komunikat o błędzie nie jest zgłaszany na serwerze.</span><span class="sxs-lookup"><span data-stu-id="14c6a-116">If no matching method is found on the client, nothing happens, and no error message is raised on the server.</span></span>

<span data-ttu-id="14c6a-117">W celu dalszego badania nie pobierania wywoływać metody klienta, można włączyć rejestrowanie przed wywołaniem metody start koncentratora, aby zobaczyć, jakie wywołania pochodzą z serwera.</span><span class="sxs-lookup"><span data-stu-id="14c6a-117">To further investigate client methods not getting called, you can turn on logging before the calling the start method on the hub to see what calls are coming from the server.</span></span> <span data-ttu-id="14c6a-118">Aby włączyć rejestrowanie w aplikacji JavaScript, zobacz [włączania rejestrowania po stronie klienta (wersja klienta JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span><span class="sxs-lookup"><span data-stu-id="14c6a-118">To enable logging in a JavaScript application, see [How to enable client-side logging (JavaScript client version)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span></span> <span data-ttu-id="14c6a-119">Aby włączyć rejestrowanie w aplikacji klienckiej .NET, zobacz [włączania rejestrowania po stronie klienta (wersja klienta .NET)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span><span class="sxs-lookup"><span data-stu-id="14c6a-119">To enable logging in a .NET client application, see [How to enable client-side logging (.NET Client version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span></span>

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a><span data-ttu-id="14c6a-120">Błędnie — metoda, podpis metody nieprawidłowe lub nazwa nieprawidłowa koncentratora</span><span class="sxs-lookup"><span data-stu-id="14c6a-120">Misspelled method, incorrect method signature, or incorrect hub name</span></span>

<span data-ttu-id="14c6a-121">Jeśli w nazwie lub podpisie metody wywoływanego jest dokładnie zgodny odpowiedniej metody na kliencie, wywołanie zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="14c6a-121">If the name or signature of a called method does not exactly match an appropriate method on the client, the call will fail.</span></span> <span data-ttu-id="14c6a-122">Sprawdź, czy nazwa metody wywoływane przez serwer zgodna Nazwa metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="14c6a-122">Verify that the method name called by the server matches the name of the method on the client.</span></span> <span data-ttu-id="14c6a-123">Ponadto SignalR tworzy serwera proxy koncentratora, za pomocą metody formatu — z uwzględnieniem wielkości liter, jest odpowiednie w języku JavaScript, więc wywołana metoda `SendMessage` na serwerze może być wywoływana `sendMessage` na serwerze proxy klienta.</span><span class="sxs-lookup"><span data-stu-id="14c6a-123">Also, SignalR creates the hub proxy using camel-cased methods, as is appropriate in JavaScript, so a method called `SendMessage` on the server would be called `sendMessage` in the client proxy.</span></span> <span data-ttu-id="14c6a-124">Jeśli używasz `HubName` atrybutu w kodzie po stronie serwera, sprawdź, czy nazwa używana zgodna z nazwą pozwala utworzyć koncentratora na kliencie.</span><span class="sxs-lookup"><span data-stu-id="14c6a-124">If you use the `HubName` attribute in your server-side code, verify that the name used matches the name used to create the hub on the client.</span></span> <span data-ttu-id="14c6a-125">Jeśli nie używasz `HubName` atrybutu, sprawdzić, czy nazwa koncentratora na kliencie JavaScript jest formatu — z uwzględnieniem wielkości liter, takie jak chatHub zamiast ChatHub.</span><span class="sxs-lookup"><span data-stu-id="14c6a-125">If you do not use the `HubName` attribute, verify that the name of the hub in a JavaScript client is camel-cased, such as chatHub instead of ChatHub.</span></span>

### <a name="duplicate-method-name-on-client"></a><span data-ttu-id="14c6a-126">Zduplikowana nazwa metody na kliencie</span><span class="sxs-lookup"><span data-stu-id="14c6a-126">Duplicate method name on client</span></span>

<span data-ttu-id="14c6a-127">Sprawdź, czy nie zduplikowaną metodę na kliencie, który różni się tylko wielkością liter.</span><span class="sxs-lookup"><span data-stu-id="14c6a-127">Verify that you do not have a duplicate method on the client that differs only by case.</span></span> <span data-ttu-id="14c6a-128">Jeśli aplikacja kliencka ma metodę o nazwie `sendMessage`, sprawdź, czy nie ma również metody o nazwie `SendMessage` również.</span><span class="sxs-lookup"><span data-stu-id="14c6a-128">If your client application has a method called `sendMessage`, verify that there isn't also a method called `SendMessage` as well.</span></span>

### <a name="missing-json-parser-on-the-client"></a><span data-ttu-id="14c6a-129">Brak analizator JSON na kliencie</span><span class="sxs-lookup"><span data-stu-id="14c6a-129">Missing JSON parser on the client</span></span>

<span data-ttu-id="14c6a-130">SignalR wymaga analizatora składni JSON do serializacji połączeń między serwerem a klientem.</span><span class="sxs-lookup"><span data-stu-id="14c6a-130">SignalR requires a JSON parser to be present to serialize calls between the server and the client.</span></span> <span data-ttu-id="14c6a-131">Jeśli klient nie ma wbudowanej analizatora składni JSON (takich jak program Internet Explorer 7), należy uwzględnić w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14c6a-131">If your client doesn't have a built-in JSON parser (such as Internet Explorer 7), you'll need to include one in your application.</span></span> <span data-ttu-id="14c6a-132">Możesz pobrać analizatora składni JSON [tutaj](http://nuget.org/packages/json2).</span><span class="sxs-lookup"><span data-stu-id="14c6a-132">You can download the JSON parser [here](http://nuget.org/packages/json2).</span></span>

### <a name="mixing-hub-and-persistentconnection-syntax"></a><span data-ttu-id="14c6a-133">Mieszanie koncentratora i klasy PersistentConnection składni</span><span class="sxs-lookup"><span data-stu-id="14c6a-133">Mixing Hub and PersistentConnection syntax</span></span>

<span data-ttu-id="14c6a-134">Biblioteka SignalR używa dwóch modeli komunikacji: koncentratorów i PersistentConnections.</span><span class="sxs-lookup"><span data-stu-id="14c6a-134">SignalR uses two communication models: Hubs and PersistentConnections.</span></span> <span data-ttu-id="14c6a-135">Składnia wywoływania tych dwóch komunikacji modeli różni się w kodzie klienta.</span><span class="sxs-lookup"><span data-stu-id="14c6a-135">The syntax for calling these two communication models is different in the client code.</span></span> <span data-ttu-id="14c6a-136">Jeśli koncentrator został dodany w kodzie serwera, Sprawdź cały kod klienta jest używana składnia prawidłowego koncentratora.</span><span class="sxs-lookup"><span data-stu-id="14c6a-136">If you have added a hub in your server code, verify that all of your client code uses the proper hub syntax.</span></span>

<span data-ttu-id="14c6a-137">**Kod klienta JavaScript, który tworzy klasy PersistentConnection w kliencie JavaScript**</span><span class="sxs-lookup"><span data-stu-id="14c6a-137">**JavaScript client code that creates a PersistentConnection in a JavaScript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

<span data-ttu-id="14c6a-138">**Kod JavaScript klienta, która tworzy Proxy koncentratora na kliencie Javascript**</span><span class="sxs-lookup"><span data-stu-id="14c6a-138">**JavaScript client code that creates a Hub Proxy in a Javascript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

<span data-ttu-id="14c6a-139">**C# serwera kod, który mapuje trasę do klasy PersistentConnection**</span><span class="sxs-lookup"><span data-stu-id="14c6a-139">**C# server code that maps a route to a PersistentConnection**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

<span data-ttu-id="14c6a-140">**C# serwera kod, który mapuje trasy z koncentratorem lub do wielu koncentratorów, jeśli masz wiele aplikacji**</span><span class="sxs-lookup"><span data-stu-id="14c6a-140">**C# server code that maps a route to a Hub, or to mulitple hubs if you have multiple applications**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a><span data-ttu-id="14c6a-141">Połączenie zostało uruchomione przed subskrypcje są dodawane</span><span class="sxs-lookup"><span data-stu-id="14c6a-141">Connection started before subscriptions are added</span></span>

<span data-ttu-id="14c6a-142">Jeśli połączenie koncentratora jest uruchomiona, zanim metod, które mogą być wywoływane z serwera zostaną dodane do serwera proxy, nie można odebrać wiadomości.</span><span class="sxs-lookup"><span data-stu-id="14c6a-142">If the Hub's connection is started before methods that can be called from the server are added to the proxy, messages will not be received.</span></span> <span data-ttu-id="14c6a-143">Następujący kod JavaScript nie zostanie prawidłowo uruchomić Centrum:</span><span class="sxs-lookup"><span data-stu-id="14c6a-143">The following JavaScript code will not start the hub properly:</span></span>

<span data-ttu-id="14c6a-144">**Niepoprawny kod klienta JavaScript nie zezwala na komunikaty koncentratory do odebrania**</span><span class="sxs-lookup"><span data-stu-id="14c6a-144">**Incorrect JavaScript client code that will not allow Hubs messages to be received**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

<span data-ttu-id="14c6a-145">Zamiast tego dodać subskrypcji metoda przed wywołaniem metody Start:</span><span class="sxs-lookup"><span data-stu-id="14c6a-145">Instead, add the method subscriptions before calling Start:</span></span>

<span data-ttu-id="14c6a-146">**Kod klienta JavaScript poprawnie dodające subskrypcji z koncentratorem**</span><span class="sxs-lookup"><span data-stu-id="14c6a-146">**JavaScript client code that correctly adds subscriptions to a hub**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a><span data-ttu-id="14c6a-147">Brak nazwy metody dla serwera proxy koncentratora</span><span class="sxs-lookup"><span data-stu-id="14c6a-147">Missing method name on the hub proxy</span></span>

<span data-ttu-id="14c6a-148">Upewnij się, że metoda, zdefiniowanego na serwerze jest subskrybentem na kliencie.</span><span class="sxs-lookup"><span data-stu-id="14c6a-148">Verify that the method defined on the server is subscribed to on the client.</span></span> <span data-ttu-id="14c6a-149">Nawet jeśli serwer definiuje metodę, nadal należy dodać do serwera proxy klienta.</span><span class="sxs-lookup"><span data-stu-id="14c6a-149">Even though the server defines the method, it must still be added to the client proxy.</span></span> <span data-ttu-id="14c6a-150">Metody można dodać do serwera proxy klienta w następujący sposób (należy pamiętać, że metoda jest dodawana do `client` elementu członkowskiego koncentratora, a nie bezpośrednio koncentratora):</span><span class="sxs-lookup"><span data-stu-id="14c6a-150">Methods can be added to the client proxy in the following ways (Note that the method is added to the `client` member of the hub, not the hub directly):</span></span>

<span data-ttu-id="14c6a-151">**Kod JavaScript klienta, który dodaje metody do serwera proxy koncentratora**</span><span class="sxs-lookup"><span data-stu-id="14c6a-151">**JavaScript client code that adds methods to a hub proxy**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a><span data-ttu-id="14c6a-152">Centrum i metod koncentratorów nie jest zadeklarowany jako publiczny</span><span class="sxs-lookup"><span data-stu-id="14c6a-152">Hub or hub methods not declared as Public</span></span>

<span data-ttu-id="14c6a-153">Aby były widoczne na kliencie, implementacji koncentratora i metody musi być zadeklarowany jako `public`.</span><span class="sxs-lookup"><span data-stu-id="14c6a-153">To be visible on the client, the hub implementation and methods must be declared as `public`.</span></span>

### <a name="accessing-hub-from-a-different-application"></a><span data-ttu-id="14c6a-154">Uzyskiwanie dostępu do Centrum z innej aplikacji</span><span class="sxs-lookup"><span data-stu-id="14c6a-154">Accessing hub from a different application</span></span>

<span data-ttu-id="14c6a-155">Koncentratory SignalR można uzyskać tylko za pośrednictwem aplikacji, które implementują klientów SignalR.</span><span class="sxs-lookup"><span data-stu-id="14c6a-155">SignalR Hubs can only be accessed through applications that implement SignalR clients.</span></span> <span data-ttu-id="14c6a-156">SignalR nie może współpracować z innymi bibliotekami komunikacji (np. SOAP lub WCF usługi sieci web.) Jeśli nie jest dostępny dla danej platformy docelowej nie klienta SignalR, nie masz dostępu do serwera punktu końcowego bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="14c6a-156">SignalR cannot interoperate with other communication libraries (like SOAP or WCF web services.) If there is no SignalR client available for your target platform, you cannot access the server's endpoint directly.</span></span>

### <a name="manually-serializing-data"></a><span data-ttu-id="14c6a-157">Ręcznie serializacja danych</span><span class="sxs-lookup"><span data-stu-id="14c6a-157">Manually serializing data</span></span>

<span data-ttu-id="14c6a-158">SignalR będą automatycznie używać JSON do serializacji metodę parametry tam nie ma potrzeby zrobić samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="14c6a-158">SignalR will automatically use JSON to serialize your method parameters- there's no need to do it yourself.</span></span>

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a><span data-ttu-id="14c6a-159">Nie wykonano na kliencie w funkcji ondisconnected elementu zdalnym metody koncentratora</span><span class="sxs-lookup"><span data-stu-id="14c6a-159">Remote Hub method not executed on client in OnDisconnected function</span></span>

<span data-ttu-id="14c6a-160">To zachowanie jest celowe.</span><span class="sxs-lookup"><span data-stu-id="14c6a-160">This behavior is by design.</span></span> <span data-ttu-id="14c6a-161">Gdy `OnDisconnected` jest wywoływana, koncentratora został już wprowadzony `Disconnected` stanu, który nie zezwala na dalsze można wywołać metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="14c6a-161">When `OnDisconnected` is called, the hub has already entered the `Disconnected` state, which does not allow further hub methods to be called.</span></span>

<span data-ttu-id="14c6a-162">**C# serwera kod, który prawidłowo wykonuje kod w zdarzeniu ondisconnected elementu**</span><span class="sxs-lookup"><span data-stu-id="14c6a-162">**C# server code that correctly executes code in the OnDisconnected event**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a><span data-ttu-id="14c6a-163">Osiągnięto limit połączeń</span><span class="sxs-lookup"><span data-stu-id="14c6a-163">Connection limit reached</span></span>

<span data-ttu-id="14c6a-164">Korzystając z pełną wersję programu IIS w systemie operacyjnym klienta, takich jak Windows 7, nakłada się limit 10 połączeń.</span><span class="sxs-lookup"><span data-stu-id="14c6a-164">When using the full version of IIS on a client operating system like Windows 7, a 10-connection limit is imposed.</span></span> <span data-ttu-id="14c6a-165">Podczas korzystania z systemu operacyjnego klienta, użyj programu IIS Express w celu uniknięcia tego limitu.</span><span class="sxs-lookup"><span data-stu-id="14c6a-165">When using a client OS, use IIS Express instead to avoid this limit.</span></span>

### <a name="cross-domain-connection-not-set-up-properly"></a><span data-ttu-id="14c6a-166">Połączenie między domenami nie zostało prawidłowo skonfigurowane</span><span class="sxs-lookup"><span data-stu-id="14c6a-166">Cross-domain connection not set up properly</span></span>

<span data-ttu-id="14c6a-167">Jeśli połączenie między domenami (połączenie dla którego SignalR adres URL nie jest w tej samej domenie co stronę hostingu) nie jest skonfigurowane poprawnie, połączenie może zakończyć się niepowodzeniem bez komunikatu o błędzie.</span><span class="sxs-lookup"><span data-stu-id="14c6a-167">If a cross-domain connection (a connection for which the SignalR URL is not in the same domain as the hosting page) is not set up correctly, the connection may fail without an error message.</span></span> <span data-ttu-id="14c6a-168">Aby uzyskać informacje na temat włączania komunikacji między domenami, zobacz [jak nawiązać połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="14c6a-168">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a><span data-ttu-id="14c6a-169">Połączenia przy użyciu protokołu NTLM (Active Directory), nie będzie działać w klienta .NET</span><span class="sxs-lookup"><span data-stu-id="14c6a-169">Connection using NTLM (Active Directory) not working in .NET client</span></span>

<span data-ttu-id="14c6a-170">Połączenie w aplikacji klienta .NET, która używa zabezpieczeń domeny może zakończyć się niepowodzeniem, jeśli połączenie nie jest prawidłowo skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="14c6a-170">A connection in a .NET client application that uses Domain security may fail if the connection is not configured properly.</span></span> <span data-ttu-id="14c6a-171">Aby użyć SignalR w środowisku domeny, należy ustawić właściwość wymagane połączenia w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="14c6a-171">To use SignalR in a domain environment, set the requisite connection property as follows:</span></span>

<span data-ttu-id="14c6a-172">**C# klienta kod, który implementuje poświadczeń dla połączenia**</span><span class="sxs-lookup"><span data-stu-id="14c6a-172">**C# client code that implements connection credentials**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a><span data-ttu-id="14c6a-173">Inne problemy z połączeniem</span><span class="sxs-lookup"><span data-stu-id="14c6a-173">Other connection issues</span></span>

<span data-ttu-id="14c6a-174">W tej sekcji opisano przyczyny i rozwiązania symptomy lub komunikaty o błędach, jakie występują podczas połączenia.</span><span class="sxs-lookup"><span data-stu-id="14c6a-174">This section describes the causes and solutions for specific symptoms or error messages that occur during a connection.</span></span>

### <a name="start-must-be-called-before-data-can-be-sent-error"></a><span data-ttu-id="14c6a-175">Błąd "Start musi zostać wywołana przed wysłaniem danych"</span><span class="sxs-lookup"><span data-stu-id="14c6a-175">"Start must be called before data can be sent" error</span></span>

<span data-ttu-id="14c6a-176">Ten błąd występuje często, jeśli kod odwołuje się do obiektów SignalR przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="14c6a-176">This error is commonly seen if code references SignalR objects before the connection is started.</span></span> <span data-ttu-id="14c6a-177">Wireup dla programów obsługi i podobnych czy będzie wywołania metody, zdefiniowanego na serwerze należy dodać po zakończeniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="14c6a-177">The wireup for handlers and the like that will call methods defined on the server must be added after the connection completes.</span></span> <span data-ttu-id="14c6a-178">Należy pamiętać, że wywołanie `Start` jest asynchroniczne, więc zakończeniu kod po wywołaniu mogą być wykonywane przed.</span><span class="sxs-lookup"><span data-stu-id="14c6a-178">Note that the call to `Start` is asynchronous, so code after the call may be executed before it completes.</span></span> <span data-ttu-id="14c6a-179">Dodaj programy obsługi, połączenie zostanie całkowicie rozpoczęta najlepiej do funkcji wywołania zwrotnego, która jest przekazywana jako parametr do metody uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="14c6a-179">The best way to add handlers after a connection starts completely is to put them into a callback function that is passed as a parameter to the start method:</span></span>

<span data-ttu-id="14c6a-180">**Kod klienta JavaScript poprawnie dodające procedury obsługi zdarzeń, które odwołują się obiekty SignalR**</span><span class="sxs-lookup"><span data-stu-id="14c6a-180">**JavaScript client code that correctly adds event handlers that reference SignalR objects**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

<span data-ttu-id="14c6a-181">Ten błąd będzie także wystąpić, jeśli połączenie zostanie zatrzymana, gdy SignalR obiekty są nadal odwołuje.</span><span class="sxs-lookup"><span data-stu-id="14c6a-181">This error will also be seen if a connection stops while SignalR objects are still being referenced.</span></span>

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a><span data-ttu-id="14c6a-182">"301 Przeniesiono trwale" lub "302 został tymczasowo przeniesiony" błędu</span><span class="sxs-lookup"><span data-stu-id="14c6a-182">"301 Moved Permanently" or "302 Moved Temporarily" error</span></span>

<span data-ttu-id="14c6a-183">Ten błąd może wystąpić, jeśli projekt zawiera folder o nazwie SignalR, który będzie zakłócać proxy tworzone automatycznie.</span><span class="sxs-lookup"><span data-stu-id="14c6a-183">This error may be seen if the project contains a folder called SignalR, which will interfere with the automatically-created proxy.</span></span> <span data-ttu-id="14c6a-184">Aby uniknąć tego błędu, nie należy używać folder o nazwie `SignalR` w aplikacji, lub Włącz automatyczną serwera proxy generowania off.</span><span class="sxs-lookup"><span data-stu-id="14c6a-184">To avoid this error, do not use a folder called `SignalR` in your application, or turn automatic proxy generation off.</span></span> <span data-ttu-id="14c6a-185">Zobacz [Proxy generowany i jakie operacje dla Ciebie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) więcej szczegółów.</span><span class="sxs-lookup"><span data-stu-id="14c6a-185">See [The Generated Proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) for more details.</span></span>

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a><span data-ttu-id="14c6a-186">Błąd "403 zabroniony" w kliencie .NET lub Silverlight</span><span class="sxs-lookup"><span data-stu-id="14c6a-186">"403 Forbidden" error in .NET or Silverlight client</span></span>

<span data-ttu-id="14c6a-187">Ten błąd może wystąpić w środowiskach międzydomenowego, gdzie komunikacji między domenami nie jest prawidłowo włączone.</span><span class="sxs-lookup"><span data-stu-id="14c6a-187">This error may occur in cross-domain environments where cross-domain communication is not properly enabled.</span></span> <span data-ttu-id="14c6a-188">Aby uzyskać informacje na temat włączania komunikacji między domenami, zobacz [jak nawiązać połączenie między domenami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="14c6a-188">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span> <span data-ttu-id="14c6a-189">Aby nawiązać połączenie między domenami w kliencie programu Silverlight, zobacz [międzydomenowego połączenia od klientów programu Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span><span class="sxs-lookup"><span data-stu-id="14c6a-189">To establish a cross-domain connection in a Silverlight client, see [Cross-domain connections from Silverlight clients](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span></span>

### <a name="404-not-found-error"></a><span data-ttu-id="14c6a-190">Błąd "nie można odnaleźć 404"</span><span class="sxs-lookup"><span data-stu-id="14c6a-190">"404 Not Found" error</span></span>

<span data-ttu-id="14c6a-191">Istnieje kilka przyczyn tego problemu.</span><span class="sxs-lookup"><span data-stu-id="14c6a-191">There are several causes for this issue.</span></span> <span data-ttu-id="14c6a-192">Sprawdź wszystkie z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="14c6a-192">Verify all of the following:</span></span>

- <span data-ttu-id="14c6a-193">**Odwołanie do adresu serwera proxy koncentratora nieprawidłowo sformatowana:** ten błąd często jest widoczny, gdy odwołanie do wygenerowanego Centrum adres serwera proxy nie jest prawidłowo sformatowany.</span><span class="sxs-lookup"><span data-stu-id="14c6a-193">**Hub proxy address reference not formatted correctly:** This error is commonly seen if the reference to the generated hub proxy address is not formatted correctly.</span></span> <span data-ttu-id="14c6a-194">Upewnij się, że adres centrum odniesienia poprawnie.</span><span class="sxs-lookup"><span data-stu-id="14c6a-194">Verify that the reference to the hub address is made properly.</span></span> <span data-ttu-id="14c6a-195">Zobacz [jak odwołuje się dynamicznie generowanym obiektu pośredniczącego](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="14c6a-195">See [How to reference the dynamically generated proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) for details.</span></span>
- <span data-ttu-id="14c6a-196">**Dodawanie tras do aplikacji przed dodaniem trasę koncentratora:** Jeśli aplikacja korzysta z innych tras, sprawdź, czy trasa pierwszy dodawane wywołania `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="14c6a-196">**Adding routes to application before adding the hub route:** If your application uses other routes, verify that the first route added is the call to `MapHubs`.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="14c6a-197">"500 Wewnętrzny błąd serwera"</span><span class="sxs-lookup"><span data-stu-id="14c6a-197">"500 Internal Server Error"</span></span>

<span data-ttu-id="14c6a-198">Jest to bardzo ogólny błąd, który może występować wiele różnych przyczyn.</span><span class="sxs-lookup"><span data-stu-id="14c6a-198">This is a very generic error that could have a wide variety of causes.</span></span> <span data-ttu-id="14c6a-199">Szczegóły błędu powinny być wyświetlane w dzienniku zdarzeń serwera lub znajduje się za pomocą debugowania serwera.</span><span class="sxs-lookup"><span data-stu-id="14c6a-199">The details of the error should appear in the server's event log, or can be found through debugging the server.</span></span> <span data-ttu-id="14c6a-200">Bardziej szczegółowe informacje o błędzie można uzyskać poprzez włączenie szczegółowe błędy na serwerze.</span><span class="sxs-lookup"><span data-stu-id="14c6a-200">More detailed error information may be obtained by turning on detailed errors on the server.</span></span> <span data-ttu-id="14c6a-201">Aby uzyskać więcej informacji, zobacz [sposób obsługi błędów w klasie Centrum](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span><span class="sxs-lookup"><span data-stu-id="14c6a-201">For more information, see [How to handle errors in the Hub class](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span></span>

### <a name="typeerror-lthubtypegt-is-undefined-error"></a><span data-ttu-id="14c6a-202">"TypeError: &lt;hubType&gt; zdefiniowano" Błąd</span><span class="sxs-lookup"><span data-stu-id="14c6a-202">"TypeError: &lt;hubType&gt; is undefined" error</span></span>

<span data-ttu-id="14c6a-203">Spowoduje to błąd wywołania `MapHubs` nie zostało utworzone prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="14c6a-203">This error will result if the call to `MapHubs` is not made properly.</span></span> <span data-ttu-id="14c6a-204">Zobacz [jak zarejestrować trasy SignalR i skonfigurować opcje SignalR](../guide-to-the-api/hubs-api-guide-server.md#route) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="14c6a-204">See [How to register the SignalR route and configure SignalR options](../guide-to-the-api/hubs-api-guide-server.md#route) for more information.</span></span>

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a><span data-ttu-id="14c6a-205">JsonSerializationException został obsłużony przez kod użytkownika</span><span class="sxs-lookup"><span data-stu-id="14c6a-205">JsonSerializationException was unhandled by user code</span></span>

<span data-ttu-id="14c6a-206">Sprawdź parametry, których wysyłanie do metody nie dołączaj do serializacji typów (na przykład dojścia do plików lub połączenia z bazą danych).</span><span class="sxs-lookup"><span data-stu-id="14c6a-206">Verify that the parameters you send to your methods do not include non-serializable types (like file handles or database connections).</span></span> <span data-ttu-id="14c6a-207">Jeśli musisz użyć elementów członkowskich w obiekcie po stronie serwera, który nie ma do wysłania do klienta (lub zabezpieczeń ze względu na serializacji), użyj `JSONIgnore` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="14c6a-207">If you need to use members on a server-side object that you don't want to be sent to the client (either for security or for reasons of serialization), use the `JSONIgnore` attribute.</span></span>

### <a name="protocol-error-unknown-transport-error"></a><span data-ttu-id="14c6a-208">"Błąd protokołu: nieznany transport" Błąd</span><span class="sxs-lookup"><span data-stu-id="14c6a-208">"Protocol error: Unknown transport" error</span></span>

<span data-ttu-id="14c6a-209">Ten błąd może wystąpić, jeśli klient nie obsługuje transportów, które korzysta z SignalR.</span><span class="sxs-lookup"><span data-stu-id="14c6a-209">This error may occur if the client does not support the transports that SignalR uses.</span></span> <span data-ttu-id="14c6a-210">Zobacz [transportu i przejścia](../getting-started/introduction-to-signalr.md#transports) informacje, na którym przeglądarki może być używany z SignalR.</span><span class="sxs-lookup"><span data-stu-id="14c6a-210">See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for information on which browsers can be used with SignalR.</span></span>

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a><span data-ttu-id="14c6a-211">"Generowanie serwera proxy elementu Hub w języku JavaScript zostało wyłączone."</span><span class="sxs-lookup"><span data-stu-id="14c6a-211">"JavaScript Hub proxy generation has been disabled."</span></span>

<span data-ttu-id="14c6a-212">Ten błąd wystąpi, jeśli `DisableJavaScriptProxies` ustawiono podczas także w tym odwołania na dynamicznie generowanym serwer proxy na `signalr/hubs`.</span><span class="sxs-lookup"><span data-stu-id="14c6a-212">This error will occur if `DisableJavaScriptProxies` is set while also including a reference to the dynamically generated proxy at `signalr/hubs`.</span></span> <span data-ttu-id="14c6a-213">Aby uzyskać więcej informacji na temat tworzenia serwer proxy ręcznie, zobacz [wygenerowany serwer proxy i jakie operacje dla Ciebie](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="14c6a-213">For more information on creating the proxy manually, see [The generated proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span></span>

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a><span data-ttu-id="14c6a-214">"Identyfikator połączenia, który ma niepoprawny format" lub "tożsamości użytkownika nie można zmieniać podczas aktywnego połączenia SignalR" Błąd</span><span class="sxs-lookup"><span data-stu-id="14c6a-214">"The connection ID is in the incorrect format" or "The user identity cannot change during an active SignalR connection" error</span></span>

<span data-ttu-id="14c6a-215">Ten błąd może wystąpić, jeśli jest używane uwierzytelnianie, a klient wylogowaniu przed połączenie zostanie zatrzymana.</span><span class="sxs-lookup"><span data-stu-id="14c6a-215">This error may be seen if authentication is being used, and the client is logged out before the connection is stopped.</span></span> <span data-ttu-id="14c6a-216">Rozwiązaniem jest zatrzymanie połączenia SignalR, przed zalogowaniem klienta.</span><span class="sxs-lookup"><span data-stu-id="14c6a-216">The solution is to stop the SignalR connection before logging the client out.</span></span>

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a><span data-ttu-id="14c6a-217">"Nieprzechwyconego błędu: SignalR: nie można odnaleźć jQuery.</span><span class="sxs-lookup"><span data-stu-id="14c6a-217">"Uncaught Error: SignalR: jQuery not found.</span></span> <span data-ttu-id="14c6a-218">Upewnij się, że jQuery odwołuje się do pliku SignalR.js"Błąd</span><span class="sxs-lookup"><span data-stu-id="14c6a-218">Please ensure jQuery is referenced before the SignalR.js file" error</span></span>

<span data-ttu-id="14c6a-219">Klient programu SignalR JavaScript wymaga jQuery do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="14c6a-219">The SignalR JavaScript client requires jQuery to run.</span></span> <span data-ttu-id="14c6a-220">Sprawdź, czy odwołania do jQuery jest poprawna, czy ścieżki jest prawidłowa i czy odniesienie do jQuery jest przed odwołanie do biblioteki SignalR.</span><span class="sxs-lookup"><span data-stu-id="14c6a-220">Verify that your reference to jQuery is correct, that the path used is valid, and that the reference to jQuery is before the reference to SignalR.</span></span>

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a><span data-ttu-id="14c6a-221">"Nieprzechwyconego TypeError: nie można odczytać właściwości"&lt;właściwości&gt;"undefined" Błąd</span><span class="sxs-lookup"><span data-stu-id="14c6a-221">"Uncaught TypeError: Cannot read property '&lt;property&gt;' of undefined" error</span></span>

<span data-ttu-id="14c6a-222">Ten błąd wynika z nie posiadają jQuery lub serwera proxy koncentratory odpowiednie odwołania.</span><span class="sxs-lookup"><span data-stu-id="14c6a-222">This error results from not having jQuery or the hubs proxy referenced properly.</span></span> <span data-ttu-id="14c6a-223">Sprawdź, czy odwołania do jQuery i koncentratory serwera proxy jest poprawna, czy ścieżki jest prawidłowa i czy odniesienie do jQuery jest przed odwołanie do serwera proxy koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="14c6a-223">Verify that your reference to jQuery and the hubs proxy is correct, that the path used is valid, and that the reference to jQuery is before the reference to the hubs proxy.</span></span> <span data-ttu-id="14c6a-224">Domyślne odwołanie do serwera proxy koncentratory powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="14c6a-224">The default reference to the hubs proxy should look like the following:</span></span>

<span data-ttu-id="14c6a-225">**Kod HTML po stronie klienta, która poprawnie odwołuje się do serwera proxy koncentratory**</span><span class="sxs-lookup"><span data-stu-id="14c6a-225">**HTML client-side code that correctly references the Hubs proxy**</span></span>

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a><span data-ttu-id="14c6a-226">Błąd "RuntimeBinderException został obsłużony przez kod użytkownika"</span><span class="sxs-lookup"><span data-stu-id="14c6a-226">"RuntimeBinderException was unhandled by user code" error</span></span>

<span data-ttu-id="14c6a-227">Ten błąd może wystąpić, gdy niepoprawne przeciążenia `Hub.On` jest używany.</span><span class="sxs-lookup"><span data-stu-id="14c6a-227">This error may occur when the incorrect overload of `Hub.On` is used.</span></span> <span data-ttu-id="14c6a-228">Jeśli metoda ma wartość zwracaną, zwracany typ musi być określona jako parametr typu ogólnego:</span><span class="sxs-lookup"><span data-stu-id="14c6a-228">If the method has a return value, the return type must be specified as a generic type parameter:</span></span>

<span data-ttu-id="14c6a-229">**Metody zdefiniowane na kliencie (bez wygenerowany serwer proxy)**</span><span class="sxs-lookup"><span data-stu-id="14c6a-229">**Method defined on the client (without generated proxy)**</span></span>

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a><span data-ttu-id="14c6a-230">Identyfikator połączenia jest niespójna lub przerywa połączenia między elementami strony</span><span class="sxs-lookup"><span data-stu-id="14c6a-230">Connection ID is inconsistent or connection breaks between page loads</span></span>

<span data-ttu-id="14c6a-231">To zachowanie jest celowe.</span><span class="sxs-lookup"><span data-stu-id="14c6a-231">This behavior is by design.</span></span> <span data-ttu-id="14c6a-232">Ponieważ obiekt koncentratora znajduje się w obiekcie strony, koncentratora zostanie zniszczony po odświeżeniu strony.</span><span class="sxs-lookup"><span data-stu-id="14c6a-232">Since the hub object is hosted in the page object, the hub is destroyed when the page refreshes.</span></span> <span data-ttu-id="14c6a-233">Aplikacji wielostronicowych musi zachować skojarzenia między użytkownikami a identyfikatorów połączeń, dzięki czemu będą one spójne ładunków strona.</span><span class="sxs-lookup"><span data-stu-id="14c6a-233">A multi-page application needs to maintain the association between users and connection IDs so that they will be consistent between page loads.</span></span> <span data-ttu-id="14c6a-234">Identyfikatorów połączeń mogą być przechowywane na serwerze w jednej `ConcurrentDictionary` obiektu lub bazy danych.</span><span class="sxs-lookup"><span data-stu-id="14c6a-234">The connection IDs can be stored on the server in either a `ConcurrentDictionary` object or a database.</span></span>

### <a name="value-cannot-be-null-error"></a><span data-ttu-id="14c6a-235">Błąd "Wartości nie może mieć wartości null"</span><span class="sxs-lookup"><span data-stu-id="14c6a-235">"Value cannot be null" error</span></span>

<span data-ttu-id="14c6a-236">Po stronie serwera metody z opcjonalnymi parametrami nie są obecnie obsługiwane; opcjonalny parametr zostanie pominięty, metoda zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="14c6a-236">Server-side methods with optional parameters are not currently supported; if the optional parameter is omitted, the method will fail.</span></span> <span data-ttu-id="14c6a-237">Aby uzyskać więcej informacji, zobacz [następujące parametry opcjonalne](https://github.com/SignalR/SignalR/issues/324).</span><span class="sxs-lookup"><span data-stu-id="14c6a-237">For more information, see [Optional Parameters](https://github.com/SignalR/SignalR/issues/324).</span></span>

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a><span data-ttu-id="14c6a-238">"Firefox nie może nawiązać połączenie z serwerem przy &lt;adres&gt;" Wystąpił błąd podczas Firebug</span><span class="sxs-lookup"><span data-stu-id="14c6a-238">"Firefox can't establish a connection to the server at &lt;address&gt;" error in Firebug</span></span>

<span data-ttu-id="14c6a-239">Ten komunikat o błędzie może być widoczny w Firebug w przypadku awarii negocjacji protokołu WebSocket transportu i zamiast niego jest używana innego transportu.</span><span class="sxs-lookup"><span data-stu-id="14c6a-239">This error message can be seen in Firebug if negotiation of the WebSocket transport fails and another transport is used instead.</span></span> <span data-ttu-id="14c6a-240">To zachowanie jest celowe.</span><span class="sxs-lookup"><span data-stu-id="14c6a-240">This behavior is by design.</span></span>

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a><span data-ttu-id="14c6a-241">Błąd "jest nieprawidłowa przy uwzględnieniu procedurę sprawdzania poprawności certyfikatu zdalnego" w aplikacji klienta .NET</span><span class="sxs-lookup"><span data-stu-id="14c6a-241">"The remote certificate is invalid according to the validation procedure" error in .NET client application</span></span>

<span data-ttu-id="14c6a-242">Jeśli serwer wymaga certyfikatów klienta, a następnie dodać x509certificate do połączenia przed dokonaniem żądania.</span><span class="sxs-lookup"><span data-stu-id="14c6a-242">If your server requires custom client certificates, then you can add an x509certificate to the connection before the request is made.</span></span> <span data-ttu-id="14c6a-243">Dodawanie certyfikatu do połączenia za pomocą `Connection.AddClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="14c6a-243">Add the certificate to the connection using `Connection.AddClientCertificate`.</span></span>

### <a name="connection-drops-after-authentication-times-out"></a><span data-ttu-id="14c6a-244">Porzuca połączenia po upływie limitu czasu uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="14c6a-244">Connection drops after authentication times out</span></span>

<span data-ttu-id="14c6a-245">To zachowanie jest celowe.</span><span class="sxs-lookup"><span data-stu-id="14c6a-245">This behavior is by design.</span></span> <span data-ttu-id="14c6a-246">Nie można zmodyfikować poświadczenia uwierzytelniania, gdy połączenie jest aktywne; Aby odświeżyć poświadczenia, połączenie musi zatrzymana i uruchomiona ponownie.</span><span class="sxs-lookup"><span data-stu-id="14c6a-246">Authentication credentials cannot be modified while a connection is active; to refresh credentials, the connection must be stopped and restarted.</span></span>

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a><span data-ttu-id="14c6a-247">OnConnected jest wywoływana dwukrotnie przy użyciu jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="14c6a-247">OnConnected gets called twice when using jQuery Mobile</span></span>

<span data-ttu-id="14c6a-248">jQuery Mobile `initializePage` funkcja wymusza skryptów na każdej stronie, które mają być wykonane ponownie, co powoduje utworzenie drugiego połączenia.</span><span class="sxs-lookup"><span data-stu-id="14c6a-248">jQuery Mobile's `initializePage` function forces the scripts in each page to be re-executed, thus creating a second connection.</span></span> <span data-ttu-id="14c6a-249">Rozwiązania tego problemu obejmują:</span><span class="sxs-lookup"><span data-stu-id="14c6a-249">Solutions for this issue include:</span></span>

- <span data-ttu-id="14c6a-250">Zawiera odniesienie do przed plik JavaScript, jQuery Mobile.</span><span class="sxs-lookup"><span data-stu-id="14c6a-250">Include the reference to jQuery Mobile before your JavaScript file.</span></span>
- <span data-ttu-id="14c6a-251">Wyłącz `initializePage` funkcja przez ustawienie `$.mobile.autoInitializePage = false`.</span><span class="sxs-lookup"><span data-stu-id="14c6a-251">Disable the `initializePage` function by setting `$.mobile.autoInitializePage = false`.</span></span>
- <span data-ttu-id="14c6a-252">Poczekaj na stronie, aby zakończyć inicjowanie przed rozpoczęciem połączenia.</span><span class="sxs-lookup"><span data-stu-id="14c6a-252">Wait for the page to finish initializing before starting the connection.</span></span>

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a><span data-ttu-id="14c6a-253">Komunikaty są opóźnione w aplikacji Silverlight przy użyciu zdarzenia wysyłane serwera</span><span class="sxs-lookup"><span data-stu-id="14c6a-253">Messages are delayed in Silverlight applications using Server Sent Events</span></span>

<span data-ttu-id="14c6a-254">Przy użyciu serwera wysyłane zdarzenia na Silverlight opóźnienia wiadomości.</span><span class="sxs-lookup"><span data-stu-id="14c6a-254">Messages are delayed when using server sent events on Silverlight.</span></span> <span data-ttu-id="14c6a-255">Aby wymusić długi sondowania zamiast tego należy użyć, należy użyć następującego po rozpoczęciu połączenia:</span><span class="sxs-lookup"><span data-stu-id="14c6a-255">To force long polling to be used instead, use the following when starting the connection:</span></span>

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a><span data-ttu-id="14c6a-256">Nieskończona "Odmowa uprawnienia" za pomocą ramki protokołu</span><span class="sxs-lookup"><span data-stu-id="14c6a-256">"Permission Denied" using Forever Frame protocol</span></span>

<span data-ttu-id="14c6a-257">Jest to znany problem opisano [tutaj](https://github.com/SignalR/SignalR/issues/1963).</span><span class="sxs-lookup"><span data-stu-id="14c6a-257">This is a known issue, described [here](https://github.com/SignalR/SignalR/issues/1963).</span></span> <span data-ttu-id="14c6a-258">Objawem tego mogą być widoczne za pomocą najnowszej biblioteki JQuery; Obejście polega na starszą wersję aplikacji JQuery 1.8.2.</span><span class="sxs-lookup"><span data-stu-id="14c6a-258">This symptom may be seen using the latest JQuery library; the workaround is to downgrade your application to JQuery 1.8.2.</span></span>

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a><span data-ttu-id="14c6a-259">Błędy kompilacji i po stronie serwera</span><span class="sxs-lookup"><span data-stu-id="14c6a-259">Compilation and server-side errors</span></span>

 <span data-ttu-id="14c6a-260">Poniższa sekcja zawiera możliwych rozwiązań kompilatora i błędy podczas wykonywania po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="14c6a-260">The following section contains possible solutions to compiler and server-side runtime errors.</span></span> 

### <a name="reference-to-hub-instance-is-null"></a><span data-ttu-id="14c6a-261">Odwołanie do wystąpienia Centrum ma wartość null</span><span class="sxs-lookup"><span data-stu-id="14c6a-261">Reference to Hub instance is null</span></span>

<span data-ttu-id="14c6a-262">Ponieważ wystąpienie koncentratora jest tworzony dla każdego połączenia, nie można utworzyć wystąpienia koncentratora w kodzie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="14c6a-262">Since a hub instance is created for each connection, you can't create an instance of a hub in your code yourself.</span></span> <span data-ttu-id="14c6a-263">Aby wywołać metod w koncentratorze z poza samego koncentratora, zobacz [jak wywoływać metod klienta i zarządzanie grupami z poza klasą Centrum](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) dotyczące sposobu uzyskania odwołania do kontekst koncentratora.</span><span class="sxs-lookup"><span data-stu-id="14c6a-263">To call methods on a hub from outside the hub itself, see [How to call client methods and manage groups from outside the Hub class](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) for how to obtain a reference to the hub context.</span></span>

### <a name="httpcontextcurrentsession-is-null"></a><span data-ttu-id="14c6a-264">HTTPContext.Current.Session ma wartość null</span><span class="sxs-lookup"><span data-stu-id="14c6a-264">HTTPContext.Current.Session is null</span></span>

<span data-ttu-id="14c6a-265">To zachowanie jest celowe.</span><span class="sxs-lookup"><span data-stu-id="14c6a-265">This behavior is by design.</span></span> <span data-ttu-id="14c6a-266">SignalR nie obsługuje stanu sesji ASP.NET, ponieważ włączenie stanu sesji spowoduje przerwanie dupleksowy do obsługi komunikatów.</span><span class="sxs-lookup"><span data-stu-id="14c6a-266">SignalR does not support the ASP.NET session state, since enabling the session state would break duplex messaging.</span></span>

### <a name="no-suitable-method-to-override"></a><span data-ttu-id="14c6a-267">Nie odpowiedniej metody do przesłonięcia</span><span class="sxs-lookup"><span data-stu-id="14c6a-267">No suitable method to override</span></span>

<span data-ttu-id="14c6a-268">Ten błąd może być wyświetlana, jeśli używasz kodu ze starszą dokumentacją lub blogów.</span><span class="sxs-lookup"><span data-stu-id="14c6a-268">You may see this error if you are using code from older documentation or blogs.</span></span> <span data-ttu-id="14c6a-269">Sprawdź, czy użytkownik nie odwołują się do nazwy metody, które zostały zmienione lub przestarzałe (takie jak `OnConnectedAsync`).</span><span class="sxs-lookup"><span data-stu-id="14c6a-269">Verify that you are not referencing names of methods that have been changed or deprecated (like `OnConnectedAsync`).</span></span>

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a><span data-ttu-id="14c6a-270">HostContextExtensions.WebSocketServerUrl ma wartość null</span><span class="sxs-lookup"><span data-stu-id="14c6a-270">HostContextExtensions.WebSocketServerUrl is null</span></span>

<span data-ttu-id="14c6a-271">To zachowanie jest celowe.</span><span class="sxs-lookup"><span data-stu-id="14c6a-271">This behavior is by design.</span></span> <span data-ttu-id="14c6a-272">Ten element członkowski jest przestarzały i nie powinna być używana.</span><span class="sxs-lookup"><span data-stu-id="14c6a-272">This member is deprecated and should not be used.</span></span>

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a><span data-ttu-id="14c6a-273">Trasa o nazwie "signalr.hubs" błąd "jest już w kolekcji tras"</span><span class="sxs-lookup"><span data-stu-id="14c6a-273">"A route named 'signalr.hubs' is already in the route collection" error</span></span>

<span data-ttu-id="14c6a-274">Ten błąd będzie widoczny, gdy `MapHubs` jest wywoływana dwukrotnie przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="14c6a-274">This error will be seen if `MapHubs` is called twice by your application.</span></span> <span data-ttu-id="14c6a-275">Niektóre przykładowe aplikacje wywołania `MapHubs` bezpośrednio w pliku globalnego aplikacji; innymi wywoływania klasy otoki.</span><span class="sxs-lookup"><span data-stu-id="14c6a-275">Some example applications call `MapHubs` directly in the global application file; others make the call in a wrapper class.</span></span> <span data-ttu-id="14c6a-276">Upewnij się, że aplikacja nie wykonać obie czynności.</span><span class="sxs-lookup"><span data-stu-id="14c6a-276">Ensure that your application does not do both.</span></span>

<a id="vs"></a>

## <a name="visual-studio-issues"></a><span data-ttu-id="14c6a-277">Problemy z usługą Visual Studio</span><span class="sxs-lookup"><span data-stu-id="14c6a-277">Visual Studio issues</span></span>

<span data-ttu-id="14c6a-278">W tej sekcji opisano problemy w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14c6a-278">This section describes issues encountered in Visual Studio.</span></span>

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a><span data-ttu-id="14c6a-279">W Eksploratorze rozwiązań nie ma węzła dokumentów skryptu</span><span class="sxs-lookup"><span data-stu-id="14c6a-279">Script Documents node does not appear in Solution Explorer</span></span>

<span data-ttu-id="14c6a-280">Niektóre z naszymi samouczkami kierujące do węzła "Dokumentach skryptów" w Eksploratorze rozwiązań podczas debugowania.</span><span class="sxs-lookup"><span data-stu-id="14c6a-280">Some of our tutorials direct you to the "Script Documents" node in Solution Explorer while debugging.</span></span> <span data-ttu-id="14c6a-281">Ten węzeł jest generowany przez debuger JavaScript i zostanie wyświetlony tylko podczas debugowania klientów w przeglądarkach w programie Internet Explorer; węzeł nie będą wyświetlane, jeśli są używane Chrome lub Firefox.</span><span class="sxs-lookup"><span data-stu-id="14c6a-281">This node is produced by the JavaScript debugger, and will only appear while debugging browser clients in Internet Explorer; the node will not appear if Chrome or Firefox are used.</span></span> <span data-ttu-id="14c6a-282">Debuger JavaScript również nie będzie działać, jeśli jest uruchomiony inny debuger klienta, takich jak debugera Silverlight.</span><span class="sxs-lookup"><span data-stu-id="14c6a-282">The JavaScript debugger will also not run if another client debugger is running, such as the Silverlight debugger.</span></span>

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a><span data-ttu-id="14c6a-283">SignalR nie działa w programie Visual Studio 2008 lub starszym</span><span class="sxs-lookup"><span data-stu-id="14c6a-283">SignalR does not work on Visual Studio 2008 or earlier</span></span>

<span data-ttu-id="14c6a-284">To zachowanie jest celowe.</span><span class="sxs-lookup"><span data-stu-id="14c6a-284">This behavior is by design.</span></span> <span data-ttu-id="14c6a-285">SignalR wymaga programu .NET Framework 4 lub nowszy; wymaga to opracowanych aplikacji SignalR w Visual Studio 2010 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="14c6a-285">SignalR requires .NET Framework 4 or later; this requires that SignalR applications be developed in Visual Studio 2010 or later.</span></span>

<a id="iis"></a>

## <a name="iis-issues"></a><span data-ttu-id="14c6a-286">Problemy z usług IIS</span><span class="sxs-lookup"><span data-stu-id="14c6a-286">IIS issues</span></span>

<span data-ttu-id="14c6a-287">Ta sekcja zawiera problemy z internetowych usług informacyjnych.</span><span class="sxs-lookup"><span data-stu-id="14c6a-287">This section contains issues with Internet Information Services.</span></span>

### <a name="web-site-crashes-after-maphubs-call"></a><span data-ttu-id="14c6a-288">Witryna sieci Web ulegnie awarii po MapHubs wywołania</span><span class="sxs-lookup"><span data-stu-id="14c6a-288">Web site crashes after MapHubs call</span></span>

<span data-ttu-id="14c6a-289">Ten problem został rozwiązany w najnowszej wersji biblioteki signalr.</span><span class="sxs-lookup"><span data-stu-id="14c6a-289">This issue has been fixed in the latest version of SignalR.</span></span> <span data-ttu-id="14c6a-290">Sprawdź, że używasz najnowszej wersji biblioteki signalr, aktualizując instalacji przy użyciu narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="14c6a-290">Verify that you are using the latest released version of SignalR by updating your installation using NuGet.</span></span>

<a id="azure"></a>

## <a name="azure-issues"></a><span data-ttu-id="14c6a-291">Problemy z platformy Azure</span><span class="sxs-lookup"><span data-stu-id="14c6a-291">Azure issues</span></span>

<span data-ttu-id="14c6a-292">Ta sekcja zawiera problemy w systemie Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="14c6a-292">This section contains issues with Microsoft Azure.</span></span>

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a><span data-ttu-id="14c6a-293">Komunikaty nie są odbierane za pośrednictwem usługi Azure płyty montażowej po zmieniania nazwy tematów</span><span class="sxs-lookup"><span data-stu-id="14c6a-293">Messages are not received through the Azure backplane after altering topic names</span></span>

<span data-ttu-id="14c6a-294">Tematy używane przez usługi Azure płyty montażowej nie są przeznaczone do można konfigurować użytkownika.</span><span class="sxs-lookup"><span data-stu-id="14c6a-294">The topics used by the Azure backplane are not intended to be user-configurable.</span></span>
