---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Część 3: Widoki i ViewModels | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 3 zawiera widoki ViewModels.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 497c2898db2e03b0650982c3ad1e6b5ac5ca639d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-3-views-and-viewmodels"></a>Część 3: Widoki i ViewModels
====================
przez [Galloway Jan](https://github.com/jongalloway)

> Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.  
>   
> Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 3 zawiera widoki ViewModels.


Do tej pory możemy został właśnie zostały zwracanie ciągów z akcji kontrolera. To dobry sposób, aby uzyskać informacje o tym, jak działają kontrolery, ale jest nie będzie sposób tworzenia aplikacji sieci web rzeczywistych. Zamierzamy mają lepszym sposobem generują kod HTML do przeglądarek odwiedzający naszej witrynie — co gdzie możemy użyć plików szablonów można łatwo dostosować zawartość HTML odesłania. To dokładnie czy widoków.

## <a name="adding-a-view-template"></a>Dodawanie szablonu widoku

Aby użyć szablonu widoku, firma Microsoft będzie zmienić metodę HomeController indeks, aby zwracać element ActionResult, a jego zwracać View(), podobnie jak poniżej:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

Zmiany powyżej wskazuje, a nie zwrócił ciągu, chcemy zamiast tego użyj "Widok" wynik ponownie wygenerować.

Teraz dodamy odpowiedni szablon widoku do naszej projektu. W tym celu firma Microsoft będzie umieść kursor tekstu w metodzie akcji indeksu, a następnie kliknij prawym przyciskiem myszy i wybierz polecenie "Dodaj widok". Zostanie wyświetlone okno dialogowe dodawania widoku:

![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)

Okno dialogowe "Dodaj widok" pozwala szybko i łatwo wygenerować pliki szablonów widoku. Domyślnie "Dodaj widok" okna dialogowego wstępnie wypełnia nazwę szablonu widoku do tworzenia, tak aby był zgodny z metody akcji, która będzie go używać. Ponieważ użyliśmy menu kontekstowe "Dodaj widok" w metodzie akcji indeks() naszych HomeController powyżej okna dialogowego "Dodaj widok" ma "Index" jako nazwy widoku wstępne wypełnianie domyślnie. Nie należy zmienić opcje w tym oknie dialogowym, więc kliknij przycisk Dodaj.

Kliknięcie przycisku Dodaj, Visual Web Developer utworzy nowy Index.cshtml, Wyświetl szablon firmie Microsoft w katalogu \Views\Home tworzenia folderu, jeśli jeszcze nie istnieje.

![](mvc-music-store-part-3/_static/image2.png)

Nazwy i lokalizacji folderu pliku "Index.cshtml" jest ważna i następuje domyślnych konwencji nazewnictwa platformy ASP.NET MVC. Nazwa katalogu, \Views\Home, odpowiada kontroler - o nazwie HomeController. Nazwa szablonu widoku indeksu, odpowiada metoda akcji kontrolera, które będą wyświetlane w widoku.

ASP.NET MVC pozwala uniknąć konieczności jawnego określania nazwy lub lokalizacji szablonu widoku, gdy jest używany do zwrócenia widoku tę konwencję nazewnictwa. Go będzie domyślnie renderowania szablonu widoku \Views\Home\Index.cshtml pisząc poniższy kod podobnie jak w naszych HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer utworzony i otworzyć szablon widoku "Index.cshtml", po możemy kliknięty przycisk "Dodaj" w oknie dialogowym "Dodaj widok". Poniżej przedstawiono zawartość Index.cshtml.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Ten widok jest przy użyciu składni Razor, które jest bardziej zwięzłe niż aparat widoku formularzy sieci Web, które są używane w poprzednich wersjach programu ASP.NET MVC i formularzy sieci Web ASP.NET. Aparat widoku formularzy sieci Web są nadal dostępne w programie ASP.NET MVC 3, ale wielu deweloperów znaleźć aparatu widoku Razor naprawdę również pasujące rozwoju platformy ASP.NET MVC.

Pierwsze trzy wiersze ustawić przy użyciu ViewBag.Title tytuł strony. Firma Microsoft będzie Sprawdź jak to działa bardziej szczegółowo wkrótce, ale najpierw umożliwia aktualizacji tekst nagłówka i Wyświetl stronę. Aktualizacja &lt;h2&gt; tag znaczy "ten jest strona główna", jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Uruchomiona pokazuje aplikacji, że nasze nowy tekst jest widoczna na stronie głównej.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Przy użyciu układu dla wspólnych elementów lokacji

Większość witryn sieci Web ma zawartości, które są współużytkowane przez wiele stron: nawigacji stopki, obrazy logo, odwołania do arkusza stylów, itp. Aparat widoku Razor ułatwia to zarządzanie przy użyciu strony o nazwie \_Layout.cshtml, który automatycznie została utworzona dla nas znajdujące się w folderze/widoków/Shared.

![](mvc-music-store-part-3/_static/image4.png)

Kliknij dwukrotnie ten folder, aby wyświetlić zawartość, które są wyświetlane poniżej.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Zawartość z naszych poszczególnych widoków będą wyświetlane przez @RenderBodypolecenia () oraz typowe zawartość, która chcemy się pojawiają się poza tym, które można dodać do \_Layout.cshtml znaczników. Chcemy naszego sklepu utworów muzycznych MVC aby wspólnej nagłówek z łączami do naszej obszar strony głównej i zapisać na wszystkich stron w witrynie, więc dodamy który do szablonu bezpośrednio powyżej @RenderBodyinstrukcji ().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Aktualizowanie arkusza stylów

Pusty szablon projektu zawiera plik CSS bardzo prostsze, zawierający tylko stylów używany do wyświetlania komunikatów dotyczących sprawdzania poprawności. Nasze projektanta udostępnił niektóre dodatkowe CSS i obrazy do definiowania wyglądu i działania dla witryny, więc dodamy znajdującymi się teraz.

Zaktualizowany plik CSS i obrazy znajdują się w katalogu zawartości Assets.zip MvcMusicStore, która jest dostępna w [MVC utworów muzycznych magazynu](https://github.com/evilDave/MVC-Music-Store). Firma Microsoft zostaną wybrane oba w Eksploratorze Windows i upuść je do folderu zawartości naszych rozwiązań programu Visual Web Developer, jak pokazano poniżej:

![](mvc-music-store-part-3/_static/image5.png)

Zostanie wyświetlony monit o potwierdzenie, czy chcesz zastąpić istniejący plik Site.css. Kliknij przycisk Tak.

![](mvc-music-store-part-3/_static/image6.png)

Folder zawartości aplikacji pojawi się w następujący sposób:

![](mvc-music-store-part-3/_static/image7.png)

Teraz załóżmy Uruchom aplikację i sprawdzić wygląd naszego zmiany na stronie głównej.

![](mvc-music-store-part-3/_static/image8.png)

- Umożliwia przeglądanie, co zostało zmienione: metody akcji indeksu HomeController znaleziono i wyświetlane szablonu \Views\Home\Index.cshtmlView, mimo że naszego kodu nazywany "return View()", ponieważ nasze Wyświetl szablon, a następnie standardowej konwencji nazewnictwa.
- Strona główna wyświetla prosty komunikat powitalny, który jest zdefiniowany w szablonie \Views\Home\Index.cshtml widoku.
- Używa strony głównej naszych \_Layout.cshtml szablonu, co komunikat powitalny jest zawarty w układu standardowe witryny HTML.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Przy użyciu modelu do przekazywania informacji do naszej widoku

Szablon widoku, który wyświetla tylko zapisane na stałe HTML nie będzie bardzo interesujące witryny sieci web. Do tworzenia dynamicznych witryn sieci web, będzie zamiast tego chcemy do przekazywania informacji z naszych akcji kontrolera naszym szablony widoku.

We wzorcu Model-View-Controller określenie modelu odwołuje się do obiektów, które reprezentują danych w aplikacji. Często obiekty modelu odpowiadają tabel bazy danych, ale nie muszą oni.

Metody akcji kontrolera, zwracające element ActionResult można przekazać obiekt modelu do widoku. Dzięki temu kontroler testu pakietu wszystkie informacje potrzebne do generowania odpowiedzi, a następnie przekazać te informacje do szablonu widoku używany do generowania odpowiednich odpowiedzi HTML. To jest łatwiej zrozumieć, sprawdzając w akcji, więc Zacznijmy od początku.

Najpierw utworzymy niektóre klasy modelu do reprezentowania Genres i albumów w naszym magazynie. Zacznijmy od utworzenia klasy rodzaju. Kliknij prawym przyciskiem myszy folder "Modele" w projekcie, wybierz opcję "Dodaj klasę" i nazwę pliku "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Następnie dodaj ciąg publicznego nazwa właściwości do klasy, która została utworzona:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Uwaga: W przypadku, że zastanawiasz się {get; Ustaw;} notacji jest wprowadzenie funkcji C# dla właściwości zaimplementowane automatycznie. Dzięki temu nam korzyści wynikające z właściwością bez konieczności nam zadeklarować polem zapasowym.*

Następnie wykonaj te same czynności, aby utworzyć klasę albumu (o nazwie Album.cs), tytuł i właściwości Genre:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Teraz możemy zmodyfikować StoreController, aby móc używać widoków, które zawierają informacje o dynamicznej z modelu. Jeśli - dla celów demonstracyjnych teraz — nasze konto nazywa się naszego albumów na podstawie Identyfikatora żądania, można wyświetlić te informacje, jak w poniższym widoku.

![](mvc-music-store-part-3/_static/image10.png)

Zaczniemy przez zmianę akcji szczegóły magazynu, dzięki czemu zawiera informacje dotyczące jednego albumu. Dodaj instrukcję "przy użyciu" na początku **StoreControllers** klasy do uwzględnienia MvcMusicStore.Models przestrzeni nazw, dlatego nie należy do typu MvcMusicStore.Models.Album za każdym razem, gdy chcemy użyć klasy albumu. "Deklaracje Using" części tej klasy, powinien zostać wyświetlony jak poniżej.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Następnie będziemy informować szczegóły akcji kontrolera, aby zwraca element ActionResult, a nie typu string, jak robiliśmy metodą HomeController indeksu.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Teraz możemy zmodyfikować logiki, aby powrócić do widoku obiektu albumu. W dalszej części tego samouczka firma Microsoft będzie można pobierania danych z bazy danych — ale dla teraz używamy "fikcyjny danych" Aby rozpocząć pracę.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Uwaga: Jeśli znasz C#, możesz może założyć, że przy użyciu var oznacza, że nasze zmiennej albumu późnym wiązaniem. To nie jest prawidłowe — kompilatora C# używa wnioskowanie na podstawie co możemy jest przypisanie do zmiennej do określenia tej album jest typu albumu i kompilowanie zmiennej lokalnej albumu jako typ albumy, uzyskujemy sprawdzanie kompilacji i edytora kodu programu Visual Studio Obsługa.*

Teraz Utwórzmy szablonu widoku, który korzysta z naszych albumu do generowania odpowiedzi HTML. Zanim przejdziemy należy skompilować projekt, dzięki czemu okno dialogowe dodawania widoku zna naszych nowo utworzonej klasy albumu. Można utworzyć projekt, wybierając Debug⇨Build MvcMusicStore elementu menu (dla dodatkowe środki, można użyć skrótu Ctrl-Shift-B Aby skompilować projekt).

![](mvc-music-store-part-3/_static/image11.png)

Teraz, gdy skonfigurowaliśmy naszej klasy obsługi już wszystko gotowe do tworzenia szablonu naszych widoku. Kliknij prawym przyciskiem myszy w metodzie szczegółowe informacje i wybierz opcję "Dodaj widok..." z menu kontekstowego.

![](mvc-music-store-part-3/_static/image12.png)

Zamierzamy utworzyć nowy szablon widoku jak robiliśmy przed z HomeController. Ponieważ firma Microsoft tworzysz z StoreController go domyślnie generowania pliku \Views\Store\Index.cshtml.

W odróżnieniu od przed, zamierzamy wyboru "Utwórz silnie typizowanego" w widoku. Następnie zamierzamy Wybierz klasy Nasze "Album" w "Klasy danych widoku" upuszczania downlist. To spowoduje, że okno dialogowe "Dodaj widok", aby utworzyć szablon widoku, który oczekuje, że Album obiektu zostaną przekazane do go do użycia.

![](mvc-music-store-part-3/_static/image13.png)

Po kliknięciu przycisku przycisk "Dodaj" naszych \Views\Store\Details.cshtml widok będzie można utworzyć szablonu, zawierający następujący kod.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Zwróć uwagę, pierwszy wiersz, co oznacza, że widok jest silnie typizowaną do klasy Nasze albumu. Aparat widoku Razor rozumie, że jej został przekazany obiekt albumu możemy łatwo uzyskiwać dostęp do właściwości modelu, a nawet są korzyści IntelliSense w edytorze programu Visual Web Developer.

Aktualizacja &lt;h2&gt; tagów wyświetla przez zmodyfikowanie tego wiersza, aby wyglądają następująco albumu właściwości Title więc.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Zwróć uwagę, że IntelliSense jest wyzwalane po wprowadzeniu okres po @Model — słowo kluczowe, przedstawiający właściwości i metody, które obsługuje klasy albumu.

Umożliwia teraz ponownie uruchomić naszych projektu i odwiedź adres URL Sklepu/szczegóły/5. Firma Microsoft zostanie wyświetlone szczegóły albumu podobnie jak poniżej.

![](mvc-music-store-part-3/_static/image14.png)

Teraz wybierzemy podobne aktualizacji dla metody akcji Przeglądaj magazynu. Zaktualizuj metodę, tak aby zwracało element ActionResult i zmodyfikować logikę metody, aby tworzy nowy obiekt Genre i zwraca go do widoku.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Kliknij prawym przyciskiem myszy w metodzie Przeglądaj i wybierz opcję "Dodaj widok..." z menu kontekstowego, a następnie Dodaj widok, który jest jednoznacznie Dodaj silnie typizowaną do klasy Genre.

![](mvc-music-store-part-3/_static/image15.png)

Aktualizacja &lt;h2&gt; elementu w widoku kodu (w /Views/Store/Browse.cshtml) do wyświetlenia informacji o rodzaju.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Teraz załóżmy Uruchom ponownie naszych projektu i przejdź do/magazynu/Przeglądaj? Genre = Disco adresu URL. Firma Microsoft będzie wyświetlona strona przeglądania wyświetlony podobnie jak poniżej.

![](mvc-music-store-part-3/_static/image16.png)

Na koniec upewnijmy nieco bardziej skomplikowane aktualizacji do **przechowywania indeksu** metody akcji i widok, aby wyświetlić listę wszystkich gatunkami muzyki w naszym magazynie. Firma Microsoft to zrobić przy użyciu listy Genres naszych obiektu modelu, a nie tylko jednego rodzaju.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Kliknij prawym przyciskiem myszy w metodzie akcji indeksu magazynu i wybierz opcję Dodaj widok, wybierz Genre jako klasa modelu, a następnie naciśnij przycisk Dodaj.

![](mvc-music-store-part-3/_static/image17.png)

Najpierw zmienimy @model deklaracji, aby określić, czy widok będzie oczekiwano Genre kilka obiektów zamiast jeden z nich. Zmiana w pierwszym wierszu /Store/Index.cshtml do odczytu w następujący sposób:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Ta wartość informuje aparatu widoku Razor, który będzie działać z obiektu modelu, który może posiadać kilka obiektów rodzaju. Firma Microsoft korzysta z interfejsu IEnumerable&lt;Genre&gt; zamiast listy&lt;Genre&gt; ponieważ jest bardziej ogólnym, pozwalając nam można zmienić później naszych typ modelu do dowolnego typu obiektu, który obsługuje interfejsu IEnumerable.

Następnie będzie Pętla przez obiekty Genre w modelu, jak pokazano w poniższym kodzie ukończone.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Powiadomienie, że mamy pełną obsługę funkcji IntelliSense jak możemy wprowadź ten kod tak, aby podczas wpisywania możemy "@Model." przedstawia wszystkie metody i właściwości obsługiwane przez interfejs IEnumerable typu rodzaju.

![](mvc-music-store-part-3/_static/image18.png)

W naszym pętli "foreach" Visual Web Developer wie, że każdy element jest typu Genre, tak widzimy IntelliSence dla każdego typu Genre.

![](mvc-music-store-part-3/_static/image19.png)

Następnie funkcja szkieletów zbadane obiektu Genre i określić, że będzie mieć właściwości Name, pętli i dokonuje zapisu. Generuje on edycji, szczegóły i Usuń linki do każdego elementu. Firma Microsoft będzie korzystać tego później w naszym Menedżer magazynu, ale teraz chcemy mieć prostą listę zamiast tego.

Gdy firma Microsoft może uruchomić aplikację, a następnie przejdź do/Store, widzimy liczbę i listy gatunkami muzyki jest wyświetlany.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Dodawanie łączy między stronami

Nasze adresu URL/Store obecnie wymieniono Genres lista nazw Genre po prostu jako zwykły tekst. Zmieńmy to tak, aby zamiast zwykłego tekstu mamy zamiast tego rodzaju nazwy link odpowiedni adres URL Sklepu/Przeglądaj, dzięki czemu kliknięcie określonego rodzaju muzyki, takie jak "Disco" przejdzie do/magazynu/Przeglądaj? genre = Disco adresu URL. Firma Microsoft można zaktualizować szablon widoku naszych \Views\Store\Index.cshtml te linki przy użyciu kodu podobnie jak poniżej w danych wyjściowych **(nie wpisz to w — chcemy poprawić na nim)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Działający, ale połączenie może prowadzić do problemów później, ponieważ zależy od ciągu zapisane na stałe. Na przykład jeśli trzeba zmienić nazwę kontrolera, czy musimy wyszukiwania za pomocą naszego kodu wyszukiwanie linki, które muszą zostać zaktualizowane.

Alternatywne podejście, które możemy użyć ma korzystać z metody pomocnika kodu HTML. Platforma ASP.NET MVC zawiera metody pomocnika kodu HTML, które są dostępne z naszego kodu szablonu widoku do wykonywania różnych zadań, tak jak to. Metoda pomocnika Html.ActionLink() jest szczególnie przydatne i ułatwia tworzenie HTML &lt;&gt; łączy i zajmuje się irytujące szczegóły, takie jak upewnić ścieżki adresu URL jest poprawnie zakodowane w adresie URL.

Html.ActionLink() ma kilka przeciążeń różnych umożliwiają określenie tylu informacji potrzebnych dla łącza. Ogólnie rzecz biorąc musisz wpisać tylko tekst łącza, a także metoda akcji nastąpić przejście po kliknięciu hiperlinku na kliencie. Na przykład firma Microsoft można połączyć się z "/ magazynu /" indeks() metody na stronie Szczegóły magazynu z tekst łącza "Przejdź do magazynu indeksu" przy użyciu następujące wywołanie:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Uwaga: W tym przypadku nie należy określać nazwy kontrolera, ponieważ firma Microsoft łączona tylko innego działania w ramach tego samego kontrolera, który renderowania bieżącego widoku.*

Nasze łącza do strony przeglądania należy przekazać parametr, jednak dlatego użyjemy innego przeciążenia metody Html.ActionLink, który przyjmuje trzy parametry:

- 1. Tekst łącza, która będzie wyświetlana nazwa Genre
- 2. Nazwa akcji kontrolera (Przeglądaj)
- 3. Wartości parametrów trasy, określając zarówno nazwę (Genre), jak i wartości (nazwa Genre)

Umieszczanie, że wszystkich elementów, Oto jak możemy przedstawiono tworzenie łączy do widoku indeksu magazynu:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Teraz możemy ponownie uruchom naszych projektu i uzyskać dostępu do adresu URL /Store/ możemy zostanie wyświetlona lista gatunkami muzyki. Każdego rodzaju jest hiperłącze — po kliknięciu potrwa nam naszym magazynie/Przeglądaj? genre =*[genre]* adresu URL.

![](mvc-music-store-part-3/_static/image3.jpg)

Kod HTML pod kątem listy genre wygląda następująco:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-2.md)
> [dalej](mvc-music-store-part-4.md)
