---
title: Publikowanie aplikacji platformy ASP.NET Core dla platformy Azure z programem Visual Studio
author: rick-anderson
description: Dowiedz się, jak opublikować aplikację platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 12/16/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: e5c213a682c9bf7c64c40fad630cacfff24e23bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897629"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio"></a><span data-ttu-id="13970-103">Publikowanie aplikacji platformy ASP.NET Core dla platformy Azure z programem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13970-103">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>

<span data-ttu-id="13970-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Silveira Blum Cesarowi](https://github.com/cesarbs), i [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="13970-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="13970-105">Zobacz [publikowania na platformie Azure w programie Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) Jeśli pracujesz nad macOS.</span><span class="sxs-lookup"><span data-stu-id="13970-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on macOS.</span></span>

<span data-ttu-id="13970-106">Aby rozwiązać problem wdrażania usługi aplikacji, zobacz [Rozwiązywanie problemów z platformy ASP.NET Core w usłudze Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="13970-106">To troubleshoot an App Service deployment issue, see [Troubleshoot ASP.NET Core on Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot).</span></span>

## <a name="set-up"></a><span data-ttu-id="13970-107">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="13970-107">Set up</span></span>

* <span data-ttu-id="13970-108">Otwórz [bezpłatne konto platformy Azure](https://aka.ms/K5y5yh) Jeśli nie masz.</span><span class="sxs-lookup"><span data-stu-id="13970-108">Open a [free Azure account](https://aka.ms/K5y5yh) if you don't have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="13970-109">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="13970-109">Create a web app</span></span>

<span data-ttu-id="13970-110">W Visual Studio — strona początkowa, wybierz **Plik > Nowy > Projekt...**</span><span class="sxs-lookup"><span data-stu-id="13970-110">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![menu Plik](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="13970-112">Zakończenie **nowy projekt** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="13970-112">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="13970-113">W okienku po lewej stronie wybierz **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="13970-113">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="13970-114">W środkowym okienku wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="13970-114">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="13970-115">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="13970-115">Select **OK**.</span></span>

![Okno dialogowe nowego projektu](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="13970-117">W **nową aplikację sieci Web Core ASP.NET** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="13970-117">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="13970-118">Wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="13970-118">Select **Web Application**.</span></span>
* <span data-ttu-id="13970-119">Wybierz **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="13970-119">Select **Change Authentication**.</span></span>

![Okno dialogowe nowego projektu](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="13970-121">**Zmień uwierzytelnianie** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="13970-121">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="13970-122">Wybierz **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="13970-122">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="13970-123">Wybierz **OK** aby powrócić do **nową aplikację sieci Web Core ASP.NET**, a następnie wybierz pozycję **OK** ponownie.</span><span class="sxs-lookup"><span data-stu-id="13970-123">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Okno dialogowe nowego uwierzytelniania sieci Web platformy ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="13970-125">Program Visual Studio tworzy rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="13970-125">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="13970-126">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="13970-126">Run the app</span></span>

* <span data-ttu-id="13970-127">Naciśnij klawisze CTRL + F5, aby uruchomić projekt.</span><span class="sxs-lookup"><span data-stu-id="13970-127">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="13970-128">Test **o** i **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="13970-128">Test the **About** and **Contact** links.</span></span>

![Aplikacja sieci Web otwórz w programie Microsoft Edge na hoście lokalnym](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="13970-130">Zarejestruj użytkownika</span><span class="sxs-lookup"><span data-stu-id="13970-130">Register a user</span></span>

* <span data-ttu-id="13970-131">Wybierz **zarejestrować** i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="13970-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="13970-132">Można użyć adresu e-mail fikcyjne.</span><span class="sxs-lookup"><span data-stu-id="13970-132">You can use a fictitious email address.</span></span> <span data-ttu-id="13970-133">Podczas przesyłania, zostanie wyświetlona strona następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="13970-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="13970-134">*"Wewnętrzny błąd serwera: Operacja bazy danych nie powiodło się podczas przetwarzania żądania. Wyjątku SQL: nie można otworzyć bazy danych. Stosowanie istniejących migracje dla kontekst bazy danych aplikacji może rozwiązać ten problem."*</span><span class="sxs-lookup"><span data-stu-id="13970-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="13970-135">Wybierz **zastosować migracje** i po aktualizacji strony, Odśwież stronę.</span><span class="sxs-lookup"><span data-stu-id="13970-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Wewnętrzny błąd serwera: Operacja bazy danych nie powiodła się podczas przetwarzania żądania.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="13970-139">Aplikacja wyświetla wiadomości e-mail używany do rejestrowania nowego użytkownika i **Wyloguj się** łącza.</span><span class="sxs-lookup"><span data-stu-id="13970-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Otwórz aplikację sieci Web w programie Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="13970-142">Wdrażanie aplikacji na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="13970-142">Deploy the app to Azure</span></span>

<span data-ttu-id="13970-143">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **publikowania...** .</span><span class="sxs-lookup"><span data-stu-id="13970-143">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu kontekstowe Otwórz za pomocą łącza publikowania wyróżnione](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="13970-145">W **publikowania** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="13970-145">In the **Publish** dialog:</span></span>

* <span data-ttu-id="13970-146">Wybierz **usługi Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="13970-146">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="13970-147">Wybierz ikonę Koło zębate, a następnie wybierz **Utwórz profil**.</span><span class="sxs-lookup"><span data-stu-id="13970-147">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="13970-148">Wybierz **Utwórz profil**.</span><span class="sxs-lookup"><span data-stu-id="13970-148">Select **Create Profile**.</span></span>

![Okno dialogowe publikowania](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="13970-150">Utworzenie zasobów platformy Azure</span><span class="sxs-lookup"><span data-stu-id="13970-150">Create Azure resources</span></span>

<span data-ttu-id="13970-151">**Tworzenie usługi App Service** zostanie wyświetlone okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="13970-151">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="13970-152">Wprowadź swoją subskrypcję.</span><span class="sxs-lookup"><span data-stu-id="13970-152">Enter your subscription.</span></span>
* <span data-ttu-id="13970-153">**Nazwa aplikacji**, **grupy zasobów**, i **planu usługi App Service** pola wejścia zostaną wypełnione.</span><span class="sxs-lookup"><span data-stu-id="13970-153">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="13970-154">Można zachować te nazwy lub je zmienić.</span><span class="sxs-lookup"><span data-stu-id="13970-154">You can keep these names or change them.</span></span>

![Usługi aplikacji — okno dialogowe](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="13970-156">Wybierz **usług** kartę, aby utworzyć nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="13970-156">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="13970-157">Wybierz zielonego **+** ikonę, aby utworzyć nową bazę danych SQL</span><span class="sxs-lookup"><span data-stu-id="13970-157">Select the green **+** icon to create a new SQL Database</span></span>

![Nowej bazy danych SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="13970-159">Wybierz **nowych...**  na **Konfigurowanie bazy danych SQL** okna dialogowego, aby utworzyć nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="13970-159">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nowe bazy danych SQL i serwera](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="13970-161">**Konfiguruj serwer SQL** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="13970-161">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="13970-162">Wprowadź nazwę użytkownika administratora i hasło, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="13970-162">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="13970-163">Domyślne można zachować **nazwy serwera**.</span><span class="sxs-lookup"><span data-stu-id="13970-163">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="13970-164">"Administrator" nie jest dozwolona jako nazwa użytkownika administratora.</span><span class="sxs-lookup"><span data-stu-id="13970-164">"admin" isn't allowed as the administrator user name.</span></span>

![Konfigurowanie okna dialogowego programu SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="13970-166">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="13970-166">Select **OK**.</span></span>

<span data-ttu-id="13970-167">Visual Studio zwraca do **Tworzenie usługi App Service** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="13970-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="13970-168">Wybierz **Utwórz** na **Tworzenie usługi App Service** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="13970-168">Select **Create** on the **Create App Service** dialog.</span></span>

![Konfigurowanie bazy danych SQL w oknie dialogowym](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="13970-170">Visual Studio tworzy aplikację sieci Web i programu SQL Server na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="13970-170">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="13970-171">Ten krok może potrwać kilka minut.</span><span class="sxs-lookup"><span data-stu-id="13970-171">This step can take a few minutes.</span></span> <span data-ttu-id="13970-172">Aby uzyskać informacji na temat tworzenia zasobów, zobacz [zasoby dodatkowe](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="13970-172">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="13970-173">Po zakończeniu wdrożenia, wybierz **ustawienia**:</span><span class="sxs-lookup"><span data-stu-id="13970-173">When deployment completes, select **Settings**:</span></span>

![Konfigurowanie okna dialogowego programu SQL Server](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="13970-175">Na **ustawienia** strony **publikowania** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="13970-175">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="13970-176">Rozwiń węzeł **baz danych** i sprawdź **Użyj tych parametrów połączenia w czasie wykonywania**.</span><span class="sxs-lookup"><span data-stu-id="13970-176">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="13970-177">Rozwiń węzeł **Entity Framework migracje** i sprawdź **Zastosuj publikowania tej migracji na**.</span><span class="sxs-lookup"><span data-stu-id="13970-177">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="13970-178">Wybierz **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="13970-178">Select **Save**.</span></span> <span data-ttu-id="13970-179">Visual Studio zwraca do **publikowania** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="13970-179">Visual Studio returns to the **Publish** dialog.</span></span> 

![Okno dialogowe publikowania: panel ustawień](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="13970-181">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="13970-181">Click **Publish**.</span></span> <span data-ttu-id="13970-182">Visual Studio publishs aplikacji na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="13970-182">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="13970-183">Po zakończeniu wdrożenia aplikacji jest otwarty w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="13970-183">When the deployment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="13970-184">Testowanie aplikacji na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="13970-184">Test your app in Azure</span></span>

* <span data-ttu-id="13970-185">Test **o** i **skontaktuj się z** łącza</span><span class="sxs-lookup"><span data-stu-id="13970-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="13970-186">Zarejestruj nowy użytkownik</span><span class="sxs-lookup"><span data-stu-id="13970-186">Register a new user</span></span>

![Aplikacja sieci Web otworzyć w programie Microsoft Edge w usłudze Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="13970-188">Aktualizowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="13970-188">Update the app</span></span>

* <span data-ttu-id="13970-189">Edytuj *Pages/About.cshtml* Razor strony i zmienić jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="13970-189">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="13970-190">Na przykład można zmodyfikować akapitu znaczy "Hello platformy ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="13970-190">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="13970-191">Kliknij prawym przyciskiem myszy na projekt i wybierz **publikowania...**  ponownie.</span><span class="sxs-lookup"><span data-stu-id="13970-191">Right-click on the project and select **Publish...** again.</span></span>

![Menu kontekstowe Otwórz za pomocą łącza publikowania wyróżnione](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="13970-193">Po opublikowaniu aplikacji, sprawdź, czy dokonane zmiany są dostępne na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="13970-193">After the app is published, verify the changes you made are available on Azure.</span></span>

![Sprawdź, czy zadanie zostało ukończone](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="13970-195">Czyszczenie</span><span class="sxs-lookup"><span data-stu-id="13970-195">Clean up</span></span>

<span data-ttu-id="13970-196">Po zakończeniu testowania aplikacji, przejdź do [portalu Azure](https://portal.azure.com/) i usuwania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="13970-196">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="13970-197">Wybierz **grup zasobów**, następnie wybierz utworzoną grupę zasobów.</span><span class="sxs-lookup"><span data-stu-id="13970-197">Select **Resource groups**, then select the resource group you created.</span></span>

![Portalu Azure: Grupy zasobów w menu bocznym](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="13970-199">W **grup zasobów** wybierz pozycję **usunąć**.</span><span class="sxs-lookup"><span data-stu-id="13970-199">In the **Resource groups** page, select **Delete**.</span></span>

![Portalu Azure: Strona grup zasobów](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="13970-201">Wprowadź nazwę grupy zasobów i wybierz **usunąć**.</span><span class="sxs-lookup"><span data-stu-id="13970-201">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="13970-202">Aplikacji i innych zasobów utworzonej w tym samouczku są teraz usuwane z platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="13970-202">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="13970-203">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="13970-203">Next steps</span></span>

* [<span data-ttu-id="13970-204">Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i Git</span><span class="sxs-lookup"><span data-stu-id="13970-204">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="13970-205">Zasoby dodatkowe</span><span class="sxs-lookup"><span data-stu-id="13970-205">Additonal resources</span></span>

* [<span data-ttu-id="13970-206">Usługa aplikacji Azure</span><span class="sxs-lookup"><span data-stu-id="13970-206">Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="13970-207">Grup zasobów platformy Azure</span><span class="sxs-lookup"><span data-stu-id="13970-207">Azure resource groups</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="13970-208">Baza danych Azure SQL</span><span class="sxs-lookup"><span data-stu-id="13970-208">Azure SQL Database</span></span>](https://docs.microsoft.com/azure/sql-database/)
* [<span data-ttu-id="13970-209">Rozwiązywanie problemów z platformą ASP.NET Core w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="13970-209">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
