---
title: Opracowywanie aplikacji ASP.NET Core przy użyciu obserwatora plików
author: rick-anderson
description: W tym samouczku pokazano, jak zainstalować i używać narzędzia obserwatora plików interfejs wiersza polecenia platformy .NET Core (czujka dotnet) w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: bedb3e6a65839db915ca7bc821a267a14d34bf30
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667413"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a>Opracowywanie aplikacji ASP.NET Core przy użyciu obserwatora plików

Autorzy [Rick Anderson](https://twitter.com/RickAndMSFT) i [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

[czujka dotnet](https://www.nuget.org/packages/dotnet-watch) jest narzędziem, które uruchamia [interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools) polecenie, gdy pliki źródłowe zmieniają się. Na przykład zmiana pliku może wyzwolić kompilację, wykonanie testu lub wdrożenie.

W tym samouczku jest wykorzystywany istniejący internetowy interfejs API z dwoma punktami końcowymi: jeden, który zwraca sumę i jeden z nich zwraca produkt. Metoda produktu zawiera usterkę, która została rozwiązana w tym samouczku.

Pobierz [przykładową aplikację](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). Składa się z dwóch projektów: *webapp* (ASP.NET Core Web API) i *WebAppTests* (testy jednostkowe dla internetowego interfejsu API).

W powłoce poleceń przejdź do folderu *webapp* . Uruchom następujące polecenie:

```dotnetcli
dotnet run
```

> [!NOTE]
> Możesz użyć `dotnet run --project <PROJECT>`, aby określić projekt do uruchomienia. Na przykład uruchomienie `dotnet run --project WebApp` z poziomu głównego przykładowej aplikacji spowoduje również uruchomienie projektu *webapp* .

W danych wyjściowych konsoli są wyświetlane komunikaty podobne do następujących (wskazuje, że aplikacja działa i oczekuje na żądania):

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

W przeglądarce sieci Web przejdź do `http://localhost:<port number>/api/math/sum?a=4&b=5`. Powinien zostać wyświetlony wynik `9`.

Przejdź do interfejsu API produktu (`http://localhost:<port number>/api/math/product?a=4&b=5`). Zwraca `9`, nie `20` zgodnie z oczekiwaniami. Ten problem został rozwiązany w dalszej części tego samouczka.

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a>Dodawanie `dotnet watch` do projektu

Narzędzie obserwatora plików `dotnet watch` jest dołączone do wersji 2.1.300 zestaw .NET Core SDK. Poniższe kroki są wymagane w przypadku korzystania ze starszej wersji zestaw .NET Core SDK.

1. Dodaj odwołanie do pakietu `Microsoft.DotNet.Watcher.Tools` do pliku *csproj* :

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. Zainstaluj pakiet `Microsoft.DotNet.Watcher.Tools`, uruchamiając następujące polecenie:

    ```dotnetcli
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a>Uruchamianie poleceń interfejs wiersza polecenia platformy .NET Core przy użyciu `dotnet watch`

Każde [polecenie interfejs wiersza polecenia platformy .NET Core](/dotnet/core/tools#cli-commands) można uruchomić z `dotnet watch`. Na przykład:

| Polecenie | Polecenie z zegarkiem |
| ---- | ----- |
| dotnet run | uruchomienie czujka dotnet |
| uruchomienie dotnet-f netcoreapp 2.0 | uruchomienie czujka dotnet-f netcoreapp 2.0 |
| uruchomienie dotnet-f netcoreapp 2.0----arg1 | uruchomienie czujka dotnet-f netcoreapp 2.0----arg1 |
| dotnet test | Test czujki dotnet |

Uruchom `dotnet watch run` w folderze *webapp* . Dane wyjściowe konsoli wskazują, `watch` zostało uruchomione.

> [!NOTE]
> Możesz użyć `dotnet watch --project <PROJECT>`, aby określić projekt do obejrzenia. Na przykład uruchomienie `dotnet watch --project WebApp run` z poziomu głównego przykładowej aplikacji spowoduje również uruchomienie i obejrzyj projekt *webapp* .

## <a name="make-changes-with-dotnet-watch"></a>Wprowadzanie zmian przy użyciu `dotnet watch`

Upewnij się, że `dotnet watch` jest uruchomiony.

Usuń usterkę w `Product` metodzie *MathController.cs* , aby zwracała produkt, a nie sumę:

```csharp
public static int Product(int a, int b)
{
    return a * b;
}
```

Zapisz plik. Dane wyjściowe konsoli wskazują, że `dotnet watch` wykryła zmianę pliku i ponownie uruchomiła aplikację.

Sprawdź, czy `http://localhost:<port number>/api/math/product?a=4&b=5` zwraca prawidłowy wynik.

## <a name="run-tests-using-dotnet-watch"></a>Uruchom testy przy użyciu `dotnet watch`

1. Zmień metodę `Product` *MathController.cs* z powrotem, aby zwracać sumę. Zapisz plik.
1. W powłoce poleceń przejdź do folderu *WebAppTests* .
1. Uruchom [dotnet Restore](/dotnet/core/tools/dotnet-restore).
1. Uruchom polecenie `dotnet watch test`. Dane wyjściowe wskazują, że test zakończył się niepowodzeniem i że obserwator oczekuje na zmiany plików:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. Popraw kod metody `Product`, aby zwracał produkt. Zapisz plik.

`dotnet watch` wykrywa zmianę pliku i ponownie uruchamia testy. Dane wyjściowe konsoli wskazują zakończono testy.

## <a name="customize-files-list-to-watch"></a>Dostosuj listę plików do czujki

Domyślnie program `dotnet-watch` śledzi wszystkie pliki zgodne z następującymi wzorcami globalizowania:

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

Więcej elementów można dodać do listy obserwacje, edytując plik *csproj* . Elementy można określać pojedynczo lub przy użyciu wzorców globalizowania.

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a>Rezygnacja z plików, które mają być obserwowane

`dotnet-watch` można skonfigurować do ignorowania ustawień domyślnych. Aby zignorować określone pliki, Dodaj atrybut `Watch="false"` do definicji elementu w pliku *. csproj* :

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

## <a name="custom-watch-projects"></a>Niestandardowe projekty czujki

`dotnet-watch` nie jest ograniczony C# do projektów. Niestandardowe projekty czujki mogą być tworzone w celu obsługi różnych scenariuszy. Rozważmy następujący układ projektu:

* **badan**
  * *UnitTests/UnitTests. csproj*
  * *IntegrationTests/IntegrationTests. csproj*

Jeśli celem jest oglądanie obu projektów, Utwórz niestandardowy plik projektu, który został skonfigurowany do oglądania obu projektów:

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

Aby rozpocząć obserwowanie plików w obu projektach, przejdź do folderu *testowego* . Wykonaj następujące polecenie:

```dotnetcli
dotnet watch msbuild /t:Test
```

VSTest wykonuje się, gdy jakiekolwiek zmiany pliku w każdym projekcie testowym.

## <a name="dotnet-watch-in-github"></a>`dotnet-watch` w serwisie GitHub

`dotnet-watch` jest częścią [repozytorium dotnet/AspNetCore](https://github.com/dotnet/AspNetCore/tree/master/src/Tools/dotnet-watch)usługi GitHub.
