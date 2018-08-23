---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Część 5: Edycja formularzy i tworzenie szablonów | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 5 obejmuje Edycja formularzy i szablonów.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: c1065dcb45b6d28672edba32b95c7fc476c8b944
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753924"
---
<a name="part-5-edit-forms-and-templating"></a>Część 5: Edycja formularzy i tworzenie szablonów
====================
przez [Galloway'em Jon](https://github.com/jongalloway)

> MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.  
>   
> MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.
> 
> W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 5 obejmuje Edycja formularzy i szablonów.


W ciągu ostatnich rozdziale firma Microsoft była podczas ładowania danych z naszej bazie danych i wyświetlanie ich. W tym rozdziale będzie pozwalamy również edycji danych.

## <a name="creating-the-storemanagercontroller"></a>Tworzenie StoreManagerController

Firma Microsoft rozpocznie się, tworząc nowy kontroler o nazwie **StoreManagerController**. Dla tego kontrolera firma Microsoft będzie się korzystanie z funkcji tworzenia szkieletu ASP.NET MVC 3 Tools Update. Ustaw opcje dla okna dialogowego Dodaj kontroler, jak pokazano poniżej.

![](mvc-music-store-part-5/_static/image1.png)

Po kliknięciu przycisku Dodaj, zobaczysz, że mechanizm tworzenia szkieletu ASP.NET MVC 3 nie dobre ilość pracy za Ciebie:

- Tworzy nowy StoreManagerController za pomocą zmiennej lokalnej platformy Entity Framework
- Dodaje StoreManager folder do folderu widoków projektu
- Dodaje widok Create.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml i Index.cshtml silnie typizowaną do klasy albumu

![](mvc-music-store-part-5/_static/image2.png)

Nowa klasa kontrolera StoreManager obejmuje CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) akcji kontrolera, które wiedzą, jak pracować z Album klasa modelu i kontekstu nasze narzędzia Entity Framework na użytek dostępu do bazy danych.

## <a name="modifying-a-scaffolded-view"></a>Modyfikowanie szkieletu widoku

Należy pamiętać, że chociaż ten kod został wygenerowany dla nas, jest standardowy kod ASP.NET MVC, tak samo, jak został możemy pisania w tym samouczku. Jest ona przeznaczona do zaoszczędzić czas, możesz poświęcić na pisaniu kodu kontrolera standardowy i ręczne tworzenie silnie typizowane widoki, ale nie jest to typ wygenerowanego kodu można było zaobserwować poprzedzone znakiem dire ostrzeżeń w komentarze na temat sposobu nie może zmienić Kod. Jest to Twój kod i oczekiwaniami go zmienić.

Tak Zacznijmy od szybka edycja do widoku indeksu StoreManager (/ Views/StoreManager/Index.cshtml). Ten widok przedstawia tabeli, w którym jest wyświetlana lista albumów w naszym Sklepie za pomocą edycji / szczegóły / Usuń linki i zawiera właściwości publiczne albumu. Usuniemy pole AlbumArtUrl, ponieważ nie jest to przydatne w przypadku tego ekranu. W &lt;tabeli&gt; sekcji widoku kodu, należy usunąć &lt;th&gt; i &lt;td&gt; elementy otaczającego AlbumArtUrl odwołań, wskazane przez wyróżnione wiersze poniżej:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Wyświetl zmodyfikowany kod będzie wyglądać następująco:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Pierwsze spojrzenie na Menedżera Store

Teraz uruchom aplikację i przejdź do/StoreManager /. Spowoduje to wyświetlenie tylko zmodyfikowany, wyświetlanie listy albumów w magazynie wraz z łączami do edytowania szczegółów i usuwania indeksu Menedżera Store.

![](mvc-music-store-part-5/_static/image3.png)

Kliknij link Edytuj przedstawia formularz edycji z polami Album, w tym menu rozwijanych dla wykonawcy i gatunku.

![](mvc-music-store-part-5/_static/image4.png)

Kliknij link "Powrót do listy" u dołu, a następnie kliknij łącze szczegółowych dla albumu. Zostaną wyświetlone szczegółowe informacje dotyczące poszczególnych albumu.

![](mvc-music-store-part-5/_static/image5.png)

Ponownie kliknij przycisk Wstecz do listy łącza, a następnie kliknij łącze Usuń. Spowoduje to wyświetlenie okna dialogowego potwierdzenia, wyświetlania szczegółów albumu ani zastanawiać się, jeśli firma Microsoft upewnieniu się, że chcemy go usunąć.

![](mvc-music-store-part-5/_static/image6.png)

Kliknięcie przycisku Usuń u dołu Usuń album i powrót do strony indeksu przedstawiono albumu usunięte.

Jeszcze nie skończyliśmy przy użyciu Menedżera Store, ogranicza NAS jednak praca kontrolera i Wyświetl kod dla operacji CRUD rozpocząć od.

## <a name="looking-at-the-store-manager-controller-code"></a>Patrząc kodu Store Menedżera kontrolera

Kontroler Menedżera Store zawiera dobre ilości kodu. Przejdźmy przez to od góry do dołu. Kontroler obejmuje niektóre standardowe przestrzenie nazw dla kontrolera MVC, a także odwołania do przestrzeni nazw naszych modeli. Kontroler ma prywatnej wystąpienie MusicStoreEntities, używanego przez poszczególne akcji kontrolera, aby uzyskać dostęp do danych.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Store Menedżera indeksu oraz szczegóły akcji

Widok indeksu umożliwia pobranie listy ze zdjęciami, w tym każdego albumu odwołania wykonawcy i gatunku, jak widzieliśmy wcześniej podczas pracy w metodzie Przeglądaj Store. Widok indeksu obserwowanych odwołania do obiektów połączonych, dzięki czemu może on zawierać każdego albumu gatunku nazwy i nazwy wykonawcy, więc kontrolera jest wydajny i wykonujących zapytania o te informacje w oryginalne żądanie.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Akcji kontrolera szczegóły kontrolera StoreManager działa tak samo jak informowaliśmy wcześniej — wysyła zapytanie o Album akcji szczegółów kontrolera Store za pomocą Identyfikatora przy użyciu metody Find(), zwraca go do widoku.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Tworzenie metod akcji

Tworzenie metod akcji są nieco inne niż te, które widzieliśmy w do tej pory, ponieważ obsługują dane wejściowe formularza. Gdy użytkownik najpierw odwiedzi /StoreManager/tworzenie/zobaczy pusty formularz. Ta strona HTML będzie zawierał &lt;formularza&gt; element, który zawiera lista rozwijana i pole tekstowe dane wejściowe elementów, gdzie można wprowadzić szczegóły albumu.

Po użytkownik wypełnia wartości formularza fotograficzne, ich naciśnij przycisk "Zapisz", aby przesłać te zmiany z powrotem do naszej aplikacji do zapisania w bazie danych. Gdy użytkownik naciśnie przycisk "Zapisz" &lt;formularza&gt; będą wykonywać akcję POST protokołu HTTP, do /StoreManager/Tworzenie/adresu URL i Prześlij &lt;formularza&gt; wartości jako część POST protokołu HTTP.

ASP.NET MVC pozwala łatwo podzielić logikę te dwa scenariusze wywołania adresu URL, co pozwala na implementowanie dwóch oddzielnych metod akcji "Utwórz" w ramach naszych klasy StoreManagerController — jeden do obsługi HTTP GET początkowej wskaż /StoreManager/Create / Adres URL, a druga do obsługi protokołu HTTP-POST przesłanych zmian.

### <a name="passing-information-to-a-view-using-viewbag"></a>Przekazanie informacji do widoku, używając elementów ViewBag

Firma Microsoft była używana obiekt ViewBag wcześniej w tym samouczku, ale nie jeszcze wiele o nim rozmawialiśmy. Obiekt ViewBag pozwala nam do przekazywania informacji do widoku bez użycia obiektu silnie typizowany model. W tym przypadku nasze akcji kontrolera HTTP-GET Edytuj musi podawać zarówno listę gatunki i artystów do formularza w celu wypełniania list rozwijanych i zwraca je jako elementy obiekt ViewBag jest najprostszym sposobem wykonania tego zadania.

Obiekt ViewBag jest obiekt dynamiczny, co oznacza, że można wpisać ViewBag.Foo lub ViewBag.YourNameHere bez konieczności pisania kodu, aby zdefiniować te właściwości. W tym przypadku kontrolera kod używa ViewBag.GenreId i ViewBag.ArtistId tak, aby wartości list rozwijanych przesłane za pomocą formularza GenreId i ArtistId, które nie są właściwości fotograficzne, które będzie można ich ustawienia.

Te wartości list rozwijanych są zwracane do formularza przy użyciu obiektu SelectList, której podstawą jest tylko do tego celu. Odbywa się przy użyciu kodu w następujący sposób:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Jak widać w kodzie metody akcji, trzy parametry są używane do utworzenia tego obiektu:

- Lista elementów, które będą wyświetlane listy rozwijanej. Należy pamiętać, że nie jest to po prostu określonym ciągiem — firma Microsoft jest przekazanie listę gatunki.
- Następny parametr przekazywany do SelectList jest wybrana wartość. Ten sposób SelectList wie, jak należy wstępnie wybrać element na liście. Są to łatwiejsze do zrozumienia, gdy spojrzymy na formularz edycji, co jest bardzo podobne.
- Ostatni parametr jest właściwość do wyświetlenia. W tym przypadku to wskazujący, że właściwość Genre.Name jest, co będzie widoczna dla użytkownika.

Mając to na uwadze następnie Akcja Utwórz HTTP GET jest całkiem proste — dwa SelectLists są dodawane do obiekt ViewBag i żaden obiekt modelu jest przekazywany do formularza (ponieważ jest ona nie został jeszcze utworzony).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Pomocników HTML do wyświetlenia szczegółu Drop w tworzenie widoku

Ponieważ Rozmawialiśmy o jak listy rozwijanej wartości są przekazywane do widoku, Przyjrzyjmy szybki widok, aby zobaczyć, jak te wartości są wyświetlane. W kodzie widoku (/ Views/StoreManager/Create.cshtml), zostaną wyświetlone następujące wywołanie do wyświetlania listy gatunku w dół.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Jest to nazywane pomocnika kodu HTML — metodę narzędzia, która wykonuje wspólne zadania widoku. Pomocników HTML są bardzo przydatne w zachowaniu naszego kodu widoku zwięzłe i czytelne. Pomocnik Html.DropDownList znajduje się przez platformę ASP.NET MVC, ale jak w dalszej części można utworzyć własne elementy pomocnicze dla widoku kodu, które firma Microsoft zostanie ponownie użyty w naszej aplikacji.

Po prostu wywołanie Html.DropDownList musi dowiedzieć się dwie rzeczy — gdzie należy get listy, aby wyświetlić i jakie wartości (jeśli istnieją) powinna być wstępnie wybrane. Pierwszy parametr, GenreId, informuje DropDownList do wyszukania wartość o nazwie GenreId w modelu lub obiekt ViewBag. Drugi parametr służy do wskazania wartość, aby pokazać, jak początkowo zaznaczone na liście rozwijanej. Ponieważ ta forma jest tworzenie formularza, nie ma wartości być instalowane i String.Empty jest przekazywany.

### <a name="handling-the-posted-form-values"></a>Obsługa wartości formularza opublikowane

Tak jak Omówiliśmy to przed, istnieją dwie metody akcji skojarzonych z każdym formularzu. Pierwszy obsługuje żądania HTTP GET i zostanie wyświetlony formularz. Drugi obsługuje żądania POST protokołu HTTP, który zawiera wartości przesłanego formularza. Należy zauważyć, że akcja kontrolera ma atrybut [HttpPost], który poinformuje platformę ASP.NET MVC, czy tylko powinien odpowiadać na żądania HTTP POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Ta akcja ma cztery obowiązki:

- 1. Odczyt wartości formularza
- 2. Sprawdź, w przypadku wartości formularza przekazać reguł sprawdzania poprawności
- 3. Jeśli przesłanie formularza jest prawidłowy, Zapisz dane i wyświetlić zaktualizowaną listę
- 4. Jeśli przesłanie formularza nie jest prawidłowy, Wyświetl ponownie formularz z błędami walidacji

#### <a name="reading-form-values-with-model-binding"></a>Odczytywanie wartości formularza za pomocą wiązania modelu

Przesłanie formularza, który zawiera wartości GenreId i ArtistId (z listy rozwijanej) i pole tekstowe wartości dla tytułu, ceny i AlbumArtUrl przetwarzania akcji kontrolera. Chociaż istnieje możliwość uzyskania bezpośredniego dostępu do wartości formularza, lepszym rozwiązaniem jest korzystanie z wbudowanych w platformy ASP.NET MVC możliwości wiązania modelu. W przypadku akcji kontrolera przyjmuje typ modelu jako parametru, ASP.NET MVC będzie podejmować próby wypełnienia obiektu tego typu za pomocą formularza danych wejściowych (a także wartości trasy i querystring). Jest to realizowane przez wyszukiwanie wartości, których nazwy odpowiadają właściwości obiektu modelu, np. podczas ustawiania wartości GenreId nowy obiekt fotograficzne, szuka danych wejściowych o nazwie GenreId. Podczas tworzenia widoków przy użyciu standardowych metod we wzorcu ASP.NET MVC, formularze będą zawsze być renderowany przy użyciu nazwy właściwości jako nazwy pola wejściowego, tak to nazwy pól będą po prostu zgodne.

#### <a name="validating-the-model"></a>Sprawdzanie poprawności modelu

Model została zweryfikowana za pomocą prostego wywołania do ModelState.IsValid. Firma Microsoft nie dodano reguł sprawdzania poprawności do naszych klasy albumu jeszcze - wykonamy, które w znacznej — więc ten test nie ma wiele do zrobienia. Ważne jest, to sprawdzenie ModelStat.IsValid dostosuje się do reguł sprawdzania poprawności, które umieściliśmy na nasz model, dzięki czemu przyszłe zmiany reguł sprawdzania poprawności nie wymaga żadnych aktualizacji kodu akcji kontrolera.

#### <a name="saving-the-submitted-values"></a>Zapisywanie wartości przesłane

W przypadku przesyłania formularza pozytywnie przejdą weryfikację, nadszedł czas na zapisać wartości w bazie danych. Z platformą Entity Framework, która wymaga dodania modelu do kolekcji ze zdjęciami i wywoływać metodę SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework generuje odpowiednie polecenia SQL, aby zachować wartość. Po zapisaniu danych, organów powrót do listy ze zdjęciami, dzięki czemu możemy zobaczyć naszej aktualizacji. Polega to na zwracanie RedirectToAction nazwą akcji kontrolera, które mają być wyświetlane, firma Microsoft. W tym przypadku to metoda indeksu.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Wyświetlanie nieprawidłową postać zgłoszenia błędów sprawdzania poprawności

W przypadku wprowadzania nieprawidłową postać wartości list rozwijanych są dodawane do elementów ViewBag (tak jak w przypadku protokołu HTTP GET) i powiązany model wartości są przekazywane do widoku do wyświetlenia. Błędy sprawdzania poprawności są automatycznie wyświetlane przy użyciu @Html.ValidationMessageFor pomocnika kodu HTML.

#### <a name="testing-the-create-form"></a>Testowanie Utwórz formularz

Do przetestowania tego, uruchom aplikację i przejdź do /StoreManager/tworzenie / — spowoduje to wyświetlenie pusty formularz, który został zwrócony przez metodę HTTP GET tworzenie StoreController.

Wypełnij niektóre wartości, a następnie kliknij przycisk Utwórz, można przesłać formularza.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Obsługa zmiany

Edytowanie pary akcji (HTTP GET i POST protokołu HTTP) są bardzo podobne do tworzenia metod akcji, którą właśnie przyjrzeliśmy się. Ponieważ scenariusz edycji wymaga pracy z istniejącego albumu, Edytuj HTTP-GET metoda ładuje albumu na podstawie parametru "id" przekazaną za pomocą trasy. Ten kod do pobierania albumy AlbumId jest taka sama, jak wcześniej Opisaliśmy w szczegóły akcji kontrolera. Podobnie jak w przypadku tworzenia / metody GET protokołu HTTP, listy rozwijanej wartości są zwracane przez obiekt ViewBag. Dzięki temu do zwrócenia albumu naszych obiekt modelu do widoku (zdecydowanie jest wpisane w klasie albumu) podczas przekazywania dodatkowe dane (np. lista gatunki) za pośrednictwem obiekt ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

Akcja POST protokołu HTTP edycji jest bardzo podobny do akcji tworzenia POST protokołu HTTP. Jedyną różnicą jest to, że zamiast opcji dodawania nowych album z bazą danych. Kolekcja ze zdjęciami, firma Microsoft jest znajdowania bieżące wystąpienie Album przy użyciu bazy danych. Entry(album) i ustawianie jego stanu Modified. Informuje Entity Framework, modyfikowania istniejącego albumu zamiast tworzenia nowej.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Firma Microsoft tę możliwość testowania działania aplikacji i przechodząc do/StoreManger /, a następnie klikając link edycji dla albumu.

![](mvc-music-store-part-5/_static/image9.png)

Spowoduje to wyświetlenie formularz edycji wyświetlane przez metodę GET HTTP edycji. Wypełnij niektóre wartości, a następnie kliknij przycisk Zapisz.

![](mvc-music-store-part-5/_static/image10.png)

Publikuje formularza, zapisuje wartości i zwraca nam do listy fotograficzne, pokazujący, że wartości zostały zaktualizowane.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Usuwanie obsługi

Usuwanie następuje tego samego wzorca jako Edit and Create, za pomocą jednego kontrolera akcji w celu wyświetlenia formularza potwierdzenia i innej akcji kontrolera, do obsługi przesyłania formularza.

Akcji kontrolera HTTP GET, Usuń dokładnie jest taka sama jak nasze poprzedniej akcji w szczegółach Menedżera Store kontrolera.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Możemy wyświetlić formularz, który jest silnie typizowane typu fotograficzne, przy użyciu szablonu zawartości widoku Delete.

![](mvc-music-store-part-5/_static/image12.png)

Szablon Usuń pokazuje wszystkie pola dla modelu, ale ułatwimy że jeszcze chwilę. Zmień kod widok w /Views/StoreManager/Delete.cshtml do następujących.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Spowoduje to wyświetlenie uproszczone potwierdzenie usunięcia.

![](mvc-music-store-part-5/_static/image13.png)

Kliknięcie przycisku Usuń powoduje, że formularz, aby zaksięgowania na serwerze, który wykonuje akcję DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Nasze akcji POST protokołu HTTP do kontrolera Usuń wykonuje następujące akcje:

- 1. Ładuje albumu według Identyfikatora
- 2. Usuwa je album i Zapisz zmiany
- 3. Przekierowuje do indeksu, pokazujący, że Album został usunięty z listy

Aby to sprawdzić, uruchom aplikację i przejdź do /StoreManager. Wybierz album z listy i kliknij łącze Usuń.

![](mvc-music-store-part-5/_static/image14.png)

Spowoduje to wyświetlenie naszych ekran potwierdzenia usunięcia.

![](mvc-music-store-part-5/_static/image15.png)

Kliknięcie przycisku Usuń Usuwa album i zwraca nam do strony indeksu Menedżera Store, który pokazuje, że album został usunięty.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Obetnij tekstu przy użyciu niestandardowego pomocnika HTML

Mamy jeden potencjalny problem z naszą stronę indeksu Menedżera Store. Nasze właściwości tytuł i nazwy wykonawcy zarówno można wystarczająco długi, że może wywoływać poza naszym formatowanie tabeli. Utworzymy niestandardowego pomocnika kodu HTML do pozwalają łatwo obciąć tych i innych właściwości w naszym widokach.

![](mvc-music-store-part-5/_static/image17.png)

Firmy razor @helper składni wprowadził dość łatwo jest tworzyć swoje własne funkcje pomocnicze do użytku w widoków. Powoduje ono otwarcie widoku /Views/StoreManager/Index.cshtml i Dodaj poniższy kod bezpośrednio po @model wiersza.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Ta metoda pomocnika przyjmuje ciąg i maksymalna długość, aby umożliwić. Jeśli tekst podany jest krótszy niż długość określona, pomocnika wyświetla go w formie-to. Jeśli jest on dłuższy, następnie go obcina tekst i renderuje "..." dla pozostałych.

Teraz możemy użyć naszego Truncate pomocnika, aby upewnić się, że obie tytuł wykonawcy nazwy właściwości i mniej niż 25 znaków. Pełny przegląd kodu za pomocą naszego nowego pomocnika Truncate pojawia się poniżej.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Teraz przeglądających /StoreManager/ adres URL, albumy i tytuły są utrzymywane na poniżej naszej maksymalnej długości.

![](mvc-music-store-part-5/_static/image18.png)

Uwaga: Spowoduje to pokazanie tak proste tworzenie i używanie pomocnika w jednym widoku. Aby dowiedzieć się więcej na temat tworzenia wątków, które można użyć w całej lokacji, zobacz wpis w blogu moje: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-4.md)
> [dalej](mvc-music-store-part-6.md)
