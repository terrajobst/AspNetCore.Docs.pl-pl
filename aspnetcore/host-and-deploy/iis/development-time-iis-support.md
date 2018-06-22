---
title: Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core
author: shirhatti
description: Wykryj obsługę debugowania aplikacji ASP.NET Core, gdy za usług IIS w systemie Windows Server.
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: eb8b4369d6d5434adbac187f59b18d7a2b80055c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277657"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Usługi IIS w czasie opracowywania obsługi w programie Visual Studio dla platformy ASP.NET Core

Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti) i [Luke Latham](https://github.com/guardrex)

W tym artykule opisano [programu Visual Studio](https://www.visualstudio.com/vs/) obsługę debugowania aplikacji ASP.NET Core za usług IIS w systemie Windows Server. Ten temat przeprowadzi Cię przez włączenie tej funkcji i konfigurowanie projektu.

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio dla systemu Windows](https://www.microsoft.com/net/download/windows)
* **ASP.NET i sieć web development** obciążenia
* **Programowanie wieloplatformowych .NET core** obciążenia
* Certyfikat X.509 zabezpieczeń

## <a name="enable-iis"></a>Włącz usługi IIS

1. Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **wyłączyć funkcje systemu Windows na lub wyłącz** (po lewej stronie ekranu).
1. Wybierz **Internetowe usługi informacyjne** pole wyboru.

![Pole wyboru Internetowe usługi informacyjne funkcje systemu Windows prezentujących zaznaczone jako czarny kwadrat (nie zaznaczone), wskazujący, że niektóre funkcje usług IIS są włączone](development-time-iis-support/_static/enable_iis.png)

Instalacja usług IIS może wymagać ponownego uruchomienia systemu.

## <a name="configure-iis"></a>Konfigurowanie usług IIS

Usługi IIS musi mieć skonfigurowaną następujące witryny sieci Web:

* Nazwa hosta nazwę hosta, która odpowiada adresowi URL profilu uruchamiania aplikacji.
* Powiązanie dla portu 443 z certyfikatem przypisane.

Na przykład **nazwy hosta** dla witryny sieci Web dodano jest ustawiony na wartość "localhost" (profilu uruchamiania będą też używać w dalszej części tego tematu "localhost"). Port ten jest ustawiony na "443" (HTTPS). **Usług IIS Express certyfikatu deweloperskiego** jest przypisany do witryny sieci Web, ale dowolnego ważnego certyfikatu działania:

![Dodaj okno witryny sieci Web w usługach IIS przedstawiający powiązania zestawu hosta lokalnego na porcie 443 z certyfikatem, który został przypisany.](development-time-iis-support/_static/add-website-window.png)

Jeśli ma już instalacji usług IIS **domyślna witryna sieci Web** przy użyciu nazwy hosta, który dopasowuje nazwy hosta URL profilu uruchamiania aplikacji:

* Dodaj powiązanie portów dla portu 443 (HTTPS).
* Przypisz prawidłowy certyfikat do witryny sieci Web.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Włącz obsługę usług IIS czasie opracowywania w programie Visual Studio

1. Uruchom Instalator programu Visual Studio.
1. Wybierz **czasie opracowywania usługi IIS obsługują** składnika. Składnik jest wymieniony jako opcjonalne w **Podsumowanie** panelu dla **ASP.NET i sieć web development** obciążenia. Instaluje składnik [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), która jest moduł macierzysty usług IIS wymagane do uruchomienia aplikacji za IIS platformy ASP.NET Core w konfiguracji zwrotnego serwera proxy.

![Modyfikowanie funkcje programu Visual Studio: wybrana jest karta obciążeń. W sekcji sieci Web i w chmurze wybrano panelu programowanie ASP.NET i sieć web. Po prawej stronie w obszarze opcjonalne panel Podsumowanie ma postać pola wyboru dla rozwoju razem, gdy usługi IIS obsługują.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Konfigurowanie projektu

### <a name="https-redirection"></a>Przekierowania protokołu HTTPS

Dla nowego projektu, zaznacz pole wyboru, aby **Konfiguruj na potrzeby protokołu HTTPS** w **nową aplikację sieci Web Core ASP.NET** okno:

![Nowe okno aplikacji sieci Web platformy ASP.NET Core z konfiguracji dla protokołu HTTPS jest zaznaczone pole wyboru.](development-time-iis-support/_static/new-app.png)

W istniejącego projektu, za pomocą oprogramowania pośredniczącego przekierowania protokołu HTTPS w `Startup.Configure` przez wywołanie metody [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) — metoda rozszerzenia:

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

### <a name="iis-launch-profile"></a>Profil uruchamiania usług IIS

Utwórz nowy profil Uruchom, aby dodać obsługę usług IIS w czasie opracowywania:

1. Dla **profilu**, wybierz pozycję **nowy** przycisku. Nazwa profilu "Usług IIS" w oknie podręcznym. Wybierz **OK** do tworzenia profilu.
1. Dla **uruchamianie** wybierz pozycję **IIS** z listy.
1. Zaznacz pole wyboru dla **uruchamiania przeglądarki** i podaj adres URL punktu końcowego. Użyj protokołu HTTPS. W tym przykładzie użyto `https://localhost/WebApplication1`.
1. W **zmiennych środowiskowych** zaznacz **Dodaj** przycisku. Podaj wartość zmiennej środowiskowej z kluczem `ASPNETCORE_ENVIRONMENT` i wartości `Development`.
1. W **ustawienia serwera sieci Web** obszar, **adres URL aplikacji**. W tym przykładzie użyto `https://localhost/WebApplication1`.
1. Zapisywanie profilu.

![Okno właściwości projektu z wybraną kartą debugowania. Ustawienia profilu, a następnie uruchom są ustawione w usługach IIS. Włączono uruchamiania funkcji przeglądarki z adresem https://localhost/WebApplication1. Dostępne są również ten sam adres w polu adres URL aplikacji w obszarze Ustawienia serwera sieci Web.](development-time-iis-support/_static/project_properties.png)

Możesz też ręcznie dodać profil uruchamiania do [launchSettings.json](http://json.schemastore.org/launchsettings) plik w aplikacji:

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

## <a name="run-the-project"></a>Uruchom projekt

W interfejsie użytkownika programu VS ustawioną przycisk Uruchom **IIS** profilu, a następnie wybierz przycisk, aby uruchomić aplikację:

![Przycisk Uruchom na pasku narzędzi programu VS ustawiona do profilowania "Usług IIS".](development-time-iis-support/_static/toolbar.png)

Visual Studio może monit o ponowne uruchomienie, gdy nie jest uruchomiona jako administrator. Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.

Użycie certyfikatu deweloperskiego niezaufanych przeglądarki mogą wymagać utworzenia wyjątku niezaufanego certyfikatu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Host platformy ASP.NET Core w systemie Windows z programem IIS](xref:host-and-deploy/iis/index)
* [Wprowadzenie do platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module)
* [Odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl)
