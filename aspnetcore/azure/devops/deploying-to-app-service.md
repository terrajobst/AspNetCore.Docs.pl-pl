---
title: Wdrażanie aplikacji w usłudze App Service — metodyki DevOps z platformą ASP.NET Core i platformy Azure
author: CamSoper
description: Wdrażanie aplikacji ASP.NET Core na usłudze Azure App Service, pierwszym krokiem dla metodyki DevOps z platformą ASP.NET Core i platformy Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: e09d03f1d30f128b1db1588aa92b28ec3e4ae626
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264381"
---
# <a name="deploy-an-app-to-app-service"></a><span data-ttu-id="a49ec-103">Wdrażanie aplikacji w usłudze App Service</span><span class="sxs-lookup"><span data-stu-id="a49ec-103">Deploy an app to App Service</span></span>

<span data-ttu-id="a49ec-104">[Usługa Azure App Service](/azure/app-service/) jest Azure platforma sieci web hostingu.</span><span class="sxs-lookup"><span data-stu-id="a49ec-104">[Azure App Service](/azure/app-service/) is Azure's web hosting platform.</span></span> <span data-ttu-id="a49ec-105">Wdrażanie aplikacji sieci web w usłudze Azure App Service może odbywać się ręcznie lub za pomocą zautomatyzowanego procesu.</span><span class="sxs-lookup"><span data-stu-id="a49ec-105">Deploying a web app to Azure App Service can be done manually or by an automated process.</span></span> <span data-ttu-id="a49ec-106">W tej sekcji przewodnika omówiono metod wdrażania, które mogą być wyzwalane ręcznie lub za pomocą skryptu przy użyciu wiersza polecenia lub wyzwalane ręcznie przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a49ec-106">This section of the guide discusses deployment methods that can be triggered manually or by script using the command line, or triggered manually using Visual Studio.</span></span>

<span data-ttu-id="a49ec-107">W tej sekcji opisano następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="a49ec-107">In this section, you'll accomplish the following tasks:</span></span>

* <span data-ttu-id="a49ec-108">Pobierz i skompiluj aplikację przykładową.</span><span class="sxs-lookup"><span data-stu-id="a49ec-108">Download and build the sample app.</span></span>
* <span data-ttu-id="a49ec-109">Tworzenie usługi Azure App Service Web Apps przy użyciu usługi Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="a49ec-109">Create an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="a49ec-110">Wdróż przykładową aplikację na platformie Azure przy użyciu narzędzia Git.</span><span class="sxs-lookup"><span data-stu-id="a49ec-110">Deploy the sample app to Azure using Git.</span></span>
* <span data-ttu-id="a49ec-111">Wdrażanie zmiany w aplikacji przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a49ec-111">Deploy a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="a49ec-112">Dodaj miejsce na tymczasową do aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a49ec-112">Add a staging slot to the web app.</span></span>
* <span data-ttu-id="a49ec-113">Wdróż aktualizację w miejscu przejściowym.</span><span class="sxs-lookup"><span data-stu-id="a49ec-113">Deploy an update to the staging slot.</span></span>
* <span data-ttu-id="a49ec-114">Zamienić gniazd środowisk przejściowych i produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="a49ec-114">Swap the staging and production slots.</span></span>

## <a name="download-and-test-the-app"></a><span data-ttu-id="a49ec-115">Pobieranie i testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="a49ec-115">Download and test the app</span></span>

<span data-ttu-id="a49ec-116">Aplikacja używana w tym przewodniku jest wstępnie skompilowanych aplikacji ASP.NET Core, [proste czytnik źródła danych](https://github.com/Azure-Samples/simple-feed-reader/).</span><span class="sxs-lookup"><span data-stu-id="a49ec-116">The app used in this guide is a pre-built ASP.NET Core app, [Simple Feed Reader](https://github.com/Azure-Samples/simple-feed-reader/).</span></span> <span data-ttu-id="a49ec-117">Jest to aplikacja stron Razor, który używa `Microsoft.SyndicationFeed.ReaderWriter` interfejsu API, aby pobrać źródła danych RSS/Atom i wyświetlanie elementów wiadomości na liście.</span><span class="sxs-lookup"><span data-stu-id="a49ec-117">It's a Razor Pages app that uses the `Microsoft.SyndicationFeed.ReaderWriter` API to retrieve an RSS/Atom feed and display the news items in a list.</span></span>

<span data-ttu-id="a49ec-118">Możesz przejrzeć kod, ale jest ważne dowiedzieć się, że nie ma nic specjalnego o tej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a49ec-118">Feel free to review the code, but it's important to understand that there's nothing special about this app.</span></span> <span data-ttu-id="a49ec-119">Po prostu prostą aplikację platformy ASP.NET Core jest celach ilustracyjnych.</span><span class="sxs-lookup"><span data-stu-id="a49ec-119">It's just a simple ASP.NET Core app for illustrative purposes.</span></span>

<span data-ttu-id="a49ec-120">Z powłoki poleceń należy pobrać kod, skompilować projekt i uruchomić go w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="a49ec-120">From a command shell, download the code, build the project, and run it as follows.</span></span>

> <span data-ttu-id="a49ec-121">*Uwaga: Użytkownicy systemu Linux/macOS należy wprowadzenie odpowiednich zmian dla ścieżek, np. przy użyciu ukośnika (`/`) zamiast ukośnika (`\`).*</span><span class="sxs-lookup"><span data-stu-id="a49ec-121">*Note: Linux/macOS users should make appropriate changes for paths, e.g., using forward slash (`/`) rather than back slash (`\`).*</span></span>

1. <span data-ttu-id="a49ec-122">Klonowanie kodu do folderu na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="a49ec-122">Clone the code to a folder on your local machine.</span></span>

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. <span data-ttu-id="a49ec-123">Zmienić folder roboczy do *prosty kanału informacyjnego czytników* folder, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="a49ec-123">Change your working folder to the *simple-feed-reader* folder that was created.</span></span>

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. <span data-ttu-id="a49ec-124">Przywróć pakiety i Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="a49ec-124">Restore the packages, and build the solution.</span></span>

    ```console
    dotnet build
    ```

4. <span data-ttu-id="a49ec-125">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="a49ec-125">Run the app.</span></span>

    ```console
    dotnet run
    ```

    ![Polecenia dotnet, uruchom polecenie zakończy się pomyślnie](./media/deploying-to-app-service/dotnet-run.png)

5. <span data-ttu-id="a49ec-127">Otwórz przeglądarkę i przejdź do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a49ec-127">Open a browser and navigate to `http://localhost:5000`.</span></span> <span data-ttu-id="a49ec-128">Aplikacja pozwala wpisać lub wkleić zespolonego adres URL źródła danych i wyświetlanie listy elementów wiadomości.</span><span class="sxs-lookup"><span data-stu-id="a49ec-128">The app allows you to type or paste a syndication feed URL and view a list of news items.</span></span>

     ![Aplikacja wyświetlania zawartości ze źródła danych RSS](./media/deploying-to-app-service/app-in-browser.png)

6. <span data-ttu-id="a49ec-130">Po zakończeniu aplikacja działa prawidłowo, zamknij go, naciskając klawisz **Ctrl**+**C** w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="a49ec-130">Once you're satisfied the app is working correctly, shut it down by pressing **Ctrl**+**C** in the command shell.</span></span>

## <a name="create-the-azure-app-service-web-app"></a><span data-ttu-id="a49ec-131">Tworzenie aplikacji sieci Web w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a49ec-131">Create the Azure App Service Web App</span></span>

<span data-ttu-id="a49ec-132">Aby wdrożyć aplikację, musisz utworzyć usługi App Service [aplikacji sieci Web](/azure/app-service/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="a49ec-132">To deploy the app, you'll need to create an App Service [Web App](/azure/app-service/app-service-web-overview).</span></span> <span data-ttu-id="a49ec-133">Po utworzeniu aplikacji sieci Web będzie można wdrożyć na go z komputera lokalnego przy użyciu narzędzia Git.</span><span class="sxs-lookup"><span data-stu-id="a49ec-133">After creation of the Web App, you'll deploy to it from your local machine using Git.</span></span>

1. <span data-ttu-id="a49ec-134">Zaloguj się do [usługi Azure Cloud Shell](https://shell.azure.com/bash).</span><span class="sxs-lookup"><span data-stu-id="a49ec-134">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="a49ec-135">Uwaga: Po zalogowaniu się po raz pierwszy, usługa Cloud Shell wyświetli monit o utworzenie konta magazynu dla plików konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="a49ec-135">Note: When you sign in for the first time, Cloud Shell prompts to create a storage account for configuration files.</span></span> <span data-ttu-id="a49ec-136">Zaakceptuj wartości domyślne lub Podaj unikatową nazwę.</span><span class="sxs-lookup"><span data-stu-id="a49ec-136">Accept the defaults or provide a unique name.</span></span>

2. <span data-ttu-id="a49ec-137">Użyliśmy usługi Cloud Shell dla następujących kroków.</span><span class="sxs-lookup"><span data-stu-id="a49ec-137">Use the Cloud Shell for the following steps.</span></span>

    <span data-ttu-id="a49ec-138">a.</span><span class="sxs-lookup"><span data-stu-id="a49ec-138">a.</span></span> <span data-ttu-id="a49ec-139">Zadeklaruj zmienną do przechowywania nazwy aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a49ec-139">Declare a variable to store your web app's name.</span></span> <span data-ttu-id="a49ec-140">Nazwa musi być unikatowa, ma być używany w domyślnego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a49ec-140">The name must be unique to be used in the default URL.</span></span> <span data-ttu-id="a49ec-141">Za pomocą `$RANDOM` funkcję Bash, aby skonstruować nazwę gwarantuje unikatowość i wyniki w formacie `webappname99999`.</span><span class="sxs-lookup"><span data-stu-id="a49ec-141">Using the `$RANDOM` Bash function to construct the name guarantees uniqueness and results in the format `webappname99999`.</span></span>

    ```console
    webappname=mywebapp$RANDOM
    ```

    <span data-ttu-id="a49ec-142">b.</span><span class="sxs-lookup"><span data-stu-id="a49ec-142">b.</span></span> <span data-ttu-id="a49ec-143">Utwórz grupę zasobów.</span><span class="sxs-lookup"><span data-stu-id="a49ec-143">Create a resource group.</span></span> <span data-ttu-id="a49ec-144">Grupy zasobów zapewniają środki do agregowania zasobów platformy Azure, które mają być zarządzane jako grupa.</span><span class="sxs-lookup"><span data-stu-id="a49ec-144">Resource groups provide a means to aggregate Azure resources to be managed as a group.</span></span>

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    <span data-ttu-id="a49ec-145">`az` Olecenie wywołuje [wiersza polecenia platformy Azure](/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="a49ec-145">The `az` command invokes the [Azure CLI](/cli/azure/).</span></span> <span data-ttu-id="a49ec-146">Interfejs wiersza polecenia, które mogą być uruchamiane lokalnie, ale jej używanie w usłudze Cloud Shell oszczędza czas i konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="a49ec-146">The CLI can be run locally, but using it in the Cloud Shell saves time and configuration.</span></span>

    <span data-ttu-id="a49ec-147">c.</span><span class="sxs-lookup"><span data-stu-id="a49ec-147">c.</span></span> <span data-ttu-id="a49ec-148">Utwórz plan usługi App Service w warstwie S1.</span><span class="sxs-lookup"><span data-stu-id="a49ec-148">Create an App Service plan in the S1 tier.</span></span> <span data-ttu-id="a49ec-149">Plan usługi App Service to grupa aplikacji sieci web, które udostępnianie tej samej warstwie cenowej.</span><span class="sxs-lookup"><span data-stu-id="a49ec-149">An App Service plan is a grouping of web apps that share the same pricing tier.</span></span> <span data-ttu-id="a49ec-150">W warstwie S1 jest bezpłatne, ale jest to wymagane dla funkcji miejsca przejściowego.</span><span class="sxs-lookup"><span data-stu-id="a49ec-150">The S1 tier isn't free, but it's required for the staging slots feature.</span></span>

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    <span data-ttu-id="a49ec-151">d.</span><span class="sxs-lookup"><span data-stu-id="a49ec-151">d.</span></span> <span data-ttu-id="a49ec-152">Utwórz zasób aplikacji sieci web przy użyciu planu usługi App Service w tej samej grupie zasobów.</span><span class="sxs-lookup"><span data-stu-id="a49ec-152">Create the web app resource using the App Service plan in the same resource group.</span></span>

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    <span data-ttu-id="a49ec-153">e.</span><span class="sxs-lookup"><span data-stu-id="a49ec-153">e.</span></span> <span data-ttu-id="a49ec-154">Ustaw poświadczenia wdrażania.</span><span class="sxs-lookup"><span data-stu-id="a49ec-154">Set the deployment credentials.</span></span> <span data-ttu-id="a49ec-155">Te poświadczenia wdrożenia dotyczą wszystkich aplikacji sieci web w ramach subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="a49ec-155">These deployment credentials apply to all the web apps in your subscription.</span></span> <span data-ttu-id="a49ec-156">Nie należy używać znaków specjalnych w nazwie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a49ec-156">Don't use special characters in the user name.</span></span>

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    <span data-ttu-id="a49ec-157">f.</span><span class="sxs-lookup"><span data-stu-id="a49ec-157">f.</span></span> <span data-ttu-id="a49ec-158">Konfigurowanie aplikacji sieci web, aby zaakceptować wdrożenia z poziomu lokalnego narzędzia Git i wyświetlanie *adres URL wdrożenia Git*.</span><span class="sxs-lookup"><span data-stu-id="a49ec-158">Configure the web app to accept deployments from local Git and display the *Git deployment URL*.</span></span> <span data-ttu-id="a49ec-159">**Należy pamiętać, ten adres URL dla odwołania później**.</span><span class="sxs-lookup"><span data-stu-id="a49ec-159">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    <span data-ttu-id="a49ec-160">g.</span><span class="sxs-lookup"><span data-stu-id="a49ec-160">g.</span></span> <span data-ttu-id="a49ec-161">Wyświetlanie *adres URL aplikacji sieci web*.</span><span class="sxs-lookup"><span data-stu-id="a49ec-161">Display the *web app URL*.</span></span> <span data-ttu-id="a49ec-162">Przejdź do tego adresu URL, aby zobaczyć aplikację internetową puste.</span><span class="sxs-lookup"><span data-stu-id="a49ec-162">Browse to this URL to see the blank web app.</span></span> <span data-ttu-id="a49ec-163">**Należy pamiętać, ten adres URL dla odwołania później**.</span><span class="sxs-lookup"><span data-stu-id="a49ec-163">**Note this URL for reference later**.</span></span>

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. <span data-ttu-id="a49ec-164">Przy użyciu powłoki poleceń na komputerze lokalnym, przejdź do folderu projektu aplikacji sieci web (na przykład `.\simple-feed-reader\SimpleFeedReader`).</span><span class="sxs-lookup"><span data-stu-id="a49ec-164">Using a command shell on your local machine, navigate to the web app's project folder (for example, `.\simple-feed-reader\SimpleFeedReader`).</span></span> <span data-ttu-id="a49ec-165">Wykonaj następujące polecenia, aby konfiguracja repozytorium Git do wypychania na adres URL wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="a49ec-165">Execute the following commands to set up Git to push to the deployment URL:</span></span>

    <span data-ttu-id="a49ec-166">a.</span><span class="sxs-lookup"><span data-stu-id="a49ec-166">a.</span></span> <span data-ttu-id="a49ec-167">Dodaj zdalny adres URL do lokalnego repozytorium.</span><span class="sxs-lookup"><span data-stu-id="a49ec-167">Add the remote URL to the local repository.</span></span>

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    <span data-ttu-id="a49ec-168">b.</span><span class="sxs-lookup"><span data-stu-id="a49ec-168">b.</span></span> <span data-ttu-id="a49ec-169">Wypychanie lokalnej *wzorca* gałęzi do *produkcyjne azure* firmy zdalne *wzorca* gałęzi.</span><span class="sxs-lookup"><span data-stu-id="a49ec-169">Push the local *master* branch to the *azure-prod* remote's *master* branch.</span></span>

    ```console
    git push azure-prod master
    ```

    <span data-ttu-id="a49ec-170">Zostanie wyświetlony monit o poświadczenia wdrażania, która została utworzona wcześniej.</span><span class="sxs-lookup"><span data-stu-id="a49ec-170">You'll be prompted for the deployment credentials you created earlier.</span></span> <span data-ttu-id="a49ec-171">Sprawdzanie danych wyjściowych, w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="a49ec-171">Observe the output in the command shell.</span></span> <span data-ttu-id="a49ec-172">Azure tworzy aplikację ASP.NET Core zdalnie.</span><span class="sxs-lookup"><span data-stu-id="a49ec-172">Azure builds the ASP.NET Core app remotely.</span></span>

4. <span data-ttu-id="a49ec-173">W przeglądarce przejdź do *adres URL aplikacji sieci Web* i zwróć uwagę, aplikacja została skompilowana i wdrożona.</span><span class="sxs-lookup"><span data-stu-id="a49ec-173">In a browser, navigate to the *Web app URL* and note the app has been built and deployed.</span></span> <span data-ttu-id="a49ec-174">Dodatkowe zmiany mogą zostać zatwierdzone do lokalnego repozytorium Git z `git commit`.</span><span class="sxs-lookup"><span data-stu-id="a49ec-174">Additional changes can be committed to the local Git repository with `git commit`.</span></span> <span data-ttu-id="a49ec-175">Zmiany te są wypychane na platformie Azure z poprzednim okresem `git push` polecenia.</span><span class="sxs-lookup"><span data-stu-id="a49ec-175">These changes are pushed to Azure with the preceding `git push` command.</span></span>

## <a name="deployment-with-visual-studio"></a><span data-ttu-id="a49ec-176">Wdrażanie za pomocą programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a49ec-176">Deployment with Visual Studio</span></span>

> <span data-ttu-id="a49ec-177">*Uwaga: Ta sekcja dotyczy tylko Windows. Użytkownicy systemu Linux i macOS wprowadzić zmiany, opisanej w kroku 2 poniżej. Zapisz plik i zatwierdź zmianę w repozytorium lokalnym za pomocą `git commit`. Na koniec wypchnąć zmiany z `git push`, jak w pierwszej sekcji.*</span><span class="sxs-lookup"><span data-stu-id="a49ec-177">*Note: This section applies to Windows only. Linux and macOS users should make the change described in step 2 below. Save the file, and commit the change to the local repository with `git commit`. Finally, push the change with `git push`, as in the first section.*</span></span>

<span data-ttu-id="a49ec-178">Aplikacja została już wdrożona z powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="a49ec-178">The app has already been deployed from the command shell.</span></span> <span data-ttu-id="a49ec-179">Utwórzmy wdrażanie aktualizacji w aplikacji za pomocą zintegrowanych narzędzi programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a49ec-179">Let's use Visual Studio's integrated tools to deploy an update to the app.</span></span> <span data-ttu-id="a49ec-180">W tle programu Visual Studio w ramach tak samo, jak narzędzie wiersza polecenia, ale w obrębie znajomy interfejs użytkownika programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a49ec-180">Behind the scenes, Visual Studio accomplishes the same thing as the command line tooling, but within Visual Studio's familiar UI.</span></span>

1. <span data-ttu-id="a49ec-181">Otwórz *SimpleFeedReader.sln* w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a49ec-181">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
2. <span data-ttu-id="a49ec-182">W Eksploratorze rozwiązań Otwórz *Pages\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a49ec-182">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="a49ec-183">Zmiana `<h2>Simple Feed Reader</h2>` do `<h2>Simple Feed Reader - V2</h2>`.</span><span class="sxs-lookup"><span data-stu-id="a49ec-183">Change `<h2>Simple Feed Reader</h2>` to `<h2>Simple Feed Reader - V2</h2>`.</span></span>
3. <span data-ttu-id="a49ec-184">Naciśnij klawisz **Ctrl**+**Shift**+**B** do skompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a49ec-184">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
4. <span data-ttu-id="a49ec-185">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nad projektem, a następnie kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="a49ec-185">In Solution Explorer, right-click on the project and click **Publish**.</span></span>

    ![Zrzut ekranu przedstawiający kliknij prawym przyciskiem myszy i publikowania](./media/deploying-to-app-service/publish.png)
5. <span data-ttu-id="a49ec-187">Visual Studio można utworzyć nowy zasób usługi App Service, ale ta aktualizacja zostanie opublikowana przez istniejące wdrożenie.</span><span class="sxs-lookup"><span data-stu-id="a49ec-187">Visual Studio can create a new App Service resource, but this update will be published over the existing deployment.</span></span> <span data-ttu-id="a49ec-188">W **wybierz lokalizację docelową publikowania** okno dialogowe, wybierz opcję **usługi App Service** z listy po lewej stronie, a następnie wybierz pozycję **wybierz istniejącą**.</span><span class="sxs-lookup"><span data-stu-id="a49ec-188">In the **Pick a publish target** dialog, select **App Service** from the list on the left, and then select **Select Existing**.</span></span> <span data-ttu-id="a49ec-189">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="a49ec-189">Click **Publish**.</span></span>
6. <span data-ttu-id="a49ec-190">W **usługi App Service** okna dialogowego, upewnij się, że firmy Microsoft lub konto organizacyjne użyte do utworzenia subskrypcji platformy Azure jest wyświetlana w prawym górnym rogu.</span><span class="sxs-lookup"><span data-stu-id="a49ec-190">In the **App Service** dialog, confirm that the Microsoft or Organizational account used to create your Azure subscription is displayed in the upper right.</span></span> <span data-ttu-id="a49ec-191">Jeśli nie, kliknij listę rozwijaną i dodaj ją.</span><span class="sxs-lookup"><span data-stu-id="a49ec-191">If it's not, click the drop-down and add it.</span></span>
7. <span data-ttu-id="a49ec-192">Upewnij się, że poprawne Azure **subskrypcji** jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="a49ec-192">Confirm that the correct Azure **Subscription** is selected.</span></span> <span data-ttu-id="a49ec-193">Aby uzyskać **widoku**, wybierz opcję **grupy zasobów**.</span><span class="sxs-lookup"><span data-stu-id="a49ec-193">For **View**, select **Resource Group**.</span></span> <span data-ttu-id="a49ec-194">Rozwiń **AzureTutorial** grupy zasobów, a następnie wybierz istniejącą aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="a49ec-194">Expand the **AzureTutorial** resource group and then select the existing web app.</span></span> <span data-ttu-id="a49ec-195">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a49ec-195">Click **OK**.</span></span>

    ![Zrzut ekranu przedstawiający okno dialogowe z publikowania usługi aplikacji](./media/deploying-to-app-service/publish-dialog.png)

<span data-ttu-id="a49ec-197">Visual Studio tworzy i wdraża aplikację na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="a49ec-197">Visual Studio builds and deploys the app to Azure.</span></span> <span data-ttu-id="a49ec-198">Przejdź do adresu URL aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a49ec-198">Browse to the web app URL.</span></span> <span data-ttu-id="a49ec-199">Sprawdzić, czy `<h2>` modyfikacji elementu jest aktywna.</span><span class="sxs-lookup"><span data-stu-id="a49ec-199">Validate that the `<h2>` element modification is live.</span></span>

![Aplikacja z tytułem zmienione](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a><span data-ttu-id="a49ec-201">Miejsca wdrożenia</span><span class="sxs-lookup"><span data-stu-id="a49ec-201">Deployment slots</span></span>

<span data-ttu-id="a49ec-202">Miejsca wdrożenia obsługuje wdrażanie przejściowe zmiany, bez wywierania wpływu na aplikacji działających w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="a49ec-202">Deployment slots support the staging of changes without impacting the app running in production.</span></span> <span data-ttu-id="a49ec-203">Po zweryfikowaniu przygotowaną wersję aplikacji przez zespół zapewnienia jakości można wymieniać produkcyjne oraz przejściowe miejsce.</span><span class="sxs-lookup"><span data-stu-id="a49ec-203">Once the staged version of the app is validated by a quality assurance team, the production and staging slots can be swapped.</span></span> <span data-ttu-id="a49ec-204">Aplikacji w środowisku tymczasowym jest podwyższany do środowiska produkcyjnego w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="a49ec-204">The app in staging is promoted to production in this manner.</span></span> <span data-ttu-id="a49ec-205">Poniższe kroki tworzenia miejsca przejściowego, wdrażanie w niej kilka zmian i zamienić miejsce przejściowe z środowisku produkcyjnym po zakończeniu weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="a49ec-205">The following steps create a staging slot, deploy some changes to it, and swap the staging slot with production after verification.</span></span>

1. <span data-ttu-id="a49ec-206">Zaloguj się do [usługi Azure Cloud Shell](https://shell.azure.com/bash), jeśli nie zostało to zrobione.</span><span class="sxs-lookup"><span data-stu-id="a49ec-206">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash), if not already signed in.</span></span>
2. <span data-ttu-id="a49ec-207">Utwórz miejsca przejściowego.</span><span class="sxs-lookup"><span data-stu-id="a49ec-207">Create the staging slot.</span></span>

    <span data-ttu-id="a49ec-208">a.</span><span class="sxs-lookup"><span data-stu-id="a49ec-208">a.</span></span> <span data-ttu-id="a49ec-209">Utwórz miejsce wdrożenia o nazwie *przemieszczania*.</span><span class="sxs-lookup"><span data-stu-id="a49ec-209">Create a deployment slot with the name *staging*.</span></span>

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    <span data-ttu-id="a49ec-210">b.</span><span class="sxs-lookup"><span data-stu-id="a49ec-210">b.</span></span> <span data-ttu-id="a49ec-211">Konfigurowanie miejsca przejściowego, przy użyciu wdrażania z lokalnej usługi Git i get **przemieszczania** adres URL wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="a49ec-211">Configure the staging slot to use deployment from local Git and get the **staging** deployment URL.</span></span> <span data-ttu-id="a49ec-212">**Należy pamiętać, ten adres URL dla odwołania później**.</span><span class="sxs-lookup"><span data-stu-id="a49ec-212">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    <span data-ttu-id="a49ec-213">c.</span><span class="sxs-lookup"><span data-stu-id="a49ec-213">c.</span></span> <span data-ttu-id="a49ec-214">Wyświetlanie adresów URL w miejscu przejściowym.</span><span class="sxs-lookup"><span data-stu-id="a49ec-214">Display the staging slot's URL.</span></span> <span data-ttu-id="a49ec-215">Przejdź do adresu URL, aby wyświetlić puste miejsce na tymczasową.</span><span class="sxs-lookup"><span data-stu-id="a49ec-215">Browse to the URL to see the empty staging slot.</span></span> <span data-ttu-id="a49ec-216">**Należy pamiętać, ten adres URL dla odwołania później**.</span><span class="sxs-lookup"><span data-stu-id="a49ec-216">**Note this URL for reference later**.</span></span>

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. <span data-ttu-id="a49ec-217">W edytorze tekstu lub programu Visual Studio, należy zmodyfikować *Pages/Index.cshtml* ponownie, aby `<h2>` odczytuje element `<h2>Simple Feed Reader - V3</h2>` i Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="a49ec-217">In a text editor or Visual Studio, modify *Pages/Index.cshtml* again so that the `<h2>` element reads `<h2>Simple Feed Reader - V3</h2>` and save the file.</span></span>

4. <span data-ttu-id="a49ec-218">Przekazać plik do lokalnego repozytorium Git, za pomocą **zmiany** strony w programie Visual Studio *Team Explorer* kartę, lub przez wprowadzenie następujących przy użyciu powłoki poleceń na komputerze lokalnym:</span><span class="sxs-lookup"><span data-stu-id="a49ec-218">Commit the file to the local Git repository, using either the **Changes** page in Visual Studio's *Team Explorer* tab, or by entering the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V3"
    ```

5. <span data-ttu-id="a49ec-219">Korzystając z powłoki poleceń na komputerze lokalnym, Dodaj przejściowym adresie URL wdrożenia jako element zdalny narzędzia Git i Wypchnij zatwierdzone zmiany:</span><span class="sxs-lookup"><span data-stu-id="a49ec-219">Using the local machine's command shell, add the staging deployment URL as a Git remote and push the committed changes:</span></span>

    <span data-ttu-id="a49ec-220">a.</span><span class="sxs-lookup"><span data-stu-id="a49ec-220">a.</span></span> <span data-ttu-id="a49ec-221">Dodaj zdalny adres URL na potrzeby wdrażania przejściowego do lokalnego repozytorium Git.</span><span class="sxs-lookup"><span data-stu-id="a49ec-221">Add the remote URL for staging to the local Git repository.</span></span>

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    <span data-ttu-id="a49ec-222">b.</span><span class="sxs-lookup"><span data-stu-id="a49ec-222">b.</span></span> <span data-ttu-id="a49ec-223">Wypychanie lokalnej *wzorca* gałęzi do *przemieszczania azure* firmy zdalne *wzorca* gałęzi.</span><span class="sxs-lookup"><span data-stu-id="a49ec-223">Push the local *master* branch to the *azure-staging* remote's *master* branch.</span></span>

    ```console
    git push azure-staging master
    ```

    <span data-ttu-id="a49ec-224">Zaczekaj, aż Azure zostanie skompilowana i wdrożona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="a49ec-224">Wait while Azure builds and deploys the app.</span></span>

6. <span data-ttu-id="a49ec-225">Aby sprawdzić, czy V3 został pomyślnie wdrożony do miejsca przejściowego, Otwórz dwa okna przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a49ec-225">To verify that V3 has been deployed to the staging slot, open two browser windows.</span></span> <span data-ttu-id="a49ec-226">W jednym oknie przejdź do oryginalnego adresu URL aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a49ec-226">In one window, navigate to the original web app URL.</span></span> <span data-ttu-id="a49ec-227">W innym oknie przejdź do tymczasowej adresu URL aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a49ec-227">In the other window, navigate to the staging web app URL.</span></span> <span data-ttu-id="a49ec-228">Produkcja — adres URL służy aplikacji w wersji 2.</span><span class="sxs-lookup"><span data-stu-id="a49ec-228">The production URL serves V2 of the app.</span></span> <span data-ttu-id="a49ec-229">Przejściowym adresie URL służy aplikacji w wersji 3.</span><span class="sxs-lookup"><span data-stu-id="a49ec-229">The staging URL serves V3 of the app.</span></span>

    ![Zrzut ekranu okna przeglądarki porównanie](./media/deploying-to-app-service/ready-to-swap.png)

7. <span data-ttu-id="a49ec-231">W usłudze Cloud Shell można zamienić miejsca przejściowego zweryfikować/przygotowaniu up w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="a49ec-231">In the Cloud Shell, swap the verified/warmed-up staging slot into production.</span></span>

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. <span data-ttu-id="a49ec-232">Sprawdź wymiany, które wystąpiły, odświeżając okna przeglądarki dwa.</span><span class="sxs-lookup"><span data-stu-id="a49ec-232">Verify that the swap occurred by refreshing the two browser windows.</span></span>

    ![Porównywanie okna przeglądarki po wymiany](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a><span data-ttu-id="a49ec-234">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a49ec-234">Summary</span></span>

<span data-ttu-id="a49ec-235">W tej sekcji wykonano następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="a49ec-235">In this section, the following tasks were completed:</span></span>

* <span data-ttu-id="a49ec-236">Pobrano i zbudowane dla przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a49ec-236">Downloaded and built the sample app.</span></span>
* <span data-ttu-id="a49ec-237">Utworzony usługi Azure App Service Web Apps przy użyciu usługi Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="a49ec-237">Created an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="a49ec-238">Wdrożyć aplikację przykładową na platformie Azure przy użyciu narzędzia Git.</span><span class="sxs-lookup"><span data-stu-id="a49ec-238">Deployed the sample app to Azure using Git.</span></span>
* <span data-ttu-id="a49ec-239">Wdrożona zmiana w aplikacji przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a49ec-239">Deployed a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="a49ec-240">Dodaje gniazdo tymczasowej do aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="a49ec-240">Added a staging slot to the web app.</span></span>
* <span data-ttu-id="a49ec-241">Należy wdrożyć aktualizację do miejsca przejściowego.</span><span class="sxs-lookup"><span data-stu-id="a49ec-241">Deployed an update to the staging slot.</span></span>
* <span data-ttu-id="a49ec-242">Zamienione miejscami środowisk przejściowych i produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="a49ec-242">Swapped the staging and production slots.</span></span>

<span data-ttu-id="a49ec-243">W następnej sekcji dowiesz się, jak utworzyć potok DevOps za pomocą potoków usługi Azure.</span><span class="sxs-lookup"><span data-stu-id="a49ec-243">In the next section, you'll learn how to build a DevOps pipeline with Azure Pipelines.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="a49ec-244">Materiały uzupełniające</span><span class="sxs-lookup"><span data-stu-id="a49ec-244">Additional reading</span></span>

* [<span data-ttu-id="a49ec-245">Przegląd usługi Web Apps</span><span class="sxs-lookup"><span data-stu-id="a49ec-245">Web Apps overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="a49ec-246">Tworzenie aplikacji internetowej platformy .NET Core i SQL Database w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a49ec-246">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [<span data-ttu-id="a49ec-247">Skonfiguruj poświadczenia wdrożenia dla usługi Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a49ec-247">Configure deployment credentials for Azure App Service</span></span>](/azure/app-service/app-service-deployment-credentials)
* [<span data-ttu-id="a49ec-248">Konfigurowanie środowisk przejściowych w usłudze Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a49ec-248">Set up staging environments in Azure App Service</span></span>](/azure/app-service/web-sites-staged-publishing)
