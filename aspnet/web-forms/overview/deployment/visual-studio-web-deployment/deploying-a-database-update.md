---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji bazy danych | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 88652a33d5367241fec4d442e9deb15d88a096c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817626"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="831e7-103">Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji bazy danych</span><span class="sxs-lookup"><span data-stu-id="831e7-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="831e7-104">przez [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="831e7-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="831e7-105">Pobieranie projektu startowego</span><span class="sxs-lookup"><span data-stu-id="831e7-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="831e7-106">W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="831e7-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="831e7-107">Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="831e7-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="831e7-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="831e7-108">Overview</span></span>

<span data-ttu-id="831e7-109">W tym samouczku, wprowadzić zmianę w bazie danych i zmian kodu powiązanego przetestować zmiany w programie Visual Studio, a następnie wdrożyć aktualizację w środowiskach testowych, przejściowych i produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="831e7-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="831e7-110">Samouczek najpierw pokazano, jak można zaktualizować bazy danych, który jest zarządzany przez migracje Code First, a następnie później pokazano, jak zaktualizować bazę danych przy użyciu dostawcy dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="831e7-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="831e7-111">Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="831e7-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="831e7-112">Wdrażanie aktualizacji bazy danych przy użyciu migracje Code First</span><span class="sxs-lookup"><span data-stu-id="831e7-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="831e7-113">W tej sekcji Dodaj kolumnę daty urodzenia `Person` klasa podstawowa dla `Student` i `Instructor` jednostek.</span><span class="sxs-lookup"><span data-stu-id="831e7-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="831e7-114">Następnie należy zaktualizować Strona która wyświetla dane przez instruktorów, tak, aby wyświetlał nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="831e7-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="831e7-115">Na koniec wdrożysz zmiany do testowania, przejściowego i produkcji.</span><span class="sxs-lookup"><span data-stu-id="831e7-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="831e7-116">Dodawanie kolumny do tabeli w bazie danych aplikacji</span><span class="sxs-lookup"><span data-stu-id="831e7-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="831e7-117">W *ContosoUniversity.DAL* otwarty projekt *osoba.cs* i dodaj następującą właściwość na końcu `Person` klasy (powinny istnieć dwie zamykający nawias klamrowy następujący):</span><span class="sxs-lookup"><span data-stu-id="831e7-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="831e7-118">Następnie zaktualizuj `Seed` metodę, tak że zawiera wartość dla nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="831e7-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="831e7-119">Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z poniższy blok kodu, który zawiera informacje o dacie urodzenia:</span><span class="sxs-lookup"><span data-stu-id="831e7-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="831e7-120">Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna.</span><span class="sxs-lookup"><span data-stu-id="831e7-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="831e7-121">Upewnij się, że ContosoUniversity.DAL jest nadal wybrany jako **projekt domyślny**.</span><span class="sxs-lookup"><span data-stu-id="831e7-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="831e7-122">W **Konsola Menedżera pakietów** wybierz **ContosoUniversity.DAL** jako **projekt domyślny**, a następnie wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="831e7-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="831e7-123">Po zakończeniu tego polecenia, programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` klasy, a następnie w `Up` metody zostanie wyświetlony kod, który tworzy nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="831e7-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="831e7-124">`Up` Metoda tworzy kolumny, wprowadzając zmiany, a `Down` metoda usuwa kolumny, gdy użytkownik jest wycofywanie zmian.</span><span class="sxs-lookup"><span data-stu-id="831e7-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="831e7-126">Skompiluj rozwiązanie, a następnie wprowadź następujące polecenie w **Konsola Menedżera pakietów** okna (Upewnij się, projekt ContosoUniversity.DAL została ona zaznaczona):</span><span class="sxs-lookup"><span data-stu-id="831e7-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="831e7-127">Uruchamia programu Entity Framework `Up` metody, a następnie uruchamia `Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="831e7-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="831e7-128">Wyświetlanie nowych kolumn na stronie instruktorów</span><span class="sxs-lookup"><span data-stu-id="831e7-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="831e7-129">W projekcie ContosoUniversity Otwórz *Instructors.aspx* i dodać nowe pole do szablonu, aby wyświetlić datę urodzenia.</span><span class="sxs-lookup"><span data-stu-id="831e7-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="831e7-130">Dodać go między tymi zatrudnienia przypisania daty i pakietu office:</span><span class="sxs-lookup"><span data-stu-id="831e7-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="831e7-131">(Jeśli wcięcia kodu o szerokość jest niezsynchronizowana, można nacisnąć klawisze CTRL-K, a następnie CTRL-D do automatycznego ponownego sformatowania pliku.)</span><span class="sxs-lookup"><span data-stu-id="831e7-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="831e7-132">Uruchom aplikację, a następnie kliknij przycisk **Instruktorzy** łącza.</span><span class="sxs-lookup"><span data-stu-id="831e7-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="831e7-133">Po załadowaniu strony zobaczysz, że ma nowe pole daty urodzenia.</span><span class="sxs-lookup"><span data-stu-id="831e7-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Strona Instruktorzy z datę urodzenia](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="831e7-135">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="831e7-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="831e7-136">Wdrażanie aktualizacji bazy danych</span><span class="sxs-lookup"><span data-stu-id="831e7-136">Deploy the database update</span></span>

1. <span data-ttu-id="831e7-137">W **Eksploratora rozwiązań** wybierz projekt ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="831e7-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="831e7-138">W **Web publikacja w pojedynczym kliknięciem** narzędzi, kliknij przycisk **testu** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="831e7-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="831e7-139">(Jeśli pasek narzędzi jest wyłączona, wybierz projekt ContosoUniversity **Eksploratora rozwiązań**.)</span><span class="sxs-lookup"><span data-stu-id="831e7-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="831e7-140">Program Visual Studio wdroży zaktualizowaną aplikację i otwarciu przeglądarki do strony głównej.</span><span class="sxs-lookup"><span data-stu-id="831e7-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="831e7-141">Uruchom **Instruktorzy** stronę, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="831e7-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="831e7-142">Gdy aplikacja podejmie próbę dostępu do bazy danych dla tej strony, Code First aktualizuje schemat bazy danych i uruchamia `Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="831e7-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="831e7-143">Jeśli zostanie wyświetlona strona, zobaczysz oczekiwany **Data urodzenia** kolumnie znajdują się daty w nim.</span><span class="sxs-lookup"><span data-stu-id="831e7-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="831e7-144">W **Web publikacja w pojedynczym kliknięciem** narzędzi, kliknij przycisk **przemieszczania** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="831e7-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="831e7-145">Uruchom **Instruktorzy** strony na etapie wdrażania przejściowego, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="831e7-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="831e7-146">W **Web publikacja w pojedynczym kliknięciem** narzędzi, kliknij przycisk **produkcji** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="831e7-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="831e7-147">Uruchom **Instruktorzy** strony w środowisku produkcyjnym, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="831e7-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="831e7-148">Do rzeczywistej produkcji aktualizacji aplikacji, która obejmuje zmianę w bazie danych zwykle także wykonywane w aplikacji w trybie offline podczas wdrażania za pomocą *aplikacji\_offline.htm*, jak pokazano w poprzednim samouczku.</span><span class="sxs-lookup"><span data-stu-id="831e7-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="831e7-149">Wdrażanie aktualizacji bazy danych przy użyciu dostawcy dbDacFx</span><span class="sxs-lookup"><span data-stu-id="831e7-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="831e7-150">W tej sekcji dodasz *komentarze* kolumny *użytkownika* tabeli w bazie danych członkostwa i utworzyć stronę, która umożliwia wyświetlanie i edytowanie komentarzy dla każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="831e7-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="831e7-151">Następnie możesz wdrożyć zmiany do testowania, przejściowego i produkcji.</span><span class="sxs-lookup"><span data-stu-id="831e7-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="831e7-152">Dodawanie kolumny do tabeli w bazie danych członkostwa</span><span class="sxs-lookup"><span data-stu-id="831e7-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="831e7-153">W programie Visual Studio, otwórz **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="831e7-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="831e7-154">Rozwiń **(localdb) \v11.0**, rozwiń węzeł **baz danych**, rozwiń węzeł **aspnet ContosoUniversity** (nie **aspnet-ContosoUniversity-Prod**) a następnie rozwiń węzeł **tabel**.</span><span class="sxs-lookup"><span data-stu-id="831e7-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="831e7-155">Jeśli nie widzisz **(localdb) \v11.0** w obszarze **programu SQL Server** węzła, kliknij prawym przyciskiem myszy **programu SQL Server** węzła i kliknij przycisk **dodawania serwera SQL**.</span><span class="sxs-lookup"><span data-stu-id="831e7-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="831e7-156">W **Połącz z serwerem** wprowadź okno dialogowe *(localdb) \v11.0* jako **nazwy serwera**, a następnie kliknij przycisk **Connect**.</span><span class="sxs-lookup"><span data-stu-id="831e7-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="831e7-157">Jeśli nie widzisz **aspnet ContosoUniversity**, uruchom projekt i zaloguj się przy użyciu *administratora* poświadczenia (hasło jest *devpwd*), a następnie Odśwież  **Eksplorator obiektów SQL Server** okna.</span><span class="sxs-lookup"><span data-stu-id="831e7-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="831e7-158">Kliknij prawym przyciskiem myszy **użytkowników** tabeli, a następnie kliknij przycisk **Projektant widoków**.</span><span class="sxs-lookup"><span data-stu-id="831e7-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Projektant widoków SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="831e7-160">W projektancie, Dodaj *komentarze* kolumny i przypisz ją *nvarchar(128)* i dopuszcza wartości null, a następnie kliknij przycisk **aktualizacji**.</span><span class="sxs-lookup"><span data-stu-id="831e7-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Dodawanie kolumny Comments](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="831e7-162">W **Podgląd aktualizacji bazy danych** kliknij **Aktualizuj bazę danych**.</span><span class="sxs-lookup"><span data-stu-id="831e7-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Podgląd aktualizacji bazy danych](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="831e7-164">Utwórz stronę do wyświetlania i edytowania nowej kolumny</span><span class="sxs-lookup"><span data-stu-id="831e7-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="831e7-165">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **konta** kliknij folder w projekcie ContosoUniversity **Dodaj**, a następnie kliknij przycisk **nowy element** .</span><span class="sxs-lookup"><span data-stu-id="831e7-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="831e7-166">Utwórz nową **strona wzorcowa za pomocą formularza sieci Web** i nadaj mu nazwę *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="831e7-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="831e7-167">Zaakceptuj wartość domyślną *Site.Master* pliku strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="831e7-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="831e7-168">Skopiuj następujący kod do `MainContent` `Content` — element (ostatnie 3 `Content` elementy):</span><span class="sxs-lookup"><span data-stu-id="831e7-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="831e7-169">Kliknij prawym przyciskiem myszy *UserInfo.aspx* strony, a następnie kliknij przycisk **Pokaż w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="831e7-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="831e7-170">Zaloguj się przy użyciu usługi *administratora* poświadczeń użytkownika (hasło jest *devpwd*) i Dodaj niektóre komentarze do użytkownika, aby sprawdzić, czy strona działa poprawnie.</span><span class="sxs-lookup"><span data-stu-id="831e7-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Strona informacje o użytkowniku](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="831e7-172">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="831e7-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="831e7-173">Wdrażanie aktualizacji bazy danych</span><span class="sxs-lookup"><span data-stu-id="831e7-173">Deploy the database update</span></span>

<span data-ttu-id="831e7-174">Aby wdrożyć przy użyciu dostawcy dbDacFx, wystarczy wybrać **Aktualizuj bazę danych** opcję w profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="831e7-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="831e7-175">Jednak do początkowego wdrożenia przypadku użycia tej opcji możesz również skonfigurowana tak, niektóre dodatkowe skrypty SQL, aby uruchomić: są nadal w profilu i należy uniemożliwić ich ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="831e7-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="831e7-176">Otwórz **publikowanie w sieci Web** kreatora, kliknij prawym przyciskiem myszy projekt ContosoUniversity i klikając **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="831e7-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="831e7-177">Wybierz **testu** profilu.</span><span class="sxs-lookup"><span data-stu-id="831e7-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="831e7-178">Kliknij przycisk **ustawienia** kartę.</span><span class="sxs-lookup"><span data-stu-id="831e7-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="831e7-179">W obszarze **DefaultConnection**, wybierz opcję **Aktualizuj bazę danych**.</span><span class="sxs-lookup"><span data-stu-id="831e7-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="831e7-180">Wyłącz dodatkowe skrypty, które skonfigurowano do uruchamiania do początkowego wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="831e7-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="831e7-181">Kliknij przycisk **Konfiguruj aktualizacje bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="831e7-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="831e7-182">W **Konfiguruj aktualizacje bazy danych** okno dialogowe, usuń zaznaczenie pola wyboru obok pozycji *Grant.sql* i *aspnet-data-dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="831e7-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="831e7-183">Kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="831e7-183">Click **Close**.</span></span>
6. <span data-ttu-id="831e7-184">Kliknij przycisk **Podgląd** kartę.</span><span class="sxs-lookup"><span data-stu-id="831e7-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="831e7-185">W obszarze **baz danych** i po prawej stronie **DefaultConnection**, kliknij przycisk **bazy danych w wersji zapoznawczej** łącza.</span><span class="sxs-lookup"><span data-stu-id="831e7-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Podgląd bazy danych](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="831e7-187">Okno podglądu zawiera skrypt, który zostanie wykonany w docelowej bazie danych, że schemat bazy danych, które pasuje do schematu źródłowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="831e7-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="831e7-188">Skrypt zawiera polecenia ALTER TABLE, który dodaje nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="831e7-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="831e7-189">Zamknij **bazy danych w wersji zapoznawczej** okno dialogowe, a następnie kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="831e7-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="831e7-190">Program Visual Studio wdroży zaktualizowaną aplikację i otwarciu przeglądarki do strony głównej.</span><span class="sxs-lookup"><span data-stu-id="831e7-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="831e7-191">Uruchom na stronie informacje o użytkowniku (Dodaj *Account/UserInfo.aspx* na adres URL strony głównej) aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="831e7-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="831e7-192">Musisz zalogować się, wprowadzając *administratora* i *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="831e7-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="831e7-193">Dane w tabelach nie są wdrażane domyślnie i nie skonfigurowała skryptu wdrażania danych, aby uruchomić, dzięki czemu nie odnajdzie komentarz, który dodaje w trakcie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="831e7-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="831e7-194">Możesz teraz dodać nowy komentarz na etapie wdrażania przejściowego, aby sprawdzić, czy zmiany zostały wdrożone względem bazy danych i strony działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="831e7-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="831e7-195">Wykonaj tę samą procedurę, aby wdrożyć przejściowym i produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="831e7-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="831e7-196">Należy pamiętać wyłączyć dodatkowe skrypty.</span><span class="sxs-lookup"><span data-stu-id="831e7-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="831e7-197">Jedyną różnicą w porównaniu do profilu testowego jest czy spowoduje wyłączenie tylko jeden skrypt w tymczasowej i profile produkcyjnych, ponieważ zostały skonfigurowane do uruchamiania tylko *aspnet-prod-data.sql*.</span><span class="sxs-lookup"><span data-stu-id="831e7-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="831e7-198">Poświadczenia dla środowisk przejściowych i produkcyjnych są prodpwd i administratora.</span><span class="sxs-lookup"><span data-stu-id="831e7-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="831e7-199">Do rzeczywistej produkcji aktualizacji aplikacji, która obejmuje zmianę w bazie danych, przekazując także zazwyczaj zajęłoby aplikacji w trybie offline podczas wdrażania *aplikacji\_offline.htm* przed opublikowaniem i usuwając je później, jak przedstawiono w [poprzedniego samouczka](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="831e7-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="831e7-200">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="831e7-200">Summary</span></span>

<span data-ttu-id="831e7-201">Teraz udało Ci się wdrożyć aktualizację aplikacji, który się przy użyciu zarówno migracje Code First, jak i dostawcy dbDacFx zmianę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="831e7-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Strona Instruktorzy z datę urodzenia](deploying-a-database-update/_static/image8.png)

![Strona informacje o użytkowniku](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="831e7-204">Następnym samouczku dowiesz się, jak wykonać wdrożenia przy użyciu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="831e7-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="831e7-205">[Poprzednie](deploying-a-code-update.md)
> [dalej](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="831e7-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
