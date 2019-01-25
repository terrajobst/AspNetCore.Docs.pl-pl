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
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a>Konfigurowanie systemu backplane Redis dla biblioteki SignalR platformy ASP.NET Core skalowalnego w poziomie

Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse), [Brady'ego Gastera](https://twitter.com/bradygaster), i [Tom Dykstra](https://github.com/tdykstra),

W tym artykule wyjaśniono aspekty specyficzne dla SignalR Konfigurowanie [Redis](https://redis.io/) serwer na potrzeby skalowania aplikacji biblioteki SignalR platformy ASP.NET Core.

## <a name="set-up-a-redis-backplane"></a>Konfigurowanie montażowa pamięci podręcznej Redis

* Wdrażanie serwera Redis.

  > [!IMPORTANT] 
  > Do użytku produkcyjnego montażowa Redis jest zalecane tylko wtedy, gdy działa w tym samym centrum danych jako aplikacji SignalR. W przeciwnym razie opóźnienia sieci spadku wydajności. Jeśli uruchomiona jest aplikacja SignalR w chmurze platformy Azure, firma Microsoft zaleca Azure SignalR Service zamiast montażowa pamięci podręcznej Redis. Możesz użyć usługi Azure Redis Cache Service do tworzenia aplikacji i środowisk testowych.

  Aby uzyskać więcej informacji, zobacz następujące zasoby:

  * <xref:signalr/scale>
  * [Dokumentacja usługi redis](https://redis.io/)
  * [Dokumentacja usługi Azure Redis Cache](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* W aplikacji SignalR, należy zainstalować `Microsoft.AspNetCore.SignalR.Redis` pakietu NuGet. (Dostępna jest również `Microsoft.AspNetCore.SignalR.StackExchangeRedis` pakietu, ale był jeden dla platformy ASP.NET Core 2.2 lub nowszego.)

* W `Startup.ConfigureServices` metody, wywołanie `AddRedis` po `AddSignalR`:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* Skonfiguruj opcje, zgodnie z potrzebami:
 
  Większość opcji można ustawić w parametrach połączenia lub w [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) obiektu. Opcje określone w `ConfigurationOptions` przesłonięte ustawiony w parametrach połączenia.

  Poniższy przykład pokazuje, jak ustawić opcje w `ConfigurationOptions` obiektu. Ten przykład dodaje prefiks kanał, aby wiele aplikacji mogą udostępniać tego samego wystąpienia usługi Redis, zgodnie z opisem w następnym kroku.

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  W poprzednim kodzie `options.Configuration` jest inicjowany za pomocą dowolnie określono w parametrach połączenia.

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* W aplikacji SignalR należy zainstalować jedną z następujących pakietów NuGet:

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis` -Zależy od StackExchange.Redis 2.X.X. Jest to zalecany pakiet dla platformy ASP.NET Core 2.2 lub nowszego.
  * `Microsoft.AspNetCore.SignalR.Redis` -Zależy od StackExchange.Redis 1.X.X. Ten pakiet nie będzie wysyłać w programie ASP.NET Core 3.0.

* W `Startup.ConfigureServices` metody, wywołanie `AddStackExchangeRedis` po `AddSignalR`:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* Skonfiguruj opcje, zgodnie z potrzebami:
 
  Większość opcji można ustawić w parametrach połączenia lub w [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) obiektu. Opcje określone w `ConfigurationOptions` przesłonięte ustawiony w parametrach połączenia.

  Poniższy przykład pokazuje, jak ustawić opcje w `ConfigurationOptions` obiektu. Ten przykład dodaje prefiks kanał, aby wiele aplikacji mogą udostępniać tego samego wystąpienia usługi Redis, zgodnie z opisem w następnym kroku.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  W poprzednim kodzie `options.Configuration` jest inicjowany za pomocą dowolnie określono w parametrach połączenia.

  Aby uzyskać informacji o opcjach pamięci podręcznej Redis, zobacz [StackExchange Redis dokumentacji](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

* Jeśli używasz jednego serwera pamięci podręcznej Redis w przypadku wielu aplikacji SignalR, należy używać prefiksu różnych kanałów dla każdej aplikacji SignalR.

  Ustawianie prefiksu kanału izoluje jednej aplikacji SignalR od innych osób, które używają różnych kanałów prefiksy. Jeśli nie można przypisać różnych prefiksów, wiadomość wysłana z jednej aplikacji do wszystkich jej klienci zaczną się do wszystkich klientów wszystkie aplikacje, które używają serwera Redis jako płyty montażowej.

* Konfigurowanie usługi serwer farmy równoważenia obciążenia oprogramowania dla trwałych sesji. Poniżej przedstawiono kilka przykładów, dokumentacji, jak to zrobić:

  * [IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Błędy serwera redis

Gdy ulegnie awarii serwera Redis, SignalR zgłasza wyjątki, które wskazują, że komunikaty nie będą dostarczane. Niektóre komunikaty o typowych wyjątek:

* *Zapisywanie nie powiodło się komunikat*
* *Nie można wywołać metody koncentratora "MethodName"*
* *Połączenie Redis nie powiodło się*

SignalR nie buforuje wiadomości do wysłania ich, gdy serwer wróci do sprawności. Wszystkie komunikaty wysłane w czasie, gdy serwer Redis nie działa, zostaną utracone.

Gdy serwer Redis jest dostępny ponownie automatycznie ponownie połączy SignalR.

### <a name="custom-behavior-for-connection-failures"></a>Niestandardowe zachowanie błędów połączenia

Oto przykład pokazujący sposób obsługi zdarzenia błędów połączenia Redis.

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

## <a name="clustering"></a>Klastrowanie

Klastrowanie jest metoda dla osiągnięcia wysokiej dostępności przy użyciu wielu serwerów Redis. Klaster nie jest oficjalnie obsługiwany, ale może ona działać.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* <xref:signalr/scale>
* [Dokumentacja usługi redis](https://redis.io/documentation)
* [Dokumentacja StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/)
* [Dokumentacja usługi Azure Redis Cache](https://docs.microsoft.com/en-us/azure/redis-cache/)
