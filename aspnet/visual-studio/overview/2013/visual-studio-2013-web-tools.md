---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: "Wskazówki laboratorium: Narzędzia sieci Web 2013 Visual Studio | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Visual Studio to środowisko rozwoju znakomity. SIEĆ systemu Windows i projekty sieci web. Obejmuje on łatwo służący do edytora tekstu zaawansowane..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Wskazówki laboratorium: Narzędzia sieci Web 2013 Visual Studio
====================
przez [obozów sieci Web Team](https://twitter.com/webcamps)

[Pobierz obozów sieci Web uczenie Kit](http://aka.ms/webcamps-training-kit)

> Visual Studio to środowisko rozwoju znakomity. SIEĆ systemu Windows i projekty sieci web. Obejmuje on edytora tekstów zaawansowanych łatwo służący do edytowania plików autonomicznych bez projektu.
> 
> Visual Studio zachowuje drzewo analizy oferujący wszystkie funkcje edycji każdego pliku. Dzięki temu Visual Studio, aby zapewnić bezkonkurencyjne autouzupełniania i czynności na podstawie dokumentu jednocześnie środowisko programistyczne znacznie szybsze i bardziej przyjemne. Te funkcje są szczególnie zaawansowanych w dokumentach HTML i CSS.
> 
> Wszystkie tego uprawnienia jest również dostępny do rozszerzenia, dzięki czemu można łatwo rozszerzyć edytory nowe zaawansowane funkcje, w zależności od potrzeb. Podstawowe informacje dotyczące sieci Web jest kolekcją rozszerzeń programu Visual Studio (przeważnie) związanych z sieci web. Zawiera wiele nowych funkcji IntelliSense zakończeń (szczególnie w przypadku CSS), nowe funkcje łącze przeglądarki, automatyczne JSHint JavaScript pliki nowego ostrzeżenia dla HTML, CSS i wiele innych funkcji, które są niezbędne do tworzenia nowoczesnych witryn sieci web.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktycznego przedstawiono sposób:

- Korzystać z nowych funkcji edytora HTML zawarte w Essentials sieci Web, takich jak rozbudowanych wstawek kodu HTML5 i Zen kodowania
- Korzystać z nowych funkcji Edytor CSS zawarte w Essentials sieci Web, takich jak próbnika kolorów i etykietka narzędzia macierzy przeglądarki
- Korzystać z nowych funkcji edytora JavaScript zawarte w sieci Web Essentials, takich jak wyodrębniania do pliku i technologii IntelliSense dla wszystkich elementów HTML
- Wymiany danych między przeglądarką a Visual Studio przy użyciu Browser Link

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Poniżej jest wymagany do ukończenia tego laboratorium praktycznego:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) lub nowszej
- [Essentials sieci Web 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

Aby można było uruchomić ćwiczeń opisanych w tym laboratorium praktycznego, należy najpierw skonfigurować środowiska.

1. Otwórz okno Eksploratora Windows i przejdź do laboratorium **źródła** folderu.
2. Kliknij prawym przyciskiem myszy **Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który skonfigurujesz środowisko i zainstalować wstawki kodu programu Visual Studio dla tego laboratorium.
3. Jeśli wyświetlane jest okno dialogowe kontroli konta użytkownika, potwierdź tę akcję, aby kontynuować.

> [!NOTE]
> Upewnij się, że zaznaczono wszystkie zależności dla tego laboratorium przed uruchomieniem Instalatora.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Korzystania z wstawek kodu

W dokumencie laboratorium należy poinstruować do wstawienia bloków kodu. Dla wygody większość tego kodu jest dostępna w Visual Studio wstawki kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązania, znajdujących się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdej wykonywania niezależnie od innych. Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać do czasu wykonywania. W kodzie źródłowym wykonywania można również znaleźć **zakończenia** folderu zawierającego rozwiązanie Visual Studio z kodem, który powoduje wykonanie czynności opisane w odpowiedniej wykonywania. Jeśli potrzebujesz dodatkowej pomocy w pracy za pośrednictwem tego laboratorium praktycznego, można użyć tych rozwiązań jako wskazówki.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To laboratorium praktycznego obejmuje następujących czynnościach:

1. [Praca z łącze przeglądarki i Essentials sieci Web](#Exercise1)
2. [Dzięki funkcji IntelliSense i wstawki kodu](#Exercise2)

> [!NOTE]
> Przy pierwszym uruchomieniu programu Visual Studio, musisz wybrać jeden z wstępnie zdefiniowanych ustawień kolekcji. Każda kolekcja wstępnie zdefiniowanych zaprojektowano w celu stylem rozwoju i określa układów okien, zachowanie edytora wstawki kodu IntelliSense i opcje w oknach dialogowych. Procedury przedstawione w tym laboratorium opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólne ustawienia środowiska deweloperskiego** kolekcji. Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Ćwiczenie 1: Praca z łącze przeglądarki i Essentials sieci Web

**Sieci Web Essentials** to rozszerzenie programu Visual Studio, które dodaje różne funkcje przydatne do tworzenia nowoczesnych witryn sieci web, przede wszystkim koncentruje się na wprowadzenie środowisko programistyczne web znacznie szybsze i bardziej przyjemne. Podstawowe informacje dotyczące sieci Web można zainstalować z galerii rozszerzeń programu Visual Studio.

**Łącze przeglądarki** to nowa funkcja uwzględnione w programie Visual Studio 2013, która udostępnia kanał między środowiska IDE programu Visual Studio i otwórz przeglądarki do wymiany danych między aplikacji sieci web i Visual Studio. Podstawowe informacje dotyczące sieci Web rozszerza łączy przeglądarki z narzędziami do modyfikowania obiektu modelu DOM i style CSS stron sieci web bezpośrednio z przeglądarki.

W tym ćwiczeniu zostanie Eksploruj niektóre funkcje obsługiwane przez **Web Essentials** i **łącze przeglądarki** ulepszyć stronę kwizu proste.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Zadanie 1 - uruchamiania projektu w wielu przeglądarkach

W tym zadaniu skonfiguruj aplikację sieci web do uruchamiania w różnych przeglądarkach, które jest przydatna przy testowaniu różnych przeglądarkach.

1. Otwórz **programu Microsoft Visual Studio**.
2. W **pliku** menu, wybierz opcję **Otwórz | Projekt/rozwiązanie...**  i przejdź do **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** w **źródła** folderu laboratorium (C:\WebCampsTK\HOL\VSWebTooling\Source). Wybierz **Begin.sln** i kliknij przycisk **Otwórz**.
3. Na pasku narzędzi programu Visual Studio rozwiń menu przeglądarki i wybierz **przeglądanie za pomocą...** .

    ![Przeglądanie za pomocą opcji menu](visual-studio-2013-web-tools/_static/image1.png "Przeglądaj z menu przeglądarki")

    *Przeglądanie za pomocą opcji menu*
4. W **przeglądanie za pomocą** okno dialogowe, wybierz **Google Chrome** i **programu Internet Explorer** , przytrzymując **CTRL** klucza, a następnie kliknij przycisk  **Ustaw jako domyślny**.

    ![Przeglądaj okna dialogowego](visual-studio-2013-web-tools/_static/image2.png "Przeglądaj okna dialogowego")

    *Wybieranie wielu domyślną przeglądarką*
5. Zarówno Google Chrome, jak i przeglądarki Internet Explorer powinien zostać wyświetlony jako domyślnej przeglądarki. Kliknij przycisk **anulować** aby zamknąć okno dialogowe.

    ![Google Chrome i przeglądarki Internet Explorer jako domyślnej przeglądarki](visual-studio-2013-web-tools/_static/image3.png "domyślnej przeglądarce Google Chrome i przeglądarki Internet Explorer")

    *Google Chrome i przeglądarki Internet Explorer jako domyślnej przeglądarki*

    > [!NOTE]
    > Po skonfigurowaniu domyślną przeglądarką **wielu przeglądarek** wybrano opcję w menu przeglądarki.
    > 
    > ![Wiele przeglądarek](visual-studio-2013-web-tools/_static/image4.png "wielu przeglądarek")
6. Naciśnij klawisz **CTRL** + **F5** do uruchomienia aplikacji bez debugowania.
7. Po otwarciu oba okna przeglądarki, umieść jeden z nich nad drugim w celu wyświetlenia aktualizacji w przeglądarkach obu jednocześnie. Strony sieci web z prostokąt blue jasny powinien być wyświetlany przeglądarek.

    ![Wprowadzenie do przeglądarki jeden nad drugim](visual-studio-2013-web-tools/_static/image5.png "umieszczenie przeglądarki jeden nad drugim")

    *Wprowadzenie do przeglądarki jeden nad drugim*
8. Nie zamykaj przeglądarek. Użyjesz ich w następnego zadania.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Zadanie 2 — Zen przy użyciu kodowania do tworzenia elementów HTML

**Kodowanie Zen** jest edytorem kodowania dodatek plug-in dla szybkich HTML, XML, XSL (lub innego formatu strukturalny) i edytowania. Podstawowe ten dodatek jest aparat skrót zaawansowanych, dzięki czemu można rozwinąć wyrażeń — podobnie jak selektory CSS — kod HTML. Kodowanie Zen jest szybkim sposobem zapisu składni selektor stylu HTML za pomocą CSS.

W tym ćwiczeniu funkcję kodowania Zen podał Essentials sieci Web użyje do określenia przyciski HTML, które reprezentują opcje pytania.

1. Przełącz się do programu Visual Studio.
2. Otwórz **Index.cshtml** plik znajdujący się w **widoków** | **Home** folderu.
3. Zastąp  **&lt;!--TODO: Dodaj tutaj--opcje&gt;**  komentarza z następującego kodu i naciśnij klawisz **kartę**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Kod powinny być rozwijane w formacie HTML.

    ![Rozszerzona HTML](visual-studio-2013-web-tools/_static/image6.png "rozwinięty HTML")

    *Rozwinięte HTML*

    > [!NOTE]
    > Aby dowiedzieć się więcej o składni Zen kodowania, zobacz następujące tematy [artykułu](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Kliknij przycisk **odświeżyć połączonej przeglądarki** przycisk, aby zaktualizować obie przeglądarki.

    ![Odśwież połączonej przeglądarki](visual-studio-2013-web-tools/_static/image7.png "odświeżyć połączonej przeglądarki")

    *Odśwież połączonej przeglądarki*

    ![Internet Explorer - aktualizacji strony z czterech przycisków](visual-studio-2013-web-tools/_static/image8.png "programu Internet Explorer — strona aktualizowana z czterech przycisków")

    *Internet Explorer - aktualizacji strony z czterech przycisków*

    ![Google Chrome — strona aktualizowana z czterech przycisków](visual-studio-2013-web-tools/_static/image9.png "Google Chrome — strona aktualizowana z czterech przycisków")

    *Google Chrome — strona aktualizowana z czterech przycisków*
6. Przełącz się do programu Visual Studio.
7. Przyciski ma być dodany do strony, ale nadal konieczne jest dodanie pytania szablonu. Aby to zrobić, użyjesz nową funkcją w Essentials sieci Web o nazwie **generator Lorem Ipsum**. Zlokalizuj **div** element z **klasy** atrybutu **przodu**.
8. Dodaj następujący kod jako pierwszy element podrzędny **div**i naciśnij klawisz **kartę**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Kod powinny być rozwijane w formacie HTML.

    ![Automatycznie wygenerowany Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum wygenerowana automatycznie")

    *Automatycznie wygenerowany Lorem Ipsum*

    > [!NOTE]
    > W ramach Zen kodowania można teraz wygenerować kod Lorem Ipsum bezpośrednio w edytorze HTML. Po prostu wpisz **lorem** i trafień **kartę** i 30 word Lorem Ipsum zostanie wstawiony tekst. Np. *lorem10* wstawia 10 Lorem Ipsum słów.
10. Logo, które zostaną dodane w górnej części zapytania przy użyciu kolejną nową funkcją w Essentials sieci Web o nazwie **generator pikseli Lorem**. Dodaj następujący kod jako pierwszy element podrzędny **div** element z **kontenera** jako **klasy** wartości, a następnie naciśnij klawisz **kartę**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Kod należy rozszerzyć w formacie HTML.

    ![Wygenerowany automatycznie pikseli Lorem](visual-studio-2013-web-tools/_static/image11.png "wygenerowana automatycznie Lorem pikseli")

    *Automatycznie wygenerowany Lorem pikseli*

    > [!NOTE]
    > W ramach Zen kodowania można również generować kod pikseli Lorem bezpośrednio w edytorze HTML. Po prostu wpisz **pix-200 x 200-zwierząt** i trafień **kartę** i **img** tagu z obrazem 200 x 200 zwierzęcia zostanie wstawiony. Aby uzyskać więcej informacji, zapoznaj się [pikseli Lorem](http://www.lorempixel.com).
12. Kliknij przycisk **odświeżyć połączonej przeglądarki** przycisk, aby zaktualizować obie przeglądarki.

    ![Internet Explorer - automatycznie wygenerowany obraz i tekst](visual-studio-2013-web-tools/_static/image12.png "programu Internet Explorer - automatycznie wygenerowany obraz i tekst")

    *Internet Explorer - automatycznie wygenerowany obraz i tekst*

    ![Google Chrome — automatycznie wygenerowany obraz i tekst](visual-studio-2013-web-tools/_static/image13.png "Google Chrome — automatycznie wygenerowany obraz i tekst")

    *Google Chrome — automatycznie wygenerowany obraz i tekst*

    > [!NOTE]
    > Ponieważ obraz jest wybierane losowo podczas dodawania fragment kodu, obraz wyświetlany w przeglądarkach mogą się różnić.
13. Nie zamykaj przeglądarek. Użyjesz ich w następnego zadania.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Zadanie 3 — aktualizowanie właściwości stylu

W tym zadaniu użyje łącze przeglądarki **trybu inspekcji** funkcji wykrywania dokładnej lokalizacji, w którym jest generowany określonego elementu DOM, a następnie zaktualizuj właściwości kolor tego elementu próbnika kolorów pochodzącymi z sieci Web Essentials.

1. W przeglądarce Internet Explorer, naciśnij klawisz **CTRL** + **ALT** + **I** Aby włączyć tryb inspekcji.
2. Wskaźnika na światła niebieskie obramowanie, a następnie kliknij przycisk.

    ![Przesunięcie kursora nad światła niebieskie obramowanie](visual-studio-2013-web-tools/_static/image14.png "przesunięciu wskaźnika na granicy światła niebieski")

    *Przesunięcie kursora nad światła niebieskie obramowanie*
3. Przełącz się do programu Visual Studio. Zwróć uwagę, jak również wybrano element HTML, który wybrano w przeglądarce w edytorze programu Visual Studio HTML.

    ![Element HTML zaznaczonego w edytorze programu Visual Studio HTML](visual-studio-2013-web-tools/_static/image15.png "zaznaczonego w edytorze programu Visual Studio HTML elementu HTML")

    *Element HTML zaznaczonego w edytorze programu Visual Studio HTML*
4. Teraz zaktualizuje **przodu** klasy CSS, aby można było zmienić style wybranego elementu. Aby to zrobić, naciśnij klawisz **CTRL** + **,** otworzyć **przejdź do** pola wyszukiwania. Typ **site.css** i naciśnij klawisz **ENTER** można otworzyć pliku.

    ![Otwieranie pliku Site.css](visual-studio-2013-web-tools/_static/image16.png "otwierania pliku Site.css")

    *Otwieranie pliku Site.css*
5. Naciśnij klawisz **CTRL** + **F** i typ **.front .flip container** można znaleźć selektora CSS.
6. Kliknij światła niebieski kwadrat we właściwości obramowania klasy, aby otworzyć próbnika kolorów.

    ![Otwieranie próbnika kolorów](visual-studio-2013-web-tools/_static/image17.png "otwierania próbnika kolorów")

    *Otwieranie próbnika kolorów*
7. Rozwiń próbnika kolorów, klikając przycisk i wybierz nowy kolor.

    ![Rozszerzanie próbnika kolorów](visual-studio-2013-web-tools/_static/image18.png "rozszerzania próbnika kolorów")

    *Rozszerzanie próbnika kolorów*
8. Naciśnij klawisz **CTRL** + **ALT** + **ENTER** można odświeżyć połączonej przeglądarki.
9. Przełącz się do programu Internet Explorer i zwróć uwagę, jak został zmieniony kolor obramowania.

    ![Internet Explorer - kolor obramowania zaktualizowane](visual-studio-2013-web-tools/_static/image19.png "programu Internet Explorer — kolor obramowania zaktualizowane")

    *Internet Explorer - kolor obramowania zaktualizowane*
10. Przełącz do Google Chrome i zwróć uwagę, jak został zmieniony kolor obramowania.

    ![Google Chrome — kolor obramowania zaktualizowane](visual-studio-2013-web-tools/_static/image20.png "Google Chrome — kolor obramowania zaktualizowane")

    *Google Chrome — kolor obramowania zaktualizowane*
11. Przełącz się do programu Visual Studio.
12. Przejdź do końca **Site.css** plik i naciśnij przycisk **CTRL** + **F** zlokalizować **.btn** selektora.
13. Zwróć uwagę, że **- webkit-border-radius** właściwości jest podkreślone na zielono.

    ![Właściwość - webkit-border-radius selektora btn](visual-studio-2013-web-tools/_static/image21.png "- webkit-border-radius właściwości selektora btn")

    *Właściwość - webkit-border-radius selektora btn*
14. Umieść punkt wstawiania w **- webkit-border-radius** właściwości. Niebieski linii powinny być wyświetlane w obszarze pierwszą literę słowa pierwszej właściwości. Jest to **tagów inteligentnych**.
15. Naciśnij klawisz **CTRL** + **.** Aby otworzyć menu sugestie i kliknij przycisk **Dodawanie właściwości standardowych (border-radius)**.

    ![Dodaj brakujące sugestię standardowe właściwości](visual-studio-2013-web-tools/_static/image22.png "Dodaj Brak sugestii właściwości standardowych")

    *Dodaj brakujące sugestię właściwości standardowych*
16. **Border-radius** właściwość jest automatycznie dodawany do reguły CSS.

    ![Brak właściwości standardowe dodane](visual-studio-2013-web-tools/_static/image23.png "dodane Brak właściwości standardowych")

    *Brak właściwości standardowe dodane*
17. Przesuń wskaźnik myszy nad **border-radius** właściwość, aby wyświetlić **tooltip macierzy przeglądarki**. **Tooltip macierzy przeglądarki** przedstawia dostępność właściwości w każdej przeglądarki.

    ![Etykietka narzędzia macierzy przeglądarki](visual-studio-2013-web-tools/_static/image24.png "tooltip macierzy przeglądarki")

    *Etykietka narzędzia macierzy przeglądarki*
18. Zwróć uwagę, że wartość **border-radius** właściwość jest nadal podkreślony. Wskaźnika na wartość, aby wyświetlić komunikat ostrzegawczy.

    ![Ostrzeżenie wartość właściwości border-radius](visual-studio-2013-web-tools/_static/image25.png "ostrzeżenie wartość właściwości Border-radius")

    *Ostrzeżenie wartość właściwości border-radius*
19. Usuń jednostki **border-radius** wartość właściwości zgodnie z sugestią podaną przez etykietkę narzędzia.
20. Jako **border-radius** jest standardowe właściwości do definiowania obramowania jak zaokrąglone narożniki są, możesz usunąć **- webkit-border-radius** właściwości i wartości z reguły CSS.
21. Umieść punkt wstawiania w **zawijanie** właściwości i zwróć uwagę, że tagów inteligentnych również pojawia się poniżej.
22. Otworzyć menu, a następnie kliknij przycisk **Podaj szczegóły dostawcy Brak**.

    ![Dodaj brakujące sugestię szczegóły dostawcy](visual-studio-2013-web-tools/_static/image26.png "Dodaj brakujące sugestię szczegóły dostawcy")

    *Dodaj brakujące sugestię szczegóły dostawcy*
23. **-Ms-zawijanie** właściwość jest automatycznie dodawany do reguły CSS.

    ![Dostawcy określoną właściwość dodane](visual-studio-2013-web-tools/_static/image27.png "dostawcy dodać określoną właściwość.")

    *Właściwość określonego dostawcy dodane*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Zadanie 4. aktualizowanie kodu HTML z przeglądarki

W tym zadaniu użyje łącze przeglądarki **tryb projektowania** funkcji do edycji obiektu DOM z przeglądarki i przetransferować zmiany wprowadzone do pliku źródła HTML programu Visual Studio.

1. Google Chrome, naciśnij klawisz **CTRL** + **ALT** + **D** Aby włączyć tryb projektowania.
2. Przesuń wskaźnik myszy nad **Lorem Ipsum dolor sit amet** etykiety, a następnie kliknij przycisk.

    ![Edytowanie pytania](visual-studio-2013-web-tools/_static/image28.png "Edytowanie pytania")

    *Edytowanie pytania*
3. Powinna zostać wyświetlona kursora. Zastąp oryginalny tekst z *jak go wygląda po napisać dłużej pytanie?*, a następnie naciśnij klawisz **ESC** aby zakończyć tryb projektowania.

    ![Pytanie edytować](visual-studio-2013-web-tools/_static/image29.png "edytować pytanie")

    *Pytanie edytować*
4. Przełącz do programu Visual Studio i Otwórz **Index.cshtml**, jeśli nie jest jeszcze otwarty. Należy zauważyć, że tekst wewnętrzny z  **&lt;p&gt;**  element został zaktualizowany.

    ![Pytanie zaktualizowane na stronie HTML](visual-studio-2013-web-tools/_static/image30.png "pytanie zaktualizowane strony HTML")

    *Zaktualizowano pytanie na stronie HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Zadanie 5 - recenzowania optymalizacji dla aparatów wyszukiwania związane z ostrzeżeniami

**Optymalizacji dla aparatów wyszukiwania** (SEO) to proces polegający na wprowadzaniu wyższej pozycję witryny sieci Web na liście wyników z wyszukiwarki. Im większa jest lokacji i bardziej spójnie to wymienione, więcej odwiedzających lokacji pobierze z tego aparatu wyszukiwania. Essentials sieci Web zawiera narzędzie analityczne, który sprawdza HTML, znaleziono raporty problemów i zapewnia pomoc, aby je rozwiązać.

1. Przejdź do **widoku** menu i kliknij przycisk **listy błędów** otworzyć **listy błędów** okna.

    ![Błąd w widoku listy menu](visual-studio-2013-web-tools/_static/image31.png "listy błędów w menu Widok")

    *Błąd w widoku listy menu*
2. Zwróć uwagę, że istnieje ostrzeżenie optymalizacji dla aparatów wyszukiwania z informacją, że  **&lt;meta&gt;**  tagu dla Brak opisu strony. Kliknij dwukrotnie wpis ostrzeżenie optymalizacji dla aparatów wyszukiwania, aby go rozwiązać.

    ![Okno listy błędów](visual-studio-2013-web-tools/_static/image32.png "w oknie Lista błędów")

    *Okno listy błędów*
3. W **Web Essentials** okno dialogowe, kliknij przycisk **tak** do wstawienia opis &lt;meta&gt; tagu.

    ![Okno dialogowe Essentials Web](visual-studio-2013-web-tools/_static/image33.png "Essentials sieci Web — okno dialogowe")

    *Okno dialogowe Essentials sieci Web*
4. Edytor dla  **\_Layout.cshtml** otwiera i  **&lt;meta&gt;**  tag jest automatycznie dodawany do **head** sekcji Plik HTML.

    ![Tag meta automatycznie dodane na stronie _Layout](visual-studio-2013-web-tools/_static/image34.png "metatag automatycznie dodane w _Layout strony")

    *Tag meta automatycznie dodawane do \_układ strony*
5. Zmień wartość **zawartości** atrybutu *GeekQuiz* i Zapisz plik.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Ćwiczenie 2: Korzystanie z IntelliSense i wstawki kodu

Z sieci Web Essentials edytora HTML został rozszerzony o dodatkowe funkcje. W tym ćwiczeniu zostanie wyświetlona niektóre nowe funkcje, które są przydatne podczas tworzenia aplikacji sieci web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Zadanie 1 — za pomocą funkcji IntelliSense w dokumentach HTML

Pierwszy nowa funkcja pojawi się w ramach tego zadania jest nazywany **dynamiczne IntelliSense**. Dynamiczne IntelliSense odczytuje inne znaczniki i atrybuty do wywnioskowania możliwych identyfikatorów, które będą używane.

W ramach tego zadania spowoduje utworzenie nowego elementu formularza HTML, który zawiera etykiety i pola wejściowego. Następnie dodasz **dla** atrybutu do etykiety, aby powiązać go z danych wejściowych, i zobaczysz sugestie IntelliSense oparte na identyfikatory danych wejściowych w zakresie.

1. Otwórz **programu Visual Studio Express 2013 for Web** i **Begin.sln** rozwiązania, znajdujących się w **źródło/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folderu. Alternatywnie możesz kontynuować z rozwiązaniem uzyskanymi w poprzednim ćwiczeniu.
2. W **Eksploratora rozwiązań**, otwórz **Index.cshtml** plik znajdujący się w **widoków** | **Home** folderu.
3. Dodaj następującą postać wewnątrz  **&lt;sekcji&gt;**  elementu.

    (Fragment - kodu *VisualStudio2013WebTooling* - *Ex2* - *formularz*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Etykieta z niektórych opis pola powinien być poprzedzony tagu wejściowego. Dodaj następujące etykiety przed tagu wejściowego.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. **Dla** atrybutu  **&lt;etykiety&gt;**  Określa, który element formularza etykietę jest powiązany. Wartość atrybutu powinna być równa identyfikator elementu pokrewne. Dodaj **dla** atrybutu  **&lt;etykiety&gt;**  elementu. Jak pokazano na poniższej ilustracji, &quot;nazwa&quot; wartość pojawia się w polu IntelliSense, na podstawie identyfikatora elementów w tym samym zakresie (otaczający  **&lt;formularza&gt;**).

    ![Wyświetlanie w IntelliSense identyfikator](visual-studio-2013-web-tools/_static/image35.png "zawierające identyfikator w IntelliSense")

    *Wyświetlanie w IntelliSense identyfikator*
6. Usuń ostatnio dodany  **&lt;formularza&gt;**  elementu i jego zawartości.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Zadanie 2 — przy użyciu fragmentów kodu HTML

HTML5 wprowadzić więcej niż 25 nowych znaczników semantyki. Visual Studio już obsługę funkcji IntelliSense na te tagi, ale programu Visual Studio 2013 pozwala szybciej i łatwiej można zapisać znacznika przez dodanie nowych fragmentów kodu. Chociaż te tagi nie są skomplikowane, pochodzą z kilku precyzyjnie małych, takie jak dodanie przejścia poprawnego kodera-dekodera dla *audio* tagu. W tym zadaniu zobaczysz wstawki kodu HTML dla tagu audio.

1. W **Index.cshtml** plików, wpisz  **&lt;lub** wewnątrz  **&lt;sekcji&gt;**  element, jak pokazano na poniższej ilustracji.

    ![Wstawianie audio element](visual-studio-2013-web-tools/_static/image36.png "Wstawianie audio element")

    *Wstawianie audio element*
2. Naciśnij klawisz **kartę** dwa razy i zwróć uwagę, jak poniższy kod zostanie dodany na stronie i znajduje się kursor na **src** atrybutu pierwszego źródła.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Naciskając **kartę** klucza dwukrotnie wstawieniu fragmentu kodu. Audio fragment kodu przedstawia standardowego użycia *audio* tag o dwa pliki źródłowe ulepszoną obsługę.
3. Usuń drugi wiersz i aktualizowanie źródła pierwszego wiersza następujące łącze, aby WebCampsTV Katana Pokaz: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Poniżej przedstawiono wynikowy kod.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Plik źródłowy *KatanaProject.mp3* służy jako przykład. Jeśli wolisz, możesz użyć innego źródła.
4. Naciśnij klawisz **CTRL** + **S** można zapisać pliku.
5. Naciśnij klawisz **CTRL** + **F5** do uruchomienia aplikacji.
6. Należy zauważyć, że odtwarzacz audio została dodana do aplikacji.

    ![Odtwarzacz audio w programie Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player w programie Internet Explorer")

    *Odtwarzacz audio w programie Internet Explorer*

    ![Odtwarzacz audio w przeglądarce Google Chrome](visual-studio-2013-web-tools/_static/image38.png "odtwarzacz Audio w przeglądarce Google Chrome")

    *Odtwarzacz audio w przeglądarce Google Chrome*
7. Nie zamykaj przeglądarek. Użyjesz ich w następnego zadania.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Zadanie 3 — za pomocą funkcji IntelliSense w dokumentach JavaScript

W przypadku sieci Web 2013 Essentials HTML strony i arkusze stylów drukować listę identyfikatorów i nazw klas. W tym zadaniu dowiesz się, jak te listy poprawić obsługę funkcji IntelliSense języka JavaScript w sieci Web 2013 Essentials.

1. W **Index.cshtml** pliku, Dodaj następujący kod, aby zdefiniować **skryptu** tag dla kodu JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Dodaj następujący kod wewnątrz **skryptu** tag, aby zdefiniować funkcję wywołania zwrotnego gotowe.

    (Fragment - kodu *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Umieść punkt wstawiania w **skryptu** tagu i naciśnij klawisz **CTRL** + **.** Aby otworzyć menu uwag.
4. Kliknij przycisk **wyodrębnić pliku**.

    ![Wyodrębnij JavaScript do pliku sugestii](visual-studio-2013-web-tools/_static/image39.png "JavaScript wyodrębnić sugestią pliku")

    *Wyodrębnij JavaScript do pliku sugestii*
5. W **Zapisz jako** wybierz **skryptów** folderu, nazwa pliku **init.js** i kliknij przycisk **zapisać**.

    ![Zapisz jako okno](visual-studio-2013-web-tools/_static/image40.png "okno Zapisz jako")

    *Okno Zapisz jako*

    > [!NOTE]
    > **Init.js** plik jest tworzony i zawartość skryptu zostanie przeniesiony do pliku.
    > 
    > ![Utworzone za pomocą zawartość pliku init.js](visual-studio-2013-web-tools/_static/image41.png "Init.js pliku utworzonego za pomocą zawartość")
    > 
    > *Init.js pliku utworzonego za pomocą zawartość*
6. Otwórz **Index.cshtml** plik i sprawdź, czy tag skryptu został zastąpiony w odniesieniu do **init.js** pliku.

    ![Odwołanie html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js odwołania html")

    *Odwołanie do init.js html*
7. Przejdź do **Eksploratora rozwiązań** i zwróć uwagę, że **init.js** pliku została automatycznie uwzględniona w rozwiązaniu.

    ![Plik init.js zawartych w rozwiązaniu](visual-studio-2013-web-tools/_static/image43.png "pliku Init.js zawartych w rozwiązaniu")

    *Plik init.js zawartych w rozwiązaniu*
8. Przywraca **init.js** plik, aby zaktualizować **gotowe** funkcji wywołania zwrotnego.
9. W definicji funkcji wywołania zwrotnego, przekazywany do *gotowe*, Dodaj następujący kod, aby pobrać wszystkie elementy przez atrybut określonej klasy.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Naciśnij klawisz **CTRL** + **miejsca** ująć w cudzysłów wewnątrz **getElementsByClassName** wywołania funkcji.

    ![Wyświetlanie funkcji IntelliSense dla funkcji getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "przedstawiający IntelliSense dla funkcji getElementsByClassName")

    *Wyświetlanie funkcji IntelliSense dla funkcji getElementsByClassName*

    > [!NOTE]
    > Należy zauważyć, że IntelliSense pokazuje klas zdefiniowanych w arkuszach stylów projektu.
11. Zastąp wiersz utworzony w następującym kodem.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Umieść kursor po **au** wewnątrz cudzysłowów w **getElementsByTagName** funkcji i naciśnij klawisz **CTRL** + **miejsca**.

    ![Wyświetlanie funkcji IntelliSense dla metody getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "przedstawiający IntelliSense dla metody getElementByTagName")

    *Wyświetlanie funkcji IntelliSense dla metody getElementsByTagName*
13. Wybierz  **&quot;audio&quot;**  z listy i kliknij **ENTER**. Wynik jest wyświetlany na poniższej ilustracji.

    ![Pobieranie elementów Audio](visual-studio-2013-web-tools/_static/image46.png "pobierania elementów Audio")

    *Pobieranie elementów Audio*
14. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **init.js** w pliku **skryptów** i wybierz polecenie **pliki JavaScript zminimalizowania** z **Web Essentials** menu.

    ![Zminimalizowania pliki JavaScript](visual-studio-2013-web-tools/_static/image47.png "pliki zminimalizowania JavaScript")

    *Zminimalizowania pliki JavaScript*
15. Po wyświetleniu monitu, aby włączyć automatyczne minimalizowanie kliknięcie zmiany pliku źródłowego **tak**.

    ![Włączanie automatycznego minimalizację ostrzeżenie](visual-studio-2013-web-tools/_static/image48.png "włączenie automatycznego minimalizację ostrzeżenie")

    *Włączanie automatycznego minimalizację ostrzeżenie*

    > [!NOTE]
    > **Init.min.js** zostanie utworzona i nie zostanie dodany jako zależność z **init.js** pliku.
    > 
    > ![Utworzony plik init.min.js](visual-studio-2013-web-tools/_static/image49.png "Init.min.js plik utworzony")
    > 
    > *Utworzony plik init.min.js*
16. Otwórz **init.min.js** pliku i zwróć uwagę, że plik jest zminimalizowany.

    ![Zawartość pliku init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js pliku zawartości")

    *Zawartość pliku init.min.js*
17. W **init.js** pliku, Dodaj następujący kod poniżej **getElementsByTagName** wywołania funkcji do odtwarzania audio elementów.

    (Fragment - kodu *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Kliknij przycisk **CTRL** + **S** można zapisać pliku. Ponieważ zminimalizowany plik jest już otwarty, pojawi się okno dialogowe z informacją, że plik został zmodyfikowany poza edytorem źródła. Kliknij przycisk **Tak**.

    ![Ostrzeżenie programu Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "ostrzeżenie programu Microsoft Visual Studio")

    *Ostrzeżenie programu Microsoft Visual Studio*
19. Przywraca **init.min.js** plik, aby sprawdzić, czy plik został zaktualizowany o nowy kod.

    ![Zaktualizowany plik init.min.js](visual-studio-2013-web-tools/_static/image52.png "zaktualizowanego pliku Init.min.js")

    *Zaktualizowany plik init.min.js*
20. Kliknij przycisk **Odśwież łącze przeglądarki** przycisku.
21. Gdy obie przeglądarki są odświeżane odtwarzacze audio, który został wyświetlony w poprzednim zadaniu rozpocznie się automatyczne odtwarzanie.

    ![Odtwarzacz audio zawarte w widoku](visual-studio-2013-web-tools/_static/image53.png "odtwarzacz Audio zawarte w widoku")

    *Odtwarzacz audio zawarte w widoku*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Wykonując tego laboratorium praktycznego wiesz już, jak:

- Korzystać z nowych funkcji edytora HTML zawarte w Essentials sieci Web, takich jak rozbudowanych wstawek kodu HTML5 i Zen kodowania
- Korzystać z nowych funkcji Edytor CSS zawarte w Essentials sieci Web, takich jak próbnika kolorów i etykietka narzędzia macierzy przeglądarki
- Korzystać z nowych funkcji edytora JavaScript zawarte w sieci Web Essentials, takich jak wyodrębniania do pliku i technologii IntelliSense dla wszystkich elementów HTML
- Wymiany danych między przeglądarką a Visual Studio przy użyciu Browser Link
