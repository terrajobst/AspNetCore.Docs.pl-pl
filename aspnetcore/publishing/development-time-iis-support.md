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
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core

Autor: [Sourabh Shirhatti](https://twitter.com/sshirhatti)

W tym artykule opisano [programu Visual Studio](https://www.visualstudio.com/vs/) obsługę debugowania aplikacji ASP.NET Core za usług IIS w systemie Windows Server. Ten temat przeprowadzi Cię przez włączenie tej funkcji i konfigurowanie projektu.

## <a name="prerequisites"></a>Wymagania wstępne

* Visual Studio (2017 r/w wersji 15.3 lub nowszej)
* ASP.NET i sieć web development obciążenia *lub* obciążenia aplikacji dla wielu platform .NET Core

## <a name="enable-iis"></a>Włącz usługi IIS

Włącz usługi IIS w systemie. Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu). Wybierz **Internetowe usługi informacyjne** wyboru.

![Wyświetlanie wyboru Internetowe usługi informacyjne zaznaczone jako czarny kwadrat (nie zaznaczone) wskazujący, że niektóre funkcje usług IIS są włączone funkcje systemu Windows](development-time-iis-support/_static/enable_iis.png)

Instalację usług IIS wymaga ponownego uruchomienia systemu, należy ponownie uruchomić system.

## <a name="enable-development-time-iis-support"></a>Włącz obsługę usług IIS w czasie opracowywania

Po zainstalowaniu usług IIS, należy uruchomić Instalator programu Visual Studio, aby zmodyfikować istniejącą instalację programu Visual Studio. W Instalatorze, wybierz **czasie opracowywania usługi IIS obsługują** składnika. Składnik jest wymieniony jako opcjonalny składnik **Podsumowanie** panelu dla **ASP.NET i sieć web development** obciążenia. Spowoduje to zainstalowanie [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), która jest moduł macierzysty usług IIS wymagane do uruchamiania aplikacji platformy ASP.NET Core.

![Modyfikowanie funkcje programu Visual Studio: wybrana jest karta obciążeń. W sekcji sieci Web i w chmurze wybrano panelu programowanie ASP.NET i sieć web. Po prawej stronie w obszarze opcjonalne panel Podsumowanie jest to pole wyboru dla rozwoju razem, gdy usługi IIS obsługują.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Konfigurowanie projektu

Utwórz nowy profil Uruchom, aby dodać obsługę usług IIS w czasie opracowywania. W programie Visual Studio **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**. Wybierz **debugowania** kartę. Wybierz **IIS** z **uruchamianie** listy rozwijanej. Upewnij się, że **uruchamiania przeglądarki** z poprawny adres URL jest włączona funkcja.

![Okno właściwości projektu z wybraną kartą debugowania. Ustawienia profilu, a następnie uruchom są ustawione w usługach IIS. Przy użyciu adresu http://localhost/WebApplication2 zostanie włączona funkcja przeglądarki uruchamiania. Ten sam adres jest również udostępniany w polu adres URL aplikacji w obszarze Ustawienia serwera sieci Web z włączyć włączone jest uwierzytelnianie anonimowe.](development-time-iis-support/_static/project_properties.png)

Alternatywnie można ręcznie dodać profil uruchamiania do Twojej [launchSettings.json](http://json.schemastore.org/launchsettings) plik w aplikacji:

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

Może pojawić się prośba o ponowne uruchomienie programu Visual Studio, jeśli nie zostały uruchomione z uprawnieniami administratora. Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.

Gratulacje! W tym momencie projektu jest skonfigurowany do obsługi usług IIS w czasie opracowywania. 

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Host platformy ASP.NET Core w systemie Windows z programem IIS](xref:publishing/iis)
* [Wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module)
* [Konfiguracja modułu Core programu ASP.NET](xref:hosting/aspnet-core-module)
