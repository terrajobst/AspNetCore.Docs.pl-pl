---
title: Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core
author: shirhatti
description: Wykryj obsługę debugowania aplikacji ASP.NET Core, gdy za usług IIS w systemie Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="f1cfc-103">Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1cfc-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="f1cfc-104">Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="f1cfc-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="f1cfc-105">W tym artykule opisano [programu Visual Studio](https://www.visualstudio.com/vs/) obsługę debugowania aplikacji ASP.NET Core za usług IIS w systemie Windows Server.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="f1cfc-106">Ten temat przeprowadzi Cię przez włączenie tej funkcji i konfigurowanie projektu.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1cfc-107">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f1cfc-107">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="f1cfc-108">Włącz usługi IIS</span><span class="sxs-lookup"><span data-stu-id="f1cfc-108">Enable IIS</span></span>

<span data-ttu-id="f1cfc-109">Włącz usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-109">Enable IIS.</span></span> <span data-ttu-id="f1cfc-110">Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu).</span><span class="sxs-lookup"><span data-stu-id="f1cfc-110">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="f1cfc-111">Wybierz **Internetowe usługi informacyjne** wyboru.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-111">Select the **Internet Information Services** checkbox.</span></span>

![Wyświetlanie wyboru Internetowe usługi informacyjne zaznaczone jako czarny kwadrat (nie zaznaczone) wskazujący, że niektóre funkcje usług IIS są włączone funkcje systemu Windows](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="f1cfc-113">Jeśli instalacja usług IIS wymaga ponownego uruchomienia komputera, należy ponownie uruchomić system.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-113">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="f1cfc-114">Włącz obsługę usług IIS w czasie opracowywania</span><span class="sxs-lookup"><span data-stu-id="f1cfc-114">Enable development-time IIS support</span></span>

<span data-ttu-id="f1cfc-115">Uruchom Instalator programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-115">Launch the Visual Studio installer.</span></span> <span data-ttu-id="f1cfc-116">Wybierz **czasie opracowywania usługi IIS obsługują** składnika.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-116">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="f1cfc-117">Składnik jest wymieniony jako opcjonalne w **Podsumowanie** panelu dla **ASP.NET i sieć web development** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-117">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="f1cfc-118">Spowoduje to zainstalowanie [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), która jest moduł macierzysty usług IIS wymagane do uruchomienia aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-118">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Modyfikowanie funkcje programu Visual Studio: wybrana jest karta obciążeń.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="f1cfc-122">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="f1cfc-122">Configure the project</span></span>

<span data-ttu-id="f1cfc-123">Utwórz nowy profil Uruchom, aby dodać obsługę usług IIS w czasie opracowywania.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-123">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="f1cfc-124">W programie Visual Studio **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-124">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="f1cfc-125">Wybierz **debugowania** kartę. Wybierz **IIS** z **uruchamianie** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-125">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="f1cfc-126">Upewnij się, że **uruchamiania przeglądarki** z poprawny adres URL jest włączona funkcja.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-126">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![Okno właściwości projektu z wybraną kartą debugowania.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="f1cfc-131">Możesz też ręcznie dodać profil uruchamiania do [launchSettings.json](http://json.schemastore.org/launchsettings) plik w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f1cfc-131">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="f1cfc-132">Visual Studio może monit o ponowne uruchomienie, gdy nie jest uruchomiona jako administrator.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-132">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="f1cfc-133">Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f1cfc-133">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1cfc-134">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f1cfc-134">Additional resources</span></span>

* [<span data-ttu-id="f1cfc-135">Host platformy ASP.NET Core w systemie Windows z programem IIS</span><span class="sxs-lookup"><span data-stu-id="f1cfc-135">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="f1cfc-136">Wprowadzenie do platformy ASP.NET Core modułu</span><span class="sxs-lookup"><span data-stu-id="f1cfc-136">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="f1cfc-137">Odwołania do konfiguracji modułu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1cfc-137">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
