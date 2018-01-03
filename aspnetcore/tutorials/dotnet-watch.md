---
title: "Tworzenie aplikacji platformy ASP.NET Core za pomocą czujki dotnet"
author: rick-anderson
description: "Ten samouczek pokazuje, jak zainstalować i używać narzędzia obserwatora (dotnet czujki) pliku .NET Core CLI w aplikacji platformy ASP.NET Core."
keywords: "Platformy ASP.NET Core, przy użyciu czujki dotnet"
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 1b26beaa41f4b38e0cfd2c8300cb521a3dcce47d
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>Tworzenie aplikacji platformy ASP.NET Core za pomocą czujki dotnet

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Hurdugaci zwycięzcę](https://twitter.com/victorhurdugaci)

`dotnet watch`to narzędzie, które uruchamia [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools) poleceń podczas zmiany plików źródłowych. Na przykład zmianę pliku może wyzwolić kompilację, wykonywania testów lub wdrożenia.

W tym samouczku używamy istniejącej aplikacji interfejsu API sieci Web z dwa punkty końcowe:, który zwraca sumę i zwracające produktu. Metoda produktu zawiera usterkę, która będzie możemy naprawić w ramach tego samouczka.

Pobierz [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). Zawiera dwa projekty: *aplikacji sieci Web* (platformy ASP.NET Core interfejsu API sieci Web) i *WebAppTests* (testów jednostkowych dla interfejsu API sieci Web).

W powłoce poleceń, przejdź do *aplikacji sieci Web* folder i uruchom następujące polecenie:

```console
dotnet run
```

Dane wyjściowe konsoli przedstawia komunikaty podobne do następujących (co oznacza, że aplikacja jest uruchomiona i oczekujące na żądania):

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

W przeglądarce sieci web, przejdź do `http://localhost:<port number>/api/math/sum?a=4&b=5`. Należy sprawdzić działanie `9`.

Przejdź do produktu interfejsu API (`http://localhost:<port number>/api/math/product?a=4&b=5`). Zwraca `9`, a nie `20` zgodnie z regułami. Firma Microsoft będzie rozwiązać ten problem, później w samouczku.

## <a name="add-dotnet-watch-to-a-project"></a>Dodaj `dotnet watch` do projektu

1. Dodaj `Microsoft.DotNet.Watcher.Tools` odwołanie do pakietu *.csproj* pliku:

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. Zainstaluj `Microsoft.DotNet.Watcher.Tools` pakietu, uruchamiając następujące polecenie:
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a>Za pomocą polecenia interfejsu wiersza polecenia platformy .NET Core.`dotnet watch`

Wszelkie [polecenia interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools#cli-commands) może być uruchamiane przy `dotnet watch`. Na przykład:

| Polecenie | Polecenie z czujki |
| ---- | ----- |
| Uruchom DotNet | Uruchom czujki DotNet |
| Uruchom netcoreapp2.0 -f DotNet | Uruchom netcoreapp2.0 -f czujki DotNet |
| Uruchom netcoreapp2.0 -f - DotNet — arg1 | Obejrzyj DotNet Uruchom netcoreapp2.0 -f ---arg1 |
| DotNet test | test czujki DotNet |

Uruchom `dotnet watch run` w *aplikacji sieci Web* folderu. Dane wyjściowe konsoli wskazuje `watch` została uruchomiona.

## <a name="making-changes-with-dotnet-watch"></a>Wprowadzanie zmian z`dotnet watch`

Upewnij się, że `dotnet watch` jest uruchomiona.

Napraw błąd w `Product` metody *MathController.cs* tak aby zwracało produktu, a nie ich sumę:

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

Zapisz plik. Dane wyjściowe konsoli wskazuje, że `dotnet watch` Wykryto zmianę pliku i ponownym uruchomieniu aplikacji.

Sprawdź `http://localhost:<port number>/api/math/product?a=4&b=5` zwraca prawidłowego wyniku.

## <a name="running-tests-using-dotnet-watch"></a>Uruchamianie testów za pomocą`dotnet watch`

1. Zmień `Product` metody *MathController.cs* z powrotem do zwracania Suma i Zapisz plik.
1. W powłoce poleceń, przejdź do *WebAppTests* folderu.
1. Run `dotnet restore`.
1. Run `dotnet watch test`. Dane wyjściowe wskazuje, że testowanie nie powiodło się i tym obserwatora oczekuje na zmiany w pliku:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. Usuń `Product` metody kod, tak aby zwracało produktu. Zapisz plik.

`dotnet watch`wykrywa zmiany pliku i zwracające testy. Dane wyjściowe konsoli wskazuje testy zostały zakończone pomyślnie.

## <a name="dotnet-watch-in-github"></a>wyrażenie kontrolne DotNet w serwisie GitHub

Obejrzyj DotNet jest częścią usługi GitHub [repozytorium DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).

[Sekcji MSBuild](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) z [ReadMe czujki dotnet](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) wyjaśniono, jak czujki dotnet można skonfigurować w pliku projektu MSBuild są monitorowane. [ReadMe czujki dotnet](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) zawiera informacje na temat dotnet czujki nieuwzględnione w tym samouczku.
