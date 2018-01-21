---
title: Wprowadzenie do korzystania z programu Entity Framework 6 i ASP.NET Core
author: tdykstra
description: "W tym artykule przedstawiono sposób użycia Entity Framework 6 w aplikacji platformy ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 51445b8c110ad618aeb680148ccf4304a45ee16e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="a642b-103">Wprowadzenie do platformy ASP.NET Core oraz Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a642b-103">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="a642b-104">Przez [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), i [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a642b-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a642b-105">W tym artykule przedstawiono sposób użycia Entity Framework 6 w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a642b-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="a642b-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="a642b-106">Overview</span></span>

<span data-ttu-id="a642b-107">Aby korzystać z programu Entity Framework 6, projekt ma Kompiluj dla środowiska .NET Framework, ponieważ Entity Framework 6 nie obsługuje platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a642b-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="a642b-108">Jeśli potrzebujesz funkcji i platform musisz uaktualnić do [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="a642b-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="a642b-109">Zalecanym sposobem użycia Entity Framework 6 w aplikacji platformy ASP.NET Core jest umieszczenie kontekstu EF6 i klasy modelu w bibliotece klas projektu którego element docelowy pełna platformy.</span><span class="sxs-lookup"><span data-stu-id="a642b-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="a642b-110">Dodaj odwołanie do biblioteki klas z projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a642b-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="a642b-111">Zobacz przykład [rozwiązania Visual Studio z projektami EF6 i ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="a642b-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="a642b-112">Nie można ustawić kontekstu EF6 w projekcie platformy ASP.NET Core ponieważ projekty .NET Core nie obsługuje wszystkich funkcji, które EF6 polecenia takie jak *Enable-Migrations* wymagają.</span><span class="sxs-lookup"><span data-stu-id="a642b-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="a642b-113">Niezależnie od typu projektu, w którym możesz znaleźć kontekst EF6 tylko narzędzia wiersza polecenia EF6 pracować z kontekstem EF6.</span><span class="sxs-lookup"><span data-stu-id="a642b-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="a642b-114">Na przykład `Scaffold-DbContext` jest dostępna tylko w Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a642b-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="a642b-115">Jeśli potrzebujesz odtwarzanie bazy danych do modelu EF6, zobacz [Code First istniejącą bazę danych](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="a642b-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="a642b-116">Odwołanie pełna platformy i EF6 w projekcie platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a642b-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="a642b-117">Projekt platformy ASP.NET Core musi odwoływać się do środowiska .NET framework i EF6.</span><span class="sxs-lookup"><span data-stu-id="a642b-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="a642b-118">Na przykład *.csproj* pliku projektu platformy ASP.NET Core będzie wyglądać podobnie do poniższego przykładu (wyświetlane są tylko odpowiednich części pliku).</span><span class="sxs-lookup"><span data-stu-id="a642b-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="a642b-119">Jeśli tworzysz nowy projekt, użyj **aplikacji sieci Web platformy ASP.NET Core (.NET Framework)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="a642b-119">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="a642b-120">Obsługa parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="a642b-120">Handle connection strings</span></span>

<span data-ttu-id="a642b-121">Narzędzia wiersza polecenia EF6, które będą używane w projektu biblioteki klas EF6 wymaga konstruktora domyślnego, dzięki czemu można ich wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="a642b-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="a642b-122">Ale prawdopodobnie należy określić parametry połączenia do użycia w projekcie platformy ASP.NET Core, w którym to przypadku konstruktora z kontekstu musi mieć parametr, który umożliwia przekazywanie w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="a642b-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="a642b-123">Oto przykład.</span><span class="sxs-lookup"><span data-stu-id="a642b-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="a642b-124">Ponieważ kontekst EF6 nie ma konstruktora bez parametrów, projekt EF6 musi zapewniać implementację elementu [interfejsu IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="a642b-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="a642b-125">Narzędzia wiersza polecenia EF6 znajdzie i używać tej implementacji, dlatego można utworzyć wystąpienie kontekstu.</span><span class="sxs-lookup"><span data-stu-id="a642b-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="a642b-126">Oto przykład.</span><span class="sxs-lookup"><span data-stu-id="a642b-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="a642b-127">W tym przykładowym kodzie `IDbContextFactory` implementacji przekazuje w ciągu połączenia ustalony.</span><span class="sxs-lookup"><span data-stu-id="a642b-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="a642b-128">Jest to ciąg połączenia, który będzie używać narzędzi wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="a642b-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="a642b-129">Należy zaimplementować strategię, aby upewnić się, że biblioteka klas używa te same parametry połączenia, który korzysta z aplikacji wywołującej.</span><span class="sxs-lookup"><span data-stu-id="a642b-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="a642b-130">Na przykład można pobrać wartości ze zmiennej środowiskowej w obu tych projektów.</span><span class="sxs-lookup"><span data-stu-id="a642b-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="a642b-131">Konfigurowanie iniekcji zależności w projekcie platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a642b-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="a642b-132">W projekcie Core *Startup.cs* pliku skonfigurowane w kontekście EF6 iniekcji zależności (Podpisane) w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a642b-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="a642b-133">Obiektów kontekstu EF ma być ograniczone do istnienia danego żądania.</span><span class="sxs-lookup"><span data-stu-id="a642b-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="a642b-134">Następnie można pobrać wystąpienia kontekstu w kontrolerach przy użyciu Podpisane.</span><span class="sxs-lookup"><span data-stu-id="a642b-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="a642b-135">Kod jest podobny do piszesz kontekstu EF rdzeni:</span><span class="sxs-lookup"><span data-stu-id="a642b-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="a642b-136">Przykładowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="a642b-136">Sample application</span></span>

<span data-ttu-id="a642b-137">Przykładową aplikację pracy, zobacz [przykładowe rozwiązanie Visual Studio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) dołączony w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="a642b-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="a642b-138">W tym przykładzie można utworzyć od podstaw przez następujące czynności w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a642b-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="a642b-139">Tworzenie rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="a642b-139">Create a solution.</span></span>

* <span data-ttu-id="a642b-140">**Dodaj nowy projekt > sieci Web > Aplikacja sieci Web platformy ASP.NET Core (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="a642b-140">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="a642b-141">**Dodaj nowy projekt > klasycznego pulpitu systemu Windows > klasy biblioteki (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="a642b-141">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="a642b-142">W **Konsola Menedżera pakietów** (PMC) dla obu projektów, uruchom polecenie `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="a642b-142">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="a642b-143">W projektu biblioteki klas, utworzyć klasy modelu danych i klasę kontekstu i implementacja `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="a642b-143">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="a642b-144">W PMC dla projektu biblioteki klas, Uruchom polecenia `Enable-Migrations` i `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="a642b-144">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="a642b-145">Jeśli ustawisz projekt platformy ASP.NET Core jako projekt startowy, Dodaj `-StartupProjectName EF6` do tych poleceń.</span><span class="sxs-lookup"><span data-stu-id="a642b-145">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="a642b-146">W projekcie Core Dodaj odwołanie projektu do projektu biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="a642b-146">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="a642b-147">W projekcie Core w *Startup.cs*, zarejestruj się w kontekście celu Podpisane.</span><span class="sxs-lookup"><span data-stu-id="a642b-147">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="a642b-148">W projekcie Core w *appsettings.json*, Dodaj parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="a642b-148">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="a642b-149">W projekcie Core Dodaj kontroler i widoków, aby sprawdzić, czy można odczytywania i zapisywania danych.</span><span class="sxs-lookup"><span data-stu-id="a642b-149">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="a642b-150">(Należy pamiętać, że ASP.NET MVC podstawowych szkieletów nie będzie działać w kontekście EF6 odwołanie z biblioteki klas.)</span><span class="sxs-lookup"><span data-stu-id="a642b-150">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="a642b-151">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a642b-151">Summary</span></span>

<span data-ttu-id="a642b-152">W tym artykule udostępnił podstawowe wskazówki dotyczące korzystania z programu Entity Framework 6 w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a642b-152">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a642b-153">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a642b-153">Additional Resources</span></span>

* [<span data-ttu-id="a642b-154">Entity Framework - konfiguracji opartej na kodzie</span><span class="sxs-lookup"><span data-stu-id="a642b-154">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
