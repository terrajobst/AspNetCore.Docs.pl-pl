---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Dodawanie weryfikacji do modelu | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 1a8c186d5a6b00aaf1061bb4025f4f062203a7df
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402984"
---
<a name="adding-validation-to-the-model"></a>Dodawanie weryfikacji do modelu
====================
przez [Scotta Hanselmana](https://github.com/shanselman)

> Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych. Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.


W tej sekcji użyjemy Obsługa niezbędne do obsługi sprawdzania poprawności danych wejściowych w naszej aplikacji. Firma Microsoft będzie zagwarantować, że naszej zawartości bazy danych zawsze jest poprawna i podaj komunikaty o błędach pomocne dla użytkowników końcowych, gdy próbują i wprowadź dane filmów, która jest nieprawidłowa. Rozpocznie się, dodając niewielką logikę walidacji do klas filmu.

Kliknij prawym przyciskiem folder modelu, a następnie wybierz pozycję Dodaj klasę. Nazwa klasy filmu.

Podczas tworzenia modelu Entity Movie wcześniej, IDE utworzone klasy filmu. W rzeczywistości częścią klasy filmu może być w jednym pliku, a część w innym. Jest to nazywane klasy częściowej. Zamierzamy rozszerzyć klasę film z innego pliku.

Utworzymy klasy częściowej filmu odwołujący się do "buddy class" z niektórych atrybutów, które umożliwia uzyskanie wskazówek dotyczących sprawdzania poprawności do systemu. Firma Microsoft będzie należy oznaczyć tytuł i ceny, ponieważ wymagana, a następnie nalegać również, że cena można w pewnym zakresie. Kliknij prawym przyciskiem myszy folderu modeli, a następnie wybierz pozycję Dodaj klasę. Nazwa klasy filmu, a następnie kliknij przycisk OK. Oto, co nasz częściowe filmu klasy wygląda.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Uruchom ponownie aplikację i spróbuj wprowadzić film z ceną ponad 100. Zostanie wyświetlony błąd, po przesłaniu formularza. Błąd zostanie przechwycony po stronie serwera i występuje po opublikowaniu formularza. Zwróć uwagę, jak pomocników HTML wbudowanych w platformy ASP.NET MVC zostały inteligentnego wyświetlić komunikat o błędzie i Obsługa wartości dla nas w elementach pole tekstowe:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Działa to świetnie, ale dobrze byłoby, jeśli można o użytkownika po stronie klienta, natychmiast, zanim serwer pobiera związane.

Umożliwia włączenie niektórych weryfikacji po stronie klienta za pomocą języka JavaScript.

## <a name="adding-client-side-validation"></a>Dodawanie walidacji po stronie klienta

Ponieważ klasa nasz film zawiera już niektórych atrybutów sprawdzania poprawności, po prostu musimy dodać kilka plików JavaScript do naszych Create.aspx Wyświetl szablon i Dodaj wiersz kodu w celu włączenia weryfikacji po stronie klienta została wykonana.

Z poziomu VWD naszych folder widoków/filmu go i otwórz Create.aspx.

Otwórz folder skryptów w Eksploratorze rozwiązań i przeciągnij następujące trzy skrypty można w ciągu &lt;head&gt; tagu.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Chcesz, aby te pliki skryptów, pojawią się w podanej kolejności.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Ponadto Dodaj pojedynczy wiersz powyżej Html.BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Poniżej przedstawiono kod przedstawiony w środowisku IDE.

[![Filmy — Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Uruchom aplikację ponownie odwiedzić /Movies/Create i kliknij przycisk Utwórz, bez konieczności wprowadzania żadnych danych. Komunikaty o błędach pojawiają się natychmiast bez flash, że powiązane z wysyłania danych strony aż do serwera. Jest tak, ponieważ platformy ASP.NET MVC jest teraz Walidacja danych wejściowych, zarówno klienta (przy użyciu języka JavaScript) i na serwerze.

[![Tworzenie — Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

To jest dobra wydajność! Teraz Dodajmy jedną dodatkową kolumnę w bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part6.md)
> [dalej](getting-started-with-mvc-part8.md)
