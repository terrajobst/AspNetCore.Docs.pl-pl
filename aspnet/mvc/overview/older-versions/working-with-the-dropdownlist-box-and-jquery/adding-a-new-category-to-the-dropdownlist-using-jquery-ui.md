---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Dodawanie nowej kategorii do DropDownList przy użyciu interfejsu użytkownika jQuery | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 16f7af1d679aace24fff86abb19740beebafe785
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Dodawanie nowej kategorii do DropDownList przy użyciu interfejsu użytkownika jQuery
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

Kod HTML `Select` tag jest idealny dla prezentowanie dane stałej kategorii, ale często konieczne jest dodanie nowej kategorii. Załóżmy, że chcemy, aby dodać genre "Opera" do kategorii w naszej bazie danych? W tej sekcji użyjemy jQuery interfejsu użytkownika można dodać okno dialogowe, które możemy użyć, aby dodać nową kategorię. Na poniższym obrazie pokazano, jak interfejsu użytkownika będą obecne w przeglądarce.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Gdy użytkownik wybierze **Dodaj nowe Genre** łącza, wyskakujące okno dialogowe monituje użytkownika o nazwę nowej genre (i opcjonalnie opis). Obraz poniżej Pokaż **dodać Genre** podręczne okno dialogowe.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Po wprowadzeniu nową nazwę genre i **zapisać** przycisk jest naciśnięty, wykonywane są następujące czynności:

1. Wywołanie AJAX przesyła dane do metody Create kontrolera Genre, co nowego genre jest zapisywany w bazie danych i zwraca nowy rodzaju informacje (genre nazwy i Identyfikatora) jako JSON.
2. JavaScript dodaje nowe dane genre do listy wyboru.
3. JavaScript sprawia, że nowe genre wybranego elementu.

   Na ilustracji poniżej **Opera** został dodany do bazy danych i wybrana w **Genre** listy rozwijanej. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Otwórz *Views\StoreManager\Create.cshtml* plików i Zastąp znaczników genre następujące następujący kod:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre` Widoku częściowego będzie zawierać całą logikę umożliwiającą dołączenie JavaScript i jQuery używane do implementowania nowej funkcji genre Dodaj. Firma Microsoft ma zakończonego kod będzie prosty w tym samym z wykonawcy interfejsu użytkownika.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *Views\StoreManager* i wybierz polecenie **Dodaj**, następnie **widoku**. W **nazwy widoku** danych wejściowych, wprowadź `_ChooseGenre` następnie wybierz **Dodaj**. Zastąp kod znaczników w *Views\StoreManager\\_ChooseGenre.cshtml* pliku następującym kodem:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

Pierwszy wiersz deklaruje możemy przekazywane w `Album` jako nasz model, dokładnie tak samo modelu odnaleziony w widoku Tworzenie instrukcji. Następnych kilku wierszy są **etykiety** pomocnika znaczników. Następnego wiersza jest **DropDownList** wywołać pomocnika, dokładnie tak samo jak oryginalne widoku Utwórz. Dodaje następnego wiersza łącze o nazwie `Add New Genre`, i style go jak przycisk. Wiersz zawierający `ValidationMessageFor` jest kopiowana bezpośrednio z tworzenia widoku. Następujące wiersze:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Tworzy DZIEL ukrytego, o identyfikatorze `genreDialog`. Używamy jQuery do Podłączanie naszych **dodać Genre** okno dialogowe z Identyfikatorem `genreDialog` w tym div. Ostatnie dwa tagi skryptu zawiera łącza do plików JavaScript, które będą używane do implementowania nowej funkcji genre Dodaj. */Scripts/chooseGenre.js* pliku została podana dla Ciebie w projekcie zostaną omówione ją później w samouczku.

Uruchom aplikację i wybierz polecenie **Dodaj nowe Genre** przycisku. W **dodać Genre** okna dialogowego wprowadź **Opera** w **nazwa** pola wejściowego.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Kliknij przycisk **zapisać** przycisku. Wywołanie AJAX kategorii Opera tworzy następnie wypełnia listy rozwijanej z Opera i ustawia Opera jako wybranego rodzaju.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Wprowadź wykonawcy, tytuł i cen, a następnie wybierz **Utwórz** przycisku. Po wprowadzeniu cen mniejszej niż $8.99 nowy album pojawi się u góry widoku indeksu. Sprawdź, czy nowy wpis albumu został zapisany w bazie danych.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Spróbuj utworzyć nowy genre z tylko jedną literę. Poniższy kod w *Models\Genre.cs* plik Ustawia minimalną i maksymalną długość nazwy rodzaju.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Sprawdzanie poprawności po stronie klienta zgłasza, że należy wprowadzić ciąg od 2 do 20 znaków.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Badanie jak Genre nowe jest dodawany do bazy danych i listy wybierz.

Otwórz *Scripts\chooseGenre.js* pliku i sprawdź, czy kod.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

Drugi wiersz używa Identyfikatora `genreDialog` Aby utworzyć okno dialogowe na znacznik div w *Views\StoreManager\\_ChooseGenre.cshtml* pliku. Większość nazwane parametry to wymaga dodatkowych objaśnień. `autoOpen` Parametr ma wartość false, wybierając **utworzyć Genre** przycisku spowoduje otwarcie dialogu jawnie (to jest opisany w ostatnim). Okno dialogowe ma dwa przyciski **zapisać** i **anulować**. **Anulować** kliknięcie przycisku powoduje zamknięcie okna dialogowego. Poniższy kod przedstawia **zapisać** przycisk funkcji.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm` Wybrano `createGenreForm` identyfikatora. `createGenreForm` Identyfikator został ustawiony w poniższym kodzie w *Views\Genre\\_CreateGenre.cshtml* pliku.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) przeciążenia pomocnika używany w *Views\Genre\\_CreateGenre.cshtml* pliku generuje kod HTML z atrybutem akcji zawierający adres URL przesyłania formularza. Można to sprawdzić wyświetlanie albumów tworzenia strony w przeglądarce, a, wybierając źródło Pokaż w przeglądarce. Następujący kod przedstawia wygenerowanego kodu HTML zawierający tag formularza.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery `$.post` wiersza wykonuje wywołanie AJAX atrybut akcji (`/StoreManager/Create`) i przekazuje dane z **utworzyć Genre** okno dialogowe. Dane składa się z nazwy dla rodzaju nowy i opcjonalny opis. Jeśli wywołanie AJAX zakończy się pomyślnie, Nowa nazwa genre i wartości są dodawane do znaczników wybierz, a nowe genre ma ustawioną wartość wybranej wartości. Ponieważ jest to dynamicznie generowanym znaczników, użytkownik nie widzi nowej opcji wybierz wyświetlając źródła w przeglądarce. Widać nowy HTML przy użyciu narzędzi deweloperskich programu Internet Explorer 9 F12. Aby wyświetlić nową opcję Wybierz, w programie Internet Explorer 9, naciśnij klawisz F12, aby uruchomić narzędzia deweloperskie F12. Przejdź do tworzenia strony i dodać określonego rodzaju nowy, aby nowe genre jest zaznaczony na liście wybierz genre. W narzędziach developer F12:

1. Wybierz kartę HTML.
2. Kliknij ikonę odświeżania.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. W polu wyszukiwania wprowadź GenreID.
4. Korzystając z ikony w dalej   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Przejdź do następującego tagu wybierz:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Rozwiń ostatnią wartość opcji.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Poniższy kod w *Scripts\chooseGenre.js* pliku przedstawiono **Dodaj nowe Genre** połączeniu Zdarzenie kliknięcia przycisku i jak **Dodaj nowe Genre** jest okno dialogowe utworzony.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

Pierwszy wiersz tworzy funkcję kliknij dołączony do **Dodaj nowe Genre** przycisku. Następujący kod z Views\StoreManager\\_ChooseGenre.cshtml pliku pokazuje sposób **Dodaj nowe Genre** utworzeniu przycisk:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Metoda load tworzy i otwiera okno dialogowe Dodawanie Genre i wywołuje jQuery `parse` dlatego sprawdzanie poprawności klienta jest wykonywane na dane wprowadzane w oknie dialogowym.

W tej sekcji mają przedstawiono sposób tworzenia okna dialogowego, który może służyć do dodania nowych danych kategorii do listy wyboru. Można użyć tej samej procedury, można utworzyć interfejsu użytkownika, aby dodać nowy wykonawcy do listy wyboru wykonawcy. W tym samouczku udzielił Omówienie pracy z Pomocnik kodu HTML programu ASP.NET MVC **DropDownList**. Aby uzyskać dodatkowe informacje na temat pracy z **DropDownList**, zobacz sekcję odwołań do dodawania poniżej. Prosimy o kontakt czy został pomocne w tym samouczku.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Dodatkowe informacje

- [ASP.NET MVC — kaskadowych w listy rozwijanej wymieniono samouczek](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) przez [Radu enuca o](https://weblogs.asp.net/raduenuca/default.aspx)
- [Wybrana](http://harvesthq.github.com/chosen/) wtyczkę A JavaScript, która obsługuje wielokrotnego wyboru i filtrowania.

### <a name="contributors"></a>Współautorzy

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Osoby dokonujące przeglądu

- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Jan Pope
- Tomasz Dykstra

> [!div class="step-by-step"]
> [Poprzednie](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
