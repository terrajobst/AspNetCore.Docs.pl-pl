---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Mocking programu Entity Framework podczas testowania składnika ASP.NET Web API 2 jednostek | Dokumentacja firmy Microsoft
author: tfitzmac
description: Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych dla aplikacji sieci Web API 2, który używa programu Entity Framework. Widoczny jest sposób modyfikowania...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: abfde7edec85812de3560f4edefb110c3e374580
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152868"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="f6208-104">Mocking programu Entity Framework podczas testowania składnika ASP.NET Web API 2 jednostek</span><span class="sxs-lookup"><span data-stu-id="f6208-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f6208-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f6208-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="f6208-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="f6208-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="f6208-107">Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych dla aplikacji sieci Web API 2, który używa programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f6208-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="f6208-108">Widoczny jest sposób modyfikowania szkieletu kontrolera, aby umożliwić przekazywanie obiektu kontekstu do testowania oraz sposobu tworzenia obiektów testu, które współpracują z programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f6208-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
> 
> <span data-ttu-id="f6208-109">Aby obejrzeć wprowadzenie do testy jednostkowe za pomocą interfejsu API sieci Web platformy ASP.NET, zobacz [testów jednostkowych z ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f6208-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
> 
> <span data-ttu-id="f6208-110">W tym samouczku założono, że czytelnik zna podstawowe pojęcia związane z interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f6208-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="f6208-111">Samouczek wprowadzający, zobacz [wprowadzenie do korzystania z programu ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f6208-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f6208-112">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="f6208-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f6208-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f6208-113">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="f6208-114">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="f6208-114">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="f6208-115">W tym temacie:</span><span class="sxs-lookup"><span data-stu-id="f6208-115">In this topic</span></span>

<span data-ttu-id="f6208-116">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="f6208-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f6208-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f6208-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="f6208-118">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="f6208-118">Download code</span></span>](#download)
- [<span data-ttu-id="f6208-119">Tworzenie aplikacji z jednostkowy projekt testowy</span><span class="sxs-lookup"><span data-stu-id="f6208-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="f6208-120">Utwórz klasę modelu</span><span class="sxs-lookup"><span data-stu-id="f6208-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="f6208-121">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="f6208-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="f6208-122">Dodaj iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="f6208-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="f6208-123">Instalowanie pakietów NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="f6208-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="f6208-124">Tworzenie kontekstu testu</span><span class="sxs-lookup"><span data-stu-id="f6208-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="f6208-125">Tworzenie testów</span><span class="sxs-lookup"><span data-stu-id="f6208-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="f6208-126">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="f6208-126">Run tests</span></span>](#runtests)

<span data-ttu-id="f6208-127">Jeśli wykonano już kroki opisane w [testów jednostkowych z ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), możesz przejść do sekcji [dodać kontroler](#controller).</span><span class="sxs-lookup"><span data-stu-id="f6208-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="f6208-128">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f6208-128">Prerequisites</span></span>

<span data-ttu-id="f6208-129">Visual Studio 2017 Community, Professional or Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="f6208-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="f6208-130">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="f6208-130">Download code</span></span>

<span data-ttu-id="f6208-131">Pobierz [projektu zakończone](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="f6208-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="f6208-132">Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu i [jednostek testowania ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) tematu.</span><span class="sxs-lookup"><span data-stu-id="f6208-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="f6208-133">Tworzenie aplikacji z jednostkowy projekt testowy</span><span class="sxs-lookup"><span data-stu-id="f6208-133">Create application with unit test project</span></span>

<span data-ttu-id="f6208-134">Możesz utworzyć jednostkowy projekt testowy, podczas tworzenia aplikacji lub Dodaj jednostkowy projekt testowy do istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6208-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="f6208-135">Ten samouczek przedstawia tworzenie jednostkowy projekt testowy, podczas tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f6208-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="f6208-136">Tworzenie nowej aplikacji sieci Web programu ASP.NET o nazwie **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="f6208-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="f6208-137">W systemie windows nowy projekt ASP.NET, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f6208-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="f6208-138">Wybierz **Dodaj testy jednostkowe** opcji.</span><span class="sxs-lookup"><span data-stu-id="f6208-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="f6208-139">Automatycznie nosi nazwę jednostkowy projekt testowy **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="f6208-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="f6208-140">Możesz pozostawić tę nazwę.</span><span class="sxs-lookup"><span data-stu-id="f6208-140">You can keep this name.</span></span>

![Tworzenie projektu testu jednostki](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="f6208-142">Po utworzeniu aplikacji będą widzieli zawiera dwa projekty — **StoreApp** i **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="f6208-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="f6208-143">Utwórz klasę modelu</span><span class="sxs-lookup"><span data-stu-id="f6208-143">Create the model class</span></span>

<span data-ttu-id="f6208-144">W projekcie StoreApp dodać plik klasy do **modele** folder o nazwie **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="f6208-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="f6208-145">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="f6208-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="f6208-146">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="f6208-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="f6208-147">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="f6208-147">Add the controller</span></span>

<span data-ttu-id="f6208-148">Kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz **Dodaj** i **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="f6208-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="f6208-149">Wybierz kontroler programu Web API 2 z akcjami używający narzędzia Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f6208-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Dodawanie nowego kontrolera](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="f6208-151">Ustaw następujące wartości:</span><span class="sxs-lookup"><span data-stu-id="f6208-151">Set the following values:</span></span>

- <span data-ttu-id="f6208-152">Nazwa kontrolera: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="f6208-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="f6208-153">Klasa modelu: **produktu**</span><span class="sxs-lookup"><span data-stu-id="f6208-153">Model class: **Product**</span></span>
- <span data-ttu-id="f6208-154">Klasa kontekstu danych: [wybierz **nowy kontekst danych** przycisku, wypełnia wartości przedstawione poniżej]</span><span class="sxs-lookup"><span data-stu-id="f6208-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![Określ kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="f6208-156">Kliknij przycisk **Dodaj** do utworzenia kontrolera automatycznie wygenerowany kod.</span><span class="sxs-lookup"><span data-stu-id="f6208-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="f6208-157">Kod zawiera metody tworzenia, pobierania, aktualizowania i usuwania wystąpienia klasy produktu.</span><span class="sxs-lookup"><span data-stu-id="f6208-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="f6208-158">Poniższy kod przedstawia metody do dodawania produktu.</span><span class="sxs-lookup"><span data-stu-id="f6208-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="f6208-159">Należy zauważyć, że ta metoda zwraca wystąpienie **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="f6208-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="f6208-160">IHttpActionResult jest jedną z nowych funkcji w sieci Web API 2, a jego upraszcza tworzenie testów jednostek.</span><span class="sxs-lookup"><span data-stu-id="f6208-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="f6208-161">W następnej sekcji, możesz dostosować wygenerowany kod w celu ułatwienia przekazywanie obiektów testu na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="f6208-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="f6208-162">Dodaj iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="f6208-162">Add dependency injection</span></span>

<span data-ttu-id="f6208-163">Klasa ProductController jest obecnie ustalony Aby użyć wystąpienia klasy StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="f6208-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="f6208-164">Do modyfikowania aplikacji i tej zależności z wpisaną na stałe usunąć użyjesz wzorzec o nazwie iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="f6208-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="f6208-165">Dzięki pozbyciu się tym zależności można przekazać w obiekcie testowych podczas testowania.</span><span class="sxs-lookup"><span data-stu-id="f6208-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="f6208-166">Kliknij prawym przyciskiem myszy **modele** folderu i Dodaj nowy interfejs o nazwie **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="f6208-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="f6208-167">Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="f6208-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="f6208-168">Otwórz plik StoreAppContext.cs i wprowadź następujące zmiany zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="f6208-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="f6208-169">Ważne zmiany, należy pamiętać, są następujące:</span><span class="sxs-lookup"><span data-stu-id="f6208-169">The important changes to note are:</span></span>

- <span data-ttu-id="f6208-170">Klasa StoreAppContext implementuje interfejs IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="f6208-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="f6208-171">Metoda MarkAsModified jest zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="f6208-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="f6208-172">Otwórz plik ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="f6208-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="f6208-173">Zmień istniejący kod, aby dopasować wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="f6208-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="f6208-174">Te zmiany zepsuć zależności na StoreAppContext i Włącz innych klas umożliwia przekazywanie dla klasy kontekstu na inny obiekt.</span><span class="sxs-lookup"><span data-stu-id="f6208-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="f6208-175">Ta zmiana pozwoli podczas testów jednostkowych należy przekazać w kontekście testu.</span><span class="sxs-lookup"><span data-stu-id="f6208-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="f6208-176">Brak jednej więcej zmiany, które należy wykonać w ProductController.</span><span class="sxs-lookup"><span data-stu-id="f6208-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="f6208-177">W **PutProduct** metoda, Zastąp wiersza, która ustawia stan jednostki zmodyfikowane przez wywołanie do metody MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="f6208-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="f6208-178">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="f6208-178">Build the solution.</span></span>

<span data-ttu-id="f6208-179">Teraz można przystąpić do konfigurowania projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="f6208-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="f6208-180">Instalowanie pakietów NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="f6208-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="f6208-181">Użycie pustego szablonu do tworzenia aplikacji, jednostkowy projekt testowy (StoreApp.Tests) nie zawiera żadnych zainstalowanych pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="f6208-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="f6208-182">Inne szablony, takie jak szablon interfejsu API sieci Web obejmują niektórych pakietów NuGet w jednostkowy projekt testowy.</span><span class="sxs-lookup"><span data-stu-id="f6208-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="f6208-183">W tym samouczku muszą zawierać packge Entity Framework i pakietu Microsoft ASP.NET Web API 2 Core do projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="f6208-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="f6208-184">Kliknij prawym przyciskiem myszy projekt StoreApp.Tests i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f6208-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="f6208-185">Należy wybrać projekt StoreApp.Tests dodawania pakietów do tego projektu.</span><span class="sxs-lookup"><span data-stu-id="f6208-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Zarządzaj pakietami](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="f6208-187">Z pakietów w trybie Online Znajdź i zainstaluj pakiet EntityFramework (w wersji 6.0 lub nowszej).</span><span class="sxs-lookup"><span data-stu-id="f6208-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="f6208-188">Jeśli wygląda na to, że pakiet EntityFramework jest już zainstalowany, być może wybrano projektu StoreApp zamiast StoreApp.Tests projektu.</span><span class="sxs-lookup"><span data-stu-id="f6208-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Dodaj Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="f6208-190">Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="f6208-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Zainstaluj pakiet podstawowy interfejs api sieci web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="f6208-192">Zamknij okno Zarządzanie pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="f6208-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="f6208-193">Tworzenie kontekstu testu</span><span class="sxs-lookup"><span data-stu-id="f6208-193">Create test context</span></span>

<span data-ttu-id="f6208-194">Dodaj klasę o nazwie **TestDbSet** do projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="f6208-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="f6208-195">Ta klasa służy jako klasa podstawowa dla użytkownika testowego zestawu danych.</span><span class="sxs-lookup"><span data-stu-id="f6208-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="f6208-196">Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="f6208-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="f6208-197">Dodaj klasę o nazwie **TestProductDbSet** do projektu testowego, który zawiera następujący kod.</span><span class="sxs-lookup"><span data-stu-id="f6208-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="f6208-198">Dodaj klasę o nazwie **TestStoreAppContext** i Zastąp istniejący kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="f6208-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="f6208-199">Tworzenie testów</span><span class="sxs-lookup"><span data-stu-id="f6208-199">Create tests</span></span>

<span data-ttu-id="f6208-200">Domyślnie projekt testowy zawiera plik pusty test o nazwie **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="f6208-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="f6208-201">Ten plik zawiera atrybuty się, że jest używany do utworzenia metody testowe.</span><span class="sxs-lookup"><span data-stu-id="f6208-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="f6208-202">W tym samouczku można usunąć tego pliku, ponieważ doda nową klasę testów.</span><span class="sxs-lookup"><span data-stu-id="f6208-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="f6208-203">Dodaj klasę o nazwie **TestProductController** do projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="f6208-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="f6208-204">Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="f6208-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="f6208-205">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="f6208-205">Run tests</span></span>

<span data-ttu-id="f6208-206">Teraz można przystąpić do uruchomienia testów.</span><span class="sxs-lookup"><span data-stu-id="f6208-206">You are now ready to run the tests.</span></span> <span data-ttu-id="f6208-207">Wszystkie metody, które są oznaczone ikoną z **TestMethod** atrybut zostanie przetestowana.</span><span class="sxs-lookup"><span data-stu-id="f6208-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="f6208-208">Z **testu** element menu, uruchom testy.</span><span class="sxs-lookup"><span data-stu-id="f6208-208">From the **Test** menu item, run the tests.</span></span>

![uruchom testy](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="f6208-210">Otwórz **Eksploratora testów** okna i zwróć uwagę, wyniki testów.</span><span class="sxs-lookup"><span data-stu-id="f6208-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![wyniki testu](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
