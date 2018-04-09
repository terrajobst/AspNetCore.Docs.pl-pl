---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Trwa konfigurowanie rozwiązania kontaktów Menedżerze | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano, jak pobrać i skonfigurować Menedżera skontaktuj się z rozwiązania do uruchomienia lokalnie na stacji roboczej developer.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: e8fb24f5b2d96d864d1aa6bc0f78644773de00ab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="372e9-103">Trwa konfigurowanie rozwiązania z menedżerem kontaktu</span><span class="sxs-lookup"><span data-stu-id="372e9-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="372e9-104">przez [Lewandowski Jason](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="372e9-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="372e9-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="372e9-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="372e9-106">W tym temacie opisano, jak pobrać i skonfigurować Menedżera skontaktuj się z rozwiązania do uruchomienia lokalnie na stacji roboczej developer.</span><span class="sxs-lookup"><span data-stu-id="372e9-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="372e9-107">Wymagania systemowe</span><span class="sxs-lookup"><span data-stu-id="372e9-107">System Requirements</span></span>

<span data-ttu-id="372e9-108">Aby uruchomić rozwiązanie kontaktów Menedżerze lokalnie i wykonywać inne zadania opisane w tym samouczku, musisz zainstalować tego oprogramowania na stacji roboczej developer:</span><span class="sxs-lookup"><span data-stu-id="372e9-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="372e9-109">Visual Studio 2010 Service Pack 1, Premium lub Ultimate</span><span class="sxs-lookup"><span data-stu-id="372e9-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="372e9-110">Internetowe usługi informacyjne (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="372e9-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="372e9-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="372e9-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="372e9-112">Usługi IIS narzędzie Web Deployment (Web Deploy) 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="372e9-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="372e9-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="372e9-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="372e9-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="372e9-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="372e9-115">Program .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="372e9-115">.NET Framework 4</span></span>
- <span data-ttu-id="372e9-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="372e9-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="372e9-117">Z wyjątkiem programu Visual Studio 2010, należy pobrać i zainstalować najnowsze wersje wszystkich produktów i składników za pomocą [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="372e9-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="372e9-118">Pobierać i wyodrębniać rozwiązania</span><span class="sxs-lookup"><span data-stu-id="372e9-118">Download and Extract the Solution</span></span>

<span data-ttu-id="372e9-119">Możesz pobrać menedżera skontaktuj się z przykładową aplikację z galerii kodu MSDN [tutaj](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="372e9-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="372e9-120">Konfigurowanie i uruchamianie rozwiązania</span><span class="sxs-lookup"><span data-stu-id="372e9-120">Configure and Run the Solution</span></span>

<span data-ttu-id="372e9-121">Aby skonfigurować i uruchamianie rozwiązania Contact Manager na komputerze lokalnym, należy wykonać następujące ogólne kroki:</span><span class="sxs-lookup"><span data-stu-id="372e9-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="372e9-122">Jeśli nie masz już, Utwórz lokalną bazę danych usług aplikacji platformy ASP.NET z funkcji zarządzania członkostwa i ról włączona.</span><span class="sxs-lookup"><span data-stu-id="372e9-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="372e9-123">Edytuj parametry połączenia w *web.config* pliki, aby wskazywał lokalne wystąpienie programu SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="372e9-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="372e9-124">Uruchom rozwiązania z programu Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="372e9-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="372e9-125">W pozostałej części tej sekcji znajdują się wskazówki więcej na temat sposobu ukończenia każdego z tych zadań.</span><span class="sxs-lookup"><span data-stu-id="372e9-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="372e9-126">**Aby utworzyć bazę danych usług aplikacji**</span><span class="sxs-lookup"><span data-stu-id="372e9-126">**To create the application services database**</span></span>

1. <span data-ttu-id="372e9-127">Otwórz wiersz polecenia programu Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="372e9-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="372e9-128">Aby to zrobić, na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **programu Microsoft Visual Studio 2010**, kliknij przycisk **programu Visual Studio Tools**, a następnie Kliknij przycisk **wiersz polecenia programu Visual Studio (2010)**.</span><span class="sxs-lookup"><span data-stu-id="372e9-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="372e9-129">W wierszu polecenia wpisz następujące polecenie, a następnie naciśnij klawisz Enter:</span><span class="sxs-lookup"><span data-stu-id="372e9-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="372e9-130">Użyj **– C** przełącznik, aby określić parametry połączenia dla serwera bazy danych.</span><span class="sxs-lookup"><span data-stu-id="372e9-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="372e9-131">Użyj **–A** przełącznik, aby określić funkcje, które mają zostać dodane do bazy danych usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="372e9-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="372e9-132">W takim przypadku **m** wskazuje, że chcesz dodać obsługę dostawcy członkostwa i **r** wskazuje, że chcesz dodać obsługę menedżera ról.</span><span class="sxs-lookup"><span data-stu-id="372e9-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="372e9-133">Użyj **– d** przełącznik, aby określić nazwę bazy danych usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="372e9-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="372e9-134">Jeśli ta opcja zostanie pominięta, narzędzie utworzy bazę danych o nazwie domyślnej **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="372e9-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="372e9-135">Gdy baza danych została pomyślnie utworzona, wiersz polecenia zostanie wyświetlone potwierdzenie.</span><span class="sxs-lookup"><span data-stu-id="372e9-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="372e9-136">Aby uzyskać więcej informacji na temat aspnet\_regsql narzędzie, zobacz [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="372e9-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="372e9-137">Następnym krokiem jest, aby upewnić się, że parametry połączenia w rozwiązaniu kontaktów Menedżerze odwołują się do lokalnego wystąpienia programu SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="372e9-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="372e9-138">**Aby zaktualizować ciągi połączenia**</span><span class="sxs-lookup"><span data-stu-id="372e9-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="372e9-139">Otwórz rozwiązanie Contact Manager w programie Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="372e9-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="372e9-140">W **Eksploratora rozwiązań** okna, rozwiń węzeł **ContactManager.Mvc** projektu, a następnie kliknij dwukrotnie **Web.config** węzła.</span><span class="sxs-lookup"><span data-stu-id="372e9-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="372e9-141">Projekt ContactManager.Mvc zawiera dwa *web.config* plików.</span><span class="sxs-lookup"><span data-stu-id="372e9-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="372e9-142">Musisz zmodyfikować plik poziom projektu.</span><span class="sxs-lookup"><span data-stu-id="372e9-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="372e9-143">W **connectionStrings** elementu, sprawdź, czy parametry połączenia o nazwie **ApplicationServices** wskazuje lokalnej bazy danych usług aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="372e9-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="372e9-144">W **Eksploratora rozwiązań** okna, rozwiń węzeł **ContactManager.Service** projektu, a następnie kliknij dwukrotnie **Web.config** węzła.</span><span class="sxs-lookup"><span data-stu-id="372e9-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="372e9-145">W **connectionStrings** elementu w parametrach połączenia o nazwie **ContactManagerContext**, upewnij się, że **źródła danych** właściwość jest ustawiona na lokalnego wystąpienia programu SQL Server Server Express.</span><span class="sxs-lookup"><span data-stu-id="372e9-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="372e9-146">Nie trzeba zmieniać niczego więcej w parametrach połączenia.</span><span class="sxs-lookup"><span data-stu-id="372e9-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="372e9-147">Zapisz wszystkie otwarte pliki.</span><span class="sxs-lookup"><span data-stu-id="372e9-147">Save all open files.</span></span>

<span data-ttu-id="372e9-148">Teraz powinno być gotowy do uruchomienia rozwiązania Contact Manager na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="372e9-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="372e9-149">Jeśli wykonujesz te kroki bez tworzenia bazy danych usług aplikacji ASP.NET utworzy bazę danych po raz pierwszy próbę utworzenia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="372e9-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="372e9-150">Jednak ręcznego tworzenia bazy danych zapewnia znacznie większą kontrolę nad zestawu funkcji usług aplikacji, które mają być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="372e9-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="372e9-151">**Aby uruchomić rozwiązanie Contact Manager**</span><span class="sxs-lookup"><span data-stu-id="372e9-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="372e9-152">In Visual Studio 2010, press F5.</span><span class="sxs-lookup"><span data-stu-id="372e9-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="372e9-153">Program Internet Explorer jest uruchamiany i żąda adres URL aplikacji, skontaktuj się z Menedżera ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="372e9-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="372e9-154">Domyślnie są wyświetlane w aplikacji **wszystkie kontakty** strony.</span><span class="sxs-lookup"><span data-stu-id="372e9-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="372e9-155">Dodaj kilka kontaktów, a następnie sprawdź, czy aplikacja działa zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="372e9-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="372e9-156">Przejdź do `http://localhost:50114/Account/Register` (Dostosuj adres URL, jeśli przechowujesz aplikacji na innym porcie).</span><span class="sxs-lookup"><span data-stu-id="372e9-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="372e9-157">Dodaj nazwę użytkownika, adres e-mail i hasło i sprawdź, czy możesz pomyślnie zarejestrować konto.</span><span class="sxs-lookup"><span data-stu-id="372e9-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="372e9-158">Przejdź do `http://localhost:50114/Account/LogOn` (Dostosuj adres URL, jeśli przechowujesz aplikacji na innym porcie).</span><span class="sxs-lookup"><span data-stu-id="372e9-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="372e9-159">Sprawdź, czy będziesz mieć możliwość logowania za pomocą utworzonego konta.</span><span class="sxs-lookup"><span data-stu-id="372e9-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="372e9-160">Zamknij program Internet Explorer, aby zatrzymać debugowanie.</span><span class="sxs-lookup"><span data-stu-id="372e9-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="372e9-161">Wniosek</span><span class="sxs-lookup"><span data-stu-id="372e9-161">Conclusion</span></span>

<span data-ttu-id="372e9-162">W tym momencie rozwiązanie kontaktów Menedżerze powinna być w pełni skonfigurowane do uruchomienia na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="372e9-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="372e9-163">Rozwiązanie służy jako odwołanie podczas pracy za pośrednictwem innych tematach w tej instrukcji.</span><span class="sxs-lookup"><span data-stu-id="372e9-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="372e9-164">Następnym temacie [opis pliku projektu](understanding-the-project-file.md), wyjaśniono, jak niestandardowe pliki projektu Microsoft kompilacji Engine (MSBuild) w ramach rozwiązania Contact Manager umożliwia kontrolować proces wdrażania.</span><span class="sxs-lookup"><span data-stu-id="372e9-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="372e9-165">[Poprzednie](the-contact-manager-solution.md)
> [dalej](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="372e9-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
