---
title: Tworzenie aplikacji platformy ASP.NET Core za pomocą monitora plików
author: rick-anderson
description: Ten samouczek pokazuje, jak zainstalować i używać narzędzia obserwatora (dotnet czujki) pliku .NET Core CLI w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: 2a59267b36faf1e00ea2f0cc7e2b9ceb9828f791
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278856"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a>Tworzenie aplikacji platformy ASP.NET Core za pomocą monitora plików

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Hurdugaci zwycięzcę](https://twitter.com/victorhurdugaci)

`dotnet watch` to narzędzie, które uruchamia [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools) poleceń podczas zmiany plików źródłowych. Na przykład zmianę pliku może wyzwolić kompilację, wykonywania testów lub wdrożenia.

W tym samouczku korzysta z istniejącej interfejsu API sieci web z dwa punkty końcowe:, który zwraca sumę i zwracające produktu. Metoda produktu ma usterki, które zawartych w tym samouczku.

Pobierz [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample). Zawiera dwa projekty: *aplikacji sieci Web* (platformy ASP.NET Core interfejsu API sieci web) i *WebAppTests* (testów jednostkowych dla interfejsu API sieci web).

W powłoce poleceń, przejdź do *aplikacji sieci Web* folderu. Uruchom następujące polecenie:

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

Przejdź do produktu interfejsu API (`http://localhost:<port number>/api/math/product?a=4&b=5`). Zwraca `9`, a nie `20` zgodnie z regułami. Ten problem został rozwiązany później w samouczku.

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a>Dodaj `dotnet watch` do projektu

`dotnet watch` Narzędzia obserwatora plików są dołączone do wersji 2.1.300 zestawu SDK .NET Core. Poniższe kroki są wymagane w przypadku wcześniejszych wersji programu .NET Core SDK.

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

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a>Uruchom przy użyciu polecenia interfejsu wiersza polecenia platformy .NET Core `dotnet watch`

Wszelkie [polecenia interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools#cli-commands) może być uruchamiane przy `dotnet watch`. Na przykład:

| Polecenie | Polecenie z czujki |
| ---- | ----- |
| Uruchom DotNet | Uruchom czujki DotNet |
| Uruchom netcoreapp2.0 -f DotNet | Uruchom netcoreapp2.0 -f czujki DotNet |
| Uruchom netcoreapp2.0 -f - DotNet — arg1 | Obejrzyj DotNet Uruchom netcoreapp2.0 -f ---arg1 |
| DotNet test | test czujki DotNet |

Uruchom `dotnet watch run` w *aplikacji sieci Web* folderu. Dane wyjściowe konsoli wskazuje `watch` została uruchomiona.

## <a name="make-changes-with-dotnet-watch"></a>Wprowadź zmiany z `dotnet watch`

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

## <a name="run-tests-using-dotnet-watch"></a>Uruchom testy przy użyciu `dotnet watch`

1. Zmień `Product` metody *MathController.cs* do zwracania suma. Zapisz plik.
1. W powłoce poleceń, przejdź do *WebAppTests* folderu.
1. Uruchom [przywracania dotnet](/dotnet/core/tools/dotnet-restore).
1. Run `dotnet watch test`. Dane wyjściowe wskazuje, że testowanie nie powiodło się i że obserwatora oczekuje na zmiany w pliku:

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. Usuń `Product` metody kod, tak aby zwracało produktu. Zapisz plik.

`dotnet watch` wykrywa zmiany pliku i zwracające testy. Dane wyjściowe konsoli wskazuje testy zostały zakończone pomyślnie.

## <a name="customize-files-list-to-watch"></a>Dostosowywanie listy plików do monitorowania

Domyślnie `dotnet-watch` śledzi wszystkie pliki następujące wzorce glob dopasowywania:

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

Można dodać więcej elementów do listy czujki, edytując *.csproj* pliku. Można określić elementów pojedynczo lub za pomocą glob wzorce.

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a>Wypisz pliki, aby być monitorowane

`dotnet-watch` można skonfigurować tak, aby zignorować jego domyślnych ustawień. Ignorowanie określonych plików, dodawanie `Watch="false"` atrybutu do definicji elementu w *.csproj* pliku:

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

## <a name="custom-watch-projects"></a>Projekty niestandardowych czujki

`dotnet-watch` nie jest ograniczony do projektów C#. Można tworzyć projektów niestandardowych czujki do obsługi różnych scenariuszy. Należy wziąć pod uwagę następujące układ projektu:

* **Testowanie /**
  * *UnitTests/UnitTests.csproj*
  * *IntegrationTests/IntegrationTests.csproj*

Jeśli celem jest Obejrzyj oba projekty, Utwórz plik projektu niestandardowe skonfigurowane do wykrywania oba projekty:

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

Aby uruchomić plik obserwowanie na obu projektów, zmienić *test* folderu. Uruchom następujące polecenie:

```console
dotnet watch msbuild /t:Test
```

VSTest wykonuje po zmianie dowolnego pliku w każdym projekcie testowym.

## <a name="dotnet-watch-in-github"></a>`dotnet-watch` w witrynie GitHub

`dotnet-watch` wchodzi w skład GitHub [repozytorium DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).
