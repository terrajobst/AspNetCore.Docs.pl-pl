---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Dodawanie modelu | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 2d99a9c553ba79922cb0c5e852709ab97ea2e22b
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38211057"
---
<a name="adding-a-model"></a><span data-ttu-id="8c220-102">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="8c220-102">Adding a Model</span></span>
====================
<span data-ttu-id="8c220-103">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8c220-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="8c220-104">W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8c220-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="8c220-105">Te klasy będzie &quot;modelu&quot; wchodzi w skład aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8c220-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="8c220-106">Użyjesz technologii dostępu do danych .NET Framework, znane jako [Entity Framework](https://docs.microsoft.com/ef/) do definiowania i pracować z tych klas modelu.</span><span class="sxs-lookup"><span data-stu-id="8c220-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="8c220-107">Obsługuje platformy Entity Framework (często określanymi jako EF) o nazwie paradygmat programowania *Code First*.</span><span class="sxs-lookup"><span data-stu-id="8c220-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="8c220-108">Najpierw kod pozwala na tworzenie obiektów modelu, pisząc proste klas.</span><span class="sxs-lookup"><span data-stu-id="8c220-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="8c220-109">(Te są również nazywane klasami POCO z &quot;obiektów CLR zwykły stary.&quot;) Można następnie skonfigurować bazę danych, tworzone na bieżąco z klas, co umożliwia przepływ pracy tworzenia bardzo szybkie i przejrzysty.</span><span class="sxs-lookup"><span data-stu-id="8c220-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="8c220-110">Jeśli musisz najpierw utworzyć bazę danych, możesz wykonać ten samouczek, aby dowiedzieć się więcej o programowaniu aplikacji MVC i programem EF.</span><span class="sxs-lookup"><span data-stu-id="8c220-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="8c220-111">Następnie można wykonać Tom Fizmakens [funkcja tworzenia szkieletu ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) samouczka, który dotyczy pierwszego podejścia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8c220-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="8c220-112">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="8c220-112">Adding Model Classes</span></span>

<span data-ttu-id="8c220-113">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modeli* folderu, wybierz **Dodaj**, a następnie wybierz pozycję **klasy**.</span><span class="sxs-lookup"><span data-stu-id="8c220-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="8c220-114">Wprowadź *klasy* nazwa &quot;filmu&quot;.</span><span class="sxs-lookup"><span data-stu-id="8c220-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="8c220-115">Dodaj następujące właściwości pięć `Movie` klasy:</span><span class="sxs-lookup"><span data-stu-id="8c220-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="8c220-116">Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8c220-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="8c220-117">Każde wystąpienie `Movie` obiektu odnoszą się do wiersza w tabeli bazy danych, a każda właściwość `Movie` klasy będzie zmapowana do kolumny w tabeli.</span><span class="sxs-lookup"><span data-stu-id="8c220-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="8c220-118">Uwaga: Aby można było używać System.Data.Entity i pokrewne klasy, należy zainstalować [pakietu NuGet programu Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="8c220-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="8c220-119">Kliknij link, aby uzyskać dalsze instrukcje.</span><span class="sxs-lookup"><span data-stu-id="8c220-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="8c220-120">W tym samym pliku Dodaj następujący kod `MovieDBContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="8c220-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="8c220-121">`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8c220-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="8c220-122">`MovieDBContext` Pochodzi od klasy `DbContext` dostarczane przez program Entity Framework klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="8c220-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="8c220-123">Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następujące `using` instrukcji w górnej części pliku:</span><span class="sxs-lookup"><span data-stu-id="8c220-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="8c220-124">Można to zrobić przez ręczne dodanie do przy użyciu instrukcji lub należy umieść kursor nad czerwonych falistych linii, kliknij przycisk `Show potential fixes` i kliknij przycisk `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="8c220-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="8c220-125">Uwaga: Kilka nieużywane `using` instrukcje zostały usunięte.</span><span class="sxs-lookup"><span data-stu-id="8c220-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="8c220-126">Program Visual Studio wyświetli nieużywane zależności jako szary.</span><span class="sxs-lookup"><span data-stu-id="8c220-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="8c220-127">Możesz usunąć nieużywane zależności, ustawiając kursor nad szarego zależności, kliknij przycisk `Show potential fixes` i kliknij przycisk **Usuń nieużywane deklaracje Using.**</span><span class="sxs-lookup"><span data-stu-id="8c220-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="8c220-128">Na koniec dodaliśmy modelu (M MVC).</span><span class="sxs-lookup"><span data-stu-id="8c220-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="8c220-129">W następnej sekcji, którą będziesz pracować parametry połączenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8c220-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8c220-130">[Poprzednie](adding-a-view.md)
> [dalej](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="8c220-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
