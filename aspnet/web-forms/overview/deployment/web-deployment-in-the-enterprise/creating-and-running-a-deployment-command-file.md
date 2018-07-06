---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Tworzenie i uruchamianie wdrożenia plik poleceń | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób tworzenia pliku poleceń, które pozwoli uruchamiać wdrażania przy użyciu aparatu Microsoft Build Engine (MSBuild), pliki projektu w jednym kroku ponownie...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 6b2a75e0740f648a3a6e4f39c00bd30609325728
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830305"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="abce0-103">Tworzenie i uruchamianie pliku poleceń wdrażania</span><span class="sxs-lookup"><span data-stu-id="abce0-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="abce0-104">przez [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="abce0-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="abce0-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="abce0-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="abce0-106">W tym temacie opisano sposób tworzenia pliku polecenia, które pozwoli uruchamiać wdrażania przy użyciu aparatu Microsoft Build Engine (MSBuild), pliki projektu jako pojedynczy krok, powtarzalnego procesu.</span><span class="sxs-lookup"><span data-stu-id="abce0-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="abce0-107">Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [Contact Manager](the-contact-manager-solution.md) rozwiązania&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.</span><span class="sxs-lookup"><span data-stu-id="abce0-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="abce0-108">Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie procesu kompilacji](understanding-the-build-process.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania.</span><span class="sxs-lookup"><span data-stu-id="abce0-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="abce0-109">W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="abce0-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="abce0-110">Omówienie procesu</span><span class="sxs-lookup"><span data-stu-id="abce0-110">Process Overview</span></span>

<span data-ttu-id="abce0-111">W tym temacie dowiesz się, jak tworzenie i uruchamianie pliku poleceń, który używa tych plików projektu do wykonywania powtarzalne wdrożenia w środowisku docelowym.</span><span class="sxs-lookup"><span data-stu-id="abce0-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="abce0-112">Zasadniczo pliku poleceń po prostu musi zawierać polecenia MSBuild który:</span><span class="sxs-lookup"><span data-stu-id="abce0-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="abce0-113">Informuje program MSBuild, aby wykonać niezależnie od środowiska *Publish.proj* pliku.</span><span class="sxs-lookup"><span data-stu-id="abce0-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="abce0-114">Informuje *Publish.proj* pliku, plik, który zawiera ustawienia specyficzne dla środowiska projektu i gdzie można znaleźć go.</span><span class="sxs-lookup"><span data-stu-id="abce0-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="abce0-115">Tworzenie polecenia programu MSBuild</span><span class="sxs-lookup"><span data-stu-id="abce0-115">Create an MSBuild Command</span></span>

<span data-ttu-id="abce0-116">Zgodnie z opisem w [objaśnienie procesu kompilacji](understanding-the-build-process.md), plik projektu o specyficznych dla środowiska&#x2014;na przykład *Env Dev.proj*&#x2014;zaprojektowano w celu zaimportowania do niezależny od środowiska *Publish.proj* plik w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="abce0-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="abce0-117">Razem te dwa pliki zapewniają kompletny zestaw instrukcji, które kazać programowi MSBuild, jak tworzyć i wdrażać rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="abce0-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="abce0-118">*Publish.proj* plików używa **zaimportować** elementu, aby zaimportować plik projektu specyficznego dla danego środowiska.</span><span class="sxs-lookup"><span data-stu-id="abce0-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="abce0-119">Jako takie korzystając z MSBuild.exe do tworzenia i wdrażania rozwiązania Contact Manager, należy:</span><span class="sxs-lookup"><span data-stu-id="abce0-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="abce0-120">Uruchamianie MSBuild.exe na *Publish.proj* pliku.</span><span class="sxs-lookup"><span data-stu-id="abce0-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="abce0-121">Określ lokalizację pliku projektu specyficznymi dla środowiska, podając parametr wiersza polecenia o nazwie **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="abce0-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="abce0-122">Aby to zrobić, polecenia MSBuild powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="abce0-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="abce0-123">W tym miejscu jest prostym kroku, aby przejść do wdrożenia powtarzalne, pojedynczy krok.</span><span class="sxs-lookup"><span data-stu-id="abce0-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="abce0-124">Wszystko, co należy zrobić, jest dodanie polecenia MSBuild do pliku .cmd.</span><span class="sxs-lookup"><span data-stu-id="abce0-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="abce0-125">W przypadku rozwiązania Contact Manager folderu publikowania zawiera plik o nazwie *Dev.cmd Publikuj* dokładnie ten wykonujący.</span><span class="sxs-lookup"><span data-stu-id="abce0-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="abce0-126">**/Fl** przełącznik powoduje, że program MSBuild utworzy plik dziennika o nazwie *msbuild.log* w katalogu roboczym, w której wywołano MSBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="abce0-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="abce0-127">W przypadku wdrażania lub ponownego wdrożenia rozwiązania Contact Manager, wszystko, czego potrzebujesz w celu uruchomienia *Dev.cmd Publikuj* pliku.</span><span class="sxs-lookup"><span data-stu-id="abce0-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="abce0-128">Po uruchomieniu pliku MSBuild wykonują następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="abce0-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="abce0-129">Kompiluj wszystkie projekty w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="abce0-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="abce0-130">Generuj pakiety do wdrożenia sieci web dla projektów aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="abce0-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="abce0-131">Generowanie plików .dbschema i .deploymanifest dla projektów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="abce0-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="abce0-132">Wdrażanie pakietów internetowych do serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="abce0-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="abce0-133">Wdrażanie bazy danych na serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="abce0-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="abce0-134">Uruchom wdrożenie</span><span class="sxs-lookup"><span data-stu-id="abce0-134">Run the Deployment</span></span>

<span data-ttu-id="abce0-135">Po utworzeniu pliku poleceń w środowisku docelowym, należy wykonać całe wdrożenie, po prostu uruchamiając plik.</span><span class="sxs-lookup"><span data-stu-id="abce0-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="abce0-136">**Aby wdrożyć to rozwiązanie Contact Manager do danego środowiska testowego**</span><span class="sxs-lookup"><span data-stu-id="abce0-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="abce0-137">Na stacji roboczej dewelopera, otwórz Eksploratora Windows, a następnie przejdź do lokalizacji *Dev.cmd Publikuj* pliku.</span><span class="sxs-lookup"><span data-stu-id="abce0-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="abce0-138">Kliknij dwukrotnie plik, aby go uruchomić.</span><span class="sxs-lookup"><span data-stu-id="abce0-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="abce0-139">Jeśli **Otwórz plik — ostrzeżenie o zabezpieczeniach** pojawi się okno dialogowe, kliknij przycisk **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="abce0-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="abce0-140">Czy ustawienia konfiguracji i serwerów testu zostały skonfigurowane poprawnie, w oknie wiersza polecenia wyświetli **kompilacja powiodła się** komunikatu, gdy program MSBuild zakończył przetwarzanie plików projektu.</span><span class="sxs-lookup"><span data-stu-id="abce0-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="abce0-141">Jeśli po raz pierwszy, udało Ci się wdrożyć rozwiązanie do tego środowiska, musisz dodać konto komputera serwera sieci web test, aby **db\_datawriter** i **db\_datareader**ról na **ContactManager** bazy danych.</span><span class="sxs-lookup"><span data-stu-id="abce0-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="abce0-142">Ta procedura jest opisana w [Konfigurowanie serwera bazy danych dla wdrożenia publikowania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="abce0-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="abce0-143">Musisz przypisać te uprawnienia, podczas tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="abce0-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="abce0-144">Domyślnie proces kompilacji nie będzie ponownie utworzyć bazy danych przy każdym wdrożeniu&#x2014;zamiast tego porównuje istniejącą bazę danych do najnowszej schematu i zmiany tylko wymagane.</span><span class="sxs-lookup"><span data-stu-id="abce0-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="abce0-145">W rezultacie tylko należy zamapować te role bazy danych wdrażania rozwiązania po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="abce0-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="abce0-146">Otwórz program Internet Explorer i przejdź pod adres URL aplikacji Contact Manager (na przykład `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="abce0-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="abce0-147">Sprawdź, czy aplikacja działa zgodnie z oczekiwaniami, i możliwe będzie dodawanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="abce0-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="abce0-148">Wniosek</span><span class="sxs-lookup"><span data-stu-id="abce0-148">Conclusion</span></span>

<span data-ttu-id="abce0-149">Tworzenie pliku poleceń, zawierający instrukcje MSBuild oferuje szybki i łatwy sposób tworzenia i wdrażania rozwiązania wielu projektów w środowisku określonego miejsca docelowego.</span><span class="sxs-lookup"><span data-stu-id="abce0-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="abce0-150">Jeśli zachodzi potrzeba wielokrotnie wdrażać rozwiązania dla wielu środowisk docelowej, można utworzyć wiele plików poleceń.</span><span class="sxs-lookup"><span data-stu-id="abce0-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="abce0-151">W każdym pliku polecenia polecenie MSBuild utworzy tego samego pliku projektu w wersji uniwersalnej, ale będą wpisywać, plik inny projekt specyficznymi dla środowiska.</span><span class="sxs-lookup"><span data-stu-id="abce0-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="abce0-152">Na przykład pliku polecenia, aby opublikować z deweloperem lub środowiska testowego może zawierać tego polecenia programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="abce0-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="abce0-153">Plik poleceń do publikowania w środowisku przejściowym może zawierać tego polecenia programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="abce0-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="abce0-154">Aby uzyskać wskazówki dotyczące dostosowywania pliki projektu specyficznego dla środowiska dla środowisk serwera, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="abce0-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="abce0-155">Zastępowanie właściwości lub ustawiając różnych innymi przełącznikami w poleceniu programu MSBuild, można również dostosować proces kompilacji dla każdego środowiska.</span><span class="sxs-lookup"><span data-stu-id="abce0-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="abce0-156">Aby uzyskać więcej informacji, zobacz [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="abce0-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="abce0-157">[Poprzednie](deploying-database-projects.md)
> [dalej](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="abce0-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
