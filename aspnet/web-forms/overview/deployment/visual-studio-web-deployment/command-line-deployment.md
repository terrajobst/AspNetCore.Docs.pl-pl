---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: wdrożenia z wiersza polecenia | Dokumentacja firmy Microsoft'
author: tdykstra
description: Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: acc4a0e7f4744a3759b90e0f1b159da68b7c7362
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890531"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="6693a-103">Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: wdrożenia z wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="6693a-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>
====================
<span data-ttu-id="6693a-104">Przez [Dykstra niestandardowy](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6693a-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="6693a-105">Pobierz początkowego projektu</span><span class="sxs-lookup"><span data-stu-id="6693a-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="6693a-106">Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="6693a-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="6693a-107">Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6693a-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="6693a-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="6693a-108">Overview</span></span>

<span data-ttu-id="6693a-109">W tym samouczku przedstawiono sposób wywołania sieci web programu Visual Studio publikowania potoku z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="6693a-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="6693a-110">Jest to przydatne w scenariuszach, w którym ma zostać [zautomatyzować proces wdrażania](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) zamiast to zrobić ręcznie w programie Visual Studio, zwykle za pomocą [systemu kontroli wersji kodu źródła](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="6693a-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="6693a-111">Zmiany do wdrożenia</span><span class="sxs-lookup"><span data-stu-id="6693a-111">Make a change to deploy</span></span>

<span data-ttu-id="6693a-112">Na stronie informacje do wyświetlania kod szablonu.</span><span class="sxs-lookup"><span data-stu-id="6693a-112">Currently the About page displays the template code.</span></span>

![Strona z kodem szablonu — informacje](command-line-deployment/_static/image1.png)

<span data-ttu-id="6693a-114">Będzie to zastępującą z kodem, który wyświetla podsumowanie rejestracji dla użytkowników domowych.</span><span class="sxs-lookup"><span data-stu-id="6693a-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="6693a-115">Otwórz *About.aspx* pozycję usunięcie wszystkich znaczników wewnątrz `MainContent` `Content` elementu i Wstaw następujący kod w tym miejscu:</span><span class="sxs-lookup"><span data-stu-id="6693a-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="6693a-116">Uruchom projekt i wybierz **o** strony.</span><span class="sxs-lookup"><span data-stu-id="6693a-116">Run the project and select the **About** page.</span></span>

![Strona — informacje](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="6693a-118">Wdróż do testu przy użyciu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="6693a-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="6693a-119">Inna zmiana bazy danych, tak wyłączenie dbDacFx wdrożenia bazy danych dla bazy danych aspnet ContosoUniversity nie wdrażania.</span><span class="sxs-lookup"><span data-stu-id="6693a-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="6693a-120">Otwórz **publikowanie w sieci Web** kreatora i w każdym z tych trzech profilów, wyczyść publikowania **aktualizacji bazy danych** pole wyboru na **ustawienia** kartę.</span><span class="sxs-lookup"><span data-stu-id="6693a-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="6693a-121">Na stronie Start systemu Windows 8, wyszukaj **wiersz polecenia dla VS2012 deweloperów**.</span><span class="sxs-lookup"><span data-stu-id="6693a-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="6693a-122">Kliknij prawym przyciskiem myszy ikonę **wiersz polecenia dla VS2012 deweloperów** i kliknij przycisk **Uruchom jako administrator**.</span><span class="sxs-lookup"><span data-stu-id="6693a-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="6693a-123">Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżka do pliku rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="6693a-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="6693a-124">MSBuild rozwiązania tworzy i wdraża ją do środowiska testowego.</span><span class="sxs-lookup"><span data-stu-id="6693a-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Dane wyjściowe wiersza polecenia](command-line-deployment/_static/image3.png)

<span data-ttu-id="6693a-126">Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, następnie kliknij przycisk **o** stronę, aby sprawdzić, czy wdrożenie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="6693a-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="6693a-127">Jeśli nie utworzono żadnych studentów w teście zostanie wyświetlona pusta strona w obszarze **statystyki treści uczniowie** nagłówka.</span><span class="sxs-lookup"><span data-stu-id="6693a-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="6693a-128">Przejdź do **studentów** kliknij przycisk **dodać uczniowie**i dodać niektóre studentów, a następnie wróć do **o** stronę, aby wyświetlić statystyki dla użytkowników domowych.</span><span class="sxs-lookup"><span data-stu-id="6693a-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Strona w środowisku testowym — informacje](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="6693a-130">Opcje wiersza polecenia klucza</span><span class="sxs-lookup"><span data-stu-id="6693a-130">Key command line options</span></span>

<span data-ttu-id="6693a-131">Polecenie Wprowadzona ścieżka pliku rozwiązania i dwie właściwości przekazanego MSBuild:</span><span class="sxs-lookup"><span data-stu-id="6693a-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="6693a-132">Wdrażanie rozwiązania i wdrażania poszczególnych projektów</span><span class="sxs-lookup"><span data-stu-id="6693a-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="6693a-133">Określanie pliku rozwiązania powoduje, że wszystkie projekty w rozwiązaniu, które ma zostać utworzony.</span><span class="sxs-lookup"><span data-stu-id="6693a-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="6693a-134">Jeśli masz wiele projektów sieci web w rozwiązaniu, stosowane są następujące rozwiązania programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="6693a-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="6693a-135">Właściwości, które określają w wierszu polecenia są przekazywane do każdego projektu.</span><span class="sxs-lookup"><span data-stu-id="6693a-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="6693a-136">W związku z tym każdy projekt sieci web musi mieć profil publikowania o tej nazwie, który określisz.</span><span class="sxs-lookup"><span data-stu-id="6693a-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="6693a-137">Jeśli określisz `/p:PublishProfile=Test`, każdy projekt sieci web musi mieć profil publikowania o nazwie *testu*.</span><span class="sxs-lookup"><span data-stu-id="6693a-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="6693a-138">Jeden projekt może opublikowanie, gdy inny nawet nie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="6693a-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="6693a-139">Aby uzyskać więcej informacji, zobacz wątku stackoverflow [MSBuild kończy się niepowodzeniem i dwa pakiety](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="6693a-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="6693a-140">Jeśli określisz poszczególne projektu zamiast rozwiązanie, należy dodać parametr, który określa wersję programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6693a-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="6693a-141">Jeśli używasz programu Visual Studio 2012 w wierszu polecenia będzie podobny do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="6693a-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="6693a-142">Numer wersji dla programu Visual Studio 2010 jest 10.0.</span><span class="sxs-lookup"><span data-stu-id="6693a-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="6693a-143">Aby uzyskać więcej informacji, zobacz [zgodności projektu programu Visual Studio i VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) na blogu Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="6693a-143">For more information, see [Visual Studio project compatability and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="6693a-144">Określanie profilu publikowania</span><span class="sxs-lookup"><span data-stu-id="6693a-144">Specifying the publish profile</span></span>

<span data-ttu-id="6693a-145">Można określić profil publikowania, według nazwy lub pełną ścieżkę do *.pubxml* plików, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="6693a-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="6693a-146">Metody obsługiwany w przypadku publikowania wiersza polecenia publikowania w sieci Web</span><span class="sxs-lookup"><span data-stu-id="6693a-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="6693a-147">Publikowanie trzy metody są obsługiwane w przypadku publikowania w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="6693a-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="6693a-148">`MSDeploy` -Publikowania za pomocą narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="6693a-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="6693a-149">`Package` -Opublikuj przez utworzenie pakietu Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="6693a-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="6693a-150">Należy zainstalować pakiet niezależnie od polecenia MSBuild, który go utworzył.</span><span class="sxs-lookup"><span data-stu-id="6693a-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="6693a-151">`FileSystem` -Opublikuj przez kopiowanie plików do określonego folderu.</span><span class="sxs-lookup"><span data-stu-id="6693a-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="6693a-152">Określanie konfiguracji kompilacji i platform</span><span class="sxs-lookup"><span data-stu-id="6693a-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="6693a-153">W programie Visual Studio lub w wierszu polecenia można ustawić konfigurację kompilacji i platformy.</span><span class="sxs-lookup"><span data-stu-id="6693a-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="6693a-154">Profile zawierają właściwości, które są nazywane `LastUsedBuildConfiguration` i `LastUsedPlatform`, ale nie może ustawić te właściwości, aby określić, jak projekt jest budowany.</span><span class="sxs-lookup"><span data-stu-id="6693a-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="6693a-155">Aby uzyskać więcej informacji, zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) na blogu Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="6693a-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="6693a-156">Wdrażanie na przemieszczania</span><span class="sxs-lookup"><span data-stu-id="6693a-156">Deploy to staging</span></span>

<span data-ttu-id="6693a-157">Aby wdrożyć na platformie Azure, musi dodać hasło w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="6693a-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="6693a-158">Jeśli hasło jest zapisywane w profilu publikowania w programie Visual Studio, była przechowywana w postaci zaszyfrowanej w Twojej *. pubxml.user* pliku.</span><span class="sxs-lookup"><span data-stu-id="6693a-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="6693a-159">Ten plik nie jest dostępna przez MSBuild po wykonaniu wdrożenia wiersza polecenia, dlatego należy przekazywać hasło w polu Parametry wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="6693a-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="6693a-160">Kopiuj hasło, które należy z *.publishsettings* pliku pobranego wcześniej dla tymczasowej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="6693a-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="6693a-161">Hasło jest wartością `userPWD` atrybutu dla narzędzia Web Deploy `publishProfile` elementu.</span><span class="sxs-lookup"><span data-stu-id="6693a-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Web Deploy hasła](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="6693a-163">Na stronie Start systemu Windows 8, wyszukaj **wiersz polecenia dla VS2012 deweloperów**i kliknij ikonę, aby otworzyć okno wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="6693a-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="6693a-164">(Nie trzeba otworzyć go jako administrator teraz, ponieważ nie jest wdrażana w usługach IIS na komputerze lokalnym).</span><span class="sxs-lookup"><span data-stu-id="6693a-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="6693a-165">Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżka do pliku rozwiązania i hasła przy użyciu hasła:</span><span class="sxs-lookup"><span data-stu-id="6693a-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="6693a-166">Powiadomienie, że ten wiersz polecenia zawiera dodatkowy parametr: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="6693a-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="6693a-167">Podczas zapisywania w tym samouczku `AllowUntrustedCertificate` musi być ustawiona właściwość podczas publikowania na platformie Azure z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="6693a-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="6693a-168">Po zwolnieniu poprawkę dotyczącą tej usterki, nie trzeba tego parametru.</span><span class="sxs-lookup"><span data-stu-id="6693a-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="6693a-169">Otwórz przeglądarkę i przejdź do adresu URL witryny przemieszczania, a następnie kliknij **o** stronę, aby sprawdzić, czy wdrożenie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="6693a-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="6693a-170">Jak przedstawiono wcześniej dla środowiska testowego, być może trzeba utworzyć niektórych studentów, aby wyświetlić statystyki na **o** strony.</span><span class="sxs-lookup"><span data-stu-id="6693a-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="6693a-171">Wdrażanie w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="6693a-171">Deploy to production</span></span>

<span data-ttu-id="6693a-172">Proces wdrażania w środowisku produkcyjnym jest podobny do procesu przemieszczania.</span><span class="sxs-lookup"><span data-stu-id="6693a-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="6693a-173">Kopiuj hasło, które należy z *.publishsettings* pliku pobranego wcześniej dla witryny sieci web w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="6693a-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="6693a-174">Otwórz **wiersz polecenia dla VS2012 deweloperów**.</span><span class="sxs-lookup"><span data-stu-id="6693a-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="6693a-175">Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżka do pliku rozwiązania i hasła przy użyciu hasła:</span><span class="sxs-lookup"><span data-stu-id="6693a-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="6693a-176">Dla lokacji rzeczywistej produkcji, jeśli została również zmianę w bazie danych, czy zwykle kopiowania *aplikacji\_offline.htm* plików do lokacji przed wdrożeniem i usuń go po pomyślnym wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="6693a-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="6693a-177">Otwórz przeglądarkę i przejdź do adresu URL witryny przemieszczania, a następnie kliknij **o** stronę, aby sprawdzić, czy wdrożenie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="6693a-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="6693a-178">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="6693a-178">Summary</span></span>

<span data-ttu-id="6693a-179">Można teraz wdrożyć aktualizacji aplikacji przy użyciu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="6693a-179">You have now deployed an application update by using the command line.</span></span>

![Strona w środowisku testowym — informacje](command-line-deployment/_static/image6.png)

<span data-ttu-id="6693a-181">W następnym samouczku zobaczysz przykładem rozszerzyć sieci web publikowanie potoku.</span><span class="sxs-lookup"><span data-stu-id="6693a-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="6693a-182">Przykład opisano sposób wdrażania plików, które nie znajdują się w projekcie.</span><span class="sxs-lookup"><span data-stu-id="6693a-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6693a-183">[Poprzednie](deploying-a-database-update.md)
> [dalej](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="6693a-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
