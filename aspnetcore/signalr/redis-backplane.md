---
title: Redis plan dla SignalR skalowanie ASP.NET Core w poziomie
author: bradygaster
description: Dowiedz się, jak skonfigurować plan Redis, aby włączyć skalowanie w poziomie dla aplikacji SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/redis-backplane
ms.openlocfilehash: 379d46fcaabb8eb0d04e521a5ad698229f947b7c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963918"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-opno-locsignalr-scale-out"></a><span data-ttu-id="207da-103">Konfigurowanie planu Redis dla ASP.NET Core SignalR skalowanie w poziomie</span><span class="sxs-lookup"><span data-stu-id="207da-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="207da-104">Autorzy [Andrew Stanton-pielęgniarki](https://twitter.com/anurse), [Brady Gastera](https://twitter.com/bradygaster)i [Tomasz Dykstra](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="207da-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="207da-105">W tym artykule wyjaśniono SignalRaspekty dotyczące konfigurowania serwera [Redis](https://redis.io/) do użycia w celu skalowania aplikacji ASP.NET Core SignalR.</span><span class="sxs-lookup"><span data-stu-id="207da-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="207da-106">Konfigurowanie planu Redis</span><span class="sxs-lookup"><span data-stu-id="207da-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="207da-107">Wdróż serwer Redis.</span><span class="sxs-lookup"><span data-stu-id="207da-107">Deploy a Redis server.</span></span>

  > [!IMPORTANT] 
  > <span data-ttu-id="207da-108">W przypadku użycia w środowisku produkcyjnym Redis planuje się tylko wtedy, gdy działa on w tym samym centrum danych co aplikacja SignalR.</span><span class="sxs-lookup"><span data-stu-id="207da-108">For production use, a Redis backplane is recommended only when it runs in the same data center as the SignalR app.</span></span> <span data-ttu-id="207da-109">W przeciwnym razie opóźnienie sieci obniży wydajność.</span><span class="sxs-lookup"><span data-stu-id="207da-109">Otherwise, network latency degrades performance.</span></span> <span data-ttu-id="207da-110">Jeśli Twoja aplikacja SignalR jest uruchomiona w chmurze platformy Azure, zalecamy użycie usługi Azure SignalR zamiast planu Redis.</span><span class="sxs-lookup"><span data-stu-id="207da-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="207da-111">Usługi Azure Redis Cache można używać w środowiskach deweloperskich i testowych.</span><span class="sxs-lookup"><span data-stu-id="207da-111">You can use the Azure Redis Cache Service for development and test environments.</span></span>

  <span data-ttu-id="207da-112">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="207da-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="207da-113">Dokumentacja Redis</span><span class="sxs-lookup"><span data-stu-id="207da-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="207da-114">Dokumentacja Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="207da-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="207da-115">W aplikacji SignalR Zainstaluj pakiet NuGet `Microsoft.AspNetCore.SignalR.Redis`.</span><span class="sxs-lookup"><span data-stu-id="207da-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span> <span data-ttu-id="207da-116">(Istnieje również pakiet `Microsoft.AspNetCore.SignalR.StackExchangeRedis`, ale jest on przeznaczony dla ASP.NET Core 2,2 i nowszych).</span><span class="sxs-lookup"><span data-stu-id="207da-116">(There is also a `Microsoft.AspNetCore.SignalR.StackExchangeRedis` package, but that one is for ASP.NET Core 2.2 and later.)</span></span>

* <span data-ttu-id="207da-117">W metodzie `Startup.ConfigureServices` Wywołaj `AddRedis` po `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="207da-117">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="207da-118">Skonfiguruj opcje zgodnie z wymaganiami:</span><span class="sxs-lookup"><span data-stu-id="207da-118">Configure options as needed:</span></span>
 
  <span data-ttu-id="207da-119">Większość opcji można ustawić w parametrach połączenia lub w obiekcie [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) .</span><span class="sxs-lookup"><span data-stu-id="207da-119">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="207da-120">Opcje określone w `ConfigurationOptions` przesłaniają te ustawienia w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="207da-120">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="207da-121">Poniższy przykład pokazuje, jak ustawić opcje w obiekcie `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="207da-121">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="207da-122">Ten przykład dodaje prefiks kanału, dzięki czemu wiele aplikacji może współużytkować to samo wystąpienie Redis, co zostało wyjaśnione w poniższym kroku.</span><span class="sxs-lookup"><span data-stu-id="207da-122">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="207da-123">W powyższym kodzie, `options.Configuration` jest inicjowany z dowolnego rodzaju określono w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="207da-123">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="207da-124">W aplikacji SignalR Zainstaluj jeden z następujących pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="207da-124">In the SignalR app, install one of the following NuGet packages:</span></span>

  * <span data-ttu-id="207da-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` — zależy od StackExchange. Redis 2. X.X.</span><span class="sxs-lookup"><span data-stu-id="207da-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` - Depends on StackExchange.Redis 2.X.X.</span></span> <span data-ttu-id="207da-126">Jest to zalecany pakiet dla ASP.NET Core 2,2 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="207da-126">This is the recommended package for ASP.NET Core 2.2 and later.</span></span>
  * <span data-ttu-id="207da-127">`Microsoft.AspNetCore.SignalR.Redis` — zależy od StackExchange. Redis 1. X.X.</span><span class="sxs-lookup"><span data-stu-id="207da-127">`Microsoft.AspNetCore.SignalR.Redis` - Depends on StackExchange.Redis 1.X.X.</span></span> <span data-ttu-id="207da-128">Ten pakiet nie będzie wysyłany w ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="207da-128">This package will not be shipping in ASP.NET Core 3.0.</span></span>

* <span data-ttu-id="207da-129">W metodzie `Startup.ConfigureServices` Wywołaj `AddStackExchangeRedis` po `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="207da-129">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="207da-130">Skonfiguruj opcje zgodnie z wymaganiami:</span><span class="sxs-lookup"><span data-stu-id="207da-130">Configure options as needed:</span></span>
 
  <span data-ttu-id="207da-131">Większość opcji można ustawić w parametrach połączenia lub w obiekcie [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) .</span><span class="sxs-lookup"><span data-stu-id="207da-131">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="207da-132">Opcje określone w `ConfigurationOptions` przesłaniają te ustawienia w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="207da-132">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="207da-133">Poniższy przykład pokazuje, jak ustawić opcje w obiekcie `ConfigurationOptions`.</span><span class="sxs-lookup"><span data-stu-id="207da-133">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="207da-134">Ten przykład dodaje prefiks kanału, dzięki czemu wiele aplikacji może współużytkować to samo wystąpienie Redis, co zostało wyjaśnione w poniższym kroku.</span><span class="sxs-lookup"><span data-stu-id="207da-134">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="207da-135">W powyższym kodzie, `options.Configuration` jest inicjowany z dowolnego rodzaju określono w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="207da-135">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="207da-136">Informacje o opcjach Redis można znaleźć w [dokumentacji stackexchange Redis](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="207da-136">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="207da-137">Jeśli używasz jednego serwera Redis dla wielu aplikacji SignalR, użyj innego prefiksu kanału dla każdej aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="207da-137">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="207da-138">Ustawienie prefiksu kanału izoluje jedną aplikację SignalR od innych, które używają różnych prefiksów kanałów.</span><span class="sxs-lookup"><span data-stu-id="207da-138">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="207da-139">Jeśli nie przypiszesz różnych prefiksów, komunikat wysłany z jednej aplikacji do wszystkich klientów będzie przeszedł do wszystkich klientów wszystkich aplikacji korzystających z serwera Redis jako planu wieloplanowego.</span><span class="sxs-lookup"><span data-stu-id="207da-139">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="207da-140">Skonfiguruj oprogramowanie do równoważenia obciążenia farmy serwerów dla sesji programu Sticky Notes.</span><span class="sxs-lookup"><span data-stu-id="207da-140">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="207da-141">Oto kilka przykładów dokumentacji dotyczącej tego, jak to zrobić:</span><span class="sxs-lookup"><span data-stu-id="207da-141">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="207da-142">SERWERZE</span><span class="sxs-lookup"><span data-stu-id="207da-142">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="207da-143">HAProxy</span><span class="sxs-lookup"><span data-stu-id="207da-143">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="207da-144">Nginx</span><span class="sxs-lookup"><span data-stu-id="207da-144">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="207da-145">pfSense</span><span class="sxs-lookup"><span data-stu-id="207da-145">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="207da-146">Błędy serwera Redis</span><span class="sxs-lookup"><span data-stu-id="207da-146">Redis server errors</span></span>

<span data-ttu-id="207da-147">Gdy serwer Redis ulegnie awarii, SignalR zgłasza wyjątki wskazujące, że komunikaty nie zostaną dostarczone.</span><span class="sxs-lookup"><span data-stu-id="207da-147">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="207da-148">Niektóre typowe komunikaty o wyjątkach:</span><span class="sxs-lookup"><span data-stu-id="207da-148">Some typical exception messages:</span></span>

* <span data-ttu-id="207da-149">*Nie można zapisać komunikatu*</span><span class="sxs-lookup"><span data-stu-id="207da-149">*Failed writing message*</span></span>
* <span data-ttu-id="207da-150">*Nie można wywołać metody centrum "MethodName"*</span><span class="sxs-lookup"><span data-stu-id="207da-150">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="207da-151">*Nie można nawiązać połączenia z Redis*</span><span class="sxs-lookup"><span data-stu-id="207da-151">*Connection to Redis failed*</span></span>

SignalR<span data-ttu-id="207da-152"> nie buforuje komunikatów, aby wysłać je, gdy serwer powróci do tworzenia kopii zapasowej.</span><span class="sxs-lookup"><span data-stu-id="207da-152"> doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="207da-153">Wszystkie komunikaty wysyłane w trakcie działania serwera Redis są tracone.</span><span class="sxs-lookup"><span data-stu-id="207da-153">Any messages sent while the Redis server is down are lost.</span></span>

SignalR<span data-ttu-id="207da-154"> automatycznie ponownie nawiązuje połączenie po ponownym udostępnieniu serwera Redis.</span><span class="sxs-lookup"><span data-stu-id="207da-154"> automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="207da-155">Niestandardowe zachowanie dla błędów połączeń</span><span class="sxs-lookup"><span data-stu-id="207da-155">Custom behavior for connection failures</span></span>

<span data-ttu-id="207da-156">Oto przykład, który pokazuje, jak obsługiwać zdarzenia niepowodzeń połączeń Redis.</span><span class="sxs-lookup"><span data-stu-id="207da-156">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="redis-clustering"></a><span data-ttu-id="207da-157">Redis klastrowanie</span><span class="sxs-lookup"><span data-stu-id="207da-157">Redis Clustering</span></span>

<span data-ttu-id="207da-158">[Redis klastrowanie](https://redis.io/topics/cluster-spec) to metoda osiągania wysokiej dostępności przy użyciu wielu serwerów Redis.</span><span class="sxs-lookup"><span data-stu-id="207da-158">[Redis Clustering](https://redis.io/topics/cluster-spec) is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="207da-159">Klastrowanie nie jest oficjalnie obsługiwane, ale może zadziałało.</span><span class="sxs-lookup"><span data-stu-id="207da-159">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="207da-160">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="207da-160">Next steps</span></span>

<span data-ttu-id="207da-161">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="207da-161">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="207da-162">Dokumentacja Redis</span><span class="sxs-lookup"><span data-stu-id="207da-162">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="207da-163">Dokumentacja StackExchange Redis</span><span class="sxs-lookup"><span data-stu-id="207da-163">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="207da-164">Dokumentacja Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="207da-164">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)
