---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie aktualizacji bazy danych serwera SQL - 11, 12 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) programu ASP.NET projektu aplikacji sieci web, który zawiera bazę danych programu SQL Server Compact przy użyciu Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: c744bc54c8ce12820d1e1388ac7ab2db814ff031
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816635"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="a4db6-103">Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio lub Visual Web Developer: Wdrażanie aktualizacji bazy danych serwera SQL - 11, 12</span><span class="sxs-lookup"><span data-stu-id="a4db6-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="a4db6-104">przez [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a4db6-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="a4db6-105">Pobieranie projektu startowego</span><span class="sxs-lookup"><span data-stu-id="a4db6-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="a4db6-106">W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) ASP.NET projektu aplikacji sieci web, która zawiera bazę danych programu SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC for Web.</span><span class="sxs-lookup"><span data-stu-id="a4db6-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="a4db6-107">Umożliwia także programu Visual Studio 2010 po zainstalowaniu aktualizacji publikowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a4db6-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="a4db6-108">Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszym samouczku tej serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="a4db6-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="a4db6-109">Aby uzyskać samouczek, który zawiera funkcje wdrażania wprowadzone po wersji RC programu Visual Studio 2012, pokazuje, jak wdrażać wersje programu SQL Server, innym niż SQL Server Compact i pokazuje, jak wdrożyć do Windows Azure Web Sites, zobacz [wdrażanie aplikacji internetowych ASP.NET za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a4db6-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="a4db6-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="a4db6-110">Overview</span></span>

<span data-ttu-id="a4db6-111">W tym samouczku przedstawiono sposób wdrażania aktualizacji bazy danych do pełnego bazy danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a4db6-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="a4db6-112">Ponieważ migracje Code First wykonuje całą pracę aktualizowania bazy danych, proces jest niemal identyczny jak dla programu SQL Server Compact w [wdrażanie aktualizacji bazy danych](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="a4db6-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="a4db6-113">Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="a4db6-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="a4db6-114">Dodawanie nowej kolumny do tabeli</span><span class="sxs-lookup"><span data-stu-id="a4db6-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="a4db6-115">W tej części samouczka wprowadzisz zmiany bazy danych i odpowiednich zmian w kodzie, Testuj je w programie Visual Studio w ramach przygotowania do wdrażania ich w środowiskach testowych i produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="a4db6-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="a4db6-116">Zmiana obejmuje dodanie `OfficeHours` kolumny `Instructor` jednostki i wyświetlanie nowych informacji w **Instruktorzy** strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="a4db6-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="a4db6-117">W projekcie ContosoUniversity.DAL Otwórz *Instructor.cs* i dodaj następującą właściwość między `HireDate` i `Courses` właściwości:</span><span class="sxs-lookup"><span data-stu-id="a4db6-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="a4db6-118">Klasa inicjatora należy zaktualizować tak, aby go inicjowania inicjuje nową kolumnę z danymi.</span><span class="sxs-lookup"><span data-stu-id="a4db6-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="a4db6-119">Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z poniższy blok kodu, który zawiera nową kolumnę:</span><span class="sxs-lookup"><span data-stu-id="a4db6-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="a4db6-120">W projekcie ContosoUniversity Otwórz *Instructors.aspx* i dodać nowe pole szablonu dla godzin pracy zaraz przed zamknięciem `</Columns>` tagu w pierwszym `GridView` sterowania:</span><span class="sxs-lookup"><span data-stu-id="a4db6-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="a4db6-121">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="a4db6-121">Build the solution.</span></span>

<span data-ttu-id="a4db6-122">Otwórz **Konsola Menedżera pakietów** okna, a następnie wybierz opcję ContosoUniversity.DAL jako **projekt domyślny**.</span><span class="sxs-lookup"><span data-stu-id="a4db6-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="a4db6-123">Wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="a4db6-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="a4db6-124">Uruchom aplikację, a następnie wybierz **Instruktorzy** strony.</span><span class="sxs-lookup"><span data-stu-id="a4db6-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="a4db6-125">Strona trwa nieco dłużej niż zwykle załadować, ponieważ platforma Entity Framework ponownie tworzy bazę danych i inicjowania inicjuje ją z danymi.</span><span class="sxs-lookup"><span data-stu-id="a4db6-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="a4db6-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a4db6-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="a4db6-127">Wdrażanie aktualizacji bazy danych do środowiska testowego</span><span class="sxs-lookup"><span data-stu-id="a4db6-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="a4db6-128">Możesz użyć migracje Code First, metodę wdrażania zmianę w bazie danych programu SQL Server jest takie same jak dla programu SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="a4db6-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="a4db6-129">Jednak trzeba zmienić testu profil publikowania, ponieważ jest nadal wybrana do migracji z programu SQL Server Compact do programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a4db6-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="a4db6-130">Pierwszym krokiem jest do usunięcia przekształcenia ciągu połączenia, które zostały utworzone w poprzednim samouczku.</span><span class="sxs-lookup"><span data-stu-id="a4db6-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="a4db6-131">Te nie są już potrzebne, ponieważ w profilu publikowania, należy podać przekształcenia parametry połączenia tak jak przed skonfigurowano **Pakuj/Publikuj SQL** kartę pod kątem migracji do programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a4db6-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="a4db6-132">Otwórz *Web.Test.config* pliku i usuwania `connectionStrings` elementu.</span><span class="sxs-lookup"><span data-stu-id="a4db6-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="a4db6-133">Tylko transformacji pozostałe w *Web.Test.config* plik jest przeznaczony dla `Environment` wartość w `appSettings` elementu.</span><span class="sxs-lookup"><span data-stu-id="a4db6-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="a4db6-134">Teraz można zaktualizować profilu publikowania i Publikuj do środowiska testowego.</span><span class="sxs-lookup"><span data-stu-id="a4db6-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="a4db6-135">Otwórz **publikowanie w sieci Web** kreatora, a następnie przejdź do **profilu** kartę.</span><span class="sxs-lookup"><span data-stu-id="a4db6-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="a4db6-136">Wybierz **testu** profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="a4db6-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="a4db6-137">Wybierz **ustawienia** kartę.</span><span class="sxs-lookup"><span data-stu-id="a4db6-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="a4db6-138">Kliknij przycisk **Włącz nowe ulepszenia dotyczące publikowania bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="a4db6-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="a4db6-139">W polu ciągu połączenia dla **SchoolContext**, wprowadź tę samą wartość, która została użyta w *Web.Test.config* plik przekształcenia w poprzednim samouczku:</span><span class="sxs-lookup"><span data-stu-id="a4db6-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="a4db6-140">Wybierz **wykonaj migracje Code First (uruchomieniu aplikacji)**.</span><span class="sxs-lookup"><span data-stu-id="a4db6-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="a4db6-141">(W używanej wersji programu Visual Studio może być oznaczony jako pole wyboru **zastosować migracje Code First**.)</span><span class="sxs-lookup"><span data-stu-id="a4db6-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="a4db6-142">W polu ciągu połączenia dla **DefaultConnection**, wprowadź tę samą wartość, która została użyta w *Web.Test.config* plik przekształcenia w poprzednim samouczku:</span><span class="sxs-lookup"><span data-stu-id="a4db6-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="a4db6-143">Pozostaw **Aktualizuj bazę danych** wyczyszczone.</span><span class="sxs-lookup"><span data-stu-id="a4db6-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="a4db6-144">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="a4db6-144">Click **Publish**.</span></span>

<span data-ttu-id="a4db6-145">Visual Studio wdroży zmian kodu do środowiska testowego i otwiera przeglądarkę na stronę główną University firmy Contoso.</span><span class="sxs-lookup"><span data-stu-id="a4db6-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="a4db6-146">Wybierz stronę instruktorów.</span><span class="sxs-lookup"><span data-stu-id="a4db6-146">Select the Instructors page.</span></span>

<span data-ttu-id="a4db6-147">Gdy aplikacja zostanie uruchomiona na tej stronie, próbuje dostęp do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a4db6-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="a4db6-148">Migracje Code First sprawdza, czy baza danych jest aktualny i znajduje się, że najnowsze migracji nie została zastosowana jeszcze.</span><span class="sxs-lookup"><span data-stu-id="a4db6-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="a4db6-149">Migracje Code First dotyczy najnowszej migracji, uruchamia `Seed` metody, a następnie stronę działa normalnie.</span><span class="sxs-lookup"><span data-stu-id="a4db6-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="a4db6-150">Zobaczysz nową kolumnę godzinami pracy z wprowadzonych danych.</span><span class="sxs-lookup"><span data-stu-id="a4db6-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="a4db6-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a4db6-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="a4db6-152">Wdrażanie aktualizacji bazy danych do środowiska produkcyjnego</span><span class="sxs-lookup"><span data-stu-id="a4db6-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="a4db6-153">Należy również zmienić profil publikowania dla środowiska produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="a4db6-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="a4db6-154">W tym przypadku będzie Usuń istniejący profil i Utwórz nową, importując plik .publishsettings zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="a4db6-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="a4db6-155">Zaktualizowany plik będzie zawierać parametry połączenia dla bazy danych programu SQL Server w Cytanium.</span><span class="sxs-lookup"><span data-stu-id="a4db6-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="a4db6-156">Jak podczas wdrażania do środowiska testowego, nie potrzebujesz już przekształcenia parametrów połączenia w *Web.Production.config* plik przekształcenia.</span><span class="sxs-lookup"><span data-stu-id="a4db6-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="a4db6-157">Otwarcie tego pliku, a następnie usuń `connectionStrings` elementu.</span><span class="sxs-lookup"><span data-stu-id="a4db6-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="a4db6-158">Pozostałe przekształcenia są dla `Environment` wartość w `appSettings` elementu i `location` element, który ogranicza dostęp do raportów o błędach Elmah.</span><span class="sxs-lookup"><span data-stu-id="a4db6-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="a4db6-159">Przed utworzeniem nowego profilu publikowania w środowisku produkcyjnym należy pobrać plik .publishsettings zaktualizowane tak samo jak wcześniej w [wdrażanie w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="a4db6-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="a4db6-160">(W Panelu sterowania Cytanium kliknij **witryn sieci Web**, a następnie kliknij przycisk **contosouniversity.com** witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a4db6-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="a4db6-161">Wybierz **publikowania w sieci Web** kartę, a następnie kliknij przycisk **Pobierz profil publikowania dla tej witryny sieci web**.) Powodów, dla którego to robią to aby pobrać parametry połączenia bazy danych w pliku .publishsettings.</span><span class="sxs-lookup"><span data-stu-id="a4db6-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="a4db6-162">Parametry połączenia nie była dostępna został pobrany plik, ponieważ był używany program SQL Server Compact i nie zostało jeszcze utworzone bazy danych programu SQL Server w Cytanium po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="a4db6-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="a4db6-163">Teraz można zaktualizować profilu publikowania i Publikuj do środowiska produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="a4db6-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="a4db6-164">Otwórz **publikowanie w sieci Web** kreatora, a następnie przejdź do **profilu** kartę.</span><span class="sxs-lookup"><span data-stu-id="a4db6-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="a4db6-165">Kliknij przycisk **spravovat profily**, a następnie usuń profil produkcji.</span><span class="sxs-lookup"><span data-stu-id="a4db6-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="a4db6-166">Zamknij **publikowanie w sieci Web** kreatora, aby zapisać tę zmianę.</span><span class="sxs-lookup"><span data-stu-id="a4db6-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="a4db6-167">Otwórz **publikowanie w sieci Web** kreatora ponownie, a następnie kliknij przycisk **importu**.</span><span class="sxs-lookup"><span data-stu-id="a4db6-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="a4db6-168">Na **połączenia** kartę, zmień **docelowy adres URL** odpowiednią wartość, korzystając z tymczasowego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a4db6-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="a4db6-169">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="a4db6-169">Click **Next**.</span></span>

<span data-ttu-id="a4db6-170">Na **ustawienia** kliknij pozycję **Włącz nowe ulepszenia dotyczące publikowania bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="a4db6-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="a4db6-171">Na liście rozwijanej ciąg połączenia dla **SchoolContext**, wybierz Cytanium parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="a4db6-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="a4db6-173">Wybierz **migracje wykonania Code First (uruchomieniu aplikacji)**.</span><span class="sxs-lookup"><span data-stu-id="a4db6-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="a4db6-174">Na liście rozwijanej ciąg połączenia dla **DefaultConnection**, wybierz Cytanium parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="a4db6-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="a4db6-175">Wybierz **profilu** kliknij pozycję **spravovat profily**i Zmień nazwę profilu z "contosouniversity.com — narzędzie Web Deploy" na "Produkcyjne".</span><span class="sxs-lookup"><span data-stu-id="a4db6-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="a4db6-176">Zamknij profil publikowania, aby zapisać wprowadzone zmiany, a następnie otwórz go ponownie.</span><span class="sxs-lookup"><span data-stu-id="a4db6-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="a4db6-177">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="a4db6-177">Click **Publish**.</span></span> <span data-ttu-id="a4db6-178">(Rzeczywistej produkcji witryny sieci Web może spowodować skopiowanie *aplikacji\_offline.htm* do produkcji i umieść go w folderze projektu przed opublikowaniem, następnie usunięcia go po zakończeniu wdrożenia.)</span><span class="sxs-lookup"><span data-stu-id="a4db6-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="a4db6-179">Visual Studio wdroży zmian kodu do środowiska testowego i otwiera przeglądarkę na stronę główną University firmy Contoso.</span><span class="sxs-lookup"><span data-stu-id="a4db6-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="a4db6-180">Wybierz stronę instruktorów.</span><span class="sxs-lookup"><span data-stu-id="a4db6-180">Select the Instructors page.</span></span>

<span data-ttu-id="a4db6-181">Migracje Code First aktualizuje bazę danych w taki sam sposób jak w środowisku testowym.</span><span class="sxs-lookup"><span data-stu-id="a4db6-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="a4db6-182">Zobaczysz nową kolumnę godzinami pracy z wprowadzonych danych.</span><span class="sxs-lookup"><span data-stu-id="a4db6-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="a4db6-184">Teraz pomyślnie wdrożono aktualizację aplikacji, która obejmuje zmianę bazy danych przy użyciu bazy danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a4db6-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="a4db6-185">Więcej informacji</span><span class="sxs-lookup"><span data-stu-id="a4db6-185">More Information</span></span>

<span data-ttu-id="a4db6-186">Na tym kończy się w tej serii samouczków na temat wdrażania aplikacji sieci web ASP.NET u dostawcy hostingu innych firm.</span><span class="sxs-lookup"><span data-stu-id="a4db6-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="a4db6-187">Aby uzyskać więcej informacji na temat dowolny z tematów omówione w tych samouczkach zobacz [Mapa zawartości platformy ASP.NET wdrożenia](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="a4db6-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="a4db6-188">Potwierdzanie</span><span class="sxs-lookup"><span data-stu-id="a4db6-188">Acknowledgements</span></span>

<span data-ttu-id="a4db6-189">Chcę otrzymywać podziękować następujących osób, które istotny wkład w zawartości w tej serii samouczków:</span><span class="sxs-lookup"><span data-stu-id="a4db6-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="a4db6-190">Alberto Poblacion, MVP &amp; MCT, Hiszpania</span><span class="sxs-lookup"><span data-stu-id="a4db6-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="a4db6-191">Jarod Ferguson, programowanie na platformie danych MVP, Stany Zjednoczone</span><span class="sxs-lookup"><span data-stu-id="a4db6-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="a4db6-192">Ostrym Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a4db6-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="a4db6-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a4db6-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="a4db6-194">Mike Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a4db6-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="a4db6-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a4db6-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="a4db6-196">Raffaele Rialdi, Włochy</span><span class="sxs-lookup"><span data-stu-id="a4db6-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="a4db6-197">Rick Anderson firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="a4db6-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="a4db6-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="a4db6-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="a4db6-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="a4db6-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="a4db6-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="a4db6-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="a4db6-201">Srđan Božović, Serbia</span><span class="sxs-lookup"><span data-stu-id="a4db6-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="a4db6-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="a4db6-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a4db6-203">[Poprzednie](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [dalej](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="a4db6-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
