---
title: Wzorzec repozytorium za pomocą programu ASP.NET Core
author: ardalis
description: Dowiedz się, jak zaimplementować wzorzec projektowania aplikacji w repozytorium w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342754"
---
# <a name="repository-pattern-with-aspnet-core"></a><span data-ttu-id="c96e7-103">Wzorzec repozytorium za pomocą programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c96e7-103">Repository pattern with ASP.NET Core</span></span>

<span data-ttu-id="c96e7-104">Przez [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c96e7-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c96e7-105">*Wzorca repozytorium* jest szablon projektu, który izoluje dostęp do danych za abstrakcji interfejsu.</span><span class="sxs-lookup"><span data-stu-id="c96e7-105">The *repository pattern* is a design pattern that isolates data access behind interface abstractions.</span></span> <span data-ttu-id="c96e7-106">Połączenie z bazą danych i manipulowania obiektów magazynu danych odbywa się przy użyciu metod dostarczonych przez implementację interfejsu.</span><span class="sxs-lookup"><span data-stu-id="c96e7-106">Connecting to the database and manipulating data storage objects is performed through methods provided by the interface's implementation.</span></span> <span data-ttu-id="c96e7-107">W związku z tym nie ma potrzeby wywoływania kodu radzenia sobie z obaw bazy danych, takie jak połączenia, poleceń i czytników.</span><span class="sxs-lookup"><span data-stu-id="c96e7-107">Consequently, there's no need for calling code to deal with database concerns, such as connections, commands, and readers.</span></span>

<span data-ttu-id="c96e7-108">Implementacja wzorca repozytorium za pomocą programu ASP.NET Core oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="c96e7-108">Implementation of the repository pattern with ASP.NET Core has the following benefits:</span></span>

* <span data-ttu-id="c96e7-109">Organizacja aplikacji jest mniej złożona przy użyciu nie bezpośrednie interdependency między działalności biznesowej i warstwy dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="c96e7-109">Organization of the app is less complex with no direct interdependency between the business and data access layers.</span></span>
* <span data-ttu-id="c96e7-110">Jest łatwiejsze do ponownego użycia kodu dostępu do bazy danych, ponieważ kod jest zarządzane centralnie przez jeden lub więcej repozytoriów.</span><span class="sxs-lookup"><span data-stu-id="c96e7-110">It's easier to reuse database access code because the code is centrally managed by one or more repositories.</span></span>
* <span data-ttu-id="c96e7-111">Domeny biznesowej mogą być niezależne jednostki przetestowane z warstwy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c96e7-111">The business domain can be independently unit tested from the database layer.</span></span>

<span data-ttu-id="c96e7-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c96e7-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c96e7-113">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) zastosowanie wzorca repozytorium umożliwia inicjowanie i wyświetlić listę nazw znaków filmu.</span><span class="sxs-lookup"><span data-stu-id="c96e7-113">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) uses the repository pattern to initialize and display a list of movie character names.</span></span> <span data-ttu-id="c96e7-114">Ta aplikacja używa [Entity Framework Core](/ef/core/) i `ApplicationDbContext` klasy dla jej funkcji trwałości danych, ale infrastrukturą bazy danych nie jest widoczny w przypadku, gdy dane są dostępne.</span><span class="sxs-lookup"><span data-stu-id="c96e7-114">The app uses [Entity Framework Core](/ef/core/) and an `ApplicationDbContext` class for its data persistence, but the database infrastructure isn't apparent where the data is accessed.</span></span> <span data-ttu-id="c96e7-115">Data access i obiektów bazy danych zostały wyabstrahowane za [repozytorium](https://martinfowler.com/eaaCatalog/repository.html).</span><span class="sxs-lookup"><span data-stu-id="c96e7-115">Data access and database objects are abstracted behind a [repository](https://martinfowler.com/eaaCatalog/repository.html).</span></span>

## <a name="repository-interface"></a><span data-ttu-id="c96e7-116">Interfejs repozytorium</span><span class="sxs-lookup"><span data-stu-id="c96e7-116">Repository interface</span></span>

<span data-ttu-id="c96e7-117">Interfejs repozytorium definiuje właściwości i metody dla implementacji.</span><span class="sxs-lookup"><span data-stu-id="c96e7-117">A repository interface defines the properties and methods for implementation.</span></span> <span data-ttu-id="c96e7-118">W przykładowej aplikacji interfejsu repozytorium danych znakowych film jest `ICharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="c96e7-118">In the sample app, the repository interface for movie character data is `ICharacterRepository`.</span></span> <span data-ttu-id="c96e7-119">`ICharacterRepository` definiuje `ListAll` i `Add` metod wymaganych do pracy z `Character` wystąpień w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c96e7-119">`ICharacterRepository` defines the `ListAll` and `Add` methods required to work with `Character` instances in the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c96e7-120">`Character` definiuje się następująco:</span><span class="sxs-lookup"><span data-stu-id="c96e7-120">`Character` is defined as:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a><span data-ttu-id="c96e7-121">Konkretny typ repozytorium</span><span class="sxs-lookup"><span data-stu-id="c96e7-121">Repository concrete type</span></span>

<span data-ttu-id="c96e7-122">Interfejs jest implementowany przez konkretnego typu.</span><span class="sxs-lookup"><span data-stu-id="c96e7-122">The interface is implemented by a concrete type.</span></span> <span data-ttu-id="c96e7-123">W przykładowej aplikacji `CharacterRepository` zarządza kontekst bazy danych i implementuje `ListAll` i `Add` metody:</span><span class="sxs-lookup"><span data-stu-id="c96e7-123">In the sample app, `CharacterRepository` manages the database context and implements the `ListAll` and `Add` methods:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a><span data-ttu-id="c96e7-124">Zarejestruj usługę repozytorium</span><span class="sxs-lookup"><span data-stu-id="c96e7-124">Register the repository service</span></span>

<span data-ttu-id="c96e7-125">Kontekst repozytorium i bazy danych są zarejestrowane w usłudze container service w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c96e7-125">The repository and database context are registered with the service container in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c96e7-126">W przykładowej aplikacji `ApplicationDbContext` skonfigurowano przy użyciu wywołania metody rozszerzenia [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span><span class="sxs-lookup"><span data-stu-id="c96e7-126">In the sample app, `ApplicationDbContext` is configured with the call to the extension method [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span> <span data-ttu-id="c96e7-127">`ICharacterRepository` jest zarejestrowany jako usługa o określonym zakresie:</span><span class="sxs-lookup"><span data-stu-id="c96e7-127">`ICharacterRepository` is registered as a scoped service:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a><span data-ttu-id="c96e7-128">Wstrzykiwanie wystąpienie repozytorium</span><span class="sxs-lookup"><span data-stu-id="c96e7-128">Inject an instance of the repository</span></span>

<span data-ttu-id="c96e7-129">W klasie, w której wymagana jest dostępu do bazy danych wystąpienie repozytorium jest za pośrednictwem konstruktora i przypisane do pola prywatnego, do użytku przez metody klasy.</span><span class="sxs-lookup"><span data-stu-id="c96e7-129">In a class where database access is required, an instance of the repository is requested via the constructor and assigned to a private field for use by class methods.</span></span> <span data-ttu-id="c96e7-130">W przykładowej aplikacji `ICharacterRepository` służy do:</span><span class="sxs-lookup"><span data-stu-id="c96e7-130">In the sample app, `ICharacterRepository` is used to:</span></span>

* <span data-ttu-id="c96e7-131">Wypełniania bazy danych, jeśli żadne znaki nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="c96e7-131">Populate the database if no characters exist.</span></span>
* <span data-ttu-id="c96e7-132">Uzyskaj listę znaków do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="c96e7-132">Obtain a list of the characters for display.</span></span>

<span data-ttu-id="c96e7-133">Zwróć uwagę, jak kod wywołujący tylko współdziała z implementacją interfejsu `CharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="c96e7-133">Notice how the calling code only interacts with the interface's implementation, `CharacterRepository`.</span></span> <span data-ttu-id="c96e7-134">Wywoływanie kodu nie używa `ApplicationDbContext` bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="c96e7-134">Calling code doesn't use the `ApplicationDbContext` directly:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a><span data-ttu-id="c96e7-135">Interfejs ogólny repozytorium</span><span class="sxs-lookup"><span data-stu-id="c96e7-135">Generic repository interface</span></span>

<span data-ttu-id="c96e7-136">Ten temat i jego przykładową aplikację pokazują najprostszej implementacji wzorca repozytorium, w których jedno repozytorium jest tworzony dla każdego obiektu firm.</span><span class="sxs-lookup"><span data-stu-id="c96e7-136">This topic and its sample app demonstrate the simplest implementation of the repository pattern, where one repository is created for each business object.</span></span> <span data-ttu-id="c96e7-137">Jeśli aplikacja przekroczy kilka obiektów *interfejsu ogólnego repozytorium* może zmniejszyć ilość kodu wymaganą do zaimplementowania wzorca repozytorium.</span><span class="sxs-lookup"><span data-stu-id="c96e7-137">If the app grows beyond a few objects, a *generic repository interface* may reduce the amount of code required to implement the repository pattern.</span></span> <span data-ttu-id="c96e7-138">Aby uzyskać więcej informacji, zobacz [DevIQ: wzorzec repozytorium: ogólny interfejs repozytorium](http://deviq.com/repository-pattern/).</span><span class="sxs-lookup"><span data-stu-id="c96e7-138">For more information, see [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c96e7-139">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c96e7-139">Additional resources</span></span>

* [<span data-ttu-id="c96e7-140">DevIQ: Wzorzec repozytorium</span><span class="sxs-lookup"><span data-stu-id="c96e7-140">DevIQ: Repository Pattern</span></span>](https://deviq.com/repository-pattern/)
* [<span data-ttu-id="c96e7-141">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="c96e7-141">Dependency injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="c96e7-142">Wstrzykiwanie zależności do widoków</span><span class="sxs-lookup"><span data-stu-id="c96e7-142">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="c96e7-143">Wstrzykiwanie zależności do kontrolerów</span><span class="sxs-lookup"><span data-stu-id="c96e7-143">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="c96e7-144">Wstrzykiwanie zależności w programach obsługi wymagań</span><span class="sxs-lookup"><span data-stu-id="c96e7-144">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* [<span data-ttu-id="c96e7-145">Inwersja kontroli kontenerów i wzorzec wstrzykiwania zależności</span><span class="sxs-lookup"><span data-stu-id="c96e7-145">Inversion of Control Containers and the Dependency Injection Pattern</span></span>](https://www.martinfowler.com/articles/injection.html)
