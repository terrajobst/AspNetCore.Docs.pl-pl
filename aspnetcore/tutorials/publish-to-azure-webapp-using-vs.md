---
title: "Publikowanie aplikacji platformy ASP.NET Core dla platformy Azure przy użyciu programu Visual Studio"
author: rick-anderson
description: 
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 6f697ed4d8876a19cd058533e4f6a5d4f7cdc2fb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="54a41-103">Publikowanie aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="54a41-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="54a41-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Silveira Blum Cesarowi](https://github.com/cesarbs), i [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="54a41-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="54a41-105">Zobacz [publikowania na platformie Azure w programie Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) podczas pracy na komputerach Mac.</span><span class="sxs-lookup"><span data-stu-id="54a41-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="54a41-106">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="54a41-106">Set up</span></span>

* <span data-ttu-id="54a41-107">Otwórz [bezpłatne konto platformy Azure](https://aka.ms/K5y5yh) Jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="54a41-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="54a41-108">Tworzenie aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="54a41-108">Create a web app</span></span>

<span data-ttu-id="54a41-109">W Visual Studio — strona początkowa, wybierz **Plik > Nowy > Projekt...**</span><span class="sxs-lookup"><span data-stu-id="54a41-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![menu Plik](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="54a41-111">Zakończenie **nowy projekt** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="54a41-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="54a41-112">W okienku po lewej stronie wybierz **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="54a41-112">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="54a41-113">W środkowym okienku wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="54a41-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="54a41-114">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="54a41-114">Select **OK**.</span></span>

![Okno dialogowe nowego projektu](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="54a41-116">W **nową aplikację sieci Web Core ASP.NET** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="54a41-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="54a41-117">Wybierz **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="54a41-117">Select **Web Application**.</span></span>

* <span data-ttu-id="54a41-118">Wybierz **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="54a41-118">Select **Change Authentication**.</span></span>

![Okno dialogowe nowego projektu](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="54a41-120">**Zmień uwierzytelnianie** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="54a41-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="54a41-121">Wybierz **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="54a41-121">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="54a41-122">Wybierz **OK** aby powrócić do **nową aplikację sieci Web Core ASP.NET**, a następnie wybierz pozycję **OK** ponownie.</span><span class="sxs-lookup"><span data-stu-id="54a41-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Okno dialogowe nowego uwierzytelniania sieci Web platformy ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="54a41-124">Program Visual Studio tworzy rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="54a41-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="54a41-125">Uruchamianie aplikacji lokalnie</span><span class="sxs-lookup"><span data-stu-id="54a41-125">Run the app locally</span></span>

* <span data-ttu-id="54a41-126">Wybierz **debugowania** następnie **uruchomić bez debugowania** Aby uruchomić aplikację lokalnie.</span><span class="sxs-lookup"><span data-stu-id="54a41-126">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="54a41-127">Kliknij przycisk **o** i **skontaktuj się z** łącza do weryfikowanie działania aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="54a41-127">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Aplikacja sieci Web otwórz w programie Microsoft Edge na hoście lokalnym](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="54a41-129">Wybierz **zarejestrować** i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="54a41-129">Select **Register** and register a new user.</span></span> <span data-ttu-id="54a41-130">Można użyć adresu e-mail fikcyjne.</span><span class="sxs-lookup"><span data-stu-id="54a41-130">You can use a fictitious email address.</span></span> <span data-ttu-id="54a41-131">Podczas przesyłania, zostanie wyświetlona strona następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="54a41-131">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="54a41-132">*"Wewnętrzny błąd serwera: Operacja bazy danych nie powiodło się podczas przetwarzania żądania. Wyjątku SQL: nie można otworzyć bazy danych. Stosowanie istniejących migracje dla kontekst bazy danych aplikacji może rozwiązać ten problem."*</span><span class="sxs-lookup"><span data-stu-id="54a41-132">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="54a41-133">Wybierz **zastosować migracje** i po aktualizacji strony, Odśwież stronę.</span><span class="sxs-lookup"><span data-stu-id="54a41-133">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Wewnętrzny błąd serwera: Operacja bazy danych nie powiodła się podczas przetwarzania żądania.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="54a41-137">Aplikacja wyświetla wiadomości e-mail używany do rejestrowania nowego użytkownika i **Wyloguj się** łącza.</span><span class="sxs-lookup"><span data-stu-id="54a41-137">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Otwórz aplikację sieci Web w programie Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="54a41-140">Wdrażanie aplikacji na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="54a41-140">Deploy the app to Azure</span></span>

<span data-ttu-id="54a41-141">Zamknij stronę sieci web, wróć do programu Visual Studio i wybierz **Zatrzymaj debugowanie** z **debugowania** menu.</span><span class="sxs-lookup"><span data-stu-id="54a41-141">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="54a41-142">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **publikowania...** .</span><span class="sxs-lookup"><span data-stu-id="54a41-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu kontekstowe Otwórz za pomocą łącza publikowania wyróżnione](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="54a41-144">W **publikowania** okno dialogowe, wybierz opcję **Microsoft Azure App Service** i kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="54a41-144">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Okno dialogowe publikowania](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="54a41-146">Nadaj nazwę unikatową nazwę aplikacji.</span><span class="sxs-lookup"><span data-stu-id="54a41-146">Name the app a unique name.</span></span> 

* <span data-ttu-id="54a41-147">Wybierz subskrypcję.</span><span class="sxs-lookup"><span data-stu-id="54a41-147">Select a subscription.</span></span>

* <span data-ttu-id="54a41-148">Wybierz **nowych...**  zasobu grupy, a następnie wprowadź nazwę nowej grupy zasobów.</span><span class="sxs-lookup"><span data-stu-id="54a41-148">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="54a41-149">Wybierz **nowych...**  dla planu usługi aplikacji i wybierz lokalizację w pobliżu.</span><span class="sxs-lookup"><span data-stu-id="54a41-149">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="54a41-150">Można zachować na nazwę, która jest generowana przez domyślny.</span><span class="sxs-lookup"><span data-stu-id="54a41-150">You can keep the name that is generated by default.</span></span>

![Usługi aplikacji — okno dialogowe](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="54a41-152">Wybierz **usług** kartę, aby utworzyć nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="54a41-152">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="54a41-153">Wybierz zielonego  **+**  ikonę, aby utworzyć nową bazę danych SQL</span><span class="sxs-lookup"><span data-stu-id="54a41-153">Select the green **+** icon to create a new SQL Database</span></span>

![Nowej bazy danych SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="54a41-155">Wybierz **nowych...**  na **Konfigurowanie bazy danych SQL** okna dialogowego, aby utworzyć nową bazę danych.</span><span class="sxs-lookup"><span data-stu-id="54a41-155">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Nowe bazy danych SQL i serwera](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="54a41-157">**Konfiguruj serwer SQL** zostanie wyświetlone okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="54a41-157">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="54a41-158">Wprowadź nazwę użytkownika administratora i hasło, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="54a41-158">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="54a41-159">Nie zapomnij nazwę użytkownika i hasło utworzone w tym kroku.</span><span class="sxs-lookup"><span data-stu-id="54a41-159">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="54a41-160">Domyślne można zachować **nazwy serwera**.</span><span class="sxs-lookup"><span data-stu-id="54a41-160">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="54a41-161">Wprowadź nazwy parametrów połączenia i bazy danych.</span><span class="sxs-lookup"><span data-stu-id="54a41-161">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="54a41-162">"Administrator" nie jest dozwolona jako nazwa użytkownika administratora.</span><span class="sxs-lookup"><span data-stu-id="54a41-162">"admin" is not allowed as the administrator user name.</span></span>

![Konfigurowanie okna dialogowego programu SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="54a41-164">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="54a41-164">Select **OK**.</span></span>

<span data-ttu-id="54a41-165">Visual Studio zwraca do **Tworzenie usługi App Service** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="54a41-165">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="54a41-166">Wybierz **Utwórz** na **Tworzenie usługi App Service** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="54a41-166">Select **Create** on the **Create App Service** dialog.</span></span>

![Konfigurowanie bazy danych SQL w oknie dialogowym](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="54a41-168">Kliknij przycisk **ustawienia** łącze w **publikowania** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="54a41-168">Click the **Settings** link in the **Publish** dialog.</span></span>

![Okno dialogowe publikowania: panel połączenia](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="54a41-170">Na **ustawienia** strony **publikowania** okna dialogowego:</span><span class="sxs-lookup"><span data-stu-id="54a41-170">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="54a41-171">Rozwiń węzeł **baz danych** i sprawdź **Użyj tych parametrów połączenia w czasie wykonywania**.</span><span class="sxs-lookup"><span data-stu-id="54a41-171">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="54a41-172">Rozwiń węzeł **Entity Framework migracje** i sprawdź **Zastosuj publikowania tej migracji na**.</span><span class="sxs-lookup"><span data-stu-id="54a41-172">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="54a41-173">Wybierz **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="54a41-173">Select **Save**.</span></span> <span data-ttu-id="54a41-174">Visual Studio zwraca do **publikowania** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="54a41-174">Visual Studio returns to the **Publish** dialog.</span></span> 

![Okno dialogowe publikowania: panel ustawień](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="54a41-176">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="54a41-176">Click **Publish**.</span></span> <span data-ttu-id="54a41-177">Visual Studio będzie publikowanie aplikacji na platformie Azure i uruchomić aplikację w chmurze w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="54a41-177">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="54a41-178">Testowanie aplikacji na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="54a41-178">Test your app in Azure</span></span>

* <span data-ttu-id="54a41-179">Test **o** i **skontaktuj się z** łącza</span><span class="sxs-lookup"><span data-stu-id="54a41-179">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="54a41-180">Zarejestruj nowy użytkownik</span><span class="sxs-lookup"><span data-stu-id="54a41-180">Register a new user</span></span>

![Aplikacja sieci Web otworzyć w programie Microsoft Edge w usłudze Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="54a41-182">Aktualizowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="54a41-182">Update the app</span></span>

* <span data-ttu-id="54a41-183">Edytuj *Pages/About.cshtml* Razor strony i zmienić jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="54a41-183">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="54a41-184">Na przykład można zmodyfikować akapitu znaczy "Hello platformy ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="54a41-184">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="54a41-185">Kliknij prawym przyciskiem myszy na projekt i wybierz **publikowania...**  ponownie.</span><span class="sxs-lookup"><span data-stu-id="54a41-185">Right-click on the project and select **Publish...** again.</span></span>

![Menu kontekstowe Otwórz za pomocą łącza publikowania wyróżnione](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="54a41-187">Po opublikowaniu aplikacji, sprawdź, czy dokonane zmiany są dostępne na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="54a41-187">After the app is published, verify the changes you made are available on Azure.</span></span>

![Sprawdź, czy zadanie zostało ukończone](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="54a41-189">Czyszczenie</span><span class="sxs-lookup"><span data-stu-id="54a41-189">Clean up</span></span>

<span data-ttu-id="54a41-190">Po zakończeniu testowania aplikacji, przejdź do [portalu Azure](https://portal.azure.com/) i usuwania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="54a41-190">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="54a41-191">Wybierz **grup zasobów**, następnie wybierz utworzoną grupę zasobów.</span><span class="sxs-lookup"><span data-stu-id="54a41-191">Select **Resource groups**, then select the resource group you created.</span></span>

![Portalu Azure: Grupy zasobów w menu bocznym](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="54a41-193">W **grup zasobów** wybierz pozycję **usunąć**.</span><span class="sxs-lookup"><span data-stu-id="54a41-193">In the **Resource groups** page, select **Delete**.</span></span>

![Portalu Azure: Strona grup zasobów](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="54a41-195">Wprowadź nazwę grupy zasobów i wybierz **usunąć**.</span><span class="sxs-lookup"><span data-stu-id="54a41-195">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="54a41-196">Aplikacji i innych zasobów utworzonej w tym samouczku są teraz usuwane z platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="54a41-196">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="54a41-197">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="54a41-197">Next steps</span></span>

* [<span data-ttu-id="54a41-198">Ciągłe wdrażanie na platformie Azure za pomocą programu Visual Studio i Git</span><span class="sxs-lookup"><span data-stu-id="54a41-198">Continuous Deployment to Azure with Visual Studio and Git</span></span>](../publishing/azure-continuous-deployment.md)
