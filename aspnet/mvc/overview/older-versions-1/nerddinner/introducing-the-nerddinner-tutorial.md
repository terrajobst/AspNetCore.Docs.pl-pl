---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Wprowadzenie do samouczka NerdDinner | Dokumentacja firmy Microsoft
author: shanselman
description: "Najlepszym sposobem poznawania nowej struktury jest z nim coś kompilacji. W tym samouczku przedstawiono sposób tworzenia mały, ale pełny, aplikacji, za pomocą ASP.NE..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 57eedb224e26867c78cc399b89f91b95f722074d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="introducing-the-nerddinner-tutorial"></a>Wprowadzenie do samouczka NerdDinner
====================
przez [Scott Hanselman](https://github.com/shanselman)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Najlepszym sposobem poznawania nowej struktury jest z nim coś kompilacji. W tym samouczku przedstawiono sposób tworzenia małą, ale zakończyć aplikację przy użyciu platformy ASP.NET MVC 1 i wprowadza niektóre podstawowe koncepcje za nią.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.


## <a name="nerddinner-tutorial"></a>Samouczek NerdDinner

Najlepszym sposobem poznawania nowej struktury jest z nim coś kompilacji. W tym samouczku przedstawiono sposób tworzenia małą, ale zakończyć aplikację przy użyciu platformy ASP.NET MVC i wprowadza niektóre podstawowe koncepcje za nią.

Aplikacja, któremu zamierzamy utworzyć nazywa się "NerdDinner". NerdDinner zapewnia prosty sposób użytkownikom wyszukiwanie i organizowanie kolacji w trybie online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner umożliwia tworzenie, edytowanie i usuwanie kolacji dla zarejestrowanych użytkowników. Wymusza spójny zestaw reguł sprawdzania poprawności i biznesowych w aplikacji:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Osoby odwiedzające opartych na technologii AJAX tablica służy do wyszukiwania nadchodzących kolacji odbywają się obok nich:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Kliknięcie przycisku obiad powoduje wyświetlenie strony szczegółów gdzie można znaleźć więcej informacji:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Jeśli są zainteresowani uczestniczący obiad mogą zalogować się lub zarejestrować się w lokacji:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

One następnie kliknij łącze RSVP opartych na technologii AJAX do udziału zdarzenia:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementowanie NerdDinner

Zamierzamy rozpocząć naszej aplikacji NerdDinner przy użyciu pliku -&gt;polecenie Nowy projekt w programie Visual Studio do tworzenia nowego projektu platformy ASP.NET MVC. Następnie przyrostowo dodamy możliwości i funkcje. Wzdłuż sposób omówione zostaną następujące czynności:

1. [Jak utworzyć nowy projekt ASP.NET MVC](# "Utwórz nowy projekt ASP.NET MVC")
2. [Jak utworzyć bazę danych](# "tworzenie bazy danych")
3. [Sposób tworzenia modelu sprawdzaniem poprawności reguły biznesowej](# "Budowanie modelu sprawdzaniem poprawności reguły biznesowej")
4. [Jak używać kontrolery i widoki do zaimplementowania listę/szczegółów interfejsu użytkownika](# "używać kontrolery i widoki, do zaimplementowania interfejsu użytkownika listę/szczegółów")
5. [Jak zapewnić CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) danych tworzą Obsługa wpis](# "obsługuje wpis formularza danych Podaj CRUD (tworzenia, odczytu, aktualizacji, usuwania)")
6. [Jak używać ViewData i implementuje klasy ViewModel](# "Użyj ViewData i implementuje klasy ViewModel")
7. [Jak ponownie użyć interfejsu użytkownika przy użyciu stron wzorcowych i częściowe](# "ponownego użycia interfejsu użytkownika przy użyciu stron wzorcowych i częściowe")
8. [Implementowania stronicowania danych wydajne](# "zaimplementować wydajne danych stronicowania")
9. [Jak zabezpieczyć aplikacji przy użyciu uwierzytelniania i autoryzacji](# "bezpiecznego aplikacji przy użyciu uwierzytelniania i autoryzacji")
10. [Sposób użycia interfejsu AJAX do dostarczenia aktualizacji dynamicznej](# "używać technologii AJAX, aby dostarczać aktualizacje dynamiczne")
11. [Jak używać technologii AJAX do implementacji mapowania scenariusze](# "Użyj AJAX do implementacji mapowania scenariuszy")
12. [Jak włączyć automatyczne testy jednostkowe](# "włączenia zautomatyzowanych testów jednostkowych")

Możesz utworzyć własną kopię NerdDinner od początku, wykonując każdego kroku będziemy wskazówki w tym rozdziale. Alternatywnie możesz pobrać ukończoną wersję kodu źródłowego w tym miejscu: [NerdDinner w serwisie GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Możesz również opcjonalnie również [Pobierz bezpłatną wersję PDF tego samouczka](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) Aby odczytać samouczek w trybie offline.

Do skompilowania aplikacji, można użyć programu Visual Studio 2008 lub wolnego Visual Web Developer 2008 Express. SQL Server lub wolnego programu SQL Server Express można użyć dla bazy danych.

Można zainstalować programu ASP.NET MVC, Visual Web Developer 2008 Express i programu SQL Server Express (wszystkim jest bezpłatny) przy użyciu V2 z [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Teraz do dzieła...

Teraz, kiedy możemy zostały objęte jest NerdDinner, umożliwia rzutowanie naszych rękawami i pisania kodu.

Rozpocznie się za pomocą pliku -&gt;nowy projekt w programie Visual Studio, aby utworzyć aplikację NerdDinner.

>[!div class="step-by-step"]
[Dalej](create-a-new-aspnet-mvc-project.md)
