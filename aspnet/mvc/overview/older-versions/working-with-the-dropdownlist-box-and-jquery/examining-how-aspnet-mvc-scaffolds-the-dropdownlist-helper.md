---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Badanie sposobu platformy ASP.NET MVC scaffolds pomocnika DropDownList | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: ece3645f2b37550058a5c93bdd9badbc088ae11c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382968"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Badanie sposobu scaffolds pomocnika DropDownList przez platformy ASP.NET MVC
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folder, a następnie wybierz **Dodaj kontroler**. Nazwa kontrolera **StoreManagerController**. Ustaw opcje **Dodaj kontroler** okna dialogowego, jak pokazano na poniższej ilustracji.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Edytuj *StoreManager\Index.cshtml* wyświetlić i usunąć `AlbumArtUrl`. Usuwanie `AlbumArtUrl` spowoduje, że prezentacja bardziej czytelne. Poniżej przedstawiono kompletny kod.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Otwórz *Controllers\StoreManagerController.cs* plików i Znajdź `Index` metody. Dodaj `OrderBy` klauzuli tak albumów zostaną posortowane według ceny. Poniżej przedstawiono kompletny kod.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Sortowanie według ceny ułatwi testowanie zmian w bazie danych. Podczas testowania edycji, a następnie utworzyć metody, można użyć niskiej cenie, więc zapisane dane będą wyświetlane na początku.

Otwórz *StoreManager\Edit.cshtml* pliku. Dodaj następujący wiersz po tagu legendy.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Poniższy kod pokazuje kontekstu tej zmiany:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId` Jest wymagany do wprowadzania zmian w rekordzie albumu.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Zaznacz, aby **administratora** łącze, a następnie wybierz **Utwórz nowy** link, aby utworzyć nowego albumu. Sprawdź, czy informacje albumu zostały zapisane. Edytuj album i sprawdź, czy dokonane zmiany są zachowywane.

### <a name="the-album-schema"></a>Schemat albumu

`StoreManager` Kontroler utworzony przez mechanizm tworzenia szkieletów MVC umożliwia dostęp CRUD (tworzenia, odczytu, Update, Delete) do albumów w bazie danych magazynu music. Schemat albumu informacje znajdują się poniżej:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums` Tabeli nie przechowuje gatunku albumu i opis, przechowuje klucz obcy, aby `Genres` tabeli. `Genres` Tabela zawiera gatunku nazwę i opis. Podobnie `Albums` tabela nie zawiera nazwa artystów albumu, ale klucz obcy, aby `Artists` tabeli. `Artists` Tabela zawiera nazwę wykonawcy. Jeśli zbadanie danych w `Albums` tabeli, możesz zobaczyć każdy wiersz zawiera klucz obcy, aby `Genres` tabeli i klucz obcy, aby `Artists` tabeli. Na poniższej ilustracji Pokaż dane tabeli z `Albums` tabeli.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Tag taga Select języka HTML

Kod HTML `<select>` — element (utworzone przez HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) pomocnika) jest używany, aby wyświetlić pełną listę wartości (takie jak lista gatunki). Na Edycja formularzy gdy znana jest bieżąca wartość, listy wyboru można wyświetlić bieżącą wartość. Widzieliśmy tym wcześniej podczas ustawimy wybranej wartości **Komedia**. Lista wyboru doskonale nadaje się do wyświetlania kategorii lub danych klucza obcego. `<select>` Element klucza obcego gatunku Wyświetla listę nazw gatunku możliwe, ale po zapisaniu formularza właściwość gatunku jest aktualizowana gatunku wartość klucza obcego, nie nazwy wyświetlane gatunku. Na poniższej ilustracji jest gatunku wybrane **Najdywania** i artystów **lato Donną**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Badanie platformy ASP.NET MVC szkieletu kodu

Otwórz *Controllers\StoreManagerController.cs* plików i Znajdź `HTTP GET Create` metody.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create` Metoda dodaje dwa [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) obiekty do `ViewBag`, do zawierają informacje gatunku, a drugi zawiera informacji o wykonawcy. [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) przeciążenia konstruktora powyżej przyjmuje trzy argumenty:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *elementy*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) zawierający elementy na liście. W powyższym przykładzie lista gatunki zwrócone przez `db.Genres`.
2. *dataValueField*: Nazwa właściwości w **IEnumerable** listę zawierającą wartości klucza. W powyższym przykładzie `GenreId` i `ArtistId`.
3. *dataTextField*: Nazwa właściwości w **IEnumerable** listę zawierającą informacje do wyświetlenia. Zarówno artyści, jak i tabela gatunku `name` pole jest używane.

Otwórz *Views\StoreManager\Create.cshtml* plików i zbadaj `Html.DropDownList` znaczników pomocy dla pola gatunku.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

Pierwszy wiersz pokazuje, że trwa tworzenie widoku `Album` modelu. W `Create` metoda powyżej, model nie został przekazany, więc pobiera widoku **null** `Album` modelu. W tym momencie tworzymy nowy album więc nie ma żadnych `Album` danych dla niego.

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) przeciążenia powyżej przyjmuje nazwę pola, aby powiązać modelu. Również używa tej nazwy do wyszukania **obiekt ViewBag** obiekt zawierający [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) obiektu. Korzystając z tego przeciążenia, wymagane jest nazwa **SelectList obiekt ViewBag** obiektu `GenreId`. Drugi parametr (`String.Empty`) jest tekst do wyświetlenia, gdy nie wybrano elementu. Jest to dokładnie, co chcemy, aby podczas tworzenia nowego albumu. Jeśli usunięte drugi parametr i użyć następującego kodu:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Domyślnie lista wyboru do pierwszego elementu lub Rock w naszym przykładzie.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Badanie `HTTP POST Create` metody.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

To przeciążenie `Create` metoda przyjmuje `album` obiekt utworzony przez system powiązań modelu ASP.NET MVC z opublikowanych wartości formularza. Po przesłaniu nowego albumu, jeśli stan modelu jest nieprawidłowy i nie ma żadnych błędów bazy danych, nowy album dodaniu bazy danych. Na poniższej ilustracji przedstawiono tworzenie nowego albumu.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Możesz użyć [narzędzie fiddler](http://www.fiddler2.com/fiddler2/) umożliwiającej sprawdzenie wartości przesłanego formularza że wiązanie modelu programu ASP.NET MVC używa w celu utworzenia obiektu albumu.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Tworzenie elementów ViewBag SelectList refaktoryzacji

Zarówno `Edit` metod i `HTTP POST Create` metoda ma identyczny kod, aby skonfigurować **SelectList** w **obiekt ViewBag**. W ducha [susz](http://en.wikipedia.org/wiki/Don't_repeat_yourself), ten kod będzie refaktoryzacji. My sprawiamy, żeby to użycie zaprojektowane od nowa kodu później.

Utwórz nową metodę, aby dodać gatunku, wykonawcy i **SelectList** do **obiekt ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Zastąp dwa wiersze ustawienie `ViewBag` we wszystkich `Create` i `Edit` metody z wywołaniem `SetGenreArtistViewBag` metody. Poniżej przedstawiono kompletny kod.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Utwórz nowy album i Edytuj album, aby sprawdzić, czy zmiany działają.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Jawne przekazywanie SelectList do metody DropDownList

Widoki tworzyć i edytować utworzone przy użyciu tworzenia szkieletu ASP.NET MVC następujące **DropDownList** przeciążenia:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList` Znaczników widoku Utwórz znajdują się poniżej.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Ponieważ `ViewBag` właściwość `SelectList` nosi nazwę `GenreId`, **DropDownList** użyje Pomocnika `GenreId` **SelectList** w **obiekt ViewBag** . W następującym **DropDownList** przeciążenia, `SelectList` jawnie jest przekazywany w.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Otwórz *Views\StoreManager\Edit.cshtml* pliku, a następnie zmień **DropDownList** wywołania jawne przekazywanie w **SelectList**, za pomocą przeciążenia powyżej. W tym dla kategorii gatunku. Kompletny kod jest pokazany poniżej:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Uruchom aplikację, a następnie kliknij przycisk **administratora** połączyć, a następnie przejdź do albumów Jazz i wybierz **Edytuj** łącza.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Zamiast wyświetlania Jazz jako aktualnie wybranego gatunku, Rock jest wyświetlany. Gdy argument ciągu (właściwość można powiązać) i **SelectList** obiektu mają taką samą nazwę, wybranej wartości nie jest używany. W przypadku nie podano żadnej wartości wybranej przeglądarki domyślne do pierwszego elementu w **SelectList**(czyli **Rock** w powyższym przykładzie). Jest to znane ograniczenie **DropDownList** pomocnika.

Otwórz *Controllers\StoreManagerController.cs* pliku, a następnie zmień **SelectList** nazwy do obiektów `Genres` i `Artists`. Kompletny kod jest pokazany poniżej:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Gatunki i artystów są lepsze nazwy kategorii, ponieważ zawierają one więcej niż tylko identyfikator wystąpienia każdej kategorii. Refaktoryzacja, które były wykonywane wcześniej opłaciło. Zamiast zmieniać **obiekt ViewBag** w cztery metody były izolowane do zmian `SetGenreArtistViewBag` metody.

Zmiana **DropDownList** wywołania tworzenia i edytowania widoków, aby korzystać z nowych **SelectList** nazwy. Poniżej przedstawiono nowy kod znaczników dla widoku edycji:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Utwórz widok wymaga pusty ciąg, aby uniemożliwić wyświetlenie pierwszy element SelectList.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Utwórz nowy album i Edytuj album, aby sprawdzić, czy zmiany działają. Przetestować kod edycji, wybierając album z określonego rodzaju inne niż skale.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Przy użyciu Pomocnika DropDownList przy użyciu modelu widoku

Utwórz nową klasę w folderze modele widoków o nazwie `AlbumSelectListViewModel`. Zastąp kod w `AlbumSelectListViewModel` klasy następującym kodem:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel` Konstruktor przyjmuje albumu, listę artystów i gatunki i tworzy obiekt zawierający album i `SelectList` gatunki i artystów.

Skompiluj projekt, więc `AlbumSelectListViewModel` jest dostępna podczas tworzenia widoku w następnym kroku.

Dodaj `EditVM` metody `StoreManagerController`. Poniżej przedstawiono kompletny kod.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Kliknij prawym przyciskiem myszy `AlbumSelectListViewModel`, wybierz opcję **rozwiązać**, następnie **przy użyciu MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Alternatywnie, można dodać następujące instrukcję using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Kliknij prawym przyciskiem myszy `EditVM` i wybierz **Dodaj widok**. Za pomocą opcji poniżej.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Wybierz **Dodaj**, następnie zastąp zawartość *Views\StoreManager\EditVM.cshtml* pliku następującym kodem:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM` Znaczników jest bardzo podobny do oryginalnego `Edit` znaczników z następującymi wyjątkami.

- Właściwości w modelu `Edit` widoku mają postać `model.property`(na przykład `model.Title` ). Właściwości w modelu `EditVm` widoku mają postać `model.Album.property`(na przykład `model.Album.Title`). To dlatego, że `EditVM` widok jest przekazywany kontener `Album`, a nie `Album` jak `Edit` widoku.
- **DropDownList** drugi parametr pochodzi z modelu widoku nie **obiekt ViewBag**.
- **BeginForm** pomocnika w `EditVM` Wyświetl jawnie wpisy do `Edit` metody akcji. Publikując z powrotem do `Edit` akcji, firma Microsoft nie ma konieczności zapisywania `HTTP POST EditVM` akcji i ponownie użyć `HTTP POST` `Edit` akcji.

Uruchom aplikację, a następnie edytuj albumu. Zmień adres URL, aby użyć `EditVM`. Zmienianie pola, a następnie kliknij przycisk **Zapisz** przycisk, aby sprawdzić kod działa.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Które rozwiązanie należy użyć?

Wszystkie trzy metody wyświetlane są akceptowalne. Wielu programistów chce explictily — dostęp próbny `SelectList` do `DropDownList` przy użyciu `ViewBag`. Takie podejście ma dodatkową zaletę, zapewniając elastyczność przy użyciu bardziej odpowiednie nazwy dla kolekcji. Jedno zastrzeżenie: to nie nazwa `ViewBag SelectList` obiekt o nazwie identycznej z nazwą właściwości modelu.

Niektórzy deweloperzy preferowane podejście ViewModel. Inne należy wziąć pod uwagę pełniejszy znaczników i wygenerowany kod HTML podejście ViewModel uprzywilejowanych.

W tej sekcji nauczyliśmy się przy użyciu na trzy sposoby **DropDownList** kategorii danych. W następnej sekcji pokażemy sposób dodać nową kategorię.

> [!div class="step-by-step"]
> [Poprzednie](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [dalej](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
