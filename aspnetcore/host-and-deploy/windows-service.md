---
title: "Host usługi systemu Windows"
author: tdykstra
description: "Dowiedz się, jak hostowanie aplikacji platformy ASP.NET Core w usłudze systemu Windows."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Host aplikacji platformy ASP.NET Core w usłudze systemu Windows

przez [Dykstra niestandardowy](https://github.com/tdykstra)

Zalecanym sposobem hostowania aplikacji platformy ASP.NET Core w systemie Windows bez za pomocą usług IIS jest uruchomienie go w [usługi systemu Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications). W ten sposób go może automatycznie uruchomić po ponownych rozruchów i awarii (Crash), bez oczekiwania na użytkownika do logowania.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)). Zobacz [następne kroki](#next-steps) sekcji, aby uzyskać instrukcje dotyczące sposobu jego uruchamiania.

## <a name="prerequisites"></a>Wymagania wstępne

* Aplikacja muszą działać na środowiska uruchomieniowego .NET Framework.  W *.csproj* pliku, podaj odpowiednie wartości dla [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) i [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog). Oto przykład:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Podczas tworzenia projektu w programie Visual Studio, użyj **aplikacji platformy ASP.NET Core (.NET Framework)** szablonu.

* Jeśli aplikacja odbiera żądania z Internetu (nie tylko z sieci wewnętrznej), należy użyć [WebListener](xref:fundamentals/servers/weblistener) serwera sieci web zamiast [Kestrel](xref:fundamentals/servers/kestrel).  Kestrel musi być używany z programem IIS we wdrożeniach krawędzi.  Aby uzyskać więcej informacji, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="getting-started"></a>Wprowadzenie

W tej sekcji opisano minimalną zmiany wymagane do skonfigurowania istniejącego projektu platformy ASP.NET Core do uruchamiania w usłudze.

* Zainstaluj pakiet NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

* Wprowadź następujące zmiany w `Program.Main`:
  
  * Wywołanie `host.RunAsService` zamiast `host.Run`.
  
  * Jeśli kod wywołuje `UseContentRoot`, użyj ścieżki do lokalizacji publikacji zamiast`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* Opublikuj aplikację w folderze.

  Użyj [publikowania dotnet](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) lub [profilu publikowania programu Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) który publikuje do folderu.

* Przetestuj tworzenia i uruchamiania usługi.

  Otwórz okno wiersza polecenia administratora, aby użyć [sc.exe](https://technet.microsoft.com/library/bb490995) wiersza polecenia narzędzia do tworzenia i uruchamiania usługi.  
  
  Jeśli usługa ma nazwę Moja_usługa, Opublikuj aplikację `c:\svc`, a aplikacja nosi nazwę AspNetCoreService, poleceń będzie wyglądać następująco:

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  `binPath` Wartość jest ścieżką do pliku wykonywalnego aplikacji, łącznie z nazwą pliku wykonywalnego, sama.

  ![Okno konsoli tworzenie i uruchamianie przykładu](windows-service/_static/create-start.png)

  Po zakończeniu tych poleceń, przejdź do tej samej ścieżki, w przypadku uruchamiania jako aplikacji konsoli (domyślnie `http://localhost:5000`)

  ![Uruchomionych w usłudze](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>Umożliwiają uruchamianie poza usługą

Możliwe jest łatwiejsze testowanie i debugowanie podczas uruchamiania poza usługą, dzięki czemu zwyczajowe można dodać kod, który wywołuje `host.RunAsService` tylko w niektórych warunkach.  Na przykład aplikacja może działać jako aplikację konsoli z `--console` argumentu wiersza polecenia lub Jeśli Debuger jest dołączony.

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>Obsługa zatrzymywania i uruchamiania zdarzeń

Do obsługi `OnStarting`, `OnStarted`, i `OnStopping` zdarzenia, wprowadź następujące zmiany dodatkowe:

* Utwórz klasę pochodną `WebHostService`.

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* Create — metoda rozszerzenia dla `IWebHost` niestandardowego, który przekazuje `WebHostService` do `ServiceBase.Run`.

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* W `Program.Main` zmiany Wywołanie nowej metody rozszerzenia, zamiast `host.RunAsService`.

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

Jeśli niestandardowa `WebHostService` kodu musi pobrać usługę z iniekcji zależności (np. Rejestrator), pobierz go z `Services` właściwość `IWebHost`.

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>Następne kroki

[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) dołączony ten artykuł jest prostej aplikacji sieci web MVC, jak pokazano w poprzednich przykładach został zmodyfikowany.  Aby uruchomić go w usłudze, wykonaj następujące czynności:

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
