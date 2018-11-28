---
title: Redis płyty montażowej do biblioteki SignalR platformy ASP.NET Core skalowalnego w poziomie
author: tdykstra
description: Dowiedz się, jak skonfigurować montażowa pamięci podręcznej Redis, aby umożliwić skalowanie aplikacji biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: c8b09c0d482da344b54d167c0c9757167eaa6186
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52453027"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a><span data-ttu-id="47c4d-103">Konfigurowanie systemu backplane Redis dla biblioteki SignalR platformy ASP.NET Core skalowalnego w poziomie</span><span class="sxs-lookup"><span data-stu-id="47c4d-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="47c4d-104">Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse), [Brady'ego Gastera](https://twitter.com/bradygaster), i [Tom Dykstra](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="47c4d-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="47c4d-105">W tym artykule wyjaśniono aspekty specyficzne dla SignalR Konfigurowanie [Redis](https://redis.io/) serwer na potrzeby skalowania aplikacji biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="47c4d-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="47c4d-106">Konfigurowanie montażowa pamięci podręcznej Redis</span><span class="sxs-lookup"><span data-stu-id="47c4d-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="47c4d-107">Wdrażanie serwera Redis.</span><span class="sxs-lookup"><span data-stu-id="47c4d-107">Deploy a Redis server.</span></span>

  <span data-ttu-id="47c4d-108">Do użytku produkcyjnego montażowa Redis jest zalecane tylko w przypadku infrastruktury lokalnej.</span><span class="sxs-lookup"><span data-stu-id="47c4d-108">For production use, a Redis backplane is recommended only for on-premises infrastructure.</span></span> <span data-ttu-id="47c4d-109">Aby zminimalizować opóźnienie, serwer Redis powinien być w tym samym centrum danych jako aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="47c4d-109">To minimize latency, the Redis server should be in the same data center as the SignalR app.</span></span> <span data-ttu-id="47c4d-110">Jeśli uruchomiona jest aplikacja SignalR w chmurze platformy Azure, firma Microsoft zaleca Azure SignalR Service zamiast montażowa pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="47c4d-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="47c4d-111">Możesz użyć usługi Azure Redis Cache Service do tworzenia aplikacji i środowisk testowych.</span><span class="sxs-lookup"><span data-stu-id="47c4d-111">You can use the Azure Redis Cache Service for development and test environments.</span></span> <span data-ttu-id="47c4d-112">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="47c4d-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="47c4d-113">Dokumentacja usługi redis</span><span class="sxs-lookup"><span data-stu-id="47c4d-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="47c4d-114">Dokumentacja usługi Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="47c4d-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="47c4d-115">W aplikacji SignalR, należy zainstalować `Microsoft.AspNetCore.SignalR.Redis` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="47c4d-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span>

* <span data-ttu-id="47c4d-116">W `Startup.ConfigureServices` metody, wywołanie `AddRedis` po `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="47c4d-116">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="47c4d-117">Skonfiguruj opcje, zgodnie z potrzebami:</span><span class="sxs-lookup"><span data-stu-id="47c4d-117">Configure options as needed:</span></span>
 
  <span data-ttu-id="47c4d-118">Większość opcji można ustawić w parametrach połączenia lub w [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) obiektu.</span><span class="sxs-lookup"><span data-stu-id="47c4d-118">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="47c4d-119">Opcje określone w `ConfigurationOptions` przesłonięte ustawiony w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="47c4d-119">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="47c4d-120">Poniższy przykład pokazuje, jak ustawić opcje w `ConfigurationOptions` obiektu.</span><span class="sxs-lookup"><span data-stu-id="47c4d-120">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="47c4d-121">Ten przykład dodaje prefiks kanał, aby wiele aplikacji mogą udostępniać tego samego wystąpienia usługi Redis, zgodnie z opisem w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="47c4d-121">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="47c4d-122">W poprzednim kodzie `options.Configuration` jest inicjowany za pomocą dowolnie określono w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="47c4d-122">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="47c4d-123">W aplikacji SignalR, należy zainstalować `Microsoft.AspNetCore.SignalR.StackExchangeRedis` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="47c4d-123">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.StackExchangeRedis` NuGet package.</span></span>

* <span data-ttu-id="47c4d-124">W `Startup.ConfigureServices` metody, wywołanie `AddStackExchangeRedis` po `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="47c4d-124">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="47c4d-125">Skonfiguruj opcje, zgodnie z potrzebami:</span><span class="sxs-lookup"><span data-stu-id="47c4d-125">Configure options as needed:</span></span>
 
  <span data-ttu-id="47c4d-126">Większość opcji można ustawić w parametrach połączenia lub w [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) obiektu.</span><span class="sxs-lookup"><span data-stu-id="47c4d-126">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="47c4d-127">Opcje określone w `ConfigurationOptions` przesłonięte ustawiony w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="47c4d-127">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="47c4d-128">Poniższy przykład pokazuje, jak ustawić opcje w `ConfigurationOptions` obiektu.</span><span class="sxs-lookup"><span data-stu-id="47c4d-128">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="47c4d-129">Ten przykład dodaje prefiks kanał, aby wiele aplikacji mogą udostępniać tego samego wystąpienia usługi Redis, zgodnie z opisem w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="47c4d-129">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="47c4d-130">W poprzednim kodzie `options.Configuration` jest inicjowany za pomocą dowolnie określono w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="47c4d-130">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="47c4d-131">Aby uzyskać informacji o opcjach pamięci podręcznej Redis, zobacz [StackExchange Redis dokumentacji](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="47c4d-131">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="47c4d-132">Jeśli używasz jednego serwera pamięci podręcznej Redis w przypadku wielu aplikacji SignalR, należy używać prefiksu różnych kanałów dla każdej aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="47c4d-132">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="47c4d-133">Ustawianie prefiksu kanału izoluje jednej aplikacji SignalR od innych osób, które używają różnych kanałów prefiksy.</span><span class="sxs-lookup"><span data-stu-id="47c4d-133">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="47c4d-134">Jeśli nie można przypisać różnych prefiksów, wiadomość wysłana z jednej aplikacji do wszystkich jej klienci zaczną się do wszystkich klientów wszystkie aplikacje, które używają serwera Redis jako płyty montażowej.</span><span class="sxs-lookup"><span data-stu-id="47c4d-134">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="47c4d-135">Konfigurowanie usługi serwer farmy równoważenia obciążenia oprogramowania dla trwałych sesji.</span><span class="sxs-lookup"><span data-stu-id="47c4d-135">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="47c4d-136">Poniżej przedstawiono kilka przykładów, dokumentacji, jak to zrobić:</span><span class="sxs-lookup"><span data-stu-id="47c4d-136">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="47c4d-137">IIS</span><span class="sxs-lookup"><span data-stu-id="47c4d-137">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="47c4d-138">HAProxy</span><span class="sxs-lookup"><span data-stu-id="47c4d-138">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="47c4d-139">Nginx</span><span class="sxs-lookup"><span data-stu-id="47c4d-139">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="47c4d-140">pfSense</span><span class="sxs-lookup"><span data-stu-id="47c4d-140">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="47c4d-141">Błędy serwera redis</span><span class="sxs-lookup"><span data-stu-id="47c4d-141">Redis server errors</span></span>

<span data-ttu-id="47c4d-142">Gdy ulegnie awarii serwera Redis, SignalR zgłasza wyjątki, które wskazują, że komunikaty nie będą dostarczane.</span><span class="sxs-lookup"><span data-stu-id="47c4d-142">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="47c4d-143">Niektóre komunikaty o typowych wyjątek:</span><span class="sxs-lookup"><span data-stu-id="47c4d-143">Some typical exception messages:</span></span>

* <span data-ttu-id="47c4d-144">*Zapisywanie nie powiodło się komunikat*</span><span class="sxs-lookup"><span data-stu-id="47c4d-144">*Failed writing message*</span></span>
* <span data-ttu-id="47c4d-145">*Nie można wywołać metody koncentratora "MethodName"*</span><span class="sxs-lookup"><span data-stu-id="47c4d-145">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="47c4d-146">*Połączenie Redis nie powiodło się*</span><span class="sxs-lookup"><span data-stu-id="47c4d-146">*Connection to Redis failed*</span></span>

<span data-ttu-id="47c4d-147">SignalR nie buforuje wiadomości do wysłania ich, gdy serwer wróci do sprawności.</span><span class="sxs-lookup"><span data-stu-id="47c4d-147">SignalR doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="47c4d-148">Wszystkie komunikaty wysłane w czasie, gdy serwer Redis nie działa, zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="47c4d-148">Any messages sent while the Redis server is down are lost.</span></span>

<span data-ttu-id="47c4d-149">Gdy serwer Redis jest dostępny ponownie automatycznie ponownie połączy SignalR.</span><span class="sxs-lookup"><span data-stu-id="47c4d-149">SignalR automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="47c4d-150">Niestandardowe zachowanie błędów połączenia</span><span class="sxs-lookup"><span data-stu-id="47c4d-150">Custom behavior for connection failures</span></span>

<span data-ttu-id="47c4d-151">Oto przykład pokazujący sposób obsługi zdarzenia błędów połączenia Redis.</span><span class="sxs-lookup"><span data-stu-id="47c4d-151">Here's an example that shows how to handle Redis connection failure events.</span></span>

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

::: moniker range="> aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddMessagePackProtocol()
        .AddStackExchangeRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

## <a name="clustering"></a><span data-ttu-id="47c4d-152">Klastrowanie</span><span class="sxs-lookup"><span data-stu-id="47c4d-152">Clustering</span></span>

<span data-ttu-id="47c4d-153">Klastrowanie jest metoda dla osiągnięcia wysokiej dostępności przy użyciu wielu serwerów Redis.</span><span class="sxs-lookup"><span data-stu-id="47c4d-153">Clustering is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="47c4d-154">Klaster nie jest oficjalnie obsługiwany, ale może ona działać.</span><span class="sxs-lookup"><span data-stu-id="47c4d-154">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47c4d-155">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="47c4d-155">Next steps</span></span>

<span data-ttu-id="47c4d-156">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="47c4d-156">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="47c4d-157">Dokumentacja usługi redis</span><span class="sxs-lookup"><span data-stu-id="47c4d-157">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="47c4d-158">Dokumentacja StackExchange Redis</span><span class="sxs-lookup"><span data-stu-id="47c4d-158">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="47c4d-159">Dokumentacja usługi Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="47c4d-159">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)
