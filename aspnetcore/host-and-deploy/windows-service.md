---
title: Host platformy ASP.NET Core w usłudze systemu Windows
author: guardrex
description: Dowiedz się, jak udostępniać aplikacji platformy ASP.NET Core w usłudze systemu Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 0149039f69539b7c69d7ba45efcf09d80ffcba79
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275101"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Host platformy ASP.NET Core w usłudze systemu Windows

Przez [Luke Latham](https://github.com/guardrex) i [Dykstra niestandardowy](https://github.com/tdykstra)

Aplikacja platformy ASP.NET Core może być obsługiwany w systemie Windows bez użycia usługi IIS jako [usługi systemu Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Gdy hostowany jako usługa systemu Windows, aplikacja może automatycznie uruchamiania po ponownym uruchomieniu i awarii bez udziału człowieka.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started"></a>Wprowadzenie

Następujące minimalne zmiany są wymagane do skonfigurowania istniejącego projektu platformy ASP.NET Core do uruchamiania w usłudze:

1. W pliku projektu:

   1. Potwierdzić obecność identyfikator środowiska uruchomieniowego lub dodanie go do  **\<PropertyGroup >** zawierający platformy docelowej:
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. Dodaj odwołanie do pakietu [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

1. Wprowadź następujące zmiany w `Program.Main`:

   * Wywołanie [hosta. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) zamiast `host.Run`.

   * Jeśli kod wywołuje `UseContentRoot`, użyj ścieżki do aplikacji opublikowane lokalizacji, a nie `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Publikowanie aplikacji w folderze. Użyj [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish) lub [profilu publikowania programu Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) który publikuje do folderu.

   Aby opublikować przykładową aplikację z poziomu wiersza polecenia, uruchom następujące polecenie w oknie konsoli z folderu projektu:

   ```console
   dotnet publish --configuration Release --output c:\svc
   ```

1. Użyj [sc.exe](https://technet.microsoft.com/library/bb490995) narzędzia wiersza polecenia, aby utworzyć usługę (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`). `binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, która zawiera nazwę pliku wykonywalnego. **Odstęp między znak równości i znaku cudzysłowu, która rozpoczyna się ścieżka jest wymagana.**

   Przykładowa aplikacja i polecenia, który jest zgodny usługa jest:

   * O nazwie **Moja_usługa**.
   * Opublikowany *c:\\svc* folderu.
   * Aplikację pliku wykonywalnego o nazwie *AspNetCoreService.exe*.

   Otwórz powłokę wiersza polecenia z uprawnieniami administracyjnymi i uruchom następujące polecenie:

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   ```

   **Upewnij się, że ma miejsce, między `binPath=` argumentu i jego wartość.**

1. Uruchom usługę z `sc start <SERVICE_NAME>` polecenia.

   Aby uruchomić usługę aplikacji przykładowej, użyj następującego polecenia:

   ```console
   sc start MyService
   ```

   Polecenie zajmuje kilka sekund, aby uruchomić usługę.

1. `sc query <SERVICE_NAME>` Polecenia może służyć do sprawdzania stanu usługi można określić jego stanu:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Aby sprawdzić stan usługi aplikacji przykładowej, użyj następującego polecenia:

   ```console
   sc query MyService
   ```

1. Gdy usługa jest w `RUNNING` stanu i usługi w przypadku aplikacji sieci web, przejdź aplikację w jego ścieżki (domyślnie `http://localhost:5000`, która przekierowuje do `https://localhost:5001` przy użyciu [HTTPS przekierowanie w oprogramowaniu pośredniczącym](xref:security/enforcing-ssl)).

   Usługi aplikacji przykładowej Przeglądaj aplikację w `http://localhost:5000`.

1. Zatrzymaj usługę z `sc stop <SERVICE_NAME>` polecenia.

   Polecenie zatrzymuje usługę aplikacji przykładowej:

   ```console
   sc stop MyService
   ```

1. Z niewielkim opóźnieniem zatrzymania usługi, odinstaluj usługę z `sc delete <SERVICE_NAME>` polecenia.

   Sprawdź stan usługi aplikacji przykładowej:

   ```console
   sc query MyService
   ```

   Gdy przykładowej aplikacji usługi jest w `STOPPED` stanu, użyj następującego polecenia, aby odinstalować usługę aplikacji przykładowej:

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Umożliwiają uruchamianie poza usługą

Możliwe jest łatwiejsze testowanie i debugowanie podczas uruchamiania poza usługą, dzięki czemu zwyczajowe można dodać kod, który wywołuje `RunAsService` tylko w niektórych warunkach. Na przykład aplikacja może działać jako aplikację konsoli z `--console` argumentu wiersza polecenia lub jeśli jest dołączony debuger:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

Ponieważ konfiguracja platformy ASP.NET Core wymaga pary nazwa wartość dla argumentów wiersza polecenia, `--console` przełącznik jest usunięty przed argumenty są przekazywane do [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Obsługa zatrzymywania i uruchamiania zdarzeń

Do obsługi [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), i [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) zdarzenia, wprowadź następujące zmiany dodatkowe:

1. Utwórz klasę pochodną [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Create — metoda rozszerzenia dla [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) niestandardowego, który przekazuje `WebHostService` do [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. W `Program.Main`, wywołaj nowej metody rozszerzenia `RunAsCustomService`, zamiast [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Jeśli niestandardowa `WebHostService` kod wymaga usługi z iniekcji zależności (np. Rejestrator), Uzyskaj ją z [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) właściwości:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Usługi, które interakcję z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub usługę równoważenia obciążenia mogą wymagać dodatkowej konfiguracji. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

## <a name="kestrel-endpoint-configuration"></a>Konfiguracja punktu końcowego kestrel

Aby uzyskać informacje dotyczące konfiguracji punktu końcowego Kestrel, w tym konfiguracja protokołu HTTPS i SNI pomocy technicznej, zobacz [konfiguracji punktu końcowego Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).
