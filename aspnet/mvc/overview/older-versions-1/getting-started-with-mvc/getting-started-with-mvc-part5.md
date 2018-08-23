---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Uzyskiwanie dostępu do danych modelu za pomocą kontrolera | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 76dc324134dc93c9552741fea9f1136abdc9184a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752085"
---
<a name="accessing-your-models-data-from-a-controller"></a>Uzyskiwanie dostępu do danych modelu za pomocą kontrolera
====================
przez [Scotta Hanselmana](https://github.com/shanselman)

> Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.


W tej sekcji użyjemy Utwórz nową klasę MoviesController i pisanie kodu, który pobiera dane filmów i wyświetla go do przeglądarki przy użyciu szablonu widoku.

Kliknij prawym przyciskiem folder kontrolery i upewnij MoviesController nowe.

[![Dodawanie kontrolera](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Spowoduje to utworzenie nowego pliku "MoviesController.cs" poniżej naszej folder \Controllers w projekcie. Zaktualizujmy MovieController można pobrać listy filmów z naszym nowo wypełniania bazy danych.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Będziemy działają zapytanie LINQ, tak, aby tylko Pobieramy filmy wydana po lata 1984 roku. Będziemy potrzebować szablon widoku do renderowania tej listy filmów z powrotem, więc kliknij prawym przyciskiem myszy metodę i wybierz pozycję Dodaj widok, aby go utworzyć.

W oknie dialogowym Dodaj widok firma Microsoft będzie wskazywać, przechodzi listy&lt;Movies.Models.Movie&gt; do naszych szablonu widoku. W przeciwieństwie do poprzednich razy możemy użyć okno dialogowe dodawania widoku i wybrał opcję utworzenia "Pusty" szablon tym razem, firma Microsoft będzie wskazują, że chcemy, aby automatycznie "tworzenia szkieletu" Wyświetl szablon dla nas z niektórych domyślnej zawartości programu Visual Studio. Możemy to zrobić, wybierając pozycję "List" elementu "menu Widok zawartości listy rozwijanej.

Należy pamiętać, że jeśli masz utworzony nowej klasy, należy skompilować aplikację, aby wyświetlane w oknie dialogowym Dodawanie widoku.

![Dodawanie widoku](getting-started-with-mvc-part5/_static/image3.png)

Kliknij przycisk Dodaj, a system automatycznie wygeneruje kod dla widoku dla nas wyświetlanych z naszej listy filmów. Jest to dobry moment, aby zmienić &lt;h2&gt; nagłówek na wartość podobną "Moja lista filmu", takie jak były wykonywane wcześniej przy użyciu widoku Hello World.

[![Filmy — Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Uruchom aplikację, a następnie odwiedź /Movies na pasku adresu. Firma Microsoft została teraz pobierane są dane z bazy danych przy użyciu podstawowego zapytania wewnątrz kontrolera i zwrócone dane do widoku, który zna filmy. Ten widok, a następnie uruchamia się za pośrednictwem listy filmów i tworzy tabelę danych dla nas.

[![Lista filmu — Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Firma Microsoft nie będzie zaimplementowana funkcji edycji, szczegółów i usuwania, z tą aplikacją — więc nie potrzebujemy łącza domyślny szablon szkieletu utworzony dla nas. Otwórz plik /Movies/Index.aspx i usuń je.

Poniżej przedstawiono kod źródłowy dla naszych zaktualizowany szablon widoku jak powinien wyglądać po możemy wprowadzić te zmiany:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Łączy, które firma Microsoft nie będzie już konieczne, która zostanie utworzona, więc usuniemy je w tym przykładzie Zostanie zachowana, aby nasze Utwórz nowe łącze, ponieważ to jest dalej! Oto jak wygląda naszą aplikację z tej kolumny usunięte.

[![Lista filmu — Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

W efekcie powstał prostą listę danych filmu. Jednak po kliknięciu przycisku "Utwórz nowy" link przejdziemy błąd, ponieważ nie jest podłączany! Przejdźmy implementuje metody tworzenia akcji i umożliwić użytkownikowi wprowadzanie nowych filmów w naszej bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part4.md)
> [dalej](getting-started-with-mvc-part6.md)
