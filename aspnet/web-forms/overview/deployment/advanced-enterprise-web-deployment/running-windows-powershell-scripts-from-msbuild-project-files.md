---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Uruchamianie skryptów programu Windows PowerShell z plików projektu MSBuild | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell jako część procesu kompilacji i wdrażania. Skrypt można uruchomić lokalnie (innymi słowy, na komputerze b...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: faedcee480b6c50dc560055206fedbe7af4d5f67
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803152"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="01506-104">Uruchamianie skryptów programu Windows PowerShell z plików projektu MSBuild</span><span class="sxs-lookup"><span data-stu-id="01506-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="01506-105">przez [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="01506-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="01506-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="01506-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="01506-107">W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell jako część procesu kompilacji i wdrażania.</span><span class="sxs-lookup"><span data-stu-id="01506-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="01506-108">Możesz uruchomić skrypt lokalnie (innymi słowy, na serwerze kompilacji), lub zdalnie, takich jak na docelowym serwerze sieci web lub serwera bazy danych.</span><span class="sxs-lookup"><span data-stu-id="01506-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="01506-109">Istnieje wiele powodów dlaczego warto uruchomić skrypt po wdrożeniu programu Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="01506-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="01506-110">Na przykład możesz chcieć:</span><span class="sxs-lookup"><span data-stu-id="01506-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="01506-111">Dodawanie źródła zdarzeń niestandardowych do rejestru.</span><span class="sxs-lookup"><span data-stu-id="01506-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="01506-112">Generowanie katalogu w systemie plików przekazywania.</span><span class="sxs-lookup"><span data-stu-id="01506-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="01506-113">Wyczyść katalogów kompilacji.</span><span class="sxs-lookup"><span data-stu-id="01506-113">Clean up build directories.</span></span>
> - <span data-ttu-id="01506-114">Zapisywanie wpisów w pliku dziennika niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="01506-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="01506-115">Wysyłaj wiadomości e-mail z zaproszeniem użytkowników do aplikacji sieci web użytkownicy nowo aprowizowanych.</span><span class="sxs-lookup"><span data-stu-id="01506-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="01506-116">Utwórz konta użytkowników z odpowiednimi uprawnieniami.</span><span class="sxs-lookup"><span data-stu-id="01506-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="01506-117">Skonfiguruj replikację między wystąpieniami programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="01506-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="01506-118">W tym temacie pokazują sposób uruchamiania skryptów programu Windows PowerShell, lokalnie i zdalnie z niestandardowe obiekty docelowe w pliku projektu aparatu Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="01506-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="01506-119">Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.</span><span class="sxs-lookup"><span data-stu-id="01506-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="01506-120">Metody wdrażania w ramach tego samouczka opiera się na podejście pliku projektu Podziel opisane w [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym proces kompilacji jest kontrolowana przez dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i jeden zawierający ustawienia specyficzne dla środowiska kompilacji i wdrażania.</span><span class="sxs-lookup"><span data-stu-id="01506-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="01506-121">W czasie kompilacji pliku projektu specyficznymi dla środowiska jest scalana w pliku projektu niezależnego od środowiska w celu utworzenia kompletny zestaw instrukcji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="01506-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="01506-122">Omówienie zadań</span><span class="sxs-lookup"><span data-stu-id="01506-122">Task Overview</span></span>

<span data-ttu-id="01506-123">Aby uruchomić skrypt programu Windows PowerShell w ramach procesu wdrażania automatycznego lub pojedynczy krok, należy wykonać te zadania wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="01506-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="01506-124">Dodaj skrypt programu Windows PowerShell do rozwiązania do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="01506-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="01506-125">Utwórz polecenie, które wywołuje skrypt programu Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="01506-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="01506-126">Znak ucieczki znaków zarezerwowanych XML w poleceniu.</span><span class="sxs-lookup"><span data-stu-id="01506-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="01506-127">Tworzenie elementu docelowego w pliku projektu MSBuild niestandardowych i używanie **Exec** zadania do uruchomienia polecenia.</span><span class="sxs-lookup"><span data-stu-id="01506-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="01506-128">W tym temacie pokazują sposób wykonania tych procedur.</span><span class="sxs-lookup"><span data-stu-id="01506-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="01506-129">Zadania i wskazówki, w tym temacie założono, że znasz już właściwości i elementy docelowe programu MSBuild, i że rozumiesz, jak używać niestandardowego pliku projektu MSBuild do procesu kompilacji i wdrażania.</span><span class="sxs-lookup"><span data-stu-id="01506-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="01506-130">Aby uzyskać więcej informacji, zobacz [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="01506-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="01506-131">Tworzenie i dodawanie skryptów programu Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="01506-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="01506-132">Zadania przedstawione w tym temacie Użyj przykładowy skrypt programu Windows PowerShell o nazwie **LogDeploy.ps1** ilustruje sposób uruchamiania skryptów z programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="01506-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="01506-133">**LogDeploy.ps1** skrypt zawiera prostej funkcji, która zapisuje wpis jeden wiersz w pliku dziennika:</span><span class="sxs-lookup"><span data-stu-id="01506-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]


<span data-ttu-id="01506-134">**LogDeploy.ps1** skrypt akceptuje dwa parametry.</span><span class="sxs-lookup"><span data-stu-id="01506-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="01506-135">Pierwszy parametr reprezentuje pełną ścieżkę do pliku dziennika, do którego chcesz dodać wpis, a drugi parametr reprezentuje miejsce docelowe wdrożenia, które mają być rejestrowane w pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="01506-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="01506-136">Po uruchomieniu skryptu, dodaje wiersz do pliku dziennika w następującym formacie:</span><span class="sxs-lookup"><span data-stu-id="01506-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="01506-137">Aby **LogDeploy.ps1** skryptu dostępne dla programu MSBuild, należy:</span><span class="sxs-lookup"><span data-stu-id="01506-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="01506-138">Dodaj skrypt do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="01506-138">Add the script to source control.</span></span>
- <span data-ttu-id="01506-139">Dodaj skrypt do rozwiązania w programie Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="01506-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="01506-140">Nie trzeba wdrożyć skrypt za pomocą rozwiązania zawartości, niezależnie od tego, czy jest planowane do uruchomienia skryptu na serwerze kompilacji lub na komputerze zdalnym.</span><span class="sxs-lookup"><span data-stu-id="01506-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="01506-141">Jedną z opcji jest Dodawanie skryptu do folderu rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="01506-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="01506-142">W przykładzie Contact Manager ponieważ chcesz użyć skryptu programu Windows PowerShell jako część procesu wdrażania, dobrym pomysłem będzie Dodaj skrypt do folderu publikowania rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="01506-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="01506-143">Zawartość folderów rozwiązania są kopiowane do serwerów kompilacji jako materiał źródłowy.</span><span class="sxs-lookup"><span data-stu-id="01506-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="01506-144">Jednak część nie żadnych danych wyjściowych projektu.</span><span class="sxs-lookup"><span data-stu-id="01506-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="01506-145">Wykonywanie skryptu programu Windows PowerShell na serwerze kompilacji</span><span class="sxs-lookup"><span data-stu-id="01506-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="01506-146">W niektórych przypadkach warto uruchamiać skrypty programu Windows PowerShell na komputerze, który tworzy swoje projekty.</span><span class="sxs-lookup"><span data-stu-id="01506-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="01506-147">Może na przykład użyć skryptu programu Windows PowerShell, czyszczenie kompilacji folderów lub zapisywanie wpisów w pliku dziennika niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="01506-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="01506-148">Pod względem składni jest taka sama jak uruchomienie skryptu programu Windows PowerShell z wiersza polecenia, regularne uruchamianie skryptu programu Windows PowerShell z pliku projektu programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="01506-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="01506-149">Musisz wywołać powershell.exe pliku wykonywalnego i użyj **— polecenie** przełącznika, aby zapewnić polecenia, które mają programu Windows PowerShell do uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="01506-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="01506-150">(W programie Windows PowerShell w wersji 2 umożliwia także **— plik** przełącznika).</span><span class="sxs-lookup"><span data-stu-id="01506-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="01506-151">Polecenie należy wykonać następujący format:</span><span class="sxs-lookup"><span data-stu-id="01506-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="01506-152">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="01506-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="01506-153">Ścieżka do skryptu zawiera spacje, należy ująć ją w pliku w pojedynczym cudzysłowie poprzedzone handlowe "i".</span><span class="sxs-lookup"><span data-stu-id="01506-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="01506-154">Nie można użyć podwójnych cudzysłowów, ponieważ został już użyty do należy wpisać polecenie:</span><span class="sxs-lookup"><span data-stu-id="01506-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="01506-155">Istnieje kilka dodatkowych kwestii dotyczących po wywołaniu tego polecenia z programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="01506-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="01506-156">Po pierwsze należy uwzględnić **— NonInteractive** flagi, aby upewnić się, że skrypt wykonuje ciche.</span><span class="sxs-lookup"><span data-stu-id="01506-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="01506-157">Następnie należy uwzględnić **-ExecutionPolicy** flagi z wartością odpowiedniego argumentu.</span><span class="sxs-lookup"><span data-stu-id="01506-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="01506-158">To ustawienie określa zasady wykonywania, że program Windows PowerShell będzie miała zastosowanie do skryptu i pozwala zastąpić domyślne zasady wykonywania, które mogą uniemożliwić wykonywanie skryptu.</span><span class="sxs-lookup"><span data-stu-id="01506-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="01506-159">Możesz wybrać spośród tych wartości argumentu:</span><span class="sxs-lookup"><span data-stu-id="01506-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="01506-160">Wartość **Unrestricted** umożliwi programu Windows PowerShell w celu wykonania skryptu, niezależnie od tego, czy skrypt jest podpisany.</span><span class="sxs-lookup"><span data-stu-id="01506-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="01506-161">Wartość **RemoteSigned** umożliwi programu Windows PowerShell do wykonania niepodpisanych skryptów, które zostały utworzone na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="01506-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="01506-162">Jednak muszą być podpisane skrypty, które zostały utworzone w innym miejscu.</span><span class="sxs-lookup"><span data-stu-id="01506-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="01506-163">(W praktyce jest bardzo mało prawdopodobne, aby zostały utworzone skrypt programu Windows PowerShell lokalnie na serwerze kompilacji).</span><span class="sxs-lookup"><span data-stu-id="01506-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="01506-164">Wartość **wszystkie podpisane** umożliwi programu Windows PowerShell do wykonywania tylko skrypty podpisane.</span><span class="sxs-lookup"><span data-stu-id="01506-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="01506-165">Domyślne zasady wykonywania jest **ograniczeniami**, co uniemożliwia uruchamianie wszystkich plików skryptów programu Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="01506-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="01506-166">Na koniec należy jako znak ucieczki dla wszelkich zarezerwowanych znaków XML, które występują w polecenia programu Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="01506-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="01506-167">Zastąp apostrofy z  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="01506-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="01506-168">Zastąp znaki cudzysłowu  **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="01506-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="01506-169">Zastąp takie znaki z  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="01506-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="01506-170">Po wprowadzeniu tych zmian, polecenie będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="01506-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="01506-171">W ramach Twojego niestandardowego pliku projektu MSBuild, można utworzyć nowy obiekt docelowy i użyć **Exec** zadania do uruchomienia tego polecenia:</span><span class="sxs-lookup"><span data-stu-id="01506-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="01506-172">W tym przykładzie należy zauważyć, że:</span><span class="sxs-lookup"><span data-stu-id="01506-172">In this example, note that:</span></span>

- <span data-ttu-id="01506-173">Wszelkie zmienne, takie jak wartości parametrów i lokalizację pliku wykonywalnego programu Windows PowerShell, są deklarowane jako właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="01506-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="01506-174">Aby umożliwić użytkownikom zastąpić te wartości w wierszu polecenia znajdują się warunki.</span><span class="sxs-lookup"><span data-stu-id="01506-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="01506-175">**MSDeployComputerName** właściwość jest deklarowana w innym miejscu w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="01506-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="01506-176">Po wykonaniu ten element docelowy jako część procesu kompilacji programu Windows PowerShell Uruchom polecenie i zapisać wpisu dziennika do podanego pliku.</span><span class="sxs-lookup"><span data-stu-id="01506-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="01506-177">Wykonywanie skryptu programu Windows PowerShell na komputerze zdalnym</span><span class="sxs-lookup"><span data-stu-id="01506-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="01506-178">Programu Windows PowerShell jest możliwe uruchamianie skryptów na komputerach zdalnych za pośrednictwem [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="01506-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="01506-179">Aby to zrobić, należy użyć [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) polecenia cmdlet.</span><span class="sxs-lookup"><span data-stu-id="01506-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="01506-180">Umożliwia to wykonywanie skryptu względem co najmniej jeden komputer zdalny bez kopiowania skryptu z komputerami zdalnymi.</span><span class="sxs-lookup"><span data-stu-id="01506-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="01506-181">Wszystkie wyniki są zwracane do komputera lokalnego, z którego uruchomiono skrypt.</span><span class="sxs-lookup"><span data-stu-id="01506-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="01506-182">Przed użyciem **Invoke-Command** polecenia cmdlet programu Windows PowerShell do wykonywania skryptów na komputerze zdalnym, należy skonfigurować odbiornik usługi WinRM do akceptowania wiadomości zdalne.</span><span class="sxs-lookup"><span data-stu-id="01506-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="01506-183">Można to zrobić, uruchamiając polecenie **winrm quickconfig** na komputerze zdalnym.</span><span class="sxs-lookup"><span data-stu-id="01506-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="01506-184">Aby uzyskać więcej informacji, zobacz [instalacji i konfiguracji dla Windows zdalne zarządzanie](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="01506-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="01506-185">W oknie programu Windows PowerShell, ta składnia będzie używane do uruchamiania **LogDeploy.ps1** skryptu na komputerze zdalnym:</span><span class="sxs-lookup"><span data-stu-id="01506-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="01506-186">Istnieją różne sposoby korzystania z **Invoke-Command** do uruchamiania skryptu pliku, ale ta metoda jest najbardziej proste kiedy trzeba będzie podać wartości parametrów i zarządzanie nimi ścieżki zawierające spacje.</span><span class="sxs-lookup"><span data-stu-id="01506-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="01506-187">Po uruchomieniu tego z poziomu wiersza polecenia, musisz wywołać z programu Windows PowerShell pliku wykonywalnego i użyj **— polecenie** parametru, aby dołączyć instrukcje:</span><span class="sxs-lookup"><span data-stu-id="01506-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="01506-188">Jak wcześniej, musisz podać kilka dodatkowych przełączników i znak ucieczki znaków zarezerwowanych XML, po uruchomieniu polecenia z programu MSBuild:</span><span class="sxs-lookup"><span data-stu-id="01506-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="01506-189">Na koniec, tak jak poprzednio, możesz użyć **Exec** zadanie w ramach niestandardowy cel programu MSBuild do wykonania polecenia:</span><span class="sxs-lookup"><span data-stu-id="01506-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="01506-190">Po wykonaniu ten element docelowy jako część procesu kompilacji programu Windows PowerShell uruchomić skrypt na komputerze określonym w **– computername** argumentu.</span><span class="sxs-lookup"><span data-stu-id="01506-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="01506-191">Wniosek</span><span class="sxs-lookup"><span data-stu-id="01506-191">Conclusion</span></span>

<span data-ttu-id="01506-192">W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell z pliku projektu programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="01506-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="01506-193">Ta metoda służy do uruchamiania skryptu programu Windows PowerShell, lokalnie lub na komputerze zdalnym, w ramach automatycznych lub pojedynczy krok procesu kompilowania i wdrażania.</span><span class="sxs-lookup"><span data-stu-id="01506-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="01506-194">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="01506-194">Further Reading</span></span>

<span data-ttu-id="01506-195">Aby uzyskać wskazówki dotyczące podpisywania skryptów programu Windows PowerShell i zarządzanie zasadami wykonywania, zobacz [uruchamiania skryptów programu Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="01506-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="01506-196">Aby uzyskać wskazówki na temat uruchamiania poleceń programu Windows PowerShell z komputera zdalnego, zobacz [uruchamianie poleceń zdalnych](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="01506-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="01506-197">Aby uzyskać więcej informacji na temat korzystania z niestandardowych plików projektu MSBuild do kontrolowania procesu wdrażania, zobacz [objaśnienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [objaśnienie procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="01506-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="01506-198">[Poprzednie](taking-web-applications-offline-with-web-deploy.md)
> [dalej](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="01506-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
