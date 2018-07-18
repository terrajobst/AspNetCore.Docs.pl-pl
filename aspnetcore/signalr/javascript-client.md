---
title: ASP.NET Core SignalR JavaScript klienta
author: tdykstra
description: Omówienie platformy ASP.NET Core SignalR JavaScript klienta.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: c13c41b0344b0c880e842f2799d6ee97bd7fff7e
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095427"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="9b369-103">ASP.NET Core SignalR JavaScript klienta</span><span class="sxs-lookup"><span data-stu-id="9b369-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="9b369-104">Przez [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="9b369-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="9b369-105">Biblioteki klienta platformy ASP.NET Core SignalR JavaScript umożliwia deweloperom wywołanie kodu koncentratora po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="9b369-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="9b369-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9b369-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="9b369-107">Zainstaluj pakiet klienta SignalR</span><span class="sxs-lookup"><span data-stu-id="9b369-107">Install the SignalR client package</span></span>

<span data-ttu-id="9b369-108">Biblioteka klienta SignalR JavaScript jest dostarczana jako [npm](https://www.npmjs.com/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="9b369-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="9b369-109">Jeśli używasz programu Visual Studio, uruchom `npm install` z **Konsola Menedżera pakietów** podczas gdy w folderze głównym.</span><span class="sxs-lookup"><span data-stu-id="9b369-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="9b369-110">Dla programu Visual Studio Code, uruchom polecenie **zintegrowany Terminal**.</span><span class="sxs-lookup"><span data-stu-id="9b369-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="9b369-111">Npm instaluje zawartość pakietu w *node_modules\\ @aspnet\signalr\dist\browser*  folderu.</span><span class="sxs-lookup"><span data-stu-id="9b369-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="9b369-112">Utwórz nowy folder o nazwie *signalr* w obszarze *wwwroot\\lib* folderu.</span><span class="sxs-lookup"><span data-stu-id="9b369-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="9b369-113">Kopiuj *signalr.js* plik *wwwroot\lib\signalr* folderu.</span><span class="sxs-lookup"><span data-stu-id="9b369-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="9b369-114">Używanie klienta SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="9b369-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="9b369-115">Odwoływać się do klienta SignalR JavaScript w `<script>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9b369-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="9b369-116">Połączenia z koncentratorem</span><span class="sxs-lookup"><span data-stu-id="9b369-116">Connect to a hub</span></span>

<span data-ttu-id="9b369-117">Poniższy kod tworzy i uruchamia połączenie.</span><span class="sxs-lookup"><span data-stu-id="9b369-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="9b369-118">Nazwa Centrum jest uwzględniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="9b369-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="9b369-119">Połączenia między źródłami</span><span class="sxs-lookup"><span data-stu-id="9b369-119">Cross-origin connections</span></span>

<span data-ttu-id="9b369-120">Zazwyczaj przeglądarek ładowanie połączeń z tej samej domenie co żądanej strony.</span><span class="sxs-lookup"><span data-stu-id="9b369-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="9b369-121">Istnieją jednak sytuacje, gdy wymagane jest połączenie do innej domeny.</span><span class="sxs-lookup"><span data-stu-id="9b369-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="9b369-122">Aby zapobiec złośliwych witryn odczytywanie poufnych danych z innej lokacji [połączeń cross-origin](xref:security/cors) są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="9b369-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="9b369-123">Aby zezwolić na żądania między źródłami, włącz go w `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="9b369-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="9b369-124">Wywoływanie metod koncentratora z klienta</span><span class="sxs-lookup"><span data-stu-id="9b369-124">Call hub methods from client</span></span>

<span data-ttu-id="9b369-125">Klientów JavaScript wywołanie metod publicznych w centrach przez przy użyciu `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="9b369-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="9b369-126">`invoke` Metoda przyjmuje dwa argumenty:</span><span class="sxs-lookup"><span data-stu-id="9b369-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="9b369-127">Nazwa metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="9b369-127">The name of the hub method.</span></span> <span data-ttu-id="9b369-128">W poniższym przykładzie jest nazwa centrum `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="9b369-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="9b369-129">Argumenty zdefiniowane w metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="9b369-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="9b369-130">W poniższym przykładzie jest nazwa argumentu `message`.</span><span class="sxs-lookup"><span data-stu-id="9b369-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="9b369-131">Wywoływanie metody klienta z Centrum</span><span class="sxs-lookup"><span data-stu-id="9b369-131">Call client methods from hub</span></span>

<span data-ttu-id="9b369-132">Aby odbierać komunikaty z Centrum, definiowanie metody przy użyciu `connection.on` metody.</span><span class="sxs-lookup"><span data-stu-id="9b369-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="9b369-133">Nazwa metody klienta JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9b369-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="9b369-134">W poniższym przykładzie nazwa metody jest `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="9b369-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="9b369-135">Argumenty Centrum przekazuje do metody.</span><span class="sxs-lookup"><span data-stu-id="9b369-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="9b369-136">W poniższym przykładzie wartość argumentu jest `message`.</span><span class="sxs-lookup"><span data-stu-id="9b369-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="9b369-137">Powyższy kod w `connection.on` jest uruchamiany, gdy wywołuje kod po stronie serwera, za pomocą `SendAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="9b369-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="9b369-138">Określa SignalR, jakiej metody klienta do wywołania, dopasowując nazwy metody i argumenty zdefiniowane w `SendAsync` i `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="9b369-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="9b369-139">Najlepszym rozwiązaniem, należy wywołać `connection.start` po `connection.on` , dzięki czemu inne programy obsługi są rejestrowane, zanim jakiekolwiek komunikaty są odbierane.</span><span class="sxs-lookup"><span data-stu-id="9b369-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="9b369-140">Rejestrowanie i obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="9b369-140">Error handling and logging</span></span>

<span data-ttu-id="9b369-141">Łańcuch `catch` metody do końca `connection.start` metodę, aby obsługiwać błędy po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="9b369-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="9b369-142">Użyj `console.error` błędy dane wyjściowe do konsoli w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="9b369-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="9b369-143">Konfiguracja po stronie klienta dziennika śledzenia, przekazując rejestratora i typu zdarzenia logowania, gdy połączenie zostało nawiązane.</span><span class="sxs-lookup"><span data-stu-id="9b369-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="9b369-144">Komunikaty są rejestrowane, z poziomu określonego dziennika lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="9b369-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="9b369-145">Poziomy dziennika dostępne są następujące:</span><span class="sxs-lookup"><span data-stu-id="9b369-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="9b369-146">`signalR.LogLevel.Error` : Komunikaty o błędach.</span><span class="sxs-lookup"><span data-stu-id="9b369-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="9b369-147">Dzienniki `Error` tylko komunikaty.</span><span class="sxs-lookup"><span data-stu-id="9b369-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="9b369-148">`signalR.LogLevel.Warning` : Ostrzeżenie komunikaty o potencjalnych błędów.</span><span class="sxs-lookup"><span data-stu-id="9b369-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="9b369-149">Dzienniki `Warning`, i `Error` wiadomości.</span><span class="sxs-lookup"><span data-stu-id="9b369-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="9b369-150">`signalR.LogLevel.Information` : Komunikaty o stanie bez błędów.</span><span class="sxs-lookup"><span data-stu-id="9b369-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="9b369-151">Dzienniki `Information`, `Warning`, i `Error` wiadomości.</span><span class="sxs-lookup"><span data-stu-id="9b369-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="9b369-152">`signalR.LogLevel.Trace` : Komunikaty śledzenia.</span><span class="sxs-lookup"><span data-stu-id="9b369-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="9b369-153">Rejestruje wszystkim, łącznie z danych przesyłanych między Centrum a klientem.</span><span class="sxs-lookup"><span data-stu-id="9b369-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="9b369-154">Użyj `configureLogging` metody `HubConnectionBuilder` skonfigurować poziom dziennika.</span><span class="sxs-lookup"><span data-stu-id="9b369-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="9b369-155">Komunikaty są rejestrowane w konsoli przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="9b369-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="9b369-156">Powiązane zasoby</span><span class="sxs-lookup"><span data-stu-id="9b369-156">Related resources</span></span>

* [<span data-ttu-id="9b369-157">Centra</span><span class="sxs-lookup"><span data-stu-id="9b369-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9b369-158">Klient .NET</span><span class="sxs-lookup"><span data-stu-id="9b369-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="9b369-159">Publikowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="9b369-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="9b369-160">Włączanie żądań Cross-Origin (CORS) w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b369-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
