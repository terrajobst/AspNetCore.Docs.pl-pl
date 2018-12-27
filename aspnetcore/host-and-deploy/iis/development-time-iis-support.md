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
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Obsługa usług IIS czas opracowywania, w programie Visual Studio dla platformy ASP.NET Core

Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti) i [Luke Latham](https://github.com/guardrex)

W tym artykule opisano [programu Visual Studio](https://www.visualstudio.com/vs/) obsługę debugowania aplikacji ASP.NET Core działające poza usługą IIS w systemie Windows Server. W tym temacie omówiono poprzez włączenie tej funkcji i skonfigurowania projektu.

## <a name="prerequisites"></a>Wymagania wstępne

* [Program Visual Studio for Windows](https://www.microsoft.com/net/download/windows)
* **ASP.NET i tworzenie aplikacji internetowych** obciążenia
* **Programowanie dla wielu platform .NET core** obciążenia
* Certyfikat zabezpieczeń X.509

## <a name="enable-iis"></a>Włącz usługi IIS

1. Przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Windows Włącz funkcje w lub wyłącz** (po lewej stronie ekranu).
1. Wybierz **Internetowe usługi informacyjne** pole wyboru.

![Pole wyboru Internetowe usługi informacyjne przedstawiający funkcji Windows oznaczone jako pierwiastek czarny (nie jest zaznaczone) wskazująca, że niektóre funkcje usług IIS są włączone](development-time-iis-support/_static/enable_iis.png)

Instalacja usług IIS może wymagać ponownego uruchomienia systemu.

## <a name="configure-iis"></a>Konfigurowanie usług IIS

Usługi IIS musi mieć witrynę sieci Web skonfigurowano następujące czynności:

* Nazwa hosta, który pasuje do nazwy hosta URL profilu uruchamiania aplikacji.
* Powiązanie dla portu 443 z certyfikatem przypisane.

Na przykład **nazwy hosta** dla witryny sieci Web dodano jest ustawiony na "localhost" (profil uruchamiania również użyje "localhost" w dalszej części tego tematu). Port ten jest ustawiony na "443" (HTTPS). **Usług IIS Express tworzenia certyfikatu** jest przypisany do witryny sieci Web, ale dowolnego ważnego certyfikatu działa:

![Dodaj okno witryny sieci Web w usługach IIS, przedstawiający zestawu powiązania dla hosta lokalnego na porcie 443 z certyfikatem, który został przypisany.](development-time-iis-support/_static/add-website-window.png)

Jeśli ma już instalacji usług IIS **domyślna witryna sieci Web** z nazwą hosta, który pasuje do nazwy hosta URL profilu uruchamiania aplikacji:

* Dodaj powiązanie portu dla portu 443 (HTTPS).
* Przypisać certyfikat do witryny sieci Web.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Włącz obsługę usług IIS w czasie projektowania w programie Visual Studio

1. Uruchom Instalatora programu Visual Studio.
1. Wybierz **czas opracowywania usługi IIS obsługują** składnika. Składnik jest wymieniony jako opcjonalny w **Podsumowanie** panelu dla **ASP.NET i tworzenie aplikacji internetowych** obciążenia. Instaluje składnik [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), czyli moduł macierzysty usług IIS wymagane do uruchamiania aplikacji ASP.NET Core za pomocą programu IIS.

![Modyfikowanie funkcjami programu Visual Studio: Wybrana jest karta obciążeń. W sekcji sieć Web i chmura panelu rozwoju platformy ASP.NET i sieci web jest zaznaczona. Po prawej stronie w obszarze opcjonalne panel Podsumowanie ma postać pola wyboru raz rozwoju, obsługę usług IIS.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Konfigurowanie projektu

### <a name="https-redirection"></a>Przekierowania protokołu HTTPS

Dla nowego projektu, zaznacz pole wyboru, aby **Konfigurowanie protokołu HTTPS** w **Nowa aplikacja internetowa ASP.NET Core** okna:

![Nowe okno aplikacji sieci Web programu ASP.NET Core przy użyciu konfiguracji dla zaznaczone pole wyboru dla protokołu HTTPS.](development-time-iis-support/_static/new-app.png)

W istniejącym projekcie za pomocą oprogramowania pośredniczącego przekierowania protokołu HTTPS, w `Startup.Configure` przez wywołanie metody [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) — metoda rozszerzenia:

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

Utwórz nowy profil uruchamiania do dodania obsługi usług IIS w czasie projektowania:

1. Aby uzyskać **profilu**, wybierz opcję **nowy** przycisku. Nazwa profilu "Usług IIS" w oknie podręcznym. Wybierz **OK** do utworzenia profilu.
1. Dla **Uruchom** ustawienie, wybierz **IIS** z listy.
1. Zaznacz pole wyboru dla **uruchamiania przeglądarki** i podaj adres URL punktu końcowego. Użyj protokołu HTTPS. W tym przykładzie użyto `https://localhost/WebApplication1`.
1. W **zmienne środowiskowe** zaznacz **Dodaj** przycisku. Podaj zmienną środowiskową z kluczem `ASPNETCORE_ENVIRONMENT` oraz wartość `Development`.
1. W **ustawienia serwera sieci Web** obszar, **adres URL aplikacji**. W tym przykładzie użyto `https://localhost/WebApplication1`.
1. Zapisz profil.

![Okno właściwości projektu z wybraną kartą debugowania. Ustawienia profilu, a następnie uruchom są ustawiane w usługach IIS. Włączono uruchamiania funkcji przeglądarki z adresem https://localhost/WebApplication1. Udostępniane są również ten sam adres w polu adres URL aplikacji w obszarze Ustawienia serwera sieci Web.](development-time-iis-support/_static/project_properties.png)

Możesz też ręcznie dodać profil uruchamiania [launchSettings.json](http://json.schemastore.org/launchsettings) pliku w aplikacji:

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

W programie Visual Studio:

* Upewnij się, że konfiguracji kompilacji na liście rozwijanej jest równa **debugowania**.
* Ustaw przycisk Uruchom **IIS** profilu, a następnie wybierz przycisk aby uruchomić aplikację.

![Przycisk Uruchom na pasku narzędzi programu VS ustawiono profil usług IIS za pomocą listy rozwijanej konfiguracji kompilacji wartość wersja.](development-time-iis-support/_static/toolbar.png)

Programu Visual Studio może monit o ponowne uruchomienie komputera, jeśli nie jest uruchomiona jako administrator. Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.

Użycie certyfikatu deweloperskiego niezaufanych przeglądarka może wymagać Utwórz wyjątek dla niezaufanego certyfikatu.

> [!NOTE]
> Debugowanie wersji kompilacji konfiguracji za pomocą [tylko mój kod](/visualstudio/debugger/just-my-code) i wyniki optymalizacje kompilatora w środowisku o obniżonym poziomie. Na przykład punkty przerwania nie ma odwołań.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Host platformy ASP.NET Core na Windows za pomocą programu IIS](xref:host-and-deploy/iis/index)
* [Wprowadzenie do modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl)
