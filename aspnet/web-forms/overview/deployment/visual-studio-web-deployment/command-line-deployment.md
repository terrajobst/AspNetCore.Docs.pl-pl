---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: wdrażanie z wiersza polecenia | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5673a4733257fae88fb66a3da43dfceb98c3b37a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390877"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="2c3c0-103">Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: wdrażanie z wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="2c3c0-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>
====================
<span data-ttu-id="2c3c0-104">przez [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2c3c0-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="2c3c0-105">Pobieranie projektu startowego</span><span class="sxs-lookup"><span data-stu-id="2c3c0-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="2c3c0-106">W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="2c3c0-107">Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2c3c0-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="2c3c0-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="2c3c0-108">Overview</span></span>

<span data-ttu-id="2c3c0-109">W tym samouczku przedstawiono sposób wywołania sieci web programu Visual Studio publikowania potoku z poziomu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="2c3c0-110">Jest to przydatne w scenariuszach, w którym chcesz [zautomatyzować proces wdrażania](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) zamiast to zrobić ręcznie w programie Visual Studio, zwykle za pomocą [systemu kontroli wersji kodu źródła](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="2c3c0-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="2c3c0-111">Wprowadź zmiany do wdrożenia</span><span class="sxs-lookup"><span data-stu-id="2c3c0-111">Make a change to deploy</span></span>

<span data-ttu-id="2c3c0-112">Obecnie na stronie Informacje przedstawia kod szablonu.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-112">Currently the About page displays the template code.</span></span>

![Informacje o stronie przy użyciu kodu szablonu](command-line-deployment/_static/image1.png)

<span data-ttu-id="2c3c0-114">Zastąpisz, z kodem, który wyświetla podsumowanie rejestracji dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="2c3c0-115">Otwórz *About.aspx* strony, Usuń wszystkie znaczniki wewnątrz `MainContent` `Content` elementu i Wstaw następujący kod w jego miejsce:</span><span class="sxs-lookup"><span data-stu-id="2c3c0-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="2c3c0-116">Uruchom projekt i wybierz **o** strony.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-116">Run the project and select the **About** page.</span></span>

![Informacje o stronie](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="2c3c0-118">Wdrażanie do testu przy użyciu wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="2c3c0-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="2c3c0-119">Nie będzie wdrażanie inną zmianę w bazie danych, tak wyłączenie dbDacFx wdrożenia bazy danych dla bazy danych aspnet ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="2c3c0-120">Otwórz **publikowanie w sieci Web** kreatora i we wszystkich trzech profilów, wyczyść publikowania **Aktualizuj bazę danych** pole wyboru na **ustawienia** kartę.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="2c3c0-121">Na stronie początkowy systemu Windows 8, wyszukaj **wiersz polecenia programisty dla VS2012**.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="2c3c0-122">Kliknij prawym przyciskiem myszy ikonę **wiersz polecenia programisty dla VS2012** i kliknij przycisk **Uruchom jako administrator**.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="2c3c0-123">Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania przy użyciu ścieżki do pliku rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="2c3c0-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="2c3c0-124">Program MSBuild kompiluje rozwiązanie i wdraża ją do środowiska testowego.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Dane wyjściowe wiersza polecenia](command-line-deployment/_static/image3.png)

<span data-ttu-id="2c3c0-126">Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, następnie kliknij przycisk **o** stronę, aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="2c3c0-127">Jeśli nie utworzono żadnych studentów w teście, zobaczysz pusta strona w obszarze **statystyki treści uczniów** nagłówka.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="2c3c0-128">Przejdź do **studentów** kliknij **dodać uczniów**i Dodaj pewne studenci, a następnie wróć do **o** stronę, aby wyświetlić statystyki dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Informacje o stronie w środowisku testowym](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="2c3c0-130">Opcje klucza wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="2c3c0-130">Key command line options</span></span>

<span data-ttu-id="2c3c0-131">Polecenie, które zostały wprowadzone, przekazywane do MSBuild ścieżka pliku rozwiązania i dwie właściwości:</span><span class="sxs-lookup"><span data-stu-id="2c3c0-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="2c3c0-132">Wdrażanie rozwiązania i wdrażania poszczególnych projektów</span><span class="sxs-lookup"><span data-stu-id="2c3c0-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="2c3c0-133">Określanie pliku rozwiązania powoduje, że wszystkie projekty w rozwiązaniu, które ma zostać utworzony.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="2c3c0-134">Jeśli masz wiele projektów sieci web w rozwiązaniu, mają zastosowanie następujące zachowanie programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="2c3c0-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="2c3c0-135">Właściwości, należy określić w wierszu polecenia, które są przekazywane do każdego projektu.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="2c3c0-136">W związku z tym każdy projekt sieci web musi mieć profil publikowania o podanej nazwie.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="2c3c0-137">Jeśli określisz `/p:PublishProfile=Test`, każdy projekt sieci web musi mieć profil publikowania o nazwie *testu*.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="2c3c0-138">Jeden projekt może opublikowanie, gdy inny nawet nie da się skompilować.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="2c3c0-139">Aby uzyskać więcej informacji, zobacz wątek w witrynie stackoverflow [MSBuild nie powiedzie się z dwóch pakietów](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="2c3c0-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="2c3c0-140">Jeśli określisz pojedynczego projektu zamiast rozwiązania, należy dodać parametr określający używaną wersję programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="2c3c0-141">Jeśli używasz programu Visual Studio 2012 w wierszu polecenia będzie podobny do poniższego przykładu:</span><span class="sxs-lookup"><span data-stu-id="2c3c0-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="2c3c0-142">Numer wersji dla programu Visual Studio 2010 jest 10.0.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="2c3c0-143">Aby uzyskać więcej informacji, zobacz [ze zgodnością projektów programu Visual Studio i VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) na blogu Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-143">For more information, see [Visual Studio project compatability and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="2c3c0-144">Określanie profilu publikowania</span><span class="sxs-lookup"><span data-stu-id="2c3c0-144">Specifying the publish profile</span></span>

<span data-ttu-id="2c3c0-145">Można określić profil publikowania, według nazwy lub pełną ścieżkę do *.pubxml* pliku, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="2c3c0-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="2c3c0-146">W sieci Web publikowanie metody obsługiwany w przypadku publikowania wiersza polecenia</span><span class="sxs-lookup"><span data-stu-id="2c3c0-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="2c3c0-147">Publikowanie trzy metody są obsługiwane w przypadku publikowania w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="2c3c0-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="2c3c0-148">`MSDeploy` -Publikowanie za pomocą narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="2c3c0-149">`Package` -Opublikować, tworząc pakiet wdrażania sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="2c3c0-150">Musisz zainstalować pakiet, niezależnie od polecenia programu MSBuild, które tworzy go.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="2c3c0-151">`FileSystem` -Opublikować kopiować pliki do określonego folderu.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="2c3c0-152">Określenie konfiguracji kompilacji i platformy</span><span class="sxs-lookup"><span data-stu-id="2c3c0-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="2c3c0-153">W programie Visual Studio lub w wierszu polecenia można ustawić konfigurację kompilacji i platformy.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="2c3c0-154">Profile publikowania zawierają właściwości, które są nazywane `LastUsedBuildConfiguration` i `LastUsedPlatform`, ale nie ustawisz tych właściwości w celu określenia, jak projekt jest kompilowany.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="2c3c0-155">Aby uzyskać więcej informacji, zobacz [MSBuild: sposób ustawiania właściwości konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) na blogu Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="2c3c0-156">Wdrażanie w środowisku przejściowym</span><span class="sxs-lookup"><span data-stu-id="2c3c0-156">Deploy to staging</span></span>

<span data-ttu-id="2c3c0-157">Aby wdrożyć na platformie Azure, należy dodać hasło w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="2c3c0-158">Jeśli hasło jest zapisywane w profilu publikowania w programie Visual Studio, została zapisana w zaszyfrowanym formacie w swojej *. pubxml.user* pliku.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="2c3c0-159">Ten plik nie jest dostępny przez program MSBuild, po wykonaniu wdrażanie z wiersza polecenia, więc trzeba przekazać hasło parametr wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="2c3c0-160">Skopiuj hasło, które wymagają od *.publishsettings* pliku, który został pobrany wcześniej dla tymczasowej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="2c3c0-161">Hasło jest wartością `userPWD` atrybutu dla narzędzia Web Deploy `publishProfile` elementu.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Web Deploy hasła](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="2c3c0-163">Na stronie początkowy systemu Windows 8, wyszukaj **wiersz polecenia programisty dla VS2012**, a następnie kliknij ikonę, aby otworzyć wiersz polecenia.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="2c3c0-164">(Nie trzeba otworzyć go jako administratora tej chwili, ponieważ nie są wdrażanie w usługach IIS, na komputerze lokalnym).</span><span class="sxs-lookup"><span data-stu-id="2c3c0-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="2c3c0-165">Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania przy użyciu ścieżki do pliku rozwiązania i hasło, za pomocą hasła:</span><span class="sxs-lookup"><span data-stu-id="2c3c0-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="2c3c0-166">Zwróć uwagę, że ten wiersz polecenia zawiera dodatkowy parametr: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="2c3c0-167">Podczas zapisywania w tym samouczku `AllowUntrustedCertificate` podczas publikowania na platformie Azure z poziomu wiersza polecenia, należy ustawić właściwość.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="2c3c0-168">Po zwolnieniu poprawkę dotyczącą tej usterki nie wymaga tego parametru.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="2c3c0-169">Otwórz przeglądarkę i przejdź do adresu URL witryny tymczasowej, a następnie kliknij **o** stronę, aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="2c3c0-170">Jak przedstawiono wcześniej dla środowiska testowego, być może trzeba utworzyć Niektórzy studenci, aby wyświetlić statystyki na **o** strony.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="2c3c0-171">Wdrażanie w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="2c3c0-171">Deploy to production</span></span>

<span data-ttu-id="2c3c0-172">Proces wdrażania w środowisku produkcyjnym jest podobny do procesu przejściowego.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="2c3c0-173">Skopiuj hasło, które wymagają od *.publishsettings* pliku, który został pobrany wcześniej dla witryny sieci web w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="2c3c0-174">Otwórz **wiersz polecenia programisty dla VS2012**.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="2c3c0-175">Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania przy użyciu ścieżki do pliku rozwiązania i hasło, za pomocą hasła:</span><span class="sxs-lookup"><span data-stu-id="2c3c0-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="2c3c0-176">Dla witryny rzeczywistej produkcji, jeśli została również zmianę w bazie danych, będzie zazwyczaj kopiujesz *aplikacji\_offline.htm* plików do lokacji, przed przystąpieniem do wdrożenia i usuniesz go po pomyślnym wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="2c3c0-177">Otwórz przeglądarkę i przejdź do adresu URL witryny tymczasowej, a następnie kliknij **o** stronę, aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="2c3c0-178">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="2c3c0-178">Summary</span></span>

<span data-ttu-id="2c3c0-179">Masz teraz wdrożyć aktualizację aplikacji przy użyciu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-179">You have now deployed an application update by using the command line.</span></span>

![Informacje o stronie w środowisku testowym](command-line-deployment/_static/image6.png)

<span data-ttu-id="2c3c0-181">W następnym samouczku, zostanie wyświetlony przykładem sposobu rozszerzenia sieci web potoku publikowania.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="2c3c0-182">Przykład będzie pokazują, jak wdrożyć pliki, które nie są uwzględnione w projekcie.</span><span class="sxs-lookup"><span data-stu-id="2c3c0-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2c3c0-183">[Poprzednie](deploying-a-database-update.md)
> [dalej](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="2c3c0-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
