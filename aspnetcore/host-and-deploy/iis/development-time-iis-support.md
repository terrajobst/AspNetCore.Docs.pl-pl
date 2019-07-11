---
title: Obsługa usług IIS czas opracowywania, w programie Visual Studio dla platformy ASP.NET Core
author: guardrex
description: Dowiedz się, obsługę debugowania aplikacji ASP.NET Core, podczas korzystania z użyciem usług IIS w systemie Windows Server.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: f2d5dbbdc80eec035616ddea234ee5d3343eeae8
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815181"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="82727-103">Obsługa usług IIS czas opracowywania, w programie Visual Studio dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="82727-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="82727-104">Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="82727-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="82727-105">W tym artykule opisano [programu Visual Studio](https://visualstudio.microsoft.com) obsługę debugowania aplikacji ASP.NET Core uruchomiony z usługami IIS w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="82727-105">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="82727-106">W tym temacie przedstawiono w tym scenariuszu włączania i konfigurowania projektu.</span><span class="sxs-lookup"><span data-stu-id="82727-106">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82727-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="82727-107">Prerequisites</span></span>

* [<span data-ttu-id="82727-108">Visual Studio for Windows</span><span class="sxs-lookup"><span data-stu-id="82727-108">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="82727-109">**ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="82727-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="82727-110">**Programowanie dla wielu platform .NET core** obciążenia</span><span class="sxs-lookup"><span data-stu-id="82727-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="82727-111">Certyfikat zabezpieczeń X.509 (w przypadku obsługi protokołu HTTPS)</span><span class="sxs-lookup"><span data-stu-id="82727-111">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="82727-112">Włącz usługi IIS</span><span class="sxs-lookup"><span data-stu-id="82727-112">Enable IIS</span></span>

1. <span data-ttu-id="82727-113">W Windows, przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Włącz Windows Włącz lub wyłącz funkcje** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="82727-113">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="82727-114">Wybierz **Internetowe usługi informacyjne** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="82727-114">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="82727-115">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="82727-115">Select **OK**.</span></span>

<span data-ttu-id="82727-116">Instalacja usług IIS może wymagać ponownego uruchomienia systemu.</span><span class="sxs-lookup"><span data-stu-id="82727-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="82727-117">Konfigurowanie usług IIS</span><span class="sxs-lookup"><span data-stu-id="82727-117">Configure IIS</span></span>

<span data-ttu-id="82727-118">Usługi IIS musi mieć witrynę sieci Web skonfigurowano następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="82727-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="82727-119">**Nazwa hosta** &ndash; zazwyczaj **domyślna witryna sieci Web** jest używana z **nazwy hosta** z `localhost`.</span><span class="sxs-lookup"><span data-stu-id="82727-119">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="82727-120">Jednak wszystkie prawidłowe witryna internetowa usług IIS z nazwą hosta unikatowy działa.</span><span class="sxs-lookup"><span data-stu-id="82727-120">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="82727-121">**Powiązania witryny**</span><span class="sxs-lookup"><span data-stu-id="82727-121">**Site Binding**</span></span>
  * <span data-ttu-id="82727-122">W przypadku aplikacji, które wymagają protokołu HTTPS należy utworzyć powiązania z portem 443 przy użyciu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="82727-122">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="82727-123">Zazwyczaj **usług IIS Express tworzenia certyfikatu** jest używane, ale dowolne, prawidłowe certyfikatu działa.</span><span class="sxs-lookup"><span data-stu-id="82727-123">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="82727-124">Dla aplikacji, które używają protokołu HTTP, upewnij się, istnienie powiązania do opublikowania 80 lub utworzyć powiązania z portem 80 dla nowej witryny.</span><span class="sxs-lookup"><span data-stu-id="82727-124">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="82727-125">Na użytek pojedynczego powiązania protokołu HTTP lub HTTPS.</span><span class="sxs-lookup"><span data-stu-id="82727-125">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="82727-126">**Powiązanie z portów HTTP i HTTPS, jednocześnie nie jest obsługiwane.**</span><span class="sxs-lookup"><span data-stu-id="82727-126">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="82727-127">Włącz obsługę usług IIS w czasie projektowania w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="82727-127">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="82727-128">Uruchom Instalatora programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="82727-128">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="82727-129">Wybierz **Modyfikuj** dla instalacji programu Visual Studio, którego chcesz użyć dla usług IIS oferuje obsługę w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="82727-129">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="82727-130">Dla **ASP.NET i tworzenie aplikacji internetowych** obciążenia znajdowania i instalowania **czas opracowywania usługi IIS obsługują** składnika.</span><span class="sxs-lookup"><span data-stu-id="82727-130">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="82727-131">Składnik znajduje się w **opcjonalnie** sekcji **czas opracowywania usługi IIS obsługują** w **szczegółowe informacje dotyczące instalacji** panelu po prawej stronie obciążeń.</span><span class="sxs-lookup"><span data-stu-id="82727-131">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="82727-132">Instaluje składnik [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), czyli moduł macierzysty usług IIS wymagane do uruchamiania aplikacji ASP.NET Core za pomocą programu IIS.</span><span class="sxs-lookup"><span data-stu-id="82727-132">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="82727-133">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="82727-133">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="82727-134">Przekierowania protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="82727-134">HTTPS redirection</span></span>

<span data-ttu-id="82727-135">Dla nowego projektu, który wymaga protokołu HTTPS, zaznacz pole wyboru, aby **Konfigurowanie protokołu HTTPS** w **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okna.</span><span class="sxs-lookup"><span data-stu-id="82727-135">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="82727-136">Zaznaczenie pola wyboru dodaje [przekierowania protokołu HTTPS i oprogramowanie pośredniczące HSTS](xref:security/enforcing-ssl) do aplikacji po jej utworzeniu.</span><span class="sxs-lookup"><span data-stu-id="82727-136">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="82727-137">Istniejący projekt, który wymaga protokołu HTTPS, można użyć w przekierowania protokołu HTTPS i oprogramowanie pośredniczące HSTS, w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="82727-137">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="82727-138">Aby uzyskać więcej informacji, zobacz <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="82727-138">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="82727-139">W projekcie, który korzysta z protokołu HTTP [przekierowania protokołu HTTPS i oprogramowanie pośredniczące HSTS](xref:security/enforcing-ssl) nie są dodawane do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82727-139">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="82727-140">Aplikacja jest wymagana żadna konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="82727-140">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="82727-141">Profil uruchamiania usług IIS</span><span class="sxs-lookup"><span data-stu-id="82727-141">IIS launch profile</span></span>

<span data-ttu-id="82727-142">Utwórz nowy profil uruchamiania do dodania obsługi usług IIS w czasie projektowania:</span><span class="sxs-lookup"><span data-stu-id="82727-142">Create a new launch profile to add development-time IIS support:</span></span>

::: moniker range=">= aspnetcore-3.0"

1. <span data-ttu-id="82727-143">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="82727-143">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="82727-144">Wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="82727-144">Select **Properties**.</span></span> <span data-ttu-id="82727-145">Otwórz **debugowania** kartę.</span><span class="sxs-lookup"><span data-stu-id="82727-145">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="82727-146">Aby uzyskać **profilu**, wybierz opcję **nowy** przycisku.</span><span class="sxs-lookup"><span data-stu-id="82727-146">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="82727-147">Nazwa profilu "Usług IIS" w oknie podręcznym.</span><span class="sxs-lookup"><span data-stu-id="82727-147">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="82727-148">Wybierz **OK** do utworzenia profilu.</span><span class="sxs-lookup"><span data-stu-id="82727-148">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="82727-149">Dla **Uruchom** ustawienie, wybierz **IIS** z listy.</span><span class="sxs-lookup"><span data-stu-id="82727-149">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="82727-150">Zaznacz pole wyboru dla **uruchamiania przeglądarki** i podaj adres URL punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="82727-150">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="82727-151">Jeśli aplikacja wymaga protokołu HTTPS, użyj punktu końcowego HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="82727-151">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="82727-152">Dla protokołu HTTP, użyj protokołu HTTP (`http://`) punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="82727-152">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="82727-153">Podaj takiej samej nazwy hosta i portu jako [określona konfiguracja usług IIS wcześniejszych zastosowań](#configure-iis), zazwyczaj `localhost`.</span><span class="sxs-lookup"><span data-stu-id="82727-153">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="82727-154">Podaj nazwę aplikacji na końcu adresu URL.</span><span class="sxs-lookup"><span data-stu-id="82727-154">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="82727-155">Na przykład `https://localhost/WebApplication1` (HTTPS) lub `http://localhost/WebApplication1` (HTTP) są adresami URL prawidłowego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="82727-155">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="82727-156">W **zmienne środowiskowe** zaznacz **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="82727-156">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="82727-157">Podaj zmienną środowiskową **nazwa** z `ASPNETCORE_ENVIRONMENT` i **wartość** z `Development`.</span><span class="sxs-lookup"><span data-stu-id="82727-157">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="82727-158">W **ustawienia serwera sieci Web** obszar, **adres URL aplikacji** taką samą wartość, umożliwiający **uruchamiania przeglądarki** adres URL punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="82727-158">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="82727-159">Dla **modelu hostingu** ustawienia w programie Visual Studio 2019 lub nowszym, wybierz **domyślne** do korzystania z modelu hostingu używane przez projekt.</span><span class="sxs-lookup"><span data-stu-id="82727-159">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="82727-160">Jeśli projekt ustawia `<AspNetCoreHostingModel>` właściwość w pliku projektu, wartość właściwości (`InProcess` lub `OutOfProcess`) jest używany.</span><span class="sxs-lookup"><span data-stu-id="82727-160">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="82727-161">Jeśli właściwość nie istnieje, używana jest domyślna, hostingu modelu aplikacji, która jest w trakcie.</span><span class="sxs-lookup"><span data-stu-id="82727-161">If the property isn't present, the default hosting model of the app is used, which is in-process.</span></span> <span data-ttu-id="82727-162">Jeśli aplikacja wymaga jawnego modelu hostingu ustawienie różni się od normalnych modelu hostingu aplikacji, ustawić **modelu hostingu** do jednej `In Process` lub `Out Of Process` zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="82727-162">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="82727-163">Zapisz profil.</span><span class="sxs-lookup"><span data-stu-id="82727-163">Save the profile.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. <span data-ttu-id="82727-164">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="82727-164">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="82727-165">Wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="82727-165">Select **Properties**.</span></span> <span data-ttu-id="82727-166">Otwórz **debugowania** kartę.</span><span class="sxs-lookup"><span data-stu-id="82727-166">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="82727-167">Aby uzyskać **profilu**, wybierz opcję **nowy** przycisku.</span><span class="sxs-lookup"><span data-stu-id="82727-167">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="82727-168">Nazwa profilu "Usług IIS" w oknie podręcznym.</span><span class="sxs-lookup"><span data-stu-id="82727-168">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="82727-169">Wybierz **OK** do utworzenia profilu.</span><span class="sxs-lookup"><span data-stu-id="82727-169">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="82727-170">Dla **Uruchom** ustawienie, wybierz **IIS** z listy.</span><span class="sxs-lookup"><span data-stu-id="82727-170">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="82727-171">Zaznacz pole wyboru dla **uruchamiania przeglądarki** i podaj adres URL punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="82727-171">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="82727-172">Jeśli aplikacja wymaga protokołu HTTPS, użyj punktu końcowego HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="82727-172">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="82727-173">Dla protokołu HTTP, użyj protokołu HTTP (`http://`) punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="82727-173">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="82727-174">Podaj takiej samej nazwy hosta i portu jako [określona konfiguracja usług IIS wcześniejszych zastosowań](#configure-iis), zazwyczaj `localhost`.</span><span class="sxs-lookup"><span data-stu-id="82727-174">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="82727-175">Podaj nazwę aplikacji na końcu adresu URL.</span><span class="sxs-lookup"><span data-stu-id="82727-175">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="82727-176">Na przykład `https://localhost/WebApplication1` (HTTPS) lub `http://localhost/WebApplication1` (HTTP) są adresami URL prawidłowego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="82727-176">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="82727-177">W **zmienne środowiskowe** zaznacz **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="82727-177">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="82727-178">Podaj zmienną środowiskową **nazwa** z `ASPNETCORE_ENVIRONMENT` i **wartość** z `Development`.</span><span class="sxs-lookup"><span data-stu-id="82727-178">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="82727-179">W **ustawienia serwera sieci Web** obszar, **adres URL aplikacji** taką samą wartość, umożliwiający **uruchamiania przeglądarki** adres URL punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="82727-179">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="82727-180">Dla **modelu hostingu** ustawienia w programie Visual Studio 2019 lub nowszym, wybierz **domyślne** do korzystania z modelu hostingu używane przez projekt.</span><span class="sxs-lookup"><span data-stu-id="82727-180">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="82727-181">Jeśli projekt ustawia `<AspNetCoreHostingModel>` właściwość w pliku projektu, wartość właściwości (`InProcess` lub `OutOfProcess`) jest używany.</span><span class="sxs-lookup"><span data-stu-id="82727-181">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="82727-182">Jeśli właściwość nie jest obecny, używana jest domyślna, hostingu modelu aplikacji, która jest spoza procesu.</span><span class="sxs-lookup"><span data-stu-id="82727-182">If the property isn't present, the default hosting model of the app is used, which is out-of-process.</span></span> <span data-ttu-id="82727-183">Jeśli aplikacja wymaga jawnego modelu hostingu ustawienie różni się od normalnych modelu hostingu aplikacji, ustawić **modelu hostingu** do jednej `In Process` lub `Out Of Process` zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="82727-183">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="82727-184">Zapisz profil.</span><span class="sxs-lookup"><span data-stu-id="82727-184">Save the profile.</span></span>

::: moniker-end

<span data-ttu-id="82727-185">Kiedy nie przy użyciu programu Visual Studio, ręcznie dodać profil uruchamiania, aby [launchSettings.json](https://json.schemastore.org/launchsettings) w pliku *właściwości* folderu.</span><span class="sxs-lookup"><span data-stu-id="82727-185">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](https://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="82727-186">Poniższy przykład umożliwia skonfigurowanie profilu do używania protokołu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="82727-186">The following example configures the profile to use the HTTPS protocol:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

<span data-ttu-id="82727-187">Upewnij się, że `applicationUrl` i `launchUrl` punkty końcowe dopasowania i używać tego samego protokołu jako Konfiguracja powiązania usługi IIS: HTTP lub HTTPS.</span><span class="sxs-lookup"><span data-stu-id="82727-187">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="82727-188">Uruchom projekt</span><span class="sxs-lookup"><span data-stu-id="82727-188">Run the project</span></span>

<span data-ttu-id="82727-189">Uruchom program Visual Studio jako administrator:</span><span class="sxs-lookup"><span data-stu-id="82727-189">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="82727-190">Upewnij się, że konfiguracji kompilacji na liście rozwijanej jest równa **debugowania**.</span><span class="sxs-lookup"><span data-stu-id="82727-190">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="82727-191">Ustaw przycisk Uruchom **IIS** profilu, a następnie wybierz przycisk aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="82727-191">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="82727-192">Programu Visual Studio może monit o ponowne uruchomienie komputera, jeśli nie jest uruchomiona jako administrator.</span><span class="sxs-lookup"><span data-stu-id="82727-192">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="82727-193">Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="82727-193">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="82727-194">Użycie certyfikatu deweloperskiego niezaufanych przeglądarka może wymagać Utwórz wyjątek dla niezaufanego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="82727-194">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="82727-195">Debugowanie wersji kompilacji konfiguracji za pomocą [tylko mój kod](/visualstudio/debugger/just-my-code) i wyniki optymalizacje kompilatora w środowisku o obniżonym poziomie.</span><span class="sxs-lookup"><span data-stu-id="82727-195">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="82727-196">Na przykład punkty przerwania nie ma odwołań.</span><span class="sxs-lookup"><span data-stu-id="82727-196">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="82727-197">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="82727-197">Additional resources</span></span>

* [<span data-ttu-id="82727-198">Rozpoczęcie korzystania z Menedżera usług IIS w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="82727-198">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [<span data-ttu-id="82727-199">Host platformy ASP.NET Core na Windows za pomocą programu IIS</span><span class="sxs-lookup"><span data-stu-id="82727-199">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="82727-200">Wprowadzenie do modułu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="82727-200">Introduction to ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="82727-201">Odwołania do konfiguracji modułu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="82727-201">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="82727-202">Wymuszanie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="82727-202">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
