---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Część 3: Widoki i modele widoków | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 3 obejmuje, widoki i modele widoków.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 8fd89c2a448877bf13a7828f545ffcd400f63bb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837413"
---
<a name="part-3-views-and-viewmodels"></a>Część 3: Widoki i modele widoków
====================
przez [Galloway'em Jon](https://github.com/jongalloway)

> MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.  
>   
> MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 3 obejmuje, widoki i modele widoków.


Do tej pory firma Microsoft została właśnie zostało zwracanie ciągi z akcji kontrolera. Jest to dobre rozwiązanie sposób poznać działanie kontrolerów, ale to, że nie będzie sposób tworzenia aplikacji sieci web rzeczywistych. Firma Microsoft zamierza mają lepszy sposób generują kod HTML do przeglądarki, odwiedź naszą witrynę — jeden w którym firma Microsoft może korzystać z plików szablonu można łatwo dostosować zawartość HTML przesyła z powrotem. To dokładnie wykonaj widoków.

## <a name="adding-a-view-template"></a>Dodawanie szablonu widoku

Aby użyć szablonu widoku, firma Microsoft będzie zmienić metodę indeksu HomeController zwracać element ActionResult i jego zwracają View(), takich jak poniżej:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

Powyższych zmian wskazuje na to, a nie zwrócił ciągu, chcemy zamiast tego użyj "View", aby wygenerować ponownie wynik.

Teraz dodamy odpowiedni szablon widoku nasze projektu. W tym celu firma Microsoft będzie umieść kursor tekstu w metodzie akcji indeksu, a następnie kliknij prawym przyciskiem myszy i wybierz pozycję "Dodaj widok". Zostanie wyświetlone okno dialogowe dodawania widoku:

![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)

Okno dialogowe "Dodaj widok" pozwala szybko i łatwo wygenerować plik szablonu widoku. Domyślnie "Dodaj widok" okno dialogowe wstępnie wypełnia nazwę Wyświetl szablon do tworzenia, tak aby była zgodna z metody akcji, która będzie go używać. Ponieważ użyliśmy menu kontekstowego "Dodaj widok" w ramach metody akcji indeks() naszych HomeController, okno dialogowe "View Dodaj" powyżej ma "Index" jako nazwy widoku wstępnie wypełnione domyślnie. Nie potrzebujemy zmienić dowolne z opcjami w tym oknie dialogowym, więc kliknij przycisk Dodaj.

Gdy firma Microsoft kliknij przycisk Dodaj, Visual Web Developer utworzy nowy Index.cshtml, Wyświetl szablon dla nas w katalogu \Views\Home tworzenia folderu, jeśli jeszcze nie istnieje.

![](mvc-music-store-part-3/_static/image2.png)

Nazwy i lokalizacji folderu pliku "Index.cshtml" ważne jest i jest zgodna z konwencjami nazewnictwa platformy ASP.NET MVC domyślne. Nazwa katalogu, \Views\Home, odpowiada kontroler - o nazwie HomeController. Nazwa szablonu widoku indeksu, pasuje do metody akcji kontrolera, które będą wyświetlane w widoku.

ASP.NET MVC pozwala uniknąć konieczności jawnego określania nazwy lub lokalizacji szablonu widoku, gdy używamy tę konwencję nazewnictwa w celu zwrócenia widoku. Renderowanie zostanie domyślnie przeprowadzone szablon widoku \Views\Home\Index.cshtml podczas pisania kodu, takich jak poniżej w ramach naszych HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer utworzony i otwarty "Index.cshtml" Wyświetl szablon, po możemy kliknięto przycisk "Dodaj" w oknie dialogowym "Dodaj widok". Poniżej przedstawiono zawartość Index.cshtml.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Ten widok jest przy użyciu składni Razor, który jest bardziej zwięzłe niż aparat widoku formularzy sieci Web, które są używane w formularzach sieci Web platformy ASP.NET i wcześniejszych wersjach programu ASP.NET MVC. Aparat widoku w formularzach sieci Web jest nadal dostępny w programie ASP.NET MVC 3, ale wielu programistów uważa, aparat widoku Razor bardzo dobrze pasuje rozwoju platformy ASP.NET MVC.

Pierwsze trzy wiersze Ustaw tytuł strony, przy użyciu ViewBag.Title. Firma Microsoft będzie wyglądać jak to działa bardziej szczegółowo wkrótce, ale najpierw Przyjrzyjmy aktualizacji tekst nagłówka tekst i wyświetlić stronę. Aktualizacja &lt;h2&gt; tagu "ten to Home Page" powiedzieć, jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Uruchamianie aplikacji pokazuje, że nasz nowy tekst jest widoczna na stronie głównej.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Przy użyciu układu dla wspólnych elementów witryny

Większość witryn sieci Web mają zawartość, która jest współużytkowana przez wiele stron: nawigacji stopkach stron, obrazów logo, odwołania do arkusza stylów, itp. Aparat widoku Razor sprawia, że jest to łatwa w zarządzaniu przy użyciu strony o nazwie \_Layout.cshtml, który został automatycznie utworzony dla nas znajdujące się w folderze/widoków/Shared.

![](mvc-music-store-part-3/_static/image4.png)

Kliknij dwukrotnie ten folder, aby wyświetlić jego zawartość, poniżej.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Zawartość z naszych pojedyncze widoki, które będą wyświetlane przez @RenderBodypolecenia () oraz typowe zawartość, która ma znajdować się poza, można dodać do \_Layout.cshtml znaczników. Chcemy, aby nasze Store utworów muzycznych MVC aby wspólnej nagłówek wraz z łączami do naszej strony głównej i Store obszaru na wszystkich stronach w witrynie, dlatego dodamy, szablon bezpośrednio powyżej @RenderBodyinstrukcji ().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Aktualizowanie arkusza stylów

Szablonu pusty projekt zawiera bardzo usprawnione pliku CSS, która zawiera tylko style, które umożliwia wyświetlenie komunikatów dotyczących sprawdzania poprawności. Nasze projektanta udostępnił niektóre dodatkowe CSS i obrazów do definiowania wygląd i działanie dla naszej witrynie, dlatego dodamy te teraz.

Zaktualizowany plik CSS i obrazów, które są uwzględnione w zawartości katalogu Assets.zip MvcMusicStore, który znajduje się w temacie [MVC — muzyka-Store](https://github.com/evilDave/MVC-Music-Store). Utworzymy wybierz obie z nich w Eksploratorze Windows i upuścić je do folderu zawartości Nasze rozwiązanie programu Visual Web Developer, jak pokazano poniżej:

![](mvc-music-store-part-3/_static/image5.png)

Zostanie wyświetlony monit o potwierdzenie, czy chcesz zastąpić istniejący plik Site.css. Kliknij przycisk Tak.

![](mvc-music-store-part-3/_static/image6.png)

Folder zawartości Twojej aplikacji będą wyświetlane w następujący sposób:

![](mvc-music-store-part-3/_static/image7.png)

Teraz możemy uruchomić aplikację i zobacz, jak wyglądają nasze zmiany na stronie głównej.

![](mvc-music-store-part-3/_static/image8.png)

- Podsumujmy, co zostało zmienione: metody akcji indeksu HomeController znalezione i wyświetlane \Views\Home\Index.cshtmlView szablonu, mimo że naszego kodu o nazwie "View() zwrotu", ponieważ szablon widoku, a następnie standardowej konwencji nazewnictwa.
- Strona główna wyświetla prosty komunikat powitalny, który jest zdefiniowany w szablonie widoku \Views\Home\Index.cshtml.
- Strona główna używa naszej \_Layout.cshtml szablonu, a zatem komunikat powitalny znajduje się w układzie standardowej witrynie HTML.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Przy użyciu modelu do przekazywania informacji do naszych widoku

Wyświetl szablon, który po prostu wyświetla zapisane na stałe HTML nie będzie bardzo interesujące, witryny sieci web. Aby utworzyć dynamiczne witryny sieci web, będzie zamiast tego chcemy do przekazywania informacji z naszych akcji kontrolera do naszych szablonów widoku.

Wzorzec Model-View-Controller termin, który Model, który odwołuje się do obiektów, zawierające dane w aplikacji. Często obiekty modelu odnoszą się do tabel w bazie danych, ale nie muszą oni.

Metody akcji kontrolera, które zwracają element ActionResult, można przekazać obiekt modelu widoku. Dzięki temu kontroler na klarownie spakowanie wszystkie informacje niezbędne do generowania odpowiedzi, a następnie przekazał tych informacji do szablonu widoku na potrzeby generowania odpowiednich odpowiedzi HTML. Jest to łatwiej zrozumieć, obserwując go w działaniu, więc zaczynajmy.

Najpierw utworzymy niektóre klasy modelu do reprezentowania gatunki i albumów w naszym Sklepie. Zacznijmy od utworzenia klasy gatunku. Kliknij prawym przyciskiem myszy folder "Modele" w obrębie projektu, wybierz opcję "Dodaj klasę" i nazwij plik "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Następnie dodaj publicznego ciągu nazwy właściwości do klasy, która została utworzona:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Uwaga: W przypadku zastanawiasz się {Pobierz; Ustaw;} osiągnął notacji korzystanie z języka C# na automatycznie implementowane właściwości funkcji. To daje nam korzyści wynikające z właściwością bez konieczności NAS zadeklarować pole zapasowe.*

Następnie wykonaj te same kroki, aby utworzyć klasę albumu (o nazwie Album.cs), która ma tytuł i właściwości gatunku:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Teraz możemy zmodyfikować StoreController używać widoków, wyświetlające informacji dynamicznych z nasz Model. Jeśli — dla celów demonstracyjnych teraz — firma Microsoft o nazwie naszych albumów na podstawie Identyfikatora żądania, firma Microsoft można wyświetlić te informacje, tak jak w poniższym widoku.

![](mvc-music-store-part-3/_static/image10.png)

Zaczniemy, zmieniając akcji Store Details, aby pokazywała informacje dotyczące jednego albumu. Dodaj instrukcję "using" na początku **StoreControllers** klasy, aby uwzględnić przestrzeń nazw MvcMusicStore.Models, dzięki czemu nie trzeba wpisać MvcMusicStore.Models.Album za każdym razem, gdy chcemy użyć klasy albumu. Sekcja "dyrektywy Using" tej klasy powinien teraz wyglądać jak poniżej.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Następnie zaktualizujemy szczegóły akcji kontrolera, aby funkcja zwraca element ActionResult, a nie w ciągu, ile My mieliśmy metodą indeksu HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Teraz możemy zmodyfikować logiki, aby zwrócić obiekt albumu do widoku. W dalszej części tego samouczka firma Microsoft będzie można pobierania danych z bazy danych — ale dla obecnie firma Microsoft użyje "fikcyjny dane" Aby rozpocząć pracę.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Uwaga: Jeżeli jesteś zaznajomiony z C#, możesz może przyjąć, że za pomocą var oznacza, że nasza zmienna album z późnym wiązaniem. Nie jest prawidłowe — kompilator języka C# używa wnioskowanie typów w oparciu o co możemy przypisanie do zmiennej w celu określenia tej album jest typu albumu i kompilowanie zmiennej lokalnej albumu jako typu fotograficzne, dzięki czemu możemy sprawdzanie w czasie kompilacji i Edytor kodu programu Visual Studio Pomoc techniczna.*

Teraz Utwórz szablon widoku, który używa naszej albumu do generowania odpowiedzi HTML. Zanim do tego należy skompilować projekt, aby poinformować okno dialogowe dodawania widoku o nasze nowo utworzonej klasie albumu. Można tworzyć projekt, wybierając Debug⇨Build MvcMusicStore elementu menu (jako dodatkowe ćwiczenie można Użyj skrótu Ctrl-Shift-B do skompilowania projektu).

![](mvc-music-store-part-3/_static/image11.png)

Skoro już skonfigurowaliśmy naszych klasy pomocnicze jesteśmy gotowi do tworzenia szablonu widoku. Kliknij prawym przyciskiem myszy wewnątrz metody szczegółowe informacje, a następnie wybierz pozycję "Dodaj widok..." z menu kontekstowego.

![](mvc-music-store-part-3/_static/image12.png)

Zamierzamy utworzyć nowy szablon widoku jak zrobiliśmy przed z HomeController. Ponieważ tworzymy z StoreController go zostanie domyślnie wygenerowany plik \Views\Store\Index.cshtml.

W przeciwieństwie do wcześniej, użyjemy zaznacz pole wyboru "Utwórz silnie typizowanego" widoku. Następnie użyjemy wybierz nasze klasę "Albumu" w ciągu "Widok danych class" listy downlist. Spowoduje to okno dialogowe "Dodaj widok", aby utworzyć szablon widoku, który oczekuje, że albumu obiektów, które zostaną przekazane do go do użycia.

![](mvc-music-store-part-3/_static/image13.png)

Po kliknięciu przycisku "Dodaj" nasze \Views\Store\Details.cshtml Wyświetl szablon zostanie utworzony, zawierający poniższy kod.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Zwróć uwagę, pierwszego wiersza, co oznacza, że ten widok jest silnie typizowaną do klasy Nasze albumu. Aparat widoku Razor rozumie, że go został przekazany obiekt albumu, firma Microsoft można łatwo uzyskiwać dostęp do właściwości modelu oraz nawet korzystać z zalet technologii IntelliSense w edytorze programu Visual Web Developer.

Aktualizacja &lt;h2&gt; tag, aby wyświetlała właściwości Title fotograficzne, modyfikując ten wiersz, aby wyglądają następująco.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Należy zauważyć, że funkcja IntelliSense jest wyzwalany w przypadku Podaj okres, po @Model — słowo kluczowe, wyświetlanie właściwości i metody, które obsługuje klasa albumu.

Załóżmy teraz ponownego uruchomienia naszych projektu i skorzystaj z 5-Store/szczegóły adresu URL. Zobaczymy Szczegóły albumu podobnie jak poniżej.

![](mvc-music-store-part-3/_static/image14.png)

Teraz wprowadzimy podobne aktualizacji dla metody akcji przeglądania Store. Zaktualizuj metodę, tak aby zwracało poprawnie element ActionResult, a następnie zmodyfikuj logikę metody, więc tworzy nowy obiekt gatunku i zwraca go do widoku.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Kliknij prawym przyciskiem myszy w metodzie przeglądania i wybierz pozycję "Dodaj widok..." z menu kontekstowego, Dodaj widok, który jest silnie typizowane Dodaj silnie typizowaną do klasy gatunku.

![](mvc-music-store-part-3/_static/image15.png)

Aktualizacja &lt;h2&gt; elementu w widoku kodu (w /Views/Store/Browse.cshtml) do wyświetlania informacji gatunku.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Teraz możemy ponownie uruchomić projekcie i przejdź do/Store/przeglądania? Gatunku = Disco adres URL. Firma Microsoft zostanie wyświetlona strona przeglądania, wyświetlana, jak poniżej.

![](mvc-music-store-part-3/_static/image16.png)

Na koniec upewnijmy się nieco bardziej skomplikowaną aktualizacji **Store indeksu** metody akcji i widok, aby wyświetlić listę wszystkich gatunki w naszym Sklepie. Możemy to zrobić za pomocą listy gatunki jako naszych obiektu modelu, a nie tylko jednego rodzaju.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Kliknij prawym przyciskiem myszy w metodzie akcji indeksu Store i wybierz pozycję Dodaj widok, wybierz gatunku jako klasa modelu, a następnie naciśnij przycisk Dodaj.

![](mvc-music-store-part-3/_static/image17.png)

Pierwsze zmienimy @model deklaracji, aby wskazać, czy widok będzie oczekiwano gatunku kilka obiektów zamiast tylko jeden. Zmiana w pierwszym wierszu /Store/Index.cshtml do odczytu w następujący sposób:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Oznacza to, że będzie on pracować z obiektu modelu, który może zawierać kilka obiektów gatunku aparatu widoku Razor. Firma Microsoft korzysta z elementu IEnumerable&lt;gatunku&gt; zamiast listy&lt;gatunku&gt; ponieważ jest bardziej ogólnym, może zmienić później naszych typ modelu do dowolnego typu obiektu, który obsługuje interfejsu IEnumerable.

Następnie firma Microsoft będzie pętli gatunku obiekty w modelu, jak pokazano w poniższym kodzie ukończone.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Zwróć uwagę, że mamy już pełną obsługą technologii IntelliSense możemy wprowadź ten kod tak, że gdy wpiszesz "@Model." widzimy, wszystkie metody i właściwości obsługiwanych przez elementu IEnumerable typu gatunku.

![](mvc-music-store-part-3/_static/image18.png)

W ramach naszych pętlę "foreach" Visual Web Developer wie, że każdy element jest typu gatunku, więc widzimy IntelliSence dla każdego typu gatunku.

![](mvc-music-store-part-3/_static/image19.png)

Następnie funkcja tworzenia szkieletu zbadane obiektu gatunku i określić, że będzie mieć właściwości Name, więc w pętli i zapisuje je w. Generuje on również linki Edytuj, szczegóły i Delete do poszczególnych elementów. Firma Microsoft będzie zalet, w dalszej części nasz Menedżer magazynu, ale teraz chcemy mieć zamiast prostej listy.

Gdy firma Microsoft może uruchomić aplikację, a następnie przejdź do/Store, zobaczymy, liczby i lista gatunki jest wyświetlana.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Dodawanie łączy między stronami

Adresu URL/Store obecnie zawierającego gatunki Wyświetla nazwy gatunku, po prostu jako zwykły tekst. Wybierzmy tak, aby zamiast zwykłego tekstu mamy zamiast tego gatunku nazwy link na odpowiedni adres URL Store/przeglądania, tak aby kliknięcie określonego rodzaju utworów muzycznych, takie jak "Najdywania" spowoduje przejście do/Store/przeglądania? gatunku = Najdywania adres URL. Firma Microsoft można zaktualizować szablon widoku \Views\Store\Index.cshtml te linki przy użyciu kodu takie jak poniżej danych wyjściowych **(nie należy umieszczać w — teraz zamierzamy je poprawić)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

To działa, ale może prowadzić do problemów później, ponieważ opiera się na ciąg zapisane na stałe. Na przykład jeśli chcemy Zmień nazwę kontrolera, czy musimy przeszukiwać naszego kodu szuka łączy, które muszą zostać zaktualizowane.

Alternatywne podejście, które możemy użyć jest korzystanie z zalet metodę pomocnika kodu HTML. Platforma ASP.NET MVC zawiera metody pomocnika kodu HTML, które są dostępne z naszego kodu szablonu widoku do wykonywania różnych zadań, tak jak to. Metoda pomocnika Html.ActionLink() jest szczególnie przydatne i ułatwia tworzenie HTML &lt;&gt; łączy i dba o irytujące szczegóły, takie jak zagwarantowanie, że ścieżki URL są poprawnie zakodowane w adresie URL.

Html.ActionLink() ma kilka przeciążeń różnych, aby umożliwić określenie tylu informacji potrzebnych dla łącza. W najprostszym przypadku podasz, po prostu tekst łącza, a także metoda akcji nastąpić przejście po kliknięciu hiperlinku na komputerze klienckim. Na przykład można przejść do "/ Store /" Metoda indeks() na stronie szczegółów Store tekstem link "Przejdź do Store Index" przy użyciu następującego wywołania:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Uwaga: W tym przypadku nie należy określać nazwy kontrolera, ponieważ możemy po prostu ustanawiania innej akcji w obrębie tego samego kontrolera, który jest renderowanie bieżący widok.*

Nasze łącza do strony Przegląd należy przekazać parametr, jednak dlatego użyjemy innego przeciążenia metody Html.ActionLink, która przyjmuje trzy parametry:

- 1. Tekst łącza, co spowoduje wyświetlenie nazwy gatunku
- 2. Nazwa akcji kontrolera (Przeglądaj)
- 3. Wartości parametrów trasy, określając nazwę (gatunku) i wartości (nazwa gatunku)

Umieszczanie, że wszystko ze sobą, Oto jak będziemy pisać tych łączy do widoku indeksu Store:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Teraz możemy ponownie uruchom naszych projektów i uzyskać dostępu do adresu URL /Store/ firma Microsoft zostanie wyświetlona lista gatunki. Każdego gatunku jest hiperłącze — po kliknięciu potrwa nam naszym/Store/przeglądania? gatunku =*[gatunek]* adresu URL.

![](mvc-music-store-part-3/_static/image3.jpg)

Kod HTML dla listy gatunku wygląda następująco:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-2.md)
> [dalej](mvc-music-store-part-4.md)
