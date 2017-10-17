---
title: "Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core"
author: shirhatti
description: "Wykryj obsługę debugowania aplikacji ASP.NET Core, gdy za usług IIS w systemie Windows Server."
keywords: "Platformy ASP.NET Core internetowych usług informacyjnych, usługi iis, moduł core server,asp.net systemu windows, debugowania"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="2328c-104">Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2328c-104">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="2328c-105">Autor: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="2328c-105">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="2328c-106">W tym artykule opisano [programu Visual Studio](https://www.visualstudio.com/vs/) obsługę debugowania aplikacji ASP.NET Core za usług IIS w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="2328c-106">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="2328c-107">Ten temat przeprowadzi Cię przez włączenie tej funkcji i konfigurowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="2328c-107">This topic walks you through enabling this feature and setting up your project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2328c-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2328c-108">Prerequisites</span></span>

* <span data-ttu-id="2328c-109">Visual Studio (2017 r/w wersji 15.3 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="2328c-109">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="2328c-110">ASP.NET i sieć web development obciążenia *lub* obciążenia aplikacji dla wielu platform .NET Core</span><span class="sxs-lookup"><span data-stu-id="2328c-110">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="2328c-111">Włącz usługi IIS</span><span class="sxs-lookup"><span data-stu-id="2328c-111">Enable IIS</span></span>

<span data-ttu-id="2328c-112">Włącz usługi IIS w systemie.</span><span class="sxs-lookup"><span data-stu-id="2328c-112">Enable IIS on your system.</span></span> <span data-ttu-id="2328c-113">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="2328c-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="2328c-114">Wybierz **Internetowe usługi informacyjne** wyboru.</span><span class="sxs-lookup"><span data-stu-id="2328c-114">Select the **Internet Information Services** checkbox.</span></span>

![Wyświetlanie wyboru Internetowe usługi informacyjne zaznaczone jako czarny kwadrat (nie zaznaczone) wskazujący, że niektóre funkcje usług IIS są włączone funkcje systemu Windows](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="2328c-116">Instalację usług IIS wymaga ponownego uruchomienia systemu, należy ponownie uruchomić system.</span><span class="sxs-lookup"><span data-stu-id="2328c-116">If your IIS installation requires a reboot, reboot your system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="2328c-117">Włącz obsługę usług IIS w czasie opracowywania</span><span class="sxs-lookup"><span data-stu-id="2328c-117">Enable development-time IIS support</span></span>

<span data-ttu-id="2328c-118">Po zainstalowaniu usług IIS, należy uruchomić Instalator programu Visual Studio, aby zmodyfikować istniejącą instalację programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2328c-118">Once you've installed IIS, launch the Visual Studio installer to modify your existing Visual Studio installation.</span></span> <span data-ttu-id="2328c-119">W Instalatorze, wybierz **czasie opracowywania usługi IIS obsługują** składnika.</span><span class="sxs-lookup"><span data-stu-id="2328c-119">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="2328c-120">Składnik jest wymieniony jako opcjonalny składnik **Podsumowanie** panelu dla **ASP.NET i sieć web development** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="2328c-120">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="2328c-121">Spowoduje to zainstalowanie [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), która jest moduł macierzysty usług IIS wymagane do uruchamiania aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2328c-121">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Modyfikowanie funkcje programu Visual Studio: wybrana jest karta obciążeń.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="2328c-125">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="2328c-125">Configure the project</span></span>

<span data-ttu-id="2328c-126">Utwórz nowy profil Uruchom, aby dodać obsługę usług IIS w czasie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="2328c-126">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="2328c-127">W programie Visual Studio **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="2328c-127">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="2328c-128">Wybierz **debugowania** kartę. Wybierz **IIS** z **uruchamianie** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="2328c-128">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="2328c-129">Upewnij się, że **uruchamiania przeglądarki** z poprawny adres URL jest włączona funkcja.</span><span class="sxs-lookup"><span data-stu-id="2328c-129">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Okno właściwości projektu z wybraną kartą debugowania.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="2328c-134">Alternatywnie można ręcznie dodać profil uruchamiania do Twojej [launchSettings.json](http://json.schemastore.org/launchsettings) plik w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2328c-134">Alternatively, you can manually add a launch profile to your [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

<span data-ttu-id="2328c-135">Może pojawić się prośba o ponowne uruchomienie programu Visual Studio, jeśli nie zostały uruchomione z uprawnieniami administratora.</span><span class="sxs-lookup"><span data-stu-id="2328c-135">You may be prompted to restart Visual Studio if you weren't running as an administrator.</span></span> <span data-ttu-id="2328c-136">Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2328c-136">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="2328c-137">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="2328c-137">Congratulations!</span></span> <span data-ttu-id="2328c-138">W tym momencie projektu jest skonfigurowany do obsługi usług IIS w czasie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="2328c-138">At this point, your project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2328c-139">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2328c-139">Additional resources</span></span>

* [<span data-ttu-id="2328c-140">Host platformy ASP.NET Core w systemie Windows z programem IIS</span><span class="sxs-lookup"><span data-stu-id="2328c-140">Host ASP.NET Core on Windows with IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="2328c-141">Wprowadzenie do platformy ASP.NET Core modułu</span><span class="sxs-lookup"><span data-stu-id="2328c-141">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="2328c-142">Konfiguracja modułu Core programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2328c-142">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
