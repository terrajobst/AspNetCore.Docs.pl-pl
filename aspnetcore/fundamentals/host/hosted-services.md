---
title: Zadania w tle z usługami hostowanymi w ASP.NET Core
author: guardrex
description: Dowiedz się, jak wdrożyć zadania w tle z usługami hostowanymi na platformie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 3d4279a291182da60c0cb2fbb93a3922ed673cde
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914023"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>Zadania w tle z usługami hostowanymi w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

W ASP.NET Core zadania w tle można zaimplementować jako *usługi hostowane*. Usługa hostowana jest klasą z logiką zadań w tle, <xref:Microsoft.Extensions.Hosting.IHostedService> która implementuje interfejs. Ten temat zawiera trzy przykłady usługi hostowanej:

* Zadanie w tle, które jest uruchamiane na czasomierzu.
* Usługa hostowana, która aktywuje [usługę](xref:fundamentals/dependency-injection#service-lifetimes)o określonym zakresie. Usługa objęta zakresem może używać iniekcji zależności.
* Umieszczone w kolejce zadania w tle, które są uruchamiane sekwencyjnie.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykładowa aplikacja jest dostępna w dwóch wersjach:

* Host internetowy &ndash; hosta sieci Web jest przydatny do hostowania aplikacji sieci Web. Przykładowy kod przedstawiony w tym temacie pochodzi z wersji hosta sieci Web przykładowej. Aby uzyskać więcej informacji, zobacz temat [hosta sieci Web](xref:fundamentals/host/web-host) .
* Host ogólny &ndash; hosta ogólnego jest nowy w ASP.NET Core 2,1. Aby uzyskać więcej informacji, zobacz temat [hosta ogólnego](xref:fundamentals/host/generic-host) .

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a>Szablon usługi procesu roboczego

Szablon usługi ASP.NET Core Worker zapewnia punkt początkowy do pisania długotrwałych aplikacji usługi. Aby użyć szablonu jako podstawy dla aplikacji usług hostowanych:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Utwórz nowy projekt.
1. Wybierz **aplikacji sieci Web platformy ASP.NET Core**. Wybierz opcję **Dalej**.
1. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. Wybierz pozycję **Utwórz**.
1. W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .
1. Wybierz szablon **usługi procesu roboczego** . Wybierz pozycję **Utwórz**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Użyj szablonu usługi procesu roboczego`worker`() z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) z powłoki poleceń. W poniższym przykładzie jest tworzona aplikacja usługi Worker o nazwie `ContosoWorkerService`. Folder dla `ContosoWorkerService` aplikacji jest tworzony automatycznie podczas wykonywania polecenia.

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="package"></a>Package

Odwołuje się do pakietu [Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) lub Dodaj odwołanie do pakietu do pakietu [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .

## <a name="ihostedservice-interface"></a>IHostedService, interfejs

Usługi hostowane implementują <xref:Microsoft.Extensions.Hosting.IHostedService> interfejs. Interfejs definiuje dwie metody dla obiektów zarządzanych przez hosta:

* [StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; zawiera logikędouruchomienia`StartAsync` zadania w tle. W przypadku korzystania z `StartAsync` [hosta sieci Web](xref:fundamentals/host/web-host)jest wywoływana po uruchomieniu serwera i wyzwoleniu [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) . W przypadku korzystania z `StartAsync` [hosta generycznego](xref:fundamentals/host/generic-host)jest wywoływana `ApplicationStarted` przed wywołaniem.

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Wyzwalane, gdy host wykonuje bezpieczne zamknięcie. `StopAsync`zawiera logikę końcową zadania w tle. Implementacja <xref:System.IDisposable> i [finalizatory (destruktory)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) do usuwania wszelkich niezarządzanych zasobów.

  Token anulowania ma domyślnie pięciu sekund, aby wskazać, że proces zamykania nie powinien już być łagodny. Gdy żądanie anulowania jest wymagane na tokenie:

  * Wszystkie pozostałe operacje w tle, które wykonuje aplikacja, powinny zostać przerwane.
  * Wszelkie metody wywoływane w `StopAsync` programie powinny zwracać monity.

  Jednak zadania nie są porzucane po zażądaniu&mdash;anulowania obiekt wywołujący czeka na ukończenie wszystkich zadań.

  Jeśli aplikacja nieoczekiwanie się zamknie (na przykład proces aplikacji kończy się niepowodzeniem) `StopAsync` , może nie zostać wywołany. W związku z tym wszystkie metody wywoływane lub operacje `StopAsync` wykonywane w programie mogą nie wystąpić.

  Aby zwiększyć domyślne pięć sekund limitu czasu zamykania, ustaw:

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>w przypadku korzystania z hosta ogólnego. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Ustawienie konfiguracji hosta limitu czasu zamykania w przypadku korzystania z hosta sieci Web. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#shutdown-timeout>.

Usługa hostowana jest uaktywniana raz podczas uruchamiania aplikacji i bezpiecznie zamykana podczas zamykania aplikacji. Jeśli wystąpi błąd podczas wykonywania zadania w tle, powinien `Dispose` zostać wywołany, nawet `StopAsync` Jeśli nie jest wywoływana.

## <a name="timed-background-tasks"></a>Zadania w tle czasu

Zadanie w tle czasu używa klasy [System. Threading. Timer](xref:System.Threading.Timer) . Czasomierz wyzwala `DoWork` metodę zadania. Czasomierz jest wyłączony `StopAsync` i zlikwidowany po usunięciu kontenera `Dispose`usługi:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Usługa jest zarejestrowana w `Startup.ConfigureServices` `AddHostedService` ramach metody rozszerzającej:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Zużywanie usługi w zakresie w zadaniu w tle

Aby korzystać z [usług](xref:fundamentals/dependency-injection#service-lifetimes) o określonym zakresie `IHostedService`w ramach programu, należy utworzyć zakres. Żaden zakres nie jest tworzony domyślnie dla usługi hostowanej.

Usługa zadań w tle w zakresie zawiera logikę zadania w tle. W poniższym przykładzie <xref:Microsoft.Extensions.Logging.ILogger> jest wstrzykiwana do usługi:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

Usługa hostowana tworzy zakres, aby rozwiązać usługę zadań w tle w zakresie w celu wywołania `DoWork` jej metody:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Usługi są zarejestrowane w `Startup.ConfigureServices`usłudze. Implementacja jest zarejestrowana `AddHostedService` przy użyciu metody rozszerzającej: `IHostedService`

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Zadania w kolejce w dół

Kolejka zadań w tle jest oparta na platformie .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([wstępnie zaplanowana do wbudowania dla ASP.NET Core 3,0](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

W `QueueHostedService`programie zadania w tle w kolejce są odrzucane i wykonywane <xref:Microsoft.Extensions.Hosting.BackgroundService>jako, która jest klasą bazową do implementowania długotrwałego `IHostedService`działania:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Usługi są zarejestrowane w `Startup.ConfigureServices`usłudze. Implementacja jest zarejestrowana `AddHostedService` przy użyciu metody rozszerzającej: `IHostedService`

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

W klasie modelu strony indeksu:

* Zostaje wstrzyknięty do konstruktora i przypisany do `Queue`. `IBackgroundTaskQueue`
* Jest wstrzykiwany i przypisany do `_serviceScopeFactory`. <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> Fabryka służy do tworzenia wystąpień <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, które są używane do tworzenia usług w zakresie. Zakres jest tworzony w celu używania aplikacji `AppDbContext` (usługi w [zakresie](xref:fundamentals/dependency-injection#service-lifetimes)) do zapisywania rekordów `IBackgroundTaskQueue` bazy danych w (pojedynczej usłudze).

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

Po wybraniu przycisku **Dodaj zadanie** na stronie `OnPostAddTask` indeks jest wykonywana metoda. `QueueBackgroundWorkItem`jest wywoływana w celu dodawania do kolejki elementu pracy:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Implementowanie zadań w tle w mikrousługach za pomocą IHostedService i klasy BackgroundService](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
