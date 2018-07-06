---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Wywoływanie interfejsu API sieci Web z Windows Phone 8 aplikacji (C#) | Dokumentacja firmy Microsoft
author: rmcmurray
description: Utwórz całego scenariusza end-to-end, składający się z aplikacji interfejsu API sieci Web platformy ASP.NET, która zawiera katalog książek do aplikacji Windows Phone 8.
ms.author: aspnetcontent
ms.date: 10/09/2013
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 6b7a833818424cbf3a3bf9e1e14e5b2864742c38
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805045"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="a64b5-103">Wywoływanie interfejsu API sieci Web z aplikacji Windows Phone 8 (C#)</span><span class="sxs-lookup"><span data-stu-id="a64b5-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="a64b5-104">przez [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="a64b5-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="a64b5-105">W tym samouczku dowiesz się, jak utworzyć całego scenariusza end-to-end, składający się z aplikacji interfejsu API sieci Web platformy ASP.NET, która zawiera katalog książek do aplikacji Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="a64b5-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="a64b5-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="a64b5-106">Overview</span></span>

<span data-ttu-id="a64b5-107">RESTful usług, takich jak ASP.NET Web API upraszczają tworzenie aplikacji opartych na protokole HTTP dla deweloperów, zapewniając abstrakcyjność architektury dla aplikacji po stronie serwera i klienta.</span><span class="sxs-lookup"><span data-stu-id="a64b5-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="a64b5-108">Zamiast tworzyć własności protokołu na podstawie gniazda do komunikacji, internetowego interfejsu API deweloperzy muszą po prostu publikowania wymaganego metody HTTP w swojej aplikacji, (na przykład: GET, POST, PUT, DELETE), a deweloperzy aplikacji klienckich tylko muszą korzystać z metody HTTP, które są niezbędne do ich aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a64b5-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="a64b5-109">W tym samouczku end-to-end dowiesz się, jak używać interfejsu API sieci Web do tworzenia następujących projektów:</span><span class="sxs-lookup"><span data-stu-id="a64b5-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="a64b5-110">W [pierwszej części tego samouczka](#STEP1), utworzy aplikację interfejsu API sieci Web platformy ASP.NET, która obsługuje wszystkie operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) do zarządzania katalogiem książki.</span><span class="sxs-lookup"><span data-stu-id="a64b5-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="a64b5-111">Ta aplikacja będzie używać [przykładowy plik XML (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) z witryny MSDN.</span><span class="sxs-lookup"><span data-stu-id="a64b5-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="a64b5-112">W [druga część tego samouczka](#STEP2), utworzy interaktywną aplikację Windows Phone 8, która pobiera dane z aplikacji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a64b5-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="a64b5-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a64b5-113">Prerequisites</span></span>

- <span data-ttu-id="a64b5-114">Visual Studio 2013 z zainstalowany zestaw Windows Phone 8 SDK</span><span class="sxs-lookup"><span data-stu-id="a64b5-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="a64b5-115">Windows 8 lub nowszy na 64-bitowym systemie przy użyciu funkcji Hyper-V jest zainstalowana</span><span class="sxs-lookup"><span data-stu-id="a64b5-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="a64b5-116">Aby uzyskać listę dodatkowych wymagań, zobacz *wymagania systemowe* sekcji na [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) strony pobierania.</span><span class="sxs-lookup"><span data-stu-id="a64b5-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="a64b5-117">Jeśli użytkownik chce przetestować łączność między interfejsu API sieci Web i projektów Windows Phone 8 w systemie lokalnym, konieczne będzie postępuj zgodnie z instrukcjami w *[nawiązywania połączenia aplikacji interfejsu API sieci Web na lokalnym Emulator Windows Phone 8 Komputer](https://go.microsoft.com/fwlink/?LinkId=324014)* artykuł, aby skonfigurować środowiska testowego.</span><span class="sxs-lookup"><span data-stu-id="a64b5-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="a64b5-118">Krok 1: Tworzenie projektu księgarni interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="a64b5-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="a64b5-119">Pierwszym krokiem w tym samouczku end-to-end jest utworzenie projektu składnika Web API, która obsługuje wszystkie operacje CRUD; należy pamiętać, że doda projekt aplikacji Windows Phone w tym rozwiązaniu [kroku 2](#STEP2) po ukończeniu tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="a64b5-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="a64b5-120">Otwórz **programu Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="a64b5-121">Kliknij przycisk **pliku**, następnie **nowe**, a następnie **projektu**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="a64b5-122">Gdy **nowy projekt** zostanie wyświetlone okno dialogowe, rozwiń **zainstalowane**, następnie **szablony**, następnie **Visual C#**, a następnie **Web**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="a64b5-123">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="a64b5-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="a64b5-124">Wyróżnij **aplikacji sieci Web ASP.NET**, wprowadź **księgarni** nazwę projektu, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="a64b5-125">Gdy **nowy projekt ASP.NET** zostanie wyświetlone okno dialogowe, wybierz **interfejsu API sieci Web** szablonu, a następnie kliknij **OK**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="a64b5-126">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="a64b5-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="a64b5-127">Po otwarciu projektu składnika Web API, należy usunąć kontroler próbki z projektu:</span><span class="sxs-lookup"><span data-stu-id="a64b5-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="a64b5-128">Rozwiń **kontrolerów** folder w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="a64b5-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="a64b5-129">Kliknij prawym przyciskiem myszy **ValuesController.cs** pliku, a następnie kliknij przycisk **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="a64b5-130">Kliknij przycisk **OK** po wyświetleniu monitu o potwierdzenie usunięcia.</span><span class="sxs-lookup"><span data-stu-id="a64b5-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="a64b5-131">Dodawanie pliku danych XML do projektu interfejsu API sieci Web; Ten plik zawiera zawartość katalogu księgarni:</span><span class="sxs-lookup"><span data-stu-id="a64b5-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="a64b5-132">Kliknij prawym przyciskiem myszy **aplikacji\_danych** folder w Eksploratorze rozwiązań, następnie kliknij przycisk **Dodaj**, a następnie kliknij przycisk **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="a64b5-133">Gdy **Dodaj nowy element** zostanie wyświetlone okno dialogowe, zaznacz **pliku XML** szablonu.</span><span class="sxs-lookup"><span data-stu-id="a64b5-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="a64b5-134">Nadaj plikowi nazwę **Books.xml**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="a64b5-135">Gdy **Books.xml** plik jest otwarty, Zastąp kod w pliku XML z próbki **books.xml** pliku w witrynie MSDN:</span><span class="sxs-lookup"><span data-stu-id="a64b5-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="a64b5-136">Zapisz i zamknij plik XML.</span><span class="sxs-lookup"><span data-stu-id="a64b5-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="a64b5-137">Dodawanie modelu księgarni do projektu interfejsu API sieci Web; Ten model zawiera logikę aplikacji księgarni tworzenia, odczytu, aktualizowania lub usuwania (CRUD):</span><span class="sxs-lookup"><span data-stu-id="a64b5-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="a64b5-138">Kliknij prawym przyciskiem myszy **modeli** folder w Eksploratorze rozwiązań, następnie kliknij przycisk **Dodaj**, a następnie kliknij przycisk **klasy**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="a64b5-139">Gdy **Dodaj nowy element** zostanie wyświetlone okno dialogowe, określ nazwę pliku klasy **BookDetails.cs**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="a64b5-140">Gdy **BookDetails.cs** plik jest otwarty, Zastąp kod w pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a64b5-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="a64b5-141">Zapisz i Zamknij **BookDetails.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="a64b5-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="a64b5-142">Dodaj kontroler księgarni z projektem interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="a64b5-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="a64b5-143">Kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań, następnie kliknij przycisk **Dodaj**, a następnie kliknij przycisk **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="a64b5-144">Gdy **Dodawanie szkieletu** zostanie wyświetlone okno dialogowe, zaznacz **kontroler internetowego interfejsu API 2 — pusty**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="a64b5-145">Gdy **Dodaj kontroler** zostanie wyświetlone okno dialogowe, określ nazwę kontrolera **BooksController**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="a64b5-146">Gdy **BooksController.cs** plik jest otwarty, Zastąp kod w pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a64b5-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="a64b5-147">Zapisz i Zamknij **BooksController.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="a64b5-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="a64b5-148">Utwórz aplikację interfejsu API sieci Web, aby sprawdzić błędy.</span><span class="sxs-lookup"><span data-stu-id="a64b5-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="a64b5-149">Krok 2: Dodawanie projektu wykazu systemu Windows Phone 8 księgarni</span><span class="sxs-lookup"><span data-stu-id="a64b5-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="a64b5-150">Następnym krokiem w tym scenariuszu end-to-end jest utworzyć katalogu aplikacji dla Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="a64b5-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="a64b5-151">Ta aplikacja będzie używać *Windows Phone z danymi aplikacji* szablon domyślny interfejs użytkownika który będzie używać aplikacji internetowego interfejsu API, który został utworzony w [kroku 1](#STEP1) po ukończeniu tego samouczka jako źródło danych.</span><span class="sxs-lookup"><span data-stu-id="a64b5-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="a64b5-152">Kliknij prawym przyciskiem myszy **księgarni** rozwiązania w Eksploratorze rozwiązań, następnie kliknij przycisk **Dodaj**, a następnie **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="a64b5-153">Gdy **nowy projekt** zostanie wyświetlone okno dialogowe, rozwiń **zainstalowane**, następnie **Visual C#**, a następnie **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="a64b5-154">Wyróżnij **Windows Phone z danymi aplikacji**, wprowadź **BookCatalog** nazwę, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="a64b5-155">Dodaj pakiet NuGet struktury Json.NET do **BookCatalog** projektu:</span><span class="sxs-lookup"><span data-stu-id="a64b5-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="a64b5-156">Kliknij prawym przyciskiem myszy **odwołania** dla **BookCatalog** projektu w Eksploratorze rozwiązań, a następnie kliknij przycisk **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="a64b5-157">Gdy **Zarządzaj pakietami NuGet** zostanie wyświetlone okno dialogowe, rozwiń **Online** sekcji, a następnie zaznacz **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="a64b5-158">Wprowadź **Json.NET** w wyszukiwaniu pola, a następnie kliknij ikonę wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="a64b5-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="a64b5-159">Wyróżnij **Json.NET** w wynikach wyszukiwania, a następnie kliknij **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="a64b5-160">Po zakończeniu instalacji kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="a64b5-161">Dodaj **BookDetails** modelu do **BookCatalog** projektu; zawiera model ogólny klasy księgarni:</span><span class="sxs-lookup"><span data-stu-id="a64b5-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="a64b5-162">Kliknij prawym przyciskiem myszy **BookCatalog** projektu w Eksploratorze rozwiązań, a następnie kliknij przycisk **Dodaj**, a następnie kliknij przycisk **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="a64b5-163">Nadaj nazwę nowego folderu **modeli**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="a64b5-164">Kliknij prawym przyciskiem myszy **modeli** folder w Eksploratorze rozwiązań, następnie kliknij przycisk **Dodaj**, a następnie kliknij przycisk **klasy**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="a64b5-165">Gdy **Dodaj nowy element** zostanie wyświetlone okno dialogowe, określ nazwę pliku klasy **BookDetails.cs**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="a64b5-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="a64b5-166">Gdy **BookDetails.cs** plik jest otwarty, Zastąp kod w pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a64b5-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="a64b5-167">Zapisz i Zamknij **BookDetails.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="a64b5-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="a64b5-168">Aktualizacja **MainViewModel.cs** klasy, aby uwzględnić funkcje w celu komunikowania się z aplikacją interfejsu API sieci Web księgarni:</span><span class="sxs-lookup"><span data-stu-id="a64b5-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="a64b5-169">Rozwiń **modele widoków** folder w Eksploratorze rozwiązań, a następnie kliknij dwukrotnie plik **MainViewModel.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="a64b5-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="a64b5-170">Gdy **MainViewModel.cs** plik jest otwarty, Zastąp kod w pliku następującym kodem; należy pamiętać, że konieczne będzie zaktualizowanie wartości `apiUrl` stałej przy użyciu rzeczywistego adresu URL interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="a64b5-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="a64b5-171">Zapisz i Zamknij **MainViewModel.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="a64b5-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="a64b5-172">Aktualizacja **MainPage.xaml** plik, aby dostosować nazwę aplikacji:</span><span class="sxs-lookup"><span data-stu-id="a64b5-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="a64b5-173">Kliknij dwukrotnie **MainPage.xaml** plik w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="a64b5-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="a64b5-174">Gdy **MainPage.xaml** plik jest otwarty, Znajdź następujące wiersze kodu:</span><span class="sxs-lookup"><span data-stu-id="a64b5-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="a64b5-175">Zastąp te wiersze następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="a64b5-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="a64b5-176">Zapisz i Zamknij **MainPage.xaml** pliku.</span><span class="sxs-lookup"><span data-stu-id="a64b5-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="a64b5-177">Aktualizacja **DetailsPage.xaml** plik, aby dostosować wyświetlane elementy:</span><span class="sxs-lookup"><span data-stu-id="a64b5-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="a64b5-178">Kliknij dwukrotnie **DetailsPage.xaml** plik w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="a64b5-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="a64b5-179">Gdy **DetailsPage.xaml** plik jest otwarty, Znajdź następujące wiersze kodu:</span><span class="sxs-lookup"><span data-stu-id="a64b5-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="a64b5-180">Zastąp te wiersze następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="a64b5-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="a64b5-181">Zapisz i Zamknij **DetailsPage.xaml** pliku.</span><span class="sxs-lookup"><span data-stu-id="a64b5-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="a64b5-182">Tworzenie aplikacji Windows Phone, aby sprawdzić błędy.</span><span class="sxs-lookup"><span data-stu-id="a64b5-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="a64b5-183">Krok 3: Rozwiązania End-to-End do testowania</span><span class="sxs-lookup"><span data-stu-id="a64b5-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="a64b5-184">Jak wspomniano w *wymagania wstępne* części tego samouczka, podczas testowania łączności między interfejsu API sieci Web i Windows Phone 8 projekty w systemie lokalnym, musisz wykonaj instrukcje z sekcji *[ Łączenie z emulatora systemu Windows Phone 8 do aplikacji interfejsu API sieci Web na komputerze lokalnym](https://go.microsoft.com/fwlink/?LinkId=324014)* artykuł, aby skonfigurować środowiska testowego.</span><span class="sxs-lookup"><span data-stu-id="a64b5-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="a64b5-185">Po utworzeniu, aby skonfigurować środowisko testowe, należy skonfigurować aplikację Windows Phone jako projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="a64b5-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="a64b5-186">Aby to zrobić, zaznacz **BookCatalog** w Eksploratorze rozwiązań, a następnie kliknij przycisk **Ustaw jako projekt startowy**:</span><span class="sxs-lookup"><span data-stu-id="a64b5-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="a64b5-187">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="a64b5-187">Click image to expand</span></span> |

<span data-ttu-id="a64b5-188">Po naciśnięciu klawisza F5, program Visual Studio będzie uruchomić zarówno Windows Phone Emulator, co spowoduje wyświetlenie &quot;Czekaj&quot; komunikatu, gdy dane aplikacji są pobierane z internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="a64b5-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="a64b5-189">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="a64b5-189">Click image to expand</span></span> |

<span data-ttu-id="a64b5-190">Jeśli wszystko, co zakończy się pomyślnie, powinien zostać wyświetlony katalogu wyświetlane:</span><span class="sxs-lookup"><span data-stu-id="a64b5-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="a64b5-191">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="a64b5-191">Click image to expand</span></span> |

<span data-ttu-id="a64b5-192">Naciśnij tytuł wszystkie książki aplikacja wyświetla opis książki:</span><span class="sxs-lookup"><span data-stu-id="a64b5-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="a64b5-193">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="a64b5-193">Click image to expand</span></span> |

<span data-ttu-id="a64b5-194">Jeśli aplikacja nie może komunikować się z interfejsem API sieci Web, zostanie wyświetlony komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="a64b5-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="a64b5-195">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="a64b5-195">Click image to expand</span></span> |

<span data-ttu-id="a64b5-196">Jeśli naciśniesz pozycję dotyczącą komunikatu o błędzie, będą wyświetlane dodatkowe szczegóły dotyczące błędu:</span><span class="sxs-lookup"><span data-stu-id="a64b5-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="a64b5-197">Kliknij obraz, aby rozwinąć</span><span class="sxs-lookup"><span data-stu-id="a64b5-197">Click image to expand</span></span>                                                                 |

