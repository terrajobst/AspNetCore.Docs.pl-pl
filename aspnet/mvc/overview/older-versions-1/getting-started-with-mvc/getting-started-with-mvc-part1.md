---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Wprowadzenie do platformy ASP.NET MVC | Dokumentacja firmy Microsoft
author: shanselman
description: "Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: b884c3c535929038f047e2c4ceedf9e7ca543292
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="intro-to-aspnet-mvc"></a>Wprowadzenie do platformy ASP.NET MVC
====================
przez [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Zaktualizowanej wersji, jeśli jest dostępna w tym samouczku [tutaj](../../getting-started/introduction/getting-started.md) przy użyciu [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nowe samouczku platformy ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z tego samouczka.
> 
> 
> Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.


Upewnijmy naszych pierwszej aplikacji sieci Web platformy ASP.NET MVC przy użyciu [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Wybierzemy nieco filmu listy aplikacji, która zostanie utworzone i Daj nam listy filmów.

## <a name="what-youll-build"></a>Jakie będzie kompilacji

Poniżej przedstawiono dwa zrzuty ekranu aplikacji, która będzie kompilacji. Będziesz mieć prostą tabelę filmów z różnych kolumn.

[![Listy filmów - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

I należy utworzyć formularza, filmy można dodać do listy.

[![Tworzenie filmu — Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Dowiesz się umiejętności

Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Visual Studio. Dowiesz się:

- Jak utworzyć nowy projekt ASP.NET MVC
- Jak utworzyć nową bazę danych programu SQL Server
- Tworzenie widoków i kontrolerów MVC ASP.NET
- Jak pobrać i wyświetlanie danych
- Jak edytować dane i włączyć sprawdzanie poprawności danych
- Jak zaktualizować schemat bazy danych

## <a name="get-started"></a>Rozpocznij

Uruchom za pomocą programu Visual Web Developer 2010 Express (I będzie wywołać ją "VWD" od teraz) i wybierz nowy projekt z ekranu startowego.

Visual Web Developer jest IDE lub zintegrowane środowisko deweloperskie. Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz IDE do tworzenia aplikacji. Brak narzędzi wzdłuż górnej pokazywanie różnych dostępnych opcji użytkownik, a także menu mógł można również użyć do pliku wybierz | Nowy projekt.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Można tworzyć aplikacje przy użyciu języka Visual Basic lub Visual C#. Teraz, wybierz pozycję Visual C# po lewej stronie a następnie wybierz "Aplikacja sieci Web platformy ASP.NET MVC 2". Nazwa projektu "Filmy", a następnie kliknij przycisk OK.

[![Nowy projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Po prawej stronie jest Eksploratora rozwiązań przedstawiający wszystkie pliki i foldery w aplikacji. Big okna w środku jest, gdzie edytować kod i spędzają większość czasu. Visual Studio użyty domyślny szablon projektu programu ASP.NET MVC, który właśnie utworzony, więc teraz mieć działającą aplikację bez żadnego działania! Jest to prosty "Witaj świecie! Projekt który jest dobrym miejscem do uruchomienia dla naszej aplikacji.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Kliknij przycisk "play" na pasku narzędzi.

![Rozpocznij debugowanie](getting-started-with-mvc-part1/_static/image11.png)

Jest zielona strzałka wskazująca w prawo, program skompilować i uruchomić aplikację w przeglądarce sieci web.

*Uwaga: Możesz zamiast tego na klawiaturze naciśnij klawisz F5, lub wybrać debugowania -&gt;Rozpocznij debugowanie z menu "Debugowanie".*

Spowoduje to Visual Web Developer uruchomić serwer sieci web development i uruchomić aplikację sieci web (nie ma żadnych konfiguracji lub ręczne wykonanie czynności wymagane do włączenia to). Następnie zostanie uruchomi przeglądarkę i skonfiguruj go, aby przejść do strony głównej aplikacji. Poniżej zauważyć, że na pasku adresu przeglądarki mówi "localhost", a nie przypominać example.com. Wynika to z localhost zawsze wskazuje na własnym komputerze lokalnym — działającego w takim przypadku aplikacji, którą właśnie utworzony.

[![Strona główna](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Poza pole ten szablon domyślny zawiera dwie strony do odwiedzenia i strony logowania podstawowe. Teraz zmienić sposób działania tej aplikacji i uzyskiwanie nieco ASP.NET MVC w procesie. Zamknij przeglądarkę i pozwala zmienić kodu.

>[!div class="step-by-step"]
[Dalej](getting-started-with-mvc-part2.md)
