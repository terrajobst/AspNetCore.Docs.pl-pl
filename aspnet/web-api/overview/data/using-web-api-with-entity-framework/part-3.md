---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Użyć migracje Code First na inicjowanie bazy danych | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 0d753ea52c57af2cbffff9e1e8741bbe49bc6d7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370529"
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="e432a-102">Użyć migracje Code First na inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="e432a-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="e432a-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e432a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e432a-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="e432a-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="e432a-105">W tej sekcji użyjesz [migracje Code First](https://msdn.microsoft.com/data/jj591621) w programie EF w celu umieszczenia w bazie danych testowych.</span><span class="sxs-lookup"><span data-stu-id="e432a-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="e432a-106">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="e432a-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e432a-107">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e432a-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="e432a-108">To polecenie dodaje folder o nazwie migracji do projektu, a także o nazwie Configuration.cs w folderze migracje pliku z kodem.</span><span class="sxs-lookup"><span data-stu-id="e432a-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="e432a-109">Otwórz plik Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="e432a-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="e432a-110">Dodaj następujący kod **przy użyciu** instrukcji.</span><span class="sxs-lookup"><span data-stu-id="e432a-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="e432a-111">Następnie dodaj następujący kod do **Configuration.Seed** metody:</span><span class="sxs-lookup"><span data-stu-id="e432a-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="e432a-112">W oknie Konsola Menedżera pakietów wpisz następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="e432a-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="e432a-113">Pierwsze polecenie generuje kod, który tworzy bazę danych, a drugie polecenie wykonuje kod.</span><span class="sxs-lookup"><span data-stu-id="e432a-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="e432a-114">Baza danych jest tworzona lokalnie, za pomocą [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="e432a-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="e432a-115">Poznaj interfejs API (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="e432a-115">Explore the API (Optional)</span></span>

<span data-ttu-id="e432a-116">Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="e432a-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="e432a-117">Visual Studio uruchamia usług IIS Express i uruchamia aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="e432a-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="e432a-118">Następnie, Visual Studio otworzy w przeglądarce i otwiera strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e432a-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="e432a-119">Po uruchomieniu projektu sieci web programu Visual Studio przypisuje numer portu.</span><span class="sxs-lookup"><span data-stu-id="e432a-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="e432a-120">Na poniższej ilustracji numer portu to 50524.</span><span class="sxs-lookup"><span data-stu-id="e432a-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="e432a-121">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="e432a-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="e432a-122">Strona główna jest implementowany przy użyciu platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e432a-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="e432a-123">W górnej części strony istnieje link, który jest wyświetlany komunikat "Interfejs API".</span><span class="sxs-lookup"><span data-stu-id="e432a-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="e432a-124">Ten link oferuje do strony pomocy generowane automatycznie dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="e432a-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="e432a-125">(Aby dowiedzieć się, jak jest generowany stronę pomocy i dodawania własnych dokumentacji do strony, zobacz [Tworzenie strony pomocy dla interfejsu API sieci Web platformy ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Możesz kliknąć pomoc łączy strony, aby wyświetlić szczegóły dotyczące interfejsu API, w tym formacie żądań i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e432a-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="e432a-126">Interfejs API umożliwia wykonywanie operacji CRUD na bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e432a-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="e432a-127">Poniżej przedstawiono podsumowanie interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="e432a-127">The following summarizes the API.</span></span>

| <span data-ttu-id="e432a-128">Autorzy</span><span class="sxs-lookup"><span data-stu-id="e432a-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="e432a-129">Pobierz interfejs api/autorów</span><span class="sxs-lookup"><span data-stu-id="e432a-129">GET api/authors</span></span> | <span data-ttu-id="e432a-130">Pobieranie wszystkich autorów.</span><span class="sxs-lookup"><span data-stu-id="e432a-130">Get all authors.</span></span> |
| <span data-ttu-id="e432a-131">Interfejs api GET/autorzy / {id}</span><span class="sxs-lookup"><span data-stu-id="e432a-131">GET api/authors/{id}</span></span> | <span data-ttu-id="e432a-132">Pobierz Autor według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="e432a-132">Get an author by ID.</span></span> |
| <span data-ttu-id="e432a-133">POST/api/autorów</span><span class="sxs-lookup"><span data-stu-id="e432a-133">POST /api/authors</span></span> | <span data-ttu-id="e432a-134">Utwórz nowy autora.</span><span class="sxs-lookup"><span data-stu-id="e432a-134">Create a new author.</span></span> |
| <span data-ttu-id="e432a-135">PUT/interfejs API/autorzy / {id}</span><span class="sxs-lookup"><span data-stu-id="e432a-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="e432a-136">Zaktualizuj istniejący autora.</span><span class="sxs-lookup"><span data-stu-id="e432a-136">Update an existing author.</span></span> |
| <span data-ttu-id="e432a-137">Usuń/interfejs API/autorzy / {id}</span><span class="sxs-lookup"><span data-stu-id="e432a-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="e432a-138">Usuń autora.</span><span class="sxs-lookup"><span data-stu-id="e432a-138">Delete an author.</span></span> |

| <span data-ttu-id="e432a-139">Książki</span><span class="sxs-lookup"><span data-stu-id="e432a-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="e432a-140">Pobierz /api/books</span><span class="sxs-lookup"><span data-stu-id="e432a-140">GET /api/books</span></span> | <span data-ttu-id="e432a-141">Pobierz wszystkie książki.</span><span class="sxs-lookup"><span data-stu-id="e432a-141">Get all books.</span></span> |
| <span data-ttu-id="e432a-142">Pobierz/interfejs API/books / {id}</span><span class="sxs-lookup"><span data-stu-id="e432a-142">GET /api/books/{id}</span></span> | <span data-ttu-id="e432a-143">Pobierz książkę według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="e432a-143">Get a book by ID.</span></span> |
| <span data-ttu-id="e432a-144">WPIS książki/api /</span><span class="sxs-lookup"><span data-stu-id="e432a-144">POST /api/books</span></span> | <span data-ttu-id="e432a-145">Tworzenie nowej książki.</span><span class="sxs-lookup"><span data-stu-id="e432a-145">Create a new book.</span></span> |
| <span data-ttu-id="e432a-146">PUT/interfejs API/books / {id}</span><span class="sxs-lookup"><span data-stu-id="e432a-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="e432a-147">Aktualizowanie istniejącej.</span><span class="sxs-lookup"><span data-stu-id="e432a-147">Update an existing book.</span></span> |
| <span data-ttu-id="e432a-148">Usuń/interfejs API/books / {id}</span><span class="sxs-lookup"><span data-stu-id="e432a-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="e432a-149">Usuń książki.</span><span class="sxs-lookup"><span data-stu-id="e432a-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="e432a-150">Widok bazy danych (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="e432a-150">View the Database (Optional)</span></span>

<span data-ttu-id="e432a-151">Po uruchomieniu polecenia Update-Database, EF, tworzenia bazy danych oraz o nazwie `Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="e432a-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="e432a-152">Po uruchomieniu aplikacji lokalnie EF używa [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="e432a-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="e432a-153">Bazy danych można wyświetlić w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e432a-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="e432a-154">Z **widoku** menu, wybierz opcję **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e432a-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="e432a-155">W **Połącz z serwerem** okna dialogowego w **nazwy serwera** pole edycji, wpisz "(localdb) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="e432a-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="e432a-156">Pozostaw **uwierzytelniania** opcji "Uwierzytelnianie Windows".</span><span class="sxs-lookup"><span data-stu-id="e432a-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="e432a-157">Kliknij przycisk **Połącz**.</span><span class="sxs-lookup"><span data-stu-id="e432a-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="e432a-158">Program Visual Studio połączy się LocalDB i pokazuje istniejących baz danych w oknie Eksplorator obiektów SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e432a-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="e432a-159">Można rozwinąć węzły, aby zobaczyć tabele utworzone EF.</span><span class="sxs-lookup"><span data-stu-id="e432a-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="e432a-160">Aby wyświetlić dane, kliknij prawym przyciskiem myszy tabelę i wybrać **dane widoku**.</span><span class="sxs-lookup"><span data-stu-id="e432a-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="e432a-161">Poniższy zrzut ekranu przedstawia wyniki dla tabeli książki.</span><span class="sxs-lookup"><span data-stu-id="e432a-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="e432a-162">Należy zauważyć, że EF wypełnione w bazie danych inicjatora, a tabela zawiera klucz obcy tabeli autorów.</span><span class="sxs-lookup"><span data-stu-id="e432a-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="e432a-163">[Poprzednie](part-2.md)
> [dalej](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="e432a-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
