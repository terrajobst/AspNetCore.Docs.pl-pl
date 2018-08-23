---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Wprowadzenie do platformy ASP.NET MVC | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 3dda4701c9be90bea7c95a330845d601345ac46b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757169"
---
<a name="intro-to-aspnet-mvc"></a>Wprowadzenie do platformy ASP.NET MVC
====================
przez [Scotta Hanselmana](https://github.com/shanselman)

> > [!NOTE]
> > Zaktualizowanej wersji, jeśli w tym samouczku jest dostępny [tutaj](../../getting-started/introduction/getting-started.md) przy użyciu [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nowe samouczku ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z tego samouczka.
> 
> 
> Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.


Upewnijmy się, w naszej pierwszej aplikacji sieci Web programu ASP.NET MVC przy użyciu [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Wybierzemy nieco aplikacji listy filmów, która będzie Daj nam tworzenie i wyświetlanie listy filmów.

## <a name="what-youll-build"></a>Jakie będziesz tworzyć

Poniżej przedstawiono dwa zrzuty ekranu aplikacji, którą utworzysz. Będziesz mieć prostą tabelę filmów z różnych kolumn.

[![Lista filmu — Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

I należy utworzyć formularz, abyśmy mogli dodać do listy filmów.

[![Utwórz film — Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Umiejętności, których dowiesz się

Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Visual Studio. Dowiesz się:

- Tworzenie nowego projektu MVC ASP.NET
- Jak utworzyć nową bazę danych za pomocą programu SQL Server
- Sposób tworzenia widoków i kontrolerów MVC ASP.NET
- Sposób pobierania i wyświetlania danych
- Jak edytować dane i włączyć sprawdzanie poprawności danych
- Jak zaktualizować schemat bazy danych

## <a name="get-started"></a>Rozpocznij

Rozpocznij, korzystając z polecenia programu Visual Web Developer 2010 Express (będzie nazywam "VWD" od teraz) i wybierz pozycję Nowy projekt ekranu startowego.

Visual Web Developer jest środowiskiem IDE lub zintegrowanego środowiska deweloperskiego. Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz środowisko IDE do tworzenia aplikacji. Brak narzędzi wzdłuż górnej przedstawiający różne opcje dostępne do można, a także menu mógł możesz również użyć do wybierz plik | Nowy projekt.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Można tworzyć aplikacje przy użyciu języka Visual Basic lub Visual C#. Teraz, wybierz pozycję Visual C# po lewej stronie a następnie wybierz "Aplikacja sieci Web ASP.NET MVC 2". Nazwij swój projekt "Filmy", a następnie kliknij przycisk OK.

[![Nowy projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Po prawej stronie jest Eksploratora rozwiązań przedstawiający wszystkie pliki i foldery w aplikacji. Duże okno w środku jest umożliwiającym edytowanie kodu i spędzają większość czasu. Program Visual Studio ulegał szablonu domyślnego dla projektu platformy ASP.NET MVC, który został utworzony, więc teraz utworzono działającą aplikację bez żadnego działania! Jest to prosty tekst "Hello World! Projekt i jest dobrym miejscem do uruchomienia dla naszej aplikacji.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Wybierz przycisk "odtwarzania" na pasku narzędzi.

![Rozpocznij debugowanie](getting-started-with-mvc-part1/_static/image11.png)

Jest zielona strzałka w prawo, który będzie kompilujesz program i uruchomić aplikację w przeglądarce sieci web.

*Uwaga: Możesz zamiast tego na klawiaturze naciśnij klawisz F5 lub wybierz debugowania -&gt;Rozpocznij debugowanie z menu "Debugowanie".*

Spowoduje to Visual Web Developer uruchomić serwer sieci web development i uruchomić aplikację sieci web (nie ma żadnych konfiguracji lub wymagane umożliwiające wykonanie tej czynności ręczne). Zostanie następnie uruchomi przeglądarkę i skonfiguruj ją, aby przejść do strony głównej aplikacji. Zwróć uwagę, poniżej, na pasku adresu przeglądarki jest wyświetlany komunikat "localhost", a nie podobny do example.com. Wynika to z localhost zawsze wskazuje na swoim komputerze lokalnym — działającego w takim przypadku aplikacji, którą właśnie utworzyliśmy.

[![Strona główna](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Gotowych ten szablon domyślny zawiera dwie strony do odwiedzenia i strony logowania podstawowe. Możemy zmienić sposób działania tej aplikacji i Poznaj nieco platformy ASP.NET MVC w procesie. Zamknij przeglądarkę i umożliwia zmiany kodu.

> [!div class="step-by-step"]
> [Next](getting-started-with-mvc-part2.md)
