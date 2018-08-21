---
title: Metodyka DevOps z platformą ASP.NET Core i platformy Azure | Ciągła integracja i ciągłe wdrażanie
author: CamSoper
description: Przewodnik, który dostarcza wskazówki end-to-end na tworzeniu potoku metodyki DevOps dla aplikacji ASP.NET Core hostowanych na platformie Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/cicd
ms.openlocfilehash: 9127f26fc4e3f78ec745fa1e342de137228f484e
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722679"
---
# <a name="continuous-integration-and-deployment"></a><span data-ttu-id="2f338-103">Ciągła integracja i ciągłe wdrażanie</span><span class="sxs-lookup"><span data-stu-id="2f338-103">Continuous integration and deployment</span></span>

<span data-ttu-id="2f338-104">W poprzednim rozdziale utworzono lokalnego repozytorium Git dla aplikacji proste czytnik źródła danych.</span><span class="sxs-lookup"><span data-stu-id="2f338-104">In the previous chapter, you created a local Git repository for the Simple Feed Reader app.</span></span> <span data-ttu-id="2f338-105">W tym rozdziale możesz opublikować ten kod z repozytorium GitHub i utworzyć potok DevOps programu Visual Studio Team Services (VSTS).</span><span class="sxs-lookup"><span data-stu-id="2f338-105">In this chapter, you'll publish that code to a GitHub repository and construct a Visual Studio Team Services (VSTS) DevOps pipeline.</span></span> <span data-ttu-id="2f338-106">Potok umożliwia ciągłe kompilacje i wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f338-106">The pipeline enables continuous builds and deployments of the app.</span></span> <span data-ttu-id="2f338-107">Każdego zatwierdzenia do repozytorium GitHub wyzwala kompilację i wdrażania do miejsca przejściowego aplikacji sieci Web platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="2f338-107">Any commit to the GitHub repository triggers a build and a deployment to the Azure Web App's staging slot.</span></span>

<span data-ttu-id="2f338-108">W tej sekcji zostaną wykonane następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="2f338-108">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="2f338-109">Publikowanie kodu aplikacji w usłudze GitHub</span><span class="sxs-lookup"><span data-stu-id="2f338-109">Publish the app's code to GitHub</span></span>
* <span data-ttu-id="2f338-110">Odłącz lokalne wdrożenie narzędzia Git</span><span class="sxs-lookup"><span data-stu-id="2f338-110">Disconnect local Git deployment</span></span>
* <span data-ttu-id="2f338-111">Utwórz konto usługi VSTS</span><span class="sxs-lookup"><span data-stu-id="2f338-111">Create a VSTS account</span></span>
* <span data-ttu-id="2f338-112">Utwórz projekt zespołowy w usłudze VSTS</span><span class="sxs-lookup"><span data-stu-id="2f338-112">Create a team project in VSTS</span></span>
* <span data-ttu-id="2f338-113">Utwórz definicję kompilacji</span><span class="sxs-lookup"><span data-stu-id="2f338-113">Create a build definition</span></span>
* <span data-ttu-id="2f338-114">Tworzenie potoku tworzenia wersji</span><span class="sxs-lookup"><span data-stu-id="2f338-114">Create a release pipeline</span></span>
* <span data-ttu-id="2f338-115">Zatwierdź zmiany w usłudze GitHub i automatycznie wdrażać na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="2f338-115">Commit changes to GitHub and automatically deploy to Azure</span></span>
* <span data-ttu-id="2f338-116">Sprawdź z potokiem metodyki DevOps w usłudze VSTS</span><span class="sxs-lookup"><span data-stu-id="2f338-116">Examine the VSTS DevOps pipeline</span></span>

## <a name="publish-the-apps-code-to-github"></a><span data-ttu-id="2f338-117">Publikowanie kodu aplikacji w usłudze GitHub</span><span class="sxs-lookup"><span data-stu-id="2f338-117">Publish the app's code to GitHub</span></span>

1. <span data-ttu-id="2f338-118">Otwórz okno przeglądarki i przejdź do `https://github.com`.</span><span class="sxs-lookup"><span data-stu-id="2f338-118">Open a browser window, and navigate to `https://github.com`.</span></span>
1. <span data-ttu-id="2f338-119">Kliknij przycisk **+** listy rozwijanej w nagłówku, a następnie wybierz pozycję **nowe repozytorium**:</span><span class="sxs-lookup"><span data-stu-id="2f338-119">Click the **+** drop-down in the header, and select **New repository**:</span></span>

    ![Opcja nowego repozytorium usługi GitHub](media/cicd/github-new-repo.png)

1. <span data-ttu-id="2f338-121">Wybierz swoje konto **właściciela** listy rozwijanej i wprowadź *prosty kanału informacyjnego czytników* w **nazwę repozytorium** pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="2f338-121">Select your account in the **Owner** drop-down, and enter *simple-feed-reader* in the **Repository name** textbox.</span></span>
1. <span data-ttu-id="2f338-122">Kliknij przycisk **tworzenia repozytorium** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-122">Click the **Create repository** button.</span></span>
1. <span data-ttu-id="2f338-123">Otwórz powłokę poleceń komputer lokalny.</span><span class="sxs-lookup"><span data-stu-id="2f338-123">Open your local machine's command shell.</span></span> <span data-ttu-id="2f338-124">Przejdź do katalogu, w którym *prosty kanału informacyjnego czytników* jest przechowywany w repozytorium Git.</span><span class="sxs-lookup"><span data-stu-id="2f338-124">Navigate to the directory in which the *simple-feed-reader* Git repository is stored.</span></span>
1. <span data-ttu-id="2f338-125">Zmień nazwę istniejącego *pochodzenia* zdalnie *nadrzędnego*.</span><span class="sxs-lookup"><span data-stu-id="2f338-125">Rename the existing *origin* remote to *upstream*.</span></span> <span data-ttu-id="2f338-126">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2f338-126">Execute the following command:</span></span>
    ```console
    git remote rename origin upstream
    ```
1. <span data-ttu-id="2f338-127">Dodaj nową *pochodzenia* zdalnego wskazuje swoją kopię repozytorium w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="2f338-127">Add a new *origin* remote pointing to your copy of the repository on GitHub.</span></span> <span data-ttu-id="2f338-128">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2f338-128">Execute the following command:</span></span>
    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```
1. <span data-ttu-id="2f338-129">Publikowanie lokalnego repozytorium Git do nowo utworzonego repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="2f338-129">Publish your local Git repository to the newly created GitHub repository.</span></span> <span data-ttu-id="2f338-130">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2f338-130">Execute the following command:</span></span>
    ```console
    git push -u origin master
    ```
1. <span data-ttu-id="2f338-131">Otwórz okno przeglądarki i przejdź do `https://github.com/<GitHub_username>/simple-feed-reader/`.</span><span class="sxs-lookup"><span data-stu-id="2f338-131">Open a browser window, and navigate to `https://github.com/<GitHub_username>/simple-feed-reader/`.</span></span> <span data-ttu-id="2f338-132">Sprawdź, czy kod jest wyświetlana w repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="2f338-132">Validate that your code appears in the GitHub repository.</span></span>

## <a name="disconnect-local-git-deployment"></a><span data-ttu-id="2f338-133">Odłącz lokalne wdrożenie narzędzia Git</span><span class="sxs-lookup"><span data-stu-id="2f338-133">Disconnect local Git deployment</span></span>

<span data-ttu-id="2f338-134">Usuń lokalne wdrożenie narzędzia Git wykonując następujące kroki.</span><span class="sxs-lookup"><span data-stu-id="2f338-134">Remove the local Git deployment with the following steps.</span></span> <span data-ttu-id="2f338-135">Usługa VSTS zastępuje i rozszerzają funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="2f338-135">VSTS both replaces and augments that functionality.</span></span>

1. <span data-ttu-id="2f338-136">Otwórz [witryny Azure portal](https://portal.azure.com/)i przejdź do *przemieszczania (mywebapp\<unique_number\>/przemieszczania)* aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2f338-136">Open the [Azure portal](https://portal.azure.com/), and navigate to the *staging (mywebapp\<unique_number\>/staging)* Web App.</span></span> <span data-ttu-id="2f338-137">Aplikacji sieci Web mogą szybko znajdować, wprowadzając *przemieszczania* w polu wyszukiwania w witrynie portal:</span><span class="sxs-lookup"><span data-stu-id="2f338-137">The Web App can be quickly located by entering *staging* in the portal's search box:</span></span>

    ![tymczasową aplikację sieci Web wyszukiwany termin](media/cicd/portal-search-box.png)

1. <span data-ttu-id="2f338-139">Kliknij przycisk **opcje wdrażania**.</span><span class="sxs-lookup"><span data-stu-id="2f338-139">Click **Deployment options**.</span></span> <span data-ttu-id="2f338-140">Zostanie wyświetlony nowy panel.</span><span class="sxs-lookup"><span data-stu-id="2f338-140">A new panel appears.</span></span> <span data-ttu-id="2f338-141">Kliknij przycisk **rozłączenia** można usunąć lokalnej dodaną w poprzednim rozdziale konfiguracji kontroli źródła Git.</span><span class="sxs-lookup"><span data-stu-id="2f338-141">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="2f338-142">Potwierdź operację usunięcia, klikając przycisk **tak** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-142">Confirm the removal operation by clicking the **Yes** button.</span></span>
1. <span data-ttu-id="2f338-143">Przejdź do *mywebapp < unique_number >* usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="2f338-143">Navigate to the *mywebapp<unique_number>* App Service.</span></span> <span data-ttu-id="2f338-144">Przypominamy pole wyszukiwania portalu można szybko zlokalizować usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="2f338-144">As a reminder, the portal's search box can be used to quickly locate the App Service.</span></span>
1. <span data-ttu-id="2f338-145">Kliknij przycisk **opcje wdrażania**.</span><span class="sxs-lookup"><span data-stu-id="2f338-145">Click **Deployment options**.</span></span> <span data-ttu-id="2f338-146">Zostanie wyświetlony nowy panel.</span><span class="sxs-lookup"><span data-stu-id="2f338-146">A new panel appears.</span></span> <span data-ttu-id="2f338-147">Kliknij przycisk **rozłączenia** można usunąć lokalnej dodaną w poprzednim rozdziale konfiguracji kontroli źródła Git.</span><span class="sxs-lookup"><span data-stu-id="2f338-147">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="2f338-148">Potwierdź operację usunięcia, klikając przycisk **tak** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-148">Confirm the removal operation by clicking the **Yes** button.</span></span>

## <a name="create-a-vsts-account"></a><span data-ttu-id="2f338-149">Utwórz konto usługi VSTS</span><span class="sxs-lookup"><span data-stu-id="2f338-149">Create a VSTS account</span></span>

1. <span data-ttu-id="2f338-150">Otwórz przeglądarkę i przejdź do [strony tworzenia konta usługi VSTS](https://go.microsoft.com/fwlink/?LinkId=307137).</span><span class="sxs-lookup"><span data-stu-id="2f338-150">Open a browser, and navigate to the [VSTS account creation page](https://go.microsoft.com/fwlink/?LinkId=307137).</span></span>
1. <span data-ttu-id="2f338-151">Wpisz unikatową nazwę do **wybierz łatwą do zapamiętania nazwę** pole tekstowe w celu utworzenia adresu URL do uzyskiwania dostępu do konta usługi VSTS.</span><span class="sxs-lookup"><span data-stu-id="2f338-151">Type a unique name into the **Pick a memorable name** textbox to form the URL for accessing your VSTS account.</span></span>
1. <span data-ttu-id="2f338-152">Wybierz **Git** przycisk radiowy, ponieważ kod znajduje się w repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="2f338-152">Select the **Git** radio button, since the code is hosted in a GitHub repository.</span></span>
1. <span data-ttu-id="2f338-153">Kliknij przycisk **Kontynuuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-153">Click the **Continue** button.</span></span> <span data-ttu-id="2f338-154">Po krótkim czasie oczekiwania, konta i projektu zespołowego o nazwie *MyFirstProject*, są tworzone.</span><span class="sxs-lookup"><span data-stu-id="2f338-154">After a short wait, an account and a team project, named *MyFirstProject*, are created.</span></span>

    ![Strona tworzenia konta usługi VSTS](media/cicd/vsts-account-creation.png)

1. <span data-ttu-id="2f338-156">Otwórz potwierdzenie e-mail wskazujące, że konto usługi VSTS i projekt gotowy do użycia.</span><span class="sxs-lookup"><span data-stu-id="2f338-156">Open the confirmation email indicating that the VSTS account and project are ready for use.</span></span> <span data-ttu-id="2f338-157">Kliknij przycisk **Rozpocznij swój projekt** przycisku:</span><span class="sxs-lookup"><span data-stu-id="2f338-157">Click the **Start your project** button:</span></span>

    ![Uruchom projekt, przycisk](media/cicd/vsts-start-project.png)

1. <span data-ttu-id="2f338-159">W przeglądarce zostanie otwarty  *\<account_name\>. visualstudio.com*.</span><span class="sxs-lookup"><span data-stu-id="2f338-159">A browser opens to *\<account_name\>.visualstudio.com*.</span></span> <span data-ttu-id="2f338-160">Kliknij przycisk *MyFirstProject* link, aby rozpocząć konfigurowanie projektu DevOps potoku.</span><span class="sxs-lookup"><span data-stu-id="2f338-160">Click the *MyFirstProject* link to begin configuring the project's DevOps pipeline.</span></span>

## <a name="configure-the-devops-pipeline"></a><span data-ttu-id="2f338-161">Konfigurowanie potoku metodyki DevOps</span><span class="sxs-lookup"><span data-stu-id="2f338-161">Configure the DevOps pipeline</span></span>

<span data-ttu-id="2f338-162">Istnieją trzy różne kroki, aby zakończyć.</span><span class="sxs-lookup"><span data-stu-id="2f338-162">There are three distinct steps to complete.</span></span> <span data-ttu-id="2f338-163">Wykonując kroki w wynikach następujące trzy sekcje w operacyjnej potoku metodyki DevOps.</span><span class="sxs-lookup"><span data-stu-id="2f338-163">Completing the steps in the following three sections results in an operational DevOps pipeline.</span></span>

### <a name="grant-vsts-access-to-the-github-repository"></a><span data-ttu-id="2f338-164">Udzielanie dostępu usługi VSTS do repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="2f338-164">Grant VSTS access to the GitHub repository</span></span>

1. <span data-ttu-id="2f338-165">Rozwiń **lub kompilowania kodu z repozytorium zewnętrznego** właściwości accordion.</span><span class="sxs-lookup"><span data-stu-id="2f338-165">Expand the **or build code from an external repository** accordion.</span></span> <span data-ttu-id="2f338-166">Kliknij przycisk **konfiguracji kompilacji** przycisku:</span><span class="sxs-lookup"><span data-stu-id="2f338-166">Click the **Setup Build** button:</span></span>

    ![Konfigurowanie przycisku kompilacji](media/cicd/vsts-setup-build.png)

1. <span data-ttu-id="2f338-168">Wybierz **GitHub** opcję **wybierz źródło** sekcji:</span><span class="sxs-lookup"><span data-stu-id="2f338-168">Select the **GitHub** option from the **Select a source** section:</span></span>

    ![Wybierz źródło - GitHub](media/cicd/vsts-select-source.png)

1. <span data-ttu-id="2f338-170">Autoryzacja jest wymagana, zanim usługa VSTS mogą uzyskiwać dostęp do repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="2f338-170">Authorization is required before VSTS can access your GitHub repository.</span></span> <span data-ttu-id="2f338-171">Wprowadź *< GitHub_username > połączenie usługi GitHub* w **nazwa połączenia** pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="2f338-171">Enter *<GitHub_username> GitHub connection* in the **Connection name** textbox.</span></span> <span data-ttu-id="2f338-172">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2f338-172">For example:</span></span>

    ![Nazwa połączenia usługi GitHub](media/cicd/vsts-repo-authz.png)

1. <span data-ttu-id="2f338-174">Jeśli uwierzytelnianie dwuskładnikowe jest włączone na koncie usługi GitHub, osobisty token dostępu jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="2f338-174">If two-factor authentication is enabled on your GitHub account, a personal access token is required.</span></span> <span data-ttu-id="2f338-175">W takim przypadku kliknij **autoryzacji za pomocą osobisty token dostępu GitHub** łącza.</span><span class="sxs-lookup"><span data-stu-id="2f338-175">In that case, click the **Authorize with a GitHub personal access token** link.</span></span> <span data-ttu-id="2f338-176">Zobacz [oficjalnych instrukcji tworzenia tokenu osobistego dostępu GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) Aby uzyskać pomoc.</span><span class="sxs-lookup"><span data-stu-id="2f338-176">See the [official GitHub personal access token creation instructions](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) for help.</span></span> <span data-ttu-id="2f338-177">Tylko *repozytorium* zakres uprawnień jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="2f338-177">Only the *repo* scope of permissions is needed.</span></span> <span data-ttu-id="2f338-178">W przeciwnym razie kliknij przycisk **autoryzacji przy użyciu protokołu OAuth** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-178">Otherwise, click the **Authorize using OAuth** button.</span></span>
1. <span data-ttu-id="2f338-179">Po wyświetleniu monitu zaloguj się do konta usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="2f338-179">When prompted, sign in to your GitHub account.</span></span> <span data-ttu-id="2f338-180">Następnie wybierz polecenie Autoryzuj, aby udzielić dostępu do konta usługi VSTS.</span><span class="sxs-lookup"><span data-stu-id="2f338-180">Then select Authorize to grant access to your VSTS account.</span></span> <span data-ttu-id="2f338-181">Jeśli to się powiedzie, jest tworzony nowy punkt końcowy usługi.</span><span class="sxs-lookup"><span data-stu-id="2f338-181">If successful, a new service endpoint is created.</span></span>
1. <span data-ttu-id="2f338-182">Kliknij przycisk wielokropka obok **repozytorium** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-182">Click the ellipsis button next to the **Repository** button.</span></span> <span data-ttu-id="2f338-183">Wybierz *< GitHub_username > / prosty kanału informacyjnego czytników* repozytorium z listy.</span><span class="sxs-lookup"><span data-stu-id="2f338-183">Select the *<GitHub_username>/simple-feed-reader* repository from the list.</span></span> <span data-ttu-id="2f338-184">Kliknij przycisk **wybierz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-184">Click the **Select** button.</span></span>
1. <span data-ttu-id="2f338-185">Wybierz *wzorca* gałęzi od **domyślna gałąź dla kompilacji ręcznej i zaplanowane** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2f338-185">Select the *master* branch from the **Default branch for manual and scheduled builds** drop-down.</span></span> <span data-ttu-id="2f338-186">Kliknij przycisk **Kontynuuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-186">Click the **Continue** button.</span></span> <span data-ttu-id="2f338-187">Zostanie wyświetlona strona wybór szablonu.</span><span class="sxs-lookup"><span data-stu-id="2f338-187">The template selection page appears.</span></span>

### <a name="create-the-build-definition"></a><span data-ttu-id="2f338-188">Utwórz definicję kompilacji</span><span class="sxs-lookup"><span data-stu-id="2f338-188">Create the build definition</span></span>

1. <span data-ttu-id="2f338-189">Na stronie Wybór szablonu wprowadź *platformy ASP.NET Core* w polu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="2f338-189">From the template selection page, enter *ASP.NET Core* in the search box:</span></span>

    ![Wyszukiwanie platformy ASP.NET Core na stronie szablonu](media/cicd/vsts-template-selection.png)

1. <span data-ttu-id="2f338-191">Szablon wyniki wyszukiwania są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="2f338-191">The template search results appear.</span></span> <span data-ttu-id="2f338-192">Umieść kursor nad **platformy ASP.NET Core** szablonu, a następnie kliknij przycisk **Zastosuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-192">Hover over the **ASP.NET Core** template, and click the **Apply** button.</span></span>
1. <span data-ttu-id="2f338-193">**Zadania** zostanie wyświetlona karta definicji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2f338-193">The **Tasks** tab of the build definition appears.</span></span> <span data-ttu-id="2f338-194">Kliknij przycisk **wyzwalaczy** kartę.</span><span class="sxs-lookup"><span data-stu-id="2f338-194">Click the **Triggers** tab.</span></span>
1. <span data-ttu-id="2f338-195">Sprawdź **Włącz ciągłą integrację** pole.</span><span class="sxs-lookup"><span data-stu-id="2f338-195">Check the **Enable continuous integration** box.</span></span> <span data-ttu-id="2f338-196">W obszarze **filtry gałęzi** sekcji, upewnij się, że **typu** listy rozwijanej jest równa *Include*.</span><span class="sxs-lookup"><span data-stu-id="2f338-196">Under the **Branch filters** section, confirm that the **Type** drop-down is set to *Include*.</span></span> <span data-ttu-id="2f338-197">Ustaw **gałęzi specyfikacji** menu rozwijane *wzorca*.</span><span class="sxs-lookup"><span data-stu-id="2f338-197">Set the **Branch specification** drop-down to *master*.</span></span>

    ![Włączanie ustawień ciągłej integracji](media/cicd/vsts-enable-ci.png)

    <span data-ttu-id="2f338-199">Te ustawienia powodują kompilacji do wyzwalania wypchnięcie wszelkie zmiany do *wzorca* gałęzi w repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="2f338-199">These settings cause a build to trigger when any change is pushed to the *master* branch of the GitHub repository.</span></span> <span data-ttu-id="2f338-200">Ciągła Integracja jest sprawdzana w [Zatwierdź zmiany w usłudze GitHub i automatycznie wdrażać na platformie Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) sekcji.</span><span class="sxs-lookup"><span data-stu-id="2f338-200">Continuous integration is tested in the [Commit changes to GitHub and automatically deploy to Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) section.</span></span>

1. <span data-ttu-id="2f338-201">Kliknij przycisk **Zapisz k & olejką** przycisk, a następnie wybierz pozycję **Zapisz** opcji:</span><span class="sxs-lookup"><span data-stu-id="2f338-201">Click the **Save & queue** button, and select the **Save** option:</span></span>

    ![Przycisk Zapisz](media/cicd/vsts-save-build.png)

1. <span data-ttu-id="2f338-203">Zostanie wyświetlony następujący modalne okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="2f338-203">The following modal dialog appears:</span></span>

    ![Zapisz definicję kompilacji - modalne okno dialogowe](media/cicd/vsts-save-modal.png)

    <span data-ttu-id="2f338-205">Użyj domyślnego folderu z *\\*i kliknij przycisk **Zapisz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-205">Use the default folder of *\\*, and click the **Save** button.</span></span>

### <a name="create-the-release-pipeline"></a><span data-ttu-id="2f338-206">Twórz potoki wydania</span><span class="sxs-lookup"><span data-stu-id="2f338-206">Create the release pipeline</span></span>

1. <span data-ttu-id="2f338-207">Kliknij przycisk **wersji** kartę projektu zespołowego.</span><span class="sxs-lookup"><span data-stu-id="2f338-207">Click the **Releases** tab of your team project.</span></span> <span data-ttu-id="2f338-208">Kliknij przycisk **nowy potok** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-208">Click the **New pipeline** button.</span></span>

    ![Karta — nowy przycisk definicji wydania](media/cicd/vsts-new-release-definition.png)

    <span data-ttu-id="2f338-210">Zostanie wyświetlone okienko wyboru szablonu.</span><span class="sxs-lookup"><span data-stu-id="2f338-210">The template selection pane appears.</span></span>

1. <span data-ttu-id="2f338-211">Na stronie Wybór szablonu wprowadź *usługi App Service* w polu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="2f338-211">From the template selection page, enter *App Service* in the search box:</span></span>

    ![Pole wyszukiwania szablonu potok wydania](media/cicd/vsts-release-template-search.png)

1. <span data-ttu-id="2f338-213">Szablon wyniki wyszukiwania są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="2f338-213">The template search results appear.</span></span> <span data-ttu-id="2f338-214">Umieść kursor nad **wdrożenie usługi aplikacji Azure z gniazdem** szablonu, a następnie kliknij przycisk **Zastosuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-214">Hover over the **Azure App Service Deployment with Slot** template, and click the **Apply** button.</span></span> <span data-ttu-id="2f338-215">**Potoku** zostanie wyświetlona karta potoku tworzenia wersji.</span><span class="sxs-lookup"><span data-stu-id="2f338-215">The **Pipeline** tab of the release pipeline appears.</span></span>

    ![Potok wydań kartę potoku](media/cicd/vsts-release-definition-pipeline.png)

1. <span data-ttu-id="2f338-217">Kliknij przycisk **Dodaj** znajdujący się w **artefaktów** pole.</span><span class="sxs-lookup"><span data-stu-id="2f338-217">Click the **Add** button in the **Artifacts** box.</span></span> <span data-ttu-id="2f338-218">**Dodawanie artefaktu** zostanie wyświetlony panel:</span><span class="sxs-lookup"><span data-stu-id="2f338-218">The **Add artifact** panel appears:</span></span>

    ![Potok wydań — Dodawanie panelu artefaktu](media/cicd/vsts-release-add-artifact.png)

1. <span data-ttu-id="2f338-220">Wybierz **kompilacji** Kafelek z **typ źródła** sekcji.</span><span class="sxs-lookup"><span data-stu-id="2f338-220">Select the **Build** tile from the **Source type** section.</span></span> <span data-ttu-id="2f338-221">Tego typu umożliwia łączenie potoku tworzenia wersji do definicji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2f338-221">This type allows for the linking of the release pipeline to the build definition.</span></span>
1. <span data-ttu-id="2f338-222">Wybierz *MyFirstProject* z **projektu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2f338-222">Select *MyFirstProject* from the **Project** drop-down.</span></span>
1. <span data-ttu-id="2f338-223">Wybierz nazwę definicji kompilacji *MyFirstProject ASP.NET Core-CI*, z **źródło (definicja kompilacji)** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2f338-223">Select the build definition name, *MyFirstProject-ASP.NET Core-CI*, from the **Source (Build definition)** drop-down.</span></span>
1. <span data-ttu-id="2f338-224">Wybierz *najnowsze* z **domyślną wersję** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2f338-224">Select *Latest* from the **Default version** drop-down.</span></span> <span data-ttu-id="2f338-225">Ta opcja tworzy artefaktów generowane przez działanie najnowszych definicji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2f338-225">This option builds the artifacts produced by the latest run of the build definition.</span></span>
1. <span data-ttu-id="2f338-226">Zastąp tekst umieszczony w **alias źródła** pole tekstowe z *porzucić*.</span><span class="sxs-lookup"><span data-stu-id="2f338-226">Replace the text in the **Source alias** textbox with *Drop*.</span></span>
1. <span data-ttu-id="2f338-227">Kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-227">Click the **Add** button.</span></span> <span data-ttu-id="2f338-228">**Artefaktów** sekcji aktualizacje, aby wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="2f338-228">The **Artifacts** section updates to display the changes.</span></span>
1. <span data-ttu-id="2f338-229">Kliknij ikonę pioruna umożliwiające ciągłych wdrożeń:</span><span class="sxs-lookup"><span data-stu-id="2f338-229">Click the lightning bolt icon to enable continuous deployments:</span></span>

    ![Potok tworzenia wersji artefaktów - ikonę pioruna](media/cicd/vsts-artifacts-lightning-bolt.png)

    <span data-ttu-id="2f338-231">Po włączeniu tej opcji wdrożenia wystąpienia każdorazowo, gdy dostępna jest nowa kompilacja.</span><span class="sxs-lookup"><span data-stu-id="2f338-231">With this option enabled, a deployment occurs each time a new build is available.</span></span>
1. <span data-ttu-id="2f338-232">A **wyzwalacz ciągłego wdrażania** po prawej stronie zostanie wyświetlony panel.</span><span class="sxs-lookup"><span data-stu-id="2f338-232">A **Continuous deployment trigger** panel appears to the right.</span></span> <span data-ttu-id="2f338-233">Kliknij przycisk przełączania, aby włączyć tę funkcję.</span><span class="sxs-lookup"><span data-stu-id="2f338-233">Click the toggle button to enable the feature.</span></span> <span data-ttu-id="2f338-234">Nie jest wymagane do włączenia **wyzwalacza żądania ściągnięcia**.</span><span class="sxs-lookup"><span data-stu-id="2f338-234">It isn't necessary to enable the **Pull request trigger**.</span></span>
1. <span data-ttu-id="2f338-235">Kliknij przycisk **Dodaj** listy rozwijanej w **tworzyć filtry gałęzi** sekcji.</span><span class="sxs-lookup"><span data-stu-id="2f338-235">Click the **Add** drop-down in the **Build branch filters** section.</span></span> <span data-ttu-id="2f338-236">Wybierz **gałęzi domyślnej Build Definition** opcji.</span><span class="sxs-lookup"><span data-stu-id="2f338-236">Choose the **Build Definition's default branch** option.</span></span> <span data-ttu-id="2f338-237">Ten filtr powoduje, że wydania do wyzwalania tylko w przypadku kompilacji z repozytorium GitHub *wzorca* gałęzi.</span><span class="sxs-lookup"><span data-stu-id="2f338-237">This filter causes the release to trigger only for a build from the GitHub repository's *master* branch.</span></span>
1. <span data-ttu-id="2f338-238">Kliknij przycisk **Zapisz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-238">Click the **Save** button.</span></span> <span data-ttu-id="2f338-239">Kliknij przycisk **OK** przycisku w wynikowym **Zapisz** modalne okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="2f338-239">Click the **OK** button in the resulting **Save** modal dialog.</span></span>
1. <span data-ttu-id="2f338-240">Kliknij przycisk **środowisko 1** pole.</span><span class="sxs-lookup"><span data-stu-id="2f338-240">Click the **Environment 1** box.</span></span> <span data-ttu-id="2f338-241">**Środowiska** po prawej stronie zostanie wyświetlony panel.</span><span class="sxs-lookup"><span data-stu-id="2f338-241">An **Environment** panel appears to the right.</span></span> <span data-ttu-id="2f338-242">Zmiana *środowisko 1* tekstu w **Nazwa środowiska** polu tekstowym *produkcji*.</span><span class="sxs-lookup"><span data-stu-id="2f338-242">Change the *Environment 1* text in the **Environment name** textbox to *Production*.</span></span>

   ![Potok wydań — pole tekstowe Nazwa środowiska](media/cicd/vsts-environment-name-textbox.png)

1. <span data-ttu-id="2f338-244">Kliknij przycisk **fazy 1, 2 zadania** łącze w **produkcji** pola:</span><span class="sxs-lookup"><span data-stu-id="2f338-244">Click the **1 phase, 2 tasks** link in the **Production** box:</span></span>

    ![Potok wydań — link.png środowiska produkcyjnego](media/cicd/vsts-production-link.png)

    <span data-ttu-id="2f338-246">**Zadania** pojawi się karta środowiska.</span><span class="sxs-lookup"><span data-stu-id="2f338-246">The **Tasks** tab of the environment appears.</span></span>
1. <span data-ttu-id="2f338-247">Kliknij przycisk **wdrożenia usługi Azure App Service do gniazda** zadania.</span><span class="sxs-lookup"><span data-stu-id="2f338-247">Click the **Deploy Azure App Service to Slot** task.</span></span> <span data-ttu-id="2f338-248">Jego ustawienia są wyświetlane w panelu po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="2f338-248">Its settings appear in a panel to the right.</span></span>
1. <span data-ttu-id="2f338-249">Wybierz subskrypcję platformy Azure skojarzone z usługą App **subskrypcji platformy Azure** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2f338-249">Select the Azure subscription associated with the App Service from the **Azure subscription** drop-down.</span></span> <span data-ttu-id="2f338-250">Po wybraniu kliknij **Autoryzuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-250">Once selected, click the **Authorize** button.</span></span>
1. <span data-ttu-id="2f338-251">Wybierz *aplikacji sieci Web* z **typ aplikacji** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2f338-251">Select *Web App* from the **App type** drop-down.</span></span>
1. <span data-ttu-id="2f338-252">Wybierz *mywebapp / < unique_number / >* z **nazwa usługi App service** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2f338-252">Select *mywebapp/<unique_number/>* from the **App service name** drop-down.</span></span>
1. <span data-ttu-id="2f338-253">Wybierz *AzureTutorial* z **grupy zasobów** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2f338-253">Select *AzureTutorial* from the **Resource group** drop-down.</span></span>
1. <span data-ttu-id="2f338-254">Wybierz *przemieszczania* z **miejsca** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2f338-254">Select *staging* from the **Slot** drop-down.</span></span>
1. <span data-ttu-id="2f338-255">Kliknij przycisk **Zapisz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-255">Click the **Save** button.</span></span>
1. <span data-ttu-id="2f338-256">Umieść kursor nad domyślna nazwa potoku wydania.</span><span class="sxs-lookup"><span data-stu-id="2f338-256">Hover over the default release pipeline name.</span></span> <span data-ttu-id="2f338-257">Kliknij ikonę ołówka, aby go edytować.</span><span class="sxs-lookup"><span data-stu-id="2f338-257">Click the pencil icon to edit it.</span></span> <span data-ttu-id="2f338-258">Użyj *MyFirstProject ASP.NET Core-CD* jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="2f338-258">Use *MyFirstProject-ASP.NET Core-CD* as the name.</span></span>

    ![Nazwa potoku wydania](media/cicd/vsts-release-definition-name.png)

1. <span data-ttu-id="2f338-260">Kliknij przycisk **Zapisz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="2f338-260">Click the **Save** button.</span></span>

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a><span data-ttu-id="2f338-261">Zatwierdź zmiany w usłudze GitHub i automatycznie wdrażać na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="2f338-261">Commit changes to GitHub and automatically deploy to Azure</span></span>

1. <span data-ttu-id="2f338-262">Otwórz *SimpleFeedReader.sln* w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2f338-262">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
1. <span data-ttu-id="2f338-263">W Eksploratorze rozwiązań Otwórz *Pages\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2f338-263">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="2f338-264">Zmiana `<h2>Simple Feed Reader - V3</h2>` do `<h2>Simple Feed Reader - V4</h2>`.</span><span class="sxs-lookup"><span data-stu-id="2f338-264">Change `<h2>Simple Feed Reader - V3</h2>` to `<h2>Simple Feed Reader - V4</h2>`.</span></span>
1. <span data-ttu-id="2f338-265">Naciśnij klawisz **Ctrl**+**Shift**+**B** do skompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f338-265">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
1. <span data-ttu-id="2f338-266">Przekaż plik z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="2f338-266">Commit the file to the GitHub repository.</span></span> <span data-ttu-id="2f338-267">Użyj jednej **zmiany** strony w programie Visual Studio *Team Explorer* tab lub wykonaj następujące czynności za pomocą powłoki poleceń na komputerze lokalnym:</span><span class="sxs-lookup"><span data-stu-id="2f338-267">Use either the **Changes** page in Visual Studio's *Team Explorer* tab, or execute the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. <span data-ttu-id="2f338-268">Wypychanie zmiany *wzorca* gałęzi do *pochodzenia* zdalnego repozytorium GitHub:</span><span class="sxs-lookup"><span data-stu-id="2f338-268">Push the change in the *master* branch to the *origin* remote of your GitHub repository:</span></span>

    ```console
    git push origin master
    ```

    <span data-ttu-id="2f338-269">Zatwierdzenie pojawia się w repozytorium GitHub *wzorca* gałęzi:</span><span class="sxs-lookup"><span data-stu-id="2f338-269">The commit appears in the GitHub repository's *master* branch:</span></span>

    ![GitHub zatwierdzenia w gałęzi głównej](media/cicd/github-commit.png)

    <span data-ttu-id="2f338-271">Kompilacja zostaje wyzwolona, ponieważ integracja ciągła jest włączone w definicji kompilacji **wyzwalaczy** karty:</span><span class="sxs-lookup"><span data-stu-id="2f338-271">The build is triggered, since continuous integration is enabled in the build definition's **Triggers** tab:</span></span>

    ![Włącz ciągłą integrację](media/cicd/enable-ci.png)

1. <span data-ttu-id="2f338-273">Przejdź do **kolejce** karcie **kompilowania i wydawania** > **kompilacje** strony usługi VSTS.</span><span class="sxs-lookup"><span data-stu-id="2f338-273">Navigate to the **Queued** tab of the **Build and Release** > **Builds** page in VSTS.</span></span> <span data-ttu-id="2f338-274">Kompilacja w kolejce pokazuje gałęzi i zatwierdzeń, które wywołały kompilację:</span><span class="sxs-lookup"><span data-stu-id="2f338-274">The queued build shows the branch and commit that triggered the build:</span></span>

    ![kolejki kompilacji](media/cicd/build-queued.png)

1. <span data-ttu-id="2f338-276">Gdy kompilacja zakończy się powodzeniem, odbywa się wdrażanie na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="2f338-276">Once the build succeeds, a deployment to Azure occurs.</span></span> <span data-ttu-id="2f338-277">Przejdź do aplikacji w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="2f338-277">Navigate to the app in the browser.</span></span> <span data-ttu-id="2f338-278">Zwróć uwagę, że tekst "4" jest wyświetlany w nagłówku:</span><span class="sxs-lookup"><span data-stu-id="2f338-278">Notice that the "V4" text appears in the heading:</span></span>

    ![zaktualizowana aplikacja](media/cicd/updated-app-v4.png)

## <a name="examine-the-vsts-devops-pipeline"></a><span data-ttu-id="2f338-280">Sprawdź z potokiem metodyki DevOps w usłudze VSTS</span><span class="sxs-lookup"><span data-stu-id="2f338-280">Examine the VSTS DevOps pipeline</span></span>

### <a name="build-definition"></a><span data-ttu-id="2f338-281">Definicja kompilacji</span><span class="sxs-lookup"><span data-stu-id="2f338-281">Build definition</span></span>

<span data-ttu-id="2f338-282">Utworzono definicję kompilacji o nazwie *MyFirstProject ASP.NET Core-CI*.</span><span class="sxs-lookup"><span data-stu-id="2f338-282">A build definition was created with the name *MyFirstProject-ASP.NET Core-CI*.</span></span> <span data-ttu-id="2f338-283">Po zakończeniu kompilacji tworzy *zip* zasoby, które mają zostać opublikowane w tym pliku.</span><span class="sxs-lookup"><span data-stu-id="2f338-283">Upon completion, the build produces a *.zip* file including the assets to be published.</span></span> <span data-ttu-id="2f338-284">Potok wydania służy do wdrażania tych zasobów na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="2f338-284">The release pipeline deploys those assets to Azure.</span></span>

<span data-ttu-id="2f338-285">Definicja kompilacji **zadania** karcie wyświetlane są poszczególne kroki, które są używane.</span><span class="sxs-lookup"><span data-stu-id="2f338-285">The build definition's **Tasks** tab lists the individual steps being used.</span></span> <span data-ttu-id="2f338-286">Istnieje pięć zadań kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2f338-286">There are five build tasks.</span></span>

![Definicja zadania kompilacji](media/cicd/build-definition-tasks.png)

1. <span data-ttu-id="2f338-288">**Przywróć** &mdash; Executes `dotnet restore` polecenia w celu przywrócenia pakietów NuGet aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f338-288">**Restore** &mdash; Executes the `dotnet restore` command to restore the app's NuGet packages.</span></span> <span data-ttu-id="2f338-289">Domyślny pakiet źródła danych, używany jest adres nuget.org.</span><span class="sxs-lookup"><span data-stu-id="2f338-289">The default package feed used is nuget.org.</span></span>
1. <span data-ttu-id="2f338-290">**Tworzenie** &mdash; Executes `dotnet build --configuration Release` polecenie, aby skompilować kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f338-290">**Build** &mdash; Executes the `dotnet build --configuration Release` command to compile the app's code.</span></span> <span data-ttu-id="2f338-291">To `--configuration` opcja jest używana w celu wygenerowania zoptymalizowanych wersję kodu, który jest odpowiedni dla wdrożenia w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="2f338-291">This `--configuration` option is used to produce an optimized version of the code, which is suitable for deployment to a production environment.</span></span> <span data-ttu-id="2f338-292">Modyfikowanie *BuildConfiguration* zmiennej w definicji kompilacji **zmienne** kartę, jeśli na przykład konfiguracji debugowania nie jest konieczne.</span><span class="sxs-lookup"><span data-stu-id="2f338-292">Modify the *BuildConfiguration* variable on the build definition's **Variables** tab if, for example, a debug configuration is needed.</span></span>
1. <span data-ttu-id="2f338-293">**Test** &mdash; Executes `dotnet test --configuration Release` polecenie, aby uruchomić testy jednostkowe aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2f338-293">**Test** &mdash; Executes the `dotnet test --configuration Release` command to run the app's unit tests.</span></span> <span data-ttu-id="2f338-294">Jeśli którykolwiek z testów nie powiedzie się, kompilacja nie powiedzie się i nie jest wdrożona.</span><span class="sxs-lookup"><span data-stu-id="2f338-294">If any of the tests fail, the build fails and isn't deployed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2f338-295">Aby sprawdzić jednostki testy są poprawne, zmodyfikuj *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* celowo przerwanie jedno z badań, takie jak zmiana `Assert.True(result.Count > 0);` do `Assert.False(result.Count > 0);` w `Returns_News_Stories_Given_Valid_Uri()` Metoda.</span><span class="sxs-lookup"><span data-stu-id="2f338-295">To verify the unit tests are working correctly, modify *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* to purposefully break one of the tests, such as by changing `Assert.True(result.Count > 0);` to `Assert.False(result.Count > 0);` in the `Returns_News_Stories_Given_Valid_Uri()` method.</span></span> <span data-ttu-id="2f338-296">Zatwierdź i Wypchnij zmiany.</span><span class="sxs-lookup"><span data-stu-id="2f338-296">Commit and push the change.</span></span> <span data-ttu-id="2f338-297">Kompilacja zakończy się niepowodzeniem i stan potoku kompilacji zmienia się na **nie powiodło się**.</span><span class="sxs-lookup"><span data-stu-id="2f338-297">The build will fail and the build pipeline status changes to **failed**.</span></span> <span data-ttu-id="2f338-298">Przywróć zmiany, zatwierdzać i wypychać ponownie, a kompilacja zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="2f338-298">Revert the change, commit, and push again, and the build succeeds.</span></span>
1. <span data-ttu-id="2f338-299">**Publikowanie** &mdash; Executes `dotnet publish --configuration Release --output <local_path_on_build_agent>` polecenia w celu utworzenia *zip* pliku z artefaktami, które mają zostać wdrożone.</span><span class="sxs-lookup"><span data-stu-id="2f338-299">**Publish** &mdash; Executes the `dotnet publish --configuration Release --output <local_path_on_build_agent>` command to produce a *.zip* file with the artifacts to be deployed.</span></span> <span data-ttu-id="2f338-300">`--output` Opcja określa lokalizację publikowania *zip* pliku.</span><span class="sxs-lookup"><span data-stu-id="2f338-300">The `--output` option specifies the publish location of the *.zip* file.</span></span> <span data-ttu-id="2f338-301">Czy lokalizacja jest określona przez przekazanie [uprzednio zdefiniowanej zmiennej](https://docs.microsoft.com/vsts/pipelines/build/variables) o nazwie `$(build.artifactstagingdirectory)`.</span><span class="sxs-lookup"><span data-stu-id="2f338-301">That location is specified by passing a [predefined variable](https://docs.microsoft.com/vsts/pipelines/build/variables) named `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="2f338-302">Tej zmiennej rozwija ścieżkę lokalną, taką jak *c:\agent\_work\1\a*, w agencie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2f338-302">That variable expands to a local path, such as *c:\agent\_work\1\a*, on the build agent.</span></span>
1. <span data-ttu-id="2f338-303">**Publikowanie artefaktów** &mdash; Publishes *zip* za pomocą **Publikuj** zadania.</span><span class="sxs-lookup"><span data-stu-id="2f338-303">**Publish Artifact** &mdash; Publishes the *.zip* file produced by the **Publish** task.</span></span> <span data-ttu-id="2f338-304">Zadanie akceptuje *zip* lokalizacja jako parametr, który jest uprzednio zdefiniowanej zmiennej pliku `$(build.artifactstagingdirectory)`.</span><span class="sxs-lookup"><span data-stu-id="2f338-304">The task accepts the *.zip* file location as a parameter, which is the predefined variable `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="2f338-305">*Zip* plik jest publikowany jako folder o nazwie *porzucić*.</span><span class="sxs-lookup"><span data-stu-id="2f338-305">The *.zip* file is published as a folder named *drop*.</span></span>

<span data-ttu-id="2f338-306">Kliknij przycisk definicji kompilacji **Podsumowanie** link, aby wyświetlić historię kompilacji z definicji:</span><span class="sxs-lookup"><span data-stu-id="2f338-306">Click the build definition's **Summary** link to view a history of builds with the definition:</span></span>

![Historia definicji kompilacji](media/cicd/build-definition-summary.png)

<span data-ttu-id="2f338-308">Na wynikowej stronie kliknij link odpowiadający numerowi unikatowy kompilacji:</span><span class="sxs-lookup"><span data-stu-id="2f338-308">On the resulting page, click the link corresponding to the unique build number:</span></span>

![Strona podsumowania definicji kompilacji](media/cicd/build-definition-completed.png)

<span data-ttu-id="2f338-310">Zostanie wyświetlone podsumowanie tej określonej kompilacji.</span><span class="sxs-lookup"><span data-stu-id="2f338-310">A summary of this specific build is displayed.</span></span> <span data-ttu-id="2f338-311">Kliknij przycisk **artefaktów** kartę i zwróć uwagę, *porzucić* znajduje się folder utworzony przez kompilację:</span><span class="sxs-lookup"><span data-stu-id="2f338-311">Click the **Artifacts** tab, and notice the *drop* folder produced by the build is listed:</span></span>

![Tworzenie definicji artefaktów - folder do wrzucania](media/cicd/build-definition-artifacts.png)

<span data-ttu-id="2f338-313">Użyj **Pobierz** i **Eksploruj** łącza, aby sprawdzić opublikowanych artefaktów.</span><span class="sxs-lookup"><span data-stu-id="2f338-313">Use the **Download** and **Explore** links to inspect the published artifacts.</span></span>

### <a name="release-pipeline"></a><span data-ttu-id="2f338-314">Potok wydania</span><span class="sxs-lookup"><span data-stu-id="2f338-314">Release pipeline</span></span>

<span data-ttu-id="2f338-315">Potok tworzenia wersji została utworzona przy użyciu nazwy *MyFirstProject ASP.NET Core-CD*:</span><span class="sxs-lookup"><span data-stu-id="2f338-315">A release pipeline was created with the name *MyFirstProject-ASP.NET Core-CD*:</span></span>

![Omówienie potok wydania](media/cicd/release-definition-overview.png)

<span data-ttu-id="2f338-317">Są dwa główne składniki potoku tworzenia wersji **artefaktów** i **środowisk**.</span><span class="sxs-lookup"><span data-stu-id="2f338-317">The two major components of the release pipeline are the **Artifacts** and the **Environments**.</span></span> <span data-ttu-id="2f338-318">Kliknij pole w **artefaktów** sekcji, co spowoduje wyświetlenie następujących panelu:</span><span class="sxs-lookup"><span data-stu-id="2f338-318">Clicking the box in the **Artifacts** section reveals the following panel:</span></span>

![artefaktów potok wydania](media/cicd/release-definition-artifacts.png)

<span data-ttu-id="2f338-320">**Źródło (definicja kompilacji)** wartość reprezentuje definicję kompilacji, z którą jest połączony ten potok wersji.</span><span class="sxs-lookup"><span data-stu-id="2f338-320">The **Source (Build definition)** value represents the build definition to which this release pipeline is linked.</span></span> <span data-ttu-id="2f338-321">*Zip* znajduje się plik utworzony przez pomyślny przebieg definicję kompilacji do *produkcji* środowisko do wdrażania na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="2f338-321">The *.zip* file produced by a successful run of the build definition is provided to the *Production* environment for deployment to Azure.</span></span> <span data-ttu-id="2f338-322">Kliknij przycisk *fazy 1, 2 zadania* łącze w *produkcji* pole środowiska, aby wyświetlić zadania potoku wydania:</span><span class="sxs-lookup"><span data-stu-id="2f338-322">Click the *1 phase, 2 tasks* link in the *Production* environment box to view the release pipeline tasks:</span></span>

![zadania potoku wydania](media/cicd/release-definition-tasks.png)

<span data-ttu-id="2f338-324">Potok wersji składa się z dwóch zadań: *wdrożenia usługi Azure App Service do gniazda* i *zarządzania usługi Azure App Service — gniazda wymiany*.</span><span class="sxs-lookup"><span data-stu-id="2f338-324">The release pipeline consists of two tasks: *Deploy Azure App Service to Slot* and *Manage Azure App Service - Slot Swap*.</span></span> <span data-ttu-id="2f338-325">Kliknięcie pierwszego zadania, co spowoduje wyświetlenie następującej konfiguracji zadania:</span><span class="sxs-lookup"><span data-stu-id="2f338-325">Clicking the first task reveals the following task configuration:</span></span>

![Zadanie wdrażania potoku tworzenia wersji](media/cicd/release-definition-task1.png)

<span data-ttu-id="2f338-327">Subskrypcja platformy Azure, typ usługi, nazwa aplikacji sieci web, grupy zasobów i miejsce wdrożenia są definiowane w zadania wdrażania.</span><span class="sxs-lookup"><span data-stu-id="2f338-327">The Azure subscription, service type, web app name, resource group, and deployment slot are defined in the deployment task.</span></span> <span data-ttu-id="2f338-328">**Pakietu lub folderu** zawiera pole tekstowe *zip* ścieżka pliku do wyodrębniane i wdrożyć *przemieszczania* gniazda *mywebapp\<unikatowy _number\>*  aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="2f338-328">The **Package or folder** textbox holds the *.zip* file path to be extracted and deployed to the *staging* slot of the *mywebapp\<unique_number\>* web app.</span></span>

<span data-ttu-id="2f338-329">Kliknięcie zadania zamiany gniazda, co spowoduje wyświetlenie następującej konfiguracji zadania:</span><span class="sxs-lookup"><span data-stu-id="2f338-329">Clicking the slot swap task reveals the following task configuration:</span></span>

![Zwolnij zadanie zamiany gniazda potoku](media/cicd/release-definition-task2.png)

<span data-ttu-id="2f338-331">Subskrypcja, grupa zasobów, typ usługi, nazwa aplikacji sieci web i szczegóły miejsca wdrożenia znajdują się.</span><span class="sxs-lookup"><span data-stu-id="2f338-331">The subscription, resource group, service type, web app name, and deployment slot details are provided.</span></span> <span data-ttu-id="2f338-332">**Wymiany z produkcją** zaznaczono pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="2f338-332">The **Swap with Production** checkbox is checked.</span></span> <span data-ttu-id="2f338-333">W związku z tym, wdrożone usługi bits *przemieszczania* gniazda zostały zamienione w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="2f338-333">Consequently, the bits deployed to the *staging* slot are swapped into the production environment.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="2f338-334">Materiały uzupełniające</span><span class="sxs-lookup"><span data-stu-id="2f338-334">Additional reading</span></span>

* [<span data-ttu-id="2f338-335">Tworzenie aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f338-335">Build your ASP.NET Core app</span></span>](https://docs.microsoft.com/vsts/build-release/apps/aspnet/build-aspnet-core)
* [<span data-ttu-id="2f338-336">Tworzenie i wdrażanie aplikacji sieci Web platformy Azure</span><span class="sxs-lookup"><span data-stu-id="2f338-336">Build and deploy to an Azure Web App</span></span>](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp)
* [<span data-ttu-id="2f338-337">Zdefiniuj proces kompilacji ciągłej integracji do repozytorium GitHub</span><span class="sxs-lookup"><span data-stu-id="2f338-337">Define a CI build process for your GitHub repository</span></span>](https://docs.microsoft.com/vsts/pipelines/build/ci-build-github)
