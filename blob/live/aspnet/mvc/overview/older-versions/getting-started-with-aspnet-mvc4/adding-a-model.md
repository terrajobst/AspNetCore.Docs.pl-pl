---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Dodawanie modelu | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 1d066e4bab866a2195647f43aa886279fee941db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model"></a><span data-ttu-id="83c51-104">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="83c51-104">Adding a Model</span></span>
====================
<span data-ttu-id="83c51-105">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="83c51-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="83c51-106">Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="83c51-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="83c51-107">Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="83c51-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="83c51-108">W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="83c51-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="83c51-109">Te klasy będzie &quot;modelu&quot; częścią aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="83c51-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="83c51-110">Użyjesz technologii dostępu do danych .NET Framework, znany jako [Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx) do definiowania i pracy z tych klas modelu.</span><span class="sxs-lookup"><span data-stu-id="83c51-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="83c51-111">Obsługuje programu Entity Framework (nazywanej często EF) o nazwie modelu programowania *Code First*.</span><span class="sxs-lookup"><span data-stu-id="83c51-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="83c51-112">Kod umożliwia najpierw utworzyć obiekty modelu pisząc proste klasy.</span><span class="sxs-lookup"><span data-stu-id="83c51-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="83c51-113">(Te są nazywane także klasy POCO z &quot;obiektów CLR starego zwykłego.&quot;) Następnie możesz wybrać bazy danych na bieżąco z klas, dzięki czemu bardzo czyste i szybkie programowanie przepływu pracy.</span><span class="sxs-lookup"><span data-stu-id="83c51-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="83c51-114">Dodawanie klas modelu</span><span class="sxs-lookup"><span data-stu-id="83c51-114">Adding Model Classes</span></span>

<span data-ttu-id="83c51-115">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modele* folderu, wybierz opcję **Dodaj**, a następnie wybierz **klasy**.</span><span class="sxs-lookup"><span data-stu-id="83c51-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="83c51-116">Wprowadź *klasy* nazwa &quot;film&quot;.</span><span class="sxs-lookup"><span data-stu-id="83c51-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="83c51-117">Dodaj następujące właściwości pięć `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="83c51-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="83c51-118">Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="83c51-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="83c51-119">Każde wystąpienie `Movie` obiektu odpowiada wiersz w tabeli bazy danych, a każda właściwość `Movie` klasy przypisze do kolumny w tabeli.</span><span class="sxs-lookup"><span data-stu-id="83c51-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="83c51-120">W tym samym pliku Dodaj następujące `MovieDBContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="83c51-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="83c51-121">`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="83c51-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="83c51-122">`MovieDBContext` Pochodną `DbContext` pochodzącymi z programu Entity Framework w klasie podstawowej.</span><span class="sxs-lookup"><span data-stu-id="83c51-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="83c51-123">Aby można było odwołać `DbContext` i `DbSet`, należy dodać następujące `using` instrukcji w górnej części pliku:</span><span class="sxs-lookup"><span data-stu-id="83c51-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="83c51-124">Pełną *Movie.cs* plików są wyświetlane poniżej.</span><span class="sxs-lookup"><span data-stu-id="83c51-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="83c51-125">(Kilka przy użyciu instrukcji, które nie są potrzebne zostały usunięte.)</span><span class="sxs-lookup"><span data-stu-id="83c51-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="83c51-126">Tworzenie parametrów połączenia i pracy z bazy danych LocalDB programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="83c51-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="83c51-127">`MovieDBContext` Obsługuje klasy utworzone zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="83c51-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="83c51-128">Jedno pytanie, który może poprosić o jest jednak sposób Określ bazę danych, które będą łączyć się.</span><span class="sxs-lookup"><span data-stu-id="83c51-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="83c51-129">Należy to zrobić, dodając informacje o połączeniu w *Web.config* pliku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="83c51-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="83c51-130">Otwórz katalog główny aplikacji *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="83c51-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="83c51-131">(Nie *Web.config* w pliku *widoków* folderu.) Otwórz *Web.config* pliku wyróżnione kolorem czerwonym.</span><span class="sxs-lookup"><span data-stu-id="83c51-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="83c51-132">Dodaj następujące parametry połączenia do `<connectionStrings>` element *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="83c51-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="83c51-133">W poniższym przykładzie przedstawiono część *Web.config* plik o nowe parametry połączenia dodany:</span><span class="sxs-lookup"><span data-stu-id="83c51-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="83c51-134">Mała ilość kodu i XML to wszystko, co potrzebne do zapisu w celu reprezentowania i przechowywania danych filmu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="83c51-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="83c51-135">Następnie będzie utworzyć nowy `MoviesController` klasy, która służy do wyświetlania danych filmu i Zezwalaj użytkownikom na tworzenie nowych list filmu.</span><span class="sxs-lookup"><span data-stu-id="83c51-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="83c51-136">[Poprzednie](adding-a-view.md)
[dalej](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="83c51-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
