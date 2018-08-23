---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Dodawanie modelu | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna w tym miejscu używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7e732da3b056980c87660f6b80366438b5823c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756551"
---
<a name="adding-a-model"></a><span data-ttu-id="a4f7f-104">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="a4f7f-104">Adding a Model</span></span>
====================
<span data-ttu-id="a4f7f-105">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a4f7f-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="a4f7f-106">Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="a4f7f-107">Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="a4f7f-108">W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="a4f7f-109">Te klasy będzie &quot;modelu&quot; wchodzi w skład aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="a4f7f-110">Użyjesz technologii dostępu do danych .NET Framework, znane jako [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) do definiowania i pracować z tych klas modelu.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="a4f7f-111">Obsługuje platformy Entity Framework (często określanymi jako EF) o nazwie paradygmat programowania *Code First*.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="a4f7f-112">Najpierw kod pozwala na tworzenie obiektów modelu, pisząc proste klas.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="a4f7f-113">(Te są również nazywane klasami POCO z &quot;obiektów CLR zwykły stary.&quot;) Można następnie skonfigurować bazę danych, tworzone na bieżąco z klas, co umożliwia przepływ pracy tworzenia bardzo szybkie i przejrzysty.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="a4f7f-114">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="a4f7f-114">Adding Model Classes</span></span>

<span data-ttu-id="a4f7f-115">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modeli* folderu, wybierz **Dodaj**, a następnie wybierz pozycję **klasy**.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="a4f7f-116">Wprowadź *klasy* nazwa &quot;filmu&quot;.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="a4f7f-117">Dodaj następujące właściwości pięć `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="a4f7f-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="a4f7f-118">Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="a4f7f-119">Każde wystąpienie `Movie` obiektu odnoszą się do wiersza w tabeli bazy danych, a każda właściwość `Movie` klasy będzie zmapowana do kolumny w tabeli.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="a4f7f-120">W tym samym pliku Dodaj następujący kod `MovieDBContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="a4f7f-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="a4f7f-121">`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="a4f7f-122">`MovieDBContext` Pochodzi od klasy `DbContext` dostarczane przez program Entity Framework klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="a4f7f-123">Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następujące `using` instrukcji w górnej części pliku:</span><span class="sxs-lookup"><span data-stu-id="a4f7f-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="a4f7f-124">Pełne *Movie.cs* plików znajdują się poniżej.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="a4f7f-125">(Kilka przy użyciu instrukcji, które nie są potrzebne zostały usunięte.)</span><span class="sxs-lookup"><span data-stu-id="a4f7f-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="a4f7f-126">Tworzenie parametrów połączenia i Praca z bazą danych LocalDB programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="a4f7f-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="a4f7f-127">`MovieDBContext` Utworzone klasy obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="a4f7f-128">Jedno pytanie, które możesz zadawać, jest jednak sposób określić bazę danych, która zostanie nawiązane połączenie.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="a4f7f-129">Należy to zrobić, dodając informacje o połączeniu w *Web.config* pliku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="a4f7f-130">Otwórz katalog główny aplikacji *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="a4f7f-131">(Nie *Web.config* w pliku *widoków* folderu.) Otwórz *Web.config* pliku wyróżnione kolorem czerwonym.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="a4f7f-132">Dodaj poniższe parametry połączenia do `<connectionStrings>` element *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="a4f7f-133">W poniższym przykładzie pokazano część *Web.config* pliku dodano nowy ciąg połączenia:</span><span class="sxs-lookup"><span data-stu-id="a4f7f-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="a4f7f-134">Ta niewielką ilość kodu i XML to wszystko, czego potrzebujesz do zapisu w celu reprezentowania i przechowywanie danych filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="a4f7f-135">Następnie utworzysz nowy `MoviesController` klasę, która służy do wyświetlania danych filmów i Zezwalaj użytkownikom na tworzenie nowych list filmu.</span><span class="sxs-lookup"><span data-stu-id="a4f7f-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a4f7f-136">[Poprzednie](adding-a-view.md)
> [dalej](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="a4f7f-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
