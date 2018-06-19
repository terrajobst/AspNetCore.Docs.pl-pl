---
title: Dodaj model do aplikacji platformy ASP.NET Core Razor strony z kodem Visual Studio
author: rick-anderson
description: Dowiedz się, jak dodać modelu do aplikacji stron Razor w ASP.NET Core za pomocą programu Visual Studio Code.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 20282b491162e9f35e40702655532a78edceb89a
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32078515"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="fa68d-103">Dodaj model do aplikacji platformy ASP.NET Core Razor strony z kodem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fa68d-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="fa68d-104">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="fa68d-104">Add a data model</span></span>

* <span data-ttu-id="fa68d-105">Dodaj folder o nazwie *modele*.</span><span class="sxs-lookup"><span data-stu-id="fa68d-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="fa68d-106">Dodaj klasę do *modele* folder o nazwie *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="fa68d-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="fa68d-107">Dodaj następujący kod do *Models/Movie.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="fa68d-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="fa68d-108">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="fa68d-108">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="fa68d-109">Entity Framework Core NuGet pakietów dla migracji</span><span class="sxs-lookup"><span data-stu-id="fa68d-109">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="fa68d-110">Narzędzia EF dla interfejsu wiersza polecenia (CLI) są dostępne w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="fa68d-110">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="fa68d-111">Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="fa68d-111">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="fa68d-112">**Uwaga:** należy zainstalować ten pakiet, edytując *.csproj* pliku; nie można użyć `install-package` polecenia lub graficznego interfejsu użytkownika Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="fa68d-112">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="fa68d-113">Edytuj *RazorPagesMovie.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="fa68d-113">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="fa68d-114">Wybierz **pliku** > **Otwórz plik**, a następnie wybierz *RazorPagesMovie.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="fa68d-114">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="fa68d-115">Dodaj odwołanie do narzędzia dla `Microsoft.EntityFrameworkCore.Tools.DotNet` drugiej  **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="fa68d-115">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="fa68d-116">Tworzenie szkieletu modelu film</span><span class="sxs-lookup"><span data-stu-id="fa68d-116">Scaffold the Movie model</span></span>

* <span data-ttu-id="fa68d-117">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="fa68d-117">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fa68d-118">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="fa68d-118">Run the following command:</span></span>

<span data-ttu-id="fa68d-119">**Uwaga: Uruchom następujące polecenie w systemie Windows. System MacOS i Linux zobacz następne polecenie**</span><span class="sxs-lookup"><span data-stu-id="fa68d-119">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="fa68d-120">System MacOS i Linux uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="fa68d-120">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="fa68d-121">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="fa68d-121">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="fa68d-122">Zamknij program Visual Studio i ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="fa68d-122">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="fa68d-123">Następny samouczek wyjaśnia plików utworzonych przez funkcję szkieletów.</span><span class="sxs-lookup"><span data-stu-id="fa68d-123">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fa68d-124">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [dalej: szkieletu stron Razor](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="fa68d-124">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
