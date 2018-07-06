---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Dodawanie modelu (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna w tym miejscu używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 61ba86e4bfbb0557b34d245555bc56a320335628
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820775"
---
<a name="adding-a-model-c"></a><span data-ttu-id="64166-104">Dodawanie modelu (C#)</span><span class="sxs-lookup"><span data-stu-id="64166-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="64166-105">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="64166-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="64166-106">Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="64166-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="64166-107">Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej.</span><span class="sxs-lookup"><span data-stu-id="64166-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="64166-108">Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="64166-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="64166-109">Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:</span><span class="sxs-lookup"><span data-stu-id="64166-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="64166-110">Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.</span><span class="sxs-lookup"><span data-stu-id="64166-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="64166-111">Program ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="64166-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="64166-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)</span><span class="sxs-lookup"><span data-stu-id="64166-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="64166-113">Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="64166-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="64166-114">Projekt Visual Web Developer, przy użyciu kodu źródłowego języka C# jest dostępny powiązany z tym tematem.</span><span class="sxs-lookup"><span data-stu-id="64166-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="64166-115">[Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="64166-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="64166-116">Jeśli wolisz języka Visual Basic, przełącz się do [wersji języka Visual Basic](../vb/adding-a-model.md) po ukończeniu tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="64166-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="64166-117">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="64166-117">Adding a Model</span></span>

<span data-ttu-id="64166-118">W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="64166-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="64166-119">Te klasy będzie "model" część aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="64166-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="64166-120">Użyjesz technologii dostępu do danych .NET Framework, znane jako Entity Framework do definiowania i pracować z tych klas modelu.</span><span class="sxs-lookup"><span data-stu-id="64166-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="64166-121">Obsługuje platformy Entity Framework (często określanymi jako EF) o nazwie paradygmat programowania *Code First*.</span><span class="sxs-lookup"><span data-stu-id="64166-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="64166-122">Najpierw kod pozwala na tworzenie obiektów modelu, pisząc proste klas.</span><span class="sxs-lookup"><span data-stu-id="64166-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="64166-123">(Te są nazywane także POCO klas, od "obiektów CLR zwykły stary.") Można następnie skonfigurować bazę danych, tworzone na bieżąco z klas, co umożliwia przepływ pracy tworzenia bardzo szybkie i przejrzysty.</span><span class="sxs-lookup"><span data-stu-id="64166-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="64166-124">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="64166-124">Adding Model Classes</span></span>

<span data-ttu-id="64166-125">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modeli* folderu, wybierz **Dodaj**, a następnie wybierz pozycję **klasy**.</span><span class="sxs-lookup"><span data-stu-id="64166-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="64166-126">Nazwa *klasy* "Filmu".</span><span class="sxs-lookup"><span data-stu-id="64166-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="64166-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="64166-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="64166-128">Dodaj następujące właściwości pięć `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="64166-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="64166-129">Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="64166-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="64166-130">Każde wystąpienie `Movie` obiektu odnoszą się do wiersza w tabeli bazy danych, a każda właściwość `Movie` klasy będzie zmapowana do kolumny w tabeli.</span><span class="sxs-lookup"><span data-stu-id="64166-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="64166-131">W tym samym pliku Dodaj następujący kod `MovieDBContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="64166-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="64166-132">`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="64166-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="64166-133">`MovieDBContext` Pochodzi od klasy `DbContext` dostarczane przez program Entity Framework klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="64166-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="64166-134">Aby uzyskać więcej informacji na temat `DbContext` i `DbSet`, zobacz [udoskonalenia dotyczące produktywności programu Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="64166-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="64166-135">Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następujące `using` instrukcji w górnej części pliku:</span><span class="sxs-lookup"><span data-stu-id="64166-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="64166-136">Pełne *Movie.cs* plików znajdują się poniżej.</span><span class="sxs-lookup"><span data-stu-id="64166-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="64166-137">Tworzenie parametrów połączenia i Praca z użyciem programów SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="64166-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="64166-138">`MovieDBContext` Utworzone klasy obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="64166-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="64166-139">Jedno pytanie, które możesz zadawać, jest jednak sposób określić bazę danych, która zostanie nawiązane połączenie.</span><span class="sxs-lookup"><span data-stu-id="64166-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="64166-140">Należy to zrobić, dodając informacje o połączeniu w *Web.config* pliku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="64166-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="64166-141">Otwórz katalog główny aplikacji *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="64166-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="64166-142">(Nie *Web.config* w pliku *widoków* folderu.) Na poniższej ilustracji Pokaż obie *Web.config* plików; otwórz *Web.config* pliku zakreślony na czerwono.</span><span class="sxs-lookup"><span data-stu-id="64166-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="64166-143">Dodaj poniższe parametry połączenia do `<connectionStrings>` element *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="64166-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="64166-144">W poniższym przykładzie pokazano część *Web.config* pliku dodano nowy ciąg połączenia:</span><span class="sxs-lookup"><span data-stu-id="64166-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="64166-145">Ta niewielką ilość kodu i XML to wszystko, czego potrzebujesz do zapisu w celu reprezentowania i przechowywanie danych filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="64166-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="64166-146">Następnie utworzysz nowy `MoviesController` klasę, która służy do wyświetlania danych filmów i Zezwalaj użytkownikom na tworzenie nowych list filmu.</span><span class="sxs-lookup"><span data-stu-id="64166-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="64166-147">[Poprzednie](adding-a-view.md)
> [dalej](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="64166-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
