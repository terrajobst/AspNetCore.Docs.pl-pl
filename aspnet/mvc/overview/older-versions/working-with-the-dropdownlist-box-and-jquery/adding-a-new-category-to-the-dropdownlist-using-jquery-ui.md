---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Dodawanie nowej kategorii do metody DropDownList przy użyciu interfejs użytkownika jQuery | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: e2528b74b714a3f691b07ed2429b3fe9eb3c2074
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802529"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Dodawanie nowej kategorii do metody DropDownList przy użyciu interfejs użytkownika jQuery
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

Kod HTML `Select` tag jest idealny dla prezentowanie danych na stałych kategorii, ale często trzeba dodać nową kategorię. Załóżmy, że chcemy dodać gatunku "Opera" do kategorii w naszej bazie danych? W tej sekcji użyjemy interfejs użytkownika jQuery dodać okno dialogowe, które możemy użyć, aby dodać nową kategorię. Na poniższej ilustracji przedstawiono, jak interfejs użytkownika spowoduje wyświetlenie w przeglądarce.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Gdy użytkownik wybierze **Dodaj nowe gatunku** łącza, wyskakujące okno dialogowe monituje użytkownika o nazwę nowego gatunku (i opcjonalnie opis). Obraz poniżej przedstawiono **Dodaj gatunku** wyskakującego okna dialogowego.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Po wprowadzeniu nowych nazw gatunku i **Zapisz** wypchnięty przycisku, wykonywane są następujące czynności:

1. Wywołania AJAX przesyła dane do metody Utwórz kontroler gatunku, który zapisuje nowe gatunku do bazy danych i zwraca nowy gatunku informacje (gatunku nazwa i identyfikator) jako dane JSON.
2. JavaScript dodaje nowe dane gatunku do listy wyboru.
3. JavaScript sprawia, że nowe gatunku wybranego elementu.

   Na ilustracji poniżej **Opera** zostało dodane do bazy danych i wybrać w **gatunku** listy rozwijanej. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Otwórz *Views\StoreManager\Create.cshtml* plik i Zastąp kod znaczników gatunku następującym kodem następujący kod:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre` Widoku częściowego zawiera całą logikę umożliwiającą podpiąć JavaScript i jQuery, używany do implementowania Dodaj nową funkcję gatunku. Po ukończyliśmy kod będzie prosty sposób taki sam jak wykonawcy interfejsu użytkownika.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *Views\StoreManager* i wybierz polecenie **Dodaj**, następnie **widoku**. W **nazwy widoku** danych wejściowych, wprowadź `_ChooseGenre` polecenie **Dodaj**. Zastąp kod znaczników w *Views\StoreManager\\_ChooseGenre.cshtml* pliku następującym kodem:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

Pierwszy wiersz deklaruje, że firma Microsoft jest przesyłany w `Album` jako nasz model dokładnie tak samo modelu instrukcji znalezionej w widoku Create. Następnych kilka wierszy są **etykiety** pomocnika znaczników. Następny wiersz jest **DropDownList** wywołać pomocnika, tak samo jak w oryginalnym widokiem Utwórz. Następny wiersz dodaje link o nazwie `Add New Genre`, i style go jako przycisku. Wiersz zawierający `ValidationMessageFor` jest kopiowana bezpośrednio z widoku Create. Następujące wiersze:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Tworzy div ukrytych, o identyfikatorze `genreDialog`. Będziemy używać jQuery aby zaczepić naszych **Dodaj gatunku** okno dialogowe z Identyfikatorem `genreDialog` w tym div. Ostatnie dwa tagi skryptu zawierają łącza do plików JavaScript, które zostaną użyte do zaimplementowania Dodaj nową funkcję gatunku. */Scripts/chooseGenre.js* pliku zostanie podana dla Ciebie w projekcie, firma Microsoft będzie go sprawdzić później w samouczku.

Uruchom aplikację, a następnie kliknij pozycję **Dodaj nowe gatunku** przycisku. W **Dodaj gatunku** okna dialogowego wprowadź **Opera** w **nazwa** pola wejściowego.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Kliknij przycisk **Zapisz** przycisku. Wywołania AJAX tworzy kategorię Opera następnie wypełnia listę rozwijaną z Opera i ustawia Opera jako wybranego gatunku.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Wprowadź wykonawcy, tytuł i ceny, a następnie wybierz **Utwórz** przycisku. Jeśli wprowadzasz cenie mniej niż $8.99 nowy album pojawi się u góry widoku indeksu. Sprawdź, czy nowy wpis albumu została zapisana w bazie danych.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Spróbuj utworzyć nowy gatunku z tylko jedną literę. Poniższy kod w *Models\Genre.cs* pliku, Ustawia minimalną i maksymalną długość nazwy gatunku.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Weryfikacji po stronie klienta zgłasza, że należy wprowadzić ciąg od 2 do 20 znaków.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Badanie sposobu gatunku nowe jest dodawany do bazy danych i listy wybierz.

Otwórz *Scripts\chooseGenre.js* pliku i poszukaj w kodzie.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

Drugi wiersz używa Identyfikatora `genreDialog` utworzyć okno dialogowe na tag div w *Views\StoreManager\\_ChooseGenre.cshtml* pliku. Większość nazwanych parametrów to samodzielnie objaśnienia. `autoOpen` Parametr ma wartość false, wybierając **tworzenie gatunku** przycisku spowoduje to otwarcie okna dialogowego jawnie (jest to opisane ostatnie na). Okno dialogowe ma dwa przyciski o **Zapisz** i **anulować**. **Anulować** przycisk zamyka okno dialogowe. Poniższy kod przedstawia **Zapisz** przycisk funkcji.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm` Wybrana w zaufanym `createGenreForm` identyfikatora. `createGenreForm` Identyfikator został ustawiony w poniższym kodzie w *Views\Genre\\_CreateGenre.cshtml* pliku.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) przeciążenia pomocnika używany w *Views\Genre\\_CreateGenre.cshtml* generuje plik HTML z atrybutem akcji zawierającego adres URL można przesłać formularza. Można to zobaczyć, wyświetlając strona albumu tworzenia w przeglądarce i wybranie źródła Pokaż w przeglądarce. Następujący kod przedstawia wygenerowanego kodu HTML zawierający tag formularza.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery `$.post` wiersz wykonuje wywołanie AJAX do atrybutu działanie (`/StoreManager/Create`) i przekazuje dane z **tworzenie gatunku** okno dialogowe. Dane składają się nazwy gatunku nowy i opcjonalny opis. W przypadku pomyślnego wywołania AJAX nowej gatunku nazwy i wartości, które są dodawane do wybierz znaczników, a nowe gatunku jest ustawiona na wybranej wartości. Ponieważ jest to dynamicznie generowanych znaczników, nie widzisz nowej opcji wybierz opcję, wyświetlając źródła w przeglądarce. Możesz zobaczyć nowy HTML przy użyciu narzędzi deweloperskich programu Internet Explorer 9 F12. Aby wyświetlić nową opcję Wybierz, w programie Internet Explorer 9, naciśnij klawisz F12, aby uruchomić narzędzia programistyczne F12. Przejdź do tworzenia strony i dodać nowe gatunku, aby nowe gatunku jest zaznaczony na liście wybierz gatunku. W narzędzia programistyczne F12:

1. Wybierz kartę HTML.
2. Naciśnij ikonę odświeżania.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. W polu wyszukiwania wprowadź GenreID.
4. Za pomocą ikony dalej   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Przejdź do następującego tagu wybierz:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Rozwiń ostatnią wartość opcji.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Poniższy kod w *Scripts\chooseGenre.js* plik pokazuje, jak **Dodaj nowe gatunku** połączeniu Zdarzenie kliknięcia przycisku i sposób, w jaki **Dodaj nowe gatunku** to okno dialogowe utworzony.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

Pierwszy wiersz tworzy funkcję kliknij dołączone do **Dodaj nowe gatunku** przycisku. Następujące znaczniki z Views\StoreManager\\_ChooseGenre.cshtml plik pokazuje sposób, w jaki **Dodaj nowe gatunku** przycisku zostanie utworzony:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Metoda load tworzy i otwiera okno dialogowe Dodawanie gatunku, a wywołuje jQuery `parse` metody, aby sprawdzanie poprawności klienta występuje na dane wprowadzone w oknie dialogowym.

W tej sekcji mają przedstawiono sposób tworzenia okna dialogowego, który może służyć do dodawania nowych danych kategorii do listy wyboru. Możesz wykonać tej samej procedury, aby utworzyć interfejs użytkownika, aby dodać nowe wykonawcy do listy wyboru wykonawcy. W tym samouczku udzielił Omówienie pracy z Pomocnika ASP.NET MVC HTML **DropDownList**. Aby uzyskać dodatkowe informacje na temat pracy z **DropDownList**, zobacz sekcję odwołania do dodawania poniżej. Daj nam znać czy w tym samouczku został pomocne.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Dodatkowe informacje

- [ASP.NET MVC — kaskadowych w liście rozwijanej są wyświetlane samouczek](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) przez [Radu Enuca o](https://weblogs.asp.net/raduenuca/default.aspx)
- [Wybrane](http://harvesthq.github.com/chosen/) wtyczki języka JavaScript, który obsługuje wielokrotnego wyboru i filtrowania.

### <a name="contributors"></a>Współautorzy

- [Radu Enuca o](https://weblogs.asp.net/raduenuca/default.aspx)
- Goupil Jean Sébastien
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Recenzenci

- Goupil Jean Sébastien
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike Pope
- Tom Dykstra

> [!div class="step-by-step"]
> [Poprzednie](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
