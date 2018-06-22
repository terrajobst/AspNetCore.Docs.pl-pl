---
title: Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio
author: rick-anderson
description: Dowiedz się, jak rozpocząć pracę z platformy ASP.NET Core MVC i Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 1fb3947023843341403f4355c6ae1e61d7e4f6b1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275555"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a>Wprowadzenie do platformy ASP.NET Core MVC i Visual Studio

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Istnieją 3 wersje tego samouczka:

* System macOS: [tworzenie aplikacji platformy ASP.NET Core MVC za pomocą programu Visual Studio dla komputerów Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* System Windows: [utworzyć platformy ASP.NET Core aplikacji MVC za pomocą programu Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* System macOS, Linux i Windows: [tworzenie aplikacji platformy ASP.NET Core MVC z kodem Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>Zainstaluj oprogramowanie .NET Core i Visual Studio

::: moniker range=">= aspnetcore-2.1"

[! OBEJMUJĄ [] (~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci web

W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

Zakończenie **nowy projekt** okna dialogowego:

* W okienku po lewej stronie wybierz **.NET Core**
* W środkowym okienku wybierz **aplikacji sieci Web platformy ASP.NET Core (.NET Core)**
* Nazwa projektu "MvcMovie" (należy nazwa projektu "MvcMovie", po skopiowaniu kodu przestrzeni nazw będą zgodne.)
* Wybierz **OK**

![Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Zakończenie **nowej platformy ASP.NET Core aplikacji sieci Web (.NET Core) — MvcMovie** okna dialogowego:

* W polu listy rozwijanej selektora wersja wybierz **platformy ASP.NET Core 2.1**
* Wybierz **Application(Model-View-Controller) w sieci Web**
* Wybierz **OK**.

![Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core ](start-mvc/_static/new_project22-21.png)

Visual Studio używany domyślny szablon projektu MVC, który został utworzony. Masz aplikację pracy z teraz wpisując nazwę projektu i wybierając kilka opcji. Jest to projekt starter podstawowe i jest dobrym miejscem do rozpoczęcia,

Wybierz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchomionej aplikacji](start-mvc/_static/1.png)

* Uruchamia program Visual Studio [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację. Należy zauważyć, że na pasku adresu `localhost:port#` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Powyższy obraz numer portu to 5000. Adres URL w przeglądarce przedstawiono `localhost:5000`. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.
* Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.
* Możesz uruchomić aplikację w trybie bez debugowania z lub debugowania **debugowania** element menu:

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* Można debugować aplikacji, naciskając pozycję **usług IIS Express** przycisku

![IIS Express](start-mvc/_static/iis_express.png)

Domyślny szablon umożliwia pracy **macierzystego o** i **skontaktuj się z** łącza. Powyższy obraz przeglądarki nie wyświetla tych łączy. W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.

![Ikona nawigacji w prawym górnym rogu](start-mvc/_static/2.png)

Podczas uruchamiania w trybie debugowania, naciśnij przycisk **Shift-F5** można zatrzymać debugowania.

W następnej części tego samouczka możemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[! OBEJMUJĄ [] (~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Install Visual Studio Community 2017. Wybierz pobieranie społeczności. Jeśli masz program Visual Studio 2017 r zainstalowany, Pomiń ten krok.

* [Visual Studio 2017 głównej strony Instalatora](https://www.visualstudio.com/)

Uruchom Instalatora, a następnie wybierz następujące obciążenia:

* **ASP.NET i sieć web development** (w obszarze **sieci Web & chmurze**)
* **Programowanie wieloplatformowych .NET core** (w obszarze **inne procesami**)

![**ASP.NET i sieci web development ** (w obszarze ** & sieci Web chmury **)](start-mvc/_static/web_workload.png)

![* *.NET core cross-cross-platfrom Programowanie ** (w obszarze ** inne procesami **)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci web

W programie Visual Studio, wybierz **Plik > Nowy > Projekt**.

![Plik > Nowy > Projekt](start-mvc/_static/alt_new_project.png)

Zakończenie **nowy projekt** okna dialogowego:

* W okienku po lewej stronie wybierz **.NET Core**
* W środkowym okienku wybierz **aplikacji sieci Web platformy ASP.NET Core (.NET Core)**
* Nazwa projektu "MvcMovie" (należy nazwa projektu "MvcMovie", po skopiowaniu kodu przestrzeni nazw będą zgodne.)
* Wybierz **OK**

![Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Zakończenie **nowej platformy ASP.NET Core aplikacji sieci Web (.NET Core) — MvcMovie** okna dialogowego:

* W polu listy rozwijanej selektora wersja wybierz **platformy ASP.NET Core 2.-**
* Wybierz **Application(Model-View-Controller) w sieci Web**
* Wybierz **OK**.

![Okno dialogowe nowego projektu, .net core w okienku po lewej stronie sieci web platformy ASP.NET Core ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Zakończenie **nowej platformy ASP.NET Core aplikacji sieci Web (.NET Core) — MvcMovie** okna dialogowego:

* W naciśnij pola rozwijanego selektora wersji **ASP.NET Core 1.1**
* Wybierz **aplikacji sieci Web**
* Zachowaj ustawienie domyślne **bez uwierzytelniania**
* Wybierz **OK**.

![Nowa aplikacja sieci web platformy ASP.NET Core](start-mvc/_static/p3.png)

---

Visual Studio używany domyślny szablon projektu MVC, który został utworzony. Masz aplikację pracy z teraz wpisując nazwę projektu i wybierając kilka opcji. Jest to projekt starter podstawowe i jest dobrym miejscem do rozpoczęcia,

Wybierz **F5** do uruchomienia aplikacji w trybie debugowania lub **Ctrl-F5** w trybie bez debugowania.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![uruchomionej aplikacji](start-mvc/_static/1.png)

* Uruchamia program Visual Studio [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchamia aplikację. Należy zauważyć, że na pasku adresu `localhost:port#` i nie coś, takich jak `example.com`. Jest to spowodowane tym `localhost` jest standardowa nazwa hosta komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web. Powyższy obraz numer portu to 5000. Adres URL w przeglądarce przedstawiono `localhost:5000`. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.
* Uruchamianie aplikacji z **Ctrl + F5** (tryb-debug) pozwala wprowadzać zmiany kodu, Zapisz plik, Odśwież przeglądarkę i zobaczyć zmiany kodu. Wielu deweloperów preferowane jest w trybie bez debugowania szybko uruchomić aplikację i zobaczyć zmiany.
* Możesz uruchomić aplikację w trybie bez debugowania z lub debugowania **debugowania** element menu:

![Debugowanie menu](start-mvc/_static/debug_menu.png)

* Można debugować aplikacji, naciskając pozycję **usług IIS Express** przycisku

![IIS Express](start-mvc/_static/iis_express.png)

Domyślny szablon umożliwia pracy **macierzystego o** i **skontaktuj się z** łącza. Powyższy obraz przeglądarki nie wyświetla tych łączy. W zależności od rozmiaru przeglądarki konieczne może być kliknij ikonę nawigacji, aby je wyświetlić.

![Ikona nawigacji w prawym górnym rogu](start-mvc/_static/2.png)

Podczas uruchamiania w trybie debugowania, naciśnij przycisk **Shift-F5** można zatrzymać debugowania.

W następnej części tego samouczka możemy Dowiedz się więcej o MVC i rozpocząć pisanie kodu.

::: moniker-end
> [!div class="step-by-step"]
> [Next](adding-controller.md)  
