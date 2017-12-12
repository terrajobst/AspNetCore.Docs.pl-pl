---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Dodawanie modelu (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a><span data-ttu-id="a4ec3-103">Dodawanie modelu (VB)</span><span class="sxs-lookup"><span data-stu-id="a4ec3-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="a4ec3-104">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a4ec3-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="a4ec3-105">Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="a4ec3-106">Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="a4ec3-107">Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="a4ec3-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="a4ec3-108">Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:</span><span class="sxs-lookup"><span data-stu-id="a4ec3-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="a4ec3-109">Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a4ec3-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="a4ec3-110">Aktualizacji narzędzi programu ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="a4ec3-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="a4ec3-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)</span><span class="sxs-lookup"><span data-stu-id="a4ec3-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="a4ec3-112">Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="a4ec3-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="a4ec3-113">Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="a4ec3-114">[Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="a4ec3-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="a4ec3-115">Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/adding-a-model.md) tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="a4ec3-116">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="a4ec3-116">Adding a Model</span></span>

<span data-ttu-id="a4ec3-117">W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="a4ec3-118">Te klasy będzie "modelu" części aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="a4ec3-119">Użyjesz technologii dostępu do danych .NET Framework, znany jako Entity Framework do definiowania i pracy z tych klas modelu.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="a4ec3-120">Obsługuje programu Entity Framework (nazywanej często EF) o nazwie modelu programowania *Code First*.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="a4ec3-121">Kod umożliwia najpierw utworzyć obiekty modelu pisząc proste klasy.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="a4ec3-122">(Te są nazywane także POCO klasy, z "zwykły stary CLR obiekty".) Następnie możesz wybrać bazy danych na bieżąco z klas, dzięki czemu bardzo czyste i szybkie programowanie przepływu pracy.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="a4ec3-123">Dodawanie klas modelu</span><span class="sxs-lookup"><span data-stu-id="a4ec3-123">Adding Model Classes</span></span>

<span data-ttu-id="a4ec3-124">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modele* folderu, wybierz opcję **Dodaj**, a następnie wybierz **klasy**.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="a4ec3-125">Nazwa klasy "Filmu".</span><span class="sxs-lookup"><span data-stu-id="a4ec3-125">Name the class "Movie".</span></span>

<span data-ttu-id="a4ec3-126">Dodaj następujące właściwości pięć `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="a4ec3-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="a4ec3-127">Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="a4ec3-128">Każde wystąpienie `Movie` obiektu odpowiada wiersz w tabeli bazy danych, a każda właściwość `Movie` klasy przypisze do kolumny w tabeli.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="a4ec3-129">W tym samym pliku Dodaj następujące `MovieDBContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="a4ec3-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="a4ec3-130">`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="a4ec3-131">`MovieDBContext` Pochodną `DbContext` pochodzącymi z programu Entity Framework w klasie podstawowej.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="a4ec3-132">Aby uzyskać więcej informacji na temat `DbContext` i `DbSet`, zobacz [poprawy wydajności Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="a4ec3-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="a4ec3-133">Aby można było odwołać `DbContext` i `DbSet`, należy dodać następujące `imports` instrukcji w górnej części pliku:</span><span class="sxs-lookup"><span data-stu-id="a4ec3-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="a4ec3-134">Pełną *Movie.vb* plików są wyświetlane poniżej.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="a4ec3-135">Tworzenie parametrów połączenia i Praca z programu SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="a4ec3-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="a4ec3-136">`MovieDBContext` Obsługuje klasy utworzone zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="a4ec3-137">Jedno pytanie, który może poprosić o jest jednak sposób Określ bazę danych, które będą łączyć się.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="a4ec3-138">Należy to zrobić, dodając informacje o połączeniu w *Web.config* pliku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="a4ec3-139">Otwórz katalog główny aplikacji *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="a4ec3-140">(Nie *Web.config* w pliku *widoków* folderu.) Na poniższym obrazie Pokaż zarówno *Web.config* plików; otwórz *Web.config* w czerwonym kółku pliku.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="a4ec3-141">Dodaj następujące parametry połączenia do `<connectionStrings>` element *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="a4ec3-142">W poniższym przykładzie przedstawiono część *Web.config* plik o nowe parametry połączenia dodany:</span><span class="sxs-lookup"><span data-stu-id="a4ec3-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="a4ec3-143">Mała ilość kodu i XML to wszystko, co potrzebne do zapisu w celu reprezentowania i przechowywania danych filmu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="a4ec3-144">Następnie będzie utworzyć nowy `MoviesController` klasy, która służy do wyświetlania danych filmu i Zezwalaj użytkownikom na tworzenie nowych list filmu.</span><span class="sxs-lookup"><span data-stu-id="a4ec3-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a4ec3-145">[Poprzednie](adding-a-view.md)
[dalej](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="a4ec3-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
