---
title: Zadania w tle z usług hostowanych w ASP.NET Core
author: guardrex
description: Informacje o sposobie wykonania zadania w tle z usług hostowanych w ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosted-services
ms.openlocfilehash: 89e595fb6ef38745d7377fdaaf1780c64320efe3
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/20/2018
ms.locfileid: "29374694"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Zadania w tle z usług hostowanych w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

W ASP.NET Core, można stosować jako zadania w tle *usług hostowanych*. Hostowana usługa jest klasa z logiką zadania tła, który implementuje [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu. Ten temat zawiera trzy przykłady usługi hostowanej:

* Zadania w tle uruchamiana przez czasomierz.
* Usługa hostowana aktywuje zakresami usługi. Usługa zakresie może wykorzystać iniekcji zależności.
* Tło w kolejce zadań, które są wykonywane sekwencyjnie.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="ihostedservice-interface"></a>Interfejs IHostedService

Wdrożenie usług hostowanych [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu. Interfejs definiuje dwie metody dla obiektów, które są zarządzane przez hosta:

* [StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) — nazywany po uruchomieniu serwera i [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) zostanie wywołany. `StartAsync` zawiera logikę można uruchomić zadania w tle.

* [StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) -wyzwalane, gdy host wykonuje łagodne zamykanie. `StopAsync` zawiera logikę do zakończenia zadania w tle i usuń wszelkie niezarządzane zasoby. Jeśli aplikacja zostanie wyłączony nieoczekiwanie (na przykład aplikacji proces zakończy się niepowodzeniem), `StopAsync` nie może mieć nazwę.

Hostowana usługa jest pojedyncza po aktywowaniu uruchamiania aplikacji i bezpiecznie zamknięcia przy zamykaniu aplikacji. Gdy [IDisposable](/dotnet/api/system.idisposable) jest zaimplementowana, zasobów można usunięta po usunięciu kontenera usług. Jeśli podczas wykonywania zadania w tle, zostanie zgłoszony błąd `Dispose` powinna być wywoływana nawet wtedy, gdy `StopAsync` nie jest wywoływana.

## <a name="timed-background-tasks"></a>Zadania w tle czasu

Zadanie w tle czasu sprawia, że użycie [System.Threading.Timer](/dotnet/api/system.threading.timer) klasy. Czasomierza wyzwala zadanie `DoWork` metody. Czasomierz jest wyłączona na `StopAsync` i usunięta po usunięciu kontenera usług na `Dispose`:

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Usługa jest zarejestrowana w `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Korzystanie z zakresami usługę zadania w tle

Do użycia w zakresie usług `IHostedService`, utworzyć zakres. Zakres nie jest tworzone przez domyślny dla usługi hostowanej.

Usługa zadań tła zakresie zawiera logikę zadania w tle. W poniższym przykładzie [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) jest dodane do usługi:

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

Usługa hostowana tworzy zakres na rozpoznawanie usługi zadania tła o zakresie wywołać jej `DoWork` metody:

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Usługi są zarejestrowane w `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Zadania w tle w kolejce

Kolejki zadań tła jest oparta na platformie .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([wstępnie zaplanowane być wbudowane dla platformy ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

W `QueueHostedService`, zadania w tle (`workItem`) w kolejce usuniętej i wykonać:

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

Usługi są zarejestrowane w `Startup.ConfigureServices`:

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

W klasie indeksu strony modelu `IBackgroundTaskQueue` do konstruktora i ma przypisaną do `Queue`:

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

Gdy **Dodaj zadanie** przycisk zostanie wybrany na stronie indeksu `OnPostAddTask` metoda jest wykonywana. `QueueBackgroundWorkItem` jest wywoływana można umieścić w kolejce elementu roboczego:

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wykonania zadania w tle w mikrousług IHostedService i klasa BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [System.Threading.Timer](/dotnet/api/system.threading.timer)
