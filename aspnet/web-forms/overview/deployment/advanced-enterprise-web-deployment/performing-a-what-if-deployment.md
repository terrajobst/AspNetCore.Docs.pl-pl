---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Wykonywanie analizy warunkowej, jeśli wdrożenie | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób przeprowadzania "co jeśli" (lub symulowanych) wdrożenia przy użyciu narzędzia do wdrażania sieci Web usług Internet Information Services (IIS) (Web Deploy) i V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: f50053c3b231a1d174df51ce7aa4c6013192bca8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378102"
---
<a name="performing-a-what-if-deployment"></a><span data-ttu-id="a3f8d-103">Wykonywanie wdrożenia "What If"</span><span class="sxs-lookup"><span data-stu-id="a3f8d-103">Performing a "What If" Deployment</span></span>
====================
<span data-ttu-id="a3f8d-104">przez [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="a3f8d-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="a3f8d-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="a3f8d-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="a3f8d-106">W tym temacie opisano sposób przeprowadzania "what if" (lub symulowanych) przy użyciu narzędzia do wdrażania sieci Web usług Internet Information Services (IIS) (Web Deploy) i VSDBCMD wdrożeń.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-106">This topic describes how to perform "what if" (or simulated) deployments using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) and VSDBCMD.</span></span> <span data-ttu-id="a3f8d-107">Dzięki temu można ustalenie efektów logiki wdrożenia w środowisku określonego celu, aby rzeczywiście wdrożyć aplikację.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-107">This lets you determine the effects of your deployment logic on a particular target environment before you actually deploy your application.</span></span>


<span data-ttu-id="a3f8d-108">Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="a3f8d-109">Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w której procesu kompilacji i wdrażania jest kontrolowany przez dwa pliki projektu&#x2014;jeden zawierającej instrukcje kompilacji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="a3f8d-110">W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="performing-a-what-if-deployment-for-web-packages"></a><span data-ttu-id="a3f8d-111">Wykonywanie wdrożenia "What If" dla pakietów internetowych</span><span class="sxs-lookup"><span data-stu-id="a3f8d-111">Performing a "What If" Deployment for Web Packages</span></span>

<span data-ttu-id="a3f8d-112">Narzędzie Web Deploy zawiera funkcje, które umożliwia wykonywanie wdrożeń w "what if" (lub wersja próbna) trybu.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-112">Web Deploy includes functionality that lets you perform deployments in "what if" (or trial) mode.</span></span> <span data-ttu-id="a3f8d-113">Podczas wdrażania artefaktów w trybie "what if", narzędzie Web Deploy, generuje plik dziennika, tak, jakby były wykonywane wdrożenia, ale faktycznie nie zmienia żadnych na serwerze docelowym.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-113">When you deploy artifacts in "what if" mode, Web Deploy generates a log file as if you had performed the deployment, but it doesn't actually change anything on the destination server.</span></span> <span data-ttu-id="a3f8d-114">Przeglądanie plików dziennika, może pomóc Ci zrozumieć, jaki wpływ wdrożenia będzie mieć na serwerze docelowym, w szczególności:</span><span class="sxs-lookup"><span data-stu-id="a3f8d-114">Reviewing the log file can help you to understand what impact your deployment will have on the destination server, in particular:</span></span>

- <span data-ttu-id="a3f8d-115">Co będzie poproś o dodanie Cię.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-115">What will get added.</span></span>
- <span data-ttu-id="a3f8d-116">Jakie zostaną zaktualizowani.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-116">What will get updated.</span></span>
- <span data-ttu-id="a3f8d-117">Co zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-117">What will get deleted.</span></span>

<span data-ttu-id="a3f8d-118">Ponieważ wdrożenia "what if" faktycznie nie zmienia żadnych na serwerze docelowym, co zawsze nie jest przewidzieć wdrożenia, zostanie wykonane pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-118">Because a "what if" deployment doesn't actually change anything on the destination server, what it can't always do is predict whether a deployment will succeed.</span></span>

<span data-ttu-id="a3f8d-119">Zgodnie z opisem w [wdrażanie pakietów internetowych](../web-deployment-in-the-enterprise/deploying-web-packages.md), można wdrożyć pakietów sieci web za pomocą narzędzia Web Deploy na dwa sposoby&#x2014;za pomocą narzędzia wiersza polecenia MSDeploy.exe, bezpośrednio lub przez uruchomienie *. pliku deploy.cmd* pliku generuje procesu kompilacji.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-119">As described in [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md), you can deploy web packages using Web Deploy in two ways&#x2014;by using the MSDeploy.exe command-line utility directly or by running the *.deploy.cmd* file that the build process generates.</span></span>

<span data-ttu-id="a3f8d-120">Jeśli używasz programu MSDeploy.exe bezpośrednio, możesz uruchamiać wdrożenia "what if", dodając **whatif** flagi do swojej dyspozycji.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-120">If you're using MSDeploy.exe directly, you can run a "what if" deployment by adding the **–whatif** flag to your command.</span></span> <span data-ttu-id="a3f8d-121">Na przykład aby ocenić, co się stanie, jeśli wdrożono pakiet ContactManager.Mvc.zip w środowisku przejściowym, polecenie MSDeploy powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="a3f8d-121">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package to a staging environment, the MSDeploy command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


<span data-ttu-id="a3f8d-122">Gdy jesteś zadowolony z wyników wdrożenia "what if", możesz usunąć **whatif** flagi do uruchamiania na żywo wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-122">When you're satisfied with the results of your "what if" deployment, you can remove the **–whatif** flag to run a live deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="a3f8d-123">Aby uzyskać więcej informacji o opcjach wiersza polecenia programu MSDeploy.exe, zobacz [ustawienia operacji wdrażania w sieci Web](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="a3f8d-123">For more information on command-line options for MSDeploy.exe, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span>


<span data-ttu-id="a3f8d-124">Jeśli używasz *. pliku deploy.cmd* pliku, możesz uruchomić wdrożenia "what if", umieszczając **/t** flaga Flaga (tryb wersji próbnej) zamiast **/y** Flaga ("yes" lub tryb aktualizacji) polecenie.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-124">If you're using the *.deploy.cmd* file, you can run a "what if" deployment by including the **/t** flag (trial mode) flag instead of the **/y** flag ("yes," or update mode) in your command.</span></span> <span data-ttu-id="a3f8d-125">Na przykład, można obliczyć, co się stanie, jeśli wdrożono pakiet ContactManager.Mvc.zip, uruchamiając *. pliku deploy.cmd* pliku polecenia powinny wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="a3f8d-125">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package by running the *.deploy.cmd* file, your command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


<span data-ttu-id="a3f8d-126">Gdy jesteś zadowolony z wyników wdrożenia "tryb wersji próbnej", możesz zastąpić **/t** flaga z **/y** flagi do uruchamiania na żywo wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="a3f8d-126">When you're satisfied with the results of your "trial mode" deployment, you can replace the **/t** flag with a **/y** flag to run a live deployment:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="a3f8d-127">Aby uzyskać więcej informacji na temat opcji wiersza polecenia dla *. pliku deploy.cmd* plików, zobacz [porady: Instalowanie wdrażania pakietu przy użyciu pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="a3f8d-127">For more information on command-line options for *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="a3f8d-128">Jeśli uruchamiasz *. pliku deploy.cmd* plików bez określania flagi, wiersza polecenia wyświetli listę dostępnych flag.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-128">If you run the *.deploy.cmd* file without specifying any flags, the command prompt will display a list of available flags.</span></span>


## <a name="performing-a-what-if-deployment-for-databases"></a><span data-ttu-id="a3f8d-129">Wykonywanie wdrożenia "What If" dla bazy danych</span><span class="sxs-lookup"><span data-stu-id="a3f8d-129">Performing a "What If" Deployment for Databases</span></span>

<span data-ttu-id="a3f8d-130">W tej sekcji założono, że używasz narzędzia VSDBCMD przeprowadzić wdrożenie przyrostowe, oparte na schemacie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-130">This section assumes that you're using the VSDBCMD utility to perform incremental, schema-based database deployment.</span></span> <span data-ttu-id="a3f8d-131">To podejście jest opisane bardziej szczegółowo w [wdrażanie projektów baz danych](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span><span class="sxs-lookup"><span data-stu-id="a3f8d-131">This approach is described in more detail in [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="a3f8d-132">Zaleca się, że możesz zapoznać się z tego tematu przed zastosowaniem pojęcia opisane w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-132">We recommend that you familiarize yourself with this topic before you apply the concepts described here.</span></span>

<span data-ttu-id="a3f8d-133">Kiedy używasz VSDBCMD w **Wdróż** trybu, można użyć **/dd** (lub **/DeployToDatabase**) flaga kontroli VSDBCMD powoduje faktyczne wdrożenie bazy danych lub po prostu generuje skrypt wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-133">When you use VSDBCMD in **Deploy** mode, you can use the **/dd** (or **/DeployToDatabase**) flag to control whether VSDBCMD actually deploys the database or just generates a deployment script.</span></span> <span data-ttu-id="a3f8d-134">Jeśli wdrażasz pliku .dbschema jest to zachowanie:</span><span class="sxs-lookup"><span data-stu-id="a3f8d-134">If you're deploying a .dbschema file, this is the behavior:</span></span>

- <span data-ttu-id="a3f8d-135">Jeśli określisz **/dd+** lub **/dd**, VSDBCMD wygeneruje skryptu wdrażania i wdrażanie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-135">If you specify **/dd+** or **/dd**, VSDBCMD will generate a deployment script and deploy the database.</span></span>
- <span data-ttu-id="a3f8d-136">Jeśli określisz **/dd-** lub pominięta, VSDBCMD spowoduje wygenerowanie skryptu wdrażania.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-136">If you specify **/dd-** or omit the switch, VSDBCMD will generate a deployment script only.</span></span>

> [!NOTE]
> <span data-ttu-id="a3f8d-137">Jeśli wdrażasz .deploymanifest pliku, a nie plikiem .dbschema zachowanie **/dd** przełącznik jest o wiele bardziej skomplikowane.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-137">If you're deploying a .deploymanifest file rather than a .dbschema file, the behavior of the **/dd** switch is a lot more complicated.</span></span> <span data-ttu-id="a3f8d-138">Zasadniczo VSDBCMD zignoruje wartość **/dd** przełącznika, jeśli plik .deploymanifest zawiera **DeployToDatabase** element z wartością **True**.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-138">Essentially, VSDBCMD will ignore the value of the **/dd** switch if the .deploymanifest file includes a **DeployToDatabase** element with a value of **True**.</span></span> <span data-ttu-id="a3f8d-139">[Wdrażanie projektów baz danych](../web-deployment-in-the-enterprise/deploying-database-projects.md) opisano to zachowanie w całości.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-139">[Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md) describes this behavior in full.</span></span>


<span data-ttu-id="a3f8d-140">Na przykład, można wygenerować skryptu wdrażania dla **ContactManager** bazy danych bez faktycznego wdrażania bazy danych, a polecenie VSDBCMD powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="a3f8d-140">For example, to generate a deployment script for the **ContactManager** database without actually deploying the database, your VSDBCMD command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


<span data-ttu-id="a3f8d-141">VSDBCMD to narzędzie wdrażania bazy danych różnicowych i jako takie przez skrypt wdrażania jest generowana dynamicznie ma zawierać wszystkie niezbędne do aktualizacji bieżącej bazy danych, jeśli taki istnieje określony schemat polecenia SQL.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-141">VSDBCMD is a differential database deployment tool, and as such the deployment script is dynamically generated to contain all the SQL commands necessary to update the current database, if one exists, to the specified schema.</span></span> <span data-ttu-id="a3f8d-142">Przeglądanie przez skrypt wdrażania to wygodny sposób, aby ustalić, jaki wpływ wdrożenia będą mogli korzystać w bieżącej bazie danych i danych, które zawiera.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-142">Reviewing the deployment script is a useful way to determine what impact your deployment will have on the current database and the data it contains.</span></span> <span data-ttu-id="a3f8d-143">Na przykład możesz chcieć określić:</span><span class="sxs-lookup"><span data-stu-id="a3f8d-143">For example, you might want to determine:</span></span>

- <span data-ttu-id="a3f8d-144">Zostaną usunięte wszystkie istniejące tabele i czy, spowoduje utratę danych.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-144">Whether any existing tables will be removed, and whether that will result in data loss.</span></span>
- <span data-ttu-id="a3f8d-145">Czy kolejność operacji niesie ze sobą ryzyko utraty danych, na przykład, jeśli podział lub scalania tabel.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-145">Whether the order of operations carries a risk of data loss, for example, if you're splitting or merging tables.</span></span>

<span data-ttu-id="a3f8d-146">Jeśli jesteś zadowolony z skryptu wdrażania, możesz powtórzyć VSDBCMD z **/dd+** flagę, aby wprowadzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-146">If you're happy with the deployment script, you can repeat the VSDBCMD with a **/dd+** flag to make the changes.</span></span> <span data-ttu-id="a3f8d-147">Alternatywnie możesz edytować skrypt wdrożeniowy do potrzeb, a następnie uruchomić go ręcznie na serwerze bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-147">Alternatively, you can edit the deployment script to meet your requirements and then execute it manually on the database server.</span></span>

## <a name="integrating-what-if-functionality-into-custom-project-files"></a><span data-ttu-id="a3f8d-148">Integrowanie funkcji "What If" pliki projektów niestandardowych</span><span class="sxs-lookup"><span data-stu-id="a3f8d-148">Integrating "What If" Functionality into Custom Project Files</span></span>

<span data-ttu-id="a3f8d-149">W bardziej złożonych scenariuszy wdrażania, możesz będziesz chciał użyć niestandardowy plik aparatu Microsoft Build Engine (MSBuild) projektu do hermetyzacji logikę kompilacji i wdrażania, zgodnie z opisem w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="a3f8d-149">In more complex deployment scenarios, you'll want to use a custom Microsoft Build Engine (MSBuild) project file to encapsulate your build and deployment logic, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="a3f8d-150">Na przykład w [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) przykładowe rozwiązanie *Publish.proj* pliku:</span><span class="sxs-lookup"><span data-stu-id="a3f8d-150">For example, in the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, the *Publish.proj* file:</span></span>

- <span data-ttu-id="a3f8d-151">Kompiluje rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-151">Builds the solution.</span></span>
- <span data-ttu-id="a3f8d-152">Pakowanie i wdrażanie aplikacji ContactManager.Mvc, korzysta się z narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-152">Uses Web Deploy to package and deploy the ContactManager.Mvc application.</span></span>
- <span data-ttu-id="a3f8d-153">Pakowanie i wdrażanie aplikacji ContactManager.Service, korzysta się z narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-153">Uses Web Deploy to package and deploy the ContactManager.Service application.</span></span>
- <span data-ttu-id="a3f8d-154">Wdraża **ContactManager** bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-154">Deploys the **ContactManager** database.</span></span>

<span data-ttu-id="a3f8d-155">Po zintegrowaniu wdrożenia wielu pakietów sieci web i/lub baz danych w procesie pojedynczy krok w ten sposób może także opcję wykonania całego wdrożenia w trybie "what if".</span><span class="sxs-lookup"><span data-stu-id="a3f8d-155">When you integrate the deployment of multiple web packages and/or databases into a single-step process in this way, you may also want the option of performing the entire deployment in a "what if" mode.</span></span>

<span data-ttu-id="a3f8d-156">*Publish.proj* plik pokazuje, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-156">The *Publish.proj* file demonstrates how you can do this.</span></span> <span data-ttu-id="a3f8d-157">Najpierw należy utworzyć właściwość do przechowywania wartości "what if":</span><span class="sxs-lookup"><span data-stu-id="a3f8d-157">First, you need to create a property to store the "what if" value:</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


<span data-ttu-id="a3f8d-158">W tym przypadku został utworzony właściwość o nazwie **WhatIf** z wartością domyślną **false**.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-158">In this case, you've created a property named **WhatIf** with a default value of **false**.</span></span> <span data-ttu-id="a3f8d-159">Użytkownicy mogą przesłaniać tę wartość przez ustawienie właściwości **true** w parametr wiersza polecenia, jak będzie wyświetlana wkrótce.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-159">Users can override this value by setting the property to **true** in a command-line parameter, as you'll see shortly.</span></span>

<span data-ttu-id="a3f8d-160">Następny etap to próby parametryzacji dowolnego narzędzia Web Deploy i VSDBCMD polecenia, aby odzwierciedlić flagi **WhatIf** wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-160">The next stage is to parameterize any Web Deploy and VSDBCMD commands so that the flags reflect the **WhatIf** property value.</span></span> <span data-ttu-id="a3f8d-161">Na przykład następny element docelowy (pobierane z *Publish.proj* pliku i uproszczonym) uruchamia *. pliku deploy.cmd* plik, aby wdrożyć pakiet sieci web.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-161">For example, the next target (taken from the *Publish.proj* file and simplified) runs the *.deploy.cmd* file to deploy a web package.</span></span> <span data-ttu-id="a3f8d-162">Domyślnie, polecenie zawiera **/Y** switch ("yes" lub tryb aktualizacji).</span><span class="sxs-lookup"><span data-stu-id="a3f8d-162">By default, the command includes a **/Y** switch ("yes," or update mode).</span></span> <span data-ttu-id="a3f8d-163">Jeśli **WhatIf** ustawiono **true**, zostanie zastąpiony przez **/t** przełącznika (wersji próbnej lub tryb "what if").</span><span class="sxs-lookup"><span data-stu-id="a3f8d-163">If **WhatIf** is set to **true**, this is replaced by a **/T** switch (trial, or "what if" mode).</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


<span data-ttu-id="a3f8d-164">Podobnie następny element docelowy korzysta z narzędzia VSDBCMD wdrażanie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-164">Similarly, the next target uses the VSDBCMD utility to deploy a database.</span></span> <span data-ttu-id="a3f8d-165">Domyślnie **/dd** przełącznik nie jest dołączony.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-165">By default, a **/dd** switch is not included.</span></span> <span data-ttu-id="a3f8d-166">Oznacza to, że VSDBCMD spowoduje to wygenerowanie skryptu wdrażania, ale nie spowoduje wdrożenia bazy danych&#x2014;innymi słowy, "what if" scenariusza.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-166">This means that VSDBCMD will generate a deployment script but will not deploy the database&#x2014;in other words, a "what if" scenario.</span></span> <span data-ttu-id="a3f8d-167">Jeśli **WhatIf** nie ustawiono właściwości **true**, **/dd** przełącznik jest dodawany i VSDBCMD wdroży bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-167">If the **WhatIf** property is not set to **true**, a **/dd** switch is added and VSDBCMD will deploy the database.</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


<span data-ttu-id="a3f8d-168">Można użyć tej samej metody próby parametryzacji wszystkich odpowiednich poleceń w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-168">You can use the same approach to parameterize all the relevant commands in your project file.</span></span> <span data-ttu-id="a3f8d-169">Jeśli chcesz uruchamiać wdrożenie "what if" można następnie wystarczy podać **WhatIf** wartości właściwości w wierszu polecenia:</span><span class="sxs-lookup"><span data-stu-id="a3f8d-169">When you want to run a "what if" deployment, you can then simply provide a **WhatIf** property value from the command line:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


<span data-ttu-id="a3f8d-170">W ten sposób można uruchomić wdrożenia "what if" dla wszystkich składników usługi projektu w jednym kroku.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-170">In this way, you can run a "what if" deployment for all your project components in a single step.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a3f8d-171">Wniosek</span><span class="sxs-lookup"><span data-stu-id="a3f8d-171">Conclusion</span></span>

<span data-ttu-id="a3f8d-172">W tym temacie opisano sposób uruchamiania "what if" wdrożeń za pomocą narzędzia Web Deploy, VSDBCMD i MSBuild.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-172">This topic described how to run "what if" deployments using Web Deploy, VSDBCMD, and MSBuild.</span></span> <span data-ttu-id="a3f8d-173">Wdrożenie "what if" pozwala ocenić wpływ proponowanej wdrożenia przed faktycznie wprowadź zmiany w środowisku docelowym.</span><span class="sxs-lookup"><span data-stu-id="a3f8d-173">A "what if" deployment lets you evaluate the impact of a proposed deployment before you actually make any changes to the destination environment.</span></span>

## <a name="further-reading"></a><span data-ttu-id="a3f8d-174">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="a3f8d-174">Further Reading</span></span>

<span data-ttu-id="a3f8d-175">Aby uzyskać więcej informacji na temat składni wiersza polecenia narzędzia Web Deploy, zobacz [ustawienia operacji wdrażania w sieci Web](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="a3f8d-175">For more information on Web Deploy command-line syntax, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span> <span data-ttu-id="a3f8d-176">Aby uzyskać wskazówki dotyczące opcji wiersza polecenia, korzystając z *. pliku deploy.cmd* plików, zobacz [jak: Instalowanie wdrażania pakietu przy użyciu pliku deploy.cmd pliku](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="a3f8d-176">For guidance on command-line options when you use the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="a3f8d-177">Aby uzyskać wskazówki dotyczące VSDBCMD składni wiersza polecenia, zobacz [VSDBCMD dokumentacja wiersza polecenia. Plik EXE (wdrożenia i importowanie schematu)](https://msdn.microsoft.com/library/dd193283.aspx).</span><span class="sxs-lookup"><span data-stu-id="a3f8d-177">For guidance on VSDBCMD command-line syntax, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3f8d-178">[Poprzednie](advanced-enterprise-web-deployment.md)
> [dalej](customizing-database-deployments-for-multiple-environments.md)</span><span class="sxs-lookup"><span data-stu-id="a3f8d-178">[Previous](advanced-enterprise-web-deployment.md)
[Next](customizing-database-deployments-for-multiple-environments.md)</span></span>
