---
title: Publikowanie aplikacji platformy ASP.NET Core na platformie Azure z programem Visual Studio
author: rick-anderson
description: Dowiedz się, jak opublikować aplikację ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2018
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: d713a520368ba86726e46203da5ea33eed4f6e2c
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284594"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio"></a><span data-ttu-id="d6bc6-103">Publikowanie aplikacji platformy ASP.NET Core na platformie Azure z programem Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6bc6-103">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>

<span data-ttu-id="d6bc6-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Silveira Blum Cesarowi](https://github.com/cesarbs)</span><span class="sxs-lookup"><span data-stu-id="d6bc6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="d6bc6-105">Zobacz [Opublikuj na platformie Azure z programu Visual Studio dla komputerów Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) Jeśli pracujesz w systemie macOS.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on macOS.</span></span>

<span data-ttu-id="d6bc6-106">Aby rozwiązać problem wdrożenia usługi App Service, zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-106">To troubleshoot an App Service deployment issue, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="set-up"></a><span data-ttu-id="d6bc6-107">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="d6bc6-107">Set up</span></span>

* <span data-ttu-id="d6bc6-108">Otwórz [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/dotnet/) Jeśli nie masz.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-108">Open a [free Azure account](https://azure.microsoft.com/free/dotnet/) if you don't have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="d6bc6-109">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="d6bc6-109">Create a web app</span></span>

<span data-ttu-id="d6bc6-110">W programie Visual Studio strony początkowej, wybierz **Plik > Nowy > Projekt...**</span><span class="sxs-lookup"><span data-stu-id="d6bc6-110">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![menu Plik](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="d6bc6-112">Wykonaj **nowy projekt** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="d6bc6-112">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="d6bc6-113">W okienku po lewej stronie wybierz **platformy .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-113">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="d6bc6-114">W środkowym okienku wybierz **aplikacji sieci Web programu ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-114">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="d6bc6-115">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-115">Select **OK**.</span></span>

![Okno dialogowe nowego projektu](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="d6bc6-117">W **Nowa aplikacja internetowa ASP.NET Core** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="d6bc6-117">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="d6bc6-118">Wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-118">Select **Web Application**.</span></span>
* <span data-ttu-id="d6bc6-119">Wybierz **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-119">Select **Change Authentication**.</span></span>

![Okno dialogowe nowego projektu](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="d6bc6-121">**Zmień uwierzytelnianie** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-121">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="d6bc6-122">Wybierz **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-122">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="d6bc6-123">Wybierz **OK** aby powrócić do **Nowa aplikacja internetowa ASP.NET Core**, a następnie wybierz **OK** ponownie.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-123">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Nowe okno dialogowe uwierzytelniania sieci Web platformy ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="d6bc6-125">Program Visual Studio tworzy rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-125">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="d6bc6-126">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d6bc6-126">Run the app</span></span>

* <span data-ttu-id="d6bc6-127">Naciśnij klawisze CTRL + F5, aby uruchomić projekt.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-127">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="d6bc6-128">Test **o** i **skontaktuj się z pomocą** łącza.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-128">Test the **About** and **Contact** links.</span></span>

![Aplikacja sieci Web otwórz w programie Microsoft Edge na hoście lokalnym](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="d6bc6-130">Rejestrowanie użytkownika</span><span class="sxs-lookup"><span data-stu-id="d6bc6-130">Register a user</span></span>

* <span data-ttu-id="d6bc6-131">Wybierz **zarejestrować** i rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="d6bc6-132">Można użyć adresu e-mail fikcyjne.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-132">You can use a fictitious email address.</span></span> <span data-ttu-id="d6bc6-133">Podczas przesyłania, zostanie wyświetlona strona następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="d6bc6-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="d6bc6-134">*"Wewnętrzny błąd serwera: Operacja bazy danych nie powiodło się podczas przetwarzania żądania. Wyjątek SQL: Nie można otworzyć bazy danych. Stosowanie migracji istniejących w kontekście bazy danych aplikacji może rozwiązać ten problem."*</span><span class="sxs-lookup"><span data-stu-id="d6bc6-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="d6bc6-135">Wybierz **zastosować migracje** i, po aktualizacji strony, Odśwież stronę.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Wewnętrzny błąd serwera: Operacja bazy danych nie powiodło się podczas przetwarzania żądania.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="d6bc6-139">Aplikacja wyświetla adres e-mail używany do rejestrowania nowych użytkowników i **Wyloguj** łącza.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Otwórz aplikację sieci Web w programie Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="d6bc6-142">Wdrażanie aplikacji na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="d6bc6-142">Deploy the app to Azure</span></span>

<span data-ttu-id="d6bc6-143">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **publikowania...** .</span><span class="sxs-lookup"><span data-stu-id="d6bc6-143">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu kontekstowe jest otwarte z wyróżnionym linkiem publikowania](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="d6bc6-145">W **Publikuj** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="d6bc6-145">In the **Publish** dialog:</span></span>

* <span data-ttu-id="d6bc6-146">Wybierz **platformy Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-146">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="d6bc6-147">Wybierz ikonę koła zębatego, a następnie wybierz pozycję **Utwórz profil**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-147">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="d6bc6-148">Wybierz **Utwórz profil**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-148">Select **Create Profile**.</span></span>

![Okno dialogowe publikowanie](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="d6bc6-150">Tworzenie zasobów platformy Azure</span><span class="sxs-lookup"><span data-stu-id="d6bc6-150">Create Azure resources</span></span>

<span data-ttu-id="d6bc6-151">**Tworzenie usługi App Service** zostanie wyświetlone okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="d6bc6-151">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="d6bc6-152">Wprowadź swoją subskrypcję.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-152">Enter your subscription.</span></span>
* <span data-ttu-id="d6bc6-153">**Nazwy aplikacji**, **grupy zasobów**, i **planu usługi App Service** zostaną wypełnione pola wprowadzania.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-153">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="d6bc6-154">Można zachować te nazwy lub je zmienić.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-154">You can keep these names or change them.</span></span>

![Okno dialogowe usługi App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="d6bc6-156">Wybierz **usług** kartę, aby utworzyć nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-156">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="d6bc6-157">Zaznacz zielony **+** ikonę, aby utworzyć nową bazę danych SQL</span><span class="sxs-lookup"><span data-stu-id="d6bc6-157">Select the green **+** icon to create a new SQL Database</span></span>

![Nowa baza danych SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="d6bc6-159">Wybierz **nowy...**  na **Konfigurowanie bazy danych SQL** okno dialogowe, aby utworzyć nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-159">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nowe bazy danych SQL Database i serwera](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="d6bc6-161">**Konfiguruj serwer SQL** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-161">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="d6bc6-162">Wprowadź nazwę użytkownika administratora i hasło, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-162">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="d6bc6-163">Domyślne można zachować **nazwy serwera**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-163">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="d6bc6-164">"admin" nie jest dozwolona jako nazwa użytkownika administratora.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-164">"admin" isn't allowed as the administrator user name.</span></span>

![Konfigurowanie programu SQL Server w oknie dialogowym](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="d6bc6-166">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-166">Select **OK**.</span></span>

<span data-ttu-id="d6bc6-167">Program Visual Studio zwraca **Tworzenie usługi App Service** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="d6bc6-168">Wybierz **Utwórz** na **Tworzenie usługi App Service** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-168">Select **Create** on the **Create App Service** dialog.</span></span>

![Konfigurowanie okna dialogowego baza danych SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="d6bc6-170">Visual Studio tworzy aplikację sieci Web i SQL Server na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-170">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="d6bc6-171">Może to potrwać kilka minut.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-171">This step can take a few minutes.</span></span> <span data-ttu-id="d6bc6-172">Aby uzyskać informacji na temat tworzenia zasobów, zobacz [dodatkowe zasoby](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="d6bc6-172">For information on the resources created, see [Additional resources](#additonal-resources).</span></span>

<span data-ttu-id="d6bc6-173">Po zakończeniu wdrażania wybierz **ustawienia**:</span><span class="sxs-lookup"><span data-stu-id="d6bc6-173">When deployment completes, select **Settings**:</span></span>

![Konfigurowanie programu SQL Server w oknie dialogowym](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="d6bc6-175">Na **ustawienia** strony **Publikuj** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="d6bc6-175">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="d6bc6-176">Rozwiń **baz danych** i sprawdź **Użyj tych parametrów połączenia w czasie wykonywania**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-176">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="d6bc6-177">Rozwiń **migracją architektury jednostek** i sprawdź **Zastosuj tej migracji na publikowanie**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-177">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="d6bc6-178">Wybierz pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-178">Select **Save**.</span></span> <span data-ttu-id="d6bc6-179">Program Visual Studio zwraca **Publikuj** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-179">Visual Studio returns to the **Publish** dialog.</span></span> 

![Okno dialogowe publikowanie: Panel ustawień](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="d6bc6-181">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-181">Click **Publish**.</span></span> <span data-ttu-id="d6bc6-182">Program Visual Studio publikuje aplikację na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-182">Visual Studio publishes your app to Azure.</span></span> <span data-ttu-id="d6bc6-183">Po zakończeniu wdrażania aplikacji jest otwarty w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-183">When the deployment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="d6bc6-184">Przetestuj swoją aplikację na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="d6bc6-184">Test your app in Azure</span></span>

* <span data-ttu-id="d6bc6-185">Test **o** i **skontaktuj się z pomocą** łącza</span><span class="sxs-lookup"><span data-stu-id="d6bc6-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="d6bc6-186">Rejestrowanie nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="d6bc6-186">Register a new user</span></span>

![Aplikacja sieci Web otwarte w programie Microsoft Edge w usłudze Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="d6bc6-188">Aktualizowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d6bc6-188">Update the app</span></span>

* <span data-ttu-id="d6bc6-189">Edytuj *Pages/About.cshtml* Razor strony i zmienić jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-189">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="d6bc6-190">Na przykład można zmodyfikować akapitu powiedzieć "Hello platformy ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="d6bc6-190">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="d6bc6-191">Kliknij prawym przyciskiem myszy projekt i wybierz pozycję **publikowania...**  ponownie.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-191">Right-click on the project and select **Publish...** again.</span></span>

![Menu kontekstowe jest otwarte z wyróżnionym linkiem publikowania](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="d6bc6-193">Po opublikowaniu aplikacji, sprawdź, czy dokonane zmiany są dostępne na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-193">After the app is published, verify the changes you made are available on Azure.</span></span>

![Sprawdź, czy zadanie zostało ukończone](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="d6bc6-195">Czyszczenie</span><span class="sxs-lookup"><span data-stu-id="d6bc6-195">Clean up</span></span>

<span data-ttu-id="d6bc6-196">Po zakończeniu testowania aplikacji, przejdź do [witryny Azure portal](https://portal.azure.com/) i usunąć aplikację.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-196">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="d6bc6-197">Wybierz **grup zasobów**, następnie wybierz utworzoną grupę zasobów.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-197">Select **Resource groups**, then select the resource group you created.</span></span>

![Witryna Azure Portal: Grupy zasobów, w menu bocznym](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="d6bc6-199">W **grup zasobów** wybierz opcję **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-199">In the **Resource groups** page, select **Delete**.</span></span>

![Witryna Azure Portal: Strony grup zasobów](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="d6bc6-201">Wprowadź nazwę grupy zasobów, a następnie wybierz pozycję **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-201">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="d6bc6-202">Aplikacja i inne zasoby utworzone w ramach tego samouczka, teraz są usuwane z usługi Azure.</span><span class="sxs-lookup"><span data-stu-id="d6bc6-202">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="d6bc6-203">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="d6bc6-203">Next steps</span></span>

* <xref:host-and-deploy/azure-apps/azure-continuous-deployment>

## <a name="additonal-resources"></a><span data-ttu-id="d6bc6-204">Zasoby dodatkowe</span><span class="sxs-lookup"><span data-stu-id="d6bc6-204">Additonal resources</span></span>

* [<span data-ttu-id="d6bc6-205">Usługa Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d6bc6-205">Azure App Service</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="d6bc6-206">Grupy zasobów platformy Azure</span><span class="sxs-lookup"><span data-stu-id="d6bc6-206">Azure resource groups</span></span>](/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="d6bc6-207">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="d6bc6-207">Azure SQL Database</span></span>](/azure/sql-database/)
* <xref:host-and-deploy/visual-studio-publish-profiles>
* <xref:host-and-deploy/azure-apps/troubleshoot>
