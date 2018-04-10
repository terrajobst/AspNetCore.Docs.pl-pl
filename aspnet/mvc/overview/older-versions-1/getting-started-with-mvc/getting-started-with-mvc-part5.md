---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Uzyskiwanie dostępu do modelu danych z kontrolera | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utwórz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Uzyskiwanie dostępu do modelu danych z kontrolera
====================
przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.


W tej sekcji zamierzamy Utwórz nową klasę MoviesController i napisanie kodu, pobiera naszych danych filmu i wyświetla je do przeglądarki przy użyciu szablonu widoku.

Kliknij prawym przyciskiem myszy folder kontrolery i utworzyć nowy MoviesController.

[![Dodawanie kontrolera](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Spowoduje to utworzenie nowego pliku "MoviesController.cs" poniżej naszych folderu \Controllers w ramach naszych projektu. Teraz zaktualizuj MovieController można pobrać listy filmów z naszych nowo wypełnione bazy danych.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Firma Microsoft wykonywania zapytań LINQ, aby tylko pobieranie filmów wydaną po lata 1984. Potrzebujemy szablonu widoku do renderowania tej listy filmów z powrotem, więc kliknij prawym przyciskiem myszy w metodzie i wybierz opcję Dodaj widok, aby go utworzyć.

W oknie dialogowym Dodawanie widoku firma Microsoft będzie wskazywać, przekazywane listy&lt;Movies.Models.Movie&gt; do naszej szablonu widoku. W przeciwieństwie do poprzednich razy możemy użyć okna dialogowego Dodawanie widoku i wybrał opcję utworzenia szablonu "Pusty" Firma Microsoft będzie wskazują, że teraz chcemy, aby Visual Studio, aby automatycznie "utworzyć szkielet" Wyświetl szablon dla nas z zawartością niektóre domyślne. Firma Microsoft będzie to zrobić, wybierając element "List" w "menu rozwijanym zawartości widoku.

Należy pamiętać, że jeśli masz utworzony nową klasę należy skompilować aplikację dla niego były wyświetlane w oknie dialogowym Dodawanie widoku.

![Dodaj widok](getting-started-with-mvc-part5/_static/image3.png)

Kliknij przycisk Dodaj, a system automatycznie generuje kod dla widoku firmie Microsoft, które wyświetla naszej listy filmów. Jest to odpowiedni moment, aby zmienić &lt;h2&gt; nagłówek podobną "Moje listy filmów", jak robiliśmy wcześniej z widokiem Hello World.

[![Filmów - Microsoft Visual Web Developer 2010 Express.](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Uruchom aplikację, a następnie odwiedź /Movies na pasku adresu. Wprowadzeniu teraz pobrać danych z bazy danych przy użyciu zapytania podstawowego wewnątrz kontrolera i zwrócone dane do widoku, który zna filmów. Ten widok, a następnie obraca za pośrednictwem listy filmów i tworzy tabelę danych firmie Microsoft.

[![Listy filmów - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Firma Microsoft nie będzie można Implementowanie funkcje edycji, szczegóły i Usuń z tej aplikacji — potrzebujemy nie domyślnych łączy utworzony szablon szkieletu firmie Microsoft. Otwarcie pliku /Movies/Index.aspx i usuń je.

Oto co powinna wyglądać naszych zaktualizowany szablon widoku po możemy wprowadzić te zmiany kodu źródłowego:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Tworzenia łącza, które nie będą potrzebne, a więc będzie usunąć je w tym przykładzie. Firma Microsoft zachowa naszych utworzyć nowy link, jak jest to dalej! Oto, jak wygląda aplikacji z tą kolumną usunięte.

[![Listy filmów - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Mamy teraz proste wykaz danych filmu. Jednak po kliknięciu pozycji "Utwórz nowy" łącza, firma Microsoft będzie wyświetlany komunikat o błędzie jako nie jest podłączonymi! Umożliwia implementuje metody tworzenia akcji i umożliwić użytkownikom wprowadzanie nowości w naszej bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part4.md)
> [dalej](getting-started-with-mvc-part6.md)
