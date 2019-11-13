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
# <a name="set-up-a-redis-backplane-for-aspnet-core-opno-locsignalr-scale-out"></a>Konfigurowanie planu Redis dla ASP.NET Core SignalR skalowanie w poziomie

Autorzy [Andrew Stanton-pielęgniarki](https://twitter.com/anurse), [Brady Gastera](https://twitter.com/bradygaster)i [Tomasz Dykstra](https://github.com/tdykstra),

W tym artykule wyjaśniono SignalRaspekty dotyczące konfigurowania serwera [Redis](https://redis.io/) do użycia w celu skalowania aplikacji ASP.NET Core SignalR.

## <a name="set-up-a-redis-backplane"></a>Konfigurowanie planu Redis

* Wdróż serwer Redis.

  > [!IMPORTANT] 
  > W przypadku użycia w środowisku produkcyjnym Redis planuje się tylko wtedy, gdy działa on w tym samym centrum danych co aplikacja SignalR. W przeciwnym razie opóźnienie sieci obniży wydajność. Jeśli Twoja aplikacja SignalR jest uruchomiona w chmurze platformy Azure, zalecamy użycie usługi Azure SignalR zamiast planu Redis. Usługi Azure Redis Cache można używać w środowiskach deweloperskich i testowych.

  Aby uzyskać więcej informacji, zobacz następujące zasoby:

  * <xref:signalr/scale>
  * [Dokumentacja Redis](https://redis.io/)
  * [Dokumentacja Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* W aplikacji SignalR Zainstaluj pakiet NuGet `Microsoft.AspNetCore.SignalR.Redis`. (Istnieje również pakiet `Microsoft.AspNetCore.SignalR.StackExchangeRedis`, ale jest on przeznaczony dla ASP.NET Core 2,2 i nowszych).

* W metodzie `Startup.ConfigureServices` Wywołaj `AddRedis` po `AddSignalR`:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* Skonfiguruj opcje zgodnie z wymaganiami:
 
  Większość opcji można ustawić w parametrach połączenia lub w obiekcie [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) . Opcje określone w `ConfigurationOptions` przesłaniają te ustawienia w parametrach połączenia.

  Poniższy przykład pokazuje, jak ustawić opcje w obiekcie `ConfigurationOptions`. Ten przykład dodaje prefiks kanału, dzięki czemu wiele aplikacji może współużytkować to samo wystąpienie Redis, co zostało wyjaśnione w poniższym kroku.

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  W powyższym kodzie, `options.Configuration` jest inicjowany z dowolnego rodzaju określono w parametrach połączenia.

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* W aplikacji SignalR Zainstaluj jeden z następujących pakietów NuGet:

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis` — zależy od StackExchange. Redis 2. X.X. Jest to zalecany pakiet dla ASP.NET Core 2,2 i nowszych.
  * `Microsoft.AspNetCore.SignalR.Redis` — zależy od StackExchange. Redis 1. X.X. Ten pakiet nie będzie wysyłany w ASP.NET Core 3,0.

* W metodzie `Startup.ConfigureServices` Wywołaj `AddStackExchangeRedis` po `AddSignalR`:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* Skonfiguruj opcje zgodnie z wymaganiami:
 
  Większość opcji można ustawić w parametrach połączenia lub w obiekcie [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) . Opcje określone w `ConfigurationOptions` przesłaniają te ustawienia w parametrach połączenia.

  Poniższy przykład pokazuje, jak ustawić opcje w obiekcie `ConfigurationOptions`. Ten przykład dodaje prefiks kanału, dzięki czemu wiele aplikacji może współużytkować to samo wystąpienie Redis, co zostało wyjaśnione w poniższym kroku.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  W powyższym kodzie, `options.Configuration` jest inicjowany z dowolnego rodzaju określono w parametrach połączenia.

  Informacje o opcjach Redis można znaleźć w [dokumentacji stackexchange Redis](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

* Jeśli używasz jednego serwera Redis dla wielu aplikacji SignalR, użyj innego prefiksu kanału dla każdej aplikacji SignalR.

  Ustawienie prefiksu kanału izoluje jedną aplikację SignalR od innych, które używają różnych prefiksów kanałów. Jeśli nie przypiszesz różnych prefiksów, komunikat wysłany z jednej aplikacji do wszystkich klientów będzie przeszedł do wszystkich klientów wszystkich aplikacji korzystających z serwera Redis jako planu wieloplanowego.

* Skonfiguruj oprogramowanie do równoważenia obciążenia farmy serwerów dla sesji programu Sticky Notes. Oto kilka przykładów dokumentacji dotyczącej tego, jak to zrobić:

  * [SERWERZE](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Błędy serwera Redis

Gdy serwer Redis ulegnie awarii, SignalR zgłasza wyjątki wskazujące, że komunikaty nie zostaną dostarczone. Niektóre typowe komunikaty o wyjątkach:

* *Nie można zapisać komunikatu*
* *Nie można wywołać metody centrum "MethodName"*
* *Nie można nawiązać połączenia z Redis*

SignalR nie buforuje komunikatów, aby wysłać je, gdy serwer powróci do tworzenia kopii zapasowej. Wszystkie komunikaty wysyłane w trakcie działania serwera Redis są tracone.

SignalR automatycznie ponownie nawiązuje połączenie po ponownym udostępnieniu serwera Redis.

### <a name="custom-behavior-for-connection-failures"></a>Niestandardowe zachowanie dla błędów połączeń

Oto przykład, który pokazuje, jak obsługiwać zdarzenia niepowodzeń połączeń Redis.

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

## <a name="redis-clustering"></a>Redis klastrowanie

[Redis klastrowanie](https://redis.io/topics/cluster-spec) to metoda osiągania wysokiej dostępności przy użyciu wielu serwerów Redis. Klastrowanie nie jest oficjalnie obsługiwane, ale może zadziałało.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* <xref:signalr/scale>
* [Dokumentacja Redis](https://redis.io/documentation)
* [Dokumentacja StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/)
* [Dokumentacja Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/)
