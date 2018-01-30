---
title: "Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core"
author: shirhatti
description: "Wykryj obsługę debugowania aplikacji ASP.NET Core, gdy za usług IIS w systemie Windows Server."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a5f727dd21ac0c6702691df2215c42f4adc0ec27
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="6d2e4-103">Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d2e4-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="6d2e4-104">Autor: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="6d2e4-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="6d2e4-105">W tym artykule opisano [programu Visual Studio](https://www.visualstudio.com/vs/) obsługę debugowania aplikacji ASP.NET Core za usług IIS w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="6d2e4-106">Ten temat przeprowadzi Cię przez włączenie tej funkcji i konfigurowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6d2e4-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6d2e4-107">Prerequisites</span></span>

* <span data-ttu-id="6d2e4-108">Visual Studio (2017 r/w wersji 15.3 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="6d2e4-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="6d2e4-109">ASP.NET i sieć web development obciążenia *lub* obciążenia aplikacji dla wielu platform .NET Core</span><span class="sxs-lookup"><span data-stu-id="6d2e4-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="6d2e4-110">Włącz usługi IIS</span><span class="sxs-lookup"><span data-stu-id="6d2e4-110">Enable IIS</span></span>

<span data-ttu-id="6d2e4-111">Włącz usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-111">Enable IIS.</span></span> <span data-ttu-id="6d2e4-112">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="6d2e4-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="6d2e4-113">Wybierz **Internetowe usługi informacyjne** wyboru.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-113">Select the **Internet Information Services** checkbox.</span></span>

![Wyświetlanie wyboru Internetowe usługi informacyjne zaznaczone jako czarny kwadrat (nie zaznaczone) wskazujący, że niektóre funkcje usług IIS są włączone funkcje systemu Windows](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="6d2e4-115">Instalacja usług IIS wymaga ponownego uruchomienia systemu, należy ponownie uruchomić system.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-115">If the IIS installation requires a reboot, reboot the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="6d2e4-116">Włącz obsługę usług IIS w czasie opracowywania</span><span class="sxs-lookup"><span data-stu-id="6d2e4-116">Enable development-time IIS support</span></span>

<span data-ttu-id="6d2e4-117">Po zainstalowaniu usług IIS, należy uruchomić Instalator programu Visual Studio, aby zmodyfikować istniejącą instalację programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-117">Once IIS is installed, launch the Visual Studio installer to modify the existing Visual Studio installation.</span></span> <span data-ttu-id="6d2e4-118">W Instalatorze, wybierz **czasie opracowywania usługi IIS obsługują** składnika.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-118">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="6d2e4-119">Składnik jest wymieniony jako opcjonalny składnik **Podsumowanie** panelu dla **ASP.NET i sieć web development** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-119">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="6d2e4-120">Spowoduje to zainstalowanie [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), która jest moduł macierzysty usług IIS wymagane do uruchamiania aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Modyfikowanie funkcje programu Visual Studio: wybrana jest karta obciążeń.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="6d2e4-124">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="6d2e4-124">Configure the project</span></span>

<span data-ttu-id="6d2e4-125">Utwórz nowy profil Uruchom, aby dodać obsługę usług IIS w czasie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="6d2e4-126">W programie Visual Studio **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="6d2e4-127">Wybierz **debugowania** kartę. Wybierz **IIS** z **uruchamianie** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="6d2e4-128">Upewnij się, że **uruchamiania przeglądarki** z poprawny adres URL jest włączona funkcja.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Okno właściwości projektu z wybraną kartą debugowania.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="6d2e4-133">Możesz też ręcznie dodać profil uruchamiania do [launchSettings.json](http://json.schemastore.org/launchsettings) plik w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6d2e4-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="6d2e4-134">Visual Studio może monit o ponowne uruchomienie, gdy nie jest uruchomiona jako administrator.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="6d2e4-135">Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-135">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="6d2e4-136">Gratulacje!</span><span class="sxs-lookup"><span data-stu-id="6d2e4-136">Congratulations!</span></span> <span data-ttu-id="6d2e4-137">W tym momencie projekt jest skonfigurowany do obsługi usług IIS w czasie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="6d2e4-137">At this point, the project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6d2e4-138">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6d2e4-138">Additional resources</span></span>

* [<span data-ttu-id="6d2e4-139">Host platformy ASP.NET Core w systemie Windows z programem IIS</span><span class="sxs-lookup"><span data-stu-id="6d2e4-139">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="6d2e4-140">Wprowadzenie do platformy ASP.NET Core modułu</span><span class="sxs-lookup"><span data-stu-id="6d2e4-140">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="6d2e4-141">Odwołania do konfiguracji modułu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d2e4-141">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
