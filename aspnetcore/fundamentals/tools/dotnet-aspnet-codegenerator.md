---
title: polecenie elementu codegenerator aspnet DotNet
author: rick-anderson
description: Polecenia dotnet elementu codegenerator aspnet szkielety mechanizmów projektów ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/04/2019
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: c96362f320efd84c35dc07294a2968a2c687ee94
ms.sourcegitcommit: b9e914ef274b5ec359582f299724af6234dce135
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67596140"
---
# <a name="dotnet-aspnet-codegenerator"></a>dotnet aspnet-codegenerator

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

`dotnet aspnet-codegenerator` — Działa aparat tworzenia szkieletu ASP.NET Core. `dotnet aspnet-codegenerator` jest tylko wymagane do tworzenia szkieletu z wiersza polecenia, nie jest konieczność tworzenia szkieletu za pomocą programu Visual Studio.

Ten artykuł dotyczy [zestawu SDK programu .NET Core 2.1](https://dotnet.microsoft.com/download/dotnet-core/2.1) i nowszych.

## <a name="installing-aspnet-codegenerator"></a>Instalowanie elementu codegenerator aspnet

`dotnet-aspnet-codegenerator` jest [narzędzie globalne](/dotnet/core/tools/global-tools) musi być zainstalowany. Poniższe polecenie instaluje najnowszą stabilną wersję `dotnet-aspnet-codegenerator` narzędzie:

```console
dotnet tool install -g dotnet-aspnet-codegenerator
```

Następujące polecenie aktualizacji `dotnet-aspnet-codegenerator` do najnowszej stabilnej wersji dostępne z zainstalowanych zestawów .NET Core SDK:

```console
dotnet tool update -g dotnet-aspnet-codegenerator
```

## <a name="synopsis"></a>Streszczenie

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a>Opis

`dotnet aspnet-codegenerator` Globalnego polecenie uruchamia platformy ASP.NET Core, generator kodu i aparatu do tworzenia szkieletów.

## <a name="arguments"></a>Argumenty

`generator`

Generator kodu do uruchomienia. Dostępne są następujące generatory:

| Generator | Operacja |
| ----------------- | ------------ | 
| Obszar      | [Szkielety mechanizmów obszar](/aspnet/core/mvc/controllers/areas) |
  kontroler| [Scaffolds kontrolera](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  tożsamość  | [Szkielety mechanizmów tożsamości](/aspnet/core/security/authentication/scaffold-identity) |
  razorpage | [Szkielety mechanizmów stron Razor](/aspnet/core/tutorials/razor-pages/model) |
  Widok      | [Scaffolds widoku](/aspnet/core/mvc/views/overview) |

## <a name="options"></a>Opcje

`-n|--nuget-package-dir`

Określa katalog pakietu NuGet.

`-c|--configuration {Debug|Release}`

Definiuje konfigurację kompilacji. Wartość domyślna to `Debug`.

`-tfm|--target-framework`

Docelowy [Framework](/dotnet/standard/frameworks) do użycia. Na przykład `net46`.

`-b|--build-base-path`

Ścieżka podstawowa kompilacji.

`-h|--help`

Drukuje krótki pomoc dotyczącą polecenia.

`--no-build`

Nie da się skompilować projekt przed uruchomieniem. Ustawia również niejawnie `--no-restore` flagi.

`-p|--project <PATH>`

Określa ścieżkę do pliku projektu do uruchomienia (nazwa folderu lub pełną ścieżkę). Jeśli nie zostanie określony, jego wartość domyślna w bieżącym katalogu.

## <a name="generator-options"></a>Opcje Generator

Opcje dostępne dla obsługiwanych generatorów można znaleźć w poniższych sekcjach:

* Obszar
* Kontroler
* Tożsamość  
* Razorpage
* Widok

<a name="area"></a>

### <a name="area-options"></a>Opcji obszaru

To narzędzie jest przeznaczony dla projektów sieci web platformy ASP.NET Core za pomocą kontrolerów i widoków. Nie jest przeznaczona dla aplikacji stron Razor.

Użycie: `dotnet aspnet-codegenerator area AreaNameToGenerate`

Poprzednie polecenie generuje następujące foldery:

* *Obszary*
  * *AreaNameToGenerate*
    * *Kontrolery*
    * *Dane*
    * *Modele*
    * *Widoki*

<a name="ctl"></a>

### <a name="controller-options"></a>Opcje kontrolera

W poniższej tabeli wymieniono opcje `aspnet-codegenerator` `controller` i `razorpage`:

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

W poniższej tabeli wymieniono opcje unikatowe dla `aspnet-codegenerator controller`:

| Opcja               | Opis|
| ----------------- | ------------ |
| Element controllerName — lub — nazwa | Nazwa kontrolera. |
| --useAsyncActions lub - async | Generowanie asynchronicznych akcji kontrolera. |
| --noViews lub -nv | Generowanie **nie** widoków. |
| --restWithNoViews lub - interfejsu api  | Generowanie kontrolera ze stylem REST API. `noViews` przyjęto, że i dowolne Wyświetl powiązane opcje są ignorowane. |
| --readWriteActions lub - akcje | Generowanie kontroler z akcjami odczytu/zapisu bez modelu. |

Użyj `-h` przełączać w celu uzyskania pomocy na `aspnet-codegenerator controller` polecenia:

```console
dotnet aspnet-codegenerator controller -h
```

Zobacz [tworzenia szkieletu modelu movie](/aspnet/core/tutorials/razor-pages/model) przykład `dotnet aspnet-codegenerator controller`.

### <a name="razorpage"></a>Razorpage

<a name="rp"></a>

Strony razor można indywidualnie szkieletu, określając nazwę nowej strony i szablon do użycia. Są obsługiwane szablony:

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

Na przykład następujące polecenie używa Edytuj szablon do generowania *MyEdit.cshtml* i *MyEdit.cshtml.cs*:

```console
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

Typowo, szablon i wygenerowanej nazwy pliku nie zostanie określony, i są tworzone następujące szablony:

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

W poniższej tabeli wymieniono opcje `aspnet-codegenerator` `razorpage` i `controller`:

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

W poniższej tabeli wymieniono opcje unikatowe dla `aspnet-codegenerator razorpage`:

| Opcja               | Opis|
| ----------------- | ------------ |
|   --namespaceName lub — przestrzeń nazw | Nazwa przestrzeni nazw do użycia dla wygenerowanej klasy PageModel |
| --partialView ani - częściowy | Generowanie widoku częściowego. Układ opcji -l i - udl są ignorowane, jeśli jest określony. |
| --noPageModel lub - npm | Przełącz na nie Generuj klasę PageModel w przypadku pustego szablonu |

Użyj `-h` przełączać w celu uzyskania pomocy na `aspnet-codegenerator razorpage` polecenia:

```console
dotnet aspnet-codegenerator razorpage -h
```

Zobacz [tworzenia szkieletu modelu movie](/aspnet/core/tutorials/razor-pages/model) przykład `dotnet aspnet-codegenerator razorpage`.

### <a name="identity"></a>Tożsamość

Zobacz [tworzenia szkieletu tożsamości](/aspnet/core/security/authentication/scaffold-identity)
