---
ms.openlocfilehash: 7fb5e956264f3cc7c04c4023ce69a058e5d0c0ed
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265365"
---
<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu Movie

* Uruchom następujące polecenie w wierszu polecenia (w katalogu projektu, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików):

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Jeśli zostanie wyświetlony błąd:

  ```
  No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Poprzedni błąd występuje, gdy jesteś w niewłaściwej katalogu. Otwórz powłokę wiersza polecenia do katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików), a następnie uruchom poprzednie polecenie.

Jeśli zostanie wyświetlony błąd:

  ```
  The process cannot access the file
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll'
  because it is being used by another process.
  ```

Zamknij program Visual Studio i ponownie uruchom polecenie.
