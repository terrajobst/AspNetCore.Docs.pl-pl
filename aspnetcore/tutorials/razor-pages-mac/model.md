---
title: "Dodawanie modelu do aplikacji stron Razor przy użyciu programu Visual Studio dla komputerów Mac"
author: rick-anderson
description: "Dodawanie modelu do aplikacji stron Razor w ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 792b98a79f193972cce56d3ad388b9fa9c58ac9c
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="2251f-103">Dodawanie modelu do aplikacji stron Razor w ASP.NET Core za pomocą programu Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="2251f-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="2251f-104">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="2251f-104">Add a data model</span></span>

* <span data-ttu-id="2251f-105">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="2251f-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="2251f-106">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="2251f-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="2251f-107">Kliknij prawym przyciskiem myszy *modele* folder, a następnie wybierz **Dodaj** > **nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="2251f-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="2251f-108">W **nowy plik** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="2251f-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="2251f-109">Wybierz **ogólne** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="2251f-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="2251f-110">Wybierz **pustą klasę** w słabe center.</span><span class="sxs-lookup"><span data-stu-id="2251f-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="2251f-111">Nazwa klasy **film** i wybierz **nowy**.</span><span class="sxs-lookup"><span data-stu-id="2251f-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="2251f-112">Kliknij prawym przyciskiem myszy red linii o dowolnym kształcie, na przykład `MovieContext` w wierszu `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="2251f-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="2251f-113">Wybierz **Quick Fix > przy użyciu RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="2251f-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="2251f-114">Dodaje programu Visual studio za pomocą instrukcji.</span><span class="sxs-lookup"><span data-stu-id="2251f-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="2251f-115">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="2251f-115">Build the project to verify you don't have any errors.</span></span>

![Tworzenie strony](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="2251f-117">Entity Framework Core NuGet pakietów dla migracji</span><span class="sxs-lookup"><span data-stu-id="2251f-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="2251f-118">Narzędzia EF dla interfejsu wiersza polecenia (CLI) są dostępne w [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="2251f-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="2251f-119">Polecenie [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) łącze, aby uzyskać numer wersji do użycia.</span><span class="sxs-lookup"><span data-stu-id="2251f-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="2251f-120">Aby zainstalować ten pakiet, należy dodać go do `DotNetCliToolReference` kolekcji w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="2251f-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="2251f-121">**Uwaga:** należy zainstalować ten pakiet, edytując *.csproj* pliku; nie można użyć `install-package` polecenia lub graficznego interfejsu użytkownika Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="2251f-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="2251f-122">Aby edytować *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="2251f-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="2251f-123">Wybierz **pliku** > **Otwórz**, a następnie wybierz *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="2251f-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="2251f-124">Wybierz **opcje**.</span><span class="sxs-lookup"><span data-stu-id="2251f-124">Select **Options**.</span></span>
* <span data-ttu-id="2251f-125">Zmień **Otwórz za pomocą** do **Edytor kodu źródłowego**.</span><span class="sxs-lookup"><span data-stu-id="2251f-125">Change **Open with** to **Source Code Editor**.</span></span>

![Edytuj plik csproj](model/csproj.png)

<span data-ttu-id="2251f-127">Dodaj `Microsoft.EntityFrameworkCore.Tools.DotNet` narzędzia odwołanie do drugiego  **\<ItemGroup >**.:</span><span class="sxs-lookup"><span data-stu-id="2251f-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="2251f-128">Numery wersji pokazano w poniższym kodzie były prawidłowe w czasie zapisywania.</span><span class="sxs-lookup"><span data-stu-id="2251f-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="2251f-129">Dodaj pliki stron/filmów do projektu</span><span class="sxs-lookup"><span data-stu-id="2251f-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="2251f-130">W programie Visual Studio, kliknij prawym przyciskiem myszy *stron* i wybierz polecenie **Dodaj > Dodaj istniejący Folder**.</span><span class="sxs-lookup"><span data-stu-id="2251f-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="2251f-131">Wybierz *filmy* folderu.</span><span class="sxs-lookup"><span data-stu-id="2251f-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="2251f-132">W *Chosse plików do uwzględnienia w projekcie* okno dialogowe, wybierz opcję **obejmują wszystkie**.</span><span class="sxs-lookup"><span data-stu-id="2251f-132">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="2251f-133">Następny samouczek wyjaśnia plików utworzonych przez funkcję szkieletów.</span><span class="sxs-lookup"><span data-stu-id="2251f-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2251f-134">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages-mac/razor-pages-start)
[dalej: szkieletu stron Razor](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="2251f-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
