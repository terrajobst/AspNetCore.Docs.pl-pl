---
title: Wprowadzenie do korzystania z programu Entity Framework 6 i ASP.NET Core
author: tdykstra
description: "W tym artykule przedstawiono sposób użycia Entity Framework 6 w aplikacji platformy ASP.NET Core."
keywords: EF platformy ASP.NET Core programu Entity Framework 6
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.assetid: 016cc836-4c43-45a4-b9a7-9efaf53350df
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 8abec95c591f20069e20eec55fd21503e74f8606
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="7f6f4-104">Wprowadzenie do platformy ASP.NET Core oraz Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="7f6f4-104">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="7f6f4-105">Przez [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), i [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7f6f4-105">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="7f6f4-106">W tym artykule przedstawiono sposób użycia Entity Framework 6 w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-106">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="7f6f4-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="7f6f4-107">Overview</span></span>

<span data-ttu-id="7f6f4-108">Aby korzystać z programu Entity Framework 6, projekt ma Kompiluj dla środowiska .NET Framework, ponieważ Entity Framework 6 nie obsługuje platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-108">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="7f6f4-109">Jeśli potrzebujesz funkcji i platform musisz uaktualnić do [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="7f6f4-109">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="7f6f4-110">Zalecanym sposobem użycia Entity Framework 6 w aplikacji platformy ASP.NET Core jest umieszczenie kontekstu EF6 i klasy modelu w bibliotece klas projektu którego element docelowy pełna platformy.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-110">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="7f6f4-111">Dodaj odwołanie do biblioteki klas z projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-111">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="7f6f4-112">Zobacz przykład [rozwiązania Visual Studio z projektami EF6 i ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="7f6f4-112">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="7f6f4-113">Nie można ustawić kontekstu EF6 w projekcie platformy ASP.NET Core ponieważ projekty .NET Core nie obsługuje wszystkich funkcji, które EF6 polecenia takie jak *Enable-Migrations* wymagają.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-113">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="7f6f4-114">Niezależnie od typu projektu, w którym możesz znaleźć kontekst EF6 tylko narzędzia wiersza polecenia EF6 pracować z kontekstem EF6.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-114">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="7f6f4-115">Na przykład `Scaffold-DbContext` jest dostępna tylko w Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-115">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="7f6f4-116">Jeśli potrzebujesz odtwarzanie bazy danych do modelu EF6, zobacz [Code First istniejącą bazę danych](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="7f6f4-116">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="7f6f4-117">Odwołanie pełna platformy i EF6 w projekcie platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f6f4-117">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="7f6f4-118">Projekt platformy ASP.NET Core musi odwoływać się do środowiska .NET framework i EF6.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-118">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="7f6f4-119">Na przykład *.csproj* pliku projektu platformy ASP.NET Core będzie wyglądać podobnie do poniższego przykładu (wyświetlane są tylko odpowiednich części pliku).</span><span class="sxs-lookup"><span data-stu-id="7f6f4-119">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="7f6f4-120">Jeśli tworzysz nowy projekt, użyj **aplikacji sieci Web platformy ASP.NET Core (.NET Framework)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-120">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="7f6f4-121">Obsługa parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="7f6f4-121">Handle connection strings</span></span>

<span data-ttu-id="7f6f4-122">Narzędzia wiersza polecenia EF6, które będą używane w projektu biblioteki klas EF6 wymaga konstruktora domyślnego, dzięki czemu można ich wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-122">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="7f6f4-123">Ale prawdopodobnie należy określić parametry połączenia do użycia w projekcie platformy ASP.NET Core, w którym to przypadku konstruktora z kontekstu musi mieć parametr, który umożliwia przekazywanie w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-123">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="7f6f4-124">Oto przykład.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-124">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="7f6f4-125">Ponieważ kontekst EF6 nie ma konstruktora bez parametrów, projekt EF6 musi zapewniać implementację elementu [interfejsu IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="7f6f4-125">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="7f6f4-126">Narzędzia wiersza polecenia EF6 znajdzie i używać tej implementacji, dlatego można utworzyć wystąpienie kontekstu.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-126">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="7f6f4-127">Oto przykład.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-127">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="7f6f4-128">W tym przykładowym kodzie `IDbContextFactory` implementacji przekazuje w ciągu połączenia ustalony.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-128">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="7f6f4-129">Jest to ciąg połączenia, który będzie używać narzędzi wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-129">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="7f6f4-130">Należy zaimplementować strategię, aby upewnić się, że biblioteka klas używa te same parametry połączenia, który korzysta z aplikacji wywołującej.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-130">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="7f6f4-131">Na przykład można pobrać wartości ze zmiennej środowiskowej w obu tych projektów.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-131">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="7f6f4-132">Konfigurowanie iniekcji zależności w projekcie platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f6f4-132">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="7f6f4-133">W projekcie Core *Startup.cs* pliku skonfigurowane w kontekście EF6 iniekcji zależności (Podpisane) w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-133">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="7f6f4-134">Obiektów kontekstu EF ma być ograniczone do istnienia danego żądania.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-134">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="7f6f4-135">Następnie można pobrać wystąpienia kontekstu w kontrolerach przy użyciu Podpisane.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-135">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="7f6f4-136">Kod jest podobny do piszesz kontekstu EF rdzeni:</span><span class="sxs-lookup"><span data-stu-id="7f6f4-136">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="7f6f4-137">Przykładowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="7f6f4-137">Sample application</span></span>

<span data-ttu-id="7f6f4-138">Przykładową aplikację pracy, zobacz [przykładowe rozwiązanie Visual Studio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) dołączony w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-138">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="7f6f4-139">W tym przykładzie można utworzyć od podstaw przez następujące czynności w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7f6f4-139">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="7f6f4-140">Tworzenie rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-140">Create a solution.</span></span>

* <span data-ttu-id="7f6f4-141">**Dodaj nowy projekt > sieci Web > Aplikacja sieci Web platformy ASP.NET Core (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="7f6f4-141">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="7f6f4-142">**Dodaj nowy projekt > klasycznego pulpitu systemu Windows > klasy biblioteki (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="7f6f4-142">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="7f6f4-143">W **Konsola Menedżera pakietów** (PMC) dla obu projektów, uruchom polecenie `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-143">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="7f6f4-144">W projektu biblioteki klas, utworzyć klasy modelu danych i klasę kontekstu i implementacja `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-144">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="7f6f4-145">W PMC dla projektu biblioteki klas, Uruchom polecenia `Enable-Migrations` i `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-145">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="7f6f4-146">Jeśli ustawisz projekt platformy ASP.NET Core jako projekt startowy, Dodaj `-StartupProjectName EF6` do tych poleceń.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-146">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="7f6f4-147">W projekcie Core Dodaj odwołanie projektu do projektu biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-147">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="7f6f4-148">W projekcie Core w *Startup.cs*, zarejestruj się w kontekście celu Podpisane.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-148">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="7f6f4-149">W projekcie Core w *appsettings.json*, Dodaj parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-149">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="7f6f4-150">W projekcie Core Dodaj kontroler i widoków, aby sprawdzić, czy można odczytywania i zapisywania danych.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-150">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="7f6f4-151">(Należy pamiętać, że ASP.NET MVC podstawowych szkieletów nie będzie działać w kontekście EF6 odwołanie z biblioteki klas.)</span><span class="sxs-lookup"><span data-stu-id="7f6f4-151">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="7f6f4-152">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="7f6f4-152">Summary</span></span>

<span data-ttu-id="7f6f4-153">W tym artykule udostępnił podstawowe wskazówki dotyczące korzystania z programu Entity Framework 6 w aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7f6f4-153">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f6f4-154">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7f6f4-154">Additional Resources</span></span>

* [<span data-ttu-id="7f6f4-155">Entity Framework - konfiguracji opartej na kodzie</span><span class="sxs-lookup"><span data-stu-id="7f6f4-155">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
