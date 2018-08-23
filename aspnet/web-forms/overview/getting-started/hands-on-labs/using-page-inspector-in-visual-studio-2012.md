---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Za pomocą narzędzia Page Inspector w programie Visual Studio 2012 | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym laboratorium praktyczne wykryje nowe narzędzie służące do znajdowania i rozwiązywania problemów strony sieci web w programie Visual Studio — narzędzie Page Inspector. Narzędzie Page Inspector to nowe narzędzie tego b...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: ce654eb5abd54613987f2375cc973febc9dc2ad5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757305"
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Za pomocą narzędzia Page Inspector w programie Visual Studio 2012
====================
Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

> W tym laboratorium praktyczne wykryje nowe narzędzie służące do znajdowania i rozwiązywania problemów strony sieci web w programie Visual Studio — narzędzie Page Inspector.
> 
> Narzędzie Page Inspector to nowe narzędzie wprowadzające narzędzia diagnostyczne przeglądarki do programu Visual Studio i oferujące integrację między przeglądarką, ASP.NET i kod źródłowy. On renderuje stronę sieci web (HTML, formularze sieci Web, ASP.NET MVC lub strony sieci Web) bezpośrednio z poziomu środowiska IDE programu Visual Studio i pozwala na badanie zarówno kod źródłowy, jak i dane wyjściowe. Narzędzie Page Inspector umożliwia łatwą dekompozycję witryny sieci Web, szybko tworząc strony od podstaw i szybkie diagnozowanie problemów.
> 
> Dzisiaj mamy wiele struktur sieci Web tworzyć elastyczne i skalowalne witryny sieci Web w odpowiednim czasie, takie jak ASP.NET MVC i formularzy sieci Web. Z drugiej strony otrzymuje trudniejsze można znaleźć problemy na stronach, ponieważ IDE nie obsługuje Widok projektanta w stronach na podstawie szablonu i zawartości dynamicznej. W związku z tym tych witryn sieci Web muszą być otwarte w przeglądarce, aby zobaczyć, jak pojawiają się z użytkownikiem.
> 
> Deweloperzy sieci Web użyj zewnętrznych narzędzi, aby znaleźć problemy, które regularnie uruchomić w przeglądarce. Następnie wróć do IDE i rozpocząć rozwiązywanie. To i z powrotem działanie środowiska IDE, przeglądarki i narzędzia profilowania może być mało wydajne i czasami wymaga świeże wdrożenie i każdym razem, aby odtworzyć problem czyszczenia pamięci podręcznej.
> 
> Narzędzie Page Inspector wypełnia lukę przy projektowaniu aplikacji internetowych między klientem (Narzędzia przeglądarki) i serwerem (program ASP.NET i kod źródłowy), łącząc najlepsze cechy obu tych środowisk w połączony zestaw funkcji.
> 
> Korzystając z narzędzia Page Inspector, możesz zobaczyć, które elementy w plikach źródłowych (łącznie z kodem po stronie serwera) zostały wygenerowane kod znaczników HTML do renderowania w przeglądarce. Narzędzie Page Inspector umożliwia także modyfikowanie właściwości stylów CSS oraz atrybutów elementów modelu DOM, aby zobaczyć zmiany, natychmiast odzwierciedlone w przeglądarce.
> 
> To ćwiczenie praktyczne spowoduje przeprowadzą Cię przez funkcje narzędzia Page Inspector i dowiesz się, jak można je rozwiązać problemy w aplikacjach sieci Web. **W tym laboratorium zawiera dwa ćwiczeń przy użyciu przepływów podobne, ale przeznaczone dla różnych technologii. Jeśli dla deweloperów programu ASP.NET MVC, wykonaj ćwiczenie Jeśli jesteś wykonywania postępuj zgodnie z projektanta formularzy sieci Web, dwa**.
> 
> W tym laboratorium przeprowadzi Cię przez ulepszeń i nowych funkcji, które opisano wcześniej, stosując drobne zmiany do przykładowej aplikacji sieci Web, pod warunkiem w folderze źródłowym.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym praktyczne laboratorium dowiesz się jak:

- Rozłóż witryny sieci Web za pomocą narzędzia Page Inspector
- Sprawdzanie i Podgląd zmian stylów CSS przy użyciu narzędzia Page Inspector
- Wykrywanie i rozwiązywanie problemów na stronach sieci web za pomocą narzędzia Page Inspector

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Należy dysponować następującymi elementami do przygotowania tego laboratorium:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).
- Program Internet Explorer 9 lub nowszy

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To ćwiczenie praktyczne obejmuje następujących czynnościach:

1. [Ćwiczenie 1: Korzystanie z narzędzia Page Inspector w projektach ASP.NET MVC](#Exercise1)
2. [Ćwiczenie 2: Za pomocą narzędzia Page Inspector w projektach formularzy WebForms](#Exercise2)

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązania, znajdującego się w folderze rozpoczęcia wykonywania, umożliwiający wykonanie każdego wykonywania niezależnie od innych. Wewnątrz kodu źródłowego dla ćwiczenia można również znaleźć folderu zakończenia, zawierającego rozwiązanie programu Visual Studio z kodem, powstałego w wykonaniu tych kroków, wykonując odpowiednie. Jeśli potrzebujesz dodatkowej pomocy, gdy pracujesz za pośrednictwem tego laboratorium praktycznego, można użyć jako wskazówki dotyczące tych rozwiązań.


Szacowany czas do ukończenia tego laboratorium: **30 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Ćwiczenie 1: Korzystanie z narzędzia Page Inspector w projektach ASP.NET MVC

W tym ćwiczeniu zostanie dowiesz się, jak wyświetlić podgląd i debugować **platformy ASP.NET MVC 4** rozwiązanie przy użyciu **narzędzie Page Inspector**. Należy najpierw wykonać krótkie omówienie narzędzia się funkcji, które ułatwiają debugowanie procesu w sieci Web. Następnie będzie działać na stronie sieci web, zawierającą stylów problemów. Dowiesz się jak Inspektor stron umożliwia znajdowanie kodu źródłowego, który generuje ten problem i rozwiązać ten problem.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Zadanie 1 — poznawanie narzędzia Page Inspector

W tym zadaniu dowiesz się, jak używać narzędzia Page Inspector w kontekście projektu programu ASP.NET MVC 4, pokazujący Galeria fotografii.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex1-MVC4/rozpoczęcia/** folderu.

   1. Musisz pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. W Eksploratorze rozwiązań zlokalizuj **Index.cshtml** wyświetlona w obszarze **/widoków domowych** folderu projektu, kliknij go prawym przyciskiem myszy i wybierz **widoku w narzędzia Page Inspector**.

    ![Wybieranie pliku, aby wyświetlić podgląd w narzędzia Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image1.png "wyboru pliku, aby wyświetlić podgląd w narzędzia Page Inspector")

    *Wybieranie pliku, aby wyświetlić podgląd w narzędzia Page Inspector*
3. Okno narzędzia Page Inspector będzie zawierać */Home/Index* adres URL mapowany do źródła wybranego widoku.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Pierwszy kontakt z narzędziem Page Inspector*

    Narzędzie Page Inspector jest zintegrowana w środowisku Visual Studio. Inspektor zawiera osadzony przeglądarce wraz z zaawansowany profiler HTML. Należy zauważyć, że jest konieczne uruchamianie rozwiązania, aby zobaczyć, jak wyglądają strony.

    > [!NOTE]
    > Gdy szerokość przeglądarki narzędzia Page Inspector jest mniejsza niż szerokość otwarta strona, nie zobaczysz stronę prawidłowo. Jeśli tak się stanie, należy dostosować szerokość narzędzia Page Inspector.
4. Kliknij przycisk **pliki** karcie narzędzie Page Inspector.

    Widoczne będą wszystkie pliki źródłowe, które są Tworzenie strony indeksu. Ta funkcja pomaga w identyfikacji wszystkie elementy, które na pierwszy rzut oka, szczególnie w przypadku, gdy pracujesz z widoki częściowe i szablony. Należy zauważyć, że możesz również otworzyć każdy plik po kliknięciu łącza.

    ![Karta pliki](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Karta pliki*
5. Kliknij przycisk **Przełącz tryb inspekcji** przycisk znajdujący się na kartach po lewej stronie.

    To narzędzie umożliwia wybranie dowolnego elementu strony i wyświetlić jego kod HTML i Razor.

    ![Toggle-Inspection-Mode-button](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Przełącz tryb inspekcji przycisku*
6. W przeglądarce narzędzia Page Inspector Przesuń wskaźnik myszy nad elementów strony. Podczas przesuwania wskaźnika myszy nad dowolną część renderowanej strony, typ elementu jest wyświetlany, oraz odpowiedni kod źródłowy lub kod jest wyróżniony w edytorze programu Visual Studio.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Tryb inspekcji w działaniu*

    > [!NOTE]
    > Nie zmaksymalizuj okno narzędzia Page Inspector lub nie będzie można wyświetlić na karcie podglądu przedstawiający kodu źródłowego. Kliknięcie elementu w narzędzia Page Inspector jest maksymalizacji pojawi się kod źródłowy zaznaczenia, ale będzie go ukryć okno narzędzia Page Inspector.

    Jeśli użytkownik należy zwrócić uwagę na **Index.cshtml** plik, zauważysz, że fragment kodu źródłowego, który generuje wybranego elementu jest wyróżniona. Ta funkcja ułatwia tworzenie, edytowanie plików źródłowych długie, zapewniając bezpośredni i szybki sposób uzyskiwać dostęp do kodu.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Sprawdzanie elementów*
7. Kliknij przycisk **Przełącz tryb inspekcji** przycisku (![wybierz kartę HTML, aby wyświetlić kod HTML w przeglądarce narzędzia Page Inspector.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Wybierz kartę HTML, aby wyświetlić kod HTML w przeglądarce narzędzia Page Inspector.") ) można wyłączyć kursora.
8. Wybierz **HTML** kartę, aby wyświetlić kod HTML w przeglądarce narzędzia Page Inspector.
9. W kod znaczników HTML Znajdź element listy z linkiem Koala i zaznacz je.

    Należy zauważyć, że po wybraniu kod, odpowiednie dane wyjściowe automatycznie zostanie wyróżniona w przeglądarce. Ta funkcja jest przydatna zobaczyć sposób renderowania bloku kodu HTML na stronie.

    ![Wybranie elementu HTML na stronie](using-page-inspector-in-visual-studio-2012/_static/image8.png "HTML z wybraniu elementu na stronie")

    *Wybieranie elementu HTML na stronie*
10. Kliknij przycisk **Przełącz tryb inspekcji** przycisk, aby włączyć *tryb inspekcji* i kliknij przycisk na pasku nawigacyjnym. Po prawej stronie kodu HTML, w okienku style zostanie wyświetlona lista ze stylami CSS stosowane do wybranego elementu.

    > [!NOTE]
    > Ponieważ nagłówek jest częścią układu witryny, narzędzie Page Inspector będzie również otworzyć \_plik Layout.cshtml i wyróżnienie na segmencie kodu.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Odnajdywanie stylów i pliki źródłowe zaznaczonego elementu*
11. Wskaźnikiem Przełącz Inspekcja włączona umieść kursor myszy poniżej niebieski pasek polecane, a następnie kliknij przycisk Półokrąg.

    ![Wybieranie elementu](using-page-inspector-in-visual-studio-2012/_static/image10.png "zaznaczenie elementu")

    *Wybieranie elementu*
12. W okienku style zlokalizować **obraz tła** w obszarze **.main zawartości** grupy. **Usuń zaznaczenie pola wyboru** **obraz tła** i zobacz, co się dzieje. Zauważysz, że przeglądarki w celu zmienienia natychmiast i koła jest ukryty.

    > [!NOTE]
    > Zmiany, które można zastosować na karcie Style Inspektor strony nie wpływają na oryginalny arkusza stylów. Można usunąć zaznaczenie style lub zmienić ich wartości jako tyle razy, ile chcesz, ale zostanie przywrócona po odświeżeniu strony.

    ![Włączanie i wyłączanie style CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "Włączanie i wyłączanie style CSS")

    *Włączanie i wyłączanie style CSS*
13. Teraz, kliknij pozycję "**Twoje logo**" tekst w nagłówku przy użyciu trybu inspekcji.
14. W **style** kartę, odszukaj **rozmiar czcionki** CSS atrybutu w ramach **.site tytuł** grupy. Kliknij dwukrotnie wartość atrybutu, a następnie Zamień wartość 2.3 em na **3 em**, a następnie naciśnij klawisz **ENTER**. Należy zauważyć, że tytuł wygląda większe.

    ![Zmiana wartości CSS w narzędzia Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image12.png "wartości zmiana CSS w narzędzia Page Inspector")

    *Zmiana wartości CSS w narzędzia Page Inspector*
15. Kliknij przycisk **— śledzenie stylów** karcie znajduje się w prawym okienku narzędzia Page Inspector. Jest to alternatywny sposób, aby zobaczyć wszystkie style, które są stosowane do wyboru, uporządkowane według nazwy atrybutu.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Śledzenie stylów CSS zaznaczonego elementu*
16. Kolejną funkcją narzędzia Page Inspector jest okienko układu. Tryb inspekcji, zaznacz pasek nawigacyjny, a następnie kliknij przycisk **układ** karty w okienku po prawej stronie. Zostanie wyświetlony dokładny rozmiar wybranego elementu, a także jego rozmiar przesunięcie, marża, uzupełnienie i obramowanie. Należy zauważyć, że wartości w tym widoku można również zmodyfikować.

    ![Układ elementów w narzędzia Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image14.png "układ elementów w narzędzia Page Inspector")

    *Układ elementów w narzędzia Page Inspector*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Zadanie 2 — Znajdowanie i rozwiązywanie problemów stylu w Galerii fotografii

Jak możesz zdiagnozować problemów stron sieci Web we wcześniejszych wersjach programu Visual Studio? To jest prawdopodobnie masz narzędzia debugowania, które są uruchomione spoza programu Visual Studio IDE, takich jak narzędzia dla deweloperów programu Internet Explorer lub Firebug w sieci web. Przeglądarek tylko zrozumienie kodu HTML, skryptów i stylów, podczas gdy podstawowej struktury generuje kod HTML, który będzie renderowany. Z tego powodu często konieczne do wdrożenia w całej lokacji, aby zobaczyć, jak wyglądało stron sieci web.

Ma prawdopodobnie wykonano następujące kroki, gdy chcesz wykryć i naprawić problem w witrynie sieci web:

1. Uruchom rozwiązanie za pomocą programu Visual Studio lub Wdróż strony na serwerze sieci web.
2. W przeglądarce otwórz narzędzia programistyczne używane, lub po prostu otwórz kod źródłowy i stylów i podjąć próbę dopasowania problem. Aby znaleźć pliki związane, użyto &quot;wyszukiwania&quot; lub &quot;wyszukiwania w plikach&quot; funkcji o nazwie klasy stylów.
3. Po wykryciu błędu, należy zatrzymać przeglądarki sieci Web a serwerem.
4. Wyczyść pamięć podręczną przeglądarki.
5. Wróć do programu Visual Studio, aby zastosować poprawkę. Powtórz kroki, aby przetestować.

Ponieważ istnieje nie rzeczywistych WYSIWYG w ASP.NET MVC 4, większość problemów style są wykrywane na późniejszym etapie, po uruchomiona lub jest wdrażanie aplikacji sieci web. Teraz za pomocą narzędzia Page Inspector to możliwe bez konieczności uruchamiania rozwiązania w wersji zapoznawczej dowolnej strony.

W ramach tego zadania będzie używane narzędzie Page inspector i rozwiązywanie niektórych problemów z aplikacji z galerii fotografii.

1. Za pomocą narzędzia Page Inspector zlokalizować **zarejestrować** i **Zaloguj** linki po lewej stronie nagłówka.

    Należy zauważyć, że łącza nie są wyświetlane w oczekiwanym miejscu po prawej stronie i są wyświetlane takie jak listy punktowanej. Teraz będzie wyrównać łącza z prawej strony i zmienianie stylu odpowiednio je.

    ![Lokalizowanie rejestru i dziennika w linkach](using-page-inspector-in-visual-studio-2012/_static/image15.png "lokalizowania rejestru i dziennika linki")

    *Lokalizowanie rejestru i dziennika w łącza*
2. Przełącz tryb inspekcji zaznaczone kliknij przycisk Zamknij, aby, ale nie na, link Zarejestruj, aby otworzyć jego kod.

    Należy zauważyć, że kod źródłowy łączy znajduje się w  **\_LoginPartial.cshtml** pliku nie Index.cshtml ani \_Layout.cshtml, będące miejscach może wyglądać na pierwszym miejscu. Zostały umieszczone bezpośrednio w pliku poprawnego źródła.
3. W **style** kartę, zlokalizuj i kliknij **<section> #login</section>** elementu, który jest kontenerem HTML, aby te łącza.

    Należy zauważyć, że **#login** styl automatycznie znajduje się w **Site.css** po kliknięciu przycisku. Ponadto ten kod jest podświetlona.

    ![Wybieranie style CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "wybierając style CSS")

    *Wybieranie style CSS*
4. Usuń znaczniki komentarza **text-align** atrybutu w wyróżniony kod, usuwając otwierającym i zamykającym znaków i zapisać **Site.css** pliku.

    Narzędzie Page Inspector zna wszystkie różnych plików wchodzących w skład bieżącej strony i może wykryć, gdy zmienią się któryś z tych plików. Generuje alert w każdym przypadku, gdy nie jest zsynchronizowany z plikami źródłowymi bieżącej strony w przeglądarce.
5. W przeglądarce narzędzia Page Inspector kliknij pasek znajdujący się poniżej paska adresu, aby ponownie załadować stronę.

    ![Ponownie załadować stronę](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Ponownie załadować stronę*

    Linki są teraz z prawej strony, ale nadal wyglądają listy punktowanej. Teraz spowoduje usunięcie punktory i wyrównanie w poziomie łącza przez użytkownika.

    ![Zaktualizowana strona](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Zaktualizowana strona*
6. Przy użyciu trybu inspekcji, wybierz dowolne z **&lt;li&gt;** elementów, które zawierają &quot;zarejestrować&quot; i &quot;Zaloguj&quot; łącza. Następnie kliknij przycisk  **&lt;sekcji&gt; #login** element, aby uzyskać dostęp do **Styles.css** kodu.

    ![Znajdowanie styl](using-page-inspector-in-visual-studio-2012/_static/image19.png "znajdowanie stylu")

    *Znajdowanie stylu*
7. W **Style.css**, Usuń komentarz w kodzie **#login li** elementów. Styl, który dodajesz spowoduje Ukryj punktor i wyświetlanie elementów w poziomie.

    ![Restyling łącza logowania](using-page-inspector-in-visual-studio-2012/_static/image20.png "Restyling łącza logowania")

    *Restyling łącza logowania*
8. Zapisz **Style.css** plików i kliknij jeden raz, na pasku znajdujący się poniżej adres, aby ponownie załadować stronę. Należy zauważyć, że łącza są poprawnie wyświetlane.

    ![Łącza są wyrównane do prawej strony](using-page-inspector-in-visual-studio-2012/_static/image21.png "łącza wyrównany do prawej")

    *Linki wyrównany do prawej*
9. Na koniec będzie zmienić tytuł nagłówka. W trybie inspekcji kliknij **Twoje logo** tekstu i uzyskiwać do kodu źródłowego, który generuje go.
10. Teraz znajdują się w  **\_Layout.cshtml**, zastąp "**Twoje logo**"tekstem"**Galeria fotografii**". Zapisz i zaktualizować przeglądarkę narzędzia Page Inspector.

    ![Przypisywanie nowy tytuł](using-page-inspector-in-visual-studio-2012/_static/image22.png "przypisywanie nowy tytuł")

    *Przypisywanie nowy tytuł*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Strona Galeria fotografii zaktualizowane*
11. Na koniec wybierz **PhotoGallery** projektu i naciśnij klawisz **F5** do uruchomienia aplikacji. Sprawdź wszystkie zmiany działają zgodnie z oczekiwaniami.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Ćwiczenie 2: Za pomocą narzędzia Page Inspector w projektach formularzy WebForms

W tym ćwiczeniu dowiesz się, jak wyświetlić podgląd i debugowanie rozwiązania formularzy sieci Web za pomocą narzędzia Page Inspector. Należy najpierw wykonać krótkie omówienie narzędzia się funkcje narzędzia Page Inspector, ułatwiające debugowanie procesu w sieci Web. Następnie będzie działać na stronie sieci web, zawierającą stylów problemów. Dowiesz się jak Inspektor stron umożliwia znajdowanie kodu źródłowego, który generuje ten problem i rozwiązać ten problem.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Zadanie 1 — poznawanie narzędzia Page Inspector

W tym zadaniu dowiesz się, jak używać funkcji narzędzia Page Inspector w kontekście projektu formularzy sieci Web, który pokazuje Galeria fotografii.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex2-formularzy sieciWeb/rozpoczęcia/** folderu.

   1. Musisz pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. W Eksploratorze rozwiązań zlokalizuj **Default.aspx** strony, kliknij go prawym przyciskiem myszy i wybierz **widoku w narzędzia Page Inspector**.

    ![Otwarcie Default.aspx za pomocą narzędzia Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "otwarcie Default.aspx za pomocą narzędzia Page Inspector")

    *Otwieranie Default.aspx za pomocą narzędzia Page Inspector*
3. Okno narzędzia Page Inspector będzie zawierać Default.aspx.

    ![Wyświetlanie Default.aspx w narzędzia Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image25.png "wyświetlania Default.aspx w narzędzia Page Inspector")

    *Wyświetlanie Default.aspx w narzędzia Page Inspector*

    Narzędzie Page Inspector jest zintegrowana w środowisku Visual Studio. Inspektor zawiera osadzony przeglądarce wraz z zaawansowany profiler HTML, która będzie wyświetlana w zaznaczonym kodzie. Należy zauważyć, że jest konieczne uruchamianie rozwiązania, aby zobaczyć, jak wyglądają strony.

    > [!NOTE]
    > Gdy szerokość przeglądarki narzędzia Page Inspector jest mniejsza niż szerokość otwarta strona, nie zobaczysz stronę prawidłowo. Jeśli tak się stanie, należy dostosować szerokość narzędzia Page Inspector.
4. Kliknij przycisk **pliki** karcie narzędzie Page Inspector.

    Widoczne będą wszystkie pliki źródłowe, które są redagowania renderowanej strony domyślnej. Jest to przydatne, aby zidentyfikować wszystkie elementy, które na pierwszy rzut oka, szczególnie w przypadku, gdy pracujesz z formantami użytkowników i stron wzorcowych. Należy zauważyć, że możesz także przejść do poszczególnych plików.

    ![Karta pliki](using-page-inspector-in-visual-studio-2012/_static/image26.png "karty pliki")

    *Karta pliki*
5. Kliknij przycisk **Przełącz tryb inspekcji** przycisk znajdujący się na kartach po lewej stronie.

    To narzędzie umożliwia wybranie dowolnego elementu strony i wyświetlić źródło HTML kodu i .aspx.

    ![Przełącz tryb inspekcji przycisk](using-page-inspector-in-visual-studio-2012/_static/image27.png "przycisk Przełącz tryb inspekcji")

    *Przełącz tryb inspekcji przycisku*
6. Wskaż myszą elementów strony w przeglądarce narzędzia Page Inspector. Podczas przesuwania wskaźnika myszy nad dowolną część renderowanej strony, typ elementu jest wyświetlany, oraz odpowiedni kod źródłowy lub kod jest wyróżniony w edytorze programu Visual Studio.

    ![Tryb inspekcji w działaniu](using-page-inspector-in-visual-studio-2012/_static/image28.png "tryb inspekcji w działaniu")

    *Tryb inspekcji w działaniu*

    > [!NOTE]
    > Nie zmaksymalizuj okno narzędzia Page Inspector lub nie będzie można wyświetlić na karcie podglądu przedstawiający kodu źródłowego. Kliknięcie elementu w narzędzia Page Inspector jest maksymalizacji pojawi się kod źródłowy zaznaczenia, ale będzie go ukryć okno narzędzia Page Inspector.

    Jeśli użytkownik należy zwrócić uwagę na **Default.aspx** plik, zauważysz, że fragment kodu źródłowego, który generuje wybranego elementu jest wyróżniona. Ta funkcja ułatwia wersję plików źródłowych długie, zapewniając bezpośredni i szybki sposób uzyskiwać dostęp do kodu.

    ![Sprawdzanie elementów](using-page-inspector-in-visual-studio-2012/_static/image29.png "Sprawdzanie elementów")

    *Sprawdzanie elementów*
7. Kliknij przycisk **Przełącz tryb inspekcji** przycisku (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), znajdujący się na kartach Narzędzia Page Inspector, aby wyłączyć kursora.
8. Wybierz **HTML** kartę, aby wyświetlić kod HTML w przeglądarce narzędzia Page Inspector.
9. W kodzie HTML Znajdź element listy z linkiem Koala i zaznacz je.

    Zauważ, że po wybraniu kod, odpowiednie dane wyjściowe automatycznie wyróżnione przeglądarki. Ta funkcja jest przydatna zobaczyć sposób renderowania bloku kodu HTML na stronie.

    ![Wybieranie elementu HTML na stronie](using-page-inspector-in-visual-studio-2012/_static/image31.png "zaznaczenie elementu HTML na stronie")

    *Wybieranie elementu HTML na stronie*
10. Kliknij przycisk **Przełącz tryb inspekcji** przycisk, aby włączyć *tryb inspekcji* i kliknij przycisk na pasku nawigacyjnym. Po prawej stronie kodu HTML, w okienku style zostanie wyświetlona lista ze stylami CSS stosowane do wybranego elementu.

    > [!NOTE]
    > ponieważ nagłówek jest częścią układu witryny, narzędzie Page Inspector również otworzyć plik Site.Master i zaznacz segment kodu, których to dotyczy.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "odnajdywanie stylów i pliki źródłowe zaznaczonego elementu")

    *Odnajdywanie stylów i pliki źródłowe zaznaczonego elementu*
11. Wskaźnikiem Przełącz Inspekcja włączona umieść kursor myszy poniżej paska menu, a następnie kliknij przycisk puste Półokrąg.

    ![Wybieranie elementu](using-page-inspector-in-visual-studio-2012/_static/image33.png "zaznaczenie elementu")

    *Wybieranie elementu*
12. W okienku style zlokalizować **obraz tła** w obszarze **.main zawartości** grupy. **Usuń zaznaczenie pola wyboru** **obraz tła** i zobacz, co się dzieje. Zauważysz, że przeglądarki w celu zmienienia natychmiast i koła jest ukryty.

    > [!NOTE]
    > Zmiany, które można zastosować na karcie Style Inspektor strony nie wpływają na oryginalny arkusza stylów. Można usunąć zaznaczenie style lub zmienić ich wartości jako tyle razy, ile chcesz, ale zostanie przywrócona po odświeżeniu strony.

    ![Włączanie i wyłączanie CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "Włączanie i wyłączanie style CSS")

    *Włączanie i wyłączanie style CSS*
13. Teraz, kliknij pozycję "**swoje** **logo"** tekstu na nagłówek, przy użyciu trybu inspekcji.
14. W **style** kartę, odszukaj **rozmiar czcionki** CSS atrybutu w ramach **.site tytuł** grupy. Kliknij dwukrotnie raz atrybutu do edycji jej wartości. Zastąp 2.3em wartość z **3em**, a następnie naciśnij klawisz ENTER. Należy zauważyć, że tytuł wygląda większe.

    ![Zmiana wartości CSS w Inspector2 strony](using-page-inspector-in-visual-studio-2012/_static/image35.png "wartości zmiana CSS w narzędzia Page Inspector")

    *Zmiana wartości CSS w narzędzia Page Inspector*
15. Kliknij przycisk **— śledzenie stylów** karcie znajduje się w prawym okienku narzędzia Page Inspector. Jest to alternatywny sposób, aby zobaczyć wszystkie style, które są stosowane do wyboru, uporządkowane według nazwy atrybutu.

    ![Śledzenie stylów CSS zaznaczonego elementu](using-page-inspector-in-visual-studio-2012/_static/image36.png "śledzenie stylów CSS zaznaczonego elementu")

    *Śledzenie stylów CSS zaznaczonego elementu*
16. Kolejną funkcją narzędzia Page Inspector jest okienko układu. Tryb inspekcji, zaznacz pasek nawigacyjny, a następnie kliknij przycisk **układ** karty w okienku po prawej stronie. Zostanie wyświetlony dokładny rozmiar wybranego elementu, a także jego rozmiar przesunięcie, marża, uzupełnienie i obramowanie. Należy zauważyć, że wartości w tym widoku można również zmodyfikować.

    ![Układ elementów w narzędzia Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image37.png "układ elementów w narzędzia Page Inspector")

    *Układ elementów w narzędzia Page Inspector*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Zadanie 2 — Znajdowanie i rozwiązywanie problemów stylu w Galerii fotografii

Jak możesz zdiagnozować problemów stron sieci Web we wcześniejszych wersjach programu Visual Studio? To jest prawdopodobnie masz narzędzia debugowania, które są uruchomione spoza programu Visual Studio IDE, takich jak narzędzia dla deweloperów programu Internet Explorer lub Firebug w sieci web. Przeglądarek tylko zrozumienie kodu HTML, skryptów i stylów, podczas gdy podstawowej struktury generuje kod HTML, który będzie renderowany. Z tego powodu często konieczne do wdrożenia w całej lokacji, aby zobaczyć, jak wyglądało stron sieci web.

Ma prawdopodobnie wykonano następujące kroki, gdy chcesz wykryć i naprawić problem w witrynie sieci web:

1. Uruchom rozwiązanie za pomocą programu Visual Studio lub Wdróż strony na serwerze sieci web.
2. W przeglądarce otwórz narzędzia programistyczne używane, lub po prostu otwórz kod źródłowy i stylów i podjąć próbę dopasowania problem. Aby znaleźć pliki związane, użyto &quot;wyszukiwania&quot; lub &quot;wyszukiwania w plikach&quot; funkcji o nazwie klasy stylów.
3. Po wykryciu błędu, należy zatrzymać przeglądarki sieci Web a serwerem.
4. Wyczyść pamięć podręczną przeglądarki.
5. Wróć do programu Visual Studio, aby zastosować poprawkę. Powtórz kroki, aby przetestować.

Ponieważ nie ma nie rzeczywiste WYSIWYG w formularzy sieci Web platformy ASP.NET, niektóre problemy dotyczące stylu są wykrywane na późniejszym etapie, po uruchomiona lub jest wdrażanie. Teraz za pomocą narzędzia Page Inspector to możliwe bez konieczności uruchamiania rozwiązania w wersji zapoznawczej dowolnej strony.

W tym zadaniu można będzie używać narzędzia Page inspector rozwiązywanie niektórych problemów aplikacji z galerii fotografii. W poniższych krokach będą wykrywa i szybko rozwiązać jakiś problem proste style w nagłówku.

1. Za pomocą strony kontroli, zlokalizuj **zarejestrować** i **logowanie** linki po lewej stronie nagłówka.

    Należy zauważyć, że link nie jest wyświetlany w oczekiwanym miejscu po prawej stronie. Będzie teraz wyrównać łącza z prawej strony i w związku z tym zmienianie stylu.

    ![Zaloguj się łącze umieszczone po lewej stronie](using-page-inspector-in-visual-studio-2012/_static/image38.png "Zaloguj się łącze umieszczone po lewej stronie")

    *Dziennik łącze umieszczone po lewej stronie*
2. Przełącz tryb inspekcji wybrana wybierz łącze Zaloguj, aby otworzyć jego kodu.

    Należy zauważyć, że kod źródłowy link znajduje się w **Site.Master** pliku, na stronie Default.aspx, która jest miejscem, w którym może wyglądać na pierwszym miejscu; zostanie umieszczone bezpośrednio w pliku poprawnego źródła.
3. W **style** kartę, zlokalizuj i kliknij  **&lt;sekcji&gt; #login** elementu, który jest kontenerem HTML, aby te łącza.

    Należy zauważyć, że **#login** styl automatycznie znajduje się w **Site.css** po kliknięciu przycisku. Ponadto ten kod jest podświetlona.

    ![Wybieranie style CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "wybierając style CSS")

    *Wybieranie style CSS*
4. Usuń znaczniki komentarza **text-align** atrybutu w wyróżniony kod, usuwając otwierającym i zamykającym znaków i zapisać **Site.css** pliku.

    Narzędzie Page Inspector zna wszystkie różnych plików wchodzących w skład bieżącej strony i może wykryć, gdy zmienią się któryś z tych plików. Generuje alert w każdym przypadku, gdy nie jest zsynchronizowany z plikami źródłowymi bieżącej strony w przeglądarce.
5. W przeglądarce narzędzia Page Inspector kliknij pasek znajdujący się poniżej paska adresu, aby zapisać zmiany i ponownie załaduj stronę.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Ponownie załadować stronę*

    Linki są teraz z prawej strony, ale nadal wyglądają listy punktowanej. Teraz spowoduje usunięcie punktory i wyrównanie w poziomie łącza przez użytkownika.

    ![Zaktualizowana strona](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Zaktualizowana strona*
6. Przy użyciu trybu inspekcji, wybierz dowolne z **&lt;li&gt;** elementów, które zawierają &quot;zarejestrować&quot; i &quot;Zaloguj&quot; łącza. Następnie kliknij przycisk  **&lt;sekcji&gt; #login** element, aby uzyskać dostęp do **Styles.css** kodu.

    ![Znajdowanie styl](using-page-inspector-in-visual-studio-2012/_static/image42.png "znajdowanie stylu")

    *Znajdowanie stylu*
7. W **Style.css**, Usuń komentarz w kodzie **#login li** elementów. Styl, który dodajesz spowoduje Ukryj punktor i wyświetlanie elementów w poziomie.

    ![Restyling łącza logowania](using-page-inspector-in-visual-studio-2012/_static/image43.png "Restyling łącza logowania")

    *Restyling łącza logowania*
8. Zapisz **Style.css** plików i kliknij jeden raz, na pasku znajdujący się poniżej adres, aby ponownie załadować stronę. Należy zauważyć, że łącza są poprawnie wyświetlane.

    ![Łącza są wyrównane do prawej strony](using-page-inspector-in-visual-studio-2012/_static/image44.png "łącza wyrównany do prawej")

    *Linki wyrównany do prawej*
9. Na koniec będzie zmienić tytuł nagłówka. Zamiast wyszukiwać "**Twoje logo"** tekst we wszystkich plikach w trybie inspekcji kliknij tekst i uzyskać dostęp do kodu źródłowego, który generuje go.

    ![Tytuł witryny znajdowanie](using-page-inspector-in-visual-studio-2012/_static/image45.png "wyszukiwanie tytuł witryny")

    *Znajdowanie tytuł witryny*
10. Teraz znajdują się w **Site.Master**, zastąp "**Twoje logo**"tekstem"**Galeria fotografii**". Zapisz i zaktualizować przeglądarkę narzędzia Page Inspector.

    ![Strona Galeria fotografii zaktualizowane](using-page-inspector-in-visual-studio-2012/_static/image46.png "stronę Galeria fotografii zaktualizowane")

    *Strona Galeria fotografii zaktualizowane*
11. Na koniec zaznacz **F5** do uruchomienia aplikacji, sprawdź wszystkie zmiany działają zgodnie z oczekiwaniami.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Przez ukończenie tego laboratorium praktyczne, masz dzięki modelom uczenia jak wyświetlić podgląd aplikacji sieci Web bez konieczności ponownego kompilowania i uruchamiania witryny sieci Web w przeglądarce za pomocą narzędzia Page Inspector. Ponadto zostały wcześniej jak szybkie znalezienie i naprawienie usterek, uzyskując bezpośrednio w wyniku renderowania do kodu źródłowego.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "zainstalować program Visual Studio Express")

    *Zainstaluj program Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express for Web kafelka*
