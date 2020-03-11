---
title: Publikowanie aplikacji platformy ASP.NET Core na platformie Azure z programem Visual Studio
author: rick-anderson
description: Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 7fc3644df3dcb957f2537538aaa9506c6b38a480
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662205"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio"></a><span data-ttu-id="00924-103">Publikowanie aplikacji platformy ASP.NET Core na platformie Azure z programem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="00924-103">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>

<span data-ttu-id="00924-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="00924-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>
::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

::: moniker-end


<span data-ttu-id="00924-105">Zobacz [publikowanie aplikacji sieci Web, aby Azure App Service przy użyciu Visual Studio dla komputerów Mac](https://docs.microsoft.com/visualstudio/mac/publish-app-svc?view=vsmac-2019) , jeśli pracujesz na macOS.</span><span class="sxs-lookup"><span data-stu-id="00924-105">See [Publish a Web app to Azure App Service using Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/publish-app-svc?view=vsmac-2019) if you are working on macOS.</span></span>

<span data-ttu-id="00924-106">Aby rozwiązać problem z wdrożeniem App Service, zobacz <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="00924-106">To troubleshoot an App Service deployment issue, see <xref:test/troubleshoot-azure-iis>.</span></span>

## <a name="set-up"></a><span data-ttu-id="00924-107">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="00924-107">Set up</span></span>

* <span data-ttu-id="00924-108">Otwórz [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/dotnet/) , jeśli go nie masz.</span><span class="sxs-lookup"><span data-stu-id="00924-108">Open a [free Azure account](https://azure.microsoft.com/free/dotnet/) if you don't have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="00924-109">Tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="00924-109">Create a web app</span></span>

<span data-ttu-id="00924-110">Na stronie startowej programu Visual Studio wybierz pozycję **plik > nowy > projekt...**</span><span class="sxs-lookup"><span data-stu-id="00924-110">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Menu Plik](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="00924-112">Wypełnij okno dialogowe **Nowy projekt** :</span><span class="sxs-lookup"><span data-stu-id="00924-112">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="00924-113">W lewym okienku wybierz pozycję **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="00924-113">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="00924-114">W środkowym okienku wybierz pozycję **aplikacja sieci Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="00924-114">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="00924-115">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="00924-115">Select **OK**.</span></span>

![Okno dialogowe Nowy projekt](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="00924-117">W oknie dialogowym **Nowa aplikacja sieci Web ASP.NET Core** :</span><span class="sxs-lookup"><span data-stu-id="00924-117">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="00924-118">Wybierz pozycję **aplikacja sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="00924-118">Select **Web Application**.</span></span>
* <span data-ttu-id="00924-119">Wybierz pozycję **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="00924-119">Select **Change Authentication**.</span></span>

![Okno dialogowe Nowy projekt](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="00924-121">Zostanie wyświetlone okno dialogowe **Zmienianie uwierzytelniania** .</span><span class="sxs-lookup"><span data-stu-id="00924-121">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="00924-122">Wybierz pozycję **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="00924-122">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="00924-123">Wybierz **przycisk OK** , aby powrócić do **nowej aplikacji sieci Web ASP.NET Core**, a następnie ponownie wybierz przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="00924-123">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Nowe okno dialogowe uwierzytelniania sieci Web platformy ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="00924-125">Program Visual Studio tworzy rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="00924-125">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="00924-126">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="00924-126">Run the app</span></span>

* <span data-ttu-id="00924-127">Naciśnij klawisze CTRL + F5, aby uruchomić projekt.</span><span class="sxs-lookup"><span data-stu-id="00924-127">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="00924-128">Przetestuj linki **informacje** i **kontakt** .</span><span class="sxs-lookup"><span data-stu-id="00924-128">Test the **About** and **Contact** links.</span></span>

![Aplikacja sieci Web otwórz w programie Microsoft Edge na hoście lokalnym](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="00924-130">Rejestrowanie użytkownika</span><span class="sxs-lookup"><span data-stu-id="00924-130">Register a user</span></span>

* <span data-ttu-id="00924-131">Wybierz pozycję **zarejestruj** i Zarejestruj nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="00924-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="00924-132">Można użyć adresu e-mail fikcyjne.</span><span class="sxs-lookup"><span data-stu-id="00924-132">You can use a fictitious email address.</span></span> <span data-ttu-id="00924-133">Podczas przesyłania, zostanie wyświetlona strona następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="00924-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="00924-134">*"Wewnętrzny błąd serwera: operacja bazy danych nie powiodła się podczas przetwarzania żądania. Wyjątek SQL: nie można otworzyć bazy danych. Zastosowanie istniejących migracji dla kontekstu bazy danych aplikacji może rozwiązać ten problem.*</span><span class="sxs-lookup"><span data-stu-id="00924-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="00924-135">Wybierz pozycję **Zastosuj migracje** , a po aktualizacji strony Odśwież stronę.</span><span class="sxs-lookup"><span data-stu-id="00924-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Wewnętrzny błąd serwera: Operacja bazy danych nie powiodło się podczas przetwarzania żądania.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="00924-139">W aplikacji zostanie wyświetlona wiadomość e-mail służąca do zarejestrowania nowego użytkownika i linku **wylogowywania** .</span><span class="sxs-lookup"><span data-stu-id="00924-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Otwórz aplikację sieci Web w programie Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="00924-142">Wdrażanie aplikacji na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="00924-142">Deploy the app to Azure</span></span>

<span data-ttu-id="00924-143">Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań i wybierz pozycję **Publikuj...** .</span><span class="sxs-lookup"><span data-stu-id="00924-143">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu kontekstowe jest otwarte z wyróżnionym linkiem publikowania](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="00924-145">W oknie dialogowym **publikowania** :</span><span class="sxs-lookup"><span data-stu-id="00924-145">In the **Publish** dialog:</span></span>

* <span data-ttu-id="00924-146">Wybierz **App Service Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="00924-146">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="00924-147">Wybierz ikonę koła zębatego, a następnie wybierz pozycję **Utwórz profil**.</span><span class="sxs-lookup"><span data-stu-id="00924-147">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="00924-148">Wybierz pozycję **Utwórz profil**.</span><span class="sxs-lookup"><span data-stu-id="00924-148">Select **Create Profile**.</span></span>

![Okno dialogowe publikowanie](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="00924-150">Tworzenie zasobów platformy Azure</span><span class="sxs-lookup"><span data-stu-id="00924-150">Create Azure resources</span></span>

<span data-ttu-id="00924-151">Zostanie wyświetlone okno dialogowe **tworzenie App Service** :</span><span class="sxs-lookup"><span data-stu-id="00924-151">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="00924-152">Wprowadź swoją subskrypcję.</span><span class="sxs-lookup"><span data-stu-id="00924-152">Enter your subscription.</span></span>
* <span data-ttu-id="00924-153">Pola **Nazwa aplikacji**, **Grupa zasobów**i zapis **planu App Service** są wypełniane.</span><span class="sxs-lookup"><span data-stu-id="00924-153">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="00924-154">Można zachować te nazwy lub je zmienić.</span><span class="sxs-lookup"><span data-stu-id="00924-154">You can keep these names or change them.</span></span>

![Okno dialogowe usługi App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="00924-156">Wybierz kartę **usługi** , aby utworzyć nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="00924-156">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="00924-157">Wybierz ikonę zielonej **+** , aby utworzyć nowy SQL Database</span><span class="sxs-lookup"><span data-stu-id="00924-157">Select the green **+** icon to create a new SQL Database</span></span>

![Nowa baza danych SQL Database](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="00924-159">Wybierz pozycję **Nowy...** w oknie dialogowym **Konfigurowanie SQL Database** , aby utworzyć nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="00924-159">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nowe bazy danych SQL Database i serwera](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="00924-161">Zostanie wyświetlone okno dialogowe **konfigurowanie SQL Server** .</span><span class="sxs-lookup"><span data-stu-id="00924-161">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="00924-162">Wprowadź nazwę użytkownika i hasło administratora, a następnie wybierz przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="00924-162">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="00924-163">Można zachować domyślną **nazwę serwera**.</span><span class="sxs-lookup"><span data-stu-id="00924-163">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="00924-164">"admin" nie jest dozwolona jako nazwa użytkownika administratora.</span><span class="sxs-lookup"><span data-stu-id="00924-164">"admin" isn't allowed as the administrator user name.</span></span>

![Konfigurowanie programu SQL Server w oknie dialogowym](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="00924-166">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="00924-166">Select **OK**.</span></span>

<span data-ttu-id="00924-167">Program Visual Studio powróci do okna dialogowego **tworzenie App Service** .</span><span class="sxs-lookup"><span data-stu-id="00924-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="00924-168">Wybierz pozycję **Utwórz** w oknie dialogowym **Tworzenie App Service** .</span><span class="sxs-lookup"><span data-stu-id="00924-168">Select **Create** on the **Create App Service** dialog.</span></span>

![Konfigurowanie okna dialogowego baza danych SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="00924-170">Visual Studio tworzy aplikację sieci Web i SQL Server na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="00924-170">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="00924-171">Może to potrwać kilka minut.</span><span class="sxs-lookup"><span data-stu-id="00924-171">This step can take a few minutes.</span></span> <span data-ttu-id="00924-172">Aby uzyskać informacje o utworzonych zasobach, zobacz [dodatkowe zasoby](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="00924-172">For information on the resources created, see [Additional resources](#additional-resources).</span></span>

<span data-ttu-id="00924-173">Po zakończeniu wdrażania wybierz pozycję **Ustawienia**:</span><span class="sxs-lookup"><span data-stu-id="00924-173">When deployment completes, select **Settings**:</span></span>

![Konfigurowanie programu SQL Server w oknie dialogowym](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="00924-175">Na stronie **Ustawienia** okna dialogowego **Publikowanie** :</span><span class="sxs-lookup"><span data-stu-id="00924-175">On the **Settings** page of the **Publish** dialog:</span></span>

* <span data-ttu-id="00924-176">Rozwiń węzeł **bazy danych** i zaznacz pole wyboru **Użyj tych parametrów połączenia w czasie wykonywania**.</span><span class="sxs-lookup"><span data-stu-id="00924-176">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
* <span data-ttu-id="00924-177">Rozwiń węzeł **Entity Framework migracje** i zaznacz opcję **Zastosuj tę migrację przy publikowaniu**.</span><span class="sxs-lookup"><span data-stu-id="00924-177">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="00924-178">Wybierz pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="00924-178">Select **Save**.</span></span> <span data-ttu-id="00924-179">Program Visual Studio wraca do okna dialogowego **Publikowanie** .</span><span class="sxs-lookup"><span data-stu-id="00924-179">Visual Studio returns to the **Publish** dialog.</span></span> 

![Okno dialogowe publikowanie: panel ustawień](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="00924-181">Kliknij przycisk **Opublikuj**.</span><span class="sxs-lookup"><span data-stu-id="00924-181">Click **Publish**.</span></span> <span data-ttu-id="00924-182">Program Visual Studio publikuje aplikację na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="00924-182">Visual Studio publishes your app to Azure.</span></span> <span data-ttu-id="00924-183">Po zakończeniu wdrażania aplikacji jest otwarty w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="00924-183">When the deployment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="00924-184">Testowanie aplikacji na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="00924-184">Test your app in Azure</span></span>

* <span data-ttu-id="00924-185">Testowanie linków **about** i **Contact**</span><span class="sxs-lookup"><span data-stu-id="00924-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="00924-186">Rejestrowanie nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="00924-186">Register a new user</span></span>

![Aplikacja sieci Web otwarte w programie Microsoft Edge w usłudze Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="00924-188">Aktualizowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="00924-188">Update the app</span></span>

* <span data-ttu-id="00924-189">Edytuj stronę Razor strony */informacje. cshtml* i zmień jej zawartość.</span><span class="sxs-lookup"><span data-stu-id="00924-189">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="00924-190">Na przykład można zmodyfikować akapit, aby powiedzieć "Hello ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="00924-190">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="00924-191">Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Opublikuj...** ponownie.</span><span class="sxs-lookup"><span data-stu-id="00924-191">Right-click on the project and select **Publish...** again.</span></span>

![Menu kontekstowe jest otwarte z wyróżnionym linkiem publikowania](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="00924-193">Po opublikowaniu aplikacji, sprawdź, czy dokonane zmiany są dostępne na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="00924-193">After the app is published, verify the changes you made are available on Azure.</span></span>

![Sprawdź, czy zadanie zostało ukończone](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="00924-195">Czyszczenie</span><span class="sxs-lookup"><span data-stu-id="00924-195">Clean up</span></span>

<span data-ttu-id="00924-196">Po zakończeniu testowania aplikacji przejdź do [Azure Portal](https://portal.azure.com/) i Usuń aplikację.</span><span class="sxs-lookup"><span data-stu-id="00924-196">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="00924-197">Wybierz pozycję **grupy zasobów**, a następnie wybierz utworzoną grupę zasobów.</span><span class="sxs-lookup"><span data-stu-id="00924-197">Select **Resource groups**, then select the resource group you created.</span></span>

![Witrynie Azure Portal: Grup zasobów w menu bocznym](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="00924-199">Na stronie **grupy zasobów** wybierz pozycję **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="00924-199">In the **Resource groups** page, select **Delete**.</span></span>

![Witrynie Azure Portal: Strony grup zasobów](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="00924-201">Wprowadź nazwę grupy zasobów i wybierz pozycję **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="00924-201">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="00924-202">Aplikacja i inne zasoby utworzone w ramach tego samouczka, teraz są usuwane z usługi Azure.</span><span class="sxs-lookup"><span data-stu-id="00924-202">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="00924-203">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="00924-203">Next steps</span></span>

* <xref:host-and-deploy/azure-apps/azure-continuous-deployment>

## <a name="additional-resources"></a><span data-ttu-id="00924-204">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="00924-204">Additional resources</span></span>

* <span data-ttu-id="00924-205">Aby uzyskać Visual Studio Code, zobacz temat [Publikowanie profilów](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="00924-205">For Visual Studio Code, see [Publish profiles](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span></span>
* [<span data-ttu-id="00924-206">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="00924-206">Azure App Service</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="00924-207">Grupy zasobów platformy Azure</span><span class="sxs-lookup"><span data-stu-id="00924-207">Azure resource groups</span></span>](/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="00924-208">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="00924-208">Azure SQL Database</span></span>](/azure/sql-database/)
* <xref:host-and-deploy/visual-studio-publish-profiles>
* <xref:test/troubleshoot-azure-iis>
