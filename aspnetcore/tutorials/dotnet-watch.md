---
title: Tworzenie aplikacji platformy ASP.NET Core za pomocą obserwator plików
author: rick-anderson
description: W tym samouczku pokazano, jak zainstalować i używać narzędzia obserwatora (dotnet watch) plików .NET Core interfejsu wiersza polecenia w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: f1e0d91b27df4af7cbfb6f2547c94c0370c65d0d
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207505"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a>Tworzenie aplikacji platformy ASP.NET Core za pomocą obserwator plików

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` to narzędzie, które uruchamia [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools) polecenia, gdy zmiany plików źródłowych. Na przykład zmianę pliku może wyzwalać kompilacja, wykonywanie testów lub wdrożenia.

W tym samouczku jest używany istniejący interfejs API sieci web z dwoma punktami końcowymi: taki, który zwraca sumę i jedną, która zwraca produktu. Metoda produktu zawiera usterkę, zostanie rozwiązany w ramach tego samouczka.

Pobierz [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). Składa się z dwóch projektów: *Aplikacja sieci Web* (Platforma ASP.NET Core internetowego interfejsu API) i *WebAppTests* (testy jednostkowe dla interfejsu API sieci web).

W powłoce poleceń, przejdź do *aplikacji sieci Web* folderu. Uruchom następujące polecenie:

```console
dotnet run
```

Dane wyjściowe konsoli zawierają komunikaty podobne do następujących (co oznacza, że aplikacja działa i oczekujące na żądania):

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

W przeglądarce internetowej przejdź do `http://localhost:<port number>/api/math/sum?a=4&b=5`. Powinien zostać wyświetlony wynik `9`.

Przejdź do produktu API (`http://localhost:<port number>/api/math/product?a=4&b=5`). Zwraca `9`, a nie `20` zgodnie z regułami. Ten problem został rozwiązany w dalszej części tego samouczka.

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a>Dodaj `dotnet watch` do projektu

`dotnet watch` Pliku obserwatora narzędzie jest dołączone do wersji 2.1.300 .NET Core SDK. Poniższe kroki są wymagane, gdy przy użyciu wcześniejszej wersji programu .NET Core SDK.

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

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a>Za pomocą poleceń interfejsu wiersza polecenia platformy .NET Core. `dotnet watch`

Wszelkie [polecenia interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools#cli-commands) mogą być uruchamiane za pomocą `dotnet watch`. Na przykład:

| Polecenie | Polecenie z zegarkiem |
| ---- | ----- |
| Uruchom polecenia DotNet | Obejrzyj DotNet, uruchom |
| polecenia DotNet, uruchom netcoreapp2.0 -f | Obejrzyj DotNet, uruchom netcoreapp2.0 -f |
| Uruchom netcoreapp2.0 -f - DotNet-arg1 | Obejrzyj DotNet, uruchom netcoreapp2.0 -f — — arg1 |
| polecenia DotNet test | polecenia DotNet test wyrażenie kontrolne |

Uruchom `dotnet watch run` w *aplikacji sieci Web* folderu. Dane wyjściowe konsoli wskazuje `watch` została uruchomiona.

## <a name="make-changes-with-dotnet-watch"></a>Wprowadzić zmiany za pomocą `dotnet watch`

Upewnij się, że `dotnet watch` jest uruchomiona.

Naprawienie usterki w poleceniu `Product` metody *MathController.cs* tak aby zwracało poprawnie produktu i nie o sumę wartości:

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

Zapisz plik. Dane wyjściowe konsoli wskazuje, że `dotnet watch` Wykryto zmianę pliku i ponownym uruchomieniu aplikacji.

Sprawdź `http://localhost:<port number>/api/math/product?a=4&b=5` zwraca odpowiedni wynik.

## <a name="run-tests-using-dotnet-watch"></a>Uruchom testy przy użyciu `dotnet watch`

1. Zmiana `Product` metody *MathController.cs* do zwracania sumy. Zapisz plik.
1. W powłoce poleceń, przejdź do *WebAppTests* folderu.
1. Uruchom [dotnet restore](/dotnet/core/tools/dotnet-restore).
1. Uruchom `dotnet watch test`. Dane wyjściowe wskazuje, że test nie powiódł się i o obserwatora oczekujących na zmiany w plikach:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. Napraw `Product` metoda kodu, tak aby zwracało poprawnie produktu. Zapisz plik.

`dotnet watch` wykrywa zmianę pliku i uruchomienia testów. Dane wyjściowe konsoli wskazuje testy zostały zakończone pomyślnie.

## <a name="customize-files-list-to-watch"></a>Dostosuj listę plików, aby obejrzeć

Domyślnie `dotnet-watch` śledzi wszystkich plików zgodnych z następujących wzorców glob:

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

Można dodać więcej elementów do listy obserwowanych przez edycję *.csproj* pliku. Elementy można określić osobno lub za pomocą glob wzorców.

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a>Zoptymalizowany pod kątem na poziomie plików, aby być obserwowane

`dotnet-watch` można skonfigurować tak, aby zignorować jego domyślnych ustawień. Aby zignorować określonych plików, należy dodać `Watch="false"` atrybutu do definicji przedmiotu w *.csproj* pliku:

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a>Obejrzyj niestandardowych projektów

`dotnet-watch` nie jest ograniczony do projektów C#. Obejrzyj niestandardowe projekty mogą być tworzone do obsługi różnych scenariuszy. Należy wziąć pod uwagę następujące układ projektu:

* **Test /**
  * *UnitTests/UnitTests.csproj*
  * *IntegrationTests/IntegrationTests.csproj*

Jeśli celem jest, aby obejrzeć oba projekty, należy utworzyć plik projektu niestandardowe skonfigurowane do obu projektów:

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

Aby uruchomić plik oglądania na obu projektów, zmienić *test* folderu. Wykonaj następujące polecenie:

```console
dotnet watch msbuild /t:Test
```

VSTest wykonuje, gdy wszelkich zmian w plikach w dowolnym projekcie testowym.

## <a name="dotnet-watch-in-github"></a>`dotnet-watch` w usłudze GitHub

`dotnet-watch` jest częścią usługi GitHub [repozytorium aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Tools/dotnet-watch).
