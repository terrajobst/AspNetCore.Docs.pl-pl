---
title: Wprowadzenie do ASP.NET Core MVC i programu Visual Studio
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z ASP.NET Core MVC i programu Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: fe555e4cfcaec5d4bb8ccee00b06d1bbcaae9dcd
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391209"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a>Wprowadzenie do ASP.NET Core MVC i programu Visual Studio

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Istnieją 3 wersje tego samouczka:

* System macOS: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [tworzenie aplikacji MVC platformy ASP.NET Core z programem Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* System macOS, Linux i Windows: [tworzenie aplikacji ASP.NET Core MVC za pomocą programu Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>Instalowanie programu Visual Studio i .NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci web

W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

Wykonaj **nowy projekt** okno dialogowe:

* W okienku po lewej stronie wybierz **platformy .NET Core**
* W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**
* Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)
* Naciśnij pozycję **OK**

![Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:

* W polu listy rozwijanej selektora wersji wybierz **platformy ASP.NET Core 2.1**
* Wybierz **(Model-View-Controller) aplikacji sieci Web**
* Naciśnij pozycję **OK**.

![Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core ](start-mvc/_static/new_project22-21.png)

Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony. Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji. Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia.

Naciśnij pozycję **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchamianie aplikacji](start-mvc/_static/1.png)

* Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację. Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Na powyższej ilustracji numer portu to 5000. Adres URL w pokazuje przeglądarki `localhost:5000`. Po uruchomieniu aplikacji, zobaczysz inny numer portu.
* Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.
* Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* Można debugować aplikację, naciskając pozycję **usług IIS Express** przycisku

![IIS Express](start-mvc/_static/iis_express.png)

Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza. Na powyższej ilustracji przeglądarki nie wyświetla tych łączy. W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.

![Ikona Nawigacja w prawym górnym rogu](start-mvc/_static/2.png)

Podczas uruchamiania w trybie debugowania, naciśnij **Shift-F5** Aby zatrzymać debugowanie.

W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Install Visual Studio Community 2017. Wybierz pakiet do pobrania społeczności. Pomiń ten krok, jeśli masz program Visual Studio 2017.

* [Instalator programu Visual stronę główną programu Studio 2017](https://www.visualstudio.com/)

Uruchom Instalatora i wybierz następujące obciążenia:

* **ASP.NET i tworzenie aplikacji internetowych** (w obszarze **sieć Web i chmura**)
* **Programowanie dla wielu platform .NET core** (w obszarze **inne zestawy narzędzi**)

![**ASP.NET i web development ** (w obszarze ** sieć Web i chmura **)](start-mvc/_static/web_workload.png)

![* *.NET core cross-cross-platfrom rozwoju ** (w obszarze ** inne zestawy narzędzi **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci web

W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

Wykonaj **nowy projekt** okno dialogowe:

* W okienku po lewej stronie wybierz **platformy .NET Core**
* W środkowym okienku wybierz **aplikacja sieci Web programu ASP.NET Core (.NET Core)**
* Nazwij projekt "MvcMovie" (warto nazwij projekt "MvcMovie", dzięki czemu podczas kopiowania kodu przestrzeni nazw będą zgodne.)
* Naciśnij pozycję **OK**

![Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:

* W polu listy rozwijanej selektora wersji wybierz **ASP.NET Core 2.-**
* Wybierz **Application(Model-View-Controller) w sieci Web**
* Naciśnij pozycję **OK**.

![Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Wykonaj **nowa Core aplikacja internetowa ASP.NET (.NET Core) — MvcMovie** okno dialogowe:

* We wzorcu tap pole listy rozwijanej selektora wersji **ASP.NET Core 1.1**
* Naciśnij pozycję **aplikacji sieci Web**
* Zachowaj wartość domyślną **bez uwierzytelniania**
* Naciśnij pozycję **OK**.

![Nowa aplikacja sieci web platformy ASP.NET Core](start-mvc/_static/p3.png)

---

Visual Studio używał domyślnego szablonu projektu MVC, który został utworzony. Masz działającą aplikację z chwili, wprowadzając nazwę projektu i wybraniu kilku opcji. Jest to projekt startowy podstawowe i jest dobrym miejscem do rozpoczęcia,

Naciśnij pozycję **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchamianie aplikacji](start-mvc/_static/1.png)

* Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację. Należy zauważyć, że na pasku adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Na powyższej ilustracji numer portu to 5000. Adres URL w pokazuje przeglądarki `localhost:5000`. Po uruchomieniu aplikacji, zobaczysz inny numer portu.
* Uruchamianie aplikacji za pomocą **kombinację klawiszy Ctrl + F5** (bez debugowania w trybie) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu programistów łatwiej jest w trybie bez debugowania szybko uruchomić aplikację i wyświetlić zmiany.
* Możesz uruchomić aplikację w debugowaniu lub trybie bez debugowania z **debugowania** element menu:

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* Można debugować aplikację, naciskając pozycję **usług IIS Express** przycisku

![IIS Express](start-mvc/_static/iis_express.png)

Domyślny szablon umożliwia pracę **domu o** i **skontaktuj się z pomocą** łącza. Na powyższej ilustracji przeglądarki nie wyświetla tych łączy. W zależności od rozmiaru przeglądarki może być konieczne kliknij ikonę nawigacji, aby je wyświetlić.

![Ikona Nawigacja w prawym górnym rogu](start-mvc/_static/2.png)

Podczas uruchamiania w trybie debugowania, naciśnij **Shift-F5** Aby zatrzymać debugowanie.

W następnej części tego samouczka będziemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

::: moniker-end

> [!div class="step-by-step"]
> [Next](adding-controller.md)  
