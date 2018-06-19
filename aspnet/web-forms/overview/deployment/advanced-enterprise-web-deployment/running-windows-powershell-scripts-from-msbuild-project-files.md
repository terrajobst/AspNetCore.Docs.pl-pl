---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Uruchomionych skryptów PowerShell systemu Windows z plików projektu MSBuild | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell jako część procesu kompilacji i wdrożenia. Skrypt można uruchomić lokalnie (innymi słowy, na b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: c8ef22cfbba7b3b85944ea4c49f3183e5a6aafbb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890362"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="59a8d-104">Uruchomionych skryptów PowerShell systemu Windows z plików projektów MSBuild</span><span class="sxs-lookup"><span data-stu-id="59a8d-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="59a8d-105">przez [Lewandowski Jason](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="59a8d-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="59a8d-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="59a8d-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="59a8d-107">W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell jako część procesu kompilacji i wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="59a8d-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="59a8d-108">Możesz uruchomić skrypt lokalnie (innymi słowy, na serwerze kompilacji) lub zdalnie, takich jak na docelowym serwerze sieci web lub serwer bazy danych.</span><span class="sxs-lookup"><span data-stu-id="59a8d-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="59a8d-109">Istnieje wiele przyczyn, dlaczego warto Uruchom skrypt programu Windows PowerShell po wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="59a8d-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="59a8d-110">Na przykład możesz chcieć:</span><span class="sxs-lookup"><span data-stu-id="59a8d-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="59a8d-111">Dodawanie źródła zdarzeń niestandardowych w rejestrze.</span><span class="sxs-lookup"><span data-stu-id="59a8d-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="59a8d-112">Generowanie katalogu w systemie plików przekazywania.</span><span class="sxs-lookup"><span data-stu-id="59a8d-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="59a8d-113">Czyszczenie kompilacji katalogów.</span><span class="sxs-lookup"><span data-stu-id="59a8d-113">Clean up build directories.</span></span>
> - <span data-ttu-id="59a8d-114">Zapisywanie wpisów w pliku dziennika niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="59a8d-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="59a8d-115">Wysyłaj wiadomości e-mail zapraszanie użytkowników do aplikacji sieci web nowo udostępnione.</span><span class="sxs-lookup"><span data-stu-id="59a8d-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="59a8d-116">Utwórz konta użytkowników z odpowiednimi uprawnieniami.</span><span class="sxs-lookup"><span data-stu-id="59a8d-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="59a8d-117">Konfigurowanie replikacji między wystąpieniami programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="59a8d-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="59a8d-118">W tym temacie opisano sposób uruchamiania skryptów programu Windows PowerShell lokalnie i zdalnie niestandardowy element docelowy w pliku projektu Microsoft kompilacji Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="59a8d-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="59a8d-119">Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tego samouczka serii&#x2014; [rozwiązania kontaktów Menedżerze](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, Windows Communication Usługa Foundation (WCF), a projekt bazy danych.</span><span class="sxs-lookup"><span data-stu-id="59a8d-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="59a8d-120">Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji dwa pliki projektu&#x2014;jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska.</span><span class="sxs-lookup"><span data-stu-id="59a8d-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="59a8d-121">W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="59a8d-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="59a8d-122">Omówienie zadań</span><span class="sxs-lookup"><span data-stu-id="59a8d-122">Task Overview</span></span>

<span data-ttu-id="59a8d-123">Aby uruchomić skrypt programu Windows PowerShell w ramach procesu wdrażania automatycznego lub pojedynczy krok, należy wykonać te zadania wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="59a8d-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="59a8d-124">Dodaj skrypt programu Windows PowerShell do rozwiązania i do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="59a8d-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="59a8d-125">Utwórz polecenie, które wywołuje skrypt programu Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="59a8d-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="59a8d-126">Usuń wszystkie zastrzeżone znaki XML polecenia.</span><span class="sxs-lookup"><span data-stu-id="59a8d-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="59a8d-127">Tworzenie obiektu docelowego w pliku projektu MSBuild niestandardowych i używanie **Exec** zadań do uruchomienia polecenia.</span><span class="sxs-lookup"><span data-stu-id="59a8d-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="59a8d-128">W tym temacie opisano sposób wykonywania tych procedur.</span><span class="sxs-lookup"><span data-stu-id="59a8d-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="59a8d-129">Zadania i wskazówki, w tym temacie założono, że znasz już docelowych elementów MSBuild i właściwości i że rozumiesz, jak używać niestandardowego pliku projektu MSBuild do dysków z procesem kompilacji i wdrażania.</span><span class="sxs-lookup"><span data-stu-id="59a8d-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="59a8d-130">Aby uzyskać więcej informacji, zobacz [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="59a8d-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="59a8d-131">Tworzenie i dodawanie skryptów programu Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="59a8d-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="59a8d-132">Zadania w tym temacie używają przykładowy skrypt programu Windows PowerShell o nazwie **LogDeploy.ps1** ilustrujący sposób uruchamiania skryptów z programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="59a8d-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="59a8d-133">**LogDeploy.ps1** skryptu zawiera proste funkcję, która zapisuje wpis jeden wiersz w pliku dziennika:</span><span class="sxs-lookup"><span data-stu-id="59a8d-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="59a8d-134">**LogDeploy.ps1** skrypt akceptuje dwa parametry.</span><span class="sxs-lookup"><span data-stu-id="59a8d-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="59a8d-135">Pierwszy parametr reprezentuje pełną ścieżkę do pliku dziennika, do której chcesz dodać wpis, a drugi parametr reprezentuje miejsce docelowe wdrożenia, które mają być rejestrowane w pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="59a8d-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="59a8d-136">Po uruchomieniu skryptu dodaje wiersza do pliku dziennika w następującym formacie:</span><span class="sxs-lookup"><span data-stu-id="59a8d-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="59a8d-137">Aby **LogDeploy.ps1** skryptów dostępne dla programu MSBuild, konieczne jest:</span><span class="sxs-lookup"><span data-stu-id="59a8d-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="59a8d-138">Dodaj skrypt do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="59a8d-138">Add the script to source control.</span></span>
- <span data-ttu-id="59a8d-139">Dodaj skrypt do rozwiązania w programie Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="59a8d-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="59a8d-140">Nie trzeba wdrażać skryptu zawartości rozwiązania, niezależnie od tego, czy zamierzasz uruchomić skrypt na serwerze kompilacji lub na komputerze zdalnym.</span><span class="sxs-lookup"><span data-stu-id="59a8d-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="59a8d-141">Jedną z opcji jest dodać skrypt do folderu rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="59a8d-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="59a8d-142">W przykładzie kontaktów Menedżerze ponieważ chcesz użyć skryptu programu Windows PowerShell w ramach procesu wdrażania warto dodać skrypt do folderu rozwiązania publikowania.</span><span class="sxs-lookup"><span data-stu-id="59a8d-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="59a8d-143">Zawartość folderów rozwiązania są kopiowane do tworzenia serwery jako źródło materiału.</span><span class="sxs-lookup"><span data-stu-id="59a8d-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="59a8d-144">Jednak tworzą one żadna część żadnych danych wyjściowych projektu.</span><span class="sxs-lookup"><span data-stu-id="59a8d-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="59a8d-145">Wykonywanie skryptu programu Windows PowerShell na serwerze kompilacji</span><span class="sxs-lookup"><span data-stu-id="59a8d-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="59a8d-146">W niektórych scenariuszach możesz uruchamiać skrypty programu Windows PowerShell na komputerze, który tworzy projektów.</span><span class="sxs-lookup"><span data-stu-id="59a8d-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="59a8d-147">Na przykład można użyć skryptu programu Windows PowerShell czyszczenie kompilacji folderów lub tworzyć wpisy w pliku dziennika niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="59a8d-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="59a8d-148">Pod względem składni uruchamiając skrypt programu Windows PowerShell z pliku projektu MSBuild jest taka sama jak wykonywanie skryptu programu Windows PowerShell z regularnych wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="59a8d-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="59a8d-149">Należy wywołać powershell.exe pliku wykonywalnego i użyj **— polecenie** przełącznik, aby zapewnić polecenia, które mają środowiska Windows PowerShell do uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="59a8d-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="59a8d-150">(W programie Windows PowerShell v2, można również użyć **— plik** przełącznika).</span><span class="sxs-lookup"><span data-stu-id="59a8d-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="59a8d-151">Polecenie należy wykonać ten format:</span><span class="sxs-lookup"><span data-stu-id="59a8d-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="59a8d-152">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="59a8d-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="59a8d-153">Ścieżka do skryptu zawiera spacje, należy ująć ją w pliku w apostrofy poprzedzone znakiem.</span><span class="sxs-lookup"><span data-stu-id="59a8d-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="59a8d-154">Nie można użyć podwójnego cudzysłowu, ponieważ został już użyty do umieść polecenie:</span><span class="sxs-lookup"><span data-stu-id="59a8d-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="59a8d-155">Istnieje kilka uwagi dodatkowe po wywołaniu tego polecenia programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="59a8d-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="59a8d-156">Najpierw należy dołączyć **— NonInteractive** flagę, aby upewnić się, że skrypt jest wykonywany ciche.</span><span class="sxs-lookup"><span data-stu-id="59a8d-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="59a8d-157">Następnie należy uwzględnić **-ExecutionPolicy** flagę wartość argumentu odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="59a8d-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="59a8d-158">To ustawienie określa zasad wykonywania, czy środowisko Windows PowerShell będą stosowane do skryptu i pozwala zastąpić domyślne zasady wykonywania, które mogą uniemożliwić wykonanie skryptu.</span><span class="sxs-lookup"><span data-stu-id="59a8d-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="59a8d-159">Możesz wybrać te wartości argumentu:</span><span class="sxs-lookup"><span data-stu-id="59a8d-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="59a8d-160">Wartość **bez ograniczeń** umożliwi środowiska Windows PowerShell do wykonywania skryptu, niezależnie od tego, czy skrypt ma podpis.</span><span class="sxs-lookup"><span data-stu-id="59a8d-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="59a8d-161">Wartość **RemoteSigned** umożliwi środowiska Windows PowerShell do wykonywania niepodpisanych skryptów, które zostały utworzone na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="59a8d-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="59a8d-162">Jednak skrypty, które zostały utworzone w innym miejscu muszą być podpisane.</span><span class="sxs-lookup"><span data-stu-id="59a8d-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="59a8d-163">(W praktyce, wszystko jest mało prawdopodobne, aby został utworzony skrypt programu Windows PowerShell lokalnie na serwerze kompilacji).</span><span class="sxs-lookup"><span data-stu-id="59a8d-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="59a8d-164">Wartość **AllSigned** umożliwi środowiska Windows PowerShell do wykonywania tylko skrypty podpisane.</span><span class="sxs-lookup"><span data-stu-id="59a8d-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="59a8d-165">Domyślne zasady wykonywania jest **Restricted**, co uniemożliwia uruchomienie żadnych plików skryptów środowiska Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="59a8d-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="59a8d-166">Na koniec należy Usuń wszystkie zastrzeżone znaki XML występujących polecenia programu Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="59a8d-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="59a8d-167">Zastąp apostrofy z  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="59a8d-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="59a8d-168">Zastąp znaki cudzysłowu  **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="59a8d-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="59a8d-169">Zastąp takie znaki z  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="59a8d-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="59a8d-170">Po wprowadzeniu tych zmian, polecenie będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="59a8d-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="59a8d-171">W pliku projektu MSBuild niestandardowego można utworzyć nowy obiekt docelowy i użyj **Exec** zadań, aby uruchomić to polecenie:</span><span class="sxs-lookup"><span data-stu-id="59a8d-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="59a8d-172">W tym przykładzie należy pamiętać, że:</span><span class="sxs-lookup"><span data-stu-id="59a8d-172">In this example, note that:</span></span>

- <span data-ttu-id="59a8d-173">Wszelkie zmienne, takie jak wartości parametrów i lokalizacji pliku wykonywalnego programu Windows PowerShell, są deklarowane jako właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="59a8d-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="59a8d-174">Aby umożliwić użytkownikom przesłonić te wartości w wierszu polecenia znajdują się warunki.</span><span class="sxs-lookup"><span data-stu-id="59a8d-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="59a8d-175">**MSDeployComputerName** właściwości zadeklarowano w innym miejscu w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="59a8d-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="59a8d-176">Po wykonaniu tego obiektu docelowego jako część procesu kompilacji programu Windows PowerShell Uruchom polecenie i zapisać wpisu dziennika do podanego pliku.</span><span class="sxs-lookup"><span data-stu-id="59a8d-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="59a8d-177">Wykonywanie skryptu programu Windows PowerShell na komputerze zdalnym</span><span class="sxs-lookup"><span data-stu-id="59a8d-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="59a8d-178">Program Windows PowerShell jest może uruchamiać skrypty na komputerach zdalnych za pośrednictwem [zdalnego zarządzania systemem Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="59a8d-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="59a8d-179">Aby to zrobić, należy użyć [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) polecenia cmdlet.</span><span class="sxs-lookup"><span data-stu-id="59a8d-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="59a8d-180">Dzięki temu można wykonać skryptu na co najmniej jeden komputer zdalny bez kopiowania skrypt do komputerów zdalnych.</span><span class="sxs-lookup"><span data-stu-id="59a8d-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="59a8d-181">Wyniki są zwracane do komputera lokalnego, z którego uruchomiono skrypt.</span><span class="sxs-lookup"><span data-stu-id="59a8d-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="59a8d-182">Przed użyciem **Invoke-Command** skrypty polecenia cmdlet do wykonania programu Windows PowerShell na komputerze zdalnym, należy skonfigurować odbiornik usługi WinRM do akceptowania zdalnego wiadomości.</span><span class="sxs-lookup"><span data-stu-id="59a8d-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="59a8d-183">Można to zrobić, uruchamiając polecenie **winrm quickconfig** na komputerze zdalnym.</span><span class="sxs-lookup"><span data-stu-id="59a8d-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="59a8d-184">Aby uzyskać więcej informacji, zobacz [instalacji i konfiguracji do zdalnego zarządzania systemem Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="59a8d-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="59a8d-185">Z okna programu Windows PowerShell, należy użyć następującej składni, aby Uruchom **LogDeploy.ps1** skryptu na komputerze zdalnym:</span><span class="sxs-lookup"><span data-stu-id="59a8d-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="59a8d-186">Istnieją różne sposoby używania **Invoke-Command** do uruchamiania skryptu pliku, ale ta metoda jest najbardziej oczywistym kiedy trzeba podać wartości parametrów i zarządzanie nimi ścieżki zawierające spacje.</span><span class="sxs-lookup"><span data-stu-id="59a8d-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="59a8d-187">Po uruchomieniu tej z wiersza polecenia, należy wywołać pliku wykonywalnego programu Windows PowerShell i użyć **— polecenie** parametr, aby dołączyć instrukcje:</span><span class="sxs-lookup"><span data-stu-id="59a8d-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="59a8d-188">Ponieważ przed, musisz podać kilka dodatkowych przełączników i uniknięcie znaków zarezerwowanych XML, po uruchomieniu polecenia z MSBuild:</span><span class="sxs-lookup"><span data-stu-id="59a8d-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="59a8d-189">Ostatecznie, jak poprzednio, korzystając z **Exec** zadanie w ramach niestandardowych docelowy programu MSBuild można wykonać polecenia:</span><span class="sxs-lookup"><span data-stu-id="59a8d-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="59a8d-190">Po wykonaniu tego obiektu docelowego jako część procesu kompilacji programu Windows PowerShell Uruchom skrypt na komputerze określonym w **-computername** argumentu.</span><span class="sxs-lookup"><span data-stu-id="59a8d-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="59a8d-191">Wniosek</span><span class="sxs-lookup"><span data-stu-id="59a8d-191">Conclusion</span></span>

<span data-ttu-id="59a8d-192">W tym temacie opisano sposób uruchamiania skryptu programu Windows PowerShell z pliku projektu programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="59a8d-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="59a8d-193">Takie podejście umożliwia lokalnie lub na komputerze zdalnym, uruchom skrypt programu Windows PowerShell w trakcie procesu automatyczna lub pojedynczy krok kompilacji i wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="59a8d-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="59a8d-194">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="59a8d-194">Further Reading</span></span>

<span data-ttu-id="59a8d-195">Aby uzyskać wskazówki dotyczące podpisywania skryptów programu Windows PowerShell i zarządzanie zasadami wykonywania, zobacz [uruchomione skrypty programu Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="59a8d-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="59a8d-196">Aby uzyskać wskazówki na temat uruchamiania poleceń programu Windows PowerShell z komputera zdalnego, zobacz [uruchamiania poleceń zdalnych](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="59a8d-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="59a8d-197">Aby uzyskać więcej informacji na temat używania niestandardowe pliki projektu MSBuild kontrolować proces wdrażania, zobacz [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="59a8d-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="59a8d-198">[Poprzednie](taking-web-applications-offline-with-web-deploy.md)
> [dalej](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="59a8d-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
