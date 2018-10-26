---
title: Rozpoczynanie pracy z platformą ASP.NET Core i Entity Framework 6
author: rick-anderson
description: W tym artykule pokazano, jak używać platformy Entity Framework 6 w aplikacji ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/entity-framework-6
ms.openlocfilehash: b7679afbe4c364386fe8f16d22d7e9797a3e0c27
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090062"
---
# <a name="get-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="2cdb5-103">Rozpoczynanie pracy z platformą ASP.NET Core i Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2cdb5-103">Get Started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="2cdb5-104">Przez [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2cdb5-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="2cdb5-105">W tym artykule pokazano, jak używać platformy Entity Framework 6 w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="2cdb5-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2cdb5-106">Overview</span></span>

<span data-ttu-id="2cdb5-107">Aby korzystać z platformy Entity Framework 6, projekt ma kompilowanie z użyciem platformy .NET Framework, zgodnie z platformy Entity Framework 6 nie obsługuje platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 doesn't support .NET Core.</span></span> <span data-ttu-id="2cdb5-108">Jeśli potrzebujesz funkcji dla wielu platform należy uaktualnić do [Entity Framework Core](/ef/).</span><span class="sxs-lookup"><span data-stu-id="2cdb5-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](/ef/).</span></span>

<span data-ttu-id="2cdb5-109">Zalecanym sposobem użycia platformy Entity Framework 6 w aplikacji ASP.NET Core jest umieszczenie kontekstu EF6 i klasy modeli w bibliotece klas projektu przeznaczonego pełny framework.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="2cdb5-110">Dodaj odwołanie do biblioteki klas z projektów ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="2cdb5-111">Zobacz przykład [rozwiązania Visual Studio z projektami EF6 i ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span><span class="sxs-lookup"><span data-stu-id="2cdb5-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="2cdb5-112">Nie można umieścić uprawnieniami EF6 w projektach programu ASP.NET Core, ponieważ projektów .NET Core nie obsługują wszystkie funkcje, które EF6 polecenia, takie jak *Enable-Migrations* wymagają.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="2cdb5-113">Niezależnie od typu projektu, w którym możesz znaleźć kontekstu EF6 tylko narzędzia wiersza polecenia platformy EF6 pracować z uprawnieniami EF6.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="2cdb5-114">Na przykład `Scaffold-DbContext` jest dostępna tylko w programie Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="2cdb5-115">Jeśli zachodzi potrzeba odtwarzanie bazy danych do modelu EF6, zobacz [Code First istniejącą bazę danych](https://msdn.microsoft.com/jj200620).</span><span class="sxs-lookup"><span data-stu-id="2cdb5-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="2cdb5-116">Dokumentacja pełny framework i EF6 w projekcie platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2cdb5-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="2cdb5-117">Projekt platformy ASP.NET Core musi odwoływać się do środowiska .NET framework i EF6.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="2cdb5-118">Na przykład *.csproj* plik projektu programu ASP.NET Core będzie wyglądać podobnie do poniższego przykładu (wyświetlane są tylko odpowiednie części pliku).</span><span class="sxs-lookup"><span data-stu-id="2cdb5-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="2cdb5-119">Podczas tworzenia nowego projektu, użyj **aplikacja sieci Web programu ASP.NET Core (.NET Framework)** szablonu.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-119">When creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="2cdb5-120">Obsługa parametrów połączenia</span><span class="sxs-lookup"><span data-stu-id="2cdb5-120">Handle connection strings</span></span>

<span data-ttu-id="2cdb5-121">EF6 narzędzia wiersza polecenia, których można używać w projekcie biblioteki klas platformy EF6 wymagają domyślnego konstruktora, dzięki czemu można utworzyć wystąpienie kontekstu.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="2cdb5-122">Jednak prawdopodobnie będziesz chciał określić parametry połączenia do użycia w projekcie platformy ASP.NET Core, w którym to przypadku konstruktora kontekście musi mieć parametr, który umożliwia przekazywanie parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="2cdb5-123">Oto przykład.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="2cdb5-124">Ponieważ kontekst EF6 nie ma konstruktora bez parametrów, projekt EF6 musi dostarczyć implementację [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span><span class="sxs-lookup"><span data-stu-id="2cdb5-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="2cdb5-125">Narzędzia wiersza polecenia platformy EF6 znajdzie i użyć tej implementacji, dzięki czemu można utworzyć wystąpienie kontekstu.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="2cdb5-126">Oto przykład.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="2cdb5-127">W tym przykładowym kodzie `IDbContextFactory` implementacji przekazuje ciągów połączeń zakodowanych.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="2cdb5-128">Jest to ciąg połączenia, który będzie używać narzędzi wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="2cdb5-129">Należy zaimplementować strategię, aby upewnić się, że biblioteka klas używa jednych parametrach połączenia, który używa aplikacji wywołującej.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="2cdb5-130">Na przykład można uzyskać wartość ze zmiennej środowiskowej w obu projektach.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="2cdb5-131">Konfigurowanie wstrzykiwanie zależności w projekcie platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2cdb5-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="2cdb5-132">W projekcie Core *Startup.cs* plików, skonfiguruj kontekst EF6 wstrzykiwanie zależności (DI) w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="2cdb5-133">Obiektów kontekstu EF powinien być ograniczony okres istnienia danego żądania.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="2cdb5-134">Następnie można pobrać wystąpienia kontekstu w kontrolerach przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="2cdb5-135">Kod jest podobny do piszesz dla kontekstu programu EF Core:</span><span class="sxs-lookup"><span data-stu-id="2cdb5-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="2cdb5-136">Przykładowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="2cdb5-136">Sample application</span></span>

<span data-ttu-id="2cdb5-137">Pracy przykładowej aplikacji, zobacz [przykładowe rozwiązanie programu Visual Studio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) dołączony w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="2cdb5-138">W tym przykładzie można tworzyć od podstaw przez następujące kroki w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="2cdb5-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="2cdb5-139">Tworzenie rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-139">Create a solution.</span></span>

* <span data-ttu-id="2cdb5-140">**Dodaj** > **nowy projekt** > **Web** > **aplikacji sieci Web platformy ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="2cdb5-140">**Add** > **New Project** > **Web** > **ASP.NET Core Web Application**</span></span>
  * <span data-ttu-id="2cdb5-141">W oknie dialogowym wyboru szablonów projektu wybierz na liście rozwijanej interfejsu API i .NET Framework</span><span class="sxs-lookup"><span data-stu-id="2cdb5-141">In project template selection dialog, select API and .NET Framework in dropdown</span></span>

* <span data-ttu-id="2cdb5-142">**Dodaj** > **nowy projekt** > **pulpitu Windows** > **klasy biblioteki (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="2cdb5-142">**Add** > **New Project** > **Windows Desktop** > **Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="2cdb5-143">W **Konsola Menedżera pakietów** (PMC) dla obu projektów, uruchom polecenie `Install-Package Entityframework`.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-143">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="2cdb5-144">W projekcie biblioteki klas Tworzenie klas modelu danych i klasy kontekstu i implementację `IDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-144">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="2cdb5-145">W konsoli zarządzania Pakietami dla projektu biblioteki klas, Uruchom polecenia `Enable-Migrations` i `Add-Migration Initial`.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-145">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="2cdb5-146">Jeśli ustawisz projekt platformy ASP.NET Core jako projekt startowy, Dodaj `-StartupProjectName EF6` tych poleceń.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-146">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="2cdb5-147">W projekcie podstawowe należy dodać odwołanie projektu do projektu biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-147">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="2cdb5-148">W projekcie podstawowe w *Startup.cs*, zarejestruj DI kontekstu.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-148">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="2cdb5-149">W projekcie podstawowe w *appsettings.json*, Dodaj parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-149">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="2cdb5-150">W projekcie Core Dodaj kontrolera i widoki, aby sprawdzić, czy może odczytywać i zapisywać dane.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-150">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="2cdb5-151">(Zwróć uwagę, że tworzenia szkieletu ASP.NET Core MVC nie będzie działać w kontekście platformy EF6 stanowiący odwołanie z biblioteki klas).</span><span class="sxs-lookup"><span data-stu-id="2cdb5-151">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="2cdb5-152">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="2cdb5-152">Summary</span></span>

<span data-ttu-id="2cdb5-153">Ten artykuł ma podano podstawowe wskazówki dotyczące korzystania z platformy Entity Framework 6 w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2cdb5-153">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2cdb5-154">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2cdb5-154">Additional resources</span></span>

* [<span data-ttu-id="2cdb5-155">Entity Framework - konfiguracja na podstawie kodu</span><span class="sxs-lookup"><span data-stu-id="2cdb5-155">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
