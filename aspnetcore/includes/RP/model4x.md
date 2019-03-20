---
ms.openlocfilehash: 9ac005f3381e02ad31c0b1b87f8d55a05cc0b6cb
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265309"
---
<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu Movie

* Uruchom następujące polecenie w wierszu polecenia (w katalogu projektu, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików):

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Jeśli zostanie wyświetlony błąd:

  ```
  No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Otwórz powłokę wiersza polecenia do katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
