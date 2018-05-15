---
title: Program ASP.NET Core SignalR JavaScript klienta
author: rachelappel
description: Przegląd klienta platformy ASP.NET Core SignalR JavaScript.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/09/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: 1701d9ac5222bf64f9690c1cecdf54ef95fe4a49
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="73007-103">Program ASP.NET Core SignalR JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="73007-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="73007-104">Przez [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="73007-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="73007-105">Biblioteka klienta platformy ASP.NET Core SignalR JavaScript umożliwia deweloperom wywołanie kodu koncentratora po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="73007-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="73007-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="73007-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="73007-107">Zainstaluj pakiet klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="73007-107">Install the SignalR client package</span></span>

<span data-ttu-id="73007-108">Biblioteka klienta SignalR JavaScript jest dostarczana jako [npm](https://www.npmjs.com/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="73007-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="73007-109">Jeśli używasz programu Visual Studio, uruchomić `npm install` z **Konsola Menedżera pakietów** while w folderze głównym.</span><span class="sxs-lookup"><span data-stu-id="73007-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="73007-110">Dla programu Visual Studio Code, uruchom polecenie z **zintegrowane Terminal**.</span><span class="sxs-lookup"><span data-stu-id="73007-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="73007-111">Npm instaluje zawartość pakietu w *node_modules\\ @aspnet\signalr\dist\browser*  folderu.</span><span class="sxs-lookup"><span data-stu-id="73007-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="73007-112">Utwórz nowy folder o nazwie *signalr* w obszarze *wwwroot\\lib* folderu.</span><span class="sxs-lookup"><span data-stu-id="73007-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="73007-113">Kopiuj *signalr.js* pliku *wwwroot\lib\signalr* folderu.</span><span class="sxs-lookup"><span data-stu-id="73007-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="73007-114">Korzystanie z klienta SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="73007-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="73007-115">Odwołanie klienta SignalR JavaScript w `<script>` elementu.</span><span class="sxs-lookup"><span data-stu-id="73007-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="73007-116">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="73007-116">Connect to a hub</span></span>

<span data-ttu-id="73007-117">Poniższy kod tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="73007-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="73007-118">Nazwa koncentratora jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="73007-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="73007-119">Połączenia między źródłami</span><span class="sxs-lookup"><span data-stu-id="73007-119">Cross-origin connections</span></span>

<span data-ttu-id="73007-120">Zazwyczaj przeglądarki załadować połączeń z tej samej domenie co żądanej strony.</span><span class="sxs-lookup"><span data-stu-id="73007-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="73007-121">Istnieją jednak sytuacji, gdy wymagane jest połączenie z innej domeny.</span><span class="sxs-lookup"><span data-stu-id="73007-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="73007-122">Aby zapobiec złośliwa witryna odczytywanie danych poufnych z innej lokacji, [połączeń między źródłami](xref:security/cors) są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="73007-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="73007-123">Aby zezwolić na żądanie między źródłami, włączyć go w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="73007-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="73007-124">Wywoływanie metod koncentratora od klienta</span><span class="sxs-lookup"><span data-stu-id="73007-124">Call hub methods from client</span></span>

<span data-ttu-id="73007-125">Klientów języka JavaScript wywołanie metody publiczne na koncentratorów przez przy użyciu `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="73007-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="73007-126">`invoke` Metoda przyjmuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="73007-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="73007-127">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="73007-127">The name of the hub method.</span></span> <span data-ttu-id="73007-128">W poniższym przykładzie nazwa Centrum jest `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="73007-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="73007-129">Żadnych argumentów w metodzie koncentratora.</span><span class="sxs-lookup"><span data-stu-id="73007-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="73007-130">W poniższym przykładzie jest nazwa argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="73007-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="73007-131">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="73007-131">Call client methods from hub</span></span>

<span data-ttu-id="73007-132">Aby odbierać komunikaty z koncentratora, zdefiniować przy użyciu metody `connection.on` metody.</span><span class="sxs-lookup"><span data-stu-id="73007-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="73007-133">Nazwa metody JavaScript klienta.</span><span class="sxs-lookup"><span data-stu-id="73007-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="73007-134">W poniższym przykładzie nazwa metody jest `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="73007-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="73007-135">Argumenty koncentratora przekazuje do metody.</span><span class="sxs-lookup"><span data-stu-id="73007-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="73007-136">W poniższym przykładzie jest wartość argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="73007-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="73007-137">Poprzedni kod w `connection.on` jest uruchamiany, gdy kod po stronie serwera wywołuje za pomocą `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="73007-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-javascript[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="73007-138">Określa, która metoda klienta do wywołania przez dopasowanie nazwy metody SignalR i argumenty zdefiniowanych w `SendAsync` i `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="73007-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="73007-139">Jako najlepsze rozwiązanie należy wywołać `connection.start` po `connection.on` tak programu obsługi są zarejestrowane przed komunikaty są odbierane.</span><span class="sxs-lookup"><span data-stu-id="73007-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="73007-140">Obsługa błędów i rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="73007-140">Error handling and logging</span></span>

<span data-ttu-id="73007-141">Łańcuch `catch` metody na końcu `connection.start` sposób obsługi błędów po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="73007-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="73007-142">Użyj `console.error` błędy dane wyjściowe do konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="73007-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="73007-143">Konfiguracja po stronie klienta dziennika śledzenia przez przekazanie rejestratora i typ zdarzenia logowania po nawiązaniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="73007-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="73007-144">Komunikaty są rejestrowane na poziomie określonego dziennika i wyższych.</span><span class="sxs-lookup"><span data-stu-id="73007-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="73007-145">Poziomy rejestrowania dostępne są następujące:</span><span class="sxs-lookup"><span data-stu-id="73007-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="73007-146">`signalR.LogLevel.Error` : Komunikaty wystąpił błąd.</span><span class="sxs-lookup"><span data-stu-id="73007-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="73007-147">Dzienniki `Error` tylko komunikaty.</span><span class="sxs-lookup"><span data-stu-id="73007-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="73007-148">`signalR.LogLevel.Warning` : Ostrzeżenie komunikaty o potencjalnych błędów.</span><span class="sxs-lookup"><span data-stu-id="73007-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="73007-149">Dzienniki `Warning`, i `Error` wiadomości.</span><span class="sxs-lookup"><span data-stu-id="73007-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="73007-150">`signalR.LogLevel.Information` : Komunikaty o stanie bez błędów.</span><span class="sxs-lookup"><span data-stu-id="73007-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="73007-151">Dzienniki `Information`, `Warning`, i `Error` wiadomości.</span><span class="sxs-lookup"><span data-stu-id="73007-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="73007-152">`signalR.LogLevel.Trace` : Śledzenia wiadomości.</span><span class="sxs-lookup"><span data-stu-id="73007-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="73007-153">Rejestruje wszystkim łącznie z danych przesyłanych między klientem koncentratora.</span><span class="sxs-lookup"><span data-stu-id="73007-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="73007-154">Użyj `configureLogging` metoda `HubConnectionBuilder` skonfigurować poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="73007-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="73007-155">Komunikaty są rejestrowane w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="73007-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="73007-156">Zasoby pokrewne</span><span class="sxs-lookup"><span data-stu-id="73007-156">Related resources</span></span>

* [<span data-ttu-id="73007-157">Koncentratory SignalR platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73007-157">ASP.NET Core SignalR Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="73007-158">Włącz żądania między źródłami (CORS) w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73007-158">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)