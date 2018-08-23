---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Pozorowanie programu Entity Framework podczas testowania ASP.NET Web API 2 jednostek | Dokumentacja firmy Microsoft
author: tfitzmac
description: Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych dla aplikacji sieci Web API 2, który używa programu Entity Framework. Pokazuje, jak zmodyfikować...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 0bc5ab59583a2be3f889ba05d26c6cda4589057d
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754144"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="48a1f-104">Pozorowanie programu Entity Framework podczas testowania ASP.NET Web API 2 jednostek</span><span class="sxs-lookup"><span data-stu-id="48a1f-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="48a1f-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="48a1f-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="48a1f-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="48a1f-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="48a1f-107">Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych dla aplikacji sieci Web API 2, który używa programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="48a1f-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="48a1f-108">Pokazuje sposób modyfikowania szkieletu kontrolera, aby umożliwić przekazywanie obiekt kontekstu do testowania oraz sposób tworzenia obiektów testowych, które działają z platformą Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="48a1f-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
> 
> <span data-ttu-id="48a1f-109">Aby zapoznać się z wprowadzeniem do testowanie jednostek za pomocą interfejsu API sieci Web platformy ASP.NET, zobacz [testów jednostkowych przy użyciu wzorca ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="48a1f-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
> 
> <span data-ttu-id="48a1f-110">W tym samouczku przyjęto założenie, że znasz podstawowe pojęcia dotyczące środowiska ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="48a1f-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="48a1f-111">Aby uzyskać Samouczek wprowadzający, zobacz [wprowadzenie do wzorca ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="48a1f-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="48a1f-112">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="48a1f-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="48a1f-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="48a1f-113">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="48a1f-114">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="48a1f-114">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="48a1f-115">W tym temacie:</span><span class="sxs-lookup"><span data-stu-id="48a1f-115">In this topic</span></span>

<span data-ttu-id="48a1f-116">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="48a1f-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="48a1f-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="48a1f-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="48a1f-118">Pobierz program code</span><span class="sxs-lookup"><span data-stu-id="48a1f-118">Download code</span></span>](#download)
- [<span data-ttu-id="48a1f-119">Tworzenie aplikacji przy użyciu projektu testu jednostkowego</span><span class="sxs-lookup"><span data-stu-id="48a1f-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="48a1f-120">Tworzenie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="48a1f-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="48a1f-121">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="48a1f-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="48a1f-122">Dodaj wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="48a1f-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="48a1f-123">Instalowanie pakietów NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="48a1f-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="48a1f-124">Tworzenie kontekstu testu</span><span class="sxs-lookup"><span data-stu-id="48a1f-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="48a1f-125">Tworzenie testów</span><span class="sxs-lookup"><span data-stu-id="48a1f-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="48a1f-126">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="48a1f-126">Run tests</span></span>](#runtests)

<span data-ttu-id="48a1f-127">Jeśli wykonano już kroki opisane w [testów jednostkowych przy użyciu wzorca ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), możesz przejść do sekcji [dodać kontrolera](#controller).</span><span class="sxs-lookup"><span data-stu-id="48a1f-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="48a1f-128">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="48a1f-128">Prerequisites</span></span>

<span data-ttu-id="48a1f-129">Program Visual Studio 2017 Community, Professional lub Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="48a1f-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="48a1f-130">Pobierz program code</span><span class="sxs-lookup"><span data-stu-id="48a1f-130">Download code</span></span>

<span data-ttu-id="48a1f-131">Pobierz [projektu ukończona](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="48a1f-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="48a1f-132">Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu i [jednostki testowania wzorca ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) tematu.</span><span class="sxs-lookup"><span data-stu-id="48a1f-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="48a1f-133">Tworzenie aplikacji przy użyciu projektu testu jednostkowego</span><span class="sxs-lookup"><span data-stu-id="48a1f-133">Create application with unit test project</span></span>

<span data-ttu-id="48a1f-134">Możesz utworzyć projekt testów jednostkowych, podczas tworzenia aplikacji lub dodać projekt testów jednostkowych do istniejących aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48a1f-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="48a1f-135">Ten samouczek przedstawia tworzenie projektu testu jednostkowego, podczas tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48a1f-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="48a1f-136">Utwórz nową aplikację sieci Web programu ASP.NET o nazwie **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="48a1f-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="48a1f-137">W systemie windows nowy projekt ASP.NET, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="48a1f-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="48a1f-138">Wybierz **dodać testy jednostkowe** opcji.</span><span class="sxs-lookup"><span data-stu-id="48a1f-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="48a1f-139">Projekt testów jednostkowych ma automatycznie nadawaną nazwę **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="48a1f-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="48a1f-140">Możesz zachować tę nazwę.</span><span class="sxs-lookup"><span data-stu-id="48a1f-140">You can keep this name.</span></span>

![Tworzenie projektu testu jednostkowego](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="48a1f-142">Po utworzeniu aplikacji, zostanie wyświetlony zawiera dwa projekty — **StoreApp** i **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="48a1f-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="48a1f-143">Tworzenie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="48a1f-143">Create the model class</span></span>

<span data-ttu-id="48a1f-144">W projekcie StoreApp Dodaj plik klasy do **modeli** folder o nazwie **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="48a1f-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="48a1f-145">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="48a1f-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="48a1f-146">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="48a1f-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="48a1f-147">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="48a1f-147">Add the controller</span></span>

<span data-ttu-id="48a1f-148">Kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz pozycję **Dodaj** i **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="48a1f-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="48a1f-149">Wybierz kontroler 2 internetowego interfejsu API z akcjami używający narzędzia Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="48a1f-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Dodaj nowy kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="48a1f-151">Ustaw następujące wartości:</span><span class="sxs-lookup"><span data-stu-id="48a1f-151">Set the following values:</span></span>

- <span data-ttu-id="48a1f-152">Nazwa kontrolera: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="48a1f-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="48a1f-153">Klasa modelu: **produktu**</span><span class="sxs-lookup"><span data-stu-id="48a1f-153">Model class: **Product**</span></span>
- <span data-ttu-id="48a1f-154">Klasa kontekstu danych: [wybierz **nowy kontekst danych** przycisk, który wypełnia wartości widoczne poniżej]</span><span class="sxs-lookup"><span data-stu-id="48a1f-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![Określ kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="48a1f-156">Kliknij przycisk **Dodaj** do tworzenia kontrolera przy użyciu automatycznie generowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="48a1f-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="48a1f-157">Kod zawiera metody do tworzenia, pobierania, aktualizowania i usuwania wystąpień klasy produktu.</span><span class="sxs-lookup"><span data-stu-id="48a1f-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="48a1f-158">Poniższy kod przedstawia metodę dla dodawania produktu.</span><span class="sxs-lookup"><span data-stu-id="48a1f-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="48a1f-159">Należy zauważyć, że metoda zwraca wystąpienie **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="48a1f-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="48a1f-160">IHttpActionResult jest jedną z nowych funkcji w sieci Web API 2 i upraszcza tworzenie testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="48a1f-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="48a1f-161">W następnej sekcji dostosujesz wygenerowany kod w celu ułatwienia przekazywanie obiektów testowych do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="48a1f-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="48a1f-162">Dodaj wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="48a1f-162">Add dependency injection</span></span>

<span data-ttu-id="48a1f-163">Obecnie klasy ProductController jest ustalony do korzystania z wystąpienia klasy StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="48a1f-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="48a1f-164">Wzorzec o nazwie wstrzykiwanie zależności użyje do modyfikowania aplikacji i usunąć tej ustalonych zależności.</span><span class="sxs-lookup"><span data-stu-id="48a1f-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="48a1f-165">Dzięki pozbyciu się do tej zależności możesz przekazać makiety obiektu podczas testowania.</span><span class="sxs-lookup"><span data-stu-id="48a1f-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="48a1f-166">Kliknij prawym przyciskiem myszy **modeli** folderze i Dodaj nowy interfejs o nazwie **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="48a1f-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="48a1f-167">Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="48a1f-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="48a1f-168">Otwórz plik StoreAppContext.cs i wprowadź następujące zmiany wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="48a1f-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="48a1f-169">Istotne zmiany, należy pamiętać, są następujące:</span><span class="sxs-lookup"><span data-stu-id="48a1f-169">The important changes to note are:</span></span>

- <span data-ttu-id="48a1f-170">Klasa StoreAppContext implementuje interfejs IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="48a1f-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="48a1f-171">Metoda MarkAsModified jest zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="48a1f-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="48a1f-172">Otwórz plik ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="48a1f-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="48a1f-173">Zmień istniejący kod, aby dopasować wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="48a1f-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="48a1f-174">Te zmiany zepsuć zależności na StoreAppContext i włączyć inne klasy przekazać innego obiektu klasy kontekstu.</span><span class="sxs-lookup"><span data-stu-id="48a1f-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="48a1f-175">Ta zmiana spowoduje włączenie przekazania w kontekście testu podczas testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="48a1f-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="48a1f-176">Istnieje jeden więcej zmian, które należy wykonać w ProductController.</span><span class="sxs-lookup"><span data-stu-id="48a1f-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="48a1f-177">W **PutProduct** metody, Zastąp wiersz, który ustawia stan jednostki można modyfikować za pomocą wywołanie metody MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="48a1f-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="48a1f-178">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="48a1f-178">Build the solution.</span></span>

<span data-ttu-id="48a1f-179">Teraz można przystąpić do konfigurowania projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="48a1f-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="48a1f-180">Instalowanie pakietów NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="48a1f-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="48a1f-181">Gdy używasz pusty szablon do tworzenia aplikacji projektu testu jednostkowego (StoreApp.Tests) nie obejmuje wszystkie zainstalowane pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="48a1f-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="48a1f-182">Inne szablony, takie jak szablon interfejsu API sieci Web obejmują niektóre pakiety NuGet w projekcie testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="48a1f-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="48a1f-183">W tym samouczku musi zawierać pakiet platformy Entity Framework i pakiet Microsoft ASP.NET Web API 2 Core do projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="48a1f-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="48a1f-184">Kliknij prawym przyciskiem myszy projekt StoreApp.Tests, a następnie wybierz pozycję **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="48a1f-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="48a1f-185">Musisz wybrać projekt StoreApp.Tests, aby dodać pakiety do tego projektu.</span><span class="sxs-lookup"><span data-stu-id="48a1f-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Zarządzanie pakietami](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="48a1f-187">Z pakietów w trybie Online należy znaleźć i zainstalować pakiet EntityFramework (w wersji 6.0 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="48a1f-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="48a1f-188">Jeśli wygląda na to, że pakiet EntityFramework jest już zainstalowany, może wybrany projekt StoreApp zamiast StoreApp.Tests projektu.</span><span class="sxs-lookup"><span data-stu-id="48a1f-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Dodawanie programu Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="48a1f-190">Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="48a1f-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Zainstaluj pakiet podstawowego interfejsu api sieci web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="48a1f-192">Zamknij okno Zarządzanie pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="48a1f-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="48a1f-193">Tworzenie kontekstu testu</span><span class="sxs-lookup"><span data-stu-id="48a1f-193">Create test context</span></span>

<span data-ttu-id="48a1f-194">Dodaj klasę o nazwie **TestDbSet** do projektu testu.</span><span class="sxs-lookup"><span data-stu-id="48a1f-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="48a1f-195">Ta klasa służy jako klasa bazowa dla zestawu danych testowych.</span><span class="sxs-lookup"><span data-stu-id="48a1f-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="48a1f-196">Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="48a1f-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="48a1f-197">Dodaj klasę o nazwie **TestProductDbSet** do projektu testu, który zawiera następujący kod.</span><span class="sxs-lookup"><span data-stu-id="48a1f-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="48a1f-198">Dodaj klasę o nazwie **TestStoreAppContext** i Zastąp istniejący kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="48a1f-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="48a1f-199">Tworzenie testów</span><span class="sxs-lookup"><span data-stu-id="48a1f-199">Create tests</span></span>

<span data-ttu-id="48a1f-200">Domyślnie projektu testowego zawiera plik pusty test o nazwie **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="48a1f-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="48a1f-201">Ten plik zawiera atrybuty, że umożliwia tworzenie metod testowych.</span><span class="sxs-lookup"><span data-stu-id="48a1f-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="48a1f-202">Na potrzeby tego samouczka możesz usunąć ten plik, ponieważ spowoduje dodanie nowej klasy testowej.</span><span class="sxs-lookup"><span data-stu-id="48a1f-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="48a1f-203">Dodaj klasę o nazwie **TestProductController** do projektu testu.</span><span class="sxs-lookup"><span data-stu-id="48a1f-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="48a1f-204">Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="48a1f-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="48a1f-205">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="48a1f-205">Run tests</span></span>

<span data-ttu-id="48a1f-206">Teraz można przystąpić do uruchomienia testów.</span><span class="sxs-lookup"><span data-stu-id="48a1f-206">You are now ready to run the tests.</span></span> <span data-ttu-id="48a1f-207">Wszystkie metody, które są oznaczone **TestMethod** atrybut, który będzie testowany.</span><span class="sxs-lookup"><span data-stu-id="48a1f-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="48a1f-208">Z **testu** elementu menu, uruchom testy.</span><span class="sxs-lookup"><span data-stu-id="48a1f-208">From the **Test** menu item, run the tests.</span></span>

![uruchom testy](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="48a1f-210">Otwórz **Eksplorator testów** okna i zwróć uwagę, wyniki testów.</span><span class="sxs-lookup"><span data-stu-id="48a1f-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![wyniki testu](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
