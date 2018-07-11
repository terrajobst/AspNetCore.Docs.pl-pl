---
title: Dodawanie modelu do aplikacji stron Razor programu ASP.NET Core z programem Visual Studio dla komputerów Mac
author: rick-anderson
description: Dowiedz się, jak dodać model do aplikacji stron Razor w programie ASP.NET Core przy użyciu programu Visual Studio dla komputerów Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186761"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="ee195-103">Dodawanie modelu do aplikacji stron Razor programu ASP.NET Core z programem Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ee195-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="ee195-104">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="ee195-104">Add a data model</span></span>

* <span data-ttu-id="ee195-105">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="ee195-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="ee195-106">Nazwa folderu *modeli*.</span><span class="sxs-lookup"><span data-stu-id="ee195-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="ee195-107">Kliknij prawym przyciskiem myszy *modeli* folder, a następnie wybierz **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="ee195-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="ee195-108">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="ee195-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="ee195-109">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="ee195-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="ee195-110">Wybierz **pustą klasę** w ból Centrum.</span><span class="sxs-lookup"><span data-stu-id="ee195-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="ee195-111">Nazwa klasy **filmu** i wybierz **New**.</span><span class="sxs-lookup"><span data-stu-id="ee195-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="ee195-112">Kliknij prawym przyciskiem myszy czerwona linia falista, na przykład `MovieContext` w wierszu `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="ee195-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="ee195-113">Wybierz **Quick Fix > przy użyciu RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="ee195-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="ee195-114">Program Visual studio dodaje używając instrukcji.</span><span class="sxs-lookup"><span data-stu-id="ee195-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="ee195-115">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="ee195-115">Build the project to verify you don't have any errors.</span></span>

![Tworzenie strony](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="ee195-117">Entity Framework Core NuGet pakietów dla migracji</span><span class="sxs-lookup"><span data-stu-id="ee195-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="ee195-118">Narzędzia platformy EF dla interfejsu wiersza polecenia (CLI) znajdują się w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="ee195-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="ee195-119">Kliknij pozycję [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) łącze, aby uzyskać numer wersji do użycia.</span><span class="sxs-lookup"><span data-stu-id="ee195-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="ee195-120">Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="ee195-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="ee195-121">**Uwaga:** należy zainstalować ten pakiet, edytując *.csproj* pliku; nie można użyć `install-package` polecenia lub graficznego interfejsu użytkownika Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="ee195-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="ee195-122">Aby edytować *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="ee195-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="ee195-123">Wybierz **pliku** > **Otwórz**, a następnie wybierz pozycję *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="ee195-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="ee195-124">Wybierz **opcje**.</span><span class="sxs-lookup"><span data-stu-id="ee195-124">Select **Options**.</span></span>
* <span data-ttu-id="ee195-125">Zmiana **Otwórz za pomocą** do **Edytor kodu źródłowego**.</span><span class="sxs-lookup"><span data-stu-id="ee195-125">Change **Open with** to **Source Code Editor**.</span></span>

![Edytuj plik csproj](model/csproj.png)

<span data-ttu-id="ee195-127">Dodaj `Microsoft.EntityFrameworkCore.Tools.DotNet` narzędzia odwołanie do drugiego  **\<ItemGroup >**.:</span><span class="sxs-lookup"><span data-stu-id="ee195-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="ee195-128">Numery wersji pokazano w poniższym kodzie były prawidłowe w momencie pisania.</span><span class="sxs-lookup"><span data-stu-id="ee195-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="ee195-129">Dodaj pliki stron/filmów do projektu</span><span class="sxs-lookup"><span data-stu-id="ee195-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="ee195-130">W programie Visual Studio, kliknij prawym przyciskiem myszy *stron* i wybierz polecenie **Dodaj > Dodaj istniejący Folder**.</span><span class="sxs-lookup"><span data-stu-id="ee195-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="ee195-131">Wybierz *filmy* folderu.</span><span class="sxs-lookup"><span data-stu-id="ee195-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="ee195-132">W *wybierz pliki do uwzględnienia w projekcie* okno dialogowe, wybierz opcję **obejmują wszystkie**.</span><span class="sxs-lookup"><span data-stu-id="ee195-132">In the *Choose files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="ee195-133">Następnego samouczka opisano plików utworzonych przez tworzenie szkieletów.</span><span class="sxs-lookup"><span data-stu-id="ee195-133">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ee195-134">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages-mac/razor-pages-start)
> [dalej: działanie stron Razor](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="ee195-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
