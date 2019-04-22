---
title: Obsługa usług IIS czas opracowywania, w programie Visual Studio dla platformy ASP.NET Core
author: guardrex
description: Dowiedz się, obsługę debugowania aplikacji ASP.NET Core, podczas korzystania z użyciem usług IIS w systemie Windows Server.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 6f555858239b4432d252f8b3ac7add5c3e8bfe62
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59425104"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Obsługa usług IIS czas opracowywania, w programie Visual Studio dla platformy ASP.NET Core

Przez [Sourabh Shirhatti](https://twitter.com/sshirhatti) i [Luke Latham](https://github.com/guardrex)

W tym artykule opisano [programu Visual Studio](https://www.visualstudio.com/vs/) obsługę debugowania aplikacji ASP.NET Core uruchomiony z usługami IIS w systemie Windows Server. W tym temacie przedstawiono w tym scenariuszu włączania i konfigurowania projektu.

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio for Windows](https://visualstudio.microsoft.com/downloads/)
* **ASP.NET i tworzenie aplikacji internetowych** obciążenia
* **Programowanie dla wielu platform .NET core** obciążenia
* Certyfikat zabezpieczeń X.509 (w przypadku obsługi protokołu HTTPS)

## <a name="enable-iis"></a>Włącz usługi IIS

1. W Windows, przejdź do **Panelu sterowania** > **programy** > **programy i funkcje** > **Włącz Windows Włącz lub wyłącz funkcje** (po lewej stronie ekranu).
1. Wybierz **Internetowe usługi informacyjne** pole wyboru. Kliknij przycisk **OK**.

Instalacja usług IIS może wymagać ponownego uruchomienia systemu.

## <a name="configure-iis"></a>Konfigurowanie usług IIS

Usługi IIS musi mieć witrynę sieci Web skonfigurowano następujące czynności:

* **Nazwa hosta** &ndash; zazwyczaj **domyślna witryna sieci Web** jest używana z **nazwy hosta** z `localhost`. Jednak wszystkie prawidłowe witryna internetowa usług IIS z nazwą hosta unikatowy działa.
* **Powiązania witryny**
  * W przypadku aplikacji, które wymagają protokołu HTTPS należy utworzyć powiązania z portem 443 przy użyciu certyfikatu. Zazwyczaj **usług IIS Express tworzenia certyfikatu** jest używane, ale dowolne, prawidłowe certyfikatu działa.
  * Dla aplikacji, które używają protokołu HTTP, upewnij się, istnienie powiązania do opublikowania 80 lub utworzyć powiązania z portem 80 dla nowej witryny.
  * Na użytek pojedynczego powiązania protokołu HTTP lub HTTPS. **Powiązanie z portów HTTP i HTTPS, jednocześnie nie jest obsługiwane.**

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Włącz obsługę usług IIS w czasie projektowania w programie Visual Studio

1. Uruchom Instalatora programu Visual Studio.
1. Wybierz **Modyfikuj** dla instalacji programu Visual Studio, którego chcesz użyć dla usług IIS oferuje obsługę w czasie projektowania.
1. Dla **ASP.NET i tworzenie aplikacji internetowych** obciążenia znajdowania i instalowania **czas opracowywania usługi IIS obsługują** składnika.

   Składnik znajduje się w **opcjonalnie** sekcji **czas opracowywania usługi IIS obsługują** w **szczegółowe informacje dotyczące instalacji** panelu po prawej stronie obciążeń. Instaluje składnik [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module), czyli moduł macierzysty usług IIS wymagane do uruchamiania aplikacji ASP.NET Core za pomocą programu IIS.

## <a name="configure-the-project"></a>Konfigurowanie projektu

### <a name="https-redirection"></a>Przekierowania protokołu HTTPS

Dla nowego projektu, który wymaga protokołu HTTPS, zaznacz pole wyboru, aby **Konfigurowanie protokołu HTTPS** w **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okna. Zaznaczenie pola wyboru dodaje [przekierowania protokołu HTTPS i oprogramowanie pośredniczące HSTS](xref:security/enforcing-ssl) do aplikacji po jej utworzeniu.

Istniejący projekt, który wymaga protokołu HTTPS, można użyć w przekierowania protokołu HTTPS i oprogramowanie pośredniczące HSTS, w `Startup.Configure`. Aby uzyskać więcej informacji, zobacz <xref:security/enforcing-ssl>.

W projekcie, który korzysta z protokołu HTTP [przekierowania protokołu HTTPS i oprogramowanie pośredniczące HSTS](xref:security/enforcing-ssl) nie są dodawane do aplikacji. Aplikacja jest wymagana żadna konfiguracja.

### <a name="iis-launch-profile"></a>Profil uruchamiania usług IIS

Utwórz nowy profil uruchamiania do dodania obsługi usług IIS w czasie projektowania:

::: moniker range=">= aspnetcore-3.0"

1. Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań**. Wybierz **właściwości**. Otwórz **debugowania** kartę.
1. Aby uzyskać **profilu**, wybierz opcję **nowy** przycisku. Nazwa profilu "Usług IIS" w oknie podręcznym. Wybierz **OK** do utworzenia profilu.
1. Dla **Uruchom** ustawienie, wybierz **IIS** z listy.
1. Zaznacz pole wyboru dla **uruchamiania przeglądarki** i podaj adres URL punktu końcowego.

   Jeśli aplikacja wymaga protokołu HTTPS, użyj punktu końcowego HTTPS (`https://`). Dla protokołu HTTP, użyj protokołu HTTP (`http://`) punktu końcowego.

   Podaj takiej samej nazwy hosta i portu jako [określona konfiguracja usług IIS wcześniejszych zastosowań](#configure-iis), zazwyczaj `localhost`.

   Podaj nazwę aplikacji na końcu adresu URL.

   Na przykład `https://localhost/WebApplication1` (HTTPS) lub `http://localhost/WebApplication1` (HTTP) są adresami URL prawidłowego punktu końcowego.
1. W **zmienne środowiskowe** zaznacz **Dodaj** przycisku. Podaj zmienną środowiskową **nazwa** z `ASPNETCORE_ENVIRONMENT` i **wartość** z `Development`.
1. W **ustawienia serwera sieci Web** obszar, **adres URL aplikacji** taką samą wartość, umożliwiający **uruchamiania przeglądarki** adres URL punktu końcowego.
1. Dla **modelu hostingu** ustawienia w programie Visual Studio 2019 lub nowszym, wybierz **domyślne** do korzystania z modelu hostingu używane przez projekt. Jeśli projekt ustawia `<AspNetCoreHostingModel>` właściwość w pliku projektu, wartość właściwości (`InProcess` lub `OutOfProcess`) jest używany. Jeśli właściwość nie istnieje, używana jest domyślna, hostingu modelu aplikacji, która jest w trakcie. Jeśli aplikacja wymaga jawnego modelu hostingu ustawienie różni się od normalnych modelu hostingu aplikacji, ustawić **modelu hostingu** do jednej `In Process` lub `Out Of Process` zgodnie z potrzebami.
1. Zapisz profil.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań**. Wybierz **właściwości**. Otwórz **debugowania** kartę.
1. Aby uzyskać **profilu**, wybierz opcję **nowy** przycisku. Nazwa profilu "Usług IIS" w oknie podręcznym. Wybierz **OK** do utworzenia profilu.
1. Dla **Uruchom** ustawienie, wybierz **IIS** z listy.
1. Zaznacz pole wyboru dla **uruchamiania przeglądarki** i podaj adres URL punktu końcowego.

   Jeśli aplikacja wymaga protokołu HTTPS, użyj punktu końcowego HTTPS (`https://`). Dla protokołu HTTP, użyj protokołu HTTP (`http://`) punktu końcowego.

   Podaj takiej samej nazwy hosta i portu jako [określona konfiguracja usług IIS wcześniejszych zastosowań](#configure-iis), zazwyczaj `localhost`.

   Podaj nazwę aplikacji na końcu adresu URL.

   Na przykład `https://localhost/WebApplication1` (HTTPS) lub `http://localhost/WebApplication1` (HTTP) są adresami URL prawidłowego punktu końcowego.
1. W **zmienne środowiskowe** zaznacz **Dodaj** przycisku. Podaj zmienną środowiskową **nazwa** z `ASPNETCORE_ENVIRONMENT` i **wartość** z `Development`.
1. W **ustawienia serwera sieci Web** obszar, **adres URL aplikacji** taką samą wartość, umożliwiający **uruchamiania przeglądarki** adres URL punktu końcowego.
1. Dla **modelu hostingu** ustawienia w programie Visual Studio 2019 lub nowszym, wybierz **domyślne** do korzystania z modelu hostingu używane przez projekt. Jeśli projekt ustawia `<AspNetCoreHostingModel>` właściwość w pliku projektu, wartość właściwości (`InProcess` lub `OutOfProcess`) jest używany. Jeśli właściwość nie jest obecny, używana jest domyślna, hostingu modelu aplikacji, która jest spoza procesu. Jeśli aplikacja wymaga jawnego modelu hostingu ustawienie różni się od normalnych modelu hostingu aplikacji, ustawić **modelu hostingu** do jednej `In Process` lub `Out Of Process` zgodnie z potrzebami.
1. Zapisz profil.

::: moniker-end

Kiedy nie przy użyciu programu Visual Studio, ręcznie dodać profil uruchamiania, aby [launchSettings.json](http://json.schemastore.org/launchsettings) w pliku *właściwości* folderu. Poniższy przykład umożliwia skonfigurowanie profilu do używania protokołu HTTPS:

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

Upewnij się, że `applicationUrl` i `launchUrl` punkty końcowe dopasowania i używać tego samego protokołu jako Konfiguracja powiązania usługi IIS: HTTP lub HTTPS.

## <a name="run-the-project"></a>Uruchom projekt

Uruchom program Visual Studio jako administrator:

* Upewnij się, że konfiguracji kompilacji na liście rozwijanej jest równa **debugowania**.
* Ustaw przycisk Uruchom **IIS** profilu, a następnie wybierz przycisk aby uruchomić aplikację.

Programu Visual Studio może monit o ponowne uruchomienie komputera, jeśli nie jest uruchomiona jako administrator. Jeśli zostanie wyświetlony monit, uruchom ponownie program Visual Studio.

Użycie certyfikatu deweloperskiego niezaufanych przeglądarka może wymagać Utwórz wyjątek dla niezaufanego certyfikatu.

> [!NOTE]
> Debugowanie wersji kompilacji konfiguracji za pomocą [tylko mój kod](/visualstudio/debugger/just-my-code) i wyniki optymalizacje kompilatora w środowisku o obniżonym poziomie. Na przykład punkty przerwania nie ma odwołań.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Rozpoczęcie korzystania z Menedżera usług IIS w usługach IIS](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* [Host platformy ASP.NET Core na Windows za pomocą programu IIS](xref:host-and-deploy/iis/index)
* [Wprowadzenie do modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl)
