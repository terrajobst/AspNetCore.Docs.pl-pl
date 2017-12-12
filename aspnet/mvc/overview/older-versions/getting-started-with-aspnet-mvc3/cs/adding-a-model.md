---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Dodawanie modelu (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 32f2fcdae9d92b84dcd9be8746e416cdf9a14c48
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-c"></a><span data-ttu-id="0153d-104">Dodawanie modelu (C#)</span><span class="sxs-lookup"><span data-stu-id="0153d-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="0153d-105">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0153d-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="0153d-106">Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0153d-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="0153d-107">Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="0153d-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="0153d-108">Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="0153d-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="0153d-109">Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:</span><span class="sxs-lookup"><span data-stu-id="0153d-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="0153d-110">Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="0153d-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="0153d-111">Aktualizacji narzędzi programu ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="0153d-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="0153d-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)</span><span class="sxs-lookup"><span data-stu-id="0153d-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="0153d-113">Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="0153d-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="0153d-114">Projekt Visual Web Developer z kodu źródłowego C# jest dostępna powiązany z tym tematem.</span><span class="sxs-lookup"><span data-stu-id="0153d-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="0153d-115">[Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="0153d-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="0153d-116">Jeśli wolisz Visual Basic, przełącz się do [wersji Visual Basic](../vb/adding-a-model.md) tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="0153d-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="0153d-117">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="0153d-117">Adding a Model</span></span>

<span data-ttu-id="0153d-118">W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0153d-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="0153d-119">Te klasy będzie "modelu" części aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0153d-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="0153d-120">Użyjesz technologii dostępu do danych .NET Framework, znany jako Entity Framework do definiowania i pracy z tych klas modelu.</span><span class="sxs-lookup"><span data-stu-id="0153d-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="0153d-121">Obsługuje programu Entity Framework (nazywanej często EF) o nazwie modelu programowania *Code First*.</span><span class="sxs-lookup"><span data-stu-id="0153d-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="0153d-122">Kod umożliwia najpierw utworzyć obiekty modelu pisząc proste klasy.</span><span class="sxs-lookup"><span data-stu-id="0153d-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="0153d-123">(Te są nazywane także POCO klasy, z "zwykły stary CLR obiekty".) Następnie możesz wybrać bazy danych na bieżąco z klas, dzięki czemu bardzo czyste i szybkie programowanie przepływu pracy.</span><span class="sxs-lookup"><span data-stu-id="0153d-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="0153d-124">Dodawanie klas modelu</span><span class="sxs-lookup"><span data-stu-id="0153d-124">Adding Model Classes</span></span>

<span data-ttu-id="0153d-125">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modele* folderu, wybierz opcję **Dodaj**, a następnie wybierz **klasy**.</span><span class="sxs-lookup"><span data-stu-id="0153d-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="0153d-126">Nazwa *klasy* "Filmu".</span><span class="sxs-lookup"><span data-stu-id="0153d-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="0153d-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="0153d-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="0153d-128">Dodaj następujące właściwości pięć `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="0153d-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="0153d-129">Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0153d-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="0153d-130">Każde wystąpienie `Movie` obiektu odpowiada wiersz w tabeli bazy danych, a każda właściwość `Movie` klasy przypisze do kolumny w tabeli.</span><span class="sxs-lookup"><span data-stu-id="0153d-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="0153d-131">W tym samym pliku Dodaj następujące `MovieDBContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="0153d-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="0153d-132">`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0153d-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="0153d-133">`MovieDBContext` Pochodną `DbContext` pochodzącymi z programu Entity Framework w klasie podstawowej.</span><span class="sxs-lookup"><span data-stu-id="0153d-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="0153d-134">Aby uzyskać więcej informacji na temat `DbContext` i `DbSet`, zobacz [poprawy wydajności Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="0153d-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="0153d-135">Aby można było odwołać `DbContext` i `DbSet`, należy dodać następujące `using` instrukcji w górnej części pliku:</span><span class="sxs-lookup"><span data-stu-id="0153d-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="0153d-136">Pełną *Movie.cs* plików są wyświetlane poniżej.</span><span class="sxs-lookup"><span data-stu-id="0153d-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="0153d-137">Tworzenie parametrów połączenia i Praca z programu SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="0153d-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="0153d-138">`MovieDBContext` Obsługuje klasy utworzone zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0153d-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="0153d-139">Jedno pytanie, który może poprosić o jest jednak sposób Określ bazę danych, które będą łączyć się.</span><span class="sxs-lookup"><span data-stu-id="0153d-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="0153d-140">Należy to zrobić, dodając informacje o połączeniu w *Web.config* pliku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0153d-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="0153d-141">Otwórz katalog główny aplikacji *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="0153d-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="0153d-142">(Nie *Web.config* w pliku *widoków* folderu.) Na poniższym obrazie Pokaż zarówno *Web.config* plików; otwórz *Web.config* w czerwonym kółku pliku.</span><span class="sxs-lookup"><span data-stu-id="0153d-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="0153d-143">Dodaj następujące parametry połączenia do `<connectionStrings>` element *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="0153d-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="0153d-144">W poniższym przykładzie przedstawiono część *Web.config* plik o nowe parametry połączenia dodany:</span><span class="sxs-lookup"><span data-stu-id="0153d-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="0153d-145">Mała ilość kodu i XML to wszystko, co potrzebne do zapisu w celu reprezentowania i przechowywania danych filmu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0153d-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="0153d-146">Następnie będzie utworzyć nowy `MoviesController` klasy, która służy do wyświetlania danych filmu i Zezwalaj użytkownikom na tworzenie nowych list filmu.</span><span class="sxs-lookup"><span data-stu-id="0153d-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0153d-147">[Poprzednie](adding-a-view.md)
[dalej](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="0153d-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
