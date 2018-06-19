---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: ASP.NET Web API 2 testy jednostkowe | Dokumentacja firmy Microsoft
author: tfitzmac
description: Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych proste dla aplikacji sieci Web API 2. W tym samouczku pokazano, jak dołączyć proj testu jednostki...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d6102dd81589e41894d8ecd95bf9ddd761a65bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042748"
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="e5ca7-104">ASP.NET Web API 2 testy jednostkowe</span><span class="sxs-lookup"><span data-stu-id="e5ca7-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="e5ca7-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e5ca7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="e5ca7-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="e5ca7-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="e5ca7-107">Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych proste dla aplikacji sieci Web API 2.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="e5ca7-108">W tym samouczku pokazano, jak dołączyć jednostkowy projekt testowy do rozwiązania, a zapisu metody testowe, które są zwracane wartości z metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="e5ca7-109">W tym samouczku założono, że czytelnik zna podstawowe pojęcia związane z interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="e5ca7-110">Samouczek wprowadzający, zobacz [wprowadzenie do korzystania z programu ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e5ca7-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="e5ca7-111">Testy jednostkowe w tym temacie są celowo ograniczone do scenariuszy proste danych.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="e5ca7-112">W celu testowania bardziej zaawansowanych scenariuszy danych, zobacz [Mocking Entity Framework podczas jednostki testowania ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="e5ca7-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e5ca7-113">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="e5ca7-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="e5ca7-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e5ca7-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="e5ca7-115">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="e5ca7-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="e5ca7-116">W tym temacie:</span><span class="sxs-lookup"><span data-stu-id="e5ca7-116">In this topic</span></span>

<span data-ttu-id="e5ca7-117">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="e5ca7-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="e5ca7-118">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e5ca7-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="e5ca7-119">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="e5ca7-119">Download code</span></span>](#download)
- [<span data-ttu-id="e5ca7-120">Tworzenie aplikacji z jednostkowy projekt testowy</span><span class="sxs-lookup"><span data-stu-id="e5ca7-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="e5ca7-121">Dodaj jednostkowy projekt testowy, podczas tworzenia aplikacji</span><span class="sxs-lookup"><span data-stu-id="e5ca7-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="e5ca7-122">Dodaj jednostkowy projekt testowy do istniejącej aplikacji</span><span class="sxs-lookup"><span data-stu-id="e5ca7-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="e5ca7-123">Konfigurowanie aplikacji sieci Web API 2</span><span class="sxs-lookup"><span data-stu-id="e5ca7-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="e5ca7-124">Instalowanie pakietów NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="e5ca7-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="e5ca7-125">Tworzenie testów</span><span class="sxs-lookup"><span data-stu-id="e5ca7-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="e5ca7-126">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="e5ca7-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="e5ca7-127">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="e5ca7-127">Prerequisites</span></span>

<span data-ttu-id="e5ca7-128">Visual Studio 2017 Community, Professional or Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="e5ca7-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="e5ca7-129">Pobierz kod</span><span class="sxs-lookup"><span data-stu-id="e5ca7-129">Download code</span></span>

<span data-ttu-id="e5ca7-130">Pobierz [projektu zakończone](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="e5ca7-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="e5ca7-131">Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu i [Mocking Entity Framework podczas jednostki testowania składnika ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) tematu.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="e5ca7-132">Tworzenie aplikacji z jednostkowy projekt testowy</span><span class="sxs-lookup"><span data-stu-id="e5ca7-132">Create application with unit test project</span></span>

<span data-ttu-id="e5ca7-133">Możesz utworzyć jednostkowy projekt testowy, podczas tworzenia aplikacji lub Dodaj jednostkowy projekt testowy do istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="e5ca7-134">Ten samouczek pokazuje obie metody tworzenia jednostkowy projekt testowy.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="e5ca7-135">Aby użyć w tym samouczku, można użyć jednej metody.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="e5ca7-136">Dodaj jednostkowy projekt testowy, podczas tworzenia aplikacji</span><span class="sxs-lookup"><span data-stu-id="e5ca7-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="e5ca7-137">Tworzenie nowej aplikacji sieci Web programu ASP.NET o nazwie **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Tworzenie projektu](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="e5ca7-139">W systemie windows nowy projekt ASP.NET, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="e5ca7-140">Wybierz **Dodaj testy jednostkowe** opcji.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="e5ca7-141">Automatycznie nosi nazwę jednostkowy projekt testowy **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="e5ca7-142">Możesz pozostawić tę nazwę.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-142">You can keep this name.</span></span>

![Tworzenie projektu testu jednostki](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="e5ca7-144">Po utworzeniu aplikacji, zobaczysz, że zawiera dwa projekty.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-144">After creating the application, you will see it contains two projects.</span></span>

![dwa projekty](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="e5ca7-146">Dodaj jednostkowy projekt testowy do istniejącej aplikacji</span><span class="sxs-lookup"><span data-stu-id="e5ca7-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="e5ca7-147">Jeśli nie utworzono jednostkowy projekt testowy podczas tworzenia aplikacji, możesz dodać kategorię, w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="e5ca7-148">Na przykład, załóżmy, że masz już aplikację o nazwie StoreApp, i chcesz dodać testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="e5ca7-149">Aby dodać jednostkowy projekt testowy, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj** i **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Dodaj nowy projekt do rozwiązania](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="e5ca7-151">Wybierz **testu** w okienku po lewej stronie, a następnie wybierz **jednostkowy projekt testowy** dla typu projektu.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="e5ca7-152">Nazwij projekt **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-152">Name the project **StoreApp.Tests**.</span></span>

![Dodaj jednostkowy projekt testowy](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="e5ca7-154">Zobaczysz jednostkowy projekt testowy w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="e5ca7-155">W jednostkowy projekt testowy Dodaj odwołanie projektu do oryginalnego projektu.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="e5ca7-156">Konfigurowanie aplikacji sieci Web API 2</span><span class="sxs-lookup"><span data-stu-id="e5ca7-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="e5ca7-157">W projekcie StoreApp dodać plik klasy do **modele** folder o nazwie **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="e5ca7-158">Zastąp zawartość pliku następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="e5ca7-159">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-159">Build the solution.</span></span>

<span data-ttu-id="e5ca7-160">Kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz **Dodaj** i **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="e5ca7-161">Wybierz **pusty kontroler — interfejs API sieci Web 2**.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-161">Select **Web API 2 Controller - Empty**.</span></span>

![Dodawanie nowego kontrolera](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="e5ca7-163">Ustaw nazwę kontrolera **SimpleProductController**i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Określ kontroler](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="e5ca7-165">Zastąp istniejący kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="e5ca7-166">Aby uprościć w tym przykładzie, dane są przechowywane w listy, a nie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="e5ca7-167">Lista zdefiniowane w tej klasie reprezentuje danych produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="e5ca7-168">Zwróć uwagę, że kontroler zawiera konstruktora, który przyjmuje jako parametr listę obiektów produktu.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="e5ca7-169">Ten konstruktor umożliwia przekazywanie danych testowych podczas testowania jednostek.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="e5ca7-170">Kontroler obejmuje również dwa **async** metod w celu zilustrowania testowaniu metod asynchronicznych jednostki.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="e5ca7-171">Te metody asynchroniczne zostały zaimplementowane przez wywołanie metody **Task.FromResult** aby zminimalizować nadmiarowe kod, ale zwykle metody obejmuje operacje wymagają dużej ilości zasobów.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="e5ca7-172">Metoda GetProduct Zwraca wystąpienie klasy **IHttpActionResult** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="e5ca7-173">IHttpActionResult jest jedną z nowych funkcji w sieci Web API 2, a jego upraszcza tworzenie testów jednostek.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="e5ca7-174">Klasy, które implementują interfejs IHttpActionResult znajdują się w [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="e5ca7-175">Te klasy reprezentuje możliwe odpowiedzi z akcji żądania i odpowiadają one kody stanu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="e5ca7-176">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-176">Build the solution.</span></span>

<span data-ttu-id="e5ca7-177">Teraz można przystąpić do konfigurowania projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="e5ca7-178">Instalowanie pakietów NuGet w projekcie testowym</span><span class="sxs-lookup"><span data-stu-id="e5ca7-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="e5ca7-179">Użycie pustego szablonu do tworzenia aplikacji, jednostkowy projekt testowy (StoreApp.Tests) nie zawiera żadnych zainstalowanych pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="e5ca7-180">Inne szablony, takie jak szablon interfejsu API sieci Web obejmują niektórych pakietów NuGet w jednostkowy projekt testowy.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="e5ca7-181">W tym samouczku musi zawierać pakietu Microsoft ASP.NET Web API 2 Core do projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="e5ca7-182">Kliknij prawym przyciskiem myszy projekt StoreApp.Tests i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="e5ca7-183">Należy wybrać projekt StoreApp.Tests dodawania pakietów do tego projektu.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![Zarządzaj pakietami](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="e5ca7-185">Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![Zainstaluj pakiet podstawowy interfejs api sieci web](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="e5ca7-187">Zamknij okno Zarządzanie pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="e5ca7-188">Tworzenie testów</span><span class="sxs-lookup"><span data-stu-id="e5ca7-188">Create tests</span></span>

<span data-ttu-id="e5ca7-189">Domyślnie projekt testowy zawiera plik pusty test o nazwie UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="e5ca7-190">Ten plik zawiera atrybuty się, że jest używany do utworzenia metody testowe.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="e5ca7-191">Dla testów jednostkowych możesz użyć tego pliku lub utworzyć własny plik.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="e5ca7-193">W tym samouczku utworzysz własnego klasy testowej.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="e5ca7-194">Można usunąć pliku UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="e5ca7-195">Dodaj klasę o nazwie **TestSimpleProductController.cs**i Zastąp kod następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="e5ca7-196">Uruchom testy</span><span class="sxs-lookup"><span data-stu-id="e5ca7-196">Run tests</span></span>

<span data-ttu-id="e5ca7-197">Teraz można przystąpić do uruchomienia testów.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-197">You are now ready to run the tests.</span></span> <span data-ttu-id="e5ca7-198">Wszystkie metody, które są oznaczone ikoną z **TestMethod** atrybut zostanie przetestowana.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="e5ca7-199">Z **testu** element menu, uruchom testy.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-199">From the **Test** menu item, run the tests.</span></span>

![uruchom testy](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="e5ca7-201">Otwórz **Eksploratora testów** okna i zwróć uwagę, wyniki testów.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![wyniki testu](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="e5ca7-203">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="e5ca7-203">Summary</span></span>

<span data-ttu-id="e5ca7-204">W tym samouczku została ukończona.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-204">You have completed this tutorial.</span></span> <span data-ttu-id="e5ca7-205">Dane w tym samouczku został celowo uproszczona skoncentrować się na warunki testowania jednostek.</span><span class="sxs-lookup"><span data-stu-id="e5ca7-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="e5ca7-206">W celu testowania bardziej zaawansowanych scenariuszy danych, zobacz [Mocking Entity Framework podczas jednostki testowania ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="e5ca7-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
