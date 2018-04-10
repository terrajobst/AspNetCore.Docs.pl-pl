---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: W programie Visual Studio 2012 za pomocą narzędzia Page Inspector | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym laboratorium Hands-on odnajdzie nowe narzędzie do znajdowania i rozwiązywania problemów strony sieci web w programie Visual Studio — narzędzie Page Inspector. Narzędzie Page Inspector to nowe narzędzie tego b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 052d29dba170d403c2b1c1667c55fc2c34045615
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
---
<a name="using-page-inspector-in-visual-studio-2012"></a>W programie Visual Studio 2012 za pomocą narzędzia Page Inspector
====================
Przez [obozów sieci Web Team](https://twitter.com/webcamps)

> W tym laboratorium Hands-on odnajdzie nowe narzędzie do znajdowania i rozwiązywania problemów strony sieci web w programie Visual Studio — narzędzie Page Inspector.
> 
> Narzędzie Page Inspector to nowe narzędzie wprowadzające narzędzia diagnostyczne przeglądarki do programu Visual Studio i oferujące integrację przeglądarki, ASP.NET i kod źródłowy. Renderuje stronę sieci web (HTML, formularzy sieci Web, ASP.NET MVC lub strony sieci Web) bezpośrednio z poziomu programu Visual Studio IDE, a pozwala sprawdzić zarówno kod źródłowy, jak i dane wyjściowe. Narzędzie Page Inspector umożliwia łatwą dekompozycję witryny sieci Web, błyskawiczne tworzenie stron od podstaw w i szybkie diagnozowanie problemów.
> 
> Dzisiaj mamy kilka platformy sieci Web, które tworzenie witryn sieci Web elastyczność i skalowalność w odpowiednim czasie, takich jak ASP.NET MVC i formularzy sieci Web. Z drugiej strony pobiera trudniej można znaleźć problemy na stronach IDE nie obsługują Widok projektanta strony na podstawie szablonu i zawartość dynamiczną. W związku z tym tych witryn sieci Web muszą być otwarte w przeglądarce, aby sprawdzić, jak są wyświetlane dla użytkownika.
> 
> Deweloperzy sieci Web stosowanie zewnętrznych narzędzi, aby znaleźć problemy, które regularnie uruchamianie jej w przeglądarce. Następnie wróć do środowiska IDE i rozpocząć rozwiązywanie. To i z powrotem działania między IDE, przeglądarki i narzędzia profilowania może być mało wydajne i czasami wymaga świeże wdrożenie i zawsze Aby odtworzyć problem czyszczenia pamięci podręcznej.
> 
> Narzędzie Page Inspector pomost przerwa w aplikacji sieci Web między klientem (Narzędzia przeglądarki) i serwerem (ASP.NET i kod źródłowy), łącząc najlepsze cechy obu tych środowisk w połączony zestaw funkcji.
> 
> Za pomocą narzędzia Page Inspector, można sprawdzić elementów w plikach źródłowych (w tym kod po stronie serwera) tworzą kod znaczników HTML do renderowania w przeglądarce. Narzędzie Page Inspector umożliwia także modyfikowanie właściwości stylów CSS oraz atrybutów elementów modelu DOM, aby zobaczyć zmiany natychmiast odzwierciedlone w przeglądarce.
> 
> To laboratorium praktycznego będzie zademonstrować funkcje narzędzia Page Inspector i pokazują, jak można je Rozwiąż problemy w aplikacji sieci Web. **W tym laboratorium zawiera dwa ćwiczeń przy użyciu podobnych przepływów, ale przeznaczonych dla różnych technologii. Jeżeli deweloperów programu ASP.NET MVC, należy wykonać ćwiczenia w przypadku wykonywania wykonaj projektanta formularzy sieci Web, dwa**.
> 
> W tym laboratorium przeprowadzi Cię przez rozszerzenia oraz nowe funkcje opisane wcześniej przez zastosowanie drobne zmiany do przykładowej aplikacji sieci Web w folderze źródłowym.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktycznego przedstawiono sposób:

- Rozłóż witryny sieci Web przy użyciu narzędzia Page Inspector
- Zbadaj i Podgląd zmian style CSS z narzędziem Page Inspector
- Wykrywanie i naprawianie problemów na stronach sieci web za pomocą narzędzia Page Inspector

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Musi mieć następujące elementy do przygotowania tego laboratorium:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).
- Program Internet Explorer 9 lub nowszy

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To laboratorium praktycznego obejmuje następujących czynnościach:

1. [Ćwiczenie 1: Za pomocą narzędzia Page Inspector w projekty składnika ASP.NET MVC](#Exercise1)
2. [Ćwiczenie 2: Za pomocą narzędzia Page Inspector w projektach formularzy sieci Web](#Exercise2)

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązania, znajdujących się w folderze Begin ćwiczenia, umożliwiający postępuj zgodnie z każdym wykonywania niezależnie od innych. W kodzie źródłowym wykonywania będzie również znaleźć folderu zakończenia zawierającego rozwiązanie Visual Studio z kodem, który powoduje wykonanie czynności opisane w odpowiedniej wykonywania. Jeśli potrzebujesz dodatkowej pomocy w pracy za pośrednictwem tego laboratorium praktycznego, można użyć tych rozwiązań jako wskazówki.


Szacowany czas trwania tego laboratorium: **30 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Ćwiczenie 1: Za pomocą narzędzia Page Inspector w projekty składnika ASP.NET MVC

W tym ćwiczeniu dowiesz sposób Podgląd i debugowania **ASP.NET MVC 4** przy użyciu rozwiązania **narzędzie Page Inspector**. Po pierwsze będzie wykonywać krótki laptop wokół narzędzie, aby dowiedzieć się więcej funkcji, które ułatwiają debugowanie procesu sieci Web. Następnie będzie działać na stronie sieci web zawierający style problemy. Dowiesz się używania narzędzie Page Inspector można znaleźć kodu źródłowego, który generuje problemu i rozwiąż problem.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Zadanie 1 - eksploracji narzędzie Page Inspector

W tym zadaniu dowiesz się, jak używać narzędzia Page Inspector w kontekście projektu programu ASP.NET MVC 4, pokazujący galerii fotografii.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex1-MVC4/Begin/** folderu.

   1. Należy pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. W Eksploratorze rozwiązań, Znajdź **Index.cshtml** wyświetlić w obszarze **/widoków domowych** folderu projektu, kliknij go prawym przyciskiem myszy i wybierz **widoku w narzędzie Page Inspector**.

    ![Wybieranie plików do podglądu w narzędzie Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image1.png "Wybieranie plików do podglądu w narzędzie Page Inspector")

    *Wybieranie plików do podglądu w narzędzie Page Inspector*
3. Okno narzędzia Page Inspector będzie zawierać */Home/indeksu* adres URL mapowany do źródła wybranego widoku.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Pierwszym kontakcie z narzędziem Page Inspector*

    Narzędzie narzędzie Page Inspector jest zintegrowana w środowisku Visual Studio. Inspektor zawiera osadzony przeglądarki, wraz z zaawansowanych profilera HTML. Należy zauważyć, że nie masz Aby uruchomić rozwiązanie, aby zobaczyć strony.

    > [!NOTE]
    > Gdy szerokość przeglądarkę narzędzia Page Inspector jest mniejsza niż szerokość otwarta strona, nie zobaczysz stronę poprawnie. Jeśli tak się stanie, należy dostosować szerokość narzędzie Page Inspector.
4. Kliknij przycisk **pliki** kartę w narzędzie Page Inspector.

    Zostanie wyświetlone wszystkie pliki źródłowe, które są redagowania strony indeksu. Ta funkcja pomaga zidentyfikować wszystkie elementy w skrócie, szczególnie w przypadku, gdy użytkownik pracuje z częściowa widoki i szablony. Należy zauważyć, że można również otworzyć plików po kliknięciu łącza.

    ![Karta pliki](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Karta pliki*
5. Kliknij przycisk **Przełącz tryb inspekcji** przycisk znajdujący się po lewej stronie kart.

    To narzędzie będzie można wybrać dowolny element strony i wyświetlić jego kod HTML i Razor.

    ![Toggle-Inspection-Mode-button](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Przycisk przełączania trybu inspekcji*
6. W przeglądarce narzędzie Page Inspector Przesuń wskaźnik myszy nad elementy na stronie. Podczas przesuwania wskaźnika myszy nad dowolną część renderowanej strony wyświetlany jest typ elementu, a odpowiedni kod źródłowy lub kod zostanie wyróżniona w edytorze programu Visual Studio.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Tryb inspekcji w akcji*

    > [!NOTE]
    > Nie Maksymalizuj okno narzędzia Page Inspector lub nie będzie widoczna na karcie Podgląd przedstawiający kodu źródłowego. Po kliknięciu elementu w narzędzie Page Inspector jest maksymalizacji, zostanie wyświetlony kod źródłowy zaznaczenia, ale spowoduje ukrycie okna narzędzia Page Inspector.

    Jeśli użytkownik należy zwrócić uwagę na **Index.cshtml** plików, można zauważyć, że zostanie wyróżniona części kodu źródłowego, który generuje wybranego elementu. Ta funkcja umożliwia edytowanie plików źródłowych długie, dzięki czemu bezpośredniego i szybki dostęp do kodu.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Zapoznanie się elementy*
7. Kliknij przycisk **Przełącz tryb inspekcji** przycisk (![wybierz kartę HTML, aby wyświetlić kod HTML renderowane w przeglądarce narzędzie Page Inspector.] (using-page-inspector-in-visual-studio-2012/_static/image7.png "Wybierz kartę HTML, aby wyświetlić kod HTML renderowane w przeglądarce narzędzie Page Inspector.") ) można wyłączyć kursora.
8. Wybierz **HTML** kartę, aby wyświetlić kod HTML renderowane w przeglądarce narzędzie Page Inspector.
9. W znaczniku HTML Znajdź element listy z łączem Koala i zaznacz je.

    Należy zauważyć, że po wybraniu kod odpowiednie dane wyjściowe automatycznie zostanie wyróżniona w przeglądarce. Ta funkcja jest przydatna zobaczyć, jak jest renderowany blok HTML na stronie.

    ![Wybieranie elementu HTML na stronie](using-page-inspector-in-visual-studio-2012/_static/image8.png "elementu wybranie HTML na stronie")

    *Wybranie elementu HTML na stronie*
10. Kliknij przycisk **Przełącz tryb inspekcji** przycisk, aby włączyć *trybu inspekcji* i kliknij na pasku nawigacji. Po prawej stronie kodu HTML, w okienku style zostanie wyświetlona lista przy użyciu stylów CSS stosowany do wybranego elementu.

    > [!NOTE]
    > Ponieważ nagłówka jest częścią układu witryny, narzędzie Page Inspector będzie również otworzyć \_plik Layout.cshtml i podświetl wpływ segment kodu.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Odnajdywanie stylów i pliki źródłowe zaznaczonego elementu*
11. Wskaźnikiem Przełącz inspekcji włączone Przenieś wskaźnik myszy poniżej paska polecanych niebieski, a następnie kliknij przycisk połowa okręgu.

    ![Wybranie elementu](using-page-inspector-in-visual-studio-2012/_static/image10.png "zaznaczenie elementu")

    *Wybranie elementu*
12. W okienku style zlokalizować **obraz tła** elementu w obszarze **.main zawartości** grupy. **Usuń zaznaczenie pola wyboru** **obraz tła** i zobacz, co się stanie. Można zauważyć, że przeglądarki zmienienia natychmiast i okręgu jest ukryty.

    > [!NOTE]
    > Zmiany, które należy zastosować na karcie Strona inspektora style nie wpływają na oryginalnym arkusza stylów. Możesz usunąć zaznaczenie pola wyboru style lub zmieniać tyle razy, ale będzie można przywrócić po odświeżeniu strony.

    ![Włączanie i wyłączanie style CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "Włączanie i wyłączanie stylów CSS")

    *Włączanie i wyłączanie stylów CSS*
13. Teraz, kliknij przycisk "**tutaj znak logo**" tekst w nagłówku przy użyciu trybu inspekcji.
14. W **style** zlokalizuj **rozmiar czcionki** atrybutu CSS w obszarze **.site tytuł** grupy. Kliknij dwukrotnie wartość atrybutu, a wartość 2.3 em **3 em**, a następnie naciśnij klawisz **ENTER**. Zwróć uwagę, że tytuł wygląda większy.

    ![Zmiana wartości CSS w narzędzie Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image12.png "wartości zmiana CSS w narzędzie Page Inspector")

    *Zmiana wartości CSS w narzędzie Page Inspector*
15. Kliknij przycisk **style śledzenia** kartę, znajduje się w okienku po prawej stronie narzędzie Page Inspector. Jest to alternatywny sposób, aby wyświetlić wszystkie style, które są stosowane do zaznaczenia, uporządkowanych według nazwy atrybutu.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Śledzenie style CSS wybranego elementu*
16. Inna funkcja narzędzie Page Inspector to okienko układu. Przy użyciu trybu inspekcji, wybierz na pasku nawigacyjnym, a następnie kliknij przycisk **układu** karty w okienku po prawej stronie. Zostanie wyświetlone rozmiarze wybranego elementu, a także rozmiaru przesunięcie, margines i Wypełnienie obramowania. Należy zauważyć, że wartości w tym widoku można również zmodyfikować.

    ![Elementu układu w narzędzie Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image14.png "układu elementu w narzędzie Page Inspector")

    *Elementu układu w narzędzie Page Inspector*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Zadanie 2 — Znajdowanie i rozwiązywanie problemów stylu w Galerii fotografii

Jak można zdiagnozować problemy stron sieci Web z poprzednich wersji programu Visual Studio? Jesteś prawdopodobnie znane z narzędzia debugowania, które są uruchomione spoza programu Visual Studio IDE, takich jak narzędzi deweloperskich programu Internet Explorer lub Firebug sieci web. Przeglądarki tylko zrozumieć HTML, skryptów i stylów, podczas gdy podstawowej struktury generuje kod HTML, który będzie renderowany. Z tego powodu często konieczne wdrażanie całej lokacji, aby zobaczyć, jak wyglądają stron sieci web.

Przy Wykryj i napraw problem w witrynie sieci web ma prawdopodobnie zostały wykonane następujące kroki:

1. Uruchom rozwiązania z programu Visual Studio, albo wdrożyć strony na serwerze sieci web.
2. W przeglądarce otwórz narzędzi deweloperskich używanie lub po prostu otworzyć kodu źródłowego i style i podjąć próbę dopasowania problem. Znajdź pliki związane, możesz korzystać z &quot;wyszukiwania&quot; lub &quot;wyszukiwania w plikach&quot; funkcji o nazwie klasy stylów.
3. Po wykryciu błąd zatrzymania przeglądarki sieci Web a serwerem.
4. Wyczyścić pamięć podręczną przeglądarki.
5. Wróć do programu Visual Studio, aby zastosować poprawkę. Powtórz kroki, aby przetestować.

Ponieważ w technologii ASP.NET MVC 4, nie ma żadnych rzeczywistych WYSIWYG, większość problemów styl są wykrywane na późniejszym etapie, po uruchomiona lub wdrażania aplikacji sieci web. Teraz z narzędziem Page Inspector jest możliwe do podglądu dowolną stronę bez uruchamiania rozwiązania.

To zadanie będzie używane narzędzie Page inspector i rozwiązać problemy, Galeria fotografii aplikacji.

1. Za pomocą narzędzia Page Inspector zlokalizować **zarejestrować** i **Zaloguj** linki po lewej stronie nagłówka.

    Zwróć uwagę, linki nie są wyświetlane w oczekiwanym miejscu po prawej stronie, czy są wyświetlane takie jak listy punktowanej. Zostanie teraz Dopasuj łącza po prawej stronie i odpowiednio je restyle.

    ![Lokalizowanie rejestru i dziennika w łączach](using-page-inspector-in-visual-studio-2012/_static/image15.png "lokalizowanie rejestru i dziennika w łącza")

    *Lokalizowanie rejestru i dziennika w łącza*
2. Przełącz tryb inspekcji zaznaczone kliknij przycisk Zamknij, aby, ale nie na, łącze rejestru, aby otworzyć jego kod.

    Należy zauważyć, że kod źródłowy łączy znajduje się w  **\_LoginPartial.cshtml** plików, nie Index.cshtml ani \_Layout.cshtml, które są miejsca może wyglądać na pierwszym miejscu. Zostały zastosowane bezpośrednio w pliku poprawnego źródła.
3. W **style** , zlokalizuj i kliknij **<section> #login</section>** elementu, który jest kontenerem HTML dla tych łączy.

    Zwróć uwagę, że **#login** styl automatycznie znajduje się w **Site.css** po kliknięciu przycisku. Ponadto jest teraz wyróżniony kod.

    ![Wybieranie stylów CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "wybranie style CSS")

    *Wybieranie stylów CSS*
4. Usuń znaczniki komentarza **wyrównanie tekstu** atrybutu w wyróżniony kod przez usunięcie otwarcia i zamknięcia znaków i zapisać **Site.css** pliku.

    Narzędzie Page Inspector zna wszystkie pliki różnych tworzące bieżącej strony i wykrywa zmiany dowolnego z tych plików. Generuje alert w każdym przypadku, gdy nie jest zsynchronizowana z plików źródłowych bieżącej strony w przeglądarce.
5. W przeglądarce narzędzie Page Inspector kliknij pasek znajdujący się poniżej paska adresu, aby ponownie załadować stronę.

    ![Ponowne ładowanie strony](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Ponowne ładowanie strony*

    Łącza są teraz po prawej, ale nadal wyglądają listy punktowanej. Teraz spowoduje usunięcie punktory i wyrównanie w poziomie łącza przez użytkownika.

    ![Zaktualizowane strony](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Zaktualizowane strony*
6. Przy użyciu trybu inspekcji, wybierz dowolne **&lt;li&gt;** elementów, które zawierają &quot;zarejestrować&quot; i &quot;Zaloguj&quot; łącza. Następnie kliknij przycisk  **&lt;sekcji&gt; #login** element, aby uzyskać dostęp do **Styles.css** kodu.

    ![Znajdowanie styl](using-page-inspector-in-visual-studio-2012/_static/image19.png "znajdowanie styl")

    *Znajdowanie stylu*
7. W **Style.css**, usuń znaczniki komentarza kod **#login li** elementów. Styl, który dodajesz Ukryj punktor, a następnie Wyświetl elementy w poziomie.

    ![Restyling łącza logowania](using-page-inspector-in-visual-studio-2012/_static/image20.png "Restyling łącza logowania")

    *Restyling łącza logowania*
8. Zapisz **Style.css** plików i kliknij jeden raz na pasku znajduje się pod adresem ponowne załadowanie tej strony. Należy zauważyć, że łącza są wyświetlane poprawnie.

    ![Łącza wyrównany do prawej strony](using-page-inspector-in-visual-studio-2012/_static/image21.png "łącza wyrównany do prawej strony")

    *Łącza wyrównany do prawej strony*
9. Na koniec zostanie zmieniony tytuł nagłówka. W trybie inspekcji kliknij **tutaj znak logo** tekstu i get do kodu źródłowego, który generuje go.
10. Teraz znajdują się w  **\_Layout.cshtml**, zastąp "**tutaj znak logo**"tekst z"**galerii fotografii**". Zapisz i zaktualizuj przeglądarkę narzędzia Page Inspector.

    ![Przypisywanie nowy tytuł](using-page-inspector-in-visual-studio-2012/_static/image22.png "przypisywanie nowy tytuł")

    *Przypisywanie nowy tytuł*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Zaktualizowane strony galerii fotografii*
11. Na koniec wybierz **PhotoGallery** projektu i naciśnij klawisz **F5** do uruchomienia aplikacji. Sprawdź wszystkie zmiany działać zgodnie z oczekiwaniami.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Ćwiczenie 2: Za pomocą narzędzia Page Inspector w projektach formularzy sieci Web

W tym ćwiczeniu dowiesz się, jak wyświetlić podgląd i debugowanie rozwiązania formularzy sieci Web za pomocą narzędzia Page Inspector. Najpierw będzie wykonywać krótki laptop wokół narzędzie, aby dowiedzieć się więcej funkcji narzędzie Page Inspector, które ułatwiają debugowanie procesu sieci Web. Następnie będzie działać na stronie sieci web zawierający style problemy. Dowiesz się używania narzędzie Page Inspector można znaleźć kodu źródłowego, który generuje problemu i rozwiąż problem.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Zadanie 1 - eksploracji narzędzie Page Inspector

W tym zadaniu dowiesz się, jak używać funkcji narzędzie Page Inspector w kontekście projektu formularzy sieci Web, który zawiera galerii fotografii.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex2-WebForms/Begin/** folderu.

   1. Należy pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. W Eksploratorze rozwiązań, Znajdź **Default.aspx** strony, kliknij go prawym przyciskiem myszy i wybierz **widoku w narzędzie Page Inspector**.

    ![Otwieranie Default.aspx z narzędziem Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "otwierania Default.aspx z narzędziem Page Inspector")

    *Otwieranie Default.aspx z narzędziem Page Inspector*
3. Okno narzędzia Page Inspector będzie zawierać plik Default.aspx.

    ![Wyświetlanie Default.aspx w narzędzie Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image25.png "wyświetlania Default.aspx w narzędzie Page Inspector")

    *Wyświetlanie Default.aspx w narzędzie Page Inspector*

    Narzędzie narzędzie Page Inspector jest zintegrowana w środowisku Visual Studio. Inspektor zawiera osadzony przeglądarki, wraz z zaawansowanych profilera HTML, który będzie informować o zaznaczony kod. Należy zauważyć, że nie masz Aby uruchomić rozwiązanie, aby zobaczyć strony.

    > [!NOTE]
    > Gdy szerokość przeglądarkę narzędzia Page Inspector jest mniejsza niż szerokość otwarta strona, nie zobaczysz stronę poprawnie. Jeśli tak się stanie, należy dostosować szerokość narzędzie Page Inspector.
4. Kliknij przycisk **pliki** kartę w narzędzie Page Inspector.

    Zostanie wyświetlone wszystkie pliki źródłowe, które są składających się na renderowanej stronie domyślnej. Jest to funkcja przydatna do identyfikowania wszystkie elementy w skrócie, szczególnie w przypadku, gdy użytkownik pracuje z kontrolek użytkownika i stron wzorcowych. Należy zauważyć, że można również przejść do każdego z plików.

    ![Karta pliki](using-page-inspector-in-visual-studio-2012/_static/image26.png "plikach — karta")

    *Karta pliki*
5. Kliknij przycisk **Przełącz tryb inspekcji** przycisk znajdujący się po lewej stronie kart.

    To narzędzie umożliwia wybierz dowolny element strony i wyświetlić kod i .aspx źródła HTML.

    ![Przycisk przełączania trybu inspekcji](using-page-inspector-in-visual-studio-2012/_static/image27.png "przycisk przełączania trybu inspekcji")

    *Przycisk przełączania trybu inspekcji*
6. W przeglądarce narzędzie Page Inspector kursorem myszy elementy na stronie. Podczas przesuwania wskaźnika myszy nad dowolną część renderowanej strony wyświetlany jest typ elementu, a odpowiedni kod źródłowy lub kod zostanie wyróżniona w edytorze programu Visual Studio.

    ![Tryb inspekcji w akcji](using-page-inspector-in-visual-studio-2012/_static/image28.png "trybu inspekcji, w akcji")

    *Tryb inspekcji w akcji*

    > [!NOTE]
    > Nie Maksymalizuj okno narzędzia Page Inspector lub nie będzie widoczna na karcie Podgląd przedstawiający kodu źródłowego. Po kliknięciu elementu w narzędzie Page Inspector jest maksymalizacji, zostanie wyświetlony kod źródłowy zaznaczenia, ale spowoduje ukrycie okna narzędzia Page Inspector.

    Jeśli użytkownik należy zwrócić uwagę na **Default.aspx** plików, można zauważyć, że zostanie wyróżniona części kodu źródłowego, który generuje wybranego elementu. Ta funkcja umożliwia wersji plików źródłowych długie, dzięki czemu bezpośredniego i szybki dostęp do kodu.

    ![Zapoznanie się elementy](using-page-inspector-in-visual-studio-2012/_static/image29.png "sprawdzania elementów")

    *Zapoznanie się elementy*
7. Kliknij przycisk **Przełącz tryb inspekcji** przycisk (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.] (using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), znajdujące się na kartach narzędzie Page Inspector, aby wyłączyć kursora.
8. Wybierz **HTML** kartę, aby wyświetlić kod HTML renderowane w przeglądarce narzędzie Page Inspector.
9. W kodzie HTML Znajdź element listy z łączem Koala i zaznacz je.

    Zauważ, że po wybraniu kod odpowiednie dane wyjściowe jest automatycznie wyróżnione przeglądarki. Ta funkcja jest przydatna zobaczyć, jak jest renderowany blok HTML na stronie.

    ![Wybranie elementu HTML na stronie](using-page-inspector-in-visual-studio-2012/_static/image31.png "zaznaczenie elementu HTML na stronie")

    *Wybranie elementu HTML na stronie*
10. Kliknij przycisk **Przełącz tryb inspekcji** przycisk, aby włączyć *trybu inspekcji* i kliknij na pasku nawigacji. Po prawej stronie kodu HTML, w okienku style zostanie wyświetlona lista przy użyciu stylów CSS stosowany do wybranego elementu.

    > [!NOTE]
    > ponieważ nagłówka jest częścią układu witryny, narzędzie Page Inspector również otworzyć plik Site.Master i zaznacz segment kodu, których to dotyczy.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "odnajdywanie stylów i pliki źródłowe zaznaczonego elementu")

    *Odnajdywanie stylów i pliki źródłowe zaznaczonego elementu*
11. Wskaźnikiem Przełącz inspekcji włączone Przenieś wskaźnik myszy poniżej paska menu, a następnie kliknij przycisk pusty okrąg połowa.

    ![Wybranie elementu](using-page-inspector-in-visual-studio-2012/_static/image33.png "zaznaczenie elementu")

    *Wybranie elementu*
12. W okienku style zlokalizować **obraz tła** elementu w obszarze **.main zawartości** grupy. **Usuń zaznaczenie pola wyboru** **obraz tła** i zobacz, co się stanie. Można zauważyć, że przeglądarki zmienienia natychmiast i okręgu jest ukryty.

    > [!NOTE]
    > Zmiany, które należy zastosować na karcie Strona inspektora style nie wpływają na oryginalnym arkusza stylów. Możesz usunąć zaznaczenie pola wyboru style lub zmieniać tyle razy, ale będzie można przywrócić po odświeżeniu strony.

    ![Włączanie i wyłączanie CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "Włączanie i wyłączanie stylów CSS")

    *Włączanie i wyłączanie stylów CSS*
13. Teraz, kliknij przycisk "**Twojego** **logo tutaj"** tekst w nagłówku przy użyciu trybu inspekcji.
14. W **style** zlokalizuj **rozmiar czcionki** atrybutu CSS w obszarze **.site tytuł** grupy. Kliknij dwukrotnie ten atrybut raz do edycji jej wartości. Zamień 2.3em wartości z **3em**, a następnie naciśnij klawisz ENTER. Zwróć uwagę, że tytuł wygląda większy.

    ![Zmiana wartości CSS w Inspector2 strony](using-page-inspector-in-visual-studio-2012/_static/image35.png "wartości zmiana CSS w narzędzie Page Inspector")

    *Zmiana wartości CSS w narzędzie Page Inspector*
15. Kliknij przycisk **style śledzenia** kartę, znajduje się w okienku po prawej stronie narzędzie Page Inspector. Jest to alternatywny sposób, aby wyświetlić wszystkie style, które są stosowane do zaznaczenia, uporządkowanych według nazwy atrybutu.

    ![Śledzenie style CSS wybranego elementu](using-page-inspector-in-visual-studio-2012/_static/image36.png "śledzenie style CSS wybranego elementu")

    *Śledzenie style CSS wybranego elementu*
16. Inna funkcja narzędzie Page Inspector to okienko układu. Przy użyciu trybu inspekcji, wybierz na pasku nawigacyjnym, a następnie kliknij przycisk **układu** karty w okienku po prawej stronie. Zostanie wyświetlone rozmiarze wybranego elementu, a także rozmiaru przesunięcie, margines i Wypełnienie obramowania. Należy zauważyć, że wartości w tym widoku można również zmodyfikować.

    ![Elementu układu w narzędzie Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image37.png "układu elementu w narzędzie Page Inspector")

    *Elementu układu w narzędzie Page Inspector*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Zadanie 2 — Znajdowanie i rozwiązywanie problemów stylu w Galerii fotografii

Jak można zdiagnozować problemy stron sieci Web z poprzednich wersji programu Visual Studio? Jesteś prawdopodobnie znane z narzędzia debugowania, które są uruchomione spoza programu Visual Studio IDE, takich jak narzędzi deweloperskich programu Internet Explorer lub Firebug sieci web. Przeglądarki tylko zrozumieć HTML, skryptów i stylów, podczas gdy podstawowej struktury generuje kod HTML, który będzie renderowany. Z tego powodu często konieczne wdrażanie całej lokacji, aby zobaczyć, jak wyglądają stron sieci web.

Przy Wykryj i napraw problem w witrynie sieci web ma prawdopodobnie zostały wykonane następujące kroki:

1. Uruchom rozwiązania z programu Visual Studio, albo wdrożyć strony na serwerze sieci web.
2. W przeglądarce otwórz narzędzi deweloperskich używanie lub po prostu otworzyć kodu źródłowego i style i podjąć próbę dopasowania problem. Znajdź pliki związane, możesz korzystać z &quot;wyszukiwania&quot; lub &quot;wyszukiwania w plikach&quot; funkcji o nazwie klasy stylów.
3. Po wykryciu błąd zatrzymania przeglądarki sieci Web a serwerem.
4. Wyczyścić pamięć podręczną przeglądarki.
5. Wróć do programu Visual Studio, aby zastosować poprawkę. Powtórz kroki, aby przetestować.

Jako nie ma nie rzeczywistym WYSIWYG w formularzy sieci Web ASP.NET, niektóre problemy dotyczące stylu są wykrywane na późniejszym etapie, po uruchomiona lub wdrażania. Teraz z narzędziem Page Inspector jest możliwe do podglądu dowolną stronę bez uruchamiania rozwiązania.

W tym zadaniu użyjesz narzędzie Page inspector ustalania problemy aplikacji galerii fotografii. W poniższych krokach wykryje i szybko rozwiązać problem niektóre style proste w nagłówku.

1. Za pomocą kontroli strony zlokalizować **zarejestrować** i **dziennika w** linki po lewej stronie nagłówka.

    Należy zauważyć, że łącze nie jest wyświetlany w oczekiwanym miejscu po prawej stronie. Teraz zostanie Dopasuj łącza po prawej stronie i restyle go odpowiednio.

    ![Zaloguj się za łącze znajduje się po lewej stronie](using-page-inspector-in-visual-studio-2012/_static/image38.png "logowania łącze znajduje się po lewej stronie")

    *Dziennik w łącze znajduje się po lewej stronie*
2. Przełącz wybrane w trybie inspekcji wybierz łącze Zaloguj, aby otworzyć jego kod.

    Należy zauważyć, że kod źródłowy łącze znajduje się w **Site.Master** pliku, na stronie Default.aspx, która jest miejscem, może wyglądać na pierwszym miejscu; możesz znajdować się bezpośrednio w pliku poprawnego źródła.
3. W **style** , zlokalizuj i kliknij  **&lt;sekcji&gt; #login** elementu, który jest kontenerem HTML dla tych łączy.

    Zwróć uwagę, że **#login** styl automatycznie znajduje się w **Site.css** po kliknięciu przycisku. Ponadto jest teraz wyróżniony kod.

    ![Wybieranie stylów CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "wybranie style CSS")

    *Wybieranie stylów CSS*
4. Usuń znaczniki komentarza **wyrównanie tekstu** atrybutu w wyróżniony kod przez usunięcie otwarcia i zamknięcia znaków i zapisać **Site.css** pliku.

    Narzędzie Page Inspector zna wszystkie pliki różnych tworzące bieżącej strony i wykrywa zmiany dowolnego z tych plików. Generuje alert w każdym przypadku, gdy nie jest zsynchronizowana z plików źródłowych bieżącej strony w przeglądarce.
5. W przeglądarce narzędzie Page Inspector kliknij pasek znajdujący się poniżej paska adresu, aby zapisać zmiany i ponownie załaduj stronę.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Ponowne ładowanie strony*

    Łącza są teraz po prawej, ale nadal wyglądają listy punktowanej. Teraz spowoduje usunięcie punktory i wyrównanie w poziomie łącza przez użytkownika.

    ![Zaktualizowane strony](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Zaktualizowane strony*
6. Przy użyciu trybu inspekcji, wybierz dowolne **&lt;li&gt;** elementów, które zawierają &quot;zarejestrować&quot; i &quot;Zaloguj&quot; łącza. Następnie kliknij przycisk  **&lt;sekcji&gt; #login** element, aby uzyskać dostęp do **Styles.css** kodu.

    ![Znajdowanie styl](using-page-inspector-in-visual-studio-2012/_static/image42.png "znajdowanie styl")

    *Znajdowanie stylu*
7. W **Style.css**, usuń znaczniki komentarza kod **#login li** elementów. Styl, który dodajesz Ukryj punktor, a następnie Wyświetl elementy w poziomie.

    ![Restyling łącza logowania](using-page-inspector-in-visual-studio-2012/_static/image43.png "Restyling łącza logowania")

    *Restyling łącza logowania*
8. Zapisz **Style.css** plików i kliknij jeden raz na pasku znajduje się pod adresem ponowne załadowanie tej strony. Należy zauważyć, że łącza są wyświetlane poprawnie.

    ![Łącza wyrównany do prawej strony](using-page-inspector-in-visual-studio-2012/_static/image44.png "łącza wyrównany do prawej strony")

    *Łącza wyrównany do prawej strony*
9. Na koniec zostanie zmieniony tytuł nagłówka. Zamiast wyszukiwanie "**tutaj znak logo"** tekstu w plikach w trybie inspekcji kliknij tekst i uzyskać dostęp do kodu źródłowego, który generuje go.

    ![Znajdowanie nazwy lokacji](using-page-inspector-in-visual-studio-2012/_static/image45.png "znajdowanie tytuł witryny")

    *Znajdowanie nazwy lokacji*
10. Teraz znajdują się w **Site.Master**, zastąp "**tutaj znak logo**"tekst z"**galerii fotografii**". Zapisz i zaktualizuj przeglądarkę narzędzia Page Inspector.

    ![Zaktualizowane strony galerii fotografii](using-page-inspector-in-visual-studio-2012/_static/image46.png "strony galerii fotografii zaktualizowane")

    *Zaktualizowane strony galerii fotografii*
11. Na koniec naciśnij **F5** do uruchomienia aplikacji wyewidencjonowanie wszystkie zmiany działać zgodnie z oczekiwaniami.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Wykonując tego laboratorium Hands-On ma doświadczeń jak narzędzie Page Inspector umożliwia podgląd aplikacji sieci Web bez konieczności ponownie skompilować i uruchomić witrynę sieci Web w przeglądarce. Ponadto zostały wcześniej jak szybko znaleźć i naprawić błędy uzyskując bezpośrednio z przetworzonych wyników do kodu źródłowego.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Instalowanie programu Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "instalacji programu Visual Studio Express")

    *Instalowanie programu Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express for Web kafelka*
