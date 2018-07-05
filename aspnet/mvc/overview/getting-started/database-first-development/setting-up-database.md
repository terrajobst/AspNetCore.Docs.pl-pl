---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Wprowadzenie do First w programie Entity Framework 6 bazy danych za pomocą MVC 5 | Dokumentacja firmy Microsoft
author: tfitzmac
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 98deeb91dc2b9a1bad535be1bf1e2ec85dfe4028
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371716"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="05d38-104">Wprowadzenie do First w programie Entity Framework 6 bazy danych za pomocą MVC 5</span><span class="sxs-lookup"><span data-stu-id="05d38-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="05d38-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="05d38-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="05d38-106">Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="05d38-107">W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="05d38-108">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="05d38-109">W ostatniej części serii zostanie wdrożona lokacji i bazy danych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="05d38-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="05d38-110">Ta część serii koncentruje się na utworzenie bazy danych i zapełnianie danymi.</span><span class="sxs-lookup"><span data-stu-id="05d38-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="05d38-111">W tej serii został napisany z uwzględnieniem materiałów przekazanych z Tom Dykstra i Ricka Andersona.</span><span class="sxs-lookup"><span data-stu-id="05d38-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="05d38-112">Został udoskonalony na podstawie informacji pochodzących od użytkowników w sekcji komentarzy.</span><span class="sxs-lookup"><span data-stu-id="05d38-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="05d38-113">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="05d38-113">Introduction</span></span>

<span data-ttu-id="05d38-114">W tym temacie pokazano, jak rozpocząć od istniejącej bazy danych i szybkie tworzenie aplikacji sieci web, która umożliwia użytkownikom na interakcję z danymi.</span><span class="sxs-lookup"><span data-stu-id="05d38-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="05d38-115">Aby utworzyć aplikację sieci web używa platformy Entity Framework 6 i MVC 5.</span><span class="sxs-lookup"><span data-stu-id="05d38-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="05d38-116">Funkcja tworzenia szkieletu ASP.NET umożliwia automatyczne generowanie kodu na wyświetlanie, aktualizowanie, tworzenie i usuwanie danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="05d38-117">Za pomocą narzędzia publikowania w programie Visual Studio, można łatwo wdrożysz lokacji i bazy danych na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="05d38-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="05d38-118">Ten temat dotyczy sytuacji, w którym masz bazę danych i chcesz wygenerować kod dla aplikacji sieci web na podstawie pól tej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="05d38-119">To podejście jest nazywane tworzenie pierwszej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-119">This approach is called Database First development.</span></span> <span data-ttu-id="05d38-120">Jeśli nie masz już istniejącą bazę danych, możesz zamiast tego użyć podejścia o nazwie rozwiązania deweloperskiego Code First, który obejmuje Definiowanie klas danych i generowanie bazy danych przy użyciu właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="05d38-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="05d38-121">Wprowadzający przykład rozwiązania deweloperskiego Code First, zobacz [wprowadzenie do ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="05d38-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="05d38-122">Na przykład bardziej zaawansowanych, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji platformy ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="05d38-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="05d38-123">Aby uzyskać wskazówki dotyczące wybierania rozwiązaniem platformy Entity Framework, zobacz [podejścia do projektowania struktury jednostki](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="05d38-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05d38-124">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="05d38-124">Prerequisites</span></span>

<span data-ttu-id="05d38-125">Visual Studio 2013 lub Visual Studio Express 2013 for Web</span><span class="sxs-lookup"><span data-stu-id="05d38-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="05d38-126">Konfigurowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="05d38-126">Set up the database</span></span>

<span data-ttu-id="05d38-127">Aby mógł naśladować środowisko o istniejącą bazę danych, zostanie najpierw utwórz bazę danych z pewnymi wstępnie wypełniony danymi, a następnie utwórz aplikację sieci web, który nawiązuje połączenie z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="05d38-128">W tym samouczku został opracowany, instalacja programu LocalDB za pomocą programu Visual Studio 2013 lub Visual Studio Express 2013 for Web.</span><span class="sxs-lookup"><span data-stu-id="05d38-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="05d38-129">Można użyć istniejącego serwera baz danych, zamiast programu LocalDB, ale w zależności od używanej wersji programu Visual Studio i typ bazy danych, wszystkie narzędzia danych w programie Visual Studio może nie być obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="05d38-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="05d38-130">Jeśli narzędzia nie są dostępne dla bazy danych, może być konieczne do wykonania niektórych kroków określonej bazy danych w pakiecie zarządzania z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="05d38-131">Jeśli masz problem z narzędzia graficzne bazy danych w wersji programu Visual Studio, upewnij się, że zainstalowano najnowszą wersję narzędzia graficzne bazy danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="05d38-132">Aby dowiedzieć się, czy instalujesz narzędzia graficzne bazy danych, zobacz [programu Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span><span class="sxs-lookup"><span data-stu-id="05d38-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="05d38-133">Uruchom program Visual Studio i Utwórz **projekt bazy danych programu SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="05d38-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="05d38-134">Nadaj projektowi nazwę **ContosoUniversityData**.</span><span class="sxs-lookup"><span data-stu-id="05d38-134">Name the project **ContosoUniversityData**.</span></span>

![Utwórz projekt bazy danych](setting-up-database/_static/image1.png)

<span data-ttu-id="05d38-136">Masz teraz projekt pustej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-136">You now have an empty database project.</span></span> <span data-ttu-id="05d38-137">Ta baza danych zostanie wdrożona na platformie Azure w dalszej części tego samouczka, należy ustawić usługi Azure SQL Database jako platformę docelową dla projektu.</span><span class="sxs-lookup"><span data-stu-id="05d38-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="05d38-138">Ustawienie platforma docelowa nie jest faktycznie wdrażana bazy danych. oznacza jedynie, że projekt bazy danych sprawdzi, czy projekt bazy danych jest zgodna z platformą docelową.</span><span class="sxs-lookup"><span data-stu-id="05d38-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="05d38-139">Aby ustawić platformę docelową, otwórz **właściwości** dla projektu i wybierz **Microsoft Azure SQL Database** dla platformy docelowej.</span><span class="sxs-lookup"><span data-stu-id="05d38-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![Platforma docelowa zestawu](setting-up-database/_static/image2.png)

<span data-ttu-id="05d38-141">Możesz utworzyć tabele wymagane na potrzeby tego samouczka, dodając skrypty SQL, które tabel.</span><span class="sxs-lookup"><span data-stu-id="05d38-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="05d38-142">Kliknij prawym przyciskiem myszy projekt i Dodaj nowy element.</span><span class="sxs-lookup"><span data-stu-id="05d38-142">Right-click your project and add a new item.</span></span>

![Dodaj nowy element](setting-up-database/_static/image3.png)

<span data-ttu-id="05d38-144">Dodaj nową tabelę o nazwie Student.</span><span class="sxs-lookup"><span data-stu-id="05d38-144">Add a new table named Student.</span></span>

![Dodaj tabelę dla uczniów](setting-up-database/_static/image4.png)

<span data-ttu-id="05d38-146">W pliku tabeli Zastąp następujący kod, aby utworzyć tabelę polecenia języka T-SQL.</span><span class="sxs-lookup"><span data-stu-id="05d38-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="05d38-147">Należy zauważyć, że okna projektu automatycznie synchronizuje się z kodem.</span><span class="sxs-lookup"><span data-stu-id="05d38-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="05d38-148">Możesz pracować z kodem lub projektanta.</span><span class="sxs-lookup"><span data-stu-id="05d38-148">You can work with either the code or designer.</span></span>

![Pokaż kod i projekt](setting-up-database/_static/image5.png)

<span data-ttu-id="05d38-150">Dodaj inną tabelę.</span><span class="sxs-lookup"><span data-stu-id="05d38-150">Add another table.</span></span> <span data-ttu-id="05d38-151">Ten czas, nadaj jej nazwę na kursie i użyj następującego polecenia języka T-SQL.</span><span class="sxs-lookup"><span data-stu-id="05d38-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="05d38-152">I powtórz jeszcze raz, aby utworzyć tabelę o nazwie rejestracji.</span><span class="sxs-lookup"><span data-stu-id="05d38-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="05d38-153">Możesz wypełnić bazę danych danymi za pomocą skryptu, który jest uruchamiany po wdrożeniu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="05d38-154">Skrypt po wdrożeniu należy dodać do projektu.</span><span class="sxs-lookup"><span data-stu-id="05d38-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="05d38-155">Można użyć domyślnej nazwy.</span><span class="sxs-lookup"><span data-stu-id="05d38-155">You can use the default name.</span></span>

![Dodaj skrypt po wdrożeniu](setting-up-database/_static/image6.png)

<span data-ttu-id="05d38-157">Dodaj poniższy kod T-SQL do skryptu po wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="05d38-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="05d38-158">Ten skrypt po prostu dodaje dane do bazy danych po znalezieniu odpowiedniego rekordu.</span><span class="sxs-lookup"><span data-stu-id="05d38-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="05d38-159">Nie zastąpić lub Usuń wszystkie dane, który został wprowadzony w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="05d38-160">Należy zauważyć, że skrypt po wdrożeniu jest uruchamiany za każdym razem, aby wdrożyć projekt bazy danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="05d38-161">W związku z tym należy starannie wziąć pod uwagę wymogi podczas pisania tego skryptu.</span><span class="sxs-lookup"><span data-stu-id="05d38-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="05d38-162">W niektórych przypadkach warto zacząć od nowa z znanego zestawu danych, za każdym razem, gdy projekt został wdrożony.</span><span class="sxs-lookup"><span data-stu-id="05d38-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="05d38-163">W innych przypadkach może nie chcieć zmienić istniejące dane w dowolny sposób.</span><span class="sxs-lookup"><span data-stu-id="05d38-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="05d38-164">W zależności od wymagań, możesz zdecydować, czy potrzebujesz skrypt po wdrożeniu lub potrzebnych do uwzględnienia w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="05d38-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="05d38-165">Aby uzyskać więcej informacji na temat Wypełnianie bazy danych za pomocą skryptu po wdrożeniu, zobacz [łącznie z danymi w projekt bazy danych serwera SQL](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span><span class="sxs-lookup"><span data-stu-id="05d38-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="05d38-166">Masz teraz 4 pliki skryptów SQL, ale nie rzeczywiste tabele.</span><span class="sxs-lookup"><span data-stu-id="05d38-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="05d38-167">Jesteś gotowy wdrożyć projekt bazy danych localdb.</span><span class="sxs-lookup"><span data-stu-id="05d38-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="05d38-168">W programie Visual Studio kliknij przycisk Start (lub F5) do tworzenia i wdrażania projektu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="05d38-169">Sprawdź kartę danych wyjściowych, aby sprawdzić, czy kompilacja i wdrażanie zakończyło się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="05d38-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![Pokaż dane wyjściowe](setting-up-database/_static/image7.png)

<span data-ttu-id="05d38-171">Aby sprawdzić, czy nowa baza danych została utworzona, otwórz **Eksplorator obiektów SQL Server** i wyszukaj nazwę projektu na serwerze poprawne lokalnej bazy danych (w tym przypadku **\ProjectsV12 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="05d38-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![Pokaż nowej bazy danych](setting-up-database/_static/image8.png)

<span data-ttu-id="05d38-173">Aby zobaczyć, że tabele są wypełniane przy użyciu danych, kliknij prawym przyciskiem myszy tabelę, a następnie wybierz pozycję **dane widoku**.</span><span class="sxs-lookup"><span data-stu-id="05d38-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![Pokaż dane tabeli](setting-up-database/_static/image9.png)

<span data-ttu-id="05d38-175">Wyświetlany jest widok edycji danych tabeli.</span><span class="sxs-lookup"><span data-stu-id="05d38-175">An editable view of the table data is displayed.</span></span>

![Pokaż wyniki danych tabeli](setting-up-database/_static/image10.png)

<span data-ttu-id="05d38-177">Baza danych jest teraz i wypełniony danymi.</span><span class="sxs-lookup"><span data-stu-id="05d38-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="05d38-178">W następnym samouczku utworzysz aplikację sieci web, dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="05d38-178">In the next tutorial, you will create a web application for the database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="05d38-179">Next</span><span class="sxs-lookup"><span data-stu-id="05d38-179">Next</span></span>](creating-the-web-application.md)
