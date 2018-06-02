---
title: Host platformy ASP.NET Core w usłudze systemu Windows
author: rick-anderson
description: Dowiedz się, jak udostępniać aplikacji platformy ASP.NET Core w usłudze systemu Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 69b04d2ae723b67e16bf581127a13ceab268697d
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2018
ms.locfileid: "34473028"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Host platformy ASP.NET Core w usłudze systemu Windows

Przez [Dykstra niestandardowy](https://github.com/tdykstra)

Zalecanym sposobem hostowania aplikacji platformy ASP.NET Core w systemie Windows bez za pomocą usług IIS jest uruchomienie go w [usługi systemu Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Gdy hostowany jako usługa systemu Windows, aplikacja może automatycznie uruchamiania po ponownym uruchomieniu i awarii bez udziału człowieka.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)). Instrukcje na temat uruchamiania przykładowej aplikacji, można znaleźć w próbce *README.md* pliku.

## <a name="prerequisites"></a>Wymagania wstępne

* Aplikacja muszą działać na środowiska uruchomieniowego .NET Framework. W *.csproj* pliku, podaj odpowiednie wartości dla [TargetFramework](/nuget/schema/target-frameworks) i [RuntimeIdentifier](/dotnet/articles/core/rid-catalog). Oto przykład:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Podczas tworzenia projektu w programie Visual Studio, użyj **aplikacji platformy ASP.NET Core (.NET Framework)** szablonu.

* Jeśli aplikacja odbiera żądania z Internetu (nie tylko z sieci wewnętrznej), należy użyć [HTTP.sys](xref:fundamentals/servers/httpsys) serwera sieci web (wcześniej znane jako [WebListener](xref:fundamentals/servers/weblistener) dla platformy ASP.NET Core 1.x aplikacji) zamiast [Kestrel](xref:fundamentals/servers/kestrel). Korzystanie z usług IIS w konfiguracji serwera zwrotny serwer proxy z Kestrel jest opcją w przypadku wdrożeń krawędzi. Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="get-started"></a>Wprowadzenie

W tej sekcji opisano minimalną zmiany wymagane do skonfigurowania istniejącego projektu platformy ASP.NET Core do uruchamiania w usłudze.

1. Zainstaluj pakiet NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Wprowadź następujące zmiany w `Program.Main`:

   * Wywołanie `host.RunAsService` zamiast `host.Run`.

   * Jeśli kod wywołuje `UseContentRoot`, użyj ścieżki do lokalizacji publikacji zamiast `Directory.GetCurrentDirectory()`.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. Publikowanie aplikacji w folderze. Użyj [publikowania dotnet](/dotnet/articles/core/tools/dotnet-publish) lub [profilu publikowania programu Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) który publikuje do folderu.

4. Przetestuj tworzenia i uruchamiania usługi.

   Otwórz powłokę wiersza polecenia z uprawnieniami administracyjnymi do użycia [sc.exe](https://technet.microsoft.com/library/bb490995) wiersza polecenia narzędzia do tworzenia i uruchamiania usługi. Jeśli usługa ma nazwę Moja_usługa, są publikowane w `c:\svc`, a polecenia o nazwie AspNetCoreService, są:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   `binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, która zawiera nazwę pliku wykonywalnego.

   ![Okno konsoli tworzenie i uruchamianie przykładu](windows-service/_static/create-start.png)

   Po zakończeniu tych poleceń, przejdź do tej samej ścieżki, w przypadku uruchamiania jako aplikacji konsoli (domyślnie `http://localhost:5000`):

   ![Uruchomionych w usłudze](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Umożliwiają uruchamianie poza usługą

Możliwe jest łatwiejsze testowanie i debugowanie podczas uruchamiania poza usługą, dzięki czemu zwyczajowe można dodać kod, który wywołuje `RunAsService` tylko w niektórych warunkach. Na przykład aplikacja może działać jako aplikację konsoli z `--console` argumentu wiersza polecenia lub jeśli jest dołączony debuger:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>Obsługa zatrzymywania i uruchamiania zdarzeń

Do obsługi `OnStarting`, `OnStarted`, i `OnStopping` zdarzenia, wprowadź następujące zmiany dodatkowe:

1. Utwórz klasę pochodną `WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Create — metoda rozszerzenia dla `IWebHost` niestandardowego, który przekazuje `WebHostService` do `ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. W `Program.Main`, wywołaj nowej metody rozszerzenia `RunAsCustomService`, zamiast `RunAsService`:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

Jeśli niestandardowa `WebHostService` kod wymaga usługi z iniekcji zależności (np. Rejestrator), Uzyskaj ją z `Services` właściwości `IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Serwer proxy i scenariuszy usługi równoważenia obciążenia

Usługi, które interakcję z żądaniami z Internetu lub sieci firmowej i znajdują się za serwerem proxy lub usługę równoważenia obciążenia mogą wymagać dodatkowej konfiguracji. Aby uzyskać więcej informacji, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Potwierdzenia

W tym artykule został napisany za pomocą opublikowanych źródeł:

* [Hosting platformy ASP.NET Core jako usługa systemu Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Jak obsługiwać podstawowych ASP.NET w usłudze systemu Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
