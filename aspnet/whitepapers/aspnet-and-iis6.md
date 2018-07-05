---
uid: whitepapers/aspnet-and-iis6
title: Uruchamianie platformy ASP.NET 1.1 z usługami IIS 6.0 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Gdy system Windows Server 2003 obejmuje zarówno usług IIS 6.0 i ASP.NET 1.1, składniki te są domyślnie wyłączone. Ten dokument opisuje sposób włączania usług IIS 6.0...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1983ef5b4902acc303ae224d5973eb2644284585
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383428"
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="3da2d-104">Uruchamianie platformy ASP.NET 1.1 z usługami IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="3da2d-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="3da2d-105">Gdy system Windows Server 2003 obejmuje zarówno usług IIS 6.0 i ASP.NET 1.1, składniki te są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="3da2d-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="3da2d-106">Ten oficjalny dokument, w tym artykule opisano sposób włączania usług IIS 6.0 i ASP.NET 1.1 i zaleca kilka ustawień konfiguracji, aby uzyskać optymalną wydajność usług IIS i platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3da2d-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="3da2d-107">Stosuje się do platformy ASP.NET 1.1 oraz IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="3da2d-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="3da2d-108">Platformy ASP.NET 1.1 jest dostarczany z programem Windows Server 2003, która zawiera też najnowszą wersję programu Internet Information Server (IIS) w wersji 6.0.</span><span class="sxs-lookup"><span data-stu-id="3da2d-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="3da2d-109">Usługi IIS 6.0 i ASP.NET 1.1 są przeznaczone do bezproblemowo integrują się i platformy ASP.NET teraz wartością domyślną jest nowy model proces roboczy usług IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="3da2d-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="3da2d-110">Platformy ASP.NET 1.1 nie jest instalowany domyślnie</span><span class="sxs-lookup"><span data-stu-id="3da2d-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="3da2d-111">W przeciwieństwie do poprzednich wersji systemów operacyjnych serwera firmy Microsoft Internet Information Server (IIS) jest domyślnie wyłączona; nie jest ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="3da2d-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="3da2d-112">Dostępne są dwie opcje umożliwiające usług IIS:</span><span class="sxs-lookup"><span data-stu-id="3da2d-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="3da2d-113">Włączanie usług IIS, opcja #1 — Kreator konfiguracji serwera</span><span class="sxs-lookup"><span data-stu-id="3da2d-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="3da2d-114">Windows Server 2003 jest dostarczany nowe "Kreatora konfiguracji serwera" Aby poprawnie skonfigurować serwer w trybie żądaną.</span><span class="sxs-lookup"><span data-stu-id="3da2d-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="3da2d-115">Aby uruchomić kreatora — należy pamiętać, aby uruchomić kreatora, użytkownik musi być zalogowany jako administrator — przejdź do: Start | Programy | Narzędzia administracyjne i wybierz opcję "Konfigurowanie serwera".</span><span class="sxs-lookup"><span data-stu-id="3da2d-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="3da2d-116">Po wybraniu, powinien zostać wyświetlony ekran powitalny "Kreatora konfiguracji serwera":</span><span class="sxs-lookup"><span data-stu-id="3da2d-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="3da2d-117">Kliknij przycisk "dalej &gt;":</span><span class="sxs-lookup"><span data-stu-id="3da2d-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="3da2d-118">Kliknij przycisk "dalej &gt;"</span><span class="sxs-lookup"><span data-stu-id="3da2d-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="3da2d-119">Na tym ekranie należy wybrać "serwer aplikacji (usługi IIS, ASP.NET) jako opcji, aby skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="3da2d-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="3da2d-120">Kliknij przycisk "dalej &gt;".</span><span class="sxs-lookup"><span data-stu-id="3da2d-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="3da2d-121">Po wybraniu, aby skonfigurować serwer jako serwer aplikacji, ten ekran pojawi się monit, jakie dodatkowe funkcje, które powinny być instalowane.</span><span class="sxs-lookup"><span data-stu-id="3da2d-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="3da2d-122">Żadna opcja jest zaznaczona domyślnie.</span><span class="sxs-lookup"><span data-stu-id="3da2d-122">Neither option is selected by default.</span></span> <span data-ttu-id="3da2d-123">Aby włączyć automatyczne ASP.NET, należy wybrać "Włącz ASP. NET ".</span><span class="sxs-lookup"><span data-stu-id="3da2d-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="3da2d-124">Kliknij przycisk "dalej &gt;".</span><span class="sxs-lookup"><span data-stu-id="3da2d-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="3da2d-125">Ten ekran przedstawia opcje są do zainstalowania.</span><span class="sxs-lookup"><span data-stu-id="3da2d-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="3da2d-126">Kliknij przycisk "dalej &gt;".</span><span class="sxs-lookup"><span data-stu-id="3da2d-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="3da2d-127">Zostanie wyświetlony następujący ekran, gdy wybrane opcje są instalowane.</span><span class="sxs-lookup"><span data-stu-id="3da2d-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="3da2d-128">Jest normalnym zjawiskiem, inne okno dialogowe, które pola są wyświetlane jako usługi, są instalowane.</span><span class="sxs-lookup"><span data-stu-id="3da2d-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="3da2d-129">Ponadto monit może zostać lokalizacji programu instalacyjnego dysku CD Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="3da2d-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="3da2d-130">Kliknij przycisk "dalej &gt;" po zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="3da2d-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="3da2d-131">Kliknij przycisk "Zakończ",-Windows Server 2003 jest teraz skonfigurowane do obsługi usług IIS 6.0 i ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="3da2d-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="3da2d-132">Włączanie usług IIS, opcja #2 - ręcznego konfigurowania usług IIS i platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3da2d-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="3da2d-133">Jeśli nie chcesz używać "Kreatora konfigurowania serwera" możesz opcjonalnie zainstalować usług IIS 6.0 i ASP.NET 1.1 apletu "Dodaj lub usuń programy" w Panelu sterowania.</span><span class="sxs-lookup"><span data-stu-id="3da2d-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="3da2d-134">Najpierw otwórz Panel sterowania:</span><span class="sxs-lookup"><span data-stu-id="3da2d-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="3da2d-135">Następnie kliknij przycisk "Dodaj lub usuń Windows składników" zostanie otwarte Kreatora składników Windows:</span><span class="sxs-lookup"><span data-stu-id="3da2d-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="3da2d-136">Wyróżnij i sprawdź "Serwer aplikacji", a następnie kliknij przycisk "Szczegóły"?</span><span class="sxs-lookup"><span data-stu-id="3da2d-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="3da2d-137">Przycisk:</span><span class="sxs-lookup"><span data-stu-id="3da2d-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="3da2d-138">Aby zainstalować program ASP.NET, zapoznaj się z ' ASP. NET ".</span><span class="sxs-lookup"><span data-stu-id="3da2d-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="3da2d-139">Kliknij przycisk OK, aby powrócić do Kreatora składników Windows.</span><span class="sxs-lookup"><span data-stu-id="3da2d-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="3da2d-140">Kliknij przycisk "dalej &gt;" za pomocą Kreatora składnika Windows, aby rozpocząć instalowanie:</span><span class="sxs-lookup"><span data-stu-id="3da2d-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="3da2d-141">Jest normalnym zjawiskiem, inne okno dialogowe, które pola są wyświetlane jako usługi, są instalowane.</span><span class="sxs-lookup"><span data-stu-id="3da2d-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="3da2d-142">Ponadto monit może zostać lokalizacji programu instalacyjnego dysku CD Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="3da2d-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="3da2d-143">Po zakończeniu instalacji zostanie wyświetlony ostatni ekran kreatora składnika Windows:</span><span class="sxs-lookup"><span data-stu-id="3da2d-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="3da2d-144">Usługi IIS 6.0 i ASP.NET 1.1 są teraz skonfigurowane i dostępne.</span><span class="sxs-lookup"><span data-stu-id="3da2d-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="3da2d-145">Zalecane ustawienia</span><span class="sxs-lookup"><span data-stu-id="3da2d-145">Recommended Settings</span></span>

<span data-ttu-id="3da2d-146">Podczas uruchamiania 1.1 platformy ASP.NET w usługach IIS 6.0, istnieje kilka ustawień konfiguracji, które są zalecane, aby uzyskać optymalną wydajność z platformy ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="3da2d-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="3da2d-147">Konfigurowanie limity pamięci procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="3da2d-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="3da2d-148">Konfigurowanie procesu podrzędnego</span><span class="sxs-lookup"><span data-stu-id="3da2d-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="3da2d-149">Konfigurowanie limity pamięci procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="3da2d-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="3da2d-150">Domyślnie usługi IIS 6.0 nie ustawia limit ilości pamięci, która może używać usług IIS.</span><span class="sxs-lookup"><span data-stu-id="3da2d-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="3da2d-151">ASP. NET w pamięci podręcznej funkcji opiera się na ograniczenie pamięci, dlatego pamięci podręcznej aktywnie usunąć nieużywane elementy z pamięci.</span><span class="sxs-lookup"><span data-stu-id="3da2d-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="3da2d-152">Zalecane jest skonfigurowanie odtwarzanie funkcja usług IIS 6.0 pamięci.</span><span class="sxs-lookup"><span data-stu-id="3da2d-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="3da2d-153">Aby skonfigurować ten Otwórz Menedżera internetowych usług informacyjnych (Start | Programy | Narzędzia administracyjne | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="3da2d-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="3da2d-154">Po otwarciu rozwiń folder "Puli aplikacji":</span><span class="sxs-lookup"><span data-stu-id="3da2d-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="3da2d-155">Dla każdej puli aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3da2d-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="3da2d-156">Kliknij prawym przyciskiem myszy pulę aplikacji, np. "DefaultAppPool" i wybierz polecenie Właściwości:</span><span class="sxs-lookup"><span data-stu-id="3da2d-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="3da2d-157">Następnie Włącz odtwarzanie pamięci, klikając opcję "maksymalne użycie pamięci (w megabajtach):".</span><span class="sxs-lookup"><span data-stu-id="3da2d-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="3da2d-158">Wartość nie powinna być większa niż ilość pamięci fizycznej (nie wirtualnym) na serwerze, dobrego pamięci fizycznej, np. do serwera z 512MB pamięci fizycznej wybierz 310 o 60%.</span><span class="sxs-lookup"><span data-stu-id="3da2d-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="3da2d-159">Zalecane jest również, że maksymalna przekracza 800MB podczas korzystania z przestrzeni adresowej 2GB.</span><span class="sxs-lookup"><span data-stu-id="3da2d-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="3da2d-160">Jeśli przestrzeń adresowa pamięci serwera to 3GB, limit pamięci maksymalnej dla procesu roboczego może być możliwie jak 1, 800MB:</span><span class="sxs-lookup"><span data-stu-id="3da2d-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="3da2d-161">Kliknij przycisk "Zastosuj" i "OK", aby zamknąć okno dialogowe właściwości.</span><span class="sxs-lookup"><span data-stu-id="3da2d-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="3da2d-162">Powtórz tę czynność dla wszystkich dostępnych pul aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3da2d-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="3da2d-163">Konfigurowanie odtwarzania procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="3da2d-163">Configuring worker recycling</span></span>

<span data-ttu-id="3da2d-164">Domyślnie usługi IIS 6.0 jest skonfigurowany do Odtwórz jej proces roboczy co 29 godziny.</span><span class="sxs-lookup"><span data-stu-id="3da2d-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="3da2d-165">Jest to nieco agresywne dla aplikacji ASP.NET i zaleca się, że automatycznego procesu podrzędnego jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="3da2d-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="3da2d-166">Aby wyłączyć automatyczne procesu podrzędnego, najpierw otwórz Menedżera internetowych usług informacyjnych (Start | Programy | Narzędzia administracyjne | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="3da2d-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="3da2d-167">Po otwarciu rozwiń folder "Puli aplikacji":</span><span class="sxs-lookup"><span data-stu-id="3da2d-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="3da2d-168">Dla każdej puli aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3da2d-168">For each application pool:</span></span>

1. <span data-ttu-id="3da2d-169">Kliknij prawym przyciskiem myszy pulę aplikacji, np. "DefaultAppPool" i wybierz polecenie Właściwości:</span><span class="sxs-lookup"><span data-stu-id="3da2d-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="3da2d-170">Usuń zaznaczenie pola wyboru "Odtwórz proces roboczy (w minutach):":</span><span class="sxs-lookup"><span data-stu-id="3da2d-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="3da2d-171">Kliknij przycisk "Zastosuj" i "OK", aby zamknąć okno dialogowe właściwości.</span><span class="sxs-lookup"><span data-stu-id="3da2d-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="3da2d-172">Powtórz tę czynność dla wszystkich dostępnych pul aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3da2d-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="3da2d-173">Udzielanie uprawnienia do zapisu w systemie plików</span><span class="sxs-lookup"><span data-stu-id="3da2d-173">Granting write access to the file system</span></span>

<span data-ttu-id="3da2d-174">Jeśli aplikacja wymaga uprawnienia do zapisu w systemie plików, a w przypadku korzystania z systemu plików NTFS, będzie konieczne zmodyfikowanie kontroli dostępu listy (ACL) na folder lub plik, aby udzielić dostępu programu ASP.NET do.</span><span class="sxs-lookup"><span data-stu-id="3da2d-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="3da2d-175">Na przykład udzielić ASP.NET uprawnienia do zapisu w c:\inetpub\wwwroot najpierw otwórz Eksploratora i przejdź do katalogu:</span><span class="sxs-lookup"><span data-stu-id="3da2d-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="3da2d-176">Następnie kliknij prawym przyciskiem myszy w katalogu, np. "wwwroot" i wybierz polecenie Właściwości.</span><span class="sxs-lookup"><span data-stu-id="3da2d-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="3da2d-177">Po otwarciu okna dialogowego właściwości wybierz kartę "Zabezpieczenia":</span><span class="sxs-lookup"><span data-stu-id="3da2d-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="3da2d-178">Katalog c:\inetpub\wwwroot\ jest specjalne w tym specjalnej grupy usług IIS 6.0 "usług IIS\_WPG" jest już przydzielone odczytu &amp; uprawnień wykonywanie, wyświetlanie zawartości folderu oraz Odczyt.</span><span class="sxs-lookup"><span data-stu-id="3da2d-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="3da2d-179">Jednak aby udzielić uprawnień do zapisu, musisz kliknąć przycisk wyboru Zezwalaj zapisu:</span><span class="sxs-lookup"><span data-stu-id="3da2d-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="3da2d-180">Usługi IIS 6.0 ma teraz uprawnienia do zapisu dla tego folderu.</span><span class="sxs-lookup"><span data-stu-id="3da2d-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="3da2d-181">Aby przyznać uprawnienia do zapisu w innych folderach, wykonaj poniższe kroki, — należy pamiętać, może być konieczne dodanie usług IIS\_WPG grupy, jeśli jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="3da2d-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="3da2d-182">Udzielanie uprawnienia zapisu w programu IIS\_WPG umożliwi dowolnej aplikacji platformy ASP.NET do zapisu do tego katalogu.</span><span class="sxs-lookup"><span data-stu-id="3da2d-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="3da2d-183">Obsługa uwierzytelniania zintegrowanego z programem SQL Server</span><span class="sxs-lookup"><span data-stu-id="3da2d-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="3da2d-184">Zintegrowane uwierzytelnianie umożliwia SQL Server, aby korzystać z uwierzytelniania systemu Windows NT do sprawdzania poprawności konta logowania programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3da2d-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="3da2d-185">Dzięki temu użytkownikowi na pominięcie standardowego procesu logowania programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3da2d-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="3da2d-186">Takie podejście użytkownik sieci umożliwia dostęp do bazy danych programu SQL Server bez podawania identyfikator oddzielne logowania i hasła, ponieważ program SQL Server uzyskuje użytkownika i hasło z procesu zabezpieczeń sieci systemu Windows NT.</span><span class="sxs-lookup"><span data-stu-id="3da2d-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="3da2d-187">Wybieranie uwierzytelniania zintegrowanego dla aplikacji ASP.NET jest dobrym rozwiązaniem, ponieważ żadne poświadczenia nie nigdy nie są przechowywane w ramach parametrów połączenia dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3da2d-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="3da2d-188">Zamiast parametry połączenia używane do połączenia z serwerem SQL będzie wyglądać w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3da2d-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="3da2d-189">Te parametry połączenia zawiera informacje dla programu SQL Server przy użyciu poświadczeń Windows aplikacji, próba dostępu do serwera SQL.</span><span class="sxs-lookup"><span data-stu-id="3da2d-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="3da2d-190">W przypadku ASP.NET/IIS 6 powinien to być konto w usługach IIS\_WPG grupy.</span><span class="sxs-lookup"><span data-stu-id="3da2d-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="3da2d-191">Aby włączyć zintegrowane uwierzytelnianie między serwerem SQL i programu ASP.NET, należy najpierw upewnić się, że program SQL Server jest skonfigurowany dla zintegrowanego uwierzytelniania lub uwierzytelnianie w trybie mieszanym — skontaktuj się z administratora bazy danych, aby to ustalić.</span><span class="sxs-lookup"><span data-stu-id="3da2d-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="3da2d-192">Jeśli program SQL Server znajduje się w jednym z tych dwóch trybów, można użyć uwierzytelniania zintegrowanego.</span><span class="sxs-lookup"><span data-stu-id="3da2d-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="3da2d-193">Otwórz program SQL Server Enterprise Manager (Start | Programy | Program Microsoft SQL Server | Enterprise Manager), wybierz odpowiedni serwer, a następnie rozwiń folder zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="3da2d-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="3da2d-194">Jeśli "BUILTINT\IIS\_WPG" grupa nie ma na liście, kliknij prawym przyciskiem myszy na nazwach logowania i wybierz nowy identyfikator logowania:</span><span class="sxs-lookup"><span data-stu-id="3da2d-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="3da2d-195">W "Nazwa:" Wprowadź w polu tekstowym "\IIS [nazwa serwera/domeny]\_WPG" lub kliknij przycisk wielokropka, aby otworzyć selektor użytkownika/grupy systemu Windows NT:</span><span class="sxs-lookup"><span data-stu-id="3da2d-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="3da2d-196">Wybierz pozycję IIS bieżącej maszyny\_WPG grupy i kliknij przycisk "Add" i OK, aby zamknąć selektora.</span><span class="sxs-lookup"><span data-stu-id="3da2d-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="3da2d-197">Następnie należy również ustawić domyślną bazę danych i uprawnień dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3da2d-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="3da2d-198">Można ustawić domyślną bazę danych wybierz z listy rozwijanej listy, np. poniżej Northwind jest zaznaczone:</span><span class="sxs-lookup"><span data-stu-id="3da2d-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="3da2d-199">Następnie kliknij na karcie dostęp do bazy danych:</span><span class="sxs-lookup"><span data-stu-id="3da2d-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="3da2d-200">Kliknij pole wyboru dla każdej bazy danych, który chcesz zezwolić na dostęp do zezwalania na.</span><span class="sxs-lookup"><span data-stu-id="3da2d-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="3da2d-201">Należy również wybrać ról bazy danych, sprawdzanie db\_właściciela zapewni identyfikatora logowania ma wszystkie uprawnienia wymagane do zarządzania i użyć wybranej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3da2d-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="3da2d-202">Kliknij przycisk OK, aby zamknąć okno dialogowe właściwości.</span><span class="sxs-lookup"><span data-stu-id="3da2d-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="3da2d-203">Twoja aplikacja ASP.NET jest teraz skonfigurowane do obsługi zintegrowanego uwierzytelniania programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3da2d-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="3da2d-204">Nie uruchamiaj programu ASP.NET 1.0 w trybie macierzystym usług IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="3da2d-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="3da2d-205">ASP.NET 1.0 z usługami IIS 6.0 jest obsługiwana tylko w trybie zgodności usług IIS 5.</span><span class="sxs-lookup"><span data-stu-id="3da2d-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="3da2d-206">Aby skonfigurować ASP.NET 1.0 do uruchamiania w trybie zgodności z programem IIS 5.0, otworzyć Menedżera usług internetowych i witryn sieci Web kliknij prawym przyciskiem myszy i wybierz polecenie Właściwości:</span><span class="sxs-lookup"><span data-stu-id="3da2d-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="3da2d-207">Przejdź do karty usługę i sprawdź? Uruchom usługę WWW w trybie izolacji 5.0 usług IIS?:</span><span class="sxs-lookup"><span data-stu-id="3da2d-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
