---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Pobieranie i wyświetlanie danych za pomocą modelu formularzy sieci web i powiązania | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396288"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="548e3-104">Pobieranie i wyświetlanie danych za pomocą wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="548e3-104">Retrieving and displaying data with model binding and web forms</span></span>
====================

> <span data-ttu-id="548e3-105">W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="548e3-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="548e3-106">Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="548e3-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="548e3-107">Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="548e3-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
>  <span data-ttu-id="548e3-108">Wzorca wiązania modelu współpracuje z dowolnym technologii dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="548e3-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="548e3-109">W tym samouczku użyjesz programu Entity Framework, ale można użyć technologii dostępu do danych, który jest najbardziej znane.</span><span class="sxs-lookup"><span data-stu-id="548e3-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="548e3-110">Z kontrolki powiązane z danymi serwera, na przykład kontrolki GridView, ListView, DetailsView lub FormView określane są nazwy metod na potrzeby wybierając, aktualizowanie, usuwanie i tworzenie danych.</span><span class="sxs-lookup"><span data-stu-id="548e3-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="548e3-111">W tym samouczku należy określić wartość dla metody SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="548e3-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="548e3-112">W ramach tej metody można przewidzieć logikę pobierania danych.</span><span class="sxs-lookup"><span data-stu-id="548e3-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="548e3-113">W następnym samouczku ustawi wartości dla operacji UpdateMethod, DeleteMethod i operacji InsertMethod.</span><span class="sxs-lookup"><span data-stu-id="548e3-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="548e3-114">Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w C# lub Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="548e3-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="548e3-115">Do pobrania kod działa, za pomocą programu Visual Studio 2012 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="548e3-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="548e3-116">Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2017, przedstawione w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="548e3-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="548e3-117">W tym samouczku możesz uruchomić aplikację w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="548e3-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="548e3-118">Można również wdrożyć aplikację na dostawcy usług hostingowych i był dostępny za pośrednictwem Internetu.</span><span class="sxs-lookup"><span data-stu-id="548e3-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="548e3-119">Firma Microsoft oferuje bezpłatny internetowy hostowanie do 10 witryn sieci web w</span><span class="sxs-lookup"><span data-stu-id="548e3-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="548e3-120">[bezpłatne konto wersji próbnej platformy Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="548e3-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="548e3-121">Aby uzyskać informacje o sposobie wdrażania projektu sieci web programu Visual Studio do usługi Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serii.</span><span class="sxs-lookup"><span data-stu-id="548e3-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="548e3-122">Ten samouczek pokazuje również, jak użyć migracje Code First Framework jednostki do wdrożenia bazy danych programu SQL Server do usługi Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="548e3-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="548e3-123">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="548e3-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="548e3-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="548e3-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="548e3-125">W tym samouczku współpracuje również z programu Visual Studio 2012 i Visual Studio 2013, ale istnieją pewne różnice w szablonie Projekt i interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="548e3-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="548e3-126">Będziesz tworzyć</span><span class="sxs-lookup"><span data-stu-id="548e3-126">What you'll build</span></span>

<span data-ttu-id="548e3-127">W ramach tego samouczka należy:</span><span class="sxs-lookup"><span data-stu-id="548e3-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="548e3-128">Tworzenie obiektów danych, które odzwierciedlają uniwersytetu z studentów rejestrację na kursy</span><span class="sxs-lookup"><span data-stu-id="548e3-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="548e3-129">Tworzenie tabel bazy danych z obiektów</span><span class="sxs-lookup"><span data-stu-id="548e3-129">Build database tables from the objects</span></span>
* <span data-ttu-id="548e3-130">Wypełniania bazy danych przy użyciu danych testowych</span><span class="sxs-lookup"><span data-stu-id="548e3-130">Populate the database with test data</span></span>
* <span data-ttu-id="548e3-131">Wyświetlanie danych w formularzu sieci web</span><span class="sxs-lookup"><span data-stu-id="548e3-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="548e3-132">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="548e3-132">Create the project</span></span>

1. <span data-ttu-id="548e3-133">W programie Visual Studio 2017, należy utworzyć **aplikacji sieci Web platformy ASP.NET (.NET Framework)** projekt o nazwie **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="548e3-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![Tworzenie projektu](retrieving-data/_static/image19.png)

2. <span data-ttu-id="548e3-135">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="548e3-135">Select **OK**.</span></span> <span data-ttu-id="548e3-136">Zostanie wyświetlone okno dialogowe, aby wybrać szablon.</span><span class="sxs-lookup"><span data-stu-id="548e3-136">The dialog box to select a template appears.</span></span>

   ![Wybierz formularze sieci web](retrieving-data/_static/image3.png)

3. <span data-ttu-id="548e3-138">Wybierz **formularzy sieci Web** szablonu.</span><span class="sxs-lookup"><span data-stu-id="548e3-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="548e3-139">W razie potrzeby zmień uwierzytelnianie, aby **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="548e3-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="548e3-140">Wybierz **OK** do tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="548e3-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="548e3-141">Modyfikowanie wyglądu witryny</span><span class="sxs-lookup"><span data-stu-id="548e3-141">Modify site appearance</span></span>

   <span data-ttu-id="548e3-142">Wprowadzić kilka zmian, aby dostosować wygląd witryny.</span><span class="sxs-lookup"><span data-stu-id="548e3-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="548e3-143">Otwórz plik Site.Master.</span><span class="sxs-lookup"><span data-stu-id="548e3-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="548e3-144">Zmienianie tytułu, aby wyświetlić **Contoso University** i nie **My ASP.NET Application**.</span><span class="sxs-lookup"><span data-stu-id="548e3-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="548e3-145">Zmień tekst nagłówka z **Nazwa aplikacji** do **Contoso University**.</span><span class="sxs-lookup"><span data-stu-id="548e3-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="548e3-146">Zmień łącza nagłówka nawigacji do lokacji odpowiednią z nich.</span><span class="sxs-lookup"><span data-stu-id="548e3-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="548e3-147">Usuń linki do **o** i **skontaktuj się z pomocą** i zamiast tego należy połączyć **studentów** strony, który zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="548e3-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="548e3-148">Zapisz Site.Master.</span><span class="sxs-lookup"><span data-stu-id="548e3-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="548e3-149">Dodaj formularz sieci web, aby wyświetlić dane dla uczniów</span><span class="sxs-lookup"><span data-stu-id="548e3-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="548e3-150">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj** i następnie **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="548e3-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="548e3-151">W **Dodaj nowy element** okno dialogowe, wybierz opcję **formularz sieci Web ze stroną wzorcową** szablonu i nadaj mu nazwę **Students.aspx**.</span><span class="sxs-lookup"><span data-stu-id="548e3-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![Tworzenie strony](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="548e3-153">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="548e3-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="548e3-154">Formularz sieci web stronę wzorcową, można wybrać **Site.Master**.</span><span class="sxs-lookup"><span data-stu-id="548e3-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="548e3-155">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="548e3-155">Select **OK**.</span></span>
   

## <a name="add-the-data-model"></a><span data-ttu-id="548e3-156">Dodawanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="548e3-156">Add the data model</span></span>

<span data-ttu-id="548e3-157">W **modeli** folderu, Dodaj klasę o nazwie **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="548e3-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="548e3-158">Kliknij prawym przyciskiem myszy **modeli**, wybierz opcję **Dodaj**, a następnie **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="548e3-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="548e3-159">**Dodaj nowy element** pojawi się okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="548e3-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="548e3-160">W menu nawigacji po lewej stronie wybierz **kodu**, następnie **klasy**.</span><span class="sxs-lookup"><span data-stu-id="548e3-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![Tworzenie klasy modelu](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="548e3-162">Nazwa klasy **UniversityModels.cs** i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="548e3-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="548e3-163">W tym pliku, należy zdefiniować `SchoolContext`, `Student`, `Enrollment`, i `Course` klas w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="548e3-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="548e3-164">`SchoolContext` Klasa pochodzi od `DbContext`, która zarządza połączenia z bazą danych i zmiany w danych.</span><span class="sxs-lookup"><span data-stu-id="548e3-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="548e3-165">W `Student` klasy, zwróć uwagę, atrybuty stosowane do `FirstName`, `LastName`, i `Year` właściwości.</span><span class="sxs-lookup"><span data-stu-id="548e3-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="548e3-166">W tym samouczku korzysta z tych atrybutów sprawdzania poprawności danych.</span><span class="sxs-lookup"><span data-stu-id="548e3-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="548e3-167">Aby uprościć kod, tylko te właściwości są oznaczone za pomocą atrybutów sprawdzania poprawności danych.</span><span class="sxs-lookup"><span data-stu-id="548e3-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="548e3-168">W projekcie rzeczywistych atrybutów sprawdzania poprawności będzie dotyczą wszystkich właściwości wymagające weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="548e3-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="548e3-169">Save UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="548e3-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="548e3-170">Konfigurowanie bazy danych oparte na klasach</span><span class="sxs-lookup"><span data-stu-id="548e3-170">Set up the database based on classes</span></span>

<span data-ttu-id="548e3-171">W tym samouczku [migracje Code First](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) do tworzenia obiektów i tabel bazy danych programu.</span><span class="sxs-lookup"><span data-stu-id="548e3-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="548e3-172">Te tabele są przechowywane informacje o uczniów i ich kursy.</span><span class="sxs-lookup"><span data-stu-id="548e3-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="548e3-173">Wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="548e3-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="548e3-174">W **Konsola Menedżera pakietów**, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="548e3-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="548e3-175">Jeśli polecenie zakończy się pomyślnie, zostanie wyświetlony komunikat z informacją, że włączono migracji.</span><span class="sxs-lookup"><span data-stu-id="548e3-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![Włączanie funkcji migracje](retrieving-data/_static/image8.png)

      <span data-ttu-id="548e3-177">Należy zauważyć, że plik o nazwie *Configuration.cs* została utworzona.</span><span class="sxs-lookup"><span data-stu-id="548e3-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="548e3-178">`Configuration` Klasa ma `Seed` metody, która wstępnie wypełnić tabele bazy danych z danymi.</span><span class="sxs-lookup"><span data-stu-id="548e3-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="548e3-179">Wstępne wypełnianie bazy danych</span><span class="sxs-lookup"><span data-stu-id="548e3-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="548e3-180">Otwórz Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="548e3-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="548e3-181">Dodaj następujący kod do `Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="548e3-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="548e3-182">Ponadto Dodaj `using` poufności informacji dotyczące `ContosoUniversityModelBinding. Models` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="548e3-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="548e3-183">Zapisz Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="548e3-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="548e3-184">W konsoli Menedżera pakietów, uruchom polecenie **początkowej migracji Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="548e3-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="548e3-185">Uruchom polecenie **update-database**.</span><span class="sxs-lookup"><span data-stu-id="548e3-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="548e3-186">Jeśli wystąpi wyjątek podczas uruchamiania tego polecenia `StudentID` i `CourseID` wartości może się różnić od `Seed` wartości metody.</span><span class="sxs-lookup"><span data-stu-id="548e3-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="548e3-187">Otwórz te tabele bazy danych i Znajdź istniejące wartości dla `StudentID` i `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="548e3-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="548e3-188">Dodaj te wartości do kodu rozmieszczania `Enrollments` tabeli.</span><span class="sxs-lookup"><span data-stu-id="548e3-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="548e3-189">Dodawanie kontrolki GridView</span><span class="sxs-lookup"><span data-stu-id="548e3-189">Add a GridView control</span></span>

<span data-ttu-id="548e3-190">Z wypełniania bazy danych możesz teraz pobrać te dane i wyświetlania ich.</span><span class="sxs-lookup"><span data-stu-id="548e3-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="548e3-191">Otwórz Students.aspx.</span><span class="sxs-lookup"><span data-stu-id="548e3-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="548e3-192">Znajdź `MainContent` symbol zastępczy.</span><span class="sxs-lookup"><span data-stu-id="548e3-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="548e3-193">W ramach tego symbolu zastępczego, należy dodać **GridView** formant, który zawiera ten kod.</span><span class="sxs-lookup"><span data-stu-id="548e3-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="548e3-194">Kwestii, które należy zwrócić uwagę:</span><span class="sxs-lookup"><span data-stu-id="548e3-194">Things to note:</span></span>
   * <span data-ttu-id="548e3-195">Zwróć uwagę, wartość ustawioną dla `SelectMethod` właściwości w elemencie GridView.</span><span class="sxs-lookup"><span data-stu-id="548e3-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="548e3-196">Ta wartość określa metodę używaną do pobierania danych widoku GridView, które są tworzone w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="548e3-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="548e3-197">`ItemType` Właściwość jest ustawiona na `Student` klasy utworzonej wcześniej.</span><span class="sxs-lookup"><span data-stu-id="548e3-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="548e3-198">To ustawienie umożliwia odwoływać się do właściwości klasy w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="548e3-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="548e3-199">Na przykład `Student` klasa ma kolekcję o nazwie `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="548e3-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="548e3-200">Można użyć `Item.Enrollments` pobrania tej kolekcji, a następnie użyć [składni LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) można pobrać każdy uczeń użytkownika zarejestrowane środki na korzystanie z sumą.</span><span class="sxs-lookup"><span data-stu-id="548e3-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="548e3-201">Zapisz Students.aspx.</span><span class="sxs-lookup"><span data-stu-id="548e3-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="548e3-202">Dodaj kod, aby pobrać dane</span><span class="sxs-lookup"><span data-stu-id="548e3-202">Add code to retrieve data</span></span>

   <span data-ttu-id="548e3-203">W pliku związanym z kodem Students.aspx, Dodaj metodę określone dla `SelectMethod` wartość.</span><span class="sxs-lookup"><span data-stu-id="548e3-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="548e3-204">Otwórz Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="548e3-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="548e3-205">Dodaj `using` instrukcji dla `ContosoUniversityModelBinding. Models` i `System.Data.Entity` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="548e3-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="548e3-206">Dodaj metodę określona dla `SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="548e3-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="548e3-207">`Include` Klauzuli poprawia wydajność zapytań, ale nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="548e3-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="548e3-208">Bez `Include` klauzuli, dane są pobierane przy użyciu [ *powolne ładowanie*](https://en.wikipedia.org/wiki/Lazy_loading), które obejmuje wysyłanie oddzielnego zapytania do bazy danych zawsze powiązane dane są pobierane.</span><span class="sxs-lookup"><span data-stu-id="548e3-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="548e3-209">Za pomocą `Include` klauzuli, dane są pobierane przy użyciu *wczesne ładowanie*, co oznacza, że zapytanie pojedynczej bazy danych pobiera wszystkie powiązane dane.</span><span class="sxs-lookup"><span data-stu-id="548e3-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="548e3-210">Powiązane dane nie będą używane, wczesne ładowanie jest mniej wydajne rozwiązanie, ponieważ dane są pobierane.</span><span class="sxs-lookup"><span data-stu-id="548e3-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="548e3-211">Jednak w takim przypadku wczesne ładowanie zapewnia najlepszą wydajność, ponieważ powiązane dane jest wyświetlany dla każdego rekordu.</span><span class="sxs-lookup"><span data-stu-id="548e3-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="548e3-212">Aby uzyskać więcej informacji na temat zagadnienia związane z wydajnością podczas ładowania powiązanych danych, zobacz **leniwy Eager i jawne ładowanie powiązanych danych** sekcji [odczytywania danych powiązanych z platformą Entity Framework na platformie ASP.NET Aplikacja MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) artykułu.</span><span class="sxs-lookup"><span data-stu-id="548e3-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="548e3-213">Domyślnie dane są sortowane według wartości właściwości oznaczonym jako klucz.</span><span class="sxs-lookup"><span data-stu-id="548e3-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="548e3-214">Możesz dodać `OrderBy` klauzulę, aby określić wartość sortowania.</span><span class="sxs-lookup"><span data-stu-id="548e3-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="548e3-215">W tym przykładzie domyślne `StudentID` właściwość jest używana do sortowania.</span><span class="sxs-lookup"><span data-stu-id="548e3-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="548e3-216">W [sortowanie, stronicowanie i filtrowanie danych](sorting-paging-and-filtering-data.md) artykułu, użytkownik jest włączona, aby wybrać kolumnę sortowania.</span><span class="sxs-lookup"><span data-stu-id="548e3-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="548e3-217">Zapisz Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="548e3-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="548e3-218">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="548e3-218">Run your application</span></span> 

<span data-ttu-id="548e3-219">Uruchom aplikację sieci web (**F5**) i przejdź do **studentów** stronę, która wyświetla następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="548e3-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![Pokaż dane](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="548e3-221">Automatyczne generowanie metody wiązania modelu</span><span class="sxs-lookup"><span data-stu-id="548e3-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="548e3-222">Podczas pracy za pośrednictwem tej serii samouczków, możesz po prostu skopiować kod z tego samouczka, do projektu.</span><span class="sxs-lookup"><span data-stu-id="548e3-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="548e3-223">Jednak jedną wadą tego podejścia jest, że użytkownik może nie zostaną powiadomieni o funkcji dostarczane przez program Visual Studio, aby automatycznie wygenerować kod dla metody wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="548e3-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="548e3-224">Podczas pracy z własnych projektów, automatyczne generowanie kodu można zaoszczędzić czas i pomoc, uzyskasz zorientować się, jak zaimplementować operacji.</span><span class="sxs-lookup"><span data-stu-id="548e3-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="548e3-225">W tej sekcji opisano funkcję generowanie kodu automatyczne.</span><span class="sxs-lookup"><span data-stu-id="548e3-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="548e3-226">Ta sekcja ma tylko charakter informacyjny i nie zawiera żadnego kodu, potrzebne do zaimplementowania w projekcie.</span><span class="sxs-lookup"><span data-stu-id="548e3-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="548e3-227">Podczas ustawiania wartości `SelectMethod`, `UpdateMethod`, `InsertMethod`, lub `DeleteMethod` właściwości w kodzie znaczników, możesz wybrać **Tworzenie nowej metody** opcji.</span><span class="sxs-lookup"><span data-stu-id="548e3-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![Utwórz metodę](retrieving-data/_static/image18.png)

<span data-ttu-id="548e3-229">Program Visual Studio nie tylko tworzy metodę w kodem przy użyciu prawidłowego podpisu, ale również generuje kod implementacji do wykonania tej operacji.</span><span class="sxs-lookup"><span data-stu-id="548e3-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="548e3-230">Jeśli najpierw ustawić `ItemType` właściwości przed użyciem automatyczne generowanie kodu funkcji, które należy wpisać używa wygenerowany kod dla operacji.</span><span class="sxs-lookup"><span data-stu-id="548e3-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="548e3-231">Na przykład podczas ustawiania `UpdateMethod` właściwość, poniższy kod jest generowany automatycznie:</span><span class="sxs-lookup"><span data-stu-id="548e3-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="548e3-232">Ponownie ten kod nie musi być dodany do projektu.</span><span class="sxs-lookup"><span data-stu-id="548e3-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="548e3-233">W następnym samouczku będziesz implementować metody do aktualizowania, usuwania i dodawania nowych danych.</span><span class="sxs-lookup"><span data-stu-id="548e3-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="548e3-234">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="548e3-234">Summary</span></span>

<span data-ttu-id="548e3-235">W tym samouczku utworzono klasy modelu danych i wygenerowany bazy danych na podstawie tych klas.</span><span class="sxs-lookup"><span data-stu-id="548e3-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="548e3-236">Tabele bazy danych są wypełnione danych testowych.</span><span class="sxs-lookup"><span data-stu-id="548e3-236">You filled the database tables with test data.</span></span> <span data-ttu-id="548e3-237">Używane wiązania modelu do pobierania danych z bazy danych, a następnie wyświetlane dane w widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="548e3-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="548e3-238">W ciągu następnych [samouczek](updating-deleting-and-creating-data.md) w tej serii włączysz, aktualizowanie, usuwanie i tworzenie danych.</span><span class="sxs-lookup"><span data-stu-id="548e3-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="548e3-239">Next</span><span class="sxs-lookup"><span data-stu-id="548e3-239">Next</span></span>](updating-deleting-and-creating-data.md)
