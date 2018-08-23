---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: What's New in ASP.NET i Web Development w programie Visual Studio 2012 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Nowa wersja programu Visual Studio wprowadzono szereg rozszerzeń funkcjonalności skoncentrowane na ułatwieniu komfort użytkowania i wydajność podczas pracy z technologiami sieci Web...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 263a6e0aed51a681193333b53eff8f03847fc3aa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752288"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>What's New in ASP.NET i Web Development w programie Visual Studio 2012
====================
Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

> Nowa wersja programu Visual Studio wprowadzono wiele ulepszeń skoncentrowane na ułatwieniu komfort użytkowania i wydajność podczas pracy z technologiami sieci Web. Edytory wizualne Studio CSS, JavaScript i HTML został całkowicie odnowionych, obejmujący wiele pomocy kodu najbardziej w żądanie, takie jak IntelliSense i automatyczne wcięcia. Chodzi o wydajność tworzenie pakietów i minimalizowanie są teraz zintegrowane jak wbudowane funkcje, aby łatwo ograniczyć strony — czas ładowania.
> 
> Program Visual Studio umożliwia pracę z najnowszymi technologiami witryny sieci Web. Fragmenty kodu CSS3 przeglądarek umożliwia upewnij się, że witryna działa niezależnie od platformy klienta również korzystając równocześnie z zalet nowych elementów HTML5 i funkcje.
> 
> Zapisywanie i profilowanie kodu JavaScript powinny być łatwiejsze w przypadku tej wersji programu Visual Studio. Listy funkcji IntelliSense, zintegrowane funkcje języka XML dokumentacji i nawigacji są teraz dostępne dla kodu w języku JavaScript. Wykaz języka JavaScript jest teraz dostępna w zasięgu ręki. Ponadto sprawdź zgodność ECMAScript5 za pomocą skryptów i wykrywać błędy składniowe na wczesnym etapie.
> 
> Ostatnie, ale nie najmniej tej wersji programu Visual Studio implementuje wbudowane tworzenie pakietów i minimalizowanie. Pliki skryptów i arkusze stylów zostaną spakowane i skompresować tak, aby witryna wykonuje szybciej.
> 
> W tym laboratorium przeprowadzi Cię przez ulepszeń i nowych funkcji, które opisano wcześniej, stosując drobne zmiany do przykładowej aplikacji sieci Web, pod warunkiem w folderze źródłowym.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym warsztaty, dowiesz się jak:

- Użyj nowe funkcje i ulepszenia w edytorze CSS
- Użyj nowe funkcje i ulepszenia w edytorze HTML
- Użyj nowe funkcje i ulepszenia w edytorze języka JavaScript
- Konfigurowanie i używanie tworzenie pakietów i minimalizowanie

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).
- [Program Windows PowerShell](https://support.microsoft.com/kb/968930/) (dla skryptów Instalatora — już zainstalowanym systemem Windows 8 i Windows Server 2008 R2)
- [Program Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) — lub przeglądarki zgodnej z językiem HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

Ta sesja hands on lab obejmuje następujących czynnościach:

1. [Ćwiczenie 1: Co nowego w edytorze CSS](#Exercise1)
2. [Ćwiczenie 2: Co nowego w edytorze HTML](#Exercise2)
3. [Ćwiczenie 3: What's New in Edytor kodu JavaScript](#Exercise3)
4. [Ćwiczenie 4: Tworzenie pakietów i minimalizowanie](#Exercise4)

Szacowany czas do ukończenia tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Ćwiczenie 1: Co nowego w edytorze CSS

Deweloperzy sieci Web powinni zapoznać się z wielu trudności związane z CSS do edycji. To jeden z największych problemów style CSS w różnych przeglądarkach. Często zdarza się, że po stosowanie stylów do swojej witryny, można zauważyć, że wygląda inaczej Jeśli otworzysz go w innej przeglądarki lub urządzenia. W związku z tym może poświęcać wiele czasu na rozwiązywanie tych problemów visual sobie sprawę, że ostatecznie powoduje on działać w jednej przeglądarki, jest on dzielony na innych.

Visual Studio zawiera teraz funkcje, które ułatwiają deweloperom dostęp, praca i efektywnie organizować arkusze stylów CSS. W tym ćwiczeniu będzie spełniać nowe funkcje w wersji i skuteczne organizacji, a także CSS3 fragmenty kodu w różnych przeglądarkach.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Zadanie 1 — nowe funkcje edytora

W tym zadaniu wykryje nowe funkcje edytora CSS. Ten nowy edytor pomoże zwiększyć produktywność, wykorzystując nowe inteligentnych wcięć, komentarzy w kodzie ulepszone i rozszerzoną listę IntelliSense.

1. Rozpocznij **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązanie znajduje się w **Source\WhatsNewASPNET** folder w tym laboratorium.
2. W Eksploratorze rozwiązań Otwórz **Site.css** plik znajdujący się w obszarze **style** folderu. Upewnij się, że **edytora tekstów** narzędzia są widoczne na pasku narzędzi. Aby to zrobić, wybierz **widoku** | **pasków narzędzi** opcji menu i sprawdź **edytora tekstów** opcje. Można zauważyć, że od tej nowej wersji **komentarz** przycisku (![przycisk komentarza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) i **Odkomentuj** przycisku (![przycisku Usuń znaczniki komentarza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) również są włączone dla Edytor CSS.

    ![Włączanie narzędzia CSS i edytor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "włączenie narzędzia CSS i edytora")

    *Włączanie narzędzia CSS i edytora*
3. Przewiń w kodzie i wybierz dowolna definicja klasy CSS. Kliknij przycisk **komentarz** (![przycisk komentarza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) przycisk, aby dodać komentarz z wybranych wierszy. Następnie kliknij przycisk **Odkomentuj** (![przycisk Usuń znaczniki komentarza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) przycisk, aby cofnąć zmiany.
4. Kliknij przycisk **Zwiń** (![Zwiń](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) i **rozwiń** (![rozwiń](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) przyciski znajdujących się na lewym marginesie tekstu. Należy zauważyć, że teraz można ukryć style, które nie są używane do czyszczenia widok.

    ![Zwijanie klas CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "klas CSS zwijania")

    *Zwijanie klas CSS*
5. Upewnij się, że włączona jest funkcja inteligentnego wcięcia. Wybierz **narzędzia** | **opcje** opcję menu, a następnie wybierz **edytora tekstów** | **CSS**  |  **Formatowanie** strony w okienku po lewej stronie ekranu. Sprawdź **wcięcie hierarchiczna** opcji.

    ![Włączanie wcięcie hierarchiczna](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Włączanie wcięcie hierarchiczne")

    *Włączanie wcięcie hierarchiczne*
6. Znajdź definicję klasy głównej (.main) i dołączyć stylu do elementów div. Zauważysz, kod wyrównuje automatycznie, pomagając użytkownikom można znaleźć klasy obiektu nadrzędnego w skrócie.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Hierarchiczna wyrównania w kodzie CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "hierarchiczne wyrównania w kodzie CSS")

    *Hierarchiczna wyrównania w kodzie CSS*
7. Wewnątrz **.main div** klasy, umieścić kursor na końcu **obramowanie: 0px;** i naciśnij klawisz **Enter** do wyświetlania listy funkcji IntelliSense. Zacznij wpisywać ciąg **górnej** i zwróć uwagę, jak lista jest filtrowana podczas wpisywania. Na liście będą wyświetlane elementy, które zawiera **górnej** w dowolnej części słowa (w poprzednich wersjach programu Visual Studio, lista jest filtrowana według elementów, *rozpocząć* z terminem).

    ![Ulepszenia funkcji IntelliSense w kodzie CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "ulepszeń funkcji IntelliSense w kodzie CSS")

    *Ulepszenia funkcji IntelliSense w kodzie CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Zadanie 2 — selektor kolorów

W tym zadaniu odnajdzie nowy Selektor koloru CSS włączona w funkcji IntelliSense Visual Studio.

1. W **Site.css,** Znajdź definicję klasy nagłówka (.header) i obok umieść kursor **kolor tła** atrybutu między &quot;:&quot; i &quot; # &quot; znaków w danym wierszu kodu **.**

    ![Lokalizowanie kursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "lokalizacji kursora")

    *Lokalizowanie kursora*
2. Usuń **dwukropek** (:) i zapisz go ponownie, aby wyświetlić selektor kolorów. Zauważ, że pierwszy kolorów, które zostanie wyświetlony są najczęściej używane kolory witryny. Po kliknięciu przycisku koloru białego kod HTML (#fff) spowoduje zastąpienie bieżącego kodu koloru w arkuszu stylów.

    ![Selektor kolorów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "selektor kolorów")

    *Selektor kolorów*
3. Naciśnij klawisz **rozwiń** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) przycisku na selektor kolorów, aby wyświetlić gradient kolorów, a następnie przeciągnij kursor gradientu, aby wybrać inny kolor. Następnie kliknij przycisk **Pipeta** i wybrać dowolny kolor na ekranie. Należy zauważyć, że wartość koloru tła zmienia się dynamicznie podczas przesuwania kursora.

    ![Gradient selektora kolorów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "gradientu selektor kolorów")

    *Gradient selektor kolorów*
4. W **nieprzezroczystość** suwak, Przenieś selektor do środka pasek, aby zmniejszyć przezroczystość. Zwróć uwagę, że wartość koloru tła teraz zmienia jego skalowanie do RGBA.

    ![Selektor kolorów nieprzezroczystość](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "selektor kolorów krycia")

    *Selektor kolorów krycia*

    > [!NOTE]
    > Definicja koloru RGBA (czerwony, zielony, niebieski, alfa) w standardzie CSS3 umożliwia zdefiniowanie wartość nieprzezroczystości kolorów dla pojedynczego elementu. W odróżnieniu od **nieprzezroczystość -** podobne atrybut CSS **-** RGBA kolory są również zgodne z najnowsze przeglądarki.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Zadanie 3. fragmenty kodu zgodne CSS

W tym zadaniu dowiesz się, jak używać fragmentów kodu CSS3 zgodne przeglądarek w celu wdrożenia niektóre funkcje w witrynie sieci Web.

1. W **Site.css** plików, zlokalizuj **nagłówka** CSS klasy definicji (.header) i umieść kursor poniżej **/ \*promień obramowania\* /** symbol zastępczy, aby dodać nowy fragment kodu. Naciśnij klawisz **Enter** do wyświetlenia na liście funkcji IntelliSense i typ **radius** do filtrowania listy. Wybierz **promień obramowania** opcji z listy z dwukrotnie kliknij grupę, a następnie naciśnij klawisz **kartę** klawisz, aby wstawić fragment kodu. Następnie wpisz rozmiar radius w pikselach, a następnie naciśnij klawisz **Enter**. Na przykład wpisz **15px**.

    Atrybuty CSS3, dodawane przez fragment kodu będą renderowane zaokrąglone obramowania w większości przeglądarek zgodności HTML5, w tym Mozilla i przeglądarek na podstawie aparatu WebKit.

    ![Przy użyciu fragmentu promień obramowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "przy użyciu fragmentu promień obramowania")

    *Przy użyciu fragmentu promień obramowania*
2. Zastosować te same **obramowania** fragmentów kodu w stylu strony (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie. Należy zauważyć, że każda strona teraz zaokrąglonymi obramowania.

    ![Zaokrąglone rogi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "zaokrąglone rogi")

    *Zaokrąglone narożniki*
4. Zamknij przeglądarkę i wróć do programu Visual Studio.
5. Otwórz **Custome.CSS** plik znajdujący się w obszarze **style** folder, umieść kursor wewnątrz **div.images ul li img** definicji klasy.
6. Naciśnij klawisz enter, aby wyświetlić listę funkcji IntelliSense, typ **box-shadow** i naciśnij klawisz **kartę** klawisz dwa razy, aby wstawić fragment kodu w tle domyślne wewnątrz definicji klasy. Ustaw wartości w tle **10px 10px 5px #888**. Następnie wpisz **promień obramowania** i Wstaw fragment kodu. Typ **15px** można ustawić rozmiar usługi radius, a następnie naciśnij klawisz **ENTER**.

    ![Zaokrąglone rogi z cieniem](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "zaokrąglone rogi z cieniem")

    *Zaokrąglone narożniki z cieniem*

    > [!NOTE]
    > W tej chwili atrybut w tle jest wstawiany za pomocą odpowiedniego prefiksu (moz aparatu webkit, o) do obsługi Mozilla i przeglądarki dla aparatu Webkit (Chrome i Safari, Konkeror).
7. Utwórz nową klasę **div.images ul li img:hover** poniżej **div.images ul li img** definicji klasy, umieść kursor wewnątrz nawiasów **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Typ **Przekształcanie** i naciśnij klawisz **kartę** klucz dwa razy, aby wstawić fragment kodu transformacji. Następnie wprowadź **rotate(-15deg)** Aby zmienić wartość kąta obrotu, gdy obrazy są aktywowany.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie, a następnie przejdź do strony CSS3. Należy zauważyć, że obrazy mają zaokrąglone rogi i polu cieni. Umieść kursor myszy nad obrazów i zaobserwuj, jak obrócić.

    ![Przekształcanie fragmentu Obracanie obrazu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "fragment przekształcenie Obracanie obrazu")

    *Przekształcanie fragmentu Obracanie obrazu*

    > [!NOTE]
    > Jeśli używasz programu Internet Explorer 10 i nie są widoczne cienie, upewnij się, że tryb dokumentu jest ustawiony na IE10 standardy. Naciśnij klawisz **F12** Otwórz program Internet Explorer, narzędzia dla deweloperów, a następnie kliknij przycisk **tryb dokumentu** zmienić IE10 standardy.

    ![temat-USA](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Ćwiczenie 2: Co nowego w edytorze HTML

Program Visual Studio został ulepszony edytor HTML. Niektóre rozszerzenia zawarte w tej wersji są inteligentne wcięcia w dokumentach HTML, fragmenty kodu HTML5, HTML start i dopasowanie znacznika końcowego i weryfikacji kodu HTML. Tego ćwiczenia zobaczysz, jak te zmiany ulepszyć swoje fluency podczas pracy w znacznikach witryny sieci Web.

Podobnie jak edytor CSS także ulepszono edytora HTML. Większość tych ulepszeń to małych sieci, które ułatwiają życie dla deweloperów sieci Web. Rzeczy, takich jak więcej fragmentów kodu za pomocą protokołu HTML5, inteligentne wcięcia dopasowania tagu początkowego i końcowego, w przypadku edycji i sprawdzania poprawności, przeznaczone dla dokumentu HTML DOCTYPE niektórymi ulepszeniami.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Zadanie 1 — ulepszone DOCTYPE sprawdzania poprawności

Edytor HTML obsługuje teraz możliwość sprawdzenia DOCTYPE strony sieci, nawet, jeśli definicja może być strony wzorcowej. W zależności od elementu DOCTYPE strony edytora HTML zostanie przeprowadzona Weryfikacja z poprawnym zestawem reguł i spowoduje odfiltrowanie listy funkcji IntelliSense, biorąc pod uwagę elementów DOCTYPE.

W ramach tego zadania zostanie zmieniony DOCTYPE strony, aby zobaczyć, jak zachowanie edytora HTML zmienia się odpowiednio.

1. Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązanie znajduje się w **Source\WhatsNewASPNET** folder w tym laboratorium.
2. Otwórz **Site.Master** strony.
3. Zwróć uwagę schemat docelowy dla paska narzędzi sprawdzania poprawności. Sposób edytora HTML zachowuje się (sprawdzania poprawności, IntelliSense, itp.) zmieni się prawidłowo aby dopasować Doctype wybrane.

    ![Użyj Doctype edytowanie źródła HTML na pasku narzędzi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Użyj Doctype pasku edytowanie źródła HTML")

    *Użyj Doctype pasku edytowanie źródła HTML*
4. W formacie HTML 4.01, należy zmienić schemat docelowy.

    ![Zmiana Doctype edytowanie źródła HTML na pasku narzędzi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "zmiana Doctype pasku edytowanie źródła HTML")

    *Zmiana Doctype pasku edytowanie źródła HTML*
5. Umieść kursor w obszarze **treści** elementu, a następnie wpisz nazwę elementu HTML5 (na przykład **wideo**). Należy zauważyć, że element nie jest dostępna na liście funkcji IntelliSense.

    ![Elementy języka HTML5, niewymienionych](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "elementy HTML5 niewymienione")

    *Elementy języka HTML5 niewymienione*
6. Cofnij zmiany schematu docelowego dla narzędzi weryfikacji pobrania elementu DOCTYPE: XHTML5 z listy rozwijanej.

    ![Użyj Doctype edytowanie źródła HTML na pasku narzędzi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Użyj Doctype pasku edytowanie źródła HTML")

    *Resetuj Doctype pasku edytowanie źródła HTML*
7. Umieść kursor w obszarze **treści** element i rozpocząć pisanie ponownie HTML5 element (na przykład, takich jak **wideo**). Zwróć uwagę, elementy HTML5 są teraz dostępne na liście funkcji IntelliSense.

    ![Elementy języka HTML5, jest wymienione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "elementy HTML5, które są wymienione")

    *Elementy języka HTML5, jest wymienione*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Zadanie 2 — rozpoczęcia/zakończenia tagów aktualizacji automatycznych

Aktualizacje programu Visual Studio teraz HTML otwierających ani zamykających nawiasów tagów elementu, który edytujesz ze sobą zgodne. Ta nowa funkcja poprawi wydajność pracy podczas edytowania tagów HTML.

1. Na **Default.aspx** strony, należy dodać **H3** elementu z tytułem (na przykład program Visual Studio 2012 Rocks!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Zmiana **H3** tagu i typ **H2** lub **H1.**

    Zauważ, że automatycznie aktualizuje tag końcowy. Można również zmodyfikować tagu końcowego, aby zobaczyć, że tag początkowy odpowiednio aktualizuje zbyt.

    ![Automatyczna aktualizacja tagu końcowego](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "automatycznej aktualizacji tagu końcowego")

    *Automatyczna aktualizacja tagu końcowego*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Zadanie 3 - nowych fragmentów kodu HTML5

Visual Studio zawiera obecnie kilka fragmentów kodu HTML5. W ramach tego zadania będziesz korzystać z niektórych z tych fragmentów kodu.

1. Dodaj nowy folder o nazwie **audio** do głównego folderu witryny sieci web. Otwórz Eksploratora Windows, a następnie skopiuj wszystkie plik audio w **audio** folderu **WhatsNewASPNET.sln** rozwiązania.
2. W **Default.aspx** strony, odszukaj pozycję kursora w obszarze Rocks sieci Web 11. Nagłówek. Typ **audio** i naciśnij klawisz TAB.

    Nowy edytor HTML zawiera fragmenty kodu HTML5 zawartości. Pamiętaj, aby użyć właściwego definicji typu dokumentu, aby włączyć fragmenty kodu HTML5.

    ![Wstawianie wstawek kodu HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "wstawianie fragmentów kodu HTML5")

    *Wstawianie wstawek kodu HTML5*
3. Zaktualizuj źródło audio, aby wskazać istniejący plik audio.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Należy dodać plik dźwiękowy do rozwiązania.
4. Naciśnij klawisz **F5** uruchamiania witryny i odtwarzanie dźwięku.

    ![Uruchamianie w kontrolce dźwięku](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "systemem kontrolce dźwięku")

    *Uruchamianie w kontrolce dźwięku*

    > [!NOTE]
    > Można też spróbować więcej fragmentów kodu zawarte w Visual Studio, takich jak wideo, rysunek itp.
5. Teraz spróbuj Wstawianie formantu w niektórych części strony. Na przykład, próba wstawienia **GridView** kontroli, ale zamiast wpisywać  **&lt;tki,** zacznij wpisywać ciąg  **&lt;GV**. Należy zauważyć, że lista IntelliSense zawiera **asp: GridView** kontroli.

    Funkcja IntelliSense w edytorze HTML udostępnia teraz wyszukiwania wielkości liter tytuł, a także częściowe dopasowania (pobieranie wszystkie elementy, które zawierają termin).

    ![Wstawianie w kontrolce GridView przy użyciu list IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "wstawiania w kontrolce GridView przy użyciu technologii IntelliSense list")

    *Wstawianie w kontrolce GridView przy użyciu technologii IntelliSense list*

    Jeśli wpiszesz  **&lt;siatki** uzyskasz wszystkie elementy, które odpowiadają termin, ale sugeruje programu Visual Studio **gridview** sterowania:

    ![Wstawianie w kontrolce GridView z listy technologii IntelliSense i częściowe dopasowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "wstawiania w kontrolce GridView z listy technologii IntelliSense i częściowe dopasowania")

    *Wstawianie w kontrolce GridView z listy technologii IntelliSense i częściowe dopasowania*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Zadanie 4 — tagi inteligentne edytora HTML

Poprawa innego edytora HTML jest funkcji tagów inteligentnych. Tagi inteligentne ułatwiają do wykonywania zadań tworzenia typowych lub powtarzające się na podstawie-control. Ta funkcja była już dostępna w Projektancie HTML, ale nie w edytorze HTML.

1. Otwórz **Site.Master** i Znajdź **asp: Menu** elementu. Umieść kursor w tagu początkowego i zwróć uwagę, mały glif wyświetlane w dolnej części elementu — kliknij go, aby otworzyć menu inteligentnego. Należy zauważyć, że masz szybki dostęp do niektórych zadań związanych z formant Menu.

    ![Inteligentne zadań dla formantu Menu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "inteligentne zadania formant Menu")

    *Inteligentnego dla formantu Menu*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Zadanie 5 — inteligentne wcięcie

Jedną z najlepszych rozwiązań w języku HTML jest wcięcia elementów zagnieżdżonych, aby zachować zgodność kodu do odczytu. W programie Visual Studio 2012 można zauważyć, że edytor automatycznie wcięcia elementów podczas pisania kodu.

> [!NOTE]
> W poprzedniej wersji programu Visual Studio, inteligentne wcięcia była dostępna w edytorze XML, ale nie w edytorze HTML.


1. Upewnij się, że konfiguracja Indenting w edytorze HTML inteligentnego wcięcia. Aby to zrobić, wybierz **narzędzia | Opcje** , a następnie wybierz opcję menu **Edytor tekstu | HTML | Karty** strony w okienku po lewej stronie ekranu. Wybierz opcję inteligentnego wcięcia.

    ![Ustawienia edytora HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "ustawienia edytora HTML")

    *Ustawienia edytora HTML*
2. Na **Default.aspx** strony, Usuń całą zawartość w elemencie audio.
3. Umieść kursor na końcu otwarcia **audio** element i naciśnij klawisz **ENTER**.

    Należy zauważyć, że nowe położenie kursora ma poziom wcięcia dodatkowe.

    ![Inteligentne wcięcia w edytorze HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "inteligentne wcięcia w edytorze HTML")

    *Inteligentne wcięcia w edytorze HTML*
4. Przywróć tagu audio z zawartością zostały usunięte lub Zamknij **Default.aspx** bez zapisywania zmian.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Zadanie 6 - Wyodrębnij do kontrolki użytkownika

Narzędzia Refactoring, zawarte w Visual Studio, takie jak wyodrębnianie fragmentu kodu do funkcji, są wspaniałymi funkcjami, które ułatwiają ulepszanie i refaktoryzacji istniejącego kodu. Odpowiednikiem stron ASP.NET będzie wyodrębniania kodu HTML do kontrolki użytkownika. To zrobić ręcznie obejmowałaby kilka kroków, takich jak utworzenie nowej kontrolki użytkownika, przenoszenie sekcję kodu do formantu użytkownika, rejestrowanie prefiksu tagu kontrolki użytkownika i, na koniec wystąpienia formantu użytkownika na stronach. Teraz nowe *Wyodrębnij do kontrolki użytkownika* narzędzie automatycznie wykonuje te kroki dla Ciebie.

W tym zadaniu użyjesz nowego wyodrębniania operacji kontekstowe kontrolki użytkownika do generowania nowego formantu użytkownika z zaznaczonego kodu.

1. Na **Default.aspx** wybierz opcję **H2** i **audio** elementów.
2. Kliknij prawym przyciskiem myszy i wybierz **Wyodrębnij do kontrolki użytkownika**.

    ![Wyodrębnij do kontrolki użytkownika opcji menu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Wyodrębnij do kontrolki użytkownika opcji menu")

    *Wyodrębnij do kontrolki użytkownika opcji menu*
3. Wpisz nazwę nowej kontrolki użytkownika. Na przykład **Jukebox.ascx**, a następnie kliknij przycisk **OK**.

    ![Zapisywanie kontrolki użytkownika wyodrębnione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "zapisywania kontrolki użytkownika wyodrębnione")

    *Zapisywanie kontrolki użytkownika wyodrębnione*
4. Należy zauważyć, że zaznaczony kod został wyodrębniony do kontrolki użytkownika i oryginalnej lokalizacji zaznaczony kod został zastąpiony wystąpienie nowej kontrolki użytkownika.

    ![Strona automatycznie zaktualizowana w celu używania nowej kontrolki użytkownika](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "strony automatycznie zaktualizowana w celu używania nowej kontrolki użytkownika")

    *Strona automatycznie zaktualizowana w celu używania nowej kontrolki użytkownika*
5. Naciśnij klawisz **F5** do uruchomienia strony i sprawdź, czy kontrolka działa.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Ćwiczenie 3: What's New in Edytor kodu JavaScript

Zapisywanie i edytowanie kodu JavaScript nie jest łatwym zadaniem, szczególnie w przypadku uruchamiania aplikacji na zwiększanie rozmiaru okaże się, że radzenia sobie z plikami długo, jak i setek funkcji. Deweloperzy skryptu muszą zwykle wykonać dodatkową pracę, aby zachować czytelność kodu i Przechodź między plikami. Dzięki uwzględnieniu biblioteki JavaScript, takimi jak jQuery nawigacji skryptu stał się wyzwaniem, sama ze względu na długość kodu.

Program Visual Studio odnowił Edytor kodu JavaScript przy użyciu promise się w trybie kod dostępny i zorganizowane. Wiele funkcji programu Visual Studio, które już istnieje w języku C# lub VB edytorach są teraz zaimplementowane w edytorze języka JavaScript: Przejdź do definicji, automatyczne wcięcia, dokumentację i sprawdzania poprawności podczas pisania. Przy użyciu odnowionego Lista IntelliSense masz wykaz funkcji języka JavaScript w zasięgu ręki.

W tym ćwiczeniu dowiesz się, niektóre nowe funkcje i ulepszenia edytora kodu JavaScript. Będzie przeglądać pliki przykładowe i odnajdywanie wszystkich nowych cech, składające się z programowania języka JavaScript bardziej wydajne w ramach programu Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Zadanie 1 — nowe funkcje edytora języka JavaScript

To zadanie przedstawiono niektóre nowe funkcje edytora języka JavaScript, które koncentrują się na przechowywanie kodu i szybszego wprowadzania lepsze środowisko użytkownika.

1. Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązanie znajduje się w **Source\WhatsNewASPNET** folder w tym laboratorium.
2. Naciśnij klawisz **F5** do uruchamiania aplikacji, a następnie kliknij łącze JavaScript, na pasku nawigacyjnym. Odśwież stronę kilka razy i sprawdź jak zwiększa licznik.

    ![Licznik na stronie](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "licznika na stronie")

    *Licznik na stronie*
3. Zamknij przeglądarkę i przejdź wstecz do programu Visual Studio.
4. Otwórz **JavaScript.aspx** strony, a następnie zlokalizuj **&lt;skryptu&gt;** bloku (pokazana poniżej).

    W poniższym kodzie użyto lokalny magazyn HTML5 do przechowywania *pageLoadCount* zmienna, która przechowuje liczbę strony został odwiedzony przez bieżącego użytkownika. Magazyn lokalny jest bazy danych po stronie klienta pary klucz wartość wprowadzona ze standardem HTML5. Dane są zapisywane na komputerze lokalnym, w przeglądarce użytkownika.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Upewnij się, że DOCTYPE jest ustawiona na XHTML5 przed przejściem do następnych kroków.
5. Edytowanie kodu i zwróć uwagę, że funkcja IntelliSense dla JavaScript funkcje HTML5, takie jak magazyn lokalny i ich metod wewnętrznego.

    ![Funkcje w JavaScript, HTML5, JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "funkcje w JavaScript, HTML5, JavaScript")

    *Funkcje HTML5 JavaScript w języku JavaScript*
6. Kliknij pozycję Wszystkie otwierający nawias kwadratowy (**{**) ze skryptów kodu i zwróć uwagę, że nawiasy są wyróżnione.

    ![Nawiasy są wyróżnione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "nawiasy są wyróżnione.")

    *Nawiasy są wyróżnione.*
7. Usuń znaczniki komentarza funkcja **testAutoAlign()** (Wybierz trzy wiersze, a następnie można użyć **CTRL** + **K**; **CTRL** + **U**) i Znajdź kursor wewnątrz kodu funkcji. Naciśnij klawisz enter, aby dołączyć drugi wiersz. Należy zauważyć, że kod jest teraz **wyrównane** i **wcięty automatycznie**.

    ![Kod JavaScript jest ustalana automatycznie wyrównane](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "kod JavaScript jest ustalana automatycznie wyrównane")

    *Kod JavaScript jest ustalana automatycznie wyrównane*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Zadanie 2 — Sprawdzanie poprawności języka JavaScript

W tym zadaniu wykryje nowe sprawdzanie poprawności języka JavaScript dla warstwy standardowej ECMAScript5. Ta funkcja pomaga napisać kod JavaScript zgodne, podczas zapobieganie zagadnienia dotyczące skryptów przed wdrożeniem lokacji.

> [!NOTE]
> Visual Studio 2010 implementowane ECMAStript3 zgodności, a program Visual Studio 2012 zapewnia zgodność ECMAScript5.


1. Otwórz **ECMA5script5.js** znajdujący się w folderze **Scripts\custom** folderu projektu. Będzie teraz przetestować sprawdzania poprawności dla ECMAScript5 standardowych.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Możesz sprawdzić &quot; **ograniczeniami użycia** &quot; kierunek, w pierwszym wierszu pliku, który umożliwia ECMAScript5 **trybie z ograniczeniami**. W tym trybie składa się z podzbioru języka, który wyjaśnia niejednoznaczności z poprzednich wersji i dodaje nowe funkcje, takie jak metod pobierających i ustawiających, obsługa bibliotek dla JSON, a dokładniejsza odbicia właściwości obiektu.
2. Otwórz **lista błędów** Jeśli nie jest jeszcze otwarty (**widoku** menu | **Lista błędów**). Zwróć uwagę **funkcja** deklaracji jest podkreślony. Jest to spowodowane w ECMA5 standardowych funkcji nie mogą być zagnieżdżone wewnątrz struktury języka. W wyniku błędu lista poniżej pojawią się szczegóły ostrzeżenie.

    ![Komunikat o błędzie weryfikacji JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "komunikat o błędzie weryfikacji języka JavaScript")

    *Komunikat o błędzie weryfikacji języka JavaScript*
3. Komentarz **&quot;ograniczeniami użycia&quot;** kierunek i zwróć uwagę, że błędy znikną, ale nadal ostrzeżenia.
4. W ostatnim wierszu w pliku zapisu dowolny ciąg, takich jak **&quot;test&quot;** (w tym znaki cudzysłowu, aby wskazać, że jest on jako ciąg). Zapis kropki obok ciągu, aby wyświetlić listę IntelliSense i wybrać **trim** opcji.

    W standardzie ECMAScript5 wartości parametrów i zmiennych również mieć metod ciągów zdefiniowane, takie jak przyciąć, wielkie litery, wyszukiwania i zamieniania.

    ![Lista IntelliSense w języku JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "Lista IntelliSense w języku JavaScript")

    *Lista IntelliSense w języku JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Zadanie 3 — dokumentacja XML dla języka JavaScript

W tym zadaniu przedstawimy funkcje programu Visual Studio dla dokumentacji XML w języku JavaScript. Zobaczysz, że lista JavaScript IntelliSense zawiera teraz dokumentacji XML w każdej funkcji. Ponadto spowoduje odnalezienie funkcji nawigacji w języku JavaScript.

1. Otwórz **XMLDoc.js** plik znajdujący się w **skryptów/niestandardowa** folderu projektu. Ten plik zawiera dokumentację XML na poszczególnych funkcji języka JavaScript.

    ![Dokumentacji JavaScript XML zintegrowanych funkcji IntelliSense,](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "dokumentacji JavaScript XML zintegrowanych funkcji IntelliSense")

    *Dokumentacji JavaScript XML zintegrowanych funkcji IntelliSense*
2. Poniżej **Dodaj** działa w programach **XMLDoc.js** plików, Utwórz nową funkcję o nazwie **testu**.
3. W **test** funkcji, wywołania **mnożenia** funkcja, która otrzymuje dwa parametry. Zwróć uwagę, są wyświetlane pola etykietki narzędzia **mnożenia** dokumentację funkcji.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Dokumentacja XML dla funkcji języka JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "dokumentacji XML dla funkcji języka JavaScript")

    *Dokumentacja XML dla funkcji języka JavaScript*
4. Wykonaj instrukcję wywołania funkcji i typów *kropka* aby otworzyć listę IntelliSense w zwracanej wartości. Należy zauważyć, że program Visual Studio jest wykrywanie **zwracają** wartość w dokumentacji traktowanie wartości jako liczby.

    ![Dokumentacja XML dla typów zwracanych](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dokumentacji XML dla typów zwracanych")

    *Dokumentacja XML dla typów zwracanych*
5. Teraz wstawić numer telefonu, aby dodać funkcję. Należy zauważyć, że Edytor kodu JavaScript obsługuje teraz funkcję. Podczas zapisywania nazwy funkcji, można wybrać dowolny z dostępnych przeciążeń, które określono w dokumentacji.

    ![Dokumentacja XML do przeciążenia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "dokumentacji XML dla przeciążeń")

    *Dokumentacja XML do przeciążenia*
6. Otwórz **GotoDefinition.js** pliku, a następnie zlokalizuj **$().html()** wywołania funkcji. Umieszczanie kursora na **html**.
7. Naciśnij klawisz **F12** i przejdź do definicji. Zwróć uwagę, możesz teraz uzyskać dostęp i przeglądać kod JavaScript bez użycia **znaleźć** narzędzia.
8. Umieścić kursor na instrukcji jQuery przed blok podpisu na końcu pliku kodu. Naciśnij klawisz **F12**. Nastąpi przeniesienie do pliku biblioteki jQuery. Zwróć uwagę, możesz także przejść między plikami jQuery przy użyciu **F12**.

    ![Przejdź do definicji jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "przejdź do definicji jQuery")

    *Przejdź do definicji jQuery*

> [!NOTE]
> Upewnij się, że GotoDefinition.js nie ma żadnych błędów składni przed zapisaniem pliku.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Ćwiczenie 4: Tworzenie pakietów i minimalizowanie

Ile razy witryn sieci Web ma więcej niż jednego języka JavaScript i CSS pliku? Jest to bardzo typowy scenariusz, w którym tworzenie pakietów i minimalizowanie może pomóc zmniejszyć rozmiar pliku i witryny działać szybciej. Nowa funkcja tworzenia pakietów w programie ASP.NET 4.5 pakiety zestawu plików Javascript i CSS w jeden element i zmniejsza jego rozmiar o minifikacja zawartości (czyli usuwania nie jest wymagane spacji, usuwanie komentarzy, zmniejszenie identyfikatorów).

Tworzenie pakietów i minimalizowanie w programie ASP.NET 4.5 odbywa się w czasie wykonywania, aby proces identyfikacji agent użytkownika (na przykład programu Internet Explorer, Mozilla itp.), a w związku z tym, poprawić kompresji, określając jako docelowe w przeglądarce użytkownika (na przykład, usunięcie rzeczy będącego Mozilla określonych Jeśli żądanie pochodzi z programu Internet Explorer).

W tym ćwiczeniu dowiesz się, jak włączyć i używać różnych typów tworzenie pakietów i minimalizowanie w programie ASP.NET 4.5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Zadanie 1 — Instalowanie, tworzenie pakietów i minimalizowanie pakietu NuGet

1. Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązanie znajduje się w **Source\WhatsNewASPNET** folder w tym laboratorium.
2. Otwórz konsolę Menedżera pakietów NuGet. Aby to zrobić, użyj menu **widoku** | **Windows inne** | **Konsola Menedżera pakietów**.

    ![Otwieranie file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole Menedżera pakietów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "otwierania konsoli Menedżera pakietów")

    *Otwieranie konsoli Menedżera pakietów*
3. W **Konsola Menedżera pakietów** typu **Microsoft.Web.Optimization Install-Package** i naciśnij klawisz **ENTER**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Zadanie 2 — domyślne pakiety

Najprostszym sposobem użycia tworzenie pakietów i minimalizowanie jest włączyć domyślne pakiety. Ta metoda używa konwencji, co pozwala odwoływać się do wersji powiązane i zminimalizowany dla plików Javascript i CSS w folderze.

W tym zadaniu dowiesz się, jak włączyć i powiązane i zminimalizowany plików Javascript i CSS i wyświetlić dane wyjściowe.

1. Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązanie znajduje się w **Source\WhatsNewASPNET** folder w tym laboratorium.
2. W **Eksploratora rozwiązań**, rozwiń węzeł **style**, **Scripts\custom** i **Scripts\bundle** folderów.

    Należy zauważyć, że aplikacja korzysta z więcej niż jeden CSS i JS pliku.

    ![Pliki wiele arkuszy stylów i JavaScript w aplikacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "pliki wiele arkuszy stylów i JavaScript w aplikacji")

    *Wiele plików arkusze stylów i języka JavaScript w aplikacji*
3. Otwórz **Global.asax.cs** pliku.

    Należy zauważyć, że nowy **Microsoft.Web.Optimization** przestrzeni nazw jest oznaczone jako komentarz na początku pliku. Usuń znaczniki komentarza przy użyciu dyrektywy w celu włączenia funkcji Tworzenie pakietów i minimalizowanie.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Znajdź **aplikacji\_Start** metody.

    W przypadku tej metody usuń znaczniki komentarza wywołanie EnableDefaultBundles, jak pokazano w poniższym fragmencie kodu. Pozwala to nam odwołania powiązane kolekcję plików CSS w folderze przy użyciu ścieżki do tego folderu, a także &quot;CSS&quot; lub &quot;JS&quot; sufiks.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Otwórz **Optimization.aspx** plików i lokalizacji zawartości formantu dla **HeadContent**.

    Zwróć uwagę, pliki CSS i pliki JS, mieć pojedynczy tag odwołania.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Ten kod jest dla celów demonstracyjnych. W idealnym przypadku będzie odwoływać pakiety w pliku Site.Master. Ten przykładowy kod zawiera czy niektóre powiązane pliki są odwołuje się również przy użyciu pliku Site.Master wprowadzania to ostatnie odwołanie nadmiarowe.
6. Należy zauważyć, że linki z konwencji tworzenia pakietów w **href** atrybutu, aby pobrać pliki CSS i JS z — style i Scripts\custom folderu odpowiednio.

    Można użyć ścieżki **skryptów/niestandardowe/JS** pokazany poniżej do pakietów i Minimalizowanie wszystkich plików JS wewnątrz **skryptów/niestandardowa** folderu. Jest to domyślne zachowanie z pakietami domyślne.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Otwórz **Styles\Site.css** pliku.

    Należy zauważyć, że oryginalny plik CSS zawiera kod z wcięciami, spacje i komentarze, które Powiększ pliku. (Plik JavaScript także spacje i komentarze).

    ![Jeden arkusza CSS, oryginalnym pliki w folderze skrypty](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "jednego arkusza CSS, oryginalnym pliki w folderze skryptów")

    *Jedną z oryginalnych plików CSS w folderze skryptów*
8. Naciśnij klawisz **F5** do uruchamiania aplikacji, a następnie przejdź do **optymalizacji** strony.
9. Kliknij pozycję **CSS pakietu** link, aby pobrać i otworzyć plik.

    Zapoznaj się z zminimalizowanego pliku powiązane. Zauważysz, że zostały usunięte wszystkie spacje, komentarze i znaki wcięcia, generowanie mniejszy plik.

    ![Powiązane pliki CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "pliki CSS powiązane")

    *Powiązane pliki CSS*
10. Teraz klikaj polecenie **JS pakietu** link, aby otworzyć plik JavaScript powiązane. Możesz bezpiecznie zignorować explorer ostrzeżenie. Zwróć uwagę, plików JavaScript, w obszarze **niestandardowe** folderu również są powiązane i zminimalizowania.

    ![Pliki JavaScript w pakiecie](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "pliki powiązane JavaScript")

    *Powiązane pliki JavaScript*

    Włączanie kompresji dla plików CSS i JS była dużo bardziej skomplikowany w poprzedniej wersji programu ASP.NET. Teraz, jak wiesz już, wystarczy dodać jeden wiersz w *Global.asax* pliku umożliwia tworzenie pakietów, a następnie Odwołaj pliki w pakiecie z witryny.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Zadanie 3 — statyczne pakiety

Metody statyczne pakiet umożliwia dostosowanie zestawu plików do pakietu, odwołania i metoda gotowy do przeprowadzania minifikacji, który będzie używany.

W tym zadaniu skonfiguruje zbiór statycznej do definiowania określonego zestawu plików pakietów i minimalizowanie.

1. Zamknij przeglądarkę.
2. Otwórz **Global.asax.cs** pliku, a następnie zlokalizuj **aplikacji\_Start** metody.
3. Usuń komentarz kodu statycznego pakietu, jak pokazano w poniższym kodzie.

    Definiujesz statyczne pakiet, który będzie odwoływać się za pomocą &quot; **~/StaticBundle** &quot; ścieżkę wirtualną i użyj **JsMinify** dla minimalizacji wszystkie określone pliki z **AddFile** metody. Ponadto w przypadku dodawania statyczne pakietu do **BundleTable** i włączenie go.

    Należy zauważyć, że pliki nie znajdują się w tym samym miejscu; jest to inny przewagę nad udane domyślne.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Otwórz **Optimization.aspx** pliku.

    Należy zauważyć, że link do **statyczne JS pakietu** używa ścieżki zadeklarowano po skonfigurowaniu pakietu statycznych w pliku Global.asax.cs: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Naciśnij klawisz **F5** do uruchamiania aplikacji, a następnie przejdź do **optymalizacji** strony.
6. Kliknij pozycję **statyczne JS pakietu** link, aby otworzyć plik.

    Ogłoszenie zminimalizowany powiązane plik JavaScript znajdują się dane wyjściowe dla plików JavaScript skonfigurowany w pliku pakietu statycznych w ścieżce &quot;/StaticBundle&quot;.

    ![Statyczne pakietu plików JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "pakietu plików statycznych JavaScript")

    *Pliki statyczne języka JavaScript pakietu*
7. Zamknij przeglądarkę i wróć do programu Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Zadanie 4 — pakietów dynamicznych folderu

W tym zadaniu dowiesz się, jak skonfigurować dynamiczne folderu pakietów. Możliwości dynamicznego tworzenia pakietów jest obejmują statyczne języka JavaScript, a także inne pliki w językach, które kompilowany na język JavaScript, a w związku z tym, wymagają pewnych przetwarzania, przed wykonaniem tworzenia pakietów.

W tym przykładzie zostanie dowiesz się, jak używać **DynamicFolderBundle** klasy, aby utworzyć zbiór dynamiczne pliki zapisane w CofeeScript. CofeeScript jest kompilowany na język JavaScript, która ma składnię przyspiesza pisanie kodu JavaScript, zwięzłości i czytelności języka JavaScript w języku programowania.

1. Otwórz **Global.asax.cs** pliku, a następnie zlokalizuj **aplikacji\_Start** metody.
2. Usuń komentarz kodu pakiet dynamiczny, jak pokazano w poniższym kodzie.

    Definiujesz pakiet dynamiczny folder, który będzie używany **CoffeeMinify** procesora minimalizację niestandardowego, który będzie dotyczyć tylko plików przy użyciu &quot; **.coffee** &quot; (rozszerzenia Pliki CoffeeScript). Zwróć uwagę, że można użyć wzorca wyszukiwania i wybierz pliki pakietów w folderze, takie jak "\*.coffee".

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Otwórz konsolę Menedżera pakietów NuGet. Aby to zrobić, użyj menu **widoku** | **Windows inne** | **Konsola Menedżera pakietów**.
4. W **Konsola Menedżera pakietów** typu **CoffeeSharp Install-Package** i naciśnij klawisz **ENTER**.
5. Kliknij przycisk **Pokaż wszystkie pliki** znajdujący się w **Eksploratora rozwiązań** okna

    ![Wyświetlanie wszystkich plików](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "przedstawiający wszystkie pliki")

    *Wyświetlanie wszystkich plików*
6. Kliknij prawym przyciskiem myszy **CoffeeMinify.cs** w pliku **Eksploratora rozwiązań** i wybierz **Include w projekcie**

    ![Dołącz plik CoffeeMinify.cs w projekcie](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "dołączenie pliku CoffeeMinify.cs w projekcie")

    *Dołącz plik CoffeeMinify.cs w projekcie*
7. Otwórz **CoffeeMinify.cs** pliku.

    Ta klasa dziedziczy JsMinify do zminimalizowania dane wyjściowe JavaScript wynikające z kompilacji kodu języka CoffeeScript. Wywołuje kompilator języka CoffeeScript, aby najpierw Generuj kod JavaScript, a następnie wysyła je do metody JsMinify.Process do zminimalizowania wynikowy kod.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Otwórz **Script1.coffee** i **Script2.coffee** plików ze **skryptów/pakietu** folderu.

    Te pliki będzie zawierać kod CoffeScript zestawiane w czasie wykonywania, tworzenie pakietów przy użyciu klasy CoffeeMinify.

    Dla uproszczenia pliki CoffeeScript podane są tylko tym CoffeeScript przykładowy kod. Komentarze są wyłączone przez proces JsMinify.

    ![Pliki CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "plików CoffeeScript")

    *Pliki CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) ma składnię przyspiesza pisanie kodu JavaScript, zwięzłości i czytelności języka JavaScript, a także dodawanie innych funkcji, takich jak tablica zrozumienie i dopasowywania do wzorca.
9. Otwórz **Optimization.aspx** plików i Znajdź łącza pakietu.

    Należy zauważyć, że link do **dynamiczne JS pakietu** odwołuje się do **skryptów/pakietu** folder przy użyciu **/kawy** skonfigurowane dla pakietu folderu dynamicznego sufiks.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Naciśnij klawisz **F5** do uruchamiania aplikacji, a następnie przejdź do **optymalizacji** strony.
11. Kliknij pozycję **dynamiczne JS pakietu** link umożliwiający otworzenie wygenerowany plik.

    Należy zauważyć, że zawiera tylko zawartość, która została uwzględniona w tym pakiecie **.coffee** plików. Widać również, że kod języka CoffeeScript został skompilowany w kodzie JavaScript i wiersze zakomentowany został usunięty.

    ![Dynamiczne pliki JS pakietu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "JS dynamiczne pliki pakietu")

    *Dynamiczne pliki JS pakietu*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację do witryny sieci Web systemu Windows Azure następujące [dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym laboratorium pomaga zrozumieć, co to jest nowe w programie ASP.NET i Web Development w Visual Studio 2012 i jak korzystać z różnego rodzaju rozszerzeń w programie Visual Studio 2012.

Przez ukończenie tego laboratorium praktyczne, masz dzięki modelom uczenia sposobu używania nowych funkcji i ulepszeń w programie Visual Studio 2012 edytory CSS, JavaScript i HTML. Ponadto zostały wcześniej, jak Visual Studio 2012 implementuje wbudowane tworzenie pakietów i minimalizowanie.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "zainstalować program Visual Studio Express")

    *Zainstaluj program Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express for Web kafelka*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

Ten dodatek będzie pokazują, jak utworzyć nową witrynę sieci web w portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskany postępując zgodnie z laboratorium, korzystając z zalet funkcji publikowania narzędzia Web Deploy oferowanego przez system Windows Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1. Tworzenie nowej witryny sieci Web z Windows Azure Portal

1. Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft, powiązaną z Twoją subskrypcją.

    > [!NOTE]
    > Platforma Windows Azure można bezpłatny hosting 10 witryn sieci Web platformy ASP.NET i skalowanie w miarę wzrostu ruchu. Możesz zarejestrować się [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu usługi Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Zaloguj się do portalu usługi Windows Azure")

    *Zaloguj się do portalu zarządzania systemu Azure Windows*
2. Kliknij przycisk **New** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczenia** | **witryny sieci Web**. Następnie wybierz pozycję **szybkie tworzenie** opcji. Podaj adres URL dostępny dla nowej witryny sieci web, a następnie kliknij przycisk **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Witryny sieci Web Windows Azure jest hostem dla aplikacji sieci web działające w chmurze, które można kontrolować i zarządzać nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do Windows Azure witryny sieci Web z poza portalem. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkiego tworzenia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj, aż nowe **witryny sieci Web** zostanie utworzony.
5. Po utworzeniu witryny sieci Web kliknij link w obszarze **adresu URL** kolumny. Sprawdź, czy działa nową witrynę sieci Web.

    ![Przejście do nowej witryny sieci web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "przejście do nowej witryny sieci web")

    *Przejście do nowej witryny sieci web*

    ![Witryna sieci Web działa](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "witryna sieci Web działa")

    *Witryna sieci Web działa*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie strony zarządzania witryny sieci web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie strony zarządzania witryny sieci Web*
7. W **pulpit nawigacyjny** w obszarze **Przegląd** kliknij **Pobierz profil publikowania** łącza.

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web w witrynie sieci Web usługi Windows Azure dla poszczególnych metod włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do łączenia się i uwierzytelnianie w odniesieniu do każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** Obsługa odczytywania profili publikowania do automatyzowania konfiguracji tych programów dla Publikowanie aplikacji sieci web w usłudze Windows Azure websites.

    ![Pobieranie witryny sieci web profil publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz plik profilu publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do Windows Azure Web Sites z programu Visual Studio za pomocą tego pliku.

    ![Zapisywanie pliku profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, musisz utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Należy serwera usługi SQL Database do przechowywania bazy danych aplikacji. Możesz wyświetlić serwery baz danych SQL z subskrypcji w portalu zarządzania Azure Windows pod **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**. Jeśli nie masz serwer, który został utworzony, można utworzyć ją przy użyciu **Dodaj** przycisk na pasku poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, ponieważ będziesz ich używać w kolejne zadania podrzędne. Nie należy tworzyć bazy danych, ponieważ zostanie on utworzony w późniejszym terminie.

    ![Pulpit nawigacyjny z serwera bazy danych SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny z serwera bazy danych SQL*
2. W ramach następnego zadania spowoduje przetestowanie połączenia z bazą danych z programu Visual Studio, dlatego trzeba uwzględnić adres IP lokalnego serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżący adres IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pól tekstowych. Wprowadź nazwę reguły, a następnie kliknij przycisk ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) przycisku.

    ![Dodawanie adresu IP klienta](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP listy, kliknij pozycję **Zapisz** aby potwierdzić zmiany.

    ![Potwierdź zmiany](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Potwierdź zmiany*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **Publikuj**.

    ![Publikowanie aplikacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Importuj profil publikowania, zapisaną w pierwszym zadaniem.

    ![Trwa importowanie profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "importowania profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **sprawdzanie poprawności połączenia**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Sprawdzanie poprawności zostało ukończone, gdy zostanie wyświetlony z zielonym znacznikiem wyboru pojawiają się obok przycisku Waliduj połączenie.

    ![Sprawdzanie poprawności połączenia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk obok pola tekstowego połączenia bazy danych (czyli **DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W **nazwy serwera** wpisz swoją bazę danych SQL server adresu URL przy użyciu *tcp:* prefiks.
   - W **nazwa_użytkownika** wpisz nazwę logowania administratora serwera.
   - W **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.

     ![Konfigurowanie parametrów połączenia z docelowym](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Konfigurowanie parametrów połączenia z docelowym")

     *Konfigurowanie parametrów połączenia z docelowym*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu, aby utworzyć kliknij bazę danych **tak**.

    ![Tworzenie bazy danych](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Tworzenie parametrów bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do łączenia z bazą danych SQL na platformie Windows Azure jest wyświetlana w polu tekstowym połączenia domyślne. Następnie kliknij przycisk **Dalej**.

    ![Ciąg połączenia wskazujący bazę danych SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "ciąg połączenia wskazujący bazę danych SQL")

    *Ciąg połączenia wskazujący bazę danych SQL*
8. W **Podgląd** kliknij **Publikuj**.

    ![Publikowanie aplikacji sieci web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

    ![Aplikacja została opublikowana na platformie Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "aplikacja została opublikowana na platformie Windows Azure")

    *Aplikacja opublikowana na platformie Windows Azure*
