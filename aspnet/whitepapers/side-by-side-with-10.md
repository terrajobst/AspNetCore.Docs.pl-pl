---
uid: whitepapers/side-by-side-with-10
title: ASP.NET Side-by-Side wykonywania programu .NET Framework 1.0 i 1.1 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten dokument zawiera opis sposobu instalowania .NET 1.0 i 1.1 platformy .NET na komputerze, umożliwiając aplikacji sieci Web platformy ASP.NET do uruchamiania na dowolną wersję chronometraż...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: b5457a62e127ba555674fbe3b9f75552cad041c7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756429"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="4f9c5-103">ASP.NET Side-by-Side wykonywania programu .NET Framework 1.0 i 1.1</span><span class="sxs-lookup"><span data-stu-id="4f9c5-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="4f9c5-104">Ten dokument zawiera opis sposobu instalowania .NET 1.0 i 1.1 platformy .NET na komputerze, umożliwiając aplikacji sieci Web platformy ASP.NET do uruchamiania na obu wersji framework.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="4f9c5-105">Stosuje się do platformy ASP.NET w wersji 1.0 i 1.1 programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="4f9c5-106">W programie ASP.NET: aplikacje mówi się, że jest uruchomiona obok siebie, gdy są zainstalowane na tym samym komputerze, ale korzystają z różnych wersji programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="4f9c5-107">Poniższy temat zawiera opis sposobu konfigurowania aplikacji ASP.NET w celu wykonania side-by-side i zawiera szczegółowy opis kroków:</span><span class="sxs-lookup"><span data-stu-id="4f9c5-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="4f9c5-108">Obsługa mapowania aplikacji sieci Web platformy .NET Framework w wersji 1.0, podczas instalacji</span><span class="sxs-lookup"><span data-stu-id="4f9c5-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="4f9c5-109">Mapa aplikacji sieci Web do określonej wersji programu .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4f9c5-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="4f9c5-110">Znajdź wersję .NET Framework, która używa witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="4f9c5-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="4f9c5-111">Tradycyjnie zaktualizowaniu składnik lub pojedyncza aplikacja na komputerze, starsza wersja została usunięta i zastąpiona nowszą wersją.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="4f9c5-112">Jeśli nowa wersja nie jest zgodny z poprzednią wersją, to zwykle przerywa inne aplikacje, które używają składnika lub aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="4f9c5-113">.NET Framework zapewnia obsługę wykonywania side-by-side, dzięki któremu wiele wersji zestawu lub aplikacji do zainstalowania na tym samym komputerze, w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="4f9c5-114">Ponieważ jednocześnie może być zainstalowanych wiele wersji, zarządzanych aplikacji można wybrać wersji bez wpływu na aplikacje, które korzystają z różnych wersji.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="4f9c5-115">Domyślnie podczas instalacji programu .NET Framework w wersji 1.1, wszystkich istniejących aplikacji ASP.NET są automatycznie konfigurowane do używania najnowszej wersji programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="4f9c5-116">Jeśli chcesz, aby aplikacje ASP.NET domyślnie .NET Framework 1.1, kliknij przycisk [tutaj](#1) dowiesz się, jak można temu zapobiec, podczas instalacji.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="4f9c5-117">Jeśli aktualizacja serwera sieci Web platformy .NET Framework 1.1 i ma jedną lub więcej aplikacji sieci Web do uruchamiania programu .NET Framework 1.0, musisz zaktualizować mapę skryptu Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="4f9c5-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="4f9c5-118">Mapowanie skryptu jest mechanizmem mapowania rozszerzenia pliku .aspx dla określonej aplikacji sieci Web do wersji programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="4f9c5-119">Kliknij przycisk [tutaj](#2) dowiesz się, jak do mapowania aplikacji sieci Web do określonej wersji programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="4f9c5-120">Możesz użyć Menedżera informacji Internet lub narzędzia rejestracji usług IIS platformy ASP.NET (Aspnet\_regiis.exe) można znaleźć, która wersja środowiska .NET Framework działa konkretnej aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="4f9c5-121">Kliknij przycisk [tutaj](#3) dowiesz się, jak można znaleźć wersji programu .NET Framework, która używa witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="4f9c5-122">Pierwsza kwestia importu podczas migracji do programu .NET Framework 1.1 jest, że każda wersja programu .NET Framework używa własnego pliku Machine.config.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="4f9c5-123">W rezultacie administrator sieci Web wprowadził zmiany do pliku Machine.config, muszą być migrowane do pliku Machine.config programu .NET Framework 1.1 tych zmian.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="4f9c5-124">Obsługa aplikacji sieci Web mapowania do .NET Framework 1.0, podczas instalacji</span><span class="sxs-lookup"><span data-stu-id="4f9c5-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="4f9c5-125">Domyślnie wszystkie istniejące aplikacje ASP.NET są automatycznie konfigurowane podczas instalacji w celu korzystania z nowszej wersji programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="4f9c5-126">Przy użyciu nowszej wersji programu .NET Framework, aplikacje mogą wykorzystać wprowadzono szereg usprawnień i nowych funkcji dostępnych w nowej wersji.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="4f9c5-127">W tym samym czasie administratora sieci Web, który może być zapewnia szczegółową kontrolę aplikacji, które zostały zaktualizowane, może uniemożliwić automatyczne ponowne mapowanie wszystkich istniejących aplikacji programu ASP.NET podczas instalacji programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="4f9c5-128">Aby uniknąć automatycznego ponownego mapowania całej aplikacji ASP.NET do nowszej wersji programu .NET Framework, administrator sieci Web można za pomocą opcji wiersza polecenia/noaspupgrade Dotnetfx.exe program instalacyjny.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="4f9c5-129">**Aby uniknąć ponownego mapowania łączna liczba aplikacji ASP.NET do nowszej wersji**</span><span class="sxs-lookup"><span data-stu-id="4f9c5-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="4f9c5-130">Przejdź do **Start**.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-130">Go to **Start**.</span></span>
2. <span data-ttu-id="4f9c5-131">Kliknij pozycję **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-131">Click on **run**.</span></span>
3. <span data-ttu-id="4f9c5-132">Typ **cmd**.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-132">Type **cmd**.</span></span>
4. <span data-ttu-id="4f9c5-133">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="4f9c5-134">W wierszu polecenia, wpisz następujący wiersz, aby rozpocząć instalację programu .NET Framework: **/c: Dotnetfx.exe "Instalowanie/noaspupgrade?**.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="4f9c5-135">Kliknij przycisk **tak** w Instalatora programu Microsoft .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="4f9c5-136">Spowoduje to uruchomienie procesu instalacji programu .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="4f9c5-137">Mapa aplikacji sieci Web do określonej wersji programu .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4f9c5-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="4f9c5-138">Każda wersja programu .NET Framework zawiera wersję narzędzia rejestracji usług IIS platformy ASP.NET (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="4f9c5-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="4f9c5-139">To narzędzie umożliwia administratorom określenie, czy aplikacja sieci Web, być uruchamiany w kontekście określonej wersji programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="4f9c5-140">Jest to określane jako mapowanie aplikacji sieci Web do wersji programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="4f9c5-141">Administratorzy muszą wybrać Aspnet\_regiis.exe odpowiadającą wersję programu .NET Framework, która zostanie skojarzona z aplikacją sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="4f9c5-142">Na przykład administrator, który chce, aby określić, że witryny sieci Web używa .NET Framework 1.1, należy użyć Aspnet\_regiis.exe dostarczanego z .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="4f9c5-143">Aspnet\_regiis.exe dla wersji 1.0 znajduje się w folderze:</span><span class="sxs-lookup"><span data-stu-id="4f9c5-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="4f9c5-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="4f9c5-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="4f9c5-145">Aspnet\_regiis.exe w wersji 1,1 znajduje się w folderze:</span><span class="sxs-lookup"><span data-stu-id="4f9c5-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="4f9c5-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="4f9c5-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="4f9c5-147">Aspnet\_regiis.exe oferuje dwie opcje skryptów, mapowania aplikacji sieci Web:</span><span class="sxs-lookup"><span data-stu-id="4f9c5-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="4f9c5-148">**-s** ustawia mapę skryptu w ścieżce i w jego podrzędny katalogów.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="4f9c5-149">**-sn** ustawia mapę skryptu w tylko ścieżki.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="4f9c5-150">Ścieżka definiuje sieci Web usług IIS metadanych ścieżce aplikacji, która jest zdefiniowana w formie W3SVC/ROOT / {WebSiteNumber} / {aplikacji\_nazwa}.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="4f9c5-151">Na przykład dla aplikacji sieci Web o nazwie Portal znajduje się w obszarze Domyślna witryna sieci Web, ścieżka metabazy jest W3SVC/1/główny/Portal.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="4f9c5-152">Należy pamiętać, możesz również użyć z narzędzia o nazwie Edytor metabazy, aby pobrać ścieżkę do metabazy.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="4f9c5-153">To narzędzie można pobrać z witryny Microsoft Support w [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="4f9c5-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="4f9c5-154">Uruchom Aspnet\_mapy i jego subapplication skryptów regiis.exe -s W3SVC/1/główny/portalu, aby zaktualizować portal usług IIS.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="4f9c5-155">Uruchom Aspnet\_regiis.exe -sn mapować W3SVC/1/główny/portalu, aby zaktualizować portal skryptów usług IIS, bez wywierania wpływu na aplikacje w portalu? s podkatalogów.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="4f9c5-156">Znajdź wersji .NET Framework, który jest używany przez aplikację sieci Web</span><span class="sxs-lookup"><span data-stu-id="4f9c5-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="4f9c5-157">Administrator może użyć Menedżera usług internetowych, aby dowiedzieć się, która wersja programu .NET Framework działa witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="4f9c5-158">Wersje systemów operacyjnych Uruchom Menedżera usług internetowych inaczej.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="4f9c5-159">Aby uruchomić programu service manager, wykonaj kroki opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="4f9c5-160">**Aby uruchomić Menedżera usług internetowych**</span><span class="sxs-lookup"><span data-stu-id="4f9c5-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="4f9c5-161">Przejdź do **Start**.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-161">Go to **Start**.</span></span>
2. <span data-ttu-id="4f9c5-162">Kliknij pozycję **Uruchom**.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-162">Click on **run**.</span></span>
3. <span data-ttu-id="4f9c5-163">Typ **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="4f9c5-164">Z Menedżera usług internetowych, wybierz aplikację sieci Web, których wersji programu .NET Framework, aby dowiedzieć się.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="4f9c5-165">Kliknij prawym przyciskiem myszy w aplikacji sieci Web, a następnie kliknij **właściwości.**</span><span class="sxs-lookup"><span data-stu-id="4f9c5-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="4f9c5-166">W oknie właściwości wybierz **konfiguracji.**</span><span class="sxs-lookup"><span data-stu-id="4f9c5-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="4f9c5-167">Wybierz z tabeli mapowania aplikacji **.aspx**i kliknij przycisk **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="4f9c5-168">Z **plik wykonywalny** polu tekstowym Szukaj w katalogu wersji przewijając.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="4f9c5-169">Jeśli w katalogu wersji v.1.1.4322, aplikacja zostaje zmapowana do .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="4f9c5-170">Z drugiej strony Jeśli w katalogu wersji v1.0.3705, aplikacja zostaje zmapowana do .NET Framework 1.0.</span><span class="sxs-lookup"><span data-stu-id="4f9c5-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
