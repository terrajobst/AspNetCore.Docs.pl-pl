---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: "Część 5: Edycji formularzy i tworzenia szablonów | Dokumentacja firmy Microsoft"
author: jongalloway
description: "Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 5 obejmuje edycji formularzy oraz tworzenia szablonów."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: cde6fe133291254531a797a434a4b2cdd226dd5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-5-edit-forms-and-templating"></a>Część 5: Formularze edycji i tworzenia szablonów
====================
przez [Galloway Jan](https://github.com/jongalloway)

> Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.  
>   
> Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.
> 
> Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 5 obejmuje edycji formularzy oraz tworzenia szablonów.


W ciągu ostatnich rozdziale nasz zostały ładowania danych z naszej bazie danych i wyświetlanie go. W tym rozdziale firma Microsoft będzie także włączyć, edytowanie danych.

## <a name="creating-the-storemanagercontroller"></a>Tworzenie StoreManagerController

Rozpocznie się przez utworzenie nowego kontrolera o nazwie **StoreManagerController**. Dla tego kontrolera firma Microsoft będzie można korzystanie z funkcji szkieletów dostępnych w aktualizacji narzędzi programu ASP.NET MVC 3. Ustaw opcje dla okna dialogowego Dodawanie kontrolera, jak pokazano poniżej.

![](mvc-music-store-part-5/_static/image1.png)

Po kliknięciu przycisku Dodaj zobaczysz, że mechanizm szkieletów ASP.NET MVC 3 jest dobrym ilość pracy możesz:

- Tworzy nowy StoreManagerController z zmiennej lokalnej programu Entity Framework
- Dodaje StoreManager folder do folderu widoków projektu
- Dodawany widok Create.cshtml, Delete.cshtml Details.cshtml, Edit.cshtml i Index.cshtml, silnie typizowaną do klasy albumu

![](mvc-music-store-part-5/_static/image2.png)

Nowa klasa kontrolera StoreManager obejmuje CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) akcji kontrolera, które wiedzieć, jak pracować z Album klasa modelu i użyć naszego kontekstu programu Entity Framework dla dostępu do bazy danych.

## <a name="modifying-a-scaffolded-view"></a>Modyfikowanie szkieletu widoku

Należy pamiętać, że podczas ten kod został wygenerowany w firmie Microsoft, jest standardowego kodu platformy ASP.NET MVC, tak jak zostały możemy zapisu w tym samouczku. Ma ona przeznaczona do zapisania należy poświęcić czas na zapisywanie schematyczny kod kontrolera i ręczne tworzenie widoków silnie typizowaną, ale nie jest to typ wygenerowanego kodu może przejrzane poprzedzone znakiem kierunek ostrzeżeń w komentarze na temat sposobu nie może zmienić Kod. To jest kod i oczekiwaniami je zmienić.

Tak Zacznijmy szybkie edytowanie widoku indeksu StoreManager (/ Views/StoreManager/Index.cshtml). Ten widok przedstawia tabeli, które wymieniono albumów w naszym magazynie z edycji / szczegółów / Usuń linki i zawiera właściwości publicznej albumu. Usuniemy pole AlbumArtUrl, ponieważ nie jest to przydatne w przypadku tego ekranu. W &lt;tabeli&gt; sekcji kodu widoku, Usuń &lt;th&gt; i &lt;td&gt; otaczającego odwołania AlbumArtUrl opisane w poniższych wierszach wyróżnionych elementów:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Kod widoku zmodyfikowany będzie wyglądać następująco:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Pierwszy przyjrzeć się Menedżer magazynu

Teraz uruchom aplikację i przejdź do/StoreManager /. Spowoduje to wyświetlenie możemy po modyfikacji, przedstawiający listę albumów w magazynie wraz z łączami do edytowania szczegółów i usuwania indeksu Menedżera magazynu.

![](mvc-music-store-part-5/_static/image3.png)

Kliknięcie łącza edycji wyświetli formularz edycji z polami albumu, łącznie z list rozwijanych dla wykonawcy i rodzaju.

![](mvc-music-store-part-5/_static/image4.png)

Kliknij łącze "Powrót do listy" u dołu, a następnie kliknij łącze Szczegóły albumu. Spowoduje to wyświetlenie szczegółowych informacji o poszczególnych albumu.

![](mvc-music-store-part-5/_static/image5.png)

Ponownie kliknij przycisk Wstecz do listy łącza, a następnie kliknij łącze Usuń. Zostaną wyświetlone okno dialogowe potwierdzenia, pokazywanie szczegółów albumu i pytaniem, czy już wszystko się, że jeśli chcesz go usunąć.

![](mvc-music-store-part-5/_static/image6.png)

Kliknięcie przycisku Usuń u dołu spowoduje to usunięcie album i powrót do strony indeksu przedstawiono albumu usunięte.

Firma Microsoft nie wszystko gotowe przy użyciu Menedżera magazynu, ale mamy pracy kontrolera i widoku Kod operacji CRUD uruchomić z.

## <a name="looking-at-the-store-manager-controller-code"></a>Spojrzenie na kod kontrolera Menedżera magazynu

Kontroler Menedżera magazynu zawiera dobrej ilość kodu. Przejdź przez to od góry do dołu. Kontroler zawiera niektóre standardowe przestrzeni nazw dla kontrolera MVC, a także odwołania do przestrzeni nazw naszych modeli. Kontroler ma prywatnej wystąpienie MusicStoreEntities używany przez wszystkie akcje kontrolera dla dostępu do danych.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Przechowywanie działania Menedżera indeksu i szczegóły

Widok indeksu powoduje pobranie listy albumy, w tym każdego albumu przywoływanego wykonawcy i Genre, jak widzieliśmy wcześniej podczas pracy w metodzie Przeglądaj magazynu. Widok indeksu jest po odwołania do obiektów połączonych, dzięki czemu będzie możliwe wyświetlenie każdego albumu Genre nazwy i wykonawcy, więc kontrolera są wydajne i wykonywanie zapytania dla tych informacji w oryginalne żądanie.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Akcji kontrolera szczegóły kontrolera StoreManager działa tak samo jak informowaliśmy wcześniej — wysyła zapytanie albumu akcji szczegółów kontrolera magazynu według Identyfikatora przy użyciu metody Find(), zwraca go do widoku.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Tworzenie metody akcji

Tworzenie metody akcji są nieco inne niż te, które firma Microsoft w tym samouczku do tej pory, ponieważ obsługują dane wejściowe formularza. Gdy użytkownik odwiedza najpierw /StoreManager/Utwórz/będą one wyświetlane pusty formularz. Ta strona HTML będzie zawierać &lt;formularza&gt; element, który zawiera listy rozwijanej i pola tekstowego danych wejściowych elementów, w którym mogą oni wprowadzić szczegóły albumu.

Po użytkownik wypełnia albumu wartości formularza, ich naciśnij przycisk "Zapisz" do przesyłania tych zmian z powrotem do naszej aplikacji do zapisania w bazie danych. Gdy użytkownik naciśnie przycisk "Zapisz" &lt;formularza&gt; przeprowadzi HTTP POST do /StoreManager/Create lub adres URL i przesłać &lt;formularza&gt; wartości jako część POST protokołu HTTP.

ASP.NET MVC pozwala łatwo podzielić przez włączenie firmie Microsoft w celu wykonania dwóch oddzielnych metod akcji "Utwórz" w obrębie klasy Nasze StoreManagerController — jeden do obsługi początkowej Przeglądaj HTTP GET, aby /StoreManager/Create logikę tych dwóch scenariuszy wywołania adresu URL Lub adres URL, a drugi do obsługi protokołu HTTP POST do przesłanych zmian.

### <a name="passing-information-to-a-view-using-viewbag"></a>Przekazanie informacji do widoku przy użyciu elementów ViewBag

Firma Microsoft używano wcześniej w tym samouczku obiekt ViewBag, ale nie zawsze mówię większość informacji na ten temat. Obiekt ViewBag umożliwia firmie Microsoft w celu przekazywania informacji do widoku bez użycia obiektu jednoznacznie modelu. W takim przypadku musi naszych akcji kontrolera Edytuj GET protokołu HTTP do przekazywania zarówno listę Genres i artystów do formularza, aby wypełnić listę rozwijaną i zwraca je jako elementy obiekt ViewBag jest najprostszym sposobem, aby to zrobić.

Obiekt ViewBag jest obiekt dynamiczny, co oznacza, że możesz wpisać ViewBag.Foo lub ViewBag.YourNameHere bez pisania kodu, aby zdefiniować te właściwości. W tym przypadku kontrolera kodzie użyto ViewBag.GenreId i ViewBag.ArtistId tak, aby wartości listy rozwijanej przesłane za pomocą formularza GenreId i ArtistId, które są właściwości albumu, który będzie można ich ustawienia.

Te listy rozwijanej wartości są zwracane do formularza za pomocą obiektu SelectList, który jest przeznaczony tylko dla tego celu. Jest to realizowane przy użyciu kodu w następujący sposób:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Jak widać z kodu metody akcji, trzy parametry są używany do utworzenia tego obiektu:

- Lista elementów, które będą wyświetlane listy rozwijanej. Należy pamiętać, że nie jest to po prostu określonym ciągiem - możemy przechodząc listy gatunkami muzyki.
- Następny parametr przekazywany do SelectList jest wybrana wartość. Ten sposób SelectList wie jak wstępnie wybierz element na liście. Są to ułatwia zrozumienie, kiedy dokładnie w formularzu edycji jest bardzo podobne.
- Ostatni parametr jest właściwość do wyświetlenia. W takim przypadku tego wskazująca, że właściwość Genre.Name jest co będzie pokazywana użytkownikowi.

Z tym pamiętać następnie utworzyć HTTP GET akcja jest bardzo prosty — dwa SelectLists są dodawane do obiekt ViewBag i żaden obiekt modelu jest przekazywany do formularza (ponieważ go nie został jeszcze utworzony).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Pomocników HTML do wyświetlenia szczegółu Drop w tworzenie widoku

Ponieważ zajmowaliśmy jak listy rozwijanej wartości są przekazywane do widoku, Spójrzmy szybki w widoku, aby zobaczyć, jak te wartości są wyświetlane. W widoku kodu (/ Views/StoreManager/Create.cshtml), pojawi się następujące wywołanie dotyczące wyświetlania listy Genre w dół.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Jest to nazywane pomocnika kodu HTML — metodę narzędzia, która wykonuje wspólne zadania widoku. Pomocników HTML są przydatne w przypadku aktualizowania naszego kodu widoku zwięzły i do odczytu. Pomocnik Html.DropDownList są dostarczane przez program ASP.NET MVC, ale jako zajmiemy się tym później można utworzyć własne elementy pomocnicze dla widoku kodu, który firma Microsoft będzie ponowne użycie w naszej aplikacji.

Wywołanie Html.DropDownList właśnie musi być polecenie dwie czynności — w przypadku gdy get listy, aby wyświetlić i jakie korzyści (jeśli istnieje), należy wstępnie wybrane. Pierwszy parametr GenreId, określa, że lista DropDownList do wyszukania wartość o nazwie GenreId w modelu lub obiekt ViewBag. Drugi parametr jest służy do wskazania wartości, aby wyświetlić, początkowo wybrane na liście rozwijanej. Ponieważ ten formularz jest tworzenie formularza, nie mają być instalowane żadnej wartości, a String.Empty jest przekazywana.

### <a name="handling-the-posted-form-values"></a>Obsługa wartości formularza przesłane

Jak wspomniano przed, istnieją dwie metody akcji związane z każdego formularza. Pierwszy obsługuje żądania HTTP GET i zostanie wyświetlony formularz. Drugi obsługuje żądania POST protokołu HTTP, który zawiera wartości przesłanego formularza. Należy zauważyć, że kontroler akcji ma atrybut [HttpPost], który poinformuje platformę ASP.NET MVC, że tylko powinien odpowiadać na żądania HTTP POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Ta akcja ma cztery obowiązki:

- 1. Odczytać wartości formularza
- 2. Sprawdź, czy wartości formularza przekazać reguł sprawdzania poprawności
- 3. Jeśli przesyłania formularza jest nieprawidłowy, Zapisz dane i wyświetlić zaktualizowaną listę
- 4. Jeśli przesłanie formularza nie jest prawidłowy, Wyświetl ponownie formularz błędy sprawdzania poprawności

#### <a name="reading-form-values-with-model-binding"></a>Odczytywanie wartości formularza z powiązaniem modelu

Przesłanie formularza, która zawiera wartości GenreId i ArtistId (z listy rozwijanej) i wartości tekstowe dla tytułu, ceny i AlbumArtUrl przetwarza akcji kontrolera. Użytkownik może uzyskać bezpośredniego dostępu do wartości formularza, lepszym rozwiązaniem jest korzystanie z funkcji powiązania modelu wbudowanych w platformy ASP.NET MVC. Podczas działania kontrolera przyjmuje jako parametr typu modelu, ASP.NET MVC podejmie próbę wypełnienia obiektu tego typu za pomocą formularza danych wejściowych (a także wartości trasy i querystring). Dzieje się tak, wyszukując wartości, których nazwy są zgodne np. właściwości obiektu modelu, ustawiając wartość GenreId nowy obiekt albumu wygląda danych wejściowych o nazwie GenreId. Podczas tworzenia widoków w programie ASP.NET MVC przy użyciu standardowych metod formularze będą zawsze być renderowany przy użyciu nazwy właściwości jako nazwy pola wejściowego, tak to nazwy pól będzie po prostu zgodne.

#### <a name="validating-the-model"></a>Sprawdzanie poprawności modelu

Model została zweryfikowana za pomocą prostego wywołania ModelState.IsValid. Firma Microsoft nie dodano żadnych reguł sprawdzania poprawności do naszej klasy albumu jeszcze - robimy które bit - teraz tak to sprawdzenie nie ma wiele zrobić. Co to jest ważne jest, czy sprawdzanie ModelStat.IsValid będzie dostosować do reguł sprawdzania poprawności, które testujemy w modelu, więc przyszłe zmiany reguł sprawdzania poprawności nie wymagają żadnych aktualizacji do kodu akcji kontrolera.

#### <a name="saving-the-submitted-values"></a>Zapisywanie wartości przesłane

Przesłanie formularza przeszedł sprawdzania poprawności, jest czas, aby zapisać wartości w bazie danych. Z programu Entity Framework, który wymaga dodania modelu do kolekcji albumów i wywołanie metody SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework generuje odpowiednich poleceń SQL, aby zachować wartość. Po zapisaniu danych, możemy przekierowania powrót do listy albumy, widzimy naszej aktualizacji. Jest to realizowane przez zwrócenie RedirectToAction o nazwie akcji kontrolera, którą chcemy udostępnić wyświetlane. W takim przypadku to metoda indeksu.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Wyświetlanie przesłanych nieprawidłową postać z błędami sprawdzania poprawności

W przypadku nieprawidłową postać danych wejściowych wartości listy rozwijanej są dodawane do elementów ViewBag (tak jak w przypadku HTTP GET) i powiązany model wartości są przekazywane z powrotem do widoku do wyświetlenia. Błędy sprawdzania poprawności są automatycznie wyświetlane przy użyciu @Html.ValidationMessageFor pomocnika kodu HTML.

#### <a name="testing-the-create-form"></a>Testowanie formularza tworzenia

Aby przetestować ten limit, uruchom aplikację i przejdź do /StoreManager/Utwórz / — spowoduje to wyświetlenie pusty formularz, który został zwrócony przez metodę HTTP GET utworzyć StoreController.

Wypełnij niektóre wartości, a następnie kliknij przycisk Utwórz, aby przesłać formularza.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Obsługa zmiany

Edytowanie pary akcji (HTTP GET i POST protokołu HTTP) są bardzo podobne do tworzenia metod akcji, którą właśnie analizujemy. Ponieważ Edycja scenariusz obejmuje pracę z istniejącego albumu, Edytuj HTTP GET metody ładuje albumu na podstawie parametru "id" przekazany za pomocą trasy. Ten kod do pobierania albumy AlbumId jest taka sama, jak zostało wcześniej analizujemy w szczegóły akcji kontrolera. W przypadku tworzenia / metody HTTP GET listy rozwijanej wartości są zwracane przez obiekt ViewBag. To pozwala zwrócić albumu naszych obiekt modelu do widoku (który jest silnie typizowaną do klasy albumu) podczas przejścia dodatkowe dane (np. lista gatunkami muzyki) za pośrednictwem obiekt ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

Akcję POST protokołu HTTP edycji jest bardzo podobny do akcji tworzenia POST protokołu HTTP. Jedyną różnicą jest to, że zamiast opcji dodawania nowego albumu do bazy danych. Kolekcja albumy, firma Microsoft jest znajdowania bieżące wystąpienie klasy Album przy użyciu bazy danych. Entry(album) i ustawienie stanu Modified. Informuje Entity Framework, modyfikowania istniejącego albumu zamiast tworzenia nowej.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Firma Microsoft testuje tę możliwość uruchamiania aplikacji i przeglądanie/StoreManger /, a następnie klikając link edycji dla albumu.

![](mvc-music-store-part-5/_static/image9.png)

Spowoduje to wyświetlenie formularza edycji wyświetlany przez metodę GET HTTP edycji. Wypełnij niektóre wartości, a następnie kliknij przycisk Zapisz.

![](mvc-music-store-part-5/_static/image10.png)

Posty na formularzu, zapisuje te wartości i zwraca nam do listy albumy, pokazujący, że wartości zostały zaktualizowane.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Obsługa usuwania

Usuwanie jest zgodny ze wzorcem tej samej edycji i Utwórz przy użyciu jednego kontrolera akcji do wyświetlania formularza potwierdzenia i innej akcji kontrolera do obsługi przesyłania formularza.

Akcja kontrolera HTTP GET usunąć jest dokładnie taka sama naszych poprzedniej akcji kontrolera szczegóły Menedżera magazynu.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Formularz, który jest silnie typizowane wyświetli typu albumy, przy użyciu Delete wyświetlanie zawartości szablonu.

![](mvc-music-store-part-5/_static/image12.png)

Szablon Usuń pokazuje wszystkie pola dla modelu, ale firma Microsoft może uprościć tego down sobą. Zmień kod widoku w /Views/StoreManager/Delete.cshtml do następującego.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Spowoduje to wyświetlenie uproszczony potwierdzenie usunięcia.

![](mvc-music-store-part-5/_static/image13.png)

Kliknięcie przycisku Usuń powoduje, że formularza do ponownego zaksięgowania serwera, który wykonuje akcję DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Nasze akcji POST protokołu HTTP do kontrolera usunąć wykonuje następujące akcje:

- 1. Ładuje albumu według Identyfikatora
- 2. Usuwa album i Zapisz zmiany
- 3. Przekierowuje do indeksu, pokazujący, że Album został usunięty z listy

Aby to sprawdzić, uruchom aplikację i przejdź do /StoreManager. Wybierz album z listy i kliknij łącze Usuń.

![](mvc-music-store-part-5/_static/image14.png)

Zostanie wyświetlony ekran potwierdzenia naszych Delete.

![](mvc-music-store-part-5/_static/image15.png)

Kliknięcie przycisku Usuń Usuwa album i zwraca nam do strony indeksu Menedżera magazynu zawiera album został usunięty.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Przy użyciu niestandardowych pomocnika kodu HTML do obcięcia tekstu

Mamy jeden potencjalny problem z naszą stronę indeks Menedżera magazynu. Nasze właściwości albumu tytułu i nazwy wykonawcy zarówno można wystarczająco długie, że może wywoływać poza naszych formatowanie tabeli. Utworzymy niestandardowych pomocnika kodu HTML, aby umożliwić nam łatwo obcięcia tych i innych właściwości w naszym widoków.

![](mvc-music-store-part-5/_static/image17.png)

W razor @helper składni wprowadził bardzo łatwo tworzyć własne funkcje pomocnicze do użycia w widoków. Otwórz widok /Views/StoreManager/Index.cshtml i Dodaj następujący kod bezpośrednio po @model wiersza.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Ta metoda pomocnika przyjmuje ciągu i maksymalną długość, aby umożliwić. Jeśli podany tekst jest krótszy niż określona długość, pomocnika wyświetla go w formie — jest. Jeśli jest on dłuższy, następnie je obcina tekst i renderuje "..." dla pozostałych.

Teraz możemy użyć naszych Truncate pomocnika do zapewnienia, że tytuł i nazwy wykonawcy właściwości są mniej niż 25 znaków. Pełny widok przy użyciu naszego nowego pomocnika Truncate pojawia się poniżej.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Teraz przeglądających /StoreManager/ adres URL, albumów i tytuły są przechowywane poniżej naszych maksymalną długość.

![](mvc-music-store-part-5/_static/image18.png)

Uwaga: Ten pokazuje simple case tworzenia i przy użyciu pomocnika, w jednym widoku. Aby dowiedzieć się więcej na temat tworzenia wątków, które są dostępne w całej lokacji, zapoznaj się z mojej blogu: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


>[!div class="step-by-step"]
[Poprzednie](mvc-music-store-part-4.md)
[dalej](mvc-music-store-part-6.md)
