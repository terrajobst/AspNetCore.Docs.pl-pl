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
ms.openlocfilehash: a8bdf4c0c0399c62666e6e61e70c0298a42c2c12
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core

Autor: [Sourabh Shirhatti](https://twitter.com/sshirhatti)

W tym artykule opisano [programu Visual Studio](https://www.visualstudio.com/vs/) obsługę debugowania aplikacji ASP.NET Core za usług IIS w systemie Windows Server. Ten temat przeprowadzi Cię przez włączenie tej funkcji i konfigurowanie projektu.

## <a name="prerequisites"></a>Wymagania wstępne

* Visual Studio (2017 r/w wersji 15.3 lub nowszej)
* ASP.NET i sieć web development obciążenia *lub* obciążenia aplikacji dla wielu platform .NET Core

## <a name="enable-iis"></a>Włącz usługi IIS

Włącz usługi IIS. Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu). Wybierz **Internetowe usługi informacyjne** wyboru.

![Wyświetlanie wyboru Internetowe usługi informacyjne zaznaczone jako czarny kwadrat (nie zaznaczone) wskazujący, że niektóre funkcje usług IIS są włączone funkcje systemu Windows](development-time-iis-support/_static/enable_iis.png)

Jeśli instalacja usług IIS wymaga ponownego uruchomienia komputera, należy ponownie uruchomić system.

## <a name="enable-development-time-iis-support"></a>Włącz obsługę usług IIS w czasie opracowywania

Uruchom Instalator programu Visual Studio. Wybierz **czasie opracowywania usługi IIS obsługują** składnika. Składnik jest wymieniony jako opcjonalne w **Podsumowanie** panelu dla **ASP.NET i sieć web development** obciążenia. Spowoduje to zainstalowanie [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), która jest moduł macierzysty usług IIS wymagane do uruchomienia aplikacji platformy ASP.NET Core.

![Modyfikowanie funkcje programu Visual Studio: wybrana jest karta obciążeń. W sekcji sieci Web i w chmurze wybrano panelu programowanie ASP.NET i sieć web. Po prawej stronie w obszarze opcjonalne panel Podsumowanie jest to pole wyboru dla rozwoju razem, gdy usługi IIS obsługują.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Konfigurowanie projektu

Utwórz nowy profil Uruchom, aby dodać obsługę usług IIS w czasie opracowywania. W programie Visual Studio **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**. Wybierz **debugowania** kartę. Wybierz **IIS** z **uruchamianie** listy rozwijanej. Upewnij się, że **uruchamiania przeglądarki** z poprawny adres URL jest włączona funkcja.

![Okno właściwości projektu z wybraną kartą debugowania. Ustawienia profilu, a następnie uruchom są ustawione w usługach IIS. Przy użyciu adresu http://localhost/WebApplication2 zostanie włączona funkcja przeglądarki uruchamiania. Ten sam adres jest również udostępniany w polu adres URL aplikacji w obszarze Ustawienia serwera sieci Web z włączyć włączone jest uwierzytelnianie anonimowe.](development-time-iis-support/_static/project_properties.png)

Możesz też ręcznie dodać profil uruchamiania do [launchSettings.json](http://json.schemastore.org/launchsettings) plik w aplikacji:

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

Visual Studio może monit o ponowne uruchomienie, gdy nie jest uruchomiona jako administrator. Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Host platformy ASP.NET Core w systemie Windows z programem IIS](xref:host-and-deploy/iis/index)
* [Wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module)
* [Odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
