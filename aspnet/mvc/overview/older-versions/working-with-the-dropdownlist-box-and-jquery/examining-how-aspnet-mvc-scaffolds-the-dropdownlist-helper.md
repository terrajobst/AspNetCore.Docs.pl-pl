---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Badanie sposobu ASP.NET MVC scaffolds pomocnika DropDownList | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 09d2d7a0df5e8ffa14160b7d3c16b1e9da905fa1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874086"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Badanie sposobu ASP.NET MVC scaffolds pomocnika DropDownList
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie wybierz **Dodaj kontroler**. Nazwa kontrolera **StoreManagerController**. Ustaw opcje **Dodaj kontroler** okna dialogowego, jak pokazano na poniższej ilustracji.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Edytuj *StoreManager\Index.cshtml* wyświetlić i usunąć `AlbumArtUrl`. Usuwanie `AlbumArtUrl` spowoduje, że prezentacji bardziej czytelne. Poniżej przedstawiono kompletny kod.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Otwórz *Controllers\StoreManagerController.cs* plików i Znajdź `Index` metody. Dodaj `OrderBy` klauzuli, albumów będą sortowane według ceny. Poniżej przedstawiono kompletny kod.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Sortowanie według cen ułatwi do testowania zmiany w bazie danych. Podczas testowania edycji i Utwórz metody można użyć wysokich kosztów, zapisane dane będą występować jako pierwszy.

Otwórz *StoreManager\Edit.cshtml* pliku. Dodaj następujący wiersz zaraz po znacznika legendy.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Poniższy kod przedstawia kontekstu tej zmiany:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId` Jest wymagana, aby wprowadzić zmiany w rekordzie albumu.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Zaznacz, aby **Admin** łącze, a następnie wybierz **Utwórz nowy** łącze, aby utworzyć nowy album. Sprawdź, czy informacje albumu zostały zapisane. Edytuj album i sprawdź, czy dokonane zmiany są zachowywane.

### <a name="the-album-schema"></a>Album schematu

`StoreManager` Kontrolera utworzonego przez mechanizm szkieletów MVC umożliwia CRUD (tworzenia, odczytu, aktualizacji, usuwania) dostęp do albumów w bazie danych magazynu utworów muzycznych. Poniżej przedstawiono schematu albumu informacji:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums` Tabeli nie przechowuje genre albumu i opis, przechowuje klucz obcy do `Genres` tabeli. `Genres` Tabela zawiera genre nazwę i opis. Podobnie `Albums` tabela nie zawiera nazwy wykonawcy albumu, ale klucz obcy do `Artists` tabeli. `Artists` Tabela zawiera nazwę wykonawcy. Jeśli sprawdzanie danych w `Albums` tabeli widać, każdy wiersz zawiera klucz obcy do `Genres` tabeli i klucza obcego do `Artists` tabeli. Na poniższym obrazie Pokaż dane tabeli z `Albums` tabeli.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Wybierz HTML Tag

Kod HTML `<select>` elementu (utworzony przez HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) pomocnika) służy do wyświetlania pełną listę wartości (takie jak lista gatunkami muzyki). Dla formularzy edycji gdy bieżąca wartość jest znana, lista wyboru można wyświetlić bieżącą wartość. Widzieliśmy tym wcześniej podczas umieszczania wybranej wartości **komedii**. Lista wyboru jest idealny dla wyświetlanie kategorii lub dane klucza obcego. `<select>` Element klucza obcego Genre Wyświetla listę nazw możliwe genre, ale po zapisaniu formularza właściwość Genre został zaktualizowany o rodzaju wartości klucza obcego, nie nazwy wyświetlane rodzaju. Na poniższej ilustracji jest genre wybrane **Disco** i wykonawcy **lato Donna**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Badanie platformy ASP.NET MVC szkieletu kodu

Otwórz *Controllers\StoreManagerController.cs* plików i Znajdź `HTTP GET Create` metody.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create` Metoda dodaje dwa [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) obiekty do `ViewBag`, co ma zawierać informacje o rodzaju, a drugi zawiera informacje o wykonawcy. [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) przeładowania konstruktora użyta powyżej przyjmuje trzy argumenty:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *elementy*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) zawierających elementy na liście. W powyższym przykładzie listy gatunkami muzyki zwrócone przez `db.Genres`.
2. *dataValueField*: Nazwa właściwości w **IEnumerable** listę zawierającą wartości klucza. W powyższym przykładzie `GenreId` i `ArtistId`.
3. *dataTextField*: Nazwa właściwości w **IEnumerable** listy, który zawiera informacje do wyświetlania. W artystów i tabeli genre `name` pole jest używane.

Otwórz *Views\StoreManager\Create.cshtml* pliku i sprawdź, czy `Html.DropDownList` znaczników pomocnika dla pola rodzaju.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

Pierwszy wiersz pokazuje, że trwa tworzenie widoku `Album` modelu. W `Create` metody powyższym, żaden model został przekazany, więc pobiera widoku **null** `Album` modelu. W tym momencie Tworzenie nowego albumu, firma Microsoft nie ma żadnych `Album` danych dla niego.

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) przeciążenia pokazano powyżej przyjmuje nazwę pola, które można powiązać z modelem. Również używa tej nazwy do wyszukania **obiekt ViewBag** obiekt zawierający [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) obiektu. Za pomocą tego przeciążenia, konieczne jest nazwa **SelectList obiekt ViewBag** obiektu `GenreId`. Drugi parametr (`String.Empty`) jest wyświetlany tekst wyświetlany, gdy nie wybrano elementu. Jest to dokładnie, co chcemy podczas tworzenia nowego albumu. Jeśli drugi parametr usunięte i użyć poniższego kodu:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Lista wyboru domyślnie pierwszym elementem lub skale w naszym przykładzie.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Badanie `HTTP POST Create` metody.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

To przeciążenie metody `Create` ma metodę `album` obiekt utworzony przez system powiązanie modelu platformy ASP.NET MVC z wartości formularza opublikowane. Po przesłaniu nowego albumu, jeśli stan modelu jest nieprawidłowy i nie ma żadnych błędów bazy danych, baza danych jest dodana nowego albumu. Na poniższej ilustracji przedstawiono tworzenie nowego albumu.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Można użyć [narzędzie fiddler](http://www.fiddler2.com/fiddler2/) do sprawdzenia wartości przesłanego formularza że wiązanie modelu ASP.NET MVC używa do utworzenia obiektu albumu.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Tworzenie elementów ViewBag SelectList refaktoryzacji

Zarówno `Edit` metod i `HTTP POST Create` metoda ma taki sam kod, aby skonfigurować **SelectList** w **obiekt ViewBag**. W w rozumieniu [suchej](http://en.wikipedia.org/wiki/Don't_repeat_yourself), firma Microsoft będzie Refaktoryzuj tego kodu. Firma Microsoft będzie wprowadzać później refaktoryzowane Użyj tego kodu.

Utworzenie nowej metody, aby dodać wykonawcy i genre **SelectList** do **obiekt ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Zastąp dwa wiersze, ustawienie `ViewBag` w każdym `Create` i `Edit` metody wywołaniem `SetGenreArtistViewBag` — metoda. Poniżej przedstawiono kompletny kod.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Tworzenie nowego albumu i edytowanie album, aby sprawdzić, czy działa zmiany.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Jawnie przekazywanie SelectList do DropDownList

Tworzenie i edytowanie widoków utworzone przy użyciu szkieletów ASP.NET MVC następujące **DropDownList** przeciążenia:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList` Znaczników Tworzenie widoku są wyświetlane poniżej.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Ponieważ `ViewBag` właściwość `SelectList` nosi nazwę `GenreId`, **DropDownList** użyje Pomocnika `GenreId` **SelectList** w **obiekt ViewBag** . W następujących **DropDownList** przeciążenia, `SelectList` jawnie przekazano.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Otwórz *Views\StoreManager\Edit.cshtml* pliku, a następnie zmień **DropDownList** jawnie należy przekazać w wywołaniu **SelectList**, za pomocą przeciążenia powyżej. W tym rodzaju kategorii. Kompletny kod jest pokazany poniżej:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Uruchom aplikację, a następnie kliknij przycisk **Admin** łącza, a następnie przejdź do albumów Jazz i wybierz **Edytuj** łącza.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Zamiast przedstawiający Jazz jako aktualnie zaznaczonego genre, zostanie wyświetlony skale. Jeśli argument ciągu (właściwości do powiązania) i **SelectList** obiektu mają taką samą nazwę, wybranej wartości nie jest używany. Gdy nie mają żadnej wartości wybranych pod warunkiem, przeglądarek domyślnie pierwszym elementem w **SelectList**(czyli **skale** w powyższym przykładzie). Jest to znane ograniczenie **DropDownList** pomocnika.

Otwórz *Controllers\StoreManagerController.cs* plików i zmień **SelectList** nazwy do obiektów `Genres` i `Artists`. Kompletny kod jest pokazany poniżej:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Nazwy Genres i artystów są lepiej nazwy kategorii, ponieważ zawierają one tylko identyfikator każdej kategorii. Refaktoryzacja, który wcześniej robiliśmy płatnej. Zamiast zmiany **obiekt ViewBag** w czterech metod naszych zmiany zostały odizolowaną w celu `SetGenreArtistViewBag` metody.

Zmień **DropDownList** wywołań tworzenia i edytowania widoków do używania nowych **SelectList** nazwy. Nowy kod znaczników dla widoku edycji jest pokazany poniżej:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Utwórz widok wymaga pusty ciąg, aby zapobiec wyświetlaniu pierwszy element SelectList.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Tworzenie nowego albumu i edytowanie album, aby sprawdzić, czy działa zmiany. Testowania kodu edycji, wybierając album z określonego rodzaju niż skale.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Z elementem pomocniczym DropDownList za pomocą modelu widoku

Utwórz nową klasę w folderze ViewModels o nazwie `AlbumSelectListViewModel`. Zastąp kod w `AlbumSelectListViewModel` klasy następującym kodem:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel` Konstruktor ma albumu, listę artystów i genres i tworzy obiekt zawierający album i `SelectList` genres i artystów.

Skompiluj projekt tak `AlbumSelectListViewModel` jest dostępna w przypadku możemy utworzyć widok w następnym kroku.

Dodaj `EditVM` metodę `StoreManagerController`. Poniżej przedstawiono kompletny kod.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Kliknij prawym przyciskiem myszy `AlbumSelectListViewModel`, wybierz pozycję **rozwiązać**, następnie **przy użyciu MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Alternatywnie można dodać następujące instrukcję using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Kliknij prawym przyciskiem myszy `EditVM` i wybierz **Dodaj widok**. Użyj opcji wymienionych poniżej.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Wybierz **Dodaj**, następnie zamienić zawartość *Views\StoreManager\EditVM.cshtml* pliku następującym kodem:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM` Znaczników jest bardzo podobny do oryginalnej `Edit` znaczników z następującymi wyjątkami.

- Właściwości w modelu `Edit` widoku są w formie `model.property`(na przykład `model.Title` ). Właściwości w modelu `EditVm` widoku są w formie `model.Album.property`(na przykład `model.Album.Title`). Jest to spowodowane tym `EditVM` do widoku została przekazana kontener `Album`, a nie `Album` jako w `Edit` widoku.
- **DropDownList** drugi parametr pochodzi z modelu widoku nie **obiekt ViewBag**.
- **BeginForm** pomocnika w `EditVM` widoku jawnie dokonuje ogłoszenia `Edit` metody akcji. Publikując z powrotem do `Edit` akcji, nie trzeba było zapisać `HTTP POST EditVM` akcji można wykorzystywać `HTTP POST` `Edit` akcji.

Uruchom aplikację i edytowanie albumu. Zmień adres URL do użycia `EditVM`. Zmień pola i kliknij przycisk **zapisać** przycisk, aby zweryfikować kodu.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Rozwiązania należy używać?

Wszystkie trzy podejścia, wyświetlane są acceptible. Wielu deweloperów wolą przebiegu explictily `SelectList` do `DropDownList` przy użyciu `ViewBag`. Takie podejście charakteryzuje się dodatkową zaletę, zapewniając elastyczność przy użyciu nazwy bardziej odpowiednie dla kolekcji. Jest jedno zastrzeżenie: nie można nazwać `ViewBag SelectList` obiekt o takiej samej nazwie jak właściwość modelu.

Niektórzy deweloperzy preferowane podejście ViewModel. Inne osoby należy wziąć pod uwagę na pełniejsze znaczników i generowane HTML podejścia ViewModel uprzywilejowanych.

W tej sekcji możemy uzyskanych przy użyciu na trzy sposoby **DropDownList** z danymi kategorii. W następnej sekcji pokażemy jak dodać nową kategorię.

> [!div class="step-by-step"]
> [Poprzednie](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [dalej](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
