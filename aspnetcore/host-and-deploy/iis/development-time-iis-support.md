---
title: Obsługa usług IIS w czasie projektowania w programie Visual Studio dla ASP.NET Core
author: guardrex
description: Odkryj obsługę debugowania aplikacji ASP.NET Core podczas uruchamiania programu z usługami IIS w systemie Windows Server.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/26/2019
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 2d2c8d8e4b39d18a3a727e2a2bdef3be566897c1
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034141"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="a5733-103">Obsługa usług IIS w czasie projektowania w programie Visual Studio dla ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5733-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="a5733-104">Autorzy [sourabh Shirhatti](https://twitter.com/sshirhatti) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a5733-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a5733-105">W tym artykule opisano obsługę [programu Visual Studio](https://visualstudio.microsoft.com) na potrzeby debugowania ASP.NET Core aplikacje działające z usługami IIS w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="a5733-105">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="a5733-106">Ten temat zawiera instrukcje dotyczące włączania tego scenariusza i konfigurowania projektu.</span><span class="sxs-lookup"><span data-stu-id="a5733-106">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5733-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a5733-107">Prerequisites</span></span>

* [<span data-ttu-id="a5733-108">Program Visual Studio dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="a5733-108">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="a5733-109">**ASP.NET i programowanie aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="a5733-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="a5733-110">**Tworzenie aplikacji dla wielu platform w środowisku .NET Core**</span><span class="sxs-lookup"><span data-stu-id="a5733-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="a5733-111">Certyfikat zabezpieczeń X. 509 (dla obsługi protokołu HTTPS)</span><span class="sxs-lookup"><span data-stu-id="a5733-111">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="a5733-112">Włącz usługi IIS</span><span class="sxs-lookup"><span data-stu-id="a5733-112">Enable IIS</span></span>

1. <span data-ttu-id="a5733-113">W systemie Windows przejdź do pozycji **Panel sterowania** > **programy** > **programy i funkcje** > **włączać lub wyłączać funkcje systemu Windows** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="a5733-113">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="a5733-114">Zaznacz pole wyboru **Internet Information Services** .</span><span class="sxs-lookup"><span data-stu-id="a5733-114">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="a5733-115">Wybierz **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="a5733-115">Select **OK**.</span></span>

<span data-ttu-id="a5733-116">Instalacja usług IIS może wymagać ponownego uruchomienia systemu.</span><span class="sxs-lookup"><span data-stu-id="a5733-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="a5733-117">Konfigurowanie usług IIS</span><span class="sxs-lookup"><span data-stu-id="a5733-117">Configure IIS</span></span>

<span data-ttu-id="a5733-118">Usługi IIS muszą mieć skonfigurowaną witrynę sieci Web z następującymi:</span><span class="sxs-lookup"><span data-stu-id="a5733-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="a5733-119">**Nazwa hosta** &ndash; zazwyczaj **Domyślna witryna sieci Web** jest używana z **nazwą hosta** `localhost`.</span><span class="sxs-lookup"><span data-stu-id="a5733-119">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="a5733-120">Jednak każda prawidłowa witryna sieci Web usług IIS z unikatową nazwą hosta działa.</span><span class="sxs-lookup"><span data-stu-id="a5733-120">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="a5733-121">**Powiązanie witryny**</span><span class="sxs-lookup"><span data-stu-id="a5733-121">**Site Binding**</span></span>
  * <span data-ttu-id="a5733-122">W przypadku aplikacji wymagających protokołu HTTPS Utwórz powiązanie z portem 443 z certyfikatem.</span><span class="sxs-lookup"><span data-stu-id="a5733-122">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="a5733-123">Zwykle używany jest **certyfikat deweloperski IIS Express** , ale każdy prawidłowy certyfikat działa.</span><span class="sxs-lookup"><span data-stu-id="a5733-123">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="a5733-124">W przypadku aplikacji korzystających z protokołu HTTP upewnij się, że istnieje powiązanie do opublikowania 80 lub utwórz powiązanie z portem 80 dla nowej lokacji.</span><span class="sxs-lookup"><span data-stu-id="a5733-124">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="a5733-125">Użyj pojedynczego powiązania dla protokołu HTTP lub HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a5733-125">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="a5733-126">**Powiązanie jednocześnie z portami HTTP i HTTPS nie jest obsługiwane.**</span><span class="sxs-lookup"><span data-stu-id="a5733-126">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="a5733-127">Włączanie obsługi usług IIS w czasie opracowywania w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a5733-127">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="a5733-128">Uruchom Instalatora programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a5733-128">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="a5733-129">Wybierz pozycję **Modyfikuj** dla instalacji programu Visual Studio, która ma być używana na potrzeby obsługi czasu projektowania usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a5733-129">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="a5733-130">W przypadku obciążeń **ASP.NET i sieci Web** Znajdź i Zainstaluj składnik **Obsługa usług IIS w czasie opracowywania** .</span><span class="sxs-lookup"><span data-stu-id="a5733-130">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="a5733-131">Składnik jest wymieniony w sekcji **opcjonalne** w obszarze **czas projektowania obsługa usług IIS** w panelu **szczegóły instalacji** po prawej stronie obciążeń.</span><span class="sxs-lookup"><span data-stu-id="a5733-131">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="a5733-132">Składnik instaluje [moduł ASP.NET Core](xref:host-and-deploy/aspnet-core-module), który jest natywnym modułem usług IIS wymaganym do uruchamiania aplikacji ASP.NET Core za pomocą usług IIS.</span><span class="sxs-lookup"><span data-stu-id="a5733-132">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="a5733-133">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="a5733-133">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="a5733-134">Przekierowanie HTTPS</span><span class="sxs-lookup"><span data-stu-id="a5733-134">HTTPS redirection</span></span>

<span data-ttu-id="a5733-135">Dla nowego projektu, który wymaga protokołu HTTPS, zaznacz pole wyboru w celu **skonfigurowania protokołu HTTPS** w oknie **Tworzenie nowej ASP.NET Core aplikacji sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="a5733-135">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="a5733-136">Zaznaczenie tego pola wyboru powoduje dodanie [przekierowania https i HSTS oprogramowania](xref:security/enforcing-ssl) do aplikacji podczas jej tworzenia.</span><span class="sxs-lookup"><span data-stu-id="a5733-136">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="a5733-137">W przypadku istniejącego projektu wymagającego protokołu HTTPS Użyj przekierowania HTTPS i oprogramowania pośredniczącego HSTS w `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a5733-137">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="a5733-138">Aby uzyskać więcej informacji, zobacz <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="a5733-138">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="a5733-139">W przypadku projektu korzystającego z protokołu HTTP, [przekierowania https i oprogramowania pośredniczącego HSTS](xref:security/enforcing-ssl) nie są dodawane do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5733-139">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="a5733-140">Nie jest wymagana żadna konfiguracja aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5733-140">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="a5733-141">Profil uruchamiania usług IIS</span><span class="sxs-lookup"><span data-stu-id="a5733-141">IIS launch profile</span></span>

<span data-ttu-id="a5733-142">Utwórz nowy profil uruchamiania, aby dodać obsługę usług IIS w czasie projektowania:</span><span class="sxs-lookup"><span data-stu-id="a5733-142">Create a new launch profile to add development-time IIS support:</span></span>

::: moniker range=">= aspnetcore-3.0"

1. <span data-ttu-id="a5733-143">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="a5733-143">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="a5733-144">Wybierz pozycję **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="a5733-144">Select **Properties**.</span></span> <span data-ttu-id="a5733-145">Otwórz kartę **debugowanie** .</span><span class="sxs-lookup"><span data-stu-id="a5733-145">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="a5733-146">W obszarze **profil**wybierz przycisk **Nowy** .</span><span class="sxs-lookup"><span data-stu-id="a5733-146">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="a5733-147">Nazwij profil "IIS" w oknie podręcznym.</span><span class="sxs-lookup"><span data-stu-id="a5733-147">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="a5733-148">Wybierz **przycisk OK** , aby utworzyć profil.</span><span class="sxs-lookup"><span data-stu-id="a5733-148">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="a5733-149">Dla ustawienia **uruchamiania** wybierz z listy pozycję **IIS** .</span><span class="sxs-lookup"><span data-stu-id="a5733-149">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="a5733-150">Zaznacz pole wyboru dla opcji **Uruchom przeglądarkę** i podaj adres URL punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="a5733-150">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="a5733-151">Gdy aplikacja wymaga protokołu HTTPS, użyj punktu końcowego HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="a5733-151">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="a5733-152">W przypadku protokołu HTTP Użyj punktu końcowego HTTP (`http://`).</span><span class="sxs-lookup"><span data-stu-id="a5733-152">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="a5733-153">Podaj taką samą nazwę hosta i port jak [Konfiguracja usług IIS określona wcześniej](#configure-iis), zazwyczaj `localhost`.</span><span class="sxs-lookup"><span data-stu-id="a5733-153">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="a5733-154">Podaj nazwę aplikacji na końcu adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a5733-154">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="a5733-155">Na przykład `https://localhost/WebApplication1` (HTTPS) lub `http://localhost/WebApplication1` (HTTP) są prawidłowymi adresami URL punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="a5733-155">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="a5733-156">W sekcji **zmienne środowiskowe** wybierz przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="a5733-156">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="a5733-157">Podaj zmienną środowiskową o **nazwie** `ASPNETCORE_ENVIRONMENT` i **wartości** `Development`.</span><span class="sxs-lookup"><span data-stu-id="a5733-157">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="a5733-158">W obszarze **Ustawienia serwera sieci Web** Ustaw **adres URL aplikacji** na wartość używaną dla adresu URL punktu końcowego **uruchamiania przeglądarki** .</span><span class="sxs-lookup"><span data-stu-id="a5733-158">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="a5733-159">W przypadku ustawienia **model hostingu** w programie Visual Studio 2019 lub nowszym wybierz pozycję **domyślne** , aby użyć modelu hostingu używanego przez projekt.</span><span class="sxs-lookup"><span data-stu-id="a5733-159">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="a5733-160">Jeśli projekt ustawia właściwość `<AspNetCoreHostingModel>` w pliku projektu, używana jest wartość właściwości (`InProcess` lub `OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="a5733-160">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="a5733-161">Jeśli właściwość nie jest obecna, używany jest domyślny model hostingu aplikacji, który jest w procesie.</span><span class="sxs-lookup"><span data-stu-id="a5733-161">If the property isn't present, the default hosting model of the app is used, which is in-process.</span></span> <span data-ttu-id="a5733-162">Jeśli aplikacja wymaga jawnego ustawienia modelu hostingu innego niż normalny model hostingu aplikacji, należy ustawić **model hostingu** na wartość `In Process` lub `Out Of Process` w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="a5733-162">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="a5733-163">Zapisz profil.</span><span class="sxs-lookup"><span data-stu-id="a5733-163">Save the profile.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. <span data-ttu-id="a5733-164">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="a5733-164">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="a5733-165">Wybierz pozycję **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="a5733-165">Select **Properties**.</span></span> <span data-ttu-id="a5733-166">Otwórz kartę **debugowanie** .</span><span class="sxs-lookup"><span data-stu-id="a5733-166">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="a5733-167">W obszarze **profil**wybierz przycisk **Nowy** .</span><span class="sxs-lookup"><span data-stu-id="a5733-167">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="a5733-168">Nazwij profil "IIS" w oknie podręcznym.</span><span class="sxs-lookup"><span data-stu-id="a5733-168">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="a5733-169">Wybierz **przycisk OK** , aby utworzyć profil.</span><span class="sxs-lookup"><span data-stu-id="a5733-169">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="a5733-170">Dla ustawienia **uruchamiania** wybierz z listy pozycję **IIS** .</span><span class="sxs-lookup"><span data-stu-id="a5733-170">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="a5733-171">Zaznacz pole wyboru dla opcji **Uruchom przeglądarkę** i podaj adres URL punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="a5733-171">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="a5733-172">Gdy aplikacja wymaga protokołu HTTPS, użyj punktu końcowego HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="a5733-172">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="a5733-173">W przypadku protokołu HTTP Użyj punktu końcowego HTTP (`http://`).</span><span class="sxs-lookup"><span data-stu-id="a5733-173">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="a5733-174">Podaj taką samą nazwę hosta i port jak [Konfiguracja usług IIS określona wcześniej](#configure-iis), zazwyczaj `localhost`.</span><span class="sxs-lookup"><span data-stu-id="a5733-174">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="a5733-175">Podaj nazwę aplikacji na końcu adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a5733-175">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="a5733-176">Na przykład `https://localhost/WebApplication1` (HTTPS) lub `http://localhost/WebApplication1` (HTTP) są prawidłowymi adresami URL punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="a5733-176">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="a5733-177">W sekcji **zmienne środowiskowe** wybierz przycisk **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="a5733-177">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="a5733-178">Podaj zmienną środowiskową o **nazwie** `ASPNETCORE_ENVIRONMENT` i **wartości** `Development`.</span><span class="sxs-lookup"><span data-stu-id="a5733-178">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="a5733-179">W obszarze **Ustawienia serwera sieci Web** Ustaw **adres URL aplikacji** na wartość używaną dla adresu URL punktu końcowego **uruchamiania przeglądarki** .</span><span class="sxs-lookup"><span data-stu-id="a5733-179">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="a5733-180">W przypadku ustawienia **model hostingu** w programie Visual Studio 2019 lub nowszym wybierz pozycję **domyślne** , aby użyć modelu hostingu używanego przez projekt.</span><span class="sxs-lookup"><span data-stu-id="a5733-180">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="a5733-181">Jeśli projekt ustawia właściwość `<AspNetCoreHostingModel>` w pliku projektu, używana jest wartość właściwości (`InProcess` lub `OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="a5733-181">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="a5733-182">Jeśli właściwość nie jest obecna, używany jest domyślny model hostingu aplikacji, który jest poza procesem.</span><span class="sxs-lookup"><span data-stu-id="a5733-182">If the property isn't present, the default hosting model of the app is used, which is out-of-process.</span></span> <span data-ttu-id="a5733-183">Jeśli aplikacja wymaga jawnego ustawienia modelu hostingu innego niż normalny model hostingu aplikacji, należy ustawić **model hostingu** na wartość `In Process` lub `Out Of Process` w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="a5733-183">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="a5733-184">Zapisz profil.</span><span class="sxs-lookup"><span data-stu-id="a5733-184">Save the profile.</span></span>

::: moniker-end

<span data-ttu-id="a5733-185">Gdy nie korzystasz z programu Visual Studio, ręcznie Dodaj profil uruchamiania do pliku [profilu launchsettings. JSON](https://json.schemastore.org/launchsettings) w folderze *Properties* .</span><span class="sxs-lookup"><span data-stu-id="a5733-185">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](https://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="a5733-186">Poniższy przykład konfiguruje profil do korzystania z protokołu HTTPS:</span><span class="sxs-lookup"><span data-stu-id="a5733-186">The following example configures the profile to use the HTTPS protocol:</span></span>

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

<span data-ttu-id="a5733-187">Upewnij się, że punkty końcowe `applicationUrl` i `launchUrl` są zgodne i używają tego samego protokołu, co Konfiguracja powiązania usług IIS, HTTP lub HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a5733-187">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="a5733-188">Uruchom projekt</span><span class="sxs-lookup"><span data-stu-id="a5733-188">Run the project</span></span>

<span data-ttu-id="a5733-189">Uruchom program Visual Studio jako administrator:</span><span class="sxs-lookup"><span data-stu-id="a5733-189">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="a5733-190">Upewnij się, że na liście rozwijanej konfiguracja kompilacji jest ustawiona wartość **Debuguj**.</span><span class="sxs-lookup"><span data-stu-id="a5733-190">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="a5733-191">Ustaw [przycisk Rozpocznij debugowanie](/visualstudio/debugger/debugger-feature-tour) na profil **usług IIS** i wybierz przycisk, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="a5733-191">Set the [Start Debugging button](/visualstudio/debugger/debugger-feature-tour) to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="a5733-192">Program Visual Studio może monitować o ponowne uruchomienie, jeśli nie jest uruchomiony jako administrator.</span><span class="sxs-lookup"><span data-stu-id="a5733-192">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="a5733-193">Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a5733-193">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="a5733-194">Jeśli jest używany niezaufany certyfikat programistyczny, w przeglądarce może być konieczne utworzenie wyjątku dla certyfikatu niezaufanego.</span><span class="sxs-lookup"><span data-stu-id="a5733-194">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="a5733-195">Debugowanie konfiguracji kompilacji wydania z optymalizacjami [tylko mój kod](/visualstudio/debugger/just-my-code) i kompilatora skutkuje obniżeniem wydajności.</span><span class="sxs-lookup"><span data-stu-id="a5733-195">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="a5733-196">Na przykład punkty przerwania nie trafią.</span><span class="sxs-lookup"><span data-stu-id="a5733-196">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a5733-197">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a5733-197">Additional resources</span></span>

* [<span data-ttu-id="a5733-198">Wprowadzenie z menedżerem usług IIS w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="a5733-198">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:security/enforcing-ssl>
