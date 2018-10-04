---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Tworzenie parametrów połączenia i Praca z bazą danych LocalDB programu SQL Server | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 746101344832793b199d2b3f3dfcfcd4e3b9a8da
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578526"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="80464-102">Tworzenie parametrów połączenia i Praca z bazą danych LocalDB programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="80464-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="80464-103">Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="80464-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="80464-104">Tworzenie parametrów połączenia i Praca z bazą danych LocalDB programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="80464-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="80464-105">`MovieDBContext` Utworzone klasy obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="80464-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="80464-106">Jedno pytanie, które możesz zadawać, jest jednak sposób określić bazę danych, która zostanie nawiązane połączenie.</span><span class="sxs-lookup"><span data-stu-id="80464-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="80464-107">Faktycznie nie trzeba określać bazę danych, która do użycia, będą domyślnie przy użyciu platformy Entity Framework [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="80464-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="80464-108">W tej sekcji dodasz jawnie parametrów połączenia w *Web.config* pliku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="80464-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="80464-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="80464-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="80464-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) to Uproszczona wersja aparatu programu SQL Server Express bazy danych rozpoczyna się na żądanie, która działa w trybie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="80464-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="80464-111">LocalDB działa w specjalnego trybu wykonania programu SQL Server Express, która umożliwia pracę z bazami danych jako *.mdf* plików.</span><span class="sxs-lookup"><span data-stu-id="80464-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="80464-112">Zazwyczaj pliki bazy danych LocalDB są przechowywane w *aplikacji\_danych* folderu projektu sieci web.</span><span class="sxs-lookup"><span data-stu-id="80464-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="80464-113">SQL Server Express nie jest zalecane do użytku w aplikacjach sieci web w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="80464-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="80464-114">LocalDB w szczególności nie należy używać w środowisku produkcyjnym z aplikacją sieci web ponieważ nie jest przeznaczony do pracy z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="80464-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="80464-115">Jednak bazy danych LocalDB mogą zostać łatwo zmigrowane do programu SQL Server lub SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="80464-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="80464-116">W programie Visual Studio 2017 LocalDB jest instalowany domyślnie z programem Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="80464-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="80464-117">Domyślnie platforma Entity Framework szuka parametrów połączenia o nazwie taka sama jak klasa kontekstu obiektu (`MovieDBContext` dla tego projektu).</span><span class="sxs-lookup"><span data-stu-id="80464-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="80464-118">Aby uzyskać więcej informacji, zobacz [parametry połączenia serwera SQL dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="80464-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="80464-119">Otwórz katalog główny aplikacji *Web.config* pliku pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="80464-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="80464-120">(Nie *Web.config* w pliku *widoków* folderu.)</span><span class="sxs-lookup"><span data-stu-id="80464-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="80464-121">Znajdź `<connectionStrings>` elementu:</span><span class="sxs-lookup"><span data-stu-id="80464-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="80464-122">Dodaj poniższe parametry połączenia do `<connectionStrings>` element *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="80464-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="80464-123">W poniższym przykładzie pokazano część *Web.config* pliku dodano nowy ciąg połączenia:</span><span class="sxs-lookup"><span data-stu-id="80464-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="80464-124">Dwa parametry połączenia są bardzo podobne.</span><span class="sxs-lookup"><span data-stu-id="80464-124">The two connection strings are very similar.</span></span> <span data-ttu-id="80464-125">Pierwszy ciąg połączenia o nazwie `DefaultConnection` i jest używana dla bazy danych członkostwa w celu kontrolowania, kto może uzyskiwać dostęp do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="80464-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="80464-126">Parametry połączenia zostały dodane Określa nazwę bazy danych LocalDB *Movie.mdf* na terenie *aplikacji\_danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="80464-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="80464-127">Firma Microsoft nie będzie używać bazy danych członkostwa w ramach tego samouczka, aby uzyskać więcej informacji na temat członkostwa, uwierzytelniania i zabezpieczeń, zobacz Moje samouczek [tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL i wdrażanie w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="80464-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="80464-128">Nazwa ciągu połączenia musi odpowiadać nazwa [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) klasy.</span><span class="sxs-lookup"><span data-stu-id="80464-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="80464-129">Nie jest potrzebna do dodania `MovieDBContext` parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="80464-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="80464-130">Jeśli nie określisz ciąg połączenia programu Entity Framework spowoduje utworzenie bazy danych LocalDB w katalogu użytkowników z w pełni kwalifikowana nazwa [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) klasy (w tym przypadku `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="80464-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="80464-131">Nazwać w bazie danych dowolny lubisz, tak długo, jak przedstawiono w nim *. MDF* sufiks.</span><span class="sxs-lookup"><span data-stu-id="80464-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="80464-132">Na przykład, użyjemy nazwy bazy danych *MyFilms.mdf*.</span><span class="sxs-lookup"><span data-stu-id="80464-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="80464-133">Następnie utworzysz nowy `MoviesController` klasę, która służy do wyświetlania danych filmów i Zezwalaj użytkownikom na tworzenie nowych list filmu.</span><span class="sxs-lookup"><span data-stu-id="80464-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="80464-134">[Poprzednie](adding-a-model.md)
> [dalej](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="80464-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
