---
ms.openlocfilehash: 7fb5e956264f3cc7c04c4023ce69a058e5d0c0ed
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265365"
---
<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a><span data-ttu-id="f8dc4-101">Tworzenie szkieletu modelu Movie</span><span class="sxs-lookup"><span data-stu-id="f8dc4-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="f8dc4-102">Uruchom następujące polecenie w wierszu polecenia (w katalogu projektu, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików):</span><span class="sxs-lookup"><span data-stu-id="f8dc4-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="f8dc4-103">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="f8dc4-103">If you get the error:</span></span>

  ```
  No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="f8dc4-104">Poprzedni błąd występuje, gdy jesteś w niewłaściwej katalogu.</span><span class="sxs-lookup"><span data-stu-id="f8dc4-104">The preceding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="f8dc4-105">Otwórz powłokę wiersza polecenia do katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików), a następnie uruchom poprzednie polecenie.</span><span class="sxs-lookup"><span data-stu-id="f8dc4-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceding command.</span></span>

<span data-ttu-id="f8dc4-106">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="f8dc4-106">If you get the error:</span></span>

  ```
  The process cannot access the file
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll'
  because it is being used by another process.
  ```

<span data-ttu-id="f8dc4-107">Zamknij program Visual Studio i ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="f8dc4-107">Exit Visual Studio and run the command again.</span></span>
