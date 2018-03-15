---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: "Tworzenie i uruchamianie wdrożenia polecenie Plik | Dokumentacja firmy Microsoft"
author: jrjlee
description: "W tym temacie opisano sposób tworzenia pliku poleceń, które pozwoli uruchamiać wdrażania przy użyciu plików projektu Microsoft kompilacji Engine (MSBuild) jako pojedynczy krok..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: bc31bf55b29661816e0ca9a50b51b0abc3eb2c98
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="ee76b-103">Tworzenie i uruchamianie pliku poleceń wdrażania</span><span class="sxs-lookup"><span data-stu-id="ee76b-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="ee76b-104">przez [Lewandowski Jason](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="ee76b-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="ee76b-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="ee76b-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="ee76b-106">W tym temacie opisano sposób tworzenia pliku poleceń, które pozwoli uruchamiać wdrażania przy użyciu plików projektu Microsoft kompilacji Engine (MSBuild) jako pojedynczy krok i powtarzalnej proces.</span><span class="sxs-lookup"><span data-stu-id="ee76b-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="ee76b-107">Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie & #x 2014; korzysta z tego samouczka serii [kontaktów Menedżerze](the-contact-manager-solution.md) #x 2014; & rozwiązania do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, systemu Windows Usługi Communication Foundation (WCF), a projekt bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ee76b-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="ee76b-108">Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis procesu kompilacji](understanding-the-build-process.md), w którym jest kontrolowany przez proces kompilacji projektu dwa pliki & #x 2014; jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska.</span><span class="sxs-lookup"><span data-stu-id="ee76b-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="ee76b-109">W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ee76b-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="ee76b-110">Omówienie procesu</span><span class="sxs-lookup"><span data-stu-id="ee76b-110">Process Overview</span></span>

<span data-ttu-id="ee76b-111">W tym temacie nauczysz się, jak utworzyć i uruchomić plik polecenia używane te pliki projektu do wykonywania powtarzalnych wdrożenia do środowiska docelowego.</span><span class="sxs-lookup"><span data-stu-id="ee76b-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="ee76b-112">Zasadniczo pliku polecenia po prostu musi zawierać polecenia programu MSBuild który:</span><span class="sxs-lookup"><span data-stu-id="ee76b-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="ee76b-113">Informuje MSBuild, aby wykonać niezależny od środowiska *Publish.proj* pliku.</span><span class="sxs-lookup"><span data-stu-id="ee76b-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="ee76b-114">Określa, że *Publish.proj* pliku, plik, który zawiera ustawienia projektu określonego środowiska i gdzie można znaleźć go.</span><span class="sxs-lookup"><span data-stu-id="ee76b-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="ee76b-115">Utwórz polecenie MSBuild</span><span class="sxs-lookup"><span data-stu-id="ee76b-115">Create an MSBuild Command</span></span>

<span data-ttu-id="ee76b-116">Zgodnie z opisem w [opis procesu kompilacji](understanding-the-build-process.md), plik projektu określonego środowiska & #x 2014; na przykład *Env Dev.proj*& #x 2014; jest przeznaczona do zaimportowania do niezależny od środowiska *Publish.proj* plik w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="ee76b-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="ee76b-117">Te dwa pliki Podaj ze sobą, kompletny zestaw instrukcje określające sposób tworzenia i wdrażania rozwiązania programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="ee76b-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="ee76b-118">*Publish.proj* plików używa **zaimportować** element, aby zaimportować plik projektu określonego środowiska.</span><span class="sxs-lookup"><span data-stu-id="ee76b-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="ee76b-119">Tak korzystając z programu MSBuild.exe do tworzenia i wdrażania rozwiązania Contact Manager, musisz:</span><span class="sxs-lookup"><span data-stu-id="ee76b-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="ee76b-120">Uruchom MSBuild.exe na *Publish.proj* pliku.</span><span class="sxs-lookup"><span data-stu-id="ee76b-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="ee76b-121">Określ lokalizację pliku projektu określonego środowiska, podając parametr wiersza polecenia o nazwie **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="ee76b-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="ee76b-122">W tym celu polecenia programu MSBuild powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="ee76b-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="ee76b-123">W tym miejscu jest prosty krok, aby przejść do wdrożenia powtarzalne, pojedynczy krok.</span><span class="sxs-lookup"><span data-stu-id="ee76b-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="ee76b-124">Wszystko, co należy zrobić to dodanie polecenia programu MSBuild w pliku .cmd.</span><span class="sxs-lookup"><span data-stu-id="ee76b-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="ee76b-125">W rozwiązaniu Menedżera skontaktuj się z folderu publikowania zawiera plik o nazwie *Dev.cmd publikowania* wykonuje dokładnie to.</span><span class="sxs-lookup"><span data-stu-id="ee76b-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="ee76b-126">**/Fl** przełącznik powoduje, że program MSBuild, aby utworzyć plik dziennika o nazwie *msbuild.log* w katalogu roboczym, w którym wywołano MSBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="ee76b-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="ee76b-127">Aby wdrożyć lub ponownego wdrożenia rozwiązania menedżera kontaktu, wszystko co należy zrobić Uruchom *Dev.cmd publikowania* pliku.</span><span class="sxs-lookup"><span data-stu-id="ee76b-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="ee76b-128">Po uruchomieniu pliku będzie MSBuild:</span><span class="sxs-lookup"><span data-stu-id="ee76b-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="ee76b-129">Tworzenie wszystkich projektów w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="ee76b-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="ee76b-130">Generowanie pakietów sieci web można wdrożyć dla projektów aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ee76b-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="ee76b-131">Generuj pliki .dbschema i .deploymanifest dla projektów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ee76b-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="ee76b-132">Wdrażanie pakietów sieci web na serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="ee76b-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="ee76b-133">Wdrażania bazy danych na serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ee76b-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="ee76b-134">Uruchom wdrożenie</span><span class="sxs-lookup"><span data-stu-id="ee76b-134">Run the Deployment</span></span>

<span data-ttu-id="ee76b-135">Po utworzeniu pliku poleceń dla środowiska docelowego, należy wykonać całego wdrożenia po prostu, uruchamiając plik.</span><span class="sxs-lookup"><span data-stu-id="ee76b-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="ee76b-136">**Aby wdrożyć rozwiązanie kontaktów Menedżerze do danego środowiska testowego**</span><span class="sxs-lookup"><span data-stu-id="ee76b-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="ee76b-137">Na stacji roboczej dewelopera Otwórz Eksploratora Windows, a następnie przejdź do lokalizacji *Dev.cmd publikowania* pliku.</span><span class="sxs-lookup"><span data-stu-id="ee76b-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="ee76b-138">Kliknij dwukrotnie plik, aby go uruchomić.</span><span class="sxs-lookup"><span data-stu-id="ee76b-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="ee76b-139">Jeśli **Otwórz plik — ostrzeżenie o zabezpieczeniach** zostanie wyświetlone okno dialogowe, kliknij przycisk **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="ee76b-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="ee76b-140">Jeśli Twoje ustawienia konfiguracji i serwery testu są skonfigurowane poprawnie, okna wiersza polecenia wyświetli **kompilacji zakończyło się pomyślnie** podczas MSBuild zakończył przetwarzanie plików projektu.</span><span class="sxs-lookup"><span data-stu-id="ee76b-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="ee76b-141">Jeśli jest to w tym środowisku wdrożono rozwiązanie po raz pierwszy, musisz dodać konto komputera serwera sieci web testów, aby **db\_datawriter** i **db\_datareader**ról na **ContactManager** bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ee76b-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="ee76b-142">Ta procedura jest opisana w [Konfiguracja serwera bazy danych dla wdrożenia publikowania w sieci Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="ee76b-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee76b-143">Należy przypisać te uprawnienia, podczas tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ee76b-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="ee76b-144">Domyślnie proces kompilacji nie zostanie ponownie bazy danych dla każdego wdrożenia & #x 2014; zamiast tego porównuje istniejącą bazę danych do najnowszej schematu i wprowadź zmiany wymagane.</span><span class="sxs-lookup"><span data-stu-id="ee76b-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="ee76b-145">W związku z tym należy tylko należy zamapować te role bazy danych podczas pierwszego wdrażania rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="ee76b-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="ee76b-146">Otwórz program Internet Explorer i przejdź do adresu URL aplikacji, skontaktuj się z Menedżera (na przykład `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="ee76b-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="ee76b-147">Sprawdź, czy aplikacja działa zgodnie z oczekiwaniami i możliwe będzie dodawanie kontaktów.</span><span class="sxs-lookup"><span data-stu-id="ee76b-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="ee76b-148">Wniosek</span><span class="sxs-lookup"><span data-stu-id="ee76b-148">Conclusion</span></span>

<span data-ttu-id="ee76b-149">Tworzenie pliku poleceń instrukcjami MSBuild zapewnia szybki i łatwy sposób tworzenia i wdrażania rozwiązania wielu projektów w środowisku określonego miejsca docelowego.</span><span class="sxs-lookup"><span data-stu-id="ee76b-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="ee76b-150">Jeśli potrzebujesz wielokrotnie wdrażać rozwiązania w wielu środowiskach, w miejsce docelowe, można utworzyć wiele plików poleceń.</span><span class="sxs-lookup"><span data-stu-id="ee76b-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="ee76b-151">W każdym pliku polecenia polecenie MSBuild utworzy tego samego pliku uniwersalnych projektów, ale będzie określać plik inny projekt określonego środowiska.</span><span class="sxs-lookup"><span data-stu-id="ee76b-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="ee76b-152">Na przykład użyć pliku polecenia publikować deweloperem lub środowiska testowego może zawierać tego polecenia programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="ee76b-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="ee76b-153">Plik polecenia do publikowania w środowisku przemieszczania mogą zawierać tego polecenia programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="ee76b-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="ee76b-154">Aby uzyskać wskazówki dotyczące sposobu dostosowywania pliki projektu określonego środowiska dla środowiska serwera, zobacz [konfigurowania właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="ee76b-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="ee76b-155">Proces kompilacji dla każdego środowiska można również dostosować przez zastępowanie właściwości albo ustawienie różnych przełączników inne polecenia programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="ee76b-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="ee76b-156">Aby uzyskać więcej informacji, zobacz [informacje w wierszu polecenia programu MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="ee76b-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ee76b-157">[Poprzednie](deploying-database-projects.md)
[dalej](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="ee76b-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
