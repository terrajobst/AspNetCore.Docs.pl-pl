---
title: Obsługa usług IIS czas opracowywania, w programie Visual Studio dla platformy ASP.NET Core
author: shirhatti
description: Dowiedz się, obsługę debugowania aplikacji ASP.NET Core, gdy działające poza usługą IIS w systemie Windows Server.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 44570bb28451ce4c5fde12ec77e3856fb5bd3062
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637667"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="b9afb-103">Obsługa usług IIS czas opracowywania, w programie Visual Studio dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9afb-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="b9afb-104">Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b9afb-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b9afb-105">W tym artykule opisano [programu Visual Studio](https://www.visualstudio.com/vs/) obsługę debugowania aplikacji ASP.NET Core działające poza usługą IIS w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="b9afb-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="b9afb-106">W tym temacie omówiono poprzez włączenie tej funkcji i skonfigurowania projektu.</span><span class="sxs-lookup"><span data-stu-id="b9afb-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9afb-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b9afb-107">Prerequisites</span></span>

* [<span data-ttu-id="b9afb-108">Program Visual Studio for Windows</span><span class="sxs-lookup"><span data-stu-id="b9afb-108">Visual Studio for Windows</span></span>](https://www.microsoft.com/net/download/windows)
* <span data-ttu-id="b9afb-109">**ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="b9afb-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="b9afb-110">**Programowanie dla wielu platform .NET core** obciążenia</span><span class="sxs-lookup"><span data-stu-id="b9afb-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="b9afb-111">Certyfikat zabezpieczeń X.509</span><span class="sxs-lookup"><span data-stu-id="b9afb-111">X.509 security certificate</span></span>

## <a name="enable-iis"></a><span data-ttu-id="b9afb-112">Włącz usługi IIS</span><span class="sxs-lookup"><span data-stu-id="b9afb-112">Enable IIS</span></span>

1. <span data-ttu-id="b9afb-113">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="b9afb-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="b9afb-114">Wybierz **Internetowe usługi informacyjne** pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="b9afb-114">Select the **Internet Information Services** check box.</span></span>

![Pole wyboru Internetowe usługi informacyjne przedstawiający funkcji Windows oznaczone jako pierwiastek czarny (nie jest zaznaczone) wskazująca, że niektóre funkcje usług IIS są włączone](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="b9afb-116">Instalacja usług IIS może wymagać ponownego uruchomienia systemu.</span><span class="sxs-lookup"><span data-stu-id="b9afb-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="b9afb-117">Konfigurowanie usług IIS</span><span class="sxs-lookup"><span data-stu-id="b9afb-117">Configure IIS</span></span>

<span data-ttu-id="b9afb-118">Usługi IIS musi mieć witrynę sieci Web skonfigurowano następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="b9afb-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="b9afb-119">Nazwa hosta, który pasuje do nazwy hosta URL profilu uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b9afb-119">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="b9afb-120">Powiązanie dla portu 443 z certyfikatem przypisane.</span><span class="sxs-lookup"><span data-stu-id="b9afb-120">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="b9afb-121">Na przykład **nazwy hosta** dla witryny sieci Web dodano jest ustawiony na "localhost" (profil uruchamiania również użyje "localhost" w dalszej części tego tematu).</span><span class="sxs-lookup"><span data-stu-id="b9afb-121">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="b9afb-122">Port ten jest ustawiony na "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="b9afb-122">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="b9afb-123">**Usług IIS Express tworzenia certyfikatu** jest przypisany do witryny sieci Web, ale dowolnego ważnego certyfikatu działa:</span><span class="sxs-lookup"><span data-stu-id="b9afb-123">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Dodaj okno witryny sieci Web w usługach IIS, przedstawiający zestawu powiązania dla hosta lokalnego na porcie 443 z certyfikatem, który został przypisany.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="b9afb-125">Jeśli ma już instalacji usług IIS **domyślna witryna sieci Web** z nazwą hosta, który pasuje do nazwy hosta URL profilu uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b9afb-125">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="b9afb-126">Dodaj powiązanie portu dla portu 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="b9afb-126">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="b9afb-127">Przypisać certyfikat do witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b9afb-127">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="b9afb-128">Włącz obsługę usług IIS w czasie projektowania w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9afb-128">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="b9afb-129">Uruchom Instalatora programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b9afb-129">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="b9afb-130">Wybierz **czas opracowywania usługi IIS obsługują** składnika.</span><span class="sxs-lookup"><span data-stu-id="b9afb-130">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="b9afb-131">Składnik jest wymieniony jako opcjonalny w **Podsumowanie** panelu dla **ASP.NET i tworzenie aplikacji internetowych** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="b9afb-131">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="b9afb-132">Instaluje składnik [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), czyli moduł macierzysty usług IIS wymagane do uruchamiania aplikacji ASP.NET Core za pomocą programu IIS.</span><span class="sxs-lookup"><span data-stu-id="b9afb-132">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

![Modyfikowanie funkcjami programu Visual Studio: Wybrana jest karta obciążeń.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="b9afb-136">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="b9afb-136">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="b9afb-137">Przekierowania protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="b9afb-137">HTTPS redirection</span></span>

<span data-ttu-id="b9afb-138">Dla nowego projektu, zaznacz pole wyboru, aby **Konfigurowanie protokołu HTTPS** w **Nowa aplikacja internetowa ASP.NET Core** okna:</span><span class="sxs-lookup"><span data-stu-id="b9afb-138">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Nowe okno aplikacji sieci Web programu ASP.NET Core przy użyciu konfiguracji dla zaznaczone pole wyboru dla protokołu HTTPS.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="b9afb-140">W istniejącym projekcie za pomocą oprogramowania pośredniczącego przekierowania protokołu HTTPS, w `Startup.Configure` przez wywołanie metody [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) — metoda rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="b9afb-140">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="b9afb-141">Profil uruchamiania usług IIS</span><span class="sxs-lookup"><span data-stu-id="b9afb-141">IIS launch profile</span></span>

<span data-ttu-id="b9afb-142">Utwórz nowy profil uruchamiania do dodania obsługi usług IIS w czasie projektowania:</span><span class="sxs-lookup"><span data-stu-id="b9afb-142">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="b9afb-143">Aby uzyskać **profilu**, wybierz opcję **nowy** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b9afb-143">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="b9afb-144">Nazwa profilu "Usług IIS" w oknie podręcznym.</span><span class="sxs-lookup"><span data-stu-id="b9afb-144">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="b9afb-145">Wybierz **OK** do utworzenia profilu.</span><span class="sxs-lookup"><span data-stu-id="b9afb-145">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="b9afb-146">Dla **Uruchom** ustawienie, wybierz **IIS** z listy.</span><span class="sxs-lookup"><span data-stu-id="b9afb-146">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="b9afb-147">Zaznacz pole wyboru dla **uruchamiania przeglądarki** i podaj adres URL punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="b9afb-147">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="b9afb-148">Użyj protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b9afb-148">Use the HTTPS protocol.</span></span> <span data-ttu-id="b9afb-149">W tym przykładzie użyto `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="b9afb-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="b9afb-150">W **zmienne środowiskowe** zaznacz **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="b9afb-150">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="b9afb-151">Podaj zmienną środowiskową z kluczem `ASPNETCORE_ENVIRONMENT` oraz wartość `Development`.</span><span class="sxs-lookup"><span data-stu-id="b9afb-151">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="b9afb-152">W **ustawienia serwera sieci Web** obszar, **adres URL aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="b9afb-152">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="b9afb-153">W tym przykładzie użyto `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="b9afb-153">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="b9afb-154">Zapisz profil.</span><span class="sxs-lookup"><span data-stu-id="b9afb-154">Save the profile.</span></span>

![Okno właściwości projektu z wybraną kartą debugowania.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="b9afb-159">Możesz też ręcznie dodać profil uruchamiania [launchSettings.json](http://json.schemastore.org/launchsettings) pliku w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b9afb-159">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="b9afb-160">Uruchom projekt</span><span class="sxs-lookup"><span data-stu-id="b9afb-160">Run the project</span></span>

<span data-ttu-id="b9afb-161">W programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b9afb-161">In Visual Studio:</span></span>

* <span data-ttu-id="b9afb-162">Upewnij się, że konfiguracji kompilacji na liście rozwijanej jest równa **debugowania**.</span><span class="sxs-lookup"><span data-stu-id="b9afb-162">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="b9afb-163">Ustaw przycisk Uruchom **IIS** profilu, a następnie wybierz przycisk aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="b9afb-163">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

![Przycisk Uruchom na pasku narzędzi programu VS ustawiono profil usług IIS za pomocą listy rozwijanej konfiguracji kompilacji wartość wersja.](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="b9afb-165">Programu Visual Studio może monit o ponowne uruchomienie komputera, jeśli nie jest uruchomiona jako administrator.</span><span class="sxs-lookup"><span data-stu-id="b9afb-165">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="b9afb-166">Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b9afb-166">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="b9afb-167">Użycie certyfikatu deweloperskiego niezaufanych przeglądarka może wymagać Utwórz wyjątek dla niezaufanego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="b9afb-167">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="b9afb-168">Debugowanie wersji kompilacji konfiguracji za pomocą [tylko mój kod](/visualstudio/debugger/just-my-code) i wyniki optymalizacje kompilatora w środowisku o obniżonym poziomie.</span><span class="sxs-lookup"><span data-stu-id="b9afb-168">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="b9afb-169">Na przykład punkty przerwania nie ma odwołań.</span><span class="sxs-lookup"><span data-stu-id="b9afb-169">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b9afb-170">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b9afb-170">Additional resources</span></span>

* [<span data-ttu-id="b9afb-171">Host platformy ASP.NET Core na Windows za pomocą programu IIS</span><span class="sxs-lookup"><span data-stu-id="b9afb-171">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="b9afb-172">Wprowadzenie do modułu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9afb-172">Introduction to ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="b9afb-173">Odwołania do konfiguracji modułu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9afb-173">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="b9afb-174">Wymuszanie protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="b9afb-174">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
