---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Umożliwia dodanie danych do bazy danych migracje Code First | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 33bc6d82daa9ca5f46452a1adf4e2eebea04fa6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869936"
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="5cd77-102">Umożliwia migracje Code First inicjatora bazy danych</span><span class="sxs-lookup"><span data-stu-id="5cd77-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="5cd77-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5cd77-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5cd77-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="5cd77-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="5cd77-105">W tej sekcji użyjesz [migracje Code First](https://msdn.microsoft.com/data/jj591621) w EF w celu umieszczenia bazy danych z danych testowych.</span><span class="sxs-lookup"><span data-stu-id="5cd77-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="5cd77-106">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="5cd77-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="5cd77-107">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5cd77-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="5cd77-108">To polecenie dodaje folder o nazwie migracji do projektu, a także kod plik o nazwie Configuration.cs w folderze migracji.</span><span class="sxs-lookup"><span data-stu-id="5cd77-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="5cd77-109">Otwórz plik Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="5cd77-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="5cd77-110">Dodaj następujące **przy użyciu** instrukcji.</span><span class="sxs-lookup"><span data-stu-id="5cd77-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="5cd77-111">Następnie dodaj następujący kod, aby **Configuration.Seed** metody:</span><span class="sxs-lookup"><span data-stu-id="5cd77-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="5cd77-112">W oknie Konsola Menedżera pakietów wpisz następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="5cd77-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="5cd77-113">Pierwsze polecenie generuje kod, który utworzy bazę danych, a drugie polecenie wykonuje kodu.</span><span class="sxs-lookup"><span data-stu-id="5cd77-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="5cd77-114">Bazy danych jest tworzony lokalnie, przy użyciu [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="5cd77-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="5cd77-115">Eksploruj interfejsu API (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="5cd77-115">Explore the API (Optional)</span></span>

<span data-ttu-id="5cd77-116">Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="5cd77-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="5cd77-117">Visual Studio uruchamiania usług IIS Express i uruchamia aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="5cd77-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="5cd77-118">Visual Studio następnie uruchamia przeglądarkę i spowoduje otwarcie strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5cd77-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="5cd77-119">Po uruchomieniu projektu sieci web programu Visual Studio przypisuje numeru portu.</span><span class="sxs-lookup"><span data-stu-id="5cd77-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="5cd77-120">Na poniższej ilustracji numer portu to 50524.</span><span class="sxs-lookup"><span data-stu-id="5cd77-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="5cd77-121">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="5cd77-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="5cd77-122">Strona główna jest implementowane za pomocą platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5cd77-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="5cd77-123">W górnej części strony Brak linku "Interfejsu API".</span><span class="sxs-lookup"><span data-stu-id="5cd77-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="5cd77-124">To łącze wprowadzono należy do strony pomocy generowane automatycznie dla interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="5cd77-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="5cd77-125">(Aby dowiedzieć się, jak jest generowany stronę pomocy i jak dodać własne dokumentacji do strony, zobacz [tworzenie pomocy stron ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Możesz kliknąć pomoc łączy strony, aby wyświetlić szczegółowe informacje o interfejsie API, w tym formacie żądań i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5cd77-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="5cd77-126">Interfejs API umożliwia operacje CRUD na bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5cd77-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="5cd77-127">Poniżej przedstawiono podsumowanie interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5cd77-127">The following summarizes the API.</span></span>

| <span data-ttu-id="5cd77-128">Autorzy</span><span class="sxs-lookup"><span data-stu-id="5cd77-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="5cd77-129">Pobierz interfejs api/autorów</span><span class="sxs-lookup"><span data-stu-id="5cd77-129">GET api/authors</span></span> | <span data-ttu-id="5cd77-130">Pobierz wszystkich autorów.</span><span class="sxs-lookup"><span data-stu-id="5cd77-130">Get all authors.</span></span> |
| <span data-ttu-id="5cd77-131">Interfejs api GET/autorów / {id}</span><span class="sxs-lookup"><span data-stu-id="5cd77-131">GET api/authors/{id}</span></span> | <span data-ttu-id="5cd77-132">Pobierz Autor według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="5cd77-132">Get an author by ID.</span></span> |
| <span data-ttu-id="5cd77-133">POST/api/autorów</span><span class="sxs-lookup"><span data-stu-id="5cd77-133">POST /api/authors</span></span> | <span data-ttu-id="5cd77-134">Utwórz nowy autora.</span><span class="sxs-lookup"><span data-stu-id="5cd77-134">Create a new author.</span></span> |
| <span data-ttu-id="5cd77-135">Umieść /api/autorów / {id}</span><span class="sxs-lookup"><span data-stu-id="5cd77-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="5cd77-136">Aktualizowanie istniejących autora.</span><span class="sxs-lookup"><span data-stu-id="5cd77-136">Update an existing author.</span></span> |
| <span data-ttu-id="5cd77-137">Usuń /api/autorów / {id}</span><span class="sxs-lookup"><span data-stu-id="5cd77-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="5cd77-138">Usuń autora.</span><span class="sxs-lookup"><span data-stu-id="5cd77-138">Delete an author.</span></span> |

| <span data-ttu-id="5cd77-139">Książki</span><span class="sxs-lookup"><span data-stu-id="5cd77-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="5cd77-140">Pobierz /api/books</span><span class="sxs-lookup"><span data-stu-id="5cd77-140">GET /api/books</span></span> | <span data-ttu-id="5cd77-141">Pobierz wszystkie źródłowe.</span><span class="sxs-lookup"><span data-stu-id="5cd77-141">Get all books.</span></span> |
| <span data-ttu-id="5cd77-142">Pobierz /api/books / {id}</span><span class="sxs-lookup"><span data-stu-id="5cd77-142">GET /api/books/{id}</span></span> | <span data-ttu-id="5cd77-143">Pobierz książkę według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="5cd77-143">Get a book by ID.</span></span> |
| <span data-ttu-id="5cd77-144">POST/api/książek</span><span class="sxs-lookup"><span data-stu-id="5cd77-144">POST /api/books</span></span> | <span data-ttu-id="5cd77-145">Tworzenie nowej książki.</span><span class="sxs-lookup"><span data-stu-id="5cd77-145">Create a new book.</span></span> |
| <span data-ttu-id="5cd77-146">Umieść /api/books / {id}</span><span class="sxs-lookup"><span data-stu-id="5cd77-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="5cd77-147">Aktualizowanie istniejącej.</span><span class="sxs-lookup"><span data-stu-id="5cd77-147">Update an existing book.</span></span> |
| <span data-ttu-id="5cd77-148">Usuń /api/books / {id}</span><span class="sxs-lookup"><span data-stu-id="5cd77-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="5cd77-149">Usuń książkę.</span><span class="sxs-lookup"><span data-stu-id="5cd77-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="5cd77-150">Widok bazy danych (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="5cd77-150">View the Database (Optional)</span></span>

<span data-ttu-id="5cd77-151">Po uruchomieniu polecenia Update-Database EF baza danych utworzona i wywołać `Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="5cd77-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="5cd77-152">Po uruchomieniu aplikacji lokalnie, używa EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="5cd77-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="5cd77-153">Bazy danych można wyświetlić w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5cd77-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="5cd77-154">Z **widoku** menu, wybierz opcję **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="5cd77-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="5cd77-155">W **Połącz z serwerem** okna dialogowego, w **nazwy serwera** pole edycji, wpisz "(localdb) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="5cd77-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="5cd77-156">Pozostaw **uwierzytelniania** opcję "Uwierzytelnianie systemu Windows".</span><span class="sxs-lookup"><span data-stu-id="5cd77-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="5cd77-157">Kliknij przycisk **Połącz**.</span><span class="sxs-lookup"><span data-stu-id="5cd77-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="5cd77-158">Visual Studio łączy do LocalDB i przedstawia istniejących baz danych w oknie Eksplorator obiektów SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5cd77-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="5cd77-159">Można rozwinąć węzły, aby zobaczyć tabele, które EF utworzone.</span><span class="sxs-lookup"><span data-stu-id="5cd77-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="5cd77-160">Aby wyświetlić dane, kliknij prawym przyciskiem myszy tabelę i wybierz **danych widoku**.</span><span class="sxs-lookup"><span data-stu-id="5cd77-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="5cd77-161">Poniższy zrzut ekranu przedstawia wyniki dla tabeli książek.</span><span class="sxs-lookup"><span data-stu-id="5cd77-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="5cd77-162">Zwróć uwagę, EF wypełnione w bazie danych inicjatora, czy tabela zawiera klucz obcy tabeli autorów.</span><span class="sxs-lookup"><span data-stu-id="5cd77-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="5cd77-163">[Poprzednie](part-2.md)
> [dalej](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="5cd77-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
