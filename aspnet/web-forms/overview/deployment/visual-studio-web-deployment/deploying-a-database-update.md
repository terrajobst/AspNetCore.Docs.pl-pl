---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji bazy danych | Dokumentacja firmy Microsoft'
author: tdykstra
description: Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 3020cfa19bf12f21c6d42a77ed257595431b4e86
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892624"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="d3695-103">Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji bazy danych</span><span class="sxs-lookup"><span data-stu-id="d3695-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="d3695-104">Przez [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d3695-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="d3695-105">Pobierz początkowego projektu</span><span class="sxs-lookup"><span data-stu-id="d3695-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="d3695-106">Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="d3695-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="d3695-107">Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d3695-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="d3695-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d3695-108">Overview</span></span>

<span data-ttu-id="d3695-109">W tym samouczku wprowadzić zmianę w bazie danych i zmiany kodu powiązanego przetestować zmiany w programie Visual Studio, a następnie wdrożyć aktualizację na środowiska testowego, tymczasową i produkcyjną.</span><span class="sxs-lookup"><span data-stu-id="d3695-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="d3695-110">Samouczek najpierw pokazano, jak zaktualizować bazę danych, który jest zarządzany przez migracje Code First, a następnie później go pokazano, jak zaktualizować bazę danych przy użyciu dostawcy dbDacFx narzędzia.</span><span class="sxs-lookup"><span data-stu-id="d3695-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="d3695-111">Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="d3695-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="d3695-112">Wdrażanie aktualizacji bazy danych przy użyciu migracje Code First</span><span class="sxs-lookup"><span data-stu-id="d3695-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="d3695-113">W tej sekcji, Dodaj kolumnę daty urodzenia `Person` klasę podstawową dla `Student` i `Instructor` jednostek.</span><span class="sxs-lookup"><span data-stu-id="d3695-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="d3695-114">Następnie należy zaktualizować strona, wyświetlająca instruktora danych, tak aby wyświetlone nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="d3695-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="d3695-115">Ponadto można wdrożyć zmiany do testu, tymczasową i produkcyjną.</span><span class="sxs-lookup"><span data-stu-id="d3695-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="d3695-116">Dodaj kolumnę do tabeli w bazie danych aplikacji</span><span class="sxs-lookup"><span data-stu-id="d3695-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="d3695-117">W *ContosoUniversity.DAL* otwarciu projektu *Person.cs* i dodaj następującą właściwość na końcu `Person` klasy (powinny istnieć dwie zamykający nawias klamrowy następnego):</span><span class="sxs-lookup"><span data-stu-id="d3695-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="d3695-118">Następnie zaktualizuj `Seed` metodę, którą zawiera wartość dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="d3695-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="d3695-119">Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z następujący blok kodu, który obejmuje informacje daty urodzenia:</span><span class="sxs-lookup"><span data-stu-id="d3695-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="d3695-120">Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna.</span><span class="sxs-lookup"><span data-stu-id="d3695-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="d3695-121">Upewnij się, że ContosoUniversity.DAL jest nadal wybrany jako **domyślny projekt**.</span><span class="sxs-lookup"><span data-stu-id="d3695-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="d3695-122">W **Konsola Menedżera pakietów** wybierz **ContosoUniversity.DAL** jako **domyślny projekt**, a następnie wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="d3695-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="d3695-123">Po zakończeniu działania tego polecenia programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` klasy, a następnie w `Up` — metoda zostanie wyświetlony kod, który tworzy nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="d3695-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="d3695-124">`Up` Metoda tworzy kolumny, wprowadzając zmiany i `Down` — metoda usuwa kolumny, gdy użytkownik jest wycofywanie zmian.</span><span class="sxs-lookup"><span data-stu-id="d3695-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="d3695-126">Skompiluj rozwiązanie, a następnie wprowadź następujące polecenie w **Konsola Menedżera pakietów** okna (Upewnij się, nadal wybrano projektu ContosoUniversity.DAL):</span><span class="sxs-lookup"><span data-stu-id="d3695-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="d3695-127">Uruchamia programu Entity Framework `Up` metody, a następnie uruchamia `Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="d3695-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="d3695-128">Wyświetlanie nowej kolumny, na stronie instruktorów</span><span class="sxs-lookup"><span data-stu-id="d3695-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="d3695-129">W projekcie ContosoUniversity Otwórz *Instructors.aspx* i Dodaj nowe pole do szablonu, aby wyświetlić datę urodzenia.</span><span class="sxs-lookup"><span data-stu-id="d3695-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="d3695-130">Dodaj go między te przypisania zatrudnienia daty i pakietu office:</span><span class="sxs-lookup"><span data-stu-id="d3695-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="d3695-131">(Jeśli wcięcia kodu jest niezsynchronizowana, możesz nacisnąć klawisze CTRL-K, a następnie klawisz CTRL-D automatycznie sformatuj ponownie plik.)</span><span class="sxs-lookup"><span data-stu-id="d3695-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="d3695-132">Uruchom aplikację, a następnie kliknij przycisk **instruktorów** łącza.</span><span class="sxs-lookup"><span data-stu-id="d3695-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="d3695-133">Podczas ładowania strony widać, że ma nowy Data urodzenia.</span><span class="sxs-lookup"><span data-stu-id="d3695-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Strona instruktorów z Data urodzenia](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="d3695-135">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="d3695-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="d3695-136">Wdrażanie aktualizacji bazy danych</span><span class="sxs-lookup"><span data-stu-id="d3695-136">Deploy the database update</span></span>

1. <span data-ttu-id="d3695-137">W **Eksploratora rozwiązań** wybierz projekt ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="d3695-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="d3695-138">W **jeden kliknij publikowania w sieci Web** narzędzi, kliknij przycisk **testu** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="d3695-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="d3695-139">(Wyłączenie pasku narzędzi wybierz projekt ContosoUniversity **Eksploratora rozwiązań**.)</span><span class="sxs-lookup"><span data-stu-id="d3695-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="d3695-140">Program Visual Studio wdroży zaktualizowaną aplikację, a następnie otwiera przeglądarkę na stronę główną.</span><span class="sxs-lookup"><span data-stu-id="d3695-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="d3695-141">Uruchom **instruktorów** stronę, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.</span><span class="sxs-lookup"><span data-stu-id="d3695-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="d3695-142">Gdy aplikacja podejmie próbę dostępu do bazy danych dla tej strony, Code First aktualizacje schematu bazy danych i uruchamia `Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="d3695-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="d3695-143">Gdy zostanie wyświetlona strona, można znaleźć oczekiwanej **Data urodzenia** kolumny zawierającej daty w nim.</span><span class="sxs-lookup"><span data-stu-id="d3695-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="d3695-144">W **jeden kliknij publikowania w sieci Web** narzędzi, kliknij przycisk **przemieszczania** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="d3695-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="d3695-145">Uruchom **instruktorów** strony w przejściowym można zweryfikować, że aktualizacja została pomyślnie wdrożona.</span><span class="sxs-lookup"><span data-stu-id="d3695-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="d3695-146">W **jeden kliknij publikowania w sieci Web** narzędzi, kliknij przycisk **produkcji** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="d3695-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="d3695-147">Uruchom **instruktorów** strony w środowisku produkcyjnym, aby zweryfikować, że aktualizacja została pomyślnie wdrożona.</span><span class="sxs-lookup"><span data-stu-id="d3695-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="d3695-148">Rzeczywistej produkcji aktualizacji aplikacji, która obejmuje zmianę w bazie danych przy użyciu również zwykle zajmie aplikacji w tryb offline podczas wdrażania *aplikacji\_offline.htm*, jak opisany w poprzednim samouczka.</span><span class="sxs-lookup"><span data-stu-id="d3695-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="d3695-149">Wdrażanie aktualizacji bazy danych przy użyciu dostawcy dbDacFx narzędzia</span><span class="sxs-lookup"><span data-stu-id="d3695-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="d3695-150">W tej sekcji możesz dodać *komentarze* kolumny *użytkownika* tabeli w bazie danych członkostwa i utworzyć stronę, która pozwala wyświetlić i edytować komentarze dla każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d3695-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="d3695-151">Następnie możesz wdrożyć zmiany do testu, tymczasowych i produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="d3695-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="d3695-152">Dodaj kolumnę do tabeli w bazie danych członkostwa</span><span class="sxs-lookup"><span data-stu-id="d3695-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="d3695-153">W programie Visual Studio Otwórz **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="d3695-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="d3695-154">Rozwiń węzeł **(localdb) \v11.0**, rozwiń węzeł **baz danych**, rozwiń węzeł **aspnet ContosoUniversity** (nie **aspnet-ContosoUniversity-produkcyjną**) a następnie rozwiń węzeł **tabel**.</span><span class="sxs-lookup"><span data-stu-id="d3695-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="d3695-155">Jeśli nie widzisz **(localdb) \v11.0** w obszarze **programu SQL Server** węzła, kliknij prawym przyciskiem myszy **programu SQL Server** węzeł i kliknij przycisk **dodawania serwera SQL**.</span><span class="sxs-lookup"><span data-stu-id="d3695-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="d3695-156">W **Połącz z serwerem** wprowadź okno dialogowe *(localdb) \v11.0* jako **nazwy serwera**, a następnie kliknij przycisk **Connect**.</span><span class="sxs-lookup"><span data-stu-id="d3695-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="d3695-157">Jeśli nie widzisz **aspnet ContosoUniversity**, uruchom projekt i zaloguj się za pomocą *admin* poświadczeń (hasło jest *devpwd*), a następnie Odśwież  **Eksplorator obiektów SQL Server** okna.</span><span class="sxs-lookup"><span data-stu-id="d3695-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="d3695-158">Kliknij prawym przyciskiem myszy **użytkowników** tabeli, a następnie kliknij przycisk **Widok projektanta**.</span><span class="sxs-lookup"><span data-stu-id="d3695-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Projektant widoków SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="d3695-160">W projektancie, Dodaj *komentarze* kolumny i zapewnić ich *nvarchar(128)* i wartości null, a następnie kliknij przycisk **aktualizacji**.</span><span class="sxs-lookup"><span data-stu-id="d3695-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Dodawanie komentarzy kolumny](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="d3695-162">W **aktualizacje bazy danych w wersji zapoznawczej** kliknij **aktualizacji bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="d3695-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Aktualizacje bazy danych w wersji zapoznawczej](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="d3695-164">Utwórz stronę, aby wyświetlić i edytować nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="d3695-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="d3695-165">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **konta** kliknij folder w projekcie ContosoUniversity **Dodaj**, a następnie kliknij przycisk **nowy element** .</span><span class="sxs-lookup"><span data-stu-id="d3695-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="d3695-166">Utwórz nową **strona wzorcowa przy użyciu formularza sieci Web** i nadaj mu nazwę *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="d3695-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="d3695-167">Zaakceptuj wartość domyślną *Site.Master* pliku jako strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="d3695-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="d3695-168">Skopiuj następujący kod do `MainContent` `Content` elementu (ostatni 3 `Content` elementy):</span><span class="sxs-lookup"><span data-stu-id="d3695-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="d3695-169">Kliknij prawym przyciskiem myszy *UserInfo.aspx* i kliknij przycisk **Wyświetl w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="d3695-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="d3695-170">Zaloguj się przy użyciu programu *admin* poświadczenia użytkownika (hasło jest *devpwd*) i Dodaj niektóre komentarze do użytkownika w celu zweryfikowania, że strony działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="d3695-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Strona informacje o użytkowniku](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="d3695-172">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="d3695-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="d3695-173">Wdrażanie aktualizacji bazy danych</span><span class="sxs-lookup"><span data-stu-id="d3695-173">Deploy the database update</span></span>

<span data-ttu-id="d3695-174">Aby wdrożyć przy użyciu dostawcy dbDacFx, wystarczy wybrać **aktualizacji bazy danych** opcji w profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="d3695-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="d3695-175">Jednak dla pierwszego wdrożenia tej opcji należy również skonfigurowane niektóre dodatkowe na uruchamianie skryptów SQL: te są nadal w profilu i należy uniemożliwić ich ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="d3695-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="d3695-176">Otwórz **publikowanie w sieci Web** Kreator prawym przyciskiem myszy projekt ContosoUniversity, a następnie klikając polecenie **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="d3695-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="d3695-177">Wybierz **testu** profilu.</span><span class="sxs-lookup"><span data-stu-id="d3695-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="d3695-178">Kliknij przycisk **ustawienia** kartę.</span><span class="sxs-lookup"><span data-stu-id="d3695-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="d3695-179">W obszarze **połączenia DefaultConnection**, wybierz pozycję **aktualizacji bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="d3695-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="d3695-180">Wyłącz dodatkowych skryptów, skonfigurowanych do uruchamiania dla pierwszego wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="d3695-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="d3695-181">Kliknij przycisk **skonfigurować aktualizacje bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="d3695-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="d3695-182">W **Konfigurowanie aktualizacji bazy danych** okno dialogowe, usuń zaznaczenie pola wyboru obok pozycji *Grant.sql* i *aspnet-data-dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="d3695-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="d3695-183">Kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="d3695-183">Click **Close**.</span></span>
6. <span data-ttu-id="d3695-184">Kliknij przycisk **Podgląd** kartę.</span><span class="sxs-lookup"><span data-stu-id="d3695-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="d3695-185">W obszarze **baz danych** i po prawej stronie **połączenia DefaultConnection**, kliknij przycisk **bazy danych w wersji zapoznawczej** łącza.</span><span class="sxs-lookup"><span data-stu-id="d3695-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Podgląd bazy danych](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="d3695-187">Okno podglądu pokazuje skrypt, które będą uruchamiane w docelowej bazie danych, ten schemat bazy danych pasuje do schematu źródłowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d3695-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="d3695-188">Skrypt zawiera polecenia ALTER TABLE, który dodaje nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="d3695-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="d3695-189">Zamknij **Podgląd bazy danych** , a następnie kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="d3695-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="d3695-190">Program Visual Studio wdroży zaktualizowaną aplikację, a następnie otwiera przeglądarkę na stronę główną.</span><span class="sxs-lookup"><span data-stu-id="d3695-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="d3695-191">Uruchom na stronie informacje o użytkowniku (Dodaj *Account/UserInfo.aspx* na adres URL strony głównej) można zweryfikować, że aktualizacja została pomyślnie wdrożona.</span><span class="sxs-lookup"><span data-stu-id="d3695-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="d3695-192">Należy się zalogować, wprowadzając *admin* i *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="d3695-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="d3695-193">Dane w tabelach nie została wdrożona domyślna, a nie skonfigurowała skryptu wdrażania danych do uruchomienia, więc nie zostaną odnalezione komentarz, który został dodany do rozwoju.</span><span class="sxs-lookup"><span data-stu-id="d3695-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="d3695-194">Można teraz dodać nowy komentarz w przejściowym, aby sprawdzić, czy zmiana została wdrożona do bazy danych i strony działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="d3695-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="d3695-195">Postępuj zgodnie z tą samą procedurą, aby wdrożyć tymczasową i produkcyjną.</span><span class="sxs-lookup"><span data-stu-id="d3695-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="d3695-196">Należy pamiętać wyłączyć dodatkowych skryptów.</span><span class="sxs-lookup"><span data-stu-id="d3695-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="d3695-197">Jedyną różnicą w porównaniu do profilu testu jest czy tylko jeden skrypt w tymczasowej spowoduje wyłączenie i produkcji profile, ponieważ zostały one skonfigurowane do działania tylko *aspnet produkcyjną data.sql*.</span><span class="sxs-lookup"><span data-stu-id="d3695-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="d3695-198">Poświadczenia dla tymczasowych i produkcyjnych są administratora, jak i prodpwd.</span><span class="sxs-lookup"><span data-stu-id="d3695-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="d3695-199">Rzeczywistej produkcji aktualizacji aplikacji, która obejmuje zmianę w bazie danych aplikacji w tryb offline podczas wdrażania będzie również zwykle zajmują przekazując *aplikacji\_offline.htm* przed publikowania i usuwania później, jak przedstawiono w [poprzedniego samouczek](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="d3695-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="d3695-200">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="d3695-200">Summary</span></span>

<span data-ttu-id="d3695-201">Teraz wdrożeniu aktualizacji aplikacji, która obejmuje zmianę bazy danych przy użyciu zarówno migracje Code First i dostawcy dbDacFx narzędzia.</span><span class="sxs-lookup"><span data-stu-id="d3695-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Strona instruktorów z Data urodzenia](deploying-a-database-update/_static/image8.png)

![Strona informacje o użytkowniku](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="d3695-204">Następny samouczek pokazuje, jak wykonać wdrożenia przy użyciu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="d3695-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d3695-205">[Poprzednie](deploying-a-code-update.md)
> [dalej](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="d3695-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
