---
title: Publikowanie aplikacji ASP.NET Core w usługach IIS
author: guardrex
description: Dowiedz się, jak hostować aplikację ASP.NET Core na serwerze usług IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: tutorials/publish-to-iis
ms.openlocfilehash: c31beae16f46153daac188ab1638e5530584ac88
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68927215"
---
# <a name="publish-an-aspnet-core-app-to-iis"></a><span data-ttu-id="c7d84-103">Publikowanie aplikacji ASP.NET Core w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="c7d84-103">Publish an ASP.NET Core app to IIS</span></span>

<span data-ttu-id="c7d84-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c7d84-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c7d84-105">W tym samouczku pokazano, jak hostować aplikację ASP.NET Core na serwerze usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-105">This tutorial shows how to host an ASP.NET Core app on an IIS server.</span></span>

<span data-ttu-id="c7d84-106">Ten samouczek obejmuje następujące zagadnienia:</span><span class="sxs-lookup"><span data-stu-id="c7d84-106">This tutorial covers the following subjects:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c7d84-107">Zainstaluj pakiet hostingu platformy .NET Core w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="c7d84-107">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="c7d84-108">Utwórz witrynę usług IIS w Menedżerze usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-108">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="c7d84-109">Wdróż aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7d84-109">Deploy an ASP.NET Core app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7d84-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="c7d84-110">Prerequisites</span></span>

* <span data-ttu-id="c7d84-111">[Zestaw .NET Core SDK](/dotnet/core/sdk) zainstalowane na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="c7d84-111">[.NET Core SDK](/dotnet/core/sdk) installed on the development machine.</span></span>
* <span data-ttu-id="c7d84-112">System Windows Server skonfigurowany z rolą serwera **serwer sieci Web (IIS)** .</span><span class="sxs-lookup"><span data-stu-id="c7d84-112">Windows Server configured with the **Web Server (IIS)** server role.</span></span> <span data-ttu-id="c7d84-113">Jeśli serwer nie jest skonfigurowany do hostowania witryn sieci Web za pomocą usług IIS, postępuj zgodnie ze wskazówkami w sekcji <xref:host-and-deploy/iis/index#iis-configuration> *Konfiguracja usług IIS* w artykule, a następnie wróć do tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="c7d84-113">If your server isn't configured to host websites with IIS, follow the guidance in the *IIS configuration* section of the <xref:host-and-deploy/iis/index#iis-configuration> article and then return to this tutorial.</span></span>

> [!WARNING]
> <span data-ttu-id="c7d84-114">**Konfiguracja usług IIS i zabezpieczenia witryny sieci Web obejmują pojęcia, które nie są objęte tym samouczkiem.**</span><span class="sxs-lookup"><span data-stu-id="c7d84-114">**IIS configuration and website security involve concepts that aren't covered by this tutorial.**</span></span> <span data-ttu-id="c7d84-115">Zapoznaj się ze wskazówkami dotyczącymi usług IIS w [dokumentacji usług Microsoft IIS](https://www.iis.net/) i [artykułem ASP.NET Core na hostowanie usług IIS](xref:host-and-deploy/iis/index) przed hostowania aplikacji produkcyjnych w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-115">Consult the IIS guidance in the [Microsoft IIS documentation](https://www.iis.net/) and the [ASP.NET Core article on hosting with IIS](xref:host-and-deploy/iis/index) before hosting production apps on IIS.</span></span>
>
> <span data-ttu-id="c7d84-116">Ważne scenariusze dotyczące hostingu usług IIS, które nie są objęte tym samouczkiem, obejmują:</span><span class="sxs-lookup"><span data-stu-id="c7d84-116">Important scenarios for IIS hosting not covered by this tutorial include:</span></span>
>
> * [<span data-ttu-id="c7d84-117">Tworzenie gałęzi rejestru na potrzeby ochrony danych ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7d84-117">Creation of a registry hive for ASP.NET Core Data Protection</span></span>](xref:host-and-deploy/iis/index#data-protection)
> * [<span data-ttu-id="c7d84-118">Konfiguracja listy Access Control puli aplikacji (ACL)</span><span class="sxs-lookup"><span data-stu-id="c7d84-118">Configuration of the app pool's Access Control List (ACL)</span></span>](xref:host-and-deploy/iis/index#application-pool-identity)
> * <span data-ttu-id="c7d84-119">Aby skoncentrować się na pojęciach dotyczących wdrażania usług IIS, w tym samouczku wdrożono aplikację bez zabezpieczeń protokołu HTTPS skonfigurowanej w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-119">To focus on IIS deployment concepts, this tutorial deploys an app without HTTPS security configured in IIS.</span></span> <span data-ttu-id="c7d84-120">Aby uzyskać więcej informacji na temat hostowania aplikacji z włączoną obsługą protokołu HTTPS, zobacz tematy dotyczące zabezpieczeń w sekcji [dodatkowe zasoby](#additional-resources) w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="c7d84-120">For more information on hosting an app enabled for HTTPS protocol, see the security topics in the [Additional resources](#additional-resources) section of this article.</span></span> <span data-ttu-id="c7d84-121">Dalsze wskazówki dotyczące hostingu ASP.NET Core aplikacji znajdują się w <xref:host-and-deploy/iis/index> artykule.</span><span class="sxs-lookup"><span data-stu-id="c7d84-121">Further guidance for hosting ASP.NET Core apps is provided in the <xref:host-and-deploy/iis/index> article.</span></span>

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="c7d84-122">Zainstaluj program .NET Core hostingu pakietu</span><span class="sxs-lookup"><span data-stu-id="c7d84-122">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="c7d84-123">Zainstaluj *pakiet hostingu platformy .NET Core* na serwerze usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-123">Install the *.NET Core Hosting Bundle* on the IIS server.</span></span> <span data-ttu-id="c7d84-124">Pakiet instaluje .NET Core środowisko uruchomieniowe, biblioteki platformy .NET Core i [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="c7d84-124">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="c7d84-125">Moduł umożliwia platformy ASP.NET Core w aplikacji do uruchamiania w tle usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-125">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="c7d84-126">Jeśli system nie ma dostępu do Internetu, należy uzyskać i zainstalować [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) przed zainstalowaniem pakietu hostingu platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7d84-126">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

<span data-ttu-id="c7d84-127">Pobierz Instalatora, korzystając z następującego linku:</span><span class="sxs-lookup"><span data-stu-id="c7d84-127">Download the installer using the following link:</span></span>

[<span data-ttu-id="c7d84-128">Bieżący Instalatora pakietu hostingu programu .NET Core (pobieranie bezpośrednie)</span><span class="sxs-lookup"><span data-stu-id="c7d84-128">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

1. <span data-ttu-id="c7d84-129">Uruchom Instalatora na serwerze usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-129">Run the installer on the IIS server.</span></span>

1. <span data-ttu-id="c7d84-130">Uruchom ponownie serwer, a następnie wykonaj polecenie **net start W3SVC** w powłoce poleceń.</span><span class="sxs-lookup"><span data-stu-id="c7d84-130">Restart the server or execute **net stop was /y** followed by **net start w3svc** in a command shell.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="c7d84-131">Tworzenie witryny usług IIS</span><span class="sxs-lookup"><span data-stu-id="c7d84-131">Create the IIS site</span></span>

1. <span data-ttu-id="c7d84-132">Na serwerze usług IIS Utwórz folder, który będzie zawierać opublikowane foldery i pliki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7d84-132">On the IIS server, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="c7d84-133">W poniższym kroku ścieżka folderu jest dostarczana do usług IIS jako ścieżka fizyczna do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7d84-133">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span>

1. <span data-ttu-id="c7d84-134">W Menedżerze usług IIS Otwórz węzeł serwera w panelu **połączenia** .</span><span class="sxs-lookup"><span data-stu-id="c7d84-134">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="c7d84-135">Kliknij prawym przyciskiem myszy **witryn** folderu.</span><span class="sxs-lookup"><span data-stu-id="c7d84-135">Right-click the **Sites** folder.</span></span> <span data-ttu-id="c7d84-136">Wybierz **Dodawanie witryny sieci Web** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="c7d84-136">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="c7d84-137">Podaj **nazwę lokacji** i ustaw **ścieżkę fizyczną** do utworzonego folderu wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7d84-137">Provide a **Site name** and set the **Physical path** to the app's deployment folder that you created.</span></span> <span data-ttu-id="c7d84-138">Podaj konfigurację **powiązania** i Utwórz witrynę sieci Web, wybierając **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="c7d84-138">Provide the **Binding** configuration and create the website by selecting **OK**.</span></span>

## <a name="create-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="c7d84-139">Tworzenie aplikacji Razor Pages ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7d84-139">Create an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="c7d84-140">Postępuj <xref:getting-started> zgodnie z samouczkiem, aby utworzyć aplikację Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c7d84-140">Follow the <xref:getting-started> tutorial to create a Razor Pages app.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="c7d84-141">Publikowanie i wdrażanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="c7d84-141">Publish and deploy the app</span></span>

<span data-ttu-id="c7d84-142">*Opublikowanie aplikacji* oznacza utworzenie skompilowanej aplikacji, która może być hostowana przez serwer programu.</span><span class="sxs-lookup"><span data-stu-id="c7d84-142">*Publish an app* means to produce a compiled app that can be hosted by a server.</span></span> <span data-ttu-id="c7d84-143">*Wdrożenie aplikacji* oznacza przeniesienie opublikowanej aplikacji do systemu hostingu.</span><span class="sxs-lookup"><span data-stu-id="c7d84-143">*Deploy an app* means to move the published app to a hosting system.</span></span> <span data-ttu-id="c7d84-144">Krok Publikuj jest obsługiwany przez [zestaw .NET Core SDK](/dotnet/core/sdk), podczas gdy krok wdrożenia może być obsługiwany przez różne podejścia.</span><span class="sxs-lookup"><span data-stu-id="c7d84-144">The publish step is handled by the [.NET Core SDK](/dotnet/core/sdk), while the deployment step can be handled by a variety of approaches.</span></span> <span data-ttu-id="c7d84-145">Ten samouczek przyjmuje podejście do wdrożenia *folderu* , gdzie:</span><span class="sxs-lookup"><span data-stu-id="c7d84-145">This tutorial adopts the *folder* deployment approach, where:</span></span>

* <span data-ttu-id="c7d84-146">Aplikacja zostanie opublikowana w folderze.</span><span class="sxs-lookup"><span data-stu-id="c7d84-146">The app is published to a folder.</span></span>
* <span data-ttu-id="c7d84-147">Zawartość folderu zostanie przeniesiona do folderu witryny usług IIS ( **Ścieżka fizyczna** do lokacji w Menedżerze usług IIS).</span><span class="sxs-lookup"><span data-stu-id="c7d84-147">The folder's contents are moved to the IIS site's folder (the **Physical path** to the site in IIS Manager).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c7d84-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7d84-148">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c7d84-149">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="c7d84-149">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="c7d84-150">W oknie dialogowym **Wybieranie elementu docelowego publikowania** wybierz opcję Publikuj **folder** .</span><span class="sxs-lookup"><span data-stu-id="c7d84-150">In the **Pick a publish target** dialog, select the **Folder** publish option.</span></span>
1. <span data-ttu-id="c7d84-151">Ustaw ścieżkę **folderu lub udziału plików** .</span><span class="sxs-lookup"><span data-stu-id="c7d84-151">Set the **Folder or File Share** path.</span></span>
   * <span data-ttu-id="c7d84-152">Jeśli utworzono folder dla witryny usług IIS, która jest dostępna na komputerze deweloperskim jako udział sieciowy, podaj ścieżkę do udziału.</span><span class="sxs-lookup"><span data-stu-id="c7d84-152">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="c7d84-153">Bieżący użytkownik musi mieć dostęp do zapisu w celu opublikowania w udziale.</span><span class="sxs-lookup"><span data-stu-id="c7d84-153">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="c7d84-154">Jeśli nie możesz wdrożyć bezpośrednio w folderze witryny usług IIS na serwerze IIS, Opublikuj je w folderze na nośniku umożliwiającym usunięcie i fizycznie Przenieś opublikowaną aplikację do folderu witryny usług IIS na serwerze, który jest **ścieżką fizyczną** lokacji w Menedżerze usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-154">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="c7d84-155">Przenieś zawartość folderu *bin/Release/{Target Framework}/Publish* do folderu witryny usług IIS na serwerze, który jest **ścieżką fizyczną** lokacji w Menedżerze usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-155">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c7d84-156">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c7d84-156">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="c7d84-157">W powłoce poleceń Opublikuj aplikację w konfiguracji wydania przy użyciu polecenia [dotnet Publish](/dotnet/core/tools/dotnet-publish) :</span><span class="sxs-lookup"><span data-stu-id="c7d84-157">In a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="c7d84-158">Przenieś zawartość folderu *bin/Release/{Target Framework}/Publish* do folderu witryny usług IIS na serwerze, który jest **ścieżką fizyczną** lokacji w Menedżerze usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-158">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c7d84-159">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c7d84-159">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="c7d84-160">Kliknij prawym przyciskiem myszy projekt w **rozwiązaniu** , a następnie wybierz pozycję **Publikuj** > **Publikowanie w folderze**.</span><span class="sxs-lookup"><span data-stu-id="c7d84-160">Right-click on the project in **Solution** and select **Publish** > **Publish to Folder**.</span></span>
1. <span data-ttu-id="c7d84-161">Ustaw ścieżkę do **folderu wybierz** .</span><span class="sxs-lookup"><span data-stu-id="c7d84-161">Set the **Choose a folder** path.</span></span>
   * <span data-ttu-id="c7d84-162">Jeśli utworzono folder dla witryny usług IIS, która jest dostępna na komputerze deweloperskim jako udział sieciowy, podaj ścieżkę do udziału.</span><span class="sxs-lookup"><span data-stu-id="c7d84-162">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="c7d84-163">Bieżący użytkownik musi mieć dostęp do zapisu w celu opublikowania w udziale.</span><span class="sxs-lookup"><span data-stu-id="c7d84-163">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="c7d84-164">Jeśli nie możesz wdrożyć bezpośrednio w folderze witryny usług IIS na serwerze IIS, Opublikuj je w folderze na nośniku umożliwiającym usunięcie i fizycznie Przenieś opublikowaną aplikację do folderu witryny usług IIS na serwerze, który jest **ścieżką fizyczną** lokacji w Menedżerze usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-164">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="c7d84-165">Przenieś zawartość folderu *bin/Release/{Target Framework}/Publish* do folderu witryny usług IIS na serwerze, który jest **ścieżką fizyczną** lokacji w Menedżerze usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-165">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

---

## <a name="browse-the-website"></a><span data-ttu-id="c7d84-166">Przeglądanie witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="c7d84-166">Browse the website</span></span>

<span data-ttu-id="c7d84-167">Aplikacja jest dostępna w przeglądarce po odebraniu pierwszego żądania.</span><span class="sxs-lookup"><span data-stu-id="c7d84-167">The app is accessible in a browser after it receives the first request.</span></span> <span data-ttu-id="c7d84-168">Prześlij żądanie do aplikacji przy użyciu powiązania punktu końcowego, który został ustanowiony w Menedżerze usług IIS dla tej lokacji.</span><span class="sxs-lookup"><span data-stu-id="c7d84-168">Make a request to the app at the endpoint binding that you established in IIS Manager for the site.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c7d84-169">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="c7d84-169">Next steps</span></span>

<span data-ttu-id="c7d84-170">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="c7d84-170">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c7d84-171">Zainstaluj pakiet hostingu platformy .NET Core w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="c7d84-171">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="c7d84-172">Utwórz witrynę usług IIS w Menedżerze usług IIS.</span><span class="sxs-lookup"><span data-stu-id="c7d84-172">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="c7d84-173">Wdróż aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7d84-173">Deploy an ASP.NET Core app.</span></span>

<span data-ttu-id="c7d84-174">Aby dowiedzieć się więcej na temat hostowania aplikacji ASP.NET Core w usługach IIS, zobacz artykuł Omówienie usług IIS:</span><span class="sxs-lookup"><span data-stu-id="c7d84-174">To learn more about hosting ASP.NET Core apps on IIS, see the IIS Overview article:</span></span>

> [!div class="nextstepaction"]
> <xref:host-and-deploy/iis/index>

## <a name="additional-resources"></a><span data-ttu-id="c7d84-175">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c7d84-175">Additional resources</span></span>

### <a name="articles-in-the-aspnet-core-documentation-set"></a><span data-ttu-id="c7d84-176">Artykuły w zestawie dokumentacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7d84-176">Articles in the ASP.NET Core documentation set</span></span>

* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:test/troubleshoot-azure-iis>
* <xref:security/enforcing-ssl>

### <a name="articles-pertaining-to-aspnet-core-app-deployment"></a><span data-ttu-id="c7d84-177">Artykuły dotyczące wdrażania aplikacji ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7d84-177">Articles pertaining to ASP.NET Core app deployment</span></span>

* <xref:tutorials/publish-to-azure-webapp-using-vs>
* <xref:tutorials/publish-to-azure-webapp-using-vscode>
* <xref:host-and-deploy/visual-studio-publish-profiles>
* [<span data-ttu-id="c7d84-178">Publikowanie aplikacji sieci Web w folderze przy użyciu Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="c7d84-178">Publish a Web app to a folder using Visual Studio for Mac</span></span>](/visualstudio/mac/publish-folder)

### <a name="articles-on-iis-https-configuration"></a><span data-ttu-id="c7d84-179">Artykuły na temat konfiguracji protokołu HTTPS usług IIS</span><span class="sxs-lookup"><span data-stu-id="c7d84-179">Articles on IIS HTTPS configuration</span></span>

* [<span data-ttu-id="c7d84-180">Konfigurowanie protokołu SSL w Menedżerze usług IIS</span><span class="sxs-lookup"><span data-stu-id="c7d84-180">Configuring SSL in IIS Manager</span></span>](/iis/manage/configuring-security/configuring-ssl-in-iis-manager)
* [<span data-ttu-id="c7d84-181">Jak skonfigurować protokół SSL w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="c7d84-181">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)

### <a name="articles-on-iis-and-windows-server"></a><span data-ttu-id="c7d84-182">Artykuły dotyczące usług IIS i systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="c7d84-182">Articles on IIS and Windows Server</span></span>

* [<span data-ttu-id="c7d84-183">Witryna oficjalne Microsoft IIS</span><span class="sxs-lookup"><span data-stu-id="c7d84-183">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="c7d84-184">Windows Server Technical Preview biblioteki zawartości</span><span class="sxs-lookup"><span data-stu-id="c7d84-184">Windows Server technical content library</span></span>](/windows-server/windows-server)
