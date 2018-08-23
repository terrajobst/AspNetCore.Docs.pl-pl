---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Dodawanie zawartości do kontroli źródła | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób dodawania zawartości do kontroli źródła w Team Foundation Server (TFS) 2010. Opisano w nim sposób dodawania rozwiązań i projektów do projek zespołu...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 2119705a75d0717d05d4a7db69b3f5d38b1cdd45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753283"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="e6b60-104">Dodawanie zawartości do kontroli źródła</span><span class="sxs-lookup"><span data-stu-id="e6b60-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="e6b60-105">przez [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="e6b60-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="e6b60-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="e6b60-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="e6b60-107">W tym temacie opisano sposób dodawania zawartości do kontroli źródła w Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="e6b60-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="e6b60-108">Opisano w nim sposób dodawania rozwiązań i projektów do projektu zespołowego w programie TFS, i wyjaśnia, jak dodać zależności zewnętrznych, takich jak platform ani zestawów do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="e6b60-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="e6b60-109">Ten temat jest częścią serii samouczków na podstawie wymagania dotyczące wdrażania enterprise fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Przykładowe rozwiązanie korzysta z tej serii samouczków&#x2014; [rozwiązania Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;do reprezentowania aplikacji sieci web przy użyciu realistycznej stopień złożoności, łącznie z aplikacją ASP.NET MVC 3 komunikacji Windows Usługa Foundation (WCF), a projekt bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e6b60-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="e6b60-110">Omówienie zadań</span><span class="sxs-lookup"><span data-stu-id="e6b60-110">Task Overview</span></span>

<span data-ttu-id="e6b60-111">W większości przypadków można dodać zawartości do kontroli źródła należy każdy członek zespołu deweloperów.</span><span class="sxs-lookup"><span data-stu-id="e6b60-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="e6b60-112">Aby dodać rozwiązanie do kontroli źródła w programie TFS, należy wykonać następujące czynności wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="e6b60-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="e6b60-113">Połącz z projektem zespołowym.</span><span class="sxs-lookup"><span data-stu-id="e6b60-113">Connect to a team project.</span></span>
- <span data-ttu-id="e6b60-114">Struktury folderów projektu zespołowego na serwerze są mapowane na strukturę folderów na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="e6b60-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="e6b60-115">Dodaj rozwiązanie i jego zawartość do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="e6b60-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="e6b60-116">Dodawanie wszelkich zależności zewnętrznych do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="e6b60-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="e6b60-117">W tym temacie pokazują sposób wykonania tych procedur.</span><span class="sxs-lookup"><span data-stu-id="e6b60-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="e6b60-118">Zadania i wskazówki, w tym temacie założono, że utworzono już nowego projektu zespołowego TFS można zarządzać zawartością.</span><span class="sxs-lookup"><span data-stu-id="e6b60-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="e6b60-119">Aby uzyskać więcej informacji na temat tworzenia nowego projektu zespołowego, zobacz [tworzenia projektu zespołowego w programie TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="e6b60-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="e6b60-120">Osób wykonujących te procedury?</span><span class="sxs-lookup"><span data-stu-id="e6b60-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="e6b60-121">W większości przypadków należy dodać i modyfikować zawartości w ramach projektów zespołowych określonych każdy członek zespołu deweloperów.</span><span class="sxs-lookup"><span data-stu-id="e6b60-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="e6b60-122">Łączenie z projektem zespołowym i Tworzenie mapowania folderu</span><span class="sxs-lookup"><span data-stu-id="e6b60-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="e6b60-123">Przed dodaniem żadnej zawartości do kontroli źródła, należy połączyć z projektem zespołowym i utworzyć mapowania między struktury folderów na serwerze i system plików na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="e6b60-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="e6b60-124">**Aby nawiązać połączenie z projektem zespołowym i Mapuj ścieżkę lokalną**</span><span class="sxs-lookup"><span data-stu-id="e6b60-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="e6b60-125">Na stacji roboczej dewelopera Otwórz program Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e6b60-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="e6b60-126">W programie Visual Studio na **zespołu** menu, kliknij przycisk **nawiązywanie połączenia z Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e6b60-127">Jeśli skonfigurowano już połączenie z serwerem TFS, można pominąć kroki od 3 do 6.</span><span class="sxs-lookup"><span data-stu-id="e6b60-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="e6b60-128">W **połączenia z projektem zespołowym** okno dialogowe, kliknij przycisk **serwerów**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="e6b60-129">W **Dodaj/Usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="e6b60-130">W **Dodaj Team Foundation Server** okno dialogowe, podaj szczegóły wystąpienia programu TFS, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="e6b60-131">W **Dodaj/Usuń Team Foundation Server** okno dialogowe, kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="e6b60-132">W **Połącz z projektem zespołowym** okno dialogowe, wybierz wystąpienia programu TFS, aby nawiązać połączenie, wybierz zespół projektu, kolekcji, wybierz projekt zespołowy, do którego chcesz dodać do, a następnie kliknij **Connect**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="e6b60-133">W **Team Explorer** , rozwiń węzeł projektu zespołowego, a następnie kliknij dwukrotnie ikonę **kontroli źródła**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="e6b60-134">Na **Eksploratora kontroli źródła** kliknij pozycję **niezamapowane**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="e6b60-135">W **mapy** dialogowym **lokalny folder** pola, przejdź do (lub Utwórz) folder lokalny do działania jako folder główny projektu zespołowego, a następnie kliknij przycisk **mapy**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="e6b60-136">Po wyświetleniu monitu, aby pobrać pliki źródłowe, kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="e6b60-137">W tym momencie zmapowano folderu po stronie serwera dla projektu zespołowego do folderu lokalnego na stacji roboczej dewelopera.</span><span class="sxs-lookup"><span data-stu-id="e6b60-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="e6b60-138">Również pobraniu istniejącą zawartość z projektu zespołowego na strukturze folderów lokalnych.</span><span class="sxs-lookup"><span data-stu-id="e6b60-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="e6b60-139">Możesz teraz rozpocząć dodawanie własnych zawartości do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="e6b60-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="e6b60-140">Dodaj projekty i rozwiązania do kontroli źródła</span><span class="sxs-lookup"><span data-stu-id="e6b60-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="e6b60-141">Aby dodać projekty i rozwiązania do kontroli źródła, należy najpierw przenieść je do zamapowany folder dla projektu zespołowego na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="e6b60-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="e6b60-142">Następnie można sprawdzić w zawartości do synchronizowania swoje dodatki z serwerem.</span><span class="sxs-lookup"><span data-stu-id="e6b60-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="e6b60-143">**Aby dodać projekty do kontroli źródła**</span><span class="sxs-lookup"><span data-stu-id="e6b60-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="e6b60-144">Na stacji roboczej dewelopera należy przenieść swoje projekty i rozwiązania do odpowiedniej lokalizacji w ramach struktury zamapowany folder dla projektu zespołowego.</span><span class="sxs-lookup"><span data-stu-id="e6b60-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e6b60-145">Wiele organizacji ma preferowanym sposobem sposób organizowania projektów i rozwiązań w kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="e6b60-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="e6b60-146">Aby uzyskać wskazówki dotyczące struktury folderów, zobacz [How to: struktura Your źródłowych folderów kontroli w Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6b60-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="e6b60-147">Otwórz rozwiązanie w programie Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e6b60-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="e6b60-148">W **Eksploratora rozwiązań** , kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **Dodaj rozwiązanie do kontroli źródła**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="e6b60-149">W niektórych przypadkach, w zależności od tego, jak Twoja organizacja lubi struktury zawartości w programie TFS może być konieczne dodanie projekty do kontroli źródła osobno, aby zapewnić bardziej precyzyjną kontrolę nad sposób organizowania kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="e6b60-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="e6b60-150">Upewnij się, że **Eksploratora kontroli źródła** karty wyświetla zawartość zostały dodane w ramach struktury folderów serwera dla projektu zespołowego.</span><span class="sxs-lookup"><span data-stu-id="e6b60-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="e6b60-151">**Eksploratora kontroli źródła** karty wyświetla zawartość przy użyciu żadne dalsze monity nie ponieważ rozwiązanie jest dodawane do zamapowany folder w lokalnym systemie plików.</span><span class="sxs-lookup"><span data-stu-id="e6b60-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="e6b60-152">Jeśli Twoje rozwiązanie w niezmapowanej lokalizacji, użytkownik jest proszony o Określ lokalizacje folderów w TFS i lokalnego systemu plików.</span><span class="sxs-lookup"><span data-stu-id="e6b60-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="e6b60-153">Na **Eksploratora kontroli źródła** na karcie **folderów** okienku kliknij prawym przyciskiem myszy projektu zespołowego (na przykład **ContactManager**), a następnie kliknij przycisk **Zaewidencjonuj Oczekujące zmiany**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="e6b60-154">W **Zaewidencjonuj — pliki źródłowe** okno dialogowe, wpisz komentarz, a następnie kliknij przycisk **Zaewidencjonuj**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="e6b60-155">W tym momencie dodano rozwiązania do kontroli źródła w programie TFS.</span><span class="sxs-lookup"><span data-stu-id="e6b60-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="e6b60-156">Dodaj zależności zewnętrznych do kontroli źródła</span><span class="sxs-lookup"><span data-stu-id="e6b60-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="e6b60-157">Po dodaniu projektu lub rozwiązania do kontroli źródła, wszystkie pliki i foldery znajdujące się w projekcie lub rozwiązaniu także zostaną dodane.</span><span class="sxs-lookup"><span data-stu-id="e6b60-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="e6b60-158">Jednak w wielu przypadkach, projekty i rozwiązania polegają również na zależności zewnętrzne, np. lokalnych zestawów, aby działać prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="e6b60-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="e6b60-159">Musisz dodać takich zasobów, do kontroli źródła, aby umożliwić kompilacji zespołowej i inni członkowie zespołu deweloperów pomyślnie kompilacji kodu.</span><span class="sxs-lookup"><span data-stu-id="e6b60-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="e6b60-160">Na przykład struktura folderów dla Contact Manager przykładowe rozwiązanie zawiera folder o nazwie pakietów.</span><span class="sxs-lookup"><span data-stu-id="e6b60-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="e6b60-161">Zawiera zestaw i różne zasoby pomocnicze dla ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="e6b60-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="e6b60-162">Packages folder nie jest częścią rozwiązania Contact Manager, ale to rozwiązanie nie zostanie pomyślnie skompilowany bez niego.</span><span class="sxs-lookup"><span data-stu-id="e6b60-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="e6b60-163">Aby włączyć Team Build skompilować rozwiązanie, należy dodać packages folder do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="e6b60-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="e6b60-164">Włączenie packages folder jest typowy dla co się stanie po dodaniu Entity Framework lub podobne zasoby do rozwiązania przy użyciu rozszerzenia NuGet dla programu Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e6b60-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="e6b60-165">**Aby dodać zawartość — projekt do kontroli źródła**</span><span class="sxs-lookup"><span data-stu-id="e6b60-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="e6b60-166">Upewnij się, że elementy, które chcesz dodać (np. packages folder) znajdują się w odpowiedniej lokalizacji w ramach zamapowany folder w lokalnym systemie plików.</span><span class="sxs-lookup"><span data-stu-id="e6b60-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="e6b60-167">W programie Visual Studio 2010 w **Team Explorer** , rozwiń węzeł projektu zespołowego, a następnie kliknij dwukrotnie ikonę **kontroli źródła**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="e6b60-168">Na **Eksploratora kontroli źródła** na karcie **folderów** okienku, wybierz folder, który zawiera element lub elementy użytkownik ma zostać dodana.</span><span class="sxs-lookup"><span data-stu-id="e6b60-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="e6b60-169">Kliknij przycisk **Dodaj elementy do folderu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="e6b60-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="e6b60-170">W **Dodaj do kontroli źródła** okna dialogowego Wybierz folder lub elementy, które chcesz dodać, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="e6b60-171">Na **wykluczone elementy** , a następnie wybierz wszystkie wymagane elementy, które zostały automatycznie wyłączone (na przykład zestawy), a następnie kliknij przycisk **Uwzględnij elementy**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="e6b60-172">Na **elementy do dodania** kartę, upewnij się, że są wyświetlane wszystkie pliki, które chcesz dołączyć, a następnie kliknij **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="e6b60-173">W **Eksploratora kontroli źródła** okna, kliknij przycisk **Zaewidencjonuj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="e6b60-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="e6b60-174">W **Zaewidencjonuj — pliki źródłowe** okno dialogowe, wpisz komentarz, a następnie kliknij przycisk **Zaewidencjonuj**.</span><span class="sxs-lookup"><span data-stu-id="e6b60-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="e6b60-175">W tym momencie dodano zależności zewnętrznych dla Twojego rozwiązania do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="e6b60-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e6b60-176">Wniosek</span><span class="sxs-lookup"><span data-stu-id="e6b60-176">Conclusion</span></span>

<span data-ttu-id="e6b60-177">W tym temacie opisano, jak nawiązać połączenie z projektem zespołowym, mapowanie strukturę folderów i dodawanie zawartości do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="e6b60-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="e6b60-178">Aby uzyskać więcej informacji na temat pracy z elementami pod kontrolą źródła, zobacz [przy użyciu systemu kontroli wersji](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6b60-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="e6b60-179">Następny temat [Konfigurowanie serwera kompilacji TFS dla wdrażania w Internecie](configuring-a-tfs-build-server-for-web-deployment.md), w tym artykule opisano sposób przygotowania serwera TFS Team Build do kompilowania i wdrażania rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="e6b60-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="e6b60-180">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="e6b60-180">Further Reading</span></span>

<span data-ttu-id="e6b60-181">Aby uzyskać bardziej szczegółowe informacje na temat pracy z kontrolą źródła w programie TFS, zobacz [przy użyciu systemu kontroli wersji](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6b60-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e6b60-182">[Poprzednie](creating-a-team-project-in-tfs.md)
> [dalej](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="e6b60-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
