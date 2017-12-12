---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: "Tworzenie nowego projektu zespołowego w programie TFS | Dokumentacja firmy Microsoft"
author: jrjlee
description: "W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 4cb0d72330086ecb8cd9e6fb70ce0a57698dda5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="abd2d-103">Tworzenie nowego projektu zespołowego w programie TFS</span><span class="sxs-lookup"><span data-stu-id="abd2d-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="abd2d-104">przez [Lewandowski Jason](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="abd2d-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="abd2d-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="abd2d-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="abd2d-106">W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="abd2d-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="abd2d-107">Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Ten samouczek serii używa przykładowe rozwiązanie & #x 2014; [rozwiązania z menedżerem skontaktuj się z](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, systemu Windows Usługi Communication Foundation (WCF), a projekt bazy danych.</span><span class="sxs-lookup"><span data-stu-id="abd2d-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="abd2d-108">Omówienie zadań</span><span class="sxs-lookup"><span data-stu-id="abd2d-108">Task Overview</span></span>

<span data-ttu-id="abd2d-109">Do udostępniania i użyć nowego projektu zespołowego w programie TFS, musisz wykonać następujące ogólne kroki:</span><span class="sxs-lookup"><span data-stu-id="abd2d-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="abd2d-110">Przyznaj uprawnienia do użytkownika, który spowoduje utworzenie nowego projektu zespołowego.</span><span class="sxs-lookup"><span data-stu-id="abd2d-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="abd2d-111">Tworzenie projektu zespołowego.</span><span class="sxs-lookup"><span data-stu-id="abd2d-111">Create the team project.</span></span>
- <span data-ttu-id="abd2d-112">Przyznaj uprawnienia do członków zespołu, którzy będą pracować w projekcie.</span><span class="sxs-lookup"><span data-stu-id="abd2d-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="abd2d-113">Sprawdź w części zawartości.</span><span class="sxs-lookup"><span data-stu-id="abd2d-113">Check in some content.</span></span>

<span data-ttu-id="abd2d-114">W tym temacie opisano, jak do wykonania tych procedur i zidentyfikuje użytkownicy i role zadania, które mogą być odpowiedzialne za każdej procedury.</span><span class="sxs-lookup"><span data-stu-id="abd2d-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="abd2d-115">Należy pamiętać, że, w zależności od struktury organizacji każdego z tych zadań mogą być odpowiedzialność inną osobę.</span><span class="sxs-lookup"><span data-stu-id="abd2d-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="abd2d-116">Zadania i wskazówki, w tym temacie założono, czy zostały zainstalowane i skonfigurowane TFS i utworzono kolekcję projektów zespołowych w trakcie procesu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="abd2d-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="abd2d-117">Aby uzyskać więcej informacji na temat tych założeń i bardziej ogólne informacje od scenariusza, zobacz [Konfiguracja kompilacji serwera TFS dla Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="abd2d-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="abd2d-118">Udzielanie uprawnień osobom Twórca projektu zespołowego</span><span class="sxs-lookup"><span data-stu-id="abd2d-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="abd2d-119">Aby było możliwe utworzenie nowego projektu zespołowego, potrzebne są następujące uprawnienia:</span><span class="sxs-lookup"><span data-stu-id="abd2d-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="abd2d-120">Musi mieć **tworzenie nowych projektów** uprawnień na warstwie aplikacji TFS.</span><span class="sxs-lookup"><span data-stu-id="abd2d-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="abd2d-121">Uprawnienie to można przyznać zwykle przez dodanie użytkowników do **Administratorzy kolekcji projektów** grupy TFS.</span><span class="sxs-lookup"><span data-stu-id="abd2d-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="abd2d-122">**Team Foundation Administratorzy** globalna grupa zawiera również to uprawnienie.</span><span class="sxs-lookup"><span data-stu-id="abd2d-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="abd2d-123">Musi mieć uprawnienia do tworzenia nowej witryny zespołu w zbiorze witryn programu SharePoint, odpowiadającej kolekcji projektów zespołowych TFS.</span><span class="sxs-lookup"><span data-stu-id="abd2d-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="abd2d-124">Uprawnienie to można przyznać zwykle przez dodanie użytkownika do grupy programu SharePoint z **Pełna kontrola** zbioru witryn prawa w programie SharePoint.</span><span class="sxs-lookup"><span data-stu-id="abd2d-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="abd2d-125">Jeśli używasz funkcji programu SQL Server Reporting Services, musi być członkiem **Team Foundation Content Manager** w usługach Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="abd2d-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="abd2d-126">Kto wykonuje te procedury?</span><span class="sxs-lookup"><span data-stu-id="abd2d-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="abd2d-127">Zazwyczaj osoba lub grupa, która zarządza wdrożenia TFS wykonuje także tych procedur.</span><span class="sxs-lookup"><span data-stu-id="abd2d-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="abd2d-128">Ponieważ jest to wysoko uprzywilejowane zestaw uprawnień, nowych projektów zespołowych są zazwyczaj tworzone przez mały podzbiór użytkowników odpowiedzialnego za administrowanie wdrożenia TFS.</span><span class="sxs-lookup"><span data-stu-id="abd2d-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="abd2d-129">Deweloperzy będą nie zwykle można udzielić wymaganych uprawnień do tworzenia nowych projektów zespołowych.</span><span class="sxs-lookup"><span data-stu-id="abd2d-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="abd2d-130">Udzielanie uprawnień w programie TFS</span><span class="sxs-lookup"><span data-stu-id="abd2d-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="abd2d-131">Jeśli chcesz umożliwić użytkownikom tworzenie nowych projektów zespołowych, pierwszego zadania wysokiego poziomu jest dodanie użytkownika do **Administratorzy kolekcji projektów** grupy dla kolekcji projektów zespołowych.</span><span class="sxs-lookup"><span data-stu-id="abd2d-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="abd2d-132">**Aby dodać użytkownika do grupy Administratorzy kolekcji projektów**</span><span class="sxs-lookup"><span data-stu-id="abd2d-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="abd2d-133">Na serwerze TFS na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **Microsoft Team Foundation Server 2010**, a następnie kliknij przycisk **Team Foundation Konsola administracyjna**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="abd2d-134">W widoku drzewa nawigacji rozwiń węzeł **warstwy aplikacji**, a następnie kliknij przycisk **kolekcje projektów zespołowych**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="abd2d-135">W **kolekcje projektów zespołowych** okienku, wybierz opcję chcesz zarządzać kolekcji projektów zespołowych.</span><span class="sxs-lookup"><span data-stu-id="abd2d-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="abd2d-136">Na **ogólne** , kliknij pozycję **członkostwa w grupie**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="abd2d-137">W **grupę globalną** okno dialogowe, wybierz opcję **Administratorzy kolekcji projektów** grupy, a następnie kliknij przycisk **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="abd2d-138">W **Team Foundation Server grupy właściwości** okno dialogowe, wybierz opcję **użytkownika systemu Windows lub grupy**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="abd2d-139">W **Wybieranie: użytkownicy, komputery lub grupy** okno dialogowe, wpisz nazwę użytkownika, aby można było tworzyć nowe projekty zespołowe, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="abd2d-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="abd2d-140">W **Team Foundation Server grupy właściwości** okno dialogowe, kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="abd2d-141">W **grupę globalną** okno dialogowe, kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="abd2d-142">Udzielanie uprawnień w programie SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="abd2d-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="abd2d-143">Następnie należy przyznać uprawnienia użytkownika do tworzenia nowych lokacji zespołu w kolekcji witryn programu SharePoint, która odnosi się do kolekcji projektów zespołowych TFS.</span><span class="sxs-lookup"><span data-stu-id="abd2d-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="abd2d-144">**Aby przyznać uprawnienia Pełna kontrola do zbioru witryn programu SharePoint**</span><span class="sxs-lookup"><span data-stu-id="abd2d-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="abd2d-145">W konsoli administracyjnej Team Foundation Server na **kolekcje projektów zespołowych** wybierz kolekcji projektów zespołowych, którymi chcesz zarządzać.</span><span class="sxs-lookup"><span data-stu-id="abd2d-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="abd2d-146">Na **witryny programu SharePoint** karcie, zanotuj wartość ustawienia **bieżącej lokalizacji domyślnej witryny** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="abd2d-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="abd2d-147">Otwórz program Internet Explorer, a następnie przejdź do adresu URL zanotowaną w kroku 2.</span><span class="sxs-lookup"><span data-stu-id="abd2d-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="abd2d-148">Jeśli nie zalogowaniu się do systemu Windows jako użytkownik, który utworzył kolekcji projektów zespołowych, należy zalogować się do programu SharePoint jako ten użytkownik. Aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="abd2d-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="abd2d-149">Na **Akcje witryny** menu, kliknij przycisk **ustawienia lokacji**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="abd2d-150">Na **ustawienia lokacji** w obszarze **uprawnienia użytkowników i**, kliknij przycisk **osoby i grupy**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="abd2d-151">W okienku nawigacji po lewej stronie kliknij **grup**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="abd2d-152">Na **osoby i grupy: wszystkie grupy** kliknij przycisk **Konfigurowanie grup w tej witrynie**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

    > [!NOTE]
    > <span data-ttu-id="abd2d-153">Może pojawić się **HTTP 404 — Nie znaleziono** błąd z powodu podwójnego usterki kodowania protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="abd2d-153">You may receive an **HTTP 404 Not Found** error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="abd2d-154">W takim przypadku należy zastąpić adres URL to:</span><span class="sxs-lookup"><span data-stu-id="abd2d-154">If this occurs, replace the URL with this:</span></span>   
    > <span data-ttu-id="abd2d-155">[*adres URL zbioru witryn*] /\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="abd2d-155">[*site collection URL*]/\_layouts/permsetup.aspx</span></span>  
    > <span data-ttu-id="abd2d-156">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="abd2d-156">For example:</span></span>  
    > <span data-ttu-id="abd2d-157">http://TFS/Sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="abd2d-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span></span>
8. <span data-ttu-id="abd2d-158">Na **Konfigurowanie grup w tej witrynie** strony, Dodaj użytkownika, który spowoduje utworzenie projektów zespołowych do **właścicieli** grupy, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-158">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="abd2d-159">Aby uzyskać więcej informacji na temat włączania użytkownikom na tworzenie nowych projektów zespołowych w kolekcji projektów zespołowych, zobacz [Ustawianie uprawnień administratora dla kolekcji projektu zespołowego](https://msdn.microsoft.com/en-us/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="abd2d-159">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/en-us/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="abd2d-160">Tworzenie nowego projektu zespołowego i dodawanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="abd2d-160">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="abd2d-161">Gdy masz wystarczające uprawnienia, możesz użyć **Team Explorer** okna w Visual Studio 2010 do utworzenia nowego projektu zespołowego.</span><span class="sxs-lookup"><span data-stu-id="abd2d-161">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="abd2d-162">Takie podejście udostępnia kreatora, który zbiera wszystkie wymagane informacje i wykonuje niezbędnych zadań w programie TFS, SharePoint i SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="abd2d-162">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="abd2d-163">Ponadto należy udzielić uprawnień do nowego projektu zespołowego, aby członkowie zespołu deweloperów umożliwiające dodawanie i modyfikowanie zawartości.</span><span class="sxs-lookup"><span data-stu-id="abd2d-163">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="abd2d-164">Kto wykonuje te procedury?</span><span class="sxs-lookup"><span data-stu-id="abd2d-164">Who Performs These Procedures?</span></span>

<span data-ttu-id="abd2d-165">Zazwyczaj administratora TFS lub kierownik zespołu deweloperów wykonuje te procedury.</span><span class="sxs-lookup"><span data-stu-id="abd2d-165">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="abd2d-166">Tworzenie nowego projektu zespołowego</span><span class="sxs-lookup"><span data-stu-id="abd2d-166">Create a New Team Project</span></span>

<span data-ttu-id="abd2d-167">W następnej procedurze opisano sposób tworzenia nowego projektu zespołowego w TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="abd2d-167">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="abd2d-168">**Do utworzenia nowego projektu zespołowego**</span><span class="sxs-lookup"><span data-stu-id="abd2d-168">**To create a new team project**</span></span>

1. <span data-ttu-id="abd2d-169">Na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **programu Microsoft Visual Studio 2010**, kliknij prawym przyciskiem myszy **programu Microsoft Visual Studio 2010**, a następnie kliknij przycisk **Uruchom jako administrator**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-169">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="abd2d-170">Jeżeli nie uruchomisz programu Visual Studio 2010 jako administrator, Kreator nowego projektu zespołowego zakończy się niepowodzeniem w ostatnim kroku.</span><span class="sxs-lookup"><span data-stu-id="abd2d-170">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="abd2d-171">Jeśli **Kontrola konta użytkownika** zostanie wyświetlone okno dialogowe, kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-171">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="abd2d-172">W programie Visual Studio na **zespołu** menu, kliknij przycisk **nawiązywanie połączenia z Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-172">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="abd2d-173">Jeśli skonfigurowano już połączenie z serwerem TFS, można pominąć kroki od 4 do 7.</span><span class="sxs-lookup"><span data-stu-id="abd2d-173">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="abd2d-174">W **połączenia z projektem zespołowym** okno dialogowe, kliknij przycisk **serwerów**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-174">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="abd2d-175">W **Dodaj lub usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-175">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="abd2d-176">W **Dodaj Team Foundation Server** okno dialogowe, podaj szczegóły wystąpienia TFS, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-176">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="abd2d-177">W **Dodaj lub usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-177">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="abd2d-178">W **Połącz z projektem zespołowym** okno dialogowe, wybierz wystąpienia TFS ma się połączyć, wybierz zespół projektu kolekcji, które chcesz dodać do, a następnie kliknij przycisk **Connect**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-178">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="abd2d-179">W **Team Explorer** okna, kliknij prawym przyciskiem myszy zespołu kolekcji projektów, a następnie kliknij przycisk **nowego projektu zespołowego**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-179">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="abd2d-180">W **nowego projektu zespołowego** okno dialogowe, podaj nazwę i opis dla projektu zespołowego, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-180">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="abd2d-181">Jeśli projekt zawiera spacje, po dojściu do użycia usług Internet Information Services (IIS) Narzędzie wdrażania Web (Web Deploy) do wdrożenia pakietu w ścieżce danych wyjściowych może stają przed problemy.</span><span class="sxs-lookup"><span data-stu-id="abd2d-181">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="abd2d-182">Spacje w ścieżce może utrudnić znacznie więcej do uruchomienia poleceń narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="abd2d-182">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="abd2d-183">Na **wybierz szablon procesu** wybierz szablon procesu, który ma być używany do zarządzania procesem tworzenia, a następnie kliknij pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-183">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="abd2d-184">Aby uzyskać więcej informacji dotyczących szablonów procesów dla serwera TFS, zobacz [szablonów procesów i narzędzi](https://msdn.microsoft.com/en-us/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="abd2d-184">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/en-us/vstudio/aa718795).</span></span>
12. <span data-ttu-id="abd2d-185">Na **ustawienia witryny zespołu** , pozostaw domyślne ustawienie bez zmian, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-185">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="abd2d-186">To ustawienie powoduje utworzenie lub identyfikuje witryna zespołu programu SharePoint, który jest skojarzony z projektem zespołowym TFS.</span><span class="sxs-lookup"><span data-stu-id="abd2d-186">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="abd2d-187">Zespół deweloperów można użyć tej lokacji do zarządzania dokumentacji, uczestniczyć w wątkach na dyskusję, tworzenie stron typu wiki i wykonywania różnych zadań, które nie są związane z kodem.</span><span class="sxs-lookup"><span data-stu-id="abd2d-187">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="abd2d-188">Aby uzyskać więcej informacji, zobacz [interakcje między produktów programu SharePoint i serwera Team Foundation Server](https://msdn.microsoft.com/en-us/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="abd2d-188">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/en-us/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="abd2d-189">Na **Określ ustawienia kontroli źródła** , pozostaw domyślne ustawienie bez zmian, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-189">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="abd2d-190">To ustawienie określa lub tworzy lokalizacji w hierarchii folderów TFS, który będzie pełnił rolę folderu głównego dla zawartości.</span><span class="sxs-lookup"><span data-stu-id="abd2d-190">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="abd2d-191">Na **Potwierdź ustawienia projektu zespołowego** kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-191">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="abd2d-192">Gdy nowego projektu zespołowego została pomyślnie utworzona, na **utworzyć projektu zespołowego** kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-192">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="abd2d-193">Dodawanie użytkowników do projektu zespołowego</span><span class="sxs-lookup"><span data-stu-id="abd2d-193">Add Users to a Team Project</span></span>

<span data-ttu-id="abd2d-194">Teraz, po utworzeniu nowego projektu zespołowego, można przyznać uprawnień użytkowników, aby włączyć je, aby rozpocząć dodawanie i współpracy nad zawartości.</span><span class="sxs-lookup"><span data-stu-id="abd2d-194">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="abd2d-195">**Aby dodać użytkowników do projektu zespołowego**</span><span class="sxs-lookup"><span data-stu-id="abd2d-195">**To add users to a team project**</span></span>

1. <span data-ttu-id="abd2d-196">W programie Visual Studio 2010 w **Team Explorer** okna, kliknij prawym przyciskiem myszy projektu zespołowego, wskaż pozycję **ustawienia projektu zespołowego**, a następnie kliknij przycisk **członkostwa w grupie**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-196">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="abd2d-197">Aby umożliwić użytkownikowi na dodawanie, modyfikowanie i usuwanie kodu pod kontrolą źródła, należy dodać go do **współautorzy** grupy.</span><span class="sxs-lookup"><span data-stu-id="abd2d-197">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="abd2d-198">W **grupy projektów** okno dialogowe, wybierz opcję **współautorzy** grupy, a następnie kliknij przycisk **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-198">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="abd2d-199">W **Team Foundation Server grupy właściwości** okno dialogowe, wybierz opcję **użytkownika systemu Windows lub grupy**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-199">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="abd2d-200">W **Wybieranie: użytkownicy, komputery lub grupy** okno dialogowe, wpisz nazwę użytkownika, które chcesz dodać do projektu zespołowego, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-200">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="abd2d-201">W **Team Foundation Server grupy właściwości** okno dialogowe, kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-201">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="abd2d-202">W **grupy projektów** okno dialogowe, kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="abd2d-202">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="abd2d-203">Wniosek</span><span class="sxs-lookup"><span data-stu-id="abd2d-203">Conclusion</span></span>

<span data-ttu-id="abd2d-204">W tym momencie jest gotowe do użycia nowego projektu zespołowego, a zespół deweloperów można rozpocząć dodawanie zawartości i współpracy nad procesem tworzenia.</span><span class="sxs-lookup"><span data-stu-id="abd2d-204">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="abd2d-205">Następnym temacie [Dodawanie zawartości do kontroli źródła](adding-content-to-source-control.md), w tym artykule opisano sposób dodawania zawartości do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="abd2d-205">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="abd2d-206">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="abd2d-206">Further Reading</span></span>

<span data-ttu-id="abd2d-207">Szerszych wskazówki dotyczące tworzenia projektów zespołowych w programie TFS, zobacz [utworzenia projektu zespołowego](https://msdn.microsoft.com/en-us/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="abd2d-207">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/en-us/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="abd2d-208">Aby uzyskać więcej informacji na temat włączania użytkownikom na tworzenie nowych projektów zespołowych w kolekcji projektów zespołowych, zobacz [Ustawianie uprawnień administratora dla kolekcji projektu zespołowego](https://msdn.microsoft.com/en-us/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="abd2d-208">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/en-us/library/dd547204.aspx).</span></span> <span data-ttu-id="abd2d-209">Aby uzyskać więcej informacji na temat dodawania użytkowników do zespołów i projektów, zobacz [Dodawanie użytkowników do zespołów i projektów](https://msdn.microsoft.com/en-us/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="abd2d-209">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/en-us/library/bb558971.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="abd2d-210">[Poprzednie](configuring-team-foundation-server-for-web-deployment.md)
[dalej](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="abd2d-210">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
