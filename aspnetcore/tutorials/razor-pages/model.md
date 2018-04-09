---
title: Dodawanie do aplikacji stron Razor w ASP.NET Core modelu
author: rick-anderson
description: Dowiedzieć się, jak można dodać klasy do zrządzania filmów w bazie danych przy użyciu Entity Framework Core (EF Core).
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 8542f4f3e516a62c19308d0c03e1ba4b155ed1ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="6c370-103">Dodawanie do aplikacji stron Razor w ASP.NET Core modelu</span><span class="sxs-lookup"><span data-stu-id="6c370-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="6c370-104">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="6c370-104">Add a data model</span></span>

<span data-ttu-id="6c370-105">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="6c370-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="6c370-106">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="6c370-106">Name the folder *Models*.</span></span>

<span data-ttu-id="6c370-107">Kliknij prawym przyciskiem myszy *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="6c370-107">Right click the *Models* folder.</span></span> <span data-ttu-id="6c370-108">Wybierz **dodać** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="6c370-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="6c370-109">Nazwa klasy **film** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="6c370-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="6c370-110">Dodaj parametry połączenia bazy danych</span><span class="sxs-lookup"><span data-stu-id="6c370-110">Add a database connection string</span></span>

<span data-ttu-id="6c370-111">Dodaj parametry połączenia do *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="6c370-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="6c370-112">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="6c370-112">Register the database context</span></span>

<span data-ttu-id="6c370-113">Zarejestruj kontekst bazy danych z [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="6c370-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="6c370-114">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="6c370-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="6c370-115">Dodawanie szkieletu narzędzi i przeprowadzić migrację początkowej</span><span class="sxs-lookup"><span data-stu-id="6c370-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="6c370-116">W tej sekcji można użyć do konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="6c370-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="6c370-117">Dodaj pakiet generowania kodu sieci web programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6c370-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="6c370-118">Ten pakiet jest wymagana do uruchamiania aparatu szkieletów.</span><span class="sxs-lookup"><span data-stu-id="6c370-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="6c370-119">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="6c370-119">Add an initial migration.</span></span>
* <span data-ttu-id="6c370-120">Aktualizacji bazy danych z początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="6c370-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="6c370-121">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="6c370-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menu](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="6c370-123">W kryterium wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="6c370-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

<span data-ttu-id="6c370-124">Alternatywnie można użyć następujących poleceń .NET Core interfejsu wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="6c370-124">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="6c370-125">`Install-Package` Polecenie powoduje zainstalowanie narzędzi wymagane do uruchamiania aparatu szkieletów.</span><span class="sxs-lookup"><span data-stu-id="6c370-125">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="6c370-126">`Add-Migration` Polecenie generuje kod w celu utworzenia schematu początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6c370-126">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="6c370-127">Schemat jest oparta na modelu określone w `DbContext` (w *Models/MovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="6c370-127">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="6c370-128">`Initial` Argument jest używany do nazywania migracji.</span><span class="sxs-lookup"><span data-stu-id="6c370-128">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="6c370-129">Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="6c370-129">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="6c370-130">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="6c370-130">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="6c370-131">`Update-Database` Polecenia `Up` metody w *migracje /\<sygnatury czasowej > _InitialCreate.cs* pliku, który utworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="6c370-131">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE [model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="6c370-132">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="6c370-132">Test the app</span></span>

* <span data-ttu-id="6c370-133">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="6c370-133">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="6c370-134">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="6c370-134">Test the **Create** link.</span></span>

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="6c370-136">Test **Edytuj**, **szczegóły**, i **usunąć** łącza.</span><span class="sxs-lookup"><span data-stu-id="6c370-136">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="6c370-137">Jeśli otrzymasz wyjątek SQL, sprawdź zostało uruchomione migracji i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="6c370-137">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="6c370-138">Następny samouczek wyjaśnia plików utworzonych przez funkcję szkieletów.</span><span class="sxs-lookup"><span data-stu-id="6c370-138">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6c370-139">[Poprzedni: Rozpoczynanie pracy](xref:tutorials/razor-pages/razor-pages-start)
> [dalej: szkieletu stron Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="6c370-139">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
