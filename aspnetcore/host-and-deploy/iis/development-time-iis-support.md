---
title: Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core
author: shirhatti
description: Wykryj obsługę debugowania aplikacji ASP.NET Core, gdy za usług IIS w systemie Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="bcfb4-103">Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bcfb4-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="bcfb4-104">Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bcfb4-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bcfb4-105">W tym artykule opisano [programu Visual Studio](https://www.visualstudio.com/vs/) obsługę debugowania aplikacji ASP.NET Core za usług IIS w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="bcfb4-106">Ten temat przeprowadzi Cię przez włączenie tej funkcji i konfigurowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bcfb4-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="bcfb4-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="bcfb4-108">Włącz usługi IIS</span><span class="sxs-lookup"><span data-stu-id="bcfb4-108">Enable IIS</span></span>

1. <span data-ttu-id="bcfb4-109">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="bcfb4-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="bcfb4-110">Wybierz **Internetowe usługi informacyjne** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-110">Select the **Internet Information Services** check box.</span></span>

![Pole wyboru Internetowe usługi informacyjne funkcje systemu Windows prezentujących zaznaczone jako czarny kwadrat (nie zaznaczone), wskazujący, że niektóre funkcje usług IIS są włączone](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="bcfb4-112">Instalacja usług IIS może wymagać ponownego uruchomienia systemu.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="bcfb4-113">Konfigurowanie usług IIS</span><span class="sxs-lookup"><span data-stu-id="bcfb4-113">Configure IIS</span></span>

<span data-ttu-id="bcfb4-114">Usługi IIS musi mieć skonfigurowaną następujące witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="bcfb4-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="bcfb4-115">Nazwa hosta nazwę hosta, która odpowiada adresowi URL profilu uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="bcfb4-116">Powiązanie dla portu 443 z certyfikatem przypisane.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="bcfb4-117">Na przykład **nazwy hosta** dla witryny sieci Web dodano jest ustawiony na wartość "localhost" (profilu uruchamiania będą też używać w dalszej części tego tematu "localhost").</span><span class="sxs-lookup"><span data-stu-id="bcfb4-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="bcfb4-118">Port ten jest ustawiony na "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="bcfb4-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="bcfb4-119">**Usług IIS Express certyfikatu deweloperskiego** jest przypisany do witryny sieci Web, ale dowolnego ważnego certyfikatu działania:</span><span class="sxs-lookup"><span data-stu-id="bcfb4-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Dodaj okno witryny sieci Web w usługach IIS przedstawiający powiązania zestawu hosta lokalnego na porcie 443 z certyfikatem, który został przypisany.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="bcfb4-121">Jeśli ma już instalacji usług IIS **domyślna witryna sieci Web** przy użyciu nazwy hosta, który dopasowuje nazwy hosta URL profilu uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bcfb4-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="bcfb4-122">Dodaj powiązanie portów dla portu 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="bcfb4-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="bcfb4-123">Przypisz prawidłowy certyfikat do witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="bcfb4-124">Włącz obsługę usług IIS czasie opracowywania w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bcfb4-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="bcfb4-125">Uruchom Instalator programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="bcfb4-126">Wybierz **czasie opracowywania usługi IIS obsługują** składnika.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="bcfb4-127">Składnik jest wymieniony jako opcjonalne w **Podsumowanie** panelu dla **ASP.NET i sieć web development** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="bcfb4-128">Instaluje składnik [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), która jest moduł macierzysty usług IIS wymagane do uruchomienia aplikacji za IIS platformy ASP.NET Core w konfiguracji zwrotnego serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Modyfikowanie funkcje programu Visual Studio: wybrana jest karta obciążeń.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="bcfb4-132">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="bcfb4-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="bcfb4-133">Przekierowania protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="bcfb4-133">HTTPS redirection</span></span>

<span data-ttu-id="bcfb4-134">Dla nowego projektu, zaznacz pole wyboru, aby **Konfiguruj na potrzeby protokołu HTTPS** w **nową aplikację sieci Web Core ASP.NET** okno:</span><span class="sxs-lookup"><span data-stu-id="bcfb4-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Nowe okno aplikacji sieci Web platformy ASP.NET Core z konfiguracji dla protokołu HTTPS jest zaznaczone pole wyboru.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="bcfb4-136">W istniejącego projektu, za pomocą oprogramowania pośredniczącego przekierowania protokołu HTTPS w `Startup.Configure` przez wywołanie metody [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="bcfb4-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a><span data-ttu-id="bcfb4-137">Profil uruchamiania usług IIS</span><span class="sxs-lookup"><span data-stu-id="bcfb4-137">IIS launch profile</span></span>

<span data-ttu-id="bcfb4-138">Utwórz nowy profil Uruchom, aby dodać obsługę usług IIS w czasie opracowywania:</span><span class="sxs-lookup"><span data-stu-id="bcfb4-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="bcfb4-139">Dla **profilu**, wybierz pozycję **nowy** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="bcfb4-140">Nazwa profilu "Usług IIS" w oknie podręcznym.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="bcfb4-141">Wybierz **OK** do tworzenia profilu.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="bcfb4-142">Dla **uruchamianie** wybierz pozycję **IIS** z listy.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="bcfb4-143">Zaznacz pole wyboru dla **uruchamiania przeglądarki** i podaj adres URL punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="bcfb4-144">Użyj protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="bcfb4-145">W tym przykładzie użyto `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="bcfb4-146">W **zmiennych środowiskowych** zaznacz **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="bcfb4-147">Podaj wartość zmiennej środowiskowej z kluczem `ASPNETCORE_ENVIRONMENT` i wartości `Development`.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="bcfb4-148">W **ustawienia serwera sieci Web** obszar, **adres URL aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="bcfb4-149">W tym przykładzie użyto `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="bcfb4-150">Zapisywanie profilu.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-150">Save the profile.</span></span>

![Okno właściwości projektu z wybraną kartą debugowania.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="bcfb4-155">Możesz też ręcznie dodać profil uruchamiania do [launchSettings.json](http://json.schemastore.org/launchsettings) plik w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bcfb4-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="bcfb4-156">Uruchom projekt</span><span class="sxs-lookup"><span data-stu-id="bcfb4-156">Run the project</span></span>

<span data-ttu-id="bcfb4-157">W interfejsie użytkownika programu VS ustawioną przycisk Uruchom **IIS** profilu, a następnie wybierz przycisk, aby uruchomić aplikację:</span><span class="sxs-lookup"><span data-stu-id="bcfb4-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Przycisk Uruchom na pasku narzędzi programu VS ustawiona do profilowania "Usług IIS".](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="bcfb4-159">Visual Studio może monit o ponowne uruchomienie, gdy nie jest uruchomiona jako administrator.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="bcfb4-160">Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="bcfb4-161">Użycie certyfikatu deweloperskiego niezaufanych przeglądarki mogą wymagać utworzenia wyjątku niezaufanego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="bcfb4-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bcfb4-162">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bcfb4-162">Additional resources</span></span>

* [<span data-ttu-id="bcfb4-163">Host platformy ASP.NET Core w systemie Windows z programem IIS</span><span class="sxs-lookup"><span data-stu-id="bcfb4-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="bcfb4-164">Wprowadzenie do platformy ASP.NET Core modułu</span><span class="sxs-lookup"><span data-stu-id="bcfb4-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="bcfb4-165">Odwołania do konfiguracji modułu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bcfb4-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="bcfb4-166">Wymuszanie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="bcfb4-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
