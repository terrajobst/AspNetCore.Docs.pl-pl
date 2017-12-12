---
title: "Host usługi systemu Windows"
author: tdykstra
description: "Dowiedz się, jak hostowanie aplikacji platformy ASP.NET Core w usłudze systemu Windows."
keywords: "Hosting platformy ASP.NET Core usługi systemu Windows"
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: a6d1acf5ab8f40b0b4d487a6f34cd83d13907852
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Host aplikacji platformy ASP.NET Core w usłudze systemu Windows

przez [Dykstra niestandardowy](https://github.com/tdykstra)

Zalecanym sposobem na potrzeby hostowania aplikacji platformy ASP.NET Core w systemie Windows, jeśli nie używasz usług IIS jest uruchamiany w [usługi systemu Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications). W ten sposób go może automatycznie uruchomić po ponownych rozruchów i awarii (Crash), bez oczekiwania na użytkownika do logowania.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)). Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać instrukcje dotyczące sposobu jego uruchamiania.

## <a name="prerequisites"></a>Wymagania wstępne

* Aplikacja muszą działać na środowiska uruchomieniowego .NET Framework.  W *.csproj* pliku, podaj odpowiednie wartości dla [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) i [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog). Oto przykład:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Podczas tworzenia projektu w programie Visual Studio, użyj **aplikacji platformy ASP.NET Core (.NET Framework)** szablonu.

* Jeśli aplikacja otrzyma żądania z Internetu (nie tylko z sieci wewnętrznej), należy użyć [WebListener](xref:fundamentals/servers/weblistener) serwera sieci web zamiast [Kestrel](xref:fundamentals/servers/kestrel).  Kestrel musi być używany z programem IIS we wdrożeniach krawędzi.  Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="getting-started"></a>Wprowadzenie

W tej sekcji opisano minimalną zmiany wymagane do skonfigurowania istniejącego projektu platformy ASP.NET Core do uruchamiania w usłudze.

* Zainstaluj pakiet NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

* Wprowadź następujące zmiany w `Program.Main`:
  
  * Wywołanie `host.RunAsService` zamiast `host.Run`.
  
  * Jeśli kod wywołuje `UseContentRoot`, użyj ścieżki do lokalizacji publikacji zamiast`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* Opublikuj aplikację w folderze.

  Użyj [publikowania dotnet](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) lub [profilu publikowania programu Visual Studio](xref:publishing/web-publishing-vs) który publikuje do folderu.

* Przetestuj tworzenia i uruchamiania usługi.

  Otwórz okno wiersza polecenia administratora, aby użyć [sc.exe](https://technet.microsoft.com/library/bb490995) wiersza polecenia narzędzia do tworzenia i uruchamiania usługi.  
  
  Jeśli nazwa usługi Moja_usługa, Opublikuj aplikację, aby `c:\svc`, a aplikacja nosi nazwę AspNetCoreService, poleceń będzie wyglądać następująco:

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  `binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, łącznie z nazwą pliku wykonywalnego, sama.

  ![Okno konsoli tworzenie i uruchamianie przykładu](windows-service/_static/create-start.png)

  Po zakończeniu tych poleceń, można przejść do tej samej ścieżce jako uruchomienie jako aplikacji konsoli (domyślnie `http://localhost:5000`)

  ![Uruchomionych w usłudze](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>Umożliwiają uruchamianie poza usługą

Możliwe jest łatwiejsze testowanie i debugowanie podczas pracy poza usługą, dzięki czemu zwyczajowe można dodać kod, który wywołuje `host.RunAsService` tylko w niektórych warunkach.  Na przykład można uruchomić jako aplikacji konsoli Jeśli otrzymasz `--console` argumentu wiersza polecenia lub Jeśli Debuger jest dołączony.

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>Obsługa zatrzymywania i uruchamiania zdarzeń

Jeśli chcesz obsługiwać `OnStarting`, `OnStarted`, i `OnStopping` zdarzenia, wprowadź następujące zmiany dodatkowe:

* Utwórz klasę pochodną `WebHostService`.

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* Create — metoda rozszerzenia dla `IWebHost` , który przekazuje niestandardowe `WebHostService` do `ServiceBase.Run`.

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* W `Program.Main` zmiany Wywołanie nowej metody rozszerzenia, zamiast `host.RunAsService`.

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

Jeśli niestandardowe `WebHostService` kodu musi pobrać usługę z iniekcji zależności (np. Rejestrator), możesz pobrać go z `Services` właściwość `IWebHost`.

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>Następne kroki

[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) dołączony ten artykuł jest prostej aplikacji sieci web MVC, jak pokazano w poprzednich przykładach został zmodyfikowany.  Aby uruchomić go w usłudze, wykonaj następujące czynności:

* Publikowanie *c:\svc*.

* Otwórz okno administratora.

* Wprowadź następujące polecenia:

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * W przeglądarce przejdź do http://localhost: 5000, aby sprawdzić, czy jest uruchomiona.

Jeśli aplikacja nie uruchamia się zgodnie z oczekiwaniami uruchomionej w usłudze, szybko udostępnić komunikaty o błędach jest dodanie dostawcy logowania, takie jak [Dostawca dziennika zdarzeń systemu Windows](xref:fundamentals/logging/index#eventlog).

## <a name="acknowledgments"></a>Potwierdzenia

W tym artykule został napisany za pomocą źródeł, które zostały już opublikowane. Najwcześniejsza i najbardziej przydatne z nich zostały one:

* [Hosting platformy ASP.NET Core jako usługa systemu Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Jak obsługiwać podstawowych ASP.NET w usłudze systemu Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
