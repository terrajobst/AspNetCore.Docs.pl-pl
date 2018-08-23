---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kodu | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: a3d181f3ec8db74781c550720ba4331bdf6478b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755456"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="7fcd8-103">Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kodu</span><span class="sxs-lookup"><span data-stu-id="7fcd8-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="7fcd8-104">przez [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7fcd8-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="7fcd8-105">Pobieranie projektu startowego</span><span class="sxs-lookup"><span data-stu-id="7fcd8-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="7fcd8-106">W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="7fcd8-107">Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7fcd8-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="7fcd8-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="7fcd8-108">Overview</span></span>

<span data-ttu-id="7fcd8-109">Po początkowym wdrożeniu kontynuuje swoją pracę, obsługi i tworzenia witryny sieci web, a niedługo będzie, aby wdrożyć aktualizację.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="7fcd8-110">Ten samouczek przeprowadzi Cię przez proces wdrażania aktualizacji w kodzie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="7fcd8-111">Aktualizację, implementowania i wdrażania w ramach tego samouczka nie wiąże się zmianę w bazie danych; zobaczysz, jakie są różnice dotyczące wdrażania w następnym samouczku zmianę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="7fcd8-112">Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="7fcd8-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="7fcd8-113">Wprowadź zmiana w kodzie</span><span class="sxs-lookup"><span data-stu-id="7fcd8-113">Make a code change</span></span>

<span data-ttu-id="7fcd8-114">Prostym przykładem aktualizacji do aplikacji, należy dodać do **Instruktorzy** listę kursom prowadzonym przez instruktora wybranej strony.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="7fcd8-115">Po uruchomieniu **Instruktorzy** strony, można zauważyć, że istnieją **wybierz** łącza w siatce, ale ich nie robi niczego poza upewnij wiersz szary Włącz w tle.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Strona Instruktorzy z zaznaczenia](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="7fcd8-117">Teraz dodasz kod, który jest uruchamiany, gdy **wybierz** łącze kliknięciu i wyświetla listę kursom prowadzonym przez instruktora wybrane.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="7fcd8-118">W *Instructors.aspx*, Dodaj następujący kod bezpośrednio po **ErrorMessageLabel** `Label` sterowania:</span><span class="sxs-lookup"><span data-stu-id="7fcd8-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="7fcd8-119">Uruchom strony i wybierz pod kierunkiem instruktora.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-119">Run the page and select an instructor.</span></span> <span data-ttu-id="7fcd8-120">Zobaczysz listę kursom prowadzonym przez instruktora tego.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-120">You see a list of courses taught by that instructor.</span></span>

    ![Strona Instruktorzy z kursom prowadzonym](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="7fcd8-122">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="7fcd8-123">Wdrażanie aktualizacji kodu do środowiska testowego</span><span class="sxs-lookup"><span data-stu-id="7fcd8-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="7fcd8-124">Zanim użyjesz profilów publikowania do wdrożenia do testowania, przejściowego i produkcji, należy zmienić opcje publikowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="7fcd8-125">Nie trzeba uruchamiać skrypty wdrażania danych i udzielanie dla bazy danych członkostwa.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="7fcd8-126">Otwórz **publikowanie w sieci Web** kreatora, kliknij prawym przyciskiem myszy projekt ContosoUniversity i klikając **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="7fcd8-127">Kliknij przycisk **testu** profil w **profilu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="7fcd8-128">Kliknij przycisk **ustawienia** kartę.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="7fcd8-129">W obszarze **DefaultConnection** w **baz danych** sekcji wyczyść **Aktualizuj bazę danych** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="7fcd8-130">Kliknij przycisk **profilu** kartę, a następnie kliknij przycisk **przemieszczania** profil w **profilu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="7fcd8-131">Gdy zostanie wyświetlony monit Jeśli chcesz zapisać zmiany wprowadzone do **testu** profilu, kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="7fcd8-132">Wprowadź tę samą zmianę w profilu tymczasowego.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="7fcd8-133">Powtórz te czynności, aby wprowadzić tę samą zmianę w profilu w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="7fcd8-134">Zamknij **publikowanie w sieci Web** kreatora.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="7fcd8-135">Wdrażanie do środowiska testowego jest już polegać na uruchamianie jednym kliknięciem opublikuj go ponownie.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="7fcd8-136">Aby ułatwić ten proces szybciej, można użyć **Web publikacja w pojedynczym kliknięciem** paska narzędzi.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="7fcd8-137">W **widoku** menu, wybierz **pasków narzędzi** , a następnie wybierz **Web publikacja w pojedynczym kliknięciem**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="7fcd8-139">W **Eksploratora rozwiązań**, wybierz projekt ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="7fcd8-140">**Web publikacja w pojedynczym kliknięciem** narzędzi, wybierz **testu** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web** (ikona za pomocą strzałki w lewo i w prawo).</span><span class="sxs-lookup"><span data-stu-id="7fcd8-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="7fcd8-142">Program Visual Studio wdroży zaktualizowaną aplikację, i automatycznie otwiera się przeglądarka do strony głównej.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="7fcd8-143">Uruchom stronę instruktorów i wybierz pod kierunkiem instruktora, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="7fcd8-144">Normalny również wykonać testowanie regresji (oznacza to, że test pozostałych lokacji, aby upewnić się, że nowa zmiana nie przerwać wszelkie istniejące funkcje).</span><span class="sxs-lookup"><span data-stu-id="7fcd8-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="7fcd8-145">Jednak na potrzeby tego samouczka będziesz pominąć ten krok i przejdź do wdrażania aktualizacji przejściowym i produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="7fcd8-146">Podczas ponownego wdrażania, narzędzie Web Deploy automatycznie określa które pliki uległy zmianie, a kopie tylko zmienione pliki na serwer.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="7fcd8-147">Domyślnie narzędzie Web Deploy korzysta z daty ostatniej zmiany plików w celu określenia, które uległy zmianie.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="7fcd8-148">Niektóre systemów kontroli źródła zmienić plik daty, nawet gdy nie zmieniaj zawartości pliku.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="7fcd8-149">W takim przypadku można skonfigurować narzędzie Web Deploy używać sum kontrolnych plików do określenia, które pliki uległy zmianie.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="7fcd8-150">Aby uzyskać więcej informacji, zobacz [Dlaczego wszystkie pliki Rozpoczynanie ponownego wdrożenia, ale nie je zmienić?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) w często zadawanych Pytaniach wdrożenia dla platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="7fcd8-151">Nastavit aplikaci offline podczas wdrażania</span><span class="sxs-lookup"><span data-stu-id="7fcd8-151">Take the application offline during deployment</span></span>

<span data-ttu-id="7fcd8-152">Zmiana, którą jest wdrażany jest teraz prostą zmianę do jednej strony.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="7fcd8-153">Czasami wdrażanie większe zmiany lub wdrażanie zmian w kodzie i bazy danych — a lokacji może działać nieprawidłowo, jeśli użytkownik zgłasza żądanie strony przed zakończeniem wdrażania.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="7fcd8-154">Aby uniemożliwić użytkownikom dostęp do witryny, w trakcie wdrażania, można użyć *aplikacji\_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="7fcd8-155">Po umieszczeniu pliku o nazwie *aplikacji\_offline.htm* w folderze głównym aplikacji, usługi IIS automatycznie wyświetla ten plik, zamiast uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="7fcd8-156">Tak, aby uniemożliwić dostęp podczas wdrażania, możesz umieścić *aplikacji\_offline.htm* w folderze głównym Uruchom proces wdrażania, a następnie usuń *aplikacji\_offline.htm* po pomyślnym wdrożenie.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="7fcd8-157">Można skonfigurować narzędzie Web Deploy, aby automatycznie wprowadzić domyślny *aplikacji\_offline.htm* plików na serwerze, po uruchomieniu, wdrażanie i usunąć go po zakończeniu tej operacji.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="7fcd8-158">Zrobić, że wszystkie trzeba jest Dodaj następujący element XML do pliku (.pubxml) profil publikowania:</span><span class="sxs-lookup"><span data-stu-id="7fcd8-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="7fcd8-159">W tym samouczku pokazano, jak utworzyć i używać niestandardowego *aplikacji\_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="7fcd8-160">Za pomocą *aplikacji\_offline.htm* w witrynie etapowej nie jest wymagane, ponieważ nie masz użytkowników uzyskujących dostęp do tymczasowej witryny.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="7fcd8-161">Ale jest dobrą praktyką na potrzeby przemieszczania przetestować wszystko, co w sposób, który ma zostać umieszczony w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="7fcd8-162">Tworzenie aplikacji\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="7fcd8-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="7fcd8-163">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie i kliknij przycisk **Dodaj**, a następnie kliknij przycisk **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="7fcd8-164">Tworzenie **strony HTML** o nazwie *aplikacji\_offline.htm* (Usuń końcowe "l" w *.html* rozszerzenia, które program Visual Studio tworzy domyślnie).</span><span class="sxs-lookup"><span data-stu-id="7fcd8-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="7fcd8-165">Zastąp kod znaczników szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7fcd8-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="7fcd8-166">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="7fcd8-167">Kopiowanie aplikacji\_offline.htm do głównego folderu witryny sieci web</span><span class="sxs-lookup"><span data-stu-id="7fcd8-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="7fcd8-168">Można użyć dowolnego narzędzia FTP do kopiowania plików do witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="7fcd8-169">[FileZilla](http://filezilla-project.org/) to popularne narzędzie FTP i przedstawiono zrzuty ekranu.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="7fcd8-170">Aby użyć narzędzia FTP, potrzebne są trzy rzeczy: adres URL protokołu FTP, nazwę użytkownika i hasło.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="7fcd8-171">Adres URL jest wyświetlany na stronie pulpitu nawigacyjnego witryny sieci web w portalu zarządzania systemu Azure, a nazwę użytkownika i hasło dla połączenia FTP można znaleźć w *.publishsettings* pliku, który został wcześniej pobrany.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="7fcd8-172">Poniższe kroki pokazują sposób uzyskiwania tych wartości.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="7fcd8-173">W portalu zarządzania Azure kliknij **witryn sieci Web** kartę, a następnie kliknij przycisk tymczasowej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="7fcd8-174">Na **pulpit nawigacyjny** strony, przewiń w dół do znajdowania nazwy hosta FTP **Quick Glance** sekcji.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nazwa hosta FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="7fcd8-176">Otwórz pomostowego *.publishsettings* pliku w Notatniku lub innym edytorze tekstu.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="7fcd8-177">Znajdź `publishProfile` elementu profilu FTP.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="7fcd8-178">Kopiuj `userName` i `userPWD` wartości.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Poświadczenia protokołu FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="7fcd8-180">Otwórz swoje narzędzia FTP i zaloguj się do adresu URL FTP.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="7fcd8-181">Kopiuj *aplikacji\_offline.htm* z folderu rozwiązania, aby */site/wwwroot* folder w witrynie etapowej.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Skopiuj app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="7fcd8-183">Przejdź do adresu URL witryny przemieszczania.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="7fcd8-184">Zobaczysz, że *aplikacji\_offline.htm* zamiast strony głównej zostanie teraz wyświetlona strona.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![app_offline.htm w oknie przeglądarki](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="7fcd8-186">Teraz można przystąpić do wdrażania przejściowego.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="7fcd8-187">Wdrażanie aktualizacji kodu przejściowym i produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="7fcd8-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="7fcd8-188">W **Web publikacja w pojedynczym kliknięciem** narzędzi, wybierz **przemieszczania** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="7fcd8-189">Programu Visual Studio wdroży zaktualizowaną aplikację i otworzy w przeglądarce do strony głównej.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="7fcd8-190">*Aplikacji\_offline.htm* pliku jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="7fcd8-191">Zanim można przetestować, aby zweryfikować pomyślne wdrożenie, musisz usunąć *aplikacji\_offline.htm* pliku.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="7fcd8-192">Wróć do swojego narzędzia FTP i Usuń **aplikacji\_offline.htm** z tymczasowej witryny.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="7fcd8-193">W przeglądarce otwórz stronę Instruktorzy w witrynie etapowej, a następnie wybierz pod kierunkiem instruktora, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="7fcd8-194">Postępuj zgodnie z tą samą procedurą w środowisku produkcyjnym, co na karcie tymczasowej.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="7fcd8-195">Przeglądanie zmian i wdrażanie określonych plików</span><span class="sxs-lookup"><span data-stu-id="7fcd8-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="7fcd8-196">Program Visual Studio 2012 zapewnia również możliwość wdrażania poszczególnych plików.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="7fcd8-197">Dla wybranego pliku można wyświetlać różnice między lokalną wersją a wersją wdrożone, wdrożyć plik do środowiska docelowego lub skopiuj plik ze środowiska docelowego do lokalnego projektu.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="7fcd8-198">W tej części samouczka zobaczysz, jak korzystać z tych funkcji.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="7fcd8-199">Wprowadź zmiany do wdrożenia</span><span class="sxs-lookup"><span data-stu-id="7fcd8-199">Make a change to deploy</span></span>

1. <span data-ttu-id="7fcd8-200">Otwórz *Content/Site.css*i Znajdź blok dla `body` tagu.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="7fcd8-201">Zmień wartość `background-color` z `#fff` do `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="7fcd8-202">Wyświetl zmiany w oknie Podgląd publikowania</span><span class="sxs-lookup"><span data-stu-id="7fcd8-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="7fcd8-203">Kiedy używasz **publikowanie w sieci Web** kreatora, aby opublikować projekt, możesz zobaczyć, jakie zmiany będą publikowane przez dwukrotne kliknięcie pliku w **(wersja zapoznawcza)** okna.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="7fcd8-204">Kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="7fcd8-205">Z **profilu** listy rozwijanej wybierz **testu** profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="7fcd8-206">Kliknij przycisk **Podgląd**, a następnie kliknij przycisk **Uruchom Podgląd**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="7fcd8-207">W **Podgląd** okienku kliknij dwukrotnie **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Kliknij dwukrotnie plik Site.css](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="7fcd8-209">**Podgląd zmian** okna dialogowego pokazuje jego podgląd zmian, które zostaną wdrożone.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Podgląd zmian w pliku Site.css](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="7fcd8-211">Po dwukrotnym kliknięciu *Web.config* pliku **podgląd zmian** okna dialogowego pokazuje wpływ kompilacji przekształcenia konfiguracji i przekształcenia profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="7fcd8-212">W tym momencie nie zostało zrobione wszystko, co mogłoby spowodować *Web.config* plików na serwerze, aby się zmieniają, dlatego powinna się pojawić bez zmian.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="7fcd8-213">Jednak **podgląd zmian** oknie nieprawidłowo wyświetlane dwie zmiany.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="7fcd8-214">Wygląda na to, dwa elementy XML zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="7fcd8-215">Te elementy są dodawane przez proces publikowania, po wybraniu **wykonaj migracje Code First w aplikacji w menu start** Code First klasy kontekstu.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="7fcd8-216">Porównanie jest wykonywane, zanim proces publikowania dodaje te elementy, więc prawdopodobnie są są usuwane, mimo że nie zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="7fcd8-217">Ten błąd zostanie rozwiązany w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="7fcd8-218">Kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-218">Click **Close**.</span></span>
6. <span data-ttu-id="7fcd8-219">Kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-219">Click **Publish**.</span></span>
7. <span data-ttu-id="7fcd8-220">Po otwarciu przeglądarki do strony głównej witryny testu, naciśnij klawisze CTRL + F5, aby spowodować twardych odświeżania, aby zobaczyć efekt zmian CSS.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Efekt zmiany CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="7fcd8-222">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="7fcd8-223">Publikowanie określonych plików w Eksploratorze rozwiązań</span><span class="sxs-lookup"><span data-stu-id="7fcd8-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="7fcd8-224">Załóżmy, że nie niebieskim tłem, takich jak i chcesz przywrócić oryginalny kolor.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="7fcd8-225">W tej sekcji zostaną przywrócone oryginalne ustawienia, publikując określonego pliku bezpośrednio z **Eksploratora rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="7fcd8-226">Otwórz *Content/Site.css* i przywracanie `background-color` ustawienie `#fff`.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="7fcd8-227">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Content/Site.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="7fcd8-228">Menu kontekstowe pokazuje trzy opcje publikowania.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-228">The context menu shows three publish options.</span></span>

    ![Publikowanie opcji dostępnych w Eksploratorze rozwiązań](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="7fcd8-230">Kliknij przycisk **(wersja zapoznawcza) zmieni się na Site.css**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="7fcd8-231">Zostanie otwarte okno, aby wyświetlić różnice między lokalnego pliku i wersja go w środowisku docelowym.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff zawartość/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="7fcd8-233">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **Site.css** ponownie i kliknij przycisk **publikowania pliku Site.css**.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="7fcd8-234">**Działania publikowania internetowego** okno pokazuje, że plik został opublikowany.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Okno działania publikowania w sieci Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="7fcd8-236">Otwórz w przeglądarce `http://localhost/contosouniversity` adresu URL i naciśnij klawisze CTRL + F5, aby spowodować, że twardy odświeżąć, aby zobaczyć efekt arkusze CSS, Zmień.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Strona główna z normalnym CSS](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="7fcd8-238">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="7fcd8-239">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="7fcd8-239">Summary</span></span>

<span data-ttu-id="7fcd8-240">Teraz Przedstawiliśmy kilka metod wdrażania aktualizacji aplikacji, które nie obejmują zmianę w bazie danych. Ponadto przedstawiono sposób podgląd zmian, aby sprawdzić, jakie zostaną zaktualizowane jest oczekiwanych.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="7fcd8-241">Na stronie Instruktorzy ma teraz **kursy prowadzone** sekcji.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Strona Instruktorzy z kursom prowadzonym](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="7fcd8-243">Następnym samouczku dowiesz się, jak wdrożyć zmianę w bazie danych: należy dodać pole daty urodzenia do bazy danych i do strony instruktorów.</span><span class="sxs-lookup"><span data-stu-id="7fcd8-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7fcd8-244">[Poprzednie](deploying-to-production.md)
> [dalej](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="7fcd8-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
