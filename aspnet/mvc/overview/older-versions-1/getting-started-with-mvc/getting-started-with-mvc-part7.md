---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Dodawanie walidacji do modelu | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utwórz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 78dd6bdd81fcb51a3a21a8f1ee12b4b2bfc37db5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-validation-to-the-model"></a>Dodawanie walidacji do modelu
====================
przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.


W tej sekcji zamierzamy Obsługa niezbędne do obsługi sprawdzania poprawności danych wejściowych w naszej aplikacji. Firma Microsoft będzie Sprawdź, czy zawartość bazy danych zawsze jest poprawna i udostępnić komunikaty przydatne dla użytkowników końcowych, jeśli spróbuj i wprowadź dane Movie, która jest nieprawidłowa. Rozpocznie się przez dodanie do klasy filmu niewielką ilością logiki sprawdzania poprawności.

Kliknij prawym przyciskiem folder modelu i wybierz opcję Dodaj klasę. Nazwa klasy filmu.

Gdy modelu jednostki filmu utworzony wcześniej, IDE utworzone klasy filmu. W rzeczywistości część klasy filmu może być w jednym pliku, a część w innym. Jest to klasy częściowej. Chcemy rozszerzenie klasy Movie z innego pliku.

Utworzymy klasy częściowe filmu się "Klasa buddy" z niektóre atrybuty, które zapewni wskazówek weryfikacji do systemu. Firma Microsoft będzie Oznacz tytuł i cen wymagane i domagać również, że ceny można w pewnym zakresie. Kliknij prawym przyciskiem myszy folder modeli i wybierz opcję Dodaj klasę. Nazwa klasy film, a następnie kliknij przycisk OK. Oto, co naszych częściowe filmu klasy prawdopodobnie.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Ponownie uruchom aplikację i spróbuj wprowadzić filmu cena ponad 100. Zostanie wyświetlony błąd, po przesłaniu formularza. Błąd zostanie przechwycony po stronie serwera i po opublikowania formularza. Zwróć uwagę, jak inteligentne, wyświetlony komunikat o błędzie i Obsługa wartości firmie Microsoft w elementach pole tekstowe zostały wbudowane pomocników HTML ASP.NET MVC:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

To rozwiązanie idealne, ale byłoby nieuprzywilejowany możemy podać użytkownika po stronie klienta, natychmiast, zanim pobiera związane z serwera.

Umożliwia włączenie niektórych weryfikacji po stronie klienta z użyciem języka JavaScript.

## <a name="adding-client-side-validation"></a>Dodawanie walidacji po stronie klienta

Ponieważ klasy Nasze filmu już niektóre atrybuty weryfikacji, po prostu musimy dodać kilka plików JavaScript do naszych Create.aspx wyświetlanie szablonu i dodanie wiersza kodu w celu włączenia weryfikacji po stronie klienta odbywa się.

Z poziomu VWD przejdź do folderu naszych widoków/film i otwarcie Create.aspx.

Otwórz folder skryptów w Eksploratorze rozwiązań i przeciągnij następujące trzy skrypty w &lt;head&gt; tagu.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Chcesz, aby te pliki skryptów pojawią się w tej kolejności.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Ponadto Dodaj pojedynczy wiersz powyżej Html.BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Oto kod wyświetlany w środowisku IDE.

[![Filmów - Microsoft Visual Web Developer Express 2010 (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Uruchom aplikację ponownie odwiedź /Movies/Create i kliknij przycisk Utwórz bez wprowadzania żadnych danych. Komunikaty o błędach pojawiają się natychmiast bez flash, że powiązane z wysyłanie danych strony wszystkie sposób z powrotem do serwera. Jest tak, ponieważ ASP.NET MVC jest teraz Walidacja danych wejściowych, zarówno klienta (przy użyciu języka JavaScript) i na serwerze.

[![Tworzenie - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Zależy to dobry! Teraz Dodajmy jedną dodatkową kolumnę w bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part6.md)
> [dalej](getting-started-with-mvc-part8.md)
