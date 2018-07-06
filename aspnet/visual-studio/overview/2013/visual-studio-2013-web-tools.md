---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Warsztaty: Visual Studio 2013 w sieci Web Tools | Dokumentacja firmy Microsoft'
author: rick-anderson
description: Visual Studio to środowisko doskonałymi metodami tworzenia oprogramowania. Na podstawie NET Windows oraz projekty sieci web. Zawiera on edytorem tekstu zaawansowane, która może być bez problemów używany na...
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 9553d3129ff4c942eacbba628d1daaf6c4e33635
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807531"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Warsztaty: Visual Studio 2013 w sieci Web Tools
====================
Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](http://aka.ms/webcamps-training-kit)

> Visual Studio to środowisko doskonałymi metodami tworzenia oprogramowania. Na podstawie NET Windows oraz projekty sieci web. Obejmuje ona edytorem tekstu zaawansowane, która może być bez problemów używany do edytowania plików autonomicznych bez projektu.
> 
> Program Visual Studio przechowuje drzewo analizy w pełni funkcjonalne w trakcie edycji każdego pliku. Dzięki temu program Visual Studio zapewniają niezrównaną automatycznego uzupełniania i czynności na podstawie dokumentu podczas dokonywania środowisko programistyczne, znacznie szybszy i bardziej przyjemny. Funkcje te są szczególnie wydajne w dokumentach HTML i CSS.
> 
> Wszystkie te możliwości jest również dostępna dla rozszerzenia, dzięki czemu można łatwo rozszerzyć edytorów oraz zaawansowanych nowych funkcji, zgodnie z potrzebami. Web Essentials to kolekcja rozszerzeń programu Visual Studio (najczęściej) związane z sieci web. Zawiera on też wiele nowych uzupełnianiu IntelliSense (szczególnie w przypadku CSS), nowe funkcje łączność z przeglądarkami, automatyczne plików JSHint dla języka JavaScript, nowe ostrzeżenia dla HTML, CSS i wiele innych funkcji, które są niezbędne do tworzenia nowoczesnej sieci web.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym praktyczne laboratorium dowiesz się jak:

- Korzystać z nowych funkcji edytora HTML objęte Web Essentials, takich jak rozbudowanych wstawek kodu HTML5 i Zen kodowania
- Nowe funkcje edytora CSS uwzględnione w Web Essentials, takich jak selektor kolorów i etykietkę narzędzia macierzy przeglądarki
- Korzystać z nowych funkcji edytora JavaScript zawartych w Web Essentials, takich jak Wyodrębnianie pliku, a funkcja IntelliSense dla wszystkich elementów HTML
- Dane programu Exchange między przeglądarką i Visual Studio za pomocą łączność z przeglądarkami

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego laboratorium praktycznego niezbędne jest, następujące elementy:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) lub nowszej
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

Aby można było uruchomić ćwiczeń opisanych w tym praktyczne laboratorium, należy najpierw skonfigurować swoje środowisko.

1. Otwórz okno Eksploratora Windows i przejdź do laboratorium **źródła** folderu.
2. Kliknij prawym przyciskiem myszy **plik Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który będzie skonfigurować środowisko i zainstalować fragmenty kodu programu Visual Studio, w tym środowisku laboratoryjnym.
3. Jeśli zostanie wyświetlone okno dialogowe kontroli konta użytkownika, upewnij się, działania, aby kontynuować.

> [!NOTE]
> Upewnij się, że wszystkie zależności w tym środowisku laboratoryjnym sprawdzeniu przed uruchomieniem Instalatora.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Za pomocą fragmentów kodu

W dokumencie laboratorium należy poinstruować można wstawiać bloki kodu. Dla wygody większość ten kod jest dostarczany jako Visual Studio fragmenty kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązanie znajduje się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdego wykonywania niezależnie od innych. Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać, dopóki nie zakończysz wykonywania. Wewnątrz kodu źródłowego dla ćwiczenia, można również znaleźć **zakończenia** folderu zawierającego rozwiązania programu Visual Studio z kodem, który powstały na skutek wykonaniu kroków w odpowiedniej wykonywania. Jeśli potrzebujesz dodatkowej pomocy, gdy pracujesz za pośrednictwem tego laboratorium praktycznego, można użyć jako wskazówki dotyczące tych rozwiązań.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To ćwiczenie praktyczne obejmuje następujących czynnościach:

1. [Praca z przeglądarkami i Web Essentials](#Exercise1)
2. [Korzystając z zalet IntelliSense i fragmentów kodu](#Exercise2)

> [!NOTE]
> Przy pierwszym uruchomieniu programu Visual Studio, należy wybrać jedną z kolekcji wstępnie zdefiniowanych ustawień. Każda kolekcja wstępnie zdefiniowanych służy do dopasowywania style rozwoju i określa układy okna, zachowanie edytora, fragmenty kodu IntelliSense i opcje w oknach dialogowych. Procedury przedstawione w tym środowisku laboratoryjnym opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólnych ustawieniach projektowych** kolekcji. Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Ćwiczenie 1: Praca z przeglądarkami i Web Essentials

**Web Essentials** to rozszerzenie programu Visual Studio, który dodaje różne funkcje przydatne do tworzenia nowoczesnej sieci web, koncentrując się głównie na środowisko programistyczne w sieci web, które znacznie szybszy i bardziej przyjemny. Web Essentials można zainstalować z galerii rozszerzeń w programie Visual Studio.

**Łączność z przeglądarkami** to nowa funkcja zawarte w Visual Studio 2013, która udostępnia kanał między środowiska IDE programu Visual Studio oraz otwartą przeglądarką wymiany danych między aplikacją sieci web i programu Visual Studio. Rozszerzenia Web Essentials wzbogacają łączność z przeglądarkami za pomocą narzędzi do manipulowania model obiektu modelu DOM i style CSS stron sieci web bezpośrednio z przeglądarki.

W tym ćwiczeniu zostanie Poznaj niektóre z funkcji obsługiwanych przez **Web Essentials** i **łączność z przeglądarkami** Aby ulepszyć stronę prosty test.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Zadanie 1 — Uruchamianie projektu w różnych przeglądarkach

W tym zadaniu skonfigurujesz aplikację sieci web do uruchamiania w wielu przeglądarkach jednocześnie, która jest przydatna przy testowaniu przeglądarek.

1. Otwórz **programu Microsoft Visual Studio**.
2. W **pliku** menu, wybierz opcję **Open | Projekt/rozwiązanie...**  i przejdź do **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** w **źródła** folderu laboratorium (C:\WebCampsTK\HOL\VSWebTooling\Source). Wybierz **Begin.sln** i kliknij przycisk **Otwórz**.
3. Na pasku narzędzi programu Visual Studio rozwiń menu przeglądarki, a następnie wybierz **przeglądanie za pomocą...** .

    ![Przeglądanie za pomocą opcji menu](visual-studio-2013-web-tools/_static/image1.png "Przeglądaj za pomocą menu przeglądarki")

    *Przeglądanie za pomocą opcji menu*
4. W **przeglądanie za pomocą** okno dialogowe, wybierz **Google Chrome** i **programu Internet Explorer** , przytrzymując **CTRL** klucza, a następnie kliknij przycisk  **Ustaw jako domyślny**.

    ![Przeglądaj okna dialogowego](visual-studio-2013-web-tools/_static/image2.png "Przeglądaj okna dialogowego")

    *Wybieranie wielu domyślnych przeglądarek*
5. Program Internet Explorer i Google Chrome teraz powinny się wyświetlać jako domyślną przeglądarką. Kliknij przycisk **anulować** aby zamknąć okno dialogowe.

    ![Google Chrome i przeglądarki Internet Explorer jako domyślnej przeglądarki](visual-studio-2013-web-tools/_static/image3.png "domyślnych przeglądarek Google Chrome i przeglądarki Internet Explorer")

    *Google Chrome i przeglądarki Internet Explorer jako domyślnej przeglądarki*

    > [!NOTE]
    > Po skonfigurowaniu domyślną przeglądarką **wiele przeglądarek** wybrano opcję w menu przeglądarki.
    > 
    > ![Wiele przeglądarek](visual-studio-2013-web-tools/_static/image4.png "wiele przeglądarek")
6. Naciśnij klawisz **CTRL** + **F5** Aby uruchomić aplikację bez debugowania.
7. Po otwarciu oba okna przeglądarki, umieść jeden z nich nad drugim umożliwiający zobaczenie aktualizacje w przeglądarkach dla obu jednocześnie. Przeglądarki powinien być wyświetlany na stronie sieci web z obrazie, gdzie jasnoniebieski prostokąt.

    ![Wprowadzenie do przeglądarki jeden nad drugim](visual-studio-2013-web-tools/_static/image5.png "umieszczenie przeglądarkę jeden nad drugim")

    *Wprowadzenie do przeglądarki jeden nad drugim*
8. Nie zamykaj przeglądarki. Użyjesz ich w ramach następnego zadania.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Zadanie 2 — przy użyciu Zen programowania do tworzenia elementów HTML

**Kodowanie Zen** jest edytorem kodowania wtyczki dla szybkich HTML, XML, XSL (lub innych formatów kodu ze strukturą) i edytowania. Podstawowe Ta wtyczka jest aparat zaawansowane skrót, który umożliwia rozwinięcie wyrażeń — podobnie jak selektorów CSS - kod HTML. Kodowanie Zen jest możliwość szybkiego zapisu HTML za pomocą CSS stylu selektor składni.

W tym ćwiczeniu użyje funkcji Zen kodowania, dostarczone przez Web Essentials do generowania przyciski HTML, które reprezentują opcje pytania.

1. Przełącz się do programu Visual Studio.
2. Otwórz **Index.cshtml** plik znajdujący się w **widoków** | **Home** folderu.
3. Zastąp **&lt;!--TODO: Dodaj tutaj — opcje&gt;** komentarz z poniższej kod i naciśnij klawisz **kartę**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Kod powinien rozwinięte w formacie HTML.

    ![Rozwinięte HTML](visual-studio-2013-web-tools/_static/image6.png "rozwinięte HTML")

    *Rozwinięty HTML*

    > [!NOTE]
    > Aby dowiedzieć się więcej na temat kodowania Zen składni, zapoznaj się z poniższymi [artykułu](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Kliknij przycisk **Odśwież połączone przeglądarki** przycisk, aby zaktualizować obie przeglądarki.

    ![Odśwież połączone przeglądarki](visual-studio-2013-web-tools/_static/image7.png "Odśwież połączone przeglądarki")

    *Odśwież połączone przeglądarki*

    ![Internet Explorer — strona aktualizowana z czterema przyciskami](visual-studio-2013-web-tools/_static/image8.png "programu Internet Explorer — strona aktualizowana z czterema przyciskami")

    *Internet Explorer — strona aktualizowana z czterema przyciskami*

    ![Google Chrome — strona aktualizowana z czterema przyciskami](visual-studio-2013-web-tools/_static/image9.png "Google Chrome — strona aktualizowana z czterema przyciskami")

    *Google Chrome — strona aktualizowana z czterema przyciskami*
6. Przełącz się do programu Visual Studio.
7. Przyciski zostały dodane do strony, ale nadal musisz dodać pytanie szablonu. Aby to zrobić, użyjesz nową funkcją w sieci Web Essentials o nazwie **generator Lorem Ipsum**. Znajdź **div** element z **klasy** atrybut **front**.
8. Dodaj następujący kod jako pierwszy element podrzędny elementu **div**i naciśnij klawisz **kartę**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Kod powinien rozwinięte w formacie HTML.

    ![Lorem Ipsum wygenerowany automatycznie](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum wygenerowany automatycznie")

    *Lorem Ipsum wygenerowany automatycznie*

    > [!NOTE]
    > W ramach Zen kodowania można teraz wygenerować Lorem Ipsum kodu bezpośrednio w edytorze HTML. Po prostu wpisz **lorem** i kliknij przycisk **kartę** i 30 word Lorem Ipsum, tekst zostanie wstawiony. Np. *lorem10* wstawia 10 słów Lorem Ipsum.
10. W górnej części zapytania dodasz logo przy użyciu kolejną nową funkcją w sieci Web Essentials o nazwie **generator pikseli Lorem**. Dodaj następujący kod jako pierwszy element podrzędny elementu **div** element z **kontenera** jako **klasy** wartości, a następnie naciśnij klawisz **kartę**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Kod powinni rozwinąć pozycję w formacie HTML.

    ![Wygenerowany automatycznie pikseli Lorem](visual-studio-2013-web-tools/_static/image11.png "wygenerowany automatycznie Lorem pikseli")

    *Wygenerowany automatycznie Lorem pikseli*

    > [!NOTE]
    > W ramach Zen kodowania można również wygenerować pikseli Lorem kodu bezpośrednio w edytorze HTML. Po prostu wpisz **programu pix-200 x 200-zwierzęta** i kliknij przycisk **kartę** i **img** tag z obrazem 200 x 200 zwierzęcia zostanie wstawiony. Aby uzyskać więcej informacji, zobacz [pikseli Lorem](http://www.lorempixel.com).
12. Kliknij przycisk **Odśwież połączone przeglądarki** przycisk, aby zaktualizować obie przeglądarki.

    ![Internet Explorer — automatycznie wygenerowany obraz i tekst](visual-studio-2013-web-tools/_static/image12.png "programu Internet Explorer — automatycznie wygenerowany obraz i tekst")

    *Internet Explorer — automatycznie wygenerowany obraz i tekst*

    ![Google Chrome — automatycznie wygenerowany obraz i tekst](visual-studio-2013-web-tools/_static/image13.png "Google Chrome — automatycznie wygenerowany obraz i tekst")

    *Google Chrome — automatycznie wygenerowany obraz i tekst*

    > [!NOTE]
    > Ponieważ obraz jest wybranych losowo podczas dodawania fragment kodu, obraz wyświetlany w przeglądarkach mogą się różnić.
13. Nie zamykaj przeglądarki. Użyjesz ich w ramach następnego zadania.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Zadanie 3 — aktualizowanie właściwości stylu

W tym zadaniu użyje Browser Link **sprawdzić tryb** funkcję, aby wykryć dokładną lokalizację, gdzie jest generowany określonego elementu DOM, a następnie zaktualizuj właściwość kolor elementu przy użyciu selektora kolorów, dostarczone przez sieci Web Podstawy.

1. W przeglądarce Internet Explorer, naciśnij klawisz **CTRL** + **ALT** + **I** Aby włączyć tryb inspekcji.
2. Przesuń wskaźnik nad światła niebieskie obramowanie, a następnie kliknij przycisk.

    ![Przesunięcie kursora nad światła niebieskie obramowanie](visual-studio-2013-web-tools/_static/image14.png "przesunięcie wskaźnika nad światła niebieskie obramowanie")

    *Przesunięcie kursora nad światła niebieskie obramowanie*
3. Przełącz się do programu Visual Studio. Zwróć uwagę, jak element HTML, który został wybrany w przeglądarce zostanie również wybrane w edytorze programu Visual Studio HTML.

    ![Element HTML zaznaczonego w edytorze programu Visual Studio HTML](visual-studio-2013-web-tools/_static/image15.png "HTML elementu zaznaczonego w edytorze programu Visual Studio HTML")

    *Element HTML zaznaczonego w edytorze programu Visual Studio HTML*
4. Teraz zmodyfikujemy **front** klasy CSS, aby zmienić stylu wybranego elementu. Aby to zrobić, naciśnij klawisz **CTRL** + **,** otworzyć **przejdź do** pola wyszukiwania. Typ **site.css** i naciśnij klawisz **ENTER** można otworzyć pliku.

    ![Otwieranie pliku Site.css](visual-studio-2013-web-tools/_static/image16.png "otwierania pliku Site.css")

    *Otwieranie pliku Site.css*
5. Naciśnij klawisz **CTRL** + **F** i typ **.front .flip container** można znaleźć selektora CSS.
6. Kliknij przycisk światła niebieskim kwadratem we właściwości klasy, aby otworzyć selektor kolorów obramowania.

    ![Otwieranie selektor kolorów](visual-studio-2013-web-tools/_static/image17.png "otworzyć selektor kolorów")

    *Otwieranie selektor kolorów*
7. Rozwiń selektor kolorów, klikając przycisk z podwójną strzałką i wybierz nowy kolor.

    ![Rozwijanie selektor kolorów](visual-studio-2013-web-tools/_static/image18.png "rozwijanie selektor kolorów")

    *Rozwijanie selektor kolorów*
8. Naciśnij klawisz **CTRL** + **ALT** + **ENTER** odświeżania podłączonych przeglądarek.
9. Przełącz się do programu Internet Explorer i zwróć uwagę, jak zmienił kolor obramowania.

    ![Internet Explorer — kolor obramowania zaktualizowane](visual-studio-2013-web-tools/_static/image19.png "programu Internet Explorer — kolor obramowania zaktualizowane")

    *Internet Explorer — kolor obramowania zaktualizowane*
10. Przełącz się do przeglądarki Google Chrome i zwróć uwagę, jak zmienił kolor obramowania.

    ![Google Chrome — kolor obramowania zaktualizowane](visual-studio-2013-web-tools/_static/image20.png "Google Chrome — kolor obramowania zaktualizowane")

    *Google Chrome — kolor obramowania zaktualizowane*
11. Przełącz się do programu Visual Studio.
12. Przejdź na koniec **Site.css** pliku i naciśnij klawisz **CTRL** + **F** zlokalizować **.btn** selektora.
13. Należy zauważyć, że **- webkit-border-radius** właściwość jest podkreślone w kolorze zielonym.

    ![Właściwość selektor btn - webkit-border-radius](visual-studio-2013-web-tools/_static/image21.png "właściwość selektor btn - webkit-border-radius")

    *Właściwość selektor btn - webkit-border-radius*
14. Umieść punkt wstawiania w **- webkit-border-radius** właściwości. Niebieska linia powinna zostać wyświetlona w obszarze pierwszą literę wyrazu pierwszej właściwości. Jest to **tagów inteligentnych**.
15. Naciśnij klawisz **CTRL** + **.** Aby otworzyć menu sugestie, a następnie kliknij przycisk **Dodawanie właściwości standardowych (border-radius)**.

    ![Dodaj brakujące sugestii standardowe właściwości](visual-studio-2013-web-tools/_static/image22.png "Dodaj brakujące sugestii właściwości standardowych")

    *Dodaj brakujące sugestii właściwości standardowe*
16. **Promień obramowania** reguły CSS jest automatycznie dodawana właściwość.

    ![Brak standardowych dodanie właściwości](visual-studio-2013-web-tools/_static/image23.png "Brak dodanie właściwości standardowe")

    *Brak dodanie właściwości standardowe*
17. Przesuń wskaźnik nad **promień obramowania** właściwość do wyświetlenia **etykietki narzędzia macierzy przeglądarki**. **Etykietki narzędzia macierzy przeglądarki** przedstawia dostępność właściwości w każdej przeglądarce.

    ![Etykietka narzędzia macierzy przeglądarki](visual-studio-2013-web-tools/_static/image24.png "etykietki narzędzia macierzy przeglądarki")

    *Etykietka narzędzia macierzy przeglądarki*
18. Należy zauważyć, że wartość **promień obramowania** właściwość jest nadal podkreślony. Przesuń wskaźnik nad wartość, aby zobaczyć komunikat ostrzegawczy.

    ![Ostrzeżenie wartość właściwości promień obramowania](visual-studio-2013-web-tools/_static/image25.png "ostrzeżenie wartość właściwości promień obramowania")

    *Ostrzeżenie wartość właściwości promień obramowania*
19. Usuń jednostkę **promień obramowania** wartość właściwości zgodnie z sugestią podaną w etykietce narzędzia.
20. Jako **promień obramowania** jest właściwością standardowego do definiowania obramowania jak zaokrąglone rogi są, możesz usunąć **- webkit-border-radius** właściwości i wartości z reguły CSS.
21. Umieść punkt wstawiania w **zawijanie wyrazów** właściwość i zwróć uwagę, że tagu inteligentnego również pojawia się poniżej.
22. Otwórz menu, a następnie kliknij przycisk **Dodaj brakujące szczegóły dostawcy**.

    ![Dodaj brakujące sugestii szczegóły dostawcy](visual-studio-2013-web-tools/_static/image26.png "Dodaj brakujące sugestii szczegóły dostawcy")

    *Dodaj brakujące sugestii szczegóły dostawcy*
23. **-Ms-zawijanie** reguły CSS jest automatycznie dodawana właściwość.

    ![Dodanie właściwości określonych dostawców](visual-studio-2013-web-tools/_static/image27.png "dodanie właściwości określonych dostawców")

    *Dodanie właściwości określonych dostawców*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Zadanie 4. aktualizowanie kodu HTML w przeglądarce

W tym zadaniu użyje Browser Link **trybu projektowania** funkcji do edycji obiektu DOM w przeglądarce i przesłać zmiany do pliku źródła HTML w programie Visual Studio.

1. Google Chrome, naciśnij **CTRL** + **ALT** + **D** Aby włączyć tryb projektowania.
2. Przesuń wskaźnik nad **Lorem Ipsum dolor sit amet** etykiety, a następnie kliknij przycisk.

    ![Edytowanie pytania](visual-studio-2013-web-tools/_static/image28.png "Edytowanie pytania")

    *Edytowanie pytania*
3. Kursor powinien zostać wyświetlony. Zastąp oryginalny tekst z *jak ona wygląda podczas zapisu czy dłużej pytanie?*, a następnie naciśnij klawisz **ESC** aby wyjść z trybu projektowania.

    ![Pytanie, edytować](visual-studio-2013-web-tools/_static/image29.png "edytować pytanie")

    *Edytować pytanie*
4. Przełącz do programu Visual Studio i Otwórz **Index.cshtml**, jeśli nie jest jeszcze otwarty. Należy zauważyć, że tekst zawarty wewnątrz **&lt;p&gt;** element został zaktualizowany.

    ![Zaktualizowano pytanie, na stronie HTML](visual-studio-2013-web-tools/_static/image30.png "pytanie zaktualizowane strony HTML")

    *Zaktualizowano pytanie, na stronie HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Zadanie 5 - recenzowania optymalizacji dla aparatów wyszukiwania związane z ostrzeżeniami

**Optymalizacja wyszukiwarek** (SEO) to proces polegający na wprowadzaniu wyższe rangę witryny sieci Web na liście wyników z wyszukiwarki. Im większa szereguje lokacji i skuteczniejsze ta opcja jest wyświetlana, więcej odwiedzających lokacji zostanie uzyskiwanie tego aparatu wyszukiwania. Web Essentials zawiera narzędzia analitycznego, który analizuje HTML, raporty problemy znalezione i zapewnia pomoc, aby to naprawić.

1. Przejdź do **widoku** menu i kliknij przycisk **lista błędów** otworzyć **lista błędów** okna.

    ![Błąd w widoku listy menu](visual-studio-2013-web-tools/_static/image31.png "lista błędów w menu Widok")

    *Błąd w widoku listy menu*
2. Należy zauważyć, że istnieje ostrzeżenie optymalizacji dla aparatów wyszukiwania z informacją, że **&lt;meta&gt;** tagu dla Brak opisu strony. Kliknij dwukrotnie wpis ostrzeżenie optymalizacji dla aparatów wyszukiwania, aby rozwiązać ten problem.

    ![Okno listy błędów](visual-studio-2013-web-tools/_static/image32.png "okno Lista błędów")

    *Okno listy błędów*
3. W **Web Essentials** okno dialogowe, kliknij przycisk **tak** do wstawienia opis &lt;meta&gt; tagu.

    ![Okno dialogowe usługi sieci Web Essentials](visual-studio-2013-web-tools/_static/image33.png "Web Essentials, okno dialogowe")

    *Okno dialogowe usługi sieci Web Essentials*
4. Edytor dla  **\_Layout.cshtml** otwiera i **&lt;meta&gt;** tag jest automatycznie dodawany do **head** sekcji Plik HTML.

    ![Tag meta automatycznie dodawane na stronie _układ](visual-studio-2013-web-tools/_static/image34.png "metatag automatycznie dodawane na stronie _układ")

    *Tag meta automatycznie dodawane do \_stronę układu*
5. Zmień wartość właściwości **zawartości** atrybutu *GeekQuiz* i Zapisz plik.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Ćwiczenie 2: Wykorzystanie IntelliSense i fragmentów kodu

Przy użyciu Web Essentials edytora HTML została rozszerzona o dodatkowe funkcje. W tym ćwiczeniu zobaczysz niektóre nowe funkcje, które są przydatne podczas tworzenia aplikacji sieci web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Zadanie 1 — za pomocą funkcji IntelliSense w dokumentach HTML

Pierwszy nowej funkcji, zostanie wyświetlony w ramach tego zadania jest nazywany **dynamiczne IntelliSense**. Dynamiczne IntelliSense odczytuje inne znaczniki i atrybuty w celu możliwych identyfikatorów, które będą używane.

W tym zadaniu zostanie utworzony nowy element formularza HTML, który zawiera etykiety i pola wejściowego. A następnie dodasz **dla** atrybutu do etykiety, aby powiązać go z danych wejściowych, a zobaczysz sugestie funkcji IntelliSense, w oparciu o identyfikatorach danych wejściowych w zakresie.

1. Otwórz **Visual Studio Express 2013 for Web** i **Begin.sln** rozwiązanie znajduje się w **/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/początkowy w źródle** folderu. Alternatywnie można kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.
2. W **Eksploratora rozwiązań**, otwórz **Index.cshtml** plik znajdujący się w **widoków** | **Home** folderu.
3. Dodaj następującą postać wewnątrz **&lt;sekcji&gt;** elementu.

    (Code Snippet — *VisualStudio2013WebTooling* - *Ex2* - *formularza*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Tagu wejściowego powinien być poprzedzony etykiety z niektórych opis pola. Dodaj następujące etykietę przed tagu wejściowego.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. **Dla** atrybutu **&lt;etykiety&gt;** Określa, który element formularza, a etykiety jest powiązany z. Wartość atrybutu powinna być równa identyfikator elementu powiązane. Dodaj **dla** atrybutu **&lt;etykiety&gt;** elementu. Jak pokazano na poniższej ilustracji &quot;nazwa&quot; wartość pojawia się w oknie funkcji IntelliSense, na podstawie identyfikatora elementów w obrębie tego samego zakresu (otaczający  **&lt;formularza&gt;**).

    ![Wyświetlanie identyfikatora w technologii IntelliSense](visual-studio-2013-web-tools/_static/image35.png "zawierające identyfikator w technologii IntelliSense")

    *Wyświetlanie identyfikatora w technologii IntelliSense*
6. Usuń ostatnio dodane **&lt;formularza&gt;** elementu i jego zawartości.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Zadanie 2 — przy użyciu fragmentów kodu HTML

HTML5 wprowadzono więcej niż 25 nowe znaczniki semantyczne. Program Visual Studio ma już obsługę funkcji IntelliSense na te tagi, ale Visual Studio 2013 umożliwia szybsze i prostsze do zapisania znaczników przez dodanie nowych fragmentów kodu. Chociaż te tagi nie są skomplikowane, pochodzą z kilku małych precyzyjnie, takie jak dodawanie planów awaryjnych poprawnego kodera-dekodera dla *audio* tagu. W tym zadaniu zostanie wyświetlony fragmenty kodu HTML dla tagu audio.

1. W **Index.cshtml** pliku, wpisz  **&lt;aud** wewnątrz **&lt;sekcji&gt;** elementu, jak pokazano na poniższej ilustracji.

    ![Wstawianie elementu audio](visual-studio-2013-web-tools/_static/image36.png "Wstawianie elementu audio")

    *Wstawianie elementu audio*
2. Naciśnij klawisz **kartę** dwa razy i zwróć uwagę, jak poniższy kod dodaje się na stronie, a znajduje się kursor na **src** atrybut pierwsze źródło.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Naciskając **kartę** klucza dwukrotnie wstawieniu fragmentu kodu. Audio fragment kodu przedstawia użycie standardowego *audio* tag z dwóch plików źródłowych dla Ulepszona obsługa.
3. Usuwanie drugi wiersz i aktualizowanie źródła pierwszy wiersz z następującego linku do pokazania WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Następnie kod wynikowy znajdują się poniżej.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Plik źródłowy *KatanaProject.mp3* służy jako przykład. Można użyć innego źródła, jeśli użytkownik sobie tego życzy.
4. Naciśnij klawisz **CTRL** + **S** można zapisać pliku.
5. Naciśnij klawisz **CTRL** + **F5** do uruchomienia aplikacji.
6. Należy zauważyć, że odtwarzacz audio została dodana do aplikacji.

    ![Odtwarzacz audio w programie Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "odtwarzacz Audio w programie Internet Explorer")

    *Odtwarzacz audio w programie Internet Explorer*

    ![Odtwarzacz audio w przeglądarce Google Chrome](visual-studio-2013-web-tools/_static/image38.png "odtwarzacz Audio w przeglądarce Google Chrome")

    *Odtwarzacz audio w przeglądarce Google Chrome*
7. Nie zamykaj przeglądarki. Użyjesz ich w ramach następnego zadania.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Zadanie 3 — za pomocą funkcji IntelliSense w dokumentach JavaScript

Za pomocą sieci Web Essentials 2013 arkusze stylów i strony HTML przedstawić listę identyfikatorów i nazw klas. W tym zadaniu dowiesz się, jak te listy poprawić obsługę funkcji IntelliSense języka JavaScript, w sieci Web Essentials 2013.

1. W **Index.cshtml** plików, Dodaj następujący kod, aby zdefiniować **skryptu** tag w przypadku kodu JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Dodaj następujący kod wewnątrz **skryptu** tag, aby zdefiniować funkcję wywołania zwrotnego gotowe.

    (Code Snippet — *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Umieść punkt wstawiania w **skryptu** tagu, a następnie naciśnij klawisz **CTRL** + **.** Aby otworzyć menu sugestię.
4. Kliknij przycisk **wyodrębnić pliku**.

    ![JavaScript Wyodrębnij plik sugestią](visual-studio-2013-web-tools/_static/image39.png "JavaScript wyodrębnić pliku sugestii")

    *JavaScript wyodrębnić pliku sugestii*
5. W **Zapisz jako** wybierz **skrypty** folder i nazwę pliku **init.js** i kliknij przycisk **Zapisz**.

    ![Zapisz jako okno](visual-studio-2013-web-tools/_static/image40.png "okno Zapisz jako")

    *Okno Zapisz jako*

    > [!NOTE]
    > **Init.js** plik zostanie utworzony i zawartość skryptu zostanie przeniesiony do pliku.
    > 
    > ![Plik init.js utworzony za pomocą dołączonej zawartości](visual-studio-2013-web-tools/_static/image41.png "Init.js plik utworzony za pomocą dołączonej zawartości")
    > 
    > *Plik init.js utworzony za pomocą dołączonej zawartości*
6. Otwórz **Index.cshtml** pliku i sprawdź, czy tag skryptu został zastąpiony w odniesieniu do **init.js** pliku.

    ![Odwołania html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js html odwołania")

    *Odwołania html init.js*
7. Przejdź do **Eksploratora rozwiązań** i zwróć uwagę, że **init.js** plik został automatycznie dołączony w rozwiązaniu.

    ![Plik init.js zawartych w rozwiązaniu](visual-studio-2013-web-tools/_static/image43.png "Init.js plików zawarte w rozwiązaniu")

    *Plik init.js zawartych w rozwiązaniu*
8. Przejdź z powrotem do **init.js** pliku do zaktualizowania **gotowe** funkcji wywołania zwrotnego.
9. Wewnątrz definicji funkcji wywołania zwrotnego, który jest przekazywany do *gotowe*, Dodaj następujący kod, aby pobrać wszystkie elementy w atrybucie określonej klasy.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Naciśnij klawisz **CTRL** + **miejsca** w cudzysłowie wewnątrz **getElementsByClassName** wywołania funkcji.

    ![Wyświetlanie funkcji IntelliSense dla funkcji getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "pokazywanie IntelliSense dla funkcji getElementsByClassName")

    *Wyświetlanie funkcji IntelliSense dla funkcji getElementsByClassName*

    > [!NOTE]
    > Należy zauważyć, że funkcja IntelliSense wyświetla klas zdefiniowanych w arkuszach stylów projektu.
11. Zastąp wiersz, utworzony za pomocą następującego kodu.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Umieść kursor po **au** w cudzysłowie w **getElementsByTagName** funkcji, a następnie naciśnij klawisz **CTRL** + **miejsca**.

    ![Wyświetlanie funkcji IntelliSense dla metody getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "pokazywanie IntelliSense dla metody getElementByTagName")

    *Wyświetlanie funkcji IntelliSense dla metody getElementsByTagName*
13. Wybierz **&quot;audio&quot;** z listy i kliknij **ENTER**. Wynik jest wyświetlany na poniższej ilustracji.

    ![Trwa pobieranie elementów Audio](visual-studio-2013-web-tools/_static/image46.png "podczas pobierania elementów Audio")

    *Trwa pobieranie elementów Audio*
14. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **init.js** w pliku **skrypty** i wybierz polecenie **pliki JavaScript zmniejszenie** z **Web Essentials** menu.

    ![Minimalizowanie pliki JavaScript](visual-studio-2013-web-tools/_static/image47.png "pliki zmniejszenie JavaScript")

    *Minimalizowanie pliki JavaScript*
15. Po wyświetleniu monitu, aby włączyć automatyczne minimalizowanie, po kliknięciu zmiany pliku źródłowego **tak**.

    ![Włączanie automatycznego minimalizację ostrzeżenie](visual-studio-2013-web-tools/_static/image48.png "włączenie automatycznego minimalizację ostrzeżenie")

    *Włączanie automatycznego minimalizację ostrzeżenie*

    > [!NOTE]
    > **Init.min.js** jest tworzony i zostanie dodany jako zależność z **init.js** pliku.
    > 
    > ![Utworzony plik init.min.js](visual-studio-2013-web-tools/_static/image49.png "utworzony plik Init.min.js")
    > 
    > *Utworzony plik init.min.js*
16. Otwórz **init.min.js** pliku i zwróć uwagę, że plik jest zminimalizowany.

    ![Zawartość pliku init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js zawartość pliku")

    *Zawartość pliku init.min.js*
17. W **init.js** plików, Dodaj następujący kod poniżej **getElementsByTagName** wywołania funkcji do odtwarzania dźwięku elementów.

    (Code Snippet — *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Kliknij przycisk **CTRL** + **S** można zapisać pliku. Ponieważ zminimalizowanego pliku jest już otwarty, zobaczysz okno dialogowe z informacją, że plik został zmodyfikowany poza edytorem źródła. Kliknij przycisk **Tak**.

    ![Ostrzeżenie programu Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "ostrzeżenie programu Microsoft Visual Studio")

    *Ostrzeżenie programu Microsoft Visual Studio*
19. Przejdź z powrotem do **init.min.js** plik, aby sprawdzić, czy plik został zaktualizowany o nowy kod.

    ![Zaktualizowano plik init.min.js](visual-studio-2013-web-tools/_static/image52.png "zaktualizowanego pliku Init.min.js")

    *Zaktualizowano plik init.min.js*
20. Kliknij przycisk **Odśwież łącze przeglądarki** przycisku.
21. Gdy obie przeglądarki są odświeżane odtwarzacze audio, które zostały użyte w poprzednim zadaniu rozpocznie się automatycznie odtwarzania.

    ![Odtwarzacz audio uwzględnione w widoku](visual-studio-2013-web-tools/_static/image53.png "odtwarzacz Audio uwzględnione w widoku")

    *Odtwarzacz audio uwzględnione w widoku*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Przez ukończenie tego laboratorium praktycznego wiesz już, jak:

- Korzystać z nowych funkcji edytora HTML objęte Web Essentials, takich jak rozbudowanych wstawek kodu HTML5 i Zen kodowania
- Nowe funkcje edytora CSS uwzględnione w Web Essentials, takich jak selektor kolorów i etykietkę narzędzia macierzy przeglądarki
- Korzystać z nowych funkcji edytora JavaScript zawartych w Web Essentials, takich jak Wyodrębnianie pliku, a funkcja IntelliSense dla wszystkich elementów HTML
- Dane programu Exchange między przeglądarką i Visual Studio za pomocą łączność z przeglądarkami
