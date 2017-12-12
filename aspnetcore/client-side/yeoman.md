---
title: "Kompilowanie projektów z narzędzia Yeoman w ASP.NET Core"
author: spboyer
description: "W tym artykule przedstawiono tworzenie aplikacji sieci web przy użyciu narzędzia Yeoman platformy ASP.NET Core generator na macOS."
keywords: "Platformy ASP.NET Core, narzędzia Yeoman, Cross Platform yo aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a>Wprowadzenie do tworzenia projektów z narzędzia Yeoman w ASP.NET Core

[Narzędzia yeoman](http://yeoman.io/) jest system szkieletów projektu do tworzenia wielu rodzajów aplikacji. Narzędzia Yeoman generator dla platformy ASP.NET Core zawiera różne szablony projektu do uruchomienia nowej sieci web, MVC i aplikacji konsoli.

## <a name="install-nodejs-npm-and-yeoman"></a>Instalowanie środowiska Node.js, npm i narzędzia Yeoman

### <a name="prerequisites"></a>Wymagania wstępne

Node.js i npm są wymagane dla narzędzia Yeoman. Pobierz z [Node.js](https://nodejs.org/). Instalator zawiera [Node.js](https://nodejs.org/) i [npm](https://www.npmjs.com/). Bower jest również wymagany do zainstalowania platformy interfejsu użytkownika, takie jak ładowania początkowego.

Aby zainstalować narzędzia Yeoman i Bower, uruchom następujące polecenie:

```console
npm install -g yo bower
```

>[!Note]
>Jeśli zostanie wyświetlony błąd `npm ERR! Please try running this command again as root/Administrator.` na macOS, uruchom następujące polecenia przy użyciu [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`

W wierszu polecenia należy zainstalować generatora platformy ASP.NET:

```console
npm install -g generator-aspnet
```

> [!NOTE]
> Jeśli zostanie wyświetlony błąd uprawnień, uruchom polecenie w obszarze `sudo` zgodnie z powyższym opisem.

`–g` Flagi instaluje globalnie, generator, dzięki czemu można używać z dowolną ścieżkę.

## <a name="create-an-aspnet-app"></a>Tworzenie aplikacji platformy ASP.NET

Uruchom generatora platformy ASP.NET opartych na narzędzia Yeoman:

```console
yo aspnet
```

Generator Wyświetla menu. Strzałka w dół do **sieci Web aplikacji podstawowe [bez członkostwa i autoryzacji]** projekt i wybierz **Enter**:

![Okno polecenia: typ aplikacji, czy chcesz utworzyć? Menu typy aplikacji](yeoman/_static/yeoman-yo-aspnet.png)

Wybierz Bootstrap jako platforma interfejsu użytkownika, a następnie wybierz **Enter**.

Użyj "**MyWebApp**" dla nazwy aplikacji, a następnie naciśnij przycisk **Enter**.

Narzędzia yeoman będzie szkieletu projektu i jego pliki pomocnicze. W formularzu polecenia podawane są również Sugerowane następne kroki.

![Okno polecenia: co to jest nazwa aplikacji ASP.NET? Wiersz polecenia](yeoman/_static/yeoman-yo-aspnet-created.png)

[ASP.NET generator](https://www.npmjs.com/package/generator-aspnet) tworzy platformy ASP.NET Core projektów, które mogą być ładowane do programu Visual Studio Code, Visual Studio lub uruchom w wierszu polecenia.

## <a name="restore-build-and-run"></a>Przywracanie, tworzenie i uruchamianie

Wykonaj polecenia sugerowane zmieniając katalogi do `MyWebApp` katalogu. Następnie uruchom `dotnet restore`.

![okno Polecenie](yeoman/_static/dotnet-restore.png)

Tworzenie i uruchamianie aplikacji w programie `dotnet build` i `dotnet run`:

![okno Polecenie](yeoman/_static/dotnet-build-run.png)

W tym momencie można przejść do adresu URL do przetestowania aplikacji platformy ASP.NET Core nowo utworzony.

## <a name="client-side-packages"></a>Pakiety po stronie klienta

Frontonu zasoby są dostarczane przez Szablony z narzędzia Yeoman za pomocą generatora [Bower](xref:client-side/bower) Menedżera pakietów po stronie klienta, dodawanie *bower.json* i *.bowerrc* pliki pod kątem przywracania pakietów po stronie klienta za pomocą rozwiązania Bower.

[BundlerMinifier](xref:client-side/bundling-and-minification) składnika dołączony jest również domyślnie łatwość łączenia (grupowania) i minimalizację CSS, JavaScript i HTML.

## <a name="building-and-running-from-visual-studio"></a>Tworzenie i uruchamianie z programu Visual Studio

Możesz załadować wygenerowanego projektu sieci web platformy ASP.NET Core bezpośrednio w programie Visual Studio, a następnie skompilować i uruchomić projekt w. Postępuj zgodnie z powyższymi instrukcjami, aby utworzyć szkielet nowej aplikacji platformy ASP.NET Core za pomocą narzędzia Yeoman. Tym razem wybierz **aplikacji sieci Web** z menu i nazwy aplikacji `MyWebApp`.

Otwórz program Visual Studio. Z menu Plik wybierz Otwórz ‣ projektu/rozwiązania.

W oknie dialogowym Otwórz projekt, przejdź do *.csproj* , zaznacz go, a następnie kliknij przycisk **Otwórz** przycisku. W Eksploratorze rozwiązań projektu powinien wyglądać jak na poniższym zrzucie ekranu.

![Pliki i foldery nowy projekt w Eksploratorze rozwiązań](yeoman/_static/yeoman-solution.png)

Narzędzia yeoman scaffolds aplikacji sieci web MVC, zarówno Pełna obsługa serwera i klienta kompilacji. Zależności po stronie serwera są wyświetlane w obszarze **zależności/NuGet** węzeł i zależności po stronie klienta w **zależności/Bower** węzła Eksploratora rozwiązań. Zależności zostaną przywrócone automatycznie po załadowaniu projektu.

![W węźle zależności w widoku drzewa Eksploratora rozwiązań Bower folder jest otwarty, wyświetlania jego zależności.](yeoman/_static/yeoman-loading-dependencies.png)

Gdy wszystkie zależności są przywracane, naciśnij klawisz **F5** uruchomić projekt. Wyświetla domyślną stronę główną w przeglądarce.

![Otwórz w programie Microsoft Edge aplikacji sieci Web](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a>Przywracanie, kompilowania i hostingu z wiersza polecenia

Możesz przygotowanie i hostowania aplikacji sieci web przy użyciu interfejsu wiersza polecenia platformy .NET Core.

W wierszu polecenia Zmień bieżący katalog na folder zawierający projekt (to znaczy, że folder zawierający *.csproj* pliku):

```console
cd src\MyWebApp
```

Przywróć zależności pakietów NuGet dla projektu:

```console
dotnet restore
```

Uruchom aplikację:

```console
dotnet run
```

Obsługujący wiele platform [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web rozpocznie się nasłuchiwanie na porcie 5000.

Otwórz przeglądarkę sieci web i przejdź do `http://localhost:5000`.

![Otwórz w programie Microsoft Edge aplikacji sieci Web](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a>Dodawanie do projektu za pomocą generatory sub

Przy użyciu narzędzia Yeoman [sub generatory](https://github.com/omnisharp/generator-aspnet), można dodać `nuget.config` lub `web.config` po utworzeniu projektu. Na przykład uruchom następujące polecenie z katalogu, w którym można utworzyć pliku:

```console
yo aspnet:nugetconfig
```

Wynik jest plik konfiguracji NuGet o nazwie `nuget.config` o następującej treści:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Serwery (Kestrel i WebListener)](xref:fundamentals/servers/index)
* [Podstawowe założenia](xref:fundamentals/index)
