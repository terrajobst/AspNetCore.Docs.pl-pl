---
title: Redis płyty montażowej do biblioteki SignalR platformy ASP.NET Core skalowalnego w poziomie
author: bradygaster
description: Dowiedz się, jak skonfigurować montażowa pamięci podręcznej Redis, aby umożliwić skalowanie aplikacji biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: c02d8cd5fb3b6edbb21be4889da2e880099b731b
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837445"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a><span data-ttu-id="4facb-103">Konfigurowanie systemu backplane Redis dla biblioteki SignalR platformy ASP.NET Core skalowalnego w poziomie</span><span class="sxs-lookup"><span data-stu-id="4facb-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="4facb-104">Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse), [Brady'ego Gastera](https://twitter.com/bradygaster), i [Tom Dykstra](https://github.com/tdykstra),</span><span class="sxs-lookup"><span data-stu-id="4facb-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="4facb-105">W tym artykule wyjaśniono aspekty specyficzne dla SignalR Konfigurowanie [Redis](https://redis.io/) serwer na potrzeby skalowania aplikacji biblioteki SignalR platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4facb-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="4facb-106">Konfigurowanie montażowa pamięci podręcznej Redis</span><span class="sxs-lookup"><span data-stu-id="4facb-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="4facb-107">Wdrażanie serwera Redis.</span><span class="sxs-lookup"><span data-stu-id="4facb-107">Deploy a Redis server.</span></span>

  > [!IMPORTANT] 
  > <span data-ttu-id="4facb-108">Do użytku produkcyjnego montażowa Redis jest zalecane tylko wtedy, gdy działa w tym samym centrum danych jako aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="4facb-108">For production use, a Redis backplane is recommended only when it runs in the same data center as the SignalR app.</span></span> <span data-ttu-id="4facb-109">W przeciwnym razie opóźnienia sieci spadku wydajności.</span><span class="sxs-lookup"><span data-stu-id="4facb-109">Otherwise, network latency degrades performance.</span></span> <span data-ttu-id="4facb-110">Jeśli uruchomiona jest aplikacja SignalR w chmurze platformy Azure, firma Microsoft zaleca Azure SignalR Service zamiast montażowa pamięci podręcznej Redis.</span><span class="sxs-lookup"><span data-stu-id="4facb-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="4facb-111">Możesz użyć usługi Azure Redis Cache Service do tworzenia aplikacji i środowisk testowych.</span><span class="sxs-lookup"><span data-stu-id="4facb-111">You can use the Azure Redis Cache Service for development and test environments.</span></span>

  <span data-ttu-id="4facb-112">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="4facb-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="4facb-113">Dokumentacja usługi redis</span><span class="sxs-lookup"><span data-stu-id="4facb-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="4facb-114">Dokumentacja usługi Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="4facb-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="4facb-115">W aplikacji SignalR, należy zainstalować `Microsoft.AspNetCore.SignalR.Redis` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="4facb-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span> <span data-ttu-id="4facb-116">(Dostępna jest również `Microsoft.AspNetCore.SignalR.StackExchangeRedis` pakietu, ale był jeden dla platformy ASP.NET Core 2.2 lub nowszego.)</span><span class="sxs-lookup"><span data-stu-id="4facb-116">(There is also a `Microsoft.AspNetCore.SignalR.StackExchangeRedis` package, but that one is for ASP.NET Core 2.2 and later.)</span></span>

* <span data-ttu-id="4facb-117">W `Startup.ConfigureServices` metody, wywołanie `AddRedis` po `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="4facb-117">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="4facb-118">Skonfiguruj opcje, zgodnie z potrzebami:</span><span class="sxs-lookup"><span data-stu-id="4facb-118">Configure options as needed:</span></span>
 
  <span data-ttu-id="4facb-119">Większość opcji można ustawić w parametrach połączenia lub w [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) obiektu.</span><span class="sxs-lookup"><span data-stu-id="4facb-119">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="4facb-120">Opcje określone w `ConfigurationOptions` przesłonięte ustawiony w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="4facb-120">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="4facb-121">Poniższy przykład pokazuje, jak ustawić opcje w `ConfigurationOptions` obiektu.</span><span class="sxs-lookup"><span data-stu-id="4facb-121">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="4facb-122">Ten przykład dodaje prefiks kanał, aby wiele aplikacji mogą udostępniać tego samego wystąpienia usługi Redis, zgodnie z opisem w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="4facb-122">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="4facb-123">W poprzednim kodzie `options.Configuration` jest inicjowany za pomocą dowolnie określono w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="4facb-123">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="4facb-124">W aplikacji SignalR należy zainstalować jedną z następujących pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="4facb-124">In the SignalR app, install one of the following NuGet packages:</span></span>

  * <span data-ttu-id="4facb-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` -Zależy od StackExchange.Redis 2.X.X.</span><span class="sxs-lookup"><span data-stu-id="4facb-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` - Depends on StackExchange.Redis 2.X.X.</span></span> <span data-ttu-id="4facb-126">Jest to zalecany pakiet dla platformy ASP.NET Core 2.2 lub nowszego.</span><span class="sxs-lookup"><span data-stu-id="4facb-126">This is the recommended package for ASP.NET Core 2.2 and later.</span></span>
  * <span data-ttu-id="4facb-127">`Microsoft.AspNetCore.SignalR.Redis` -Zależy od StackExchange.Redis 1.X.X.</span><span class="sxs-lookup"><span data-stu-id="4facb-127">`Microsoft.AspNetCore.SignalR.Redis` - Depends on StackExchange.Redis 1.X.X.</span></span> <span data-ttu-id="4facb-128">Ten pakiet nie będzie wysyłać w programie ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="4facb-128">This package will not be shipping in ASP.NET Core 3.0.</span></span>

* <span data-ttu-id="4facb-129">W `Startup.ConfigureServices` metody, wywołanie `AddStackExchangeRedis` po `AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="4facb-129">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="4facb-130">Skonfiguruj opcje, zgodnie z potrzebami:</span><span class="sxs-lookup"><span data-stu-id="4facb-130">Configure options as needed:</span></span>
 
  <span data-ttu-id="4facb-131">Większość opcji można ustawić w parametrach połączenia lub w [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) obiektu.</span><span class="sxs-lookup"><span data-stu-id="4facb-131">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="4facb-132">Opcje określone w `ConfigurationOptions` przesłonięte ustawiony w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="4facb-132">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="4facb-133">Poniższy przykład pokazuje, jak ustawić opcje w `ConfigurationOptions` obiektu.</span><span class="sxs-lookup"><span data-stu-id="4facb-133">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="4facb-134">Ten przykład dodaje prefiks kanał, aby wiele aplikacji mogą udostępniać tego samego wystąpienia usługi Redis, zgodnie z opisem w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="4facb-134">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="4facb-135">W poprzednim kodzie `options.Configuration` jest inicjowany za pomocą dowolnie określono w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="4facb-135">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="4facb-136">Aby uzyskać informacji o opcjach pamięci podręcznej Redis, zobacz [StackExchange Redis dokumentacji](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span><span class="sxs-lookup"><span data-stu-id="4facb-136">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="4facb-137">Jeśli używasz jednego serwera pamięci podręcznej Redis w przypadku wielu aplikacji SignalR, należy używać prefiksu różnych kanałów dla każdej aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="4facb-137">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="4facb-138">Ustawianie prefiksu kanału izoluje jednej aplikacji SignalR od innych osób, które używają różnych kanałów prefiksy.</span><span class="sxs-lookup"><span data-stu-id="4facb-138">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="4facb-139">Jeśli nie można przypisać różnych prefiksów, wiadomość wysłana z jednej aplikacji do wszystkich jej klienci zaczną się do wszystkich klientów wszystkie aplikacje, które używają serwera Redis jako płyty montażowej.</span><span class="sxs-lookup"><span data-stu-id="4facb-139">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="4facb-140">Konfigurowanie usługi serwer farmy równoważenia obciążenia oprogramowania dla trwałych sesji.</span><span class="sxs-lookup"><span data-stu-id="4facb-140">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="4facb-141">Poniżej przedstawiono kilka przykładów, dokumentacji, jak to zrobić:</span><span class="sxs-lookup"><span data-stu-id="4facb-141">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="4facb-142">IIS</span><span class="sxs-lookup"><span data-stu-id="4facb-142">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="4facb-143">HAProxy</span><span class="sxs-lookup"><span data-stu-id="4facb-143">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="4facb-144">Nginx</span><span class="sxs-lookup"><span data-stu-id="4facb-144">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="4facb-145">pfSense</span><span class="sxs-lookup"><span data-stu-id="4facb-145">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="4facb-146">Błędy serwera redis</span><span class="sxs-lookup"><span data-stu-id="4facb-146">Redis server errors</span></span>

<span data-ttu-id="4facb-147">Gdy ulegnie awarii serwera Redis, SignalR zgłasza wyjątki, które wskazują, że komunikaty nie będą dostarczane.</span><span class="sxs-lookup"><span data-stu-id="4facb-147">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="4facb-148">Niektóre komunikaty o typowych wyjątek:</span><span class="sxs-lookup"><span data-stu-id="4facb-148">Some typical exception messages:</span></span>

* <span data-ttu-id="4facb-149">*Zapisywanie nie powiodło się komunikat*</span><span class="sxs-lookup"><span data-stu-id="4facb-149">*Failed writing message*</span></span>
* <span data-ttu-id="4facb-150">*Nie można wywołać metody koncentratora "MethodName"*</span><span class="sxs-lookup"><span data-stu-id="4facb-150">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="4facb-151">*Połączenie Redis nie powiodło się*</span><span class="sxs-lookup"><span data-stu-id="4facb-151">*Connection to Redis failed*</span></span>

<span data-ttu-id="4facb-152">SignalR nie buforuje wiadomości do wysłania ich, gdy serwer wróci do sprawności.</span><span class="sxs-lookup"><span data-stu-id="4facb-152">SignalR doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="4facb-153">Wszystkie komunikaty wysłane w czasie, gdy serwer Redis nie działa, zostaną utracone.</span><span class="sxs-lookup"><span data-stu-id="4facb-153">Any messages sent while the Redis server is down are lost.</span></span>

<span data-ttu-id="4facb-154">Gdy serwer Redis jest dostępny ponownie automatycznie ponownie połączy SignalR.</span><span class="sxs-lookup"><span data-stu-id="4facb-154">SignalR automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="4facb-155">Niestandardowe zachowanie błędów połączenia</span><span class="sxs-lookup"><span data-stu-id="4facb-155">Custom behavior for connection failures</span></span>

<span data-ttu-id="4facb-156">Oto przykład pokazujący sposób obsługi zdarzenia błędów połączenia Redis.</span><span class="sxs-lookup"><span data-stu-id="4facb-156">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="clustering"></a><span data-ttu-id="4facb-157">Klastrowanie</span><span class="sxs-lookup"><span data-stu-id="4facb-157">Clustering</span></span>

<span data-ttu-id="4facb-158">Klastrowanie jest metoda dla osiągnięcia wysokiej dostępności przy użyciu wielu serwerów Redis.</span><span class="sxs-lookup"><span data-stu-id="4facb-158">Clustering is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="4facb-159">Klaster nie jest oficjalnie obsługiwany, ale może ona działać.</span><span class="sxs-lookup"><span data-stu-id="4facb-159">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4facb-160">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4facb-160">Next steps</span></span>

<span data-ttu-id="4facb-161">Aby uzyskać więcej informacji, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="4facb-161">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="4facb-162">Dokumentacja usługi redis</span><span class="sxs-lookup"><span data-stu-id="4facb-162">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="4facb-163">Dokumentacja StackExchange Redis</span><span class="sxs-lookup"><span data-stu-id="4facb-163">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="4facb-164">Dokumentacja usługi Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="4facb-164">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)
