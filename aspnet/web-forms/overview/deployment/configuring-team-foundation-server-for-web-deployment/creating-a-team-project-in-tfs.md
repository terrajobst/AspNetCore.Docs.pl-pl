---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Tworzenie projektu zespołowego w TFS | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 2c3b30cac408f47d7d15ae7456f0744219506c85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397564"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="a37aa-103">Tworzenie projektu zespołowego w TFS</span><span class="sxs-lookup"><span data-stu-id="a37aa-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="a37aa-104">przez [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="a37aa-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="a37aa-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="a37aa-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="a37aa-106">W tym temacie opisano sposób tworzenia nowego projektu zespołowego w Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="a37aa-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="a37aa-107">Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a37aa-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="a37aa-108">Omówienie zadań</span><span class="sxs-lookup"><span data-stu-id="a37aa-108">Task Overview</span></span>

<span data-ttu-id="a37aa-109">Aby aprowizować i użyć nowego projektu zespołowego w programie TFS, należy wykonać następujące czynności wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="a37aa-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="a37aa-110">Przyznaj uprawnienia do użytkownika, który spowoduje utworzenie nowego projektu zespołowego.</span><span class="sxs-lookup"><span data-stu-id="a37aa-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="a37aa-111">Utwórz projekt zespołowy.</span><span class="sxs-lookup"><span data-stu-id="a37aa-111">Create the team project.</span></span>
- <span data-ttu-id="a37aa-112">Przyznaj uprawnienia do członków zespołu, którzy będą działać w projekcie.</span><span class="sxs-lookup"><span data-stu-id="a37aa-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="a37aa-113">Zaewidencjonuj odpowiednią zawartość.</span><span class="sxs-lookup"><span data-stu-id="a37aa-113">Check in some content.</span></span>

<span data-ttu-id="a37aa-114">W tym temacie opisano, jak do wykonania tych procedur, a następnie zidentyfikuje, użytkownikami i rolami zadania, które mogą być odpowiedzialne za każdej procedury.</span><span class="sxs-lookup"><span data-stu-id="a37aa-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="a37aa-115">Należy pamiętać, że w zależności od struktury organizacji, każdy z tych zadań może być odpowiedzialność innej osobie.</span><span class="sxs-lookup"><span data-stu-id="a37aa-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="a37aa-116">Zadania i wskazówki, w tym temacie założono, został zainstalowany i skonfigurowany TFS, i czy utworzono kolekcję projektów zespołowych w ramach procesu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="a37aa-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="a37aa-117">Aby uzyskać więcej informacji na temat tych założeń i bardziej ogólne informacje od scenariusza, zobacz [skonfigurować serwer kompilacji TFS dla wdrażania w Internecie](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="a37aa-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="a37aa-118">Udziel uprawnień twórcę projektu zespołowego</span><span class="sxs-lookup"><span data-stu-id="a37aa-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="a37aa-119">W celu utworzenia nowego projektu zespołowego, potrzebne są następujące uprawnienia:</span><span class="sxs-lookup"><span data-stu-id="a37aa-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="a37aa-120">Konieczne jest posiadanie **tworzyć nowe projekty** uprawnień w warstwie aplikacji TFS.</span><span class="sxs-lookup"><span data-stu-id="a37aa-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="a37aa-121">Uprawnienie to można przyznać zwykle przez dodanie użytkowników do **Administratorzy kolekcji projektów** grupy serwera TFS.</span><span class="sxs-lookup"><span data-stu-id="a37aa-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="a37aa-122">**Administratorzy Team Foundation** globalna grupa zawiera również to uprawnienie.</span><span class="sxs-lookup"><span data-stu-id="a37aa-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="a37aa-123">Musi mieć uprawnienia do tworzenia nowych witryn zespołu w kolekcji witryn programu SharePoint, która odpowiada kolekcji projektów zespołowych TFS.</span><span class="sxs-lookup"><span data-stu-id="a37aa-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="a37aa-124">Uprawnienie to można przyznać zwykle przez dodanie użytkownika do grupy programu SharePoint przy użyciu **Pełna kontrola** zbioru witryn prawa w programie SharePoint.</span><span class="sxs-lookup"><span data-stu-id="a37aa-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="a37aa-125">Jeśli używasz funkcji programu SQL Server Reporting Services, musisz być członkiem **Menedżer zawartości Team Foundation** roli w usługach Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="a37aa-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="a37aa-126">Osób wykonujących te procedury?</span><span class="sxs-lookup"><span data-stu-id="a37aa-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="a37aa-127">Zazwyczaj osoba lub grupa, która administruje wdrożenia serwera TFS oraz wykonuje te procedury.</span><span class="sxs-lookup"><span data-stu-id="a37aa-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="a37aa-128">Ponieważ jest to wysoko uprzywilejowane zestaw uprawnień, nowych projektów zespołowych są zwykle tworzone przez mały podzbiór użytkowników odpowiedzialnych za administrowanie wdrożenia programu TFS.</span><span class="sxs-lookup"><span data-stu-id="a37aa-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="a37aa-129">Deweloperzy nie zwykle uzyska uprawnienia wymagane do utworzenia nowych projektów zespołowych.</span><span class="sxs-lookup"><span data-stu-id="a37aa-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="a37aa-130">Udziel uprawnień w programie TFS</span><span class="sxs-lookup"><span data-stu-id="a37aa-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="a37aa-131">Jeśli chcesz umożliwić użytkownikowi tworzenie nowych projektów zespołowych, jest dodanie użytkownika do pierwszego zadania wysokiego poziomu **Administratorzy kolekcji projektów** grupy dla kolekcji projektów zespołowych.</span><span class="sxs-lookup"><span data-stu-id="a37aa-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="a37aa-132">**Aby dodać użytkownika do grupy Administratorzy kolekcji projektów**</span><span class="sxs-lookup"><span data-stu-id="a37aa-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="a37aa-133">Na serwerze TFS na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **Microsoft Team Foundation Server 2010**, a następnie kliknij przycisk **Team Foundation Konsola administracyjna**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="a37aa-134">W widoku drzewa nawigacji rozwiń węzeł **warstwy aplikacji**, a następnie kliknij przycisk **kolekcji projektów zespołowych**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="a37aa-135">W **kolekcji projektów zespołowych** wybierz opcję zespołu projektu zbierania, którymi chcesz zarządzać.</span><span class="sxs-lookup"><span data-stu-id="a37aa-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="a37aa-136">Na **ogólne** kliknij pozycję **członkostwa w grupie**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="a37aa-137">W **grup globalnych** okno dialogowe, wybierz opcję **Administratorzy kolekcji projektów** grupy, a następnie kliknij przycisk **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="a37aa-138">W **właściwości grupy programu Team Foundation Server** okno dialogowe, wybierz opcję **Windows użytkownik lub grupa**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="a37aa-139">W **Wybieranie: użytkownicy, komputery lub grupy** okno dialogowe, wpisz nazwę użytkownika, aby móc tworzyć nowych projektów zespołowych kliknij **Sprawdź nazwy**, a następnie kliknij przycisk **OK** .</span><span class="sxs-lookup"><span data-stu-id="a37aa-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="a37aa-140">W **właściwości grupy programu Team Foundation Server** okno dialogowe, kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="a37aa-141">W **grup globalnych** okno dialogowe, kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="a37aa-142">Udziel uprawnień w programie SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="a37aa-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="a37aa-143">Następnie należy przyznać uprawnienia użytkownika do tworzenia nowych witryn zespołu w kolekcji witryn programu SharePoint, który odpowiada kolekcji projektów zespołowych TFS.</span><span class="sxs-lookup"><span data-stu-id="a37aa-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="a37aa-144">**Aby przyznać uprawnienia Pełna kontrola zbioru witryn programu SharePoint**</span><span class="sxs-lookup"><span data-stu-id="a37aa-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="a37aa-145">W w konsoli administracyjnej Team Foundation Server na **kolekcji projektów zespołowych** wybierz kolekcji projektów zespołowych, którymi chcesz zarządzać.</span><span class="sxs-lookup"><span data-stu-id="a37aa-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="a37aa-146">Na **witryny programu SharePoint** kartę, zanotuj wartość ustawienia **bieżąca domyślna lokalizacja witryny** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a37aa-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="a37aa-147">Otwórz program Internet Explorer, a następnie przejdź do adresu URL zanotowaną w kroku 2.</span><span class="sxs-lookup"><span data-stu-id="a37aa-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a37aa-148">Jeśli użytkownik są nie zalogowany do Windows jako użytkownik, który utworzył kolekcji projektów zespołowych, należy zalogować się do programu SharePoint jako ten użytkownik. Aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="a37aa-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="a37aa-149">Na **Akcje witryny** menu, kliknij przycisk **ustawienia lokacji**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="a37aa-150">Na **ustawienia lokacji** w obszarze **użytkownikami i uprawnieniami**, kliknij przycisk **osoby i grupy**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="a37aa-151">W okienku nawigacji po lewej stronie kliknij **grup**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="a37aa-152">Na **osoby i grupy: wszystkie grupy** kliknij **Konfigurowanie grup dla tej witryny**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="a37aa-153">Może pojawić się <strong>HTTP 404 Nie znaleziono</strong> błąd z powodu podwójnego kodowania błędów HTTP.</span><span class="sxs-lookup"><span data-stu-id="a37aa-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="a37aa-154">W takim przypadku należy zastąpić adres URL to:</span><span class="sxs-lookup"><span data-stu-id="a37aa-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="a37aa-155">`[site_collection_URL]/_layouts/permsetup.aspx` Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a37aa-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="a37aa-156">Na **Konfigurowanie grup dla tej witryny** Dodaj użytkownika, który spowoduje utworzenie projektów zespołowych pod kątem **właścicieli** grupy, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="a37aa-157">Aby uzyskać więcej informacji na temat włączania użytkownikom na tworzenie nowych projektów zespołowych w obrębie kolekcji projektu zespołowego, zobacz [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="a37aa-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="a37aa-158">Tworzenie nowego projektu zespołowego i dodawanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="a37aa-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="a37aa-159">Gdy masz odpowiednie uprawnienia, możesz użyć **Team Explorer** okna w programie Visual Studio 2010 do utworzenia nowego projektu zespołowego.</span><span class="sxs-lookup"><span data-stu-id="a37aa-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="a37aa-160">To podejście oferuje bezpieczny kreatora, który zbiera wszystkie wymagane informacje i wykonuje niezbędne zadania w programie TFS, SharePoint i SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="a37aa-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="a37aa-161">Należy również przyznać uprawnienia nowego projektu zespołowego do członków zespołu deweloperów umożliwiające dodawanie i modyfikowanie zawartości.</span><span class="sxs-lookup"><span data-stu-id="a37aa-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="a37aa-162">Osób wykonujących te procedury?</span><span class="sxs-lookup"><span data-stu-id="a37aa-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="a37aa-163">Zazwyczaj administrator programu TFS lub lider zespołu deweloperów wykonuje te procedury.</span><span class="sxs-lookup"><span data-stu-id="a37aa-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="a37aa-164">Tworzenie nowego projektu zespołowego</span><span class="sxs-lookup"><span data-stu-id="a37aa-164">Create a New Team Project</span></span>

<span data-ttu-id="a37aa-165">W następnej procedurze opisano sposób tworzenia nowego projektu zespołowego w TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="a37aa-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="a37aa-166">**Aby utworzyć nowy projekt zespołowy**</span><span class="sxs-lookup"><span data-stu-id="a37aa-166">**To create a new team project**</span></span>

1. <span data-ttu-id="a37aa-167">Na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **Microsoft Visual Studio 2010**, kliknij prawym przyciskiem myszy **Microsoft Visual Studio 2010**, a następnie kliknij przycisk **Uruchom jako administrator**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a37aa-168">Jeśli nie uruchomisz programu Visual Studio 2010 jako administrator, Kreator nowego projektu zespołowego zakończy się niepowodzeniem w ostatnim kroku.</span><span class="sxs-lookup"><span data-stu-id="a37aa-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="a37aa-169">Jeśli **Kontrola konta użytkownika** pojawi się okno dialogowe, kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="a37aa-170">W programie Visual Studio na **zespołu** menu, kliknij przycisk **nawiązywanie połączenia z Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a37aa-171">Jeśli skonfigurowano już połączenie z serwerem TFS, można pominąć kroki 4 – 7.</span><span class="sxs-lookup"><span data-stu-id="a37aa-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="a37aa-172">W **połączenia z projektem zespołowym** okno dialogowe, kliknij przycisk **serwerów**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="a37aa-173">W **Dodaj/Usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="a37aa-174">W **Dodaj Team Foundation Server** okno dialogowe, podaj szczegóły wystąpienia programu TFS, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="a37aa-175">W **Dodaj/Usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="a37aa-176">W **Połącz z projektem zespołowym** okno dialogowe, wybierz wystąpienia programu TFS, aby nawiązać połączenie, wybierz zespół projektu kolekcji, które chcesz dodać do, a następnie kliknij przycisk **Connect**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="a37aa-177">W **Team Explorer** okna, kliknij prawym przyciskiem myszy kolekcji projektów zespołu, a następnie kliknij przycisk **nowego projektu zespołowego**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="a37aa-178">W **nowego projektu zespołowego** okno dialogowe, podaj nazwę i opis dla projektu zespołowego, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a37aa-179">Jeśli Twój projekt zespołowy zawiera spacje, może twarzy niektóre problemy, po dojściu do korzystania z narzędzia wdrażania Web Internet Information Services (IIS) (Web Deploy) do wdrażania pakietów ze ścieżki danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="a37aa-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="a37aa-180">Miejsca do magazynowania w ścieżce może utrudnić znacznie więcej poleceń narzędzia Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="a37aa-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="a37aa-181">Na **wybierz szablon procesu** wybierz szablon procesu, który zarządza procesem opracowywania, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a37aa-182">Aby uzyskać więcej informacji na temat szablonów procesów programu TFS, zobacz [szablonów procesów i narzędzi](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="a37aa-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="a37aa-183">Na **ustawienia witryny zespołu** strony, pozostaw ustawienia domyślne bez zmian, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="a37aa-184">To ustawienie tworzy lub identyfikuje zespołu witryny programu SharePoint, który jest skojarzony z projektem zespołowym TFS.</span><span class="sxs-lookup"><span data-stu-id="a37aa-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="a37aa-185">Twój zespół deweloperów można użyć tej lokacji do zarządzania dokumentacją, uczestniczą w wątkach dyskusji, tworzenie strony typu wiki i wykonywania różnych zadań, które nie są związane z kodem.</span><span class="sxs-lookup"><span data-stu-id="a37aa-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="a37aa-186">Aby uzyskać więcej informacji, zobacz [interakcje między produktów programu SharePoint i serwera Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="a37aa-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="a37aa-187">Na **Określ ustawienia kontroli źródła** strony, pozostaw ustawienia domyślne bez zmian, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="a37aa-188">To ustawienie określa lub tworzy lokalizacji w hierarchii folderów TFS, który będzie pełnił rolę folderu głównego dla zawartości.</span><span class="sxs-lookup"><span data-stu-id="a37aa-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="a37aa-189">Na **Potwierdź ustawienia projektu zespołowego** kliknij **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="a37aa-190">Gdy nowy projekt zespołowy pomyślnym utworzeniu na **utworzyć projektu zespołowego** kliknij **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="a37aa-191">Dodawanie użytkowników do projektu zespołowego</span><span class="sxs-lookup"><span data-stu-id="a37aa-191">Add Users to a Team Project</span></span>

<span data-ttu-id="a37aa-192">Teraz, po utworzeniu nowego projektu zespołowego, można przyznać uprawnienia do użytkowników w celu umożliwienia im rozpocząć dodawanie i współpracę nad zawartością.</span><span class="sxs-lookup"><span data-stu-id="a37aa-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="a37aa-193">**Aby dodać użytkowników do projektu zespołowego**</span><span class="sxs-lookup"><span data-stu-id="a37aa-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="a37aa-194">W programie Visual Studio 2010 w **Team Explorer** okna, kliknij prawym przyciskiem myszy projekt zespołowy, wskaż opcję **ustawienia projektu zespołowego**, a następnie kliknij przycisk **członkostwa w grupie**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="a37aa-195">Aby umożliwić użytkownikowi na dodawanie, modyfikowanie i usuwanie kodu pod kontrolą źródła, należy dodać go do **współautorzy** grupy.</span><span class="sxs-lookup"><span data-stu-id="a37aa-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="a37aa-196">W **grupy projektów** okno dialogowe, wybierz opcję **współautorzy** grupy, a następnie kliknij przycisk **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="a37aa-197">W **właściwości grupy programu Team Foundation Server** okno dialogowe, wybierz opcję **Windows użytkownik lub grupa**, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="a37aa-198">W **Wybieranie: użytkownicy, komputery lub grupy** okno dialogowe, wpisz nazwę użytkownika, które chcesz dodać do projektu zespołowego, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="a37aa-199">W **właściwości grupy programu Team Foundation Server** okno dialogowe, kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="a37aa-200">W **grupy projektów** okno dialogowe, kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="a37aa-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a37aa-201">Wniosek</span><span class="sxs-lookup"><span data-stu-id="a37aa-201">Conclusion</span></span>

<span data-ttu-id="a37aa-202">W tym momencie nowego projektu zespołowego jest gotowy do użycia i Twojego zespołu deweloperów rozpocząć dodawanie zawartości i współpracę w procesie tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a37aa-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="a37aa-203">Następny temat [dodawania zawartości do kontroli źródła](adding-content-to-source-control.md), w tym artykule opisano sposób dodawania zawartości do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="a37aa-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="a37aa-204">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="a37aa-204">Further Reading</span></span>

<span data-ttu-id="a37aa-205">Szerszy wskazówki dotyczące tworzenia projektów zespołowych w programie TFS, zobacz [tworzenia projektu zespołowego](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="a37aa-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="a37aa-206">Aby uzyskać więcej informacji na temat włączania użytkownikom na tworzenie nowych projektów zespołowych w obrębie kolekcji projektu zespołowego, zobacz [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="a37aa-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="a37aa-207">Aby uzyskać więcej informacji na temat dodawania użytkowników do zespołów i projektów, zobacz [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="a37aa-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a37aa-208">[Poprzednie](configuring-team-foundation-server-for-web-deployment.md)
> [dalej](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="a37aa-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
