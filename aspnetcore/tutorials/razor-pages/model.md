---
title: Dodawanie do aplikacji stron Razor w ASP.NET Core modelu
author: rick-anderson
description: Dodawanie do aplikacji stron Razor w ASP.NET Core modelu
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 234af466979c8d8b14990da4d4a07e6e54649bbe
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="d7677-103">Dodawanie modelu do aplikacji stron Razor</span><span class="sxs-lookup"><span data-stu-id="d7677-103">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="d7677-104">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="d7677-104">Add a data model</span></span>

<span data-ttu-id="d7677-105">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **RazorPagesMovie** projektu > **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="d7677-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="d7677-106">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="d7677-106">Name the folder *Models*.</span></span>

<span data-ttu-id="d7677-107">Kliknij prawym przyciskiem myszy *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="d7677-107">Right click the *Models* folder.</span></span> <span data-ttu-id="d7677-108">Wybierz **dodać** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="d7677-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="d7677-109">Nazwa klasy **film** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="d7677-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="d7677-110">Dodaj parametry połączenia bazy danych</span><span class="sxs-lookup"><span data-stu-id="d7677-110">Add a database connection string</span></span>

<span data-ttu-id="d7677-111">Dodaj parametry połączenia do *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="d7677-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="d7677-112">Zarejestruj kontekst bazy danych</span><span class="sxs-lookup"><span data-stu-id="d7677-112">Register the database context</span></span>

<span data-ttu-id="d7677-113">Zarejestruj kontekst bazy danych z [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="d7677-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="d7677-114">Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów.</span><span class="sxs-lookup"><span data-stu-id="d7677-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="d7677-115">Dodawanie szkieletu narzędzi i przeprowadzić migrację początkowej</span><span class="sxs-lookup"><span data-stu-id="d7677-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="d7677-116">W tej sekcji można użyć do konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="d7677-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="d7677-117">Dodaj pakiet generowania kodu sieci web programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d7677-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="d7677-118">Ten pakiet jest wymagana do uruchamiania aparatu szkieletów.</span><span class="sxs-lookup"><span data-stu-id="d7677-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="d7677-119">Dodaj początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="d7677-119">Add an initial migration.</span></span>
* <span data-ttu-id="d7677-120">Aktualizacji bazy danych z początkowej migracji.</span><span class="sxs-lookup"><span data-stu-id="d7677-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="d7677-121">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="d7677-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC menu](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="d7677-123">W kryterium wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="d7677-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

<span data-ttu-id="d7677-124">`Install-Package` Polecenie powoduje zainstalowanie narzędzi wymagane do uruchamiania aparatu szkieletów.</span><span class="sxs-lookup"><span data-stu-id="d7677-124">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="d7677-125">`Add-Migration` Polecenie generuje kod w celu utworzenia schematu początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d7677-125">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="d7677-126">Schemat jest oparta na modelu określone w `DbContext` (w *Models/MovieContext.cs* pliku).</span><span class="sxs-lookup"><span data-stu-id="d7677-126">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="d7677-127">`Initial` Argument jest używany do nazywania migracji.</span><span class="sxs-lookup"><span data-stu-id="d7677-127">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="d7677-128">Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji.</span><span class="sxs-lookup"><span data-stu-id="d7677-128">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="d7677-129">Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="d7677-129">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="d7677-130">`Update-Database` Polecenia `Up` metody w *migracje /\<sygnatury czasowej > _InitialCreate.cs* pliku, który utworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d7677-130">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="d7677-131">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d7677-131">Test the app</span></span>

* <span data-ttu-id="d7677-132">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="d7677-132">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="d7677-133">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="d7677-133">Test the **Create** link.</span></span>

 ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="d7677-135">Test **Edytuj**, **szczegóły**, i **usunąć** łącza.</span><span class="sxs-lookup"><span data-stu-id="d7677-135">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="d7677-136">Jeśli otrzymasz wyjątek SQL, sprawdź zostało uruchomione migracji i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="d7677-136">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="d7677-137">Następny samouczek wyjaśnia plików utworzonych przez funkcję szkieletów.</span><span class="sxs-lookup"><span data-stu-id="d7677-137">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d7677-138">[Poprzedni: Wprowadzenie](xref:tutorials/razor-pages/razor-pages-start)
[dalej: szkieletu stron Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="d7677-138">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
