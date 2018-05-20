---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: What's New in ASP.NET i aplikacji sieci Web w programie Visual Studio 2012 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Nowa wersja programu Visual Studio wprowadzono wiele ulepszeń skoncentrowane na ułatwieniu środowisko i wydajności podczas pracy z technologii sieci Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f447dc0108dffb36ed6d627fb83b3117fd22c94c
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/18/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>What's New in ASP.NET i aplikacji sieci Web w programie Visual Studio 2012
====================
Przez [obozów sieci Web Team](https://twitter.com/webcamps)

> Nowa wersja programu Visual Studio wprowadzono wiele ulepszeń skoncentrowane na ułatwieniu środowisko i wydajności podczas pracy z technologii sieci Web. Visual Studio edytory CSS, JavaScript i HTML ma została utworzona całkowicie od nowa do obejmuje wiele najbardziej w żądanie pomocy kodu, takie jak IntelliSense i automatyczne wcięcia. Dotyczące wydajności tworzenie pakietów i minimalizowanie są teraz zintegrowane jak czas ładowania wbudowanych funkcji, aby łatwo ograniczyć strony.
> 
> Program Visual Studio umożliwia pracę z najnowszych technologii witryny sieci Web. Fragmenty kodu CSS3 różnych przeglądarkach umożliwia upewnij się, że witryna działa bez względu na platformę klienta również wykorzystując nowe elementy HTML5 i funkcje.
> 
> Zapisywanie i profilowanie kodu JavaScript powinny być łatwiejsze w przypadku tej wersji programu Visual Studio. Listy IntelliSense, zintegrowane funkcje dokumentacji i nawigacja XML są teraz dostępne dla kodu JavaScript. Masz teraz katalogu JavaScript w zasięgu ręki. Ponadto można sprawdzić zgodność ECMAScript5 za pomocą tych skryptów i wykrywania błędów składni na wczesnym etapie.
> 
> Ostatnio, ale nie najmniej tej wersji programu Visual Studio implementuje wbudowanych tworzenie pakietów i minimalizowanie. Pliki skryptów i arkusze stylów zostanie spakowane i skompresować tak, aby lokacji wykonuje szybciej.
> 
> W tym laboratorium przeprowadzi Cię przez rozszerzenia oraz nowe funkcje opisane wcześniej przez zastosowanie drobne zmiany do przykładowej aplikacji sieci Web w folderze źródłowym.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ręce w laboratorium dowiesz się jak:

- Użyj nowe funkcje i ulepszenia w edytorze CSS
- Użyj nowe funkcje i ulepszenia w edytorze HTML
- Użyj nowe funkcje i ulepszenia w edytorze kodu JavaScript
- Konfigurowanie i używanie tworzenie pakietów i minimalizowanie

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).
- [Środowisko Windows PowerShell](https://support.microsoft.com/kb/968930/) (w przypadku skryptów instalacji — już zainstalowanym systemem Windows 8 i Windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - lub HTML5 przeglądarki zgodne

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

Wskazówki tego laboratorium obejmuje następujących czynnościach:

1. [Ćwiczenie 1: Nowości w edytorze CSS](#Exercise1)
2. [Ćwiczenie 2: Nowości w edytorze HTML](#Exercise2)
3. [Ćwiczenie 3: Nowości w edytorze kodu JavaScript](#Exercise3)
4. [Ćwiczenie 4: Tworzenie pakietów i minimalizowanie](#Exercise4)

Szacowany czas trwania tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Ćwiczenie 1: Nowości w edytorze CSS

Deweloperzy sieci Web należy zapoznać się z wielu trudności związane z CSS edycji. Jedną z największych problemy style CSS jest pomiędzy przeglądarkami. Często zdarza się, że po zastosowaniu stylów do swojej witryny, można zauważyć, że wygląda inaczej po otwarciu w innej przeglądarki lub urządzenia. W związku z tym może poświęcać przez dłuższy czas na rozwiązywania tych problemów visual sobie sprawę, że koniec powoduje ona działać w jednej przeglądarki, jest on uszkodzony na innych.

Visual Studio teraz obejmuje funkcje, które ułatwiają deweloperom dostęp, praca i efektywnie zorganizować arkusze stylów CSS. W ramach tego ćwiczenia będzie spełniać nowych funkcji dla wersji i skuteczne organizacji, a także fragmenty kodu CSS3 zgodności różnych przeglądarkach.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Zadanie 1 — nowe funkcje edytora

W tym zadaniu możesz zauważyć nowe funkcje Edytor CSS. Ten nowy edytor pomoże Ci zwiększyć produktywność dzięki wykorzystaniu nowych inteligentne wcięcie, komentarze w kodzie ulepszone i rozszerzone listy IntelliSense.

1. Uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.
2. W Eksploratorze rozwiązań Otwórz **Site.css** plik znajdujący się w obszarze **style** folderu. Upewnij się, że **Edytor tekstu** narzędzia są widoczne na pasku narzędzi. W tym celu wybierz **widoku** | **paski narzędzi** opcji menu, a także sprawdź **Edytor tekstu** opcje. Można zauważyć, że od tej nowej wersji **komentarz** przycisk (![przycisk komentarz](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) i **Usuń komentarz** przycisk (![przycisk Usuń znaczniki komentarza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) również są włączone dla Edytor CSS.

    ![Włączanie edytora i narzędzia CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "włączenie edytora i narzędzia CSS")

    *Włączanie edytora i narzędzia CSS*
3. Przewiń w kodzie i wybierz żadnych definicji klas CSS. Kliknij przycisk **komentarz** (![przycisk komentarz](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) przycisk, aby komentarz dotyczący wybranych wierszy. Następnie kliknij przycisk **Usuń komentarz** (![przycisk Usuń znaczniki komentarza](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) przycisk, aby cofnąć zmiany.
4. Kliknij przycisk **Zwiń** (![Zwiń](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) i **rozwiń** (![rozwiń](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) przycisków znajdujących się na lewym marginesie tekstu. Należy zauważyć, że teraz można ukryć style, które nie jest używany widok oczyszczarki.

    ![Zwijanie klas CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "klas CSS zwijanie")

    *Zwijanie klas CSS*
5. Upewnij się, że włączona jest funkcja inteligentnego wcięcia. Wybierz **narzędzia** | **opcje** opcji menu, a następnie wybierz **Edytor tekstu** | **CSS**  |  **Formatowanie** strony w okienku po lewej stronie ekranu. Sprawdź **hierarchiczna wcięcia** opcji.

    ![Włączanie hierarchiczna wcięcia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "włączenie wcięcia hierarchiczne")

    *Włączanie wcięcia hierarchiczne*
6. Znajdź definicji klasy głównym (.main) i Dołącz stylu do elementów div. Można zauważyć kod wyrównuje automatycznie, myśl użytkowników można znaleźć klasy nadrzędnej jeden rzut oka.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Hierarchiczna wyrównania w kodzie CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "hierarchiczna wyrównania w kodzie CSS")

    *Hierarchiczna wyrównania w kodzie CSS*
7. Wewnątrz **.main div** klasy, Znajdź kursor na końcu **obramowania: 0px;** i naciśnij klawisz **Enter** do wyświetlenia na liście IntelliSense. Zacznij pisać **górnej** i zwróć uwagę, jak lista jest przefiltrowana podczas pisania. Lista będzie zawierać elementów, które zawierają **górnej** w jakimkolwiek Word (w poprzednich wersjach programu Visual Studio, listy są filtrowane według elementy który *rozpocząć* z terminem).

    ![Ulepszenia IntelliSense w kodzie CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "ulepszeń funkcji IntelliSense w kodzie CSS")

    *Ulepszenia IntelliSense w kodzie CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Zadanie 2 — próbnika kolorów

W tym zadaniu zostanie wykryty próbnika kolorów CSS włączona w programie Visual Studio IntelliSense.

1. W **Site.css,** zlokalizować definicji klasy nagłówka (.header), umieść kursor w polu **kolor tła** atrybutu między &quot;:&quot; i &quot; # &quot; znaków w tym wierszu kodu **.**

    ![Lokalizowanie kursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "lokalizowanie kursora")

    *Lokalizowanie kursora*
2. Usuń **dwukropek** (:) i zapisać go ponownie, aby wyświetlić próbnika kolorów. Zwróć uwagę, czy pierwszy kolorów, które będą wyświetlane są najczęściej kolory witryny. Jeśli klikniesz przycisk kolor biały, kod HTML (#fff) spowoduje zastąpienie bieżącego kodu kolorów w arkuszu stylów.

    ![Selektor kolorów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "próbnika kolorów")

    *Selektor kolorów*
3. Naciśnij klawisz **rozwiń** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) przycisk na próbnika kolorów, aby wyświetlić kolor gradientu, a następnie przeciągnij kursor gradientu, aby wybrać inny kolor. Następnie kliknij przycisk **Kroplomierz** i wybrać dowolny kolor na ekranie. Zwróć uwagę, że wartość koloru tła zmienia dynamicznie, gdy kursor jest.

    ![Kolor gradientu selektora](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "gradientu próbnika kolorów")

    *Kolor gradientu selektora*
4. W **nieprzezroczystość** suwaka, przesuń selektor środek pasek, aby zmniejszyć przezroczystość. Zwróć uwagę, że wartość koloru tła teraz zmienia jego skali do RGBA.

    ![Selektor kolorów nieprzezroczystość](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "próbnika kolorów nieprzezroczystość.")

    *Selektor kolorów nieprzezroczystość.*

    > [!NOTE]
    > Definicja kolor RGBA (czerwony, zielony, niebieski, alfa) w standardzie CSS3 umożliwia zdefiniowanie wartości Przezroczystość koloru dla pojedynczego elementu. W odróżnieniu od **nieprzezroczystość -** podobne atrybut CSS **-** kolory RGBA również są zgodne z najnowszych przeglądarkach.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Zadanie 3 — wstawki kodu zgodne CSS

W tym zadaniu dowiesz się, jak używać różnych przeglądarkach zgodne CSS3 fragmentów w celu wdrożenia niektóre funkcje w witrynie sieci Web.

1. W **Site.css** pliku, Znajdź **nagłówka** CSS klasy definicji (.header) i umieść kursor poniżej **/ \*radius obramowania\* /** symbolu zastępczego, aby dodać nowy fragment kodu. Naciśnij klawisz **Enter** do wyświetlenia na liście IntelliSense i typ **radius** Aby filtrować listę. Wybierz **border-radius** opcję z listy za pomocą podwójnego kliknięcia, a następnie naciśnij klawisz **kartę** klucz do wstawiania wstawki kodu. Następnie wpisz rozmiar radius w pikselach i naciśnij klawisz **Enter**. Na przykład wpisz **15px**.

    Atrybuty CSS3, dodawane przez fragment kodu spowoduje, że zaokrąglony obramowań w większości przeglądarek zgodności HTML5, w tym Mozilla i na podstawie WebKit przeglądarki.

    ![Przy użyciu fragment border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "przy użyciu fragment border-radius")

    *Przy użyciu fragment border-radius*
2. Zastosować te same **obramowania** fragmentów w stylu strony (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie. Zwróć uwagę, że każdej stronie teraz ma zostać zaokrąglona obramowań.

    ![Zaokrąglone narożniki](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "zaokrąglone narożniki")

    *Zaokrąglone narożniki*
4. Zamknij przeglądarkę i powrócić do programu Visual Studio.
5. Otwórz **Custome.CSS** plik znajdujący się w obszarze **style** folderu, umieść kursor w **div.images ul li img** definicji klasy.
6. Naciśnij klawisz enter, aby wyświetlić listę funkcji IntelliSense, typ **tle pole** i naciśnij klawisz **kartę** klucza dwa razy, aby wstawić fragment kodu tle domyślne wewnątrz definicji klasy. Ustaw wartości cienia **10px 10px 5px #888**. Następnie wpisz **border-radius** i wstawianie fragmentu kodu. Typ **15px** ustawienie rozmiaru usługi radius i naciśnij klawisz **ENTER**.

    ![Zaokrąglone narożniki z tle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "zaokrąglone narożniki z cień")

    *Zaokrąglone narożniki z cień*

    > [!NOTE]
    > W tej chwili atrybutu cienia są wstawiane z odpowiedniego prefiksu (moz, webkit, o) do obsługi Mozilla i przeglądarki Webkit (Chrome, Safari, Konkeror).
7. Utwórz nową klasę **div.images ul li img:hover** poniżej **div.images ul li img** definicji klasy, umieść kursor w nawiasie **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Typ **przekształcenie** i naciśnij klawisz **kartę** klucza dwa razy, aby wstawić fragment kodu transformacji. Następnie wprowadź **rotate(-15deg)** Aby zmienić wartość kąta obrotu, gdy obrazy są aktywowany.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Naciśnij klawisz **F5** uruchomić rozwiązanie, a następnie przejdź do strony CSS3. Zauważyć, że obrazy mają zaokrąglone narożniki, a pole cieni. Umieść kursor myszy nad obrazów i oglądanie Obróć.

    ![Przekształć fragment obracanie obrazów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "fragment przekształcenia obracanie obrazów")

    *Przekształć fragment obracanie obrazów*

    > [!NOTE]
    > Jeśli używasz programu Internet Explorer 10 i cieni nie jest wyświetlane, upewnij się, że ustawienie trybu dokumentu Standardy programu IE10. Naciśnij klawisz **F12** Otwórz narzędzi deweloperskich programu Internet Explorer, a następnie kliknij przycisk **tryb dokumentu** można zmienić na standardy programu IE10.

    ![o-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Ćwiczenie 2: Nowości w edytorze HTML

Visual Studio ma ulepszone edytora HTML. Oto niektóre ulepszenia uwzględnione w tej wersji są inteligentne wcięcie w dokumentach HTML, fragmenty kodu HTML5, HTML start i dopasowania tagu końcowego i weryfikacji HTML. W ramach tego ćwiczenia zobaczysz, jak te zmiany ulepszyć Twoje fluency podczas pracy w znaczniku witryny sieci Web.

Podobnie jak edytor CSS poprawiła się również edytora HTML. Większość z nich to małych sieci, które ułatwić życia deweloperów sieci Web. Takie operacje, jak więcej wstawki kodu HTML5, inteligentne wcięcie dopasowania tagiem początkowym i końcowym podczas edycji i sprawdzania poprawności przeznaczonych dla dokumentu HTML DOCTYPE są niektóre z nich.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Zadanie 1 - ulepszone DOCTYPE weryfikacji

Edytora HTML ma teraz możliwość DOCTYPE strony, nawet jeśli definicja może być na stronie głównej. W zależności od typu dokumentu, strony edytora HTML będzie weryfikowane z poprawny zestaw reguł i spowoduje odfiltrowanie listy IntelliSense, biorąc pod uwagę elementy DOCTYPE.

W tym zadaniu zostanie zmieniony DOCTYPE strony, aby zobaczyć, jak zachowanie edytora HTML zmienia się odpowiednio.

1. Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.
2. Otwórz **Site.Master** strony.
3. Zwróć uwagę schemat docelowy dla narzędzi do sprawdzania poprawności. Dopasuj Doctype wybrane sposób zachowania edytora HTML (Sprawdzanie poprawności, IntelliSense, itp.) prawidłowo zostanie zmieniona.

    ![Użyj narzędzi edycji HTML źródła Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Doctype użyj narzędzi edycji źródła HTML")

    *Użyj Doctype pakietu na pasku narzędzi*
4. Zmień schemat docelowej w formacie HTML 4.01.

    ![Zmiana typu dokumentu pakietu narzędzi](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "zmiana typu dokumentu pakietu na pasku narzędzi")

    *Zmiana typu dokumentu pakietu na pasku narzędzi*
5. Umieść kursor w obszarze **treści** elementu, a następnie wpisz nazwę elementu HTML5 (na przykład **wideo**). Zwróć uwagę, że element nie jest dostępny na liście IntelliSense.

    ![Elementy HTML5 niewymienione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "elementy HTML5 niewymienione")

    *Elementy HTML5 niewymienione*
6. Cofnij zmiany schematu docelowej dla narzędzi sprawdzania poprawności pobrania DOCTYPE: XHTML5 z listy rozwijanej.

    ![Użyj narzędzi edycji HTML źródła Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Doctype użyj narzędzi edycji źródła HTML")

    *Resetowanie Doctype w narzędzi edycji źródła HTML*
7. Umieść kursor w obszarze **treści** element i wpisz HTML5 element ponownie (na przykład, takich jak **wideo**). Zwróć uwagę, elementy HTML5 są teraz dostępne na liście IntelliSense.

    ![Elementy HTML5 są wymienione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5 elementy są wymienione")

    *Elementy HTML5 są wymienione*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Zadanie 2 — rozpoczęcia i zakończenia znaczniki aktualizacji automatycznych

Visual Studio teraz aktualizuje HTML otwierania lub zamykania znaczniki elementu, którą edytujesz, aby były zgodne ze sobą. Ta nowa funkcja zwiększa produktywność podczas edytowania tagów HTML.

1. Na **Default.aspx** Dodaj **H3** element z tytułem (na przykład programu Visual Studio 2012 skały!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Zmień **H3** tagu i typ **H2** lub **H1.**

    Należy zauważyć, że tag końcowy automatycznie aktualizacji. Można również zmodyfikować tagu końcowego, aby zobaczyć, że tag początkowy odpowiednio aktualizowany za.

    ![Automatyczna aktualizacja tagu końcowego](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "automatyczną aktualizację do tagu końcowego")

    *Automatyczna aktualizacja tagu końcowego*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Zadanie 3 - nowych fragmentów kodu HTML5

Visual Studio teraz obejmuje kilka fragmentów kodu HTML5. W tym zadaniu użyjesz niektóre z tych fragmentów.

1. Dodaj nowy folder o nazwie **audio** do głównego folderu witryny sieci web. Otwórz Eksploratora Windows i skopiuj wszystkie plik dźwiękowy do **audio** folderu **WhatsNewASPNET.sln** rozwiązania.
2. W **Default.aspx** strony, odszukaj pozycję kursora w sieci Web 11 skały!! Nagłówek. Typ **audio** i naciśnij klawisz TAB.

    Nowe edytora HTML zawiera wstawki kodu HTML5 zawartości. Pamiętaj, aby Użyj właściwej definicji typu dokumentu, aby włączyć fragmenty kodu HTML5.

    ![Wstawianie wstawek kodu HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Wstawianie wstawek kodu HTML5")

    *Wstawianie wstawek kodu HTML5*
3. Aktualizowanie źródła audio, aby wskazywał istniejącego pliku audio.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Musisz dodać plik dźwiękowy do rozwiązania.
4. Naciśnij klawisz **F5** uruchamiania witryny i odtwarzanie dźwięku.

    ![Uruchomienie formantu audio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "systemem kontroli audio")

    *Uruchomienie formantu audio*

    > [!NOTE]
    > Można też spróbować więcej wstawki uwzględnione w programie Visual Studio ilustracji wideo, np.
5. Teraz spróbuj Wstawianie formantu w niektórych części strony. Na przykład próba wstawienia **widoku GridView** kontroli, ale zamiast wpisywać  **&lt;linie,** zacznij wpisywać ciąg  **&lt;GV**. Należy zauważyć, że lista IntelliSense zawiera **asp: GridView** formantu.

    IntelliSense w edytorze HTML udostępnia teraz wyszukiwania wielkości liter tytułu, a także częściowego dopasowania (pobieranie wszystkie elementy, które zawiera wyrażenie).

    ![Wstawianie GridView z listami IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Wstawianie GridView z listami IntelliSense")

    *Wstawianie GridView z listami IntelliSense*

    W przypadku wpisania  **&lt;siatki** otrzymasz wszystkich elementów, które odpowiadają termin, ale sugeruje Visual Studio **gridview** sterowania:

    ![Wstawianie Element GridView z listy IntelliSense i częściowe dopasowanie](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Wstawianie Element GridView z listy IntelliSense i częściowe dopasowania")

    *Wstawianie Element GridView z listy IntelliSense i częściowe dopasowania*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Zadanie 4 — tagów inteligentnych edytora HTML

Poprawa innego edytora HTML jest funkcji tagów inteligentnych. Tagi inteligentne ułatwiają do wykonywania zadań związanych z projektowaniem wspólnych lub powtarzających się na zasadzie-control. Ta funkcja została już dostępne w Projektancie HTML, ale nie w edytorze HTML.

1. Otwórz **Site.Master** i Znajdź **asp: Menu** elementu. Umieść kursor w tagu początkowym i zwróć uwagę, czy małych symbolu, wyświetlane w dolnej części elementu — kliknij go, aby otworzyć menu panelu inteligentnego. Zwróć uwagę, że masz dostęp do niektórych zadań związanych z Menu kontroli.

    ![Inteligentne zadań kontrolki Menu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "inteligentne zadań kontrolki Menu")

    *Inteligentne zadań kontrolki Menu*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Zadanie 5 - inteligentne wcięcie

Jeden z najlepszych rozwiązań w formacie HTML jest wcięcia elementów zagnieżdżonych, aby zachować kod do odczytu. W programie Visual Studio 2012 można zauważyć, że edytor automatycznie wcięcia elementów podczas pisania kodu.

> [!NOTE]
> W poprzedniej wersji programu Visual Studio, inteligentne wcięcie była dostępna w edytorze XML, ale nie w edytorze HTML.


1. Upewnij się, że konfiguracja Indenting w edytorze HTML ma ustawioną wartość inteligentne wcięcie. W tym celu wybierz **narzędzia | Opcje** , a następnie wybierz opcję menu **Edytor tekstu | HTML | Karty** strony w okienku po lewej stronie ekranu. Wybierz opcję inteligentne wcięcie.

    ![Ustawienia edytora HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "ustawienia edytora HTML")

    *Ustawienia edytora HTML*
2. Na **Default.aspx** pozycję Usuń całą zawartość w elemencie audio.
3. Umieść kursor na końcu otwarcia **audio** element i naciśnij klawisz **ENTER**.

    Zwróć uwagę, że nowe położenie kursora ma poziom dodatkowego wcięcia.

    ![Inteligentne wcięcie w edytorze HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "inteligentne wcięcie w edytorze HTML")

    *Inteligentne wcięcie w edytorze HTML*
4. Przywróć audio tagu z zawartością zostały usunięte lub Zamknij **Default.aspx** bez zapisywania zmian.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Zadanie 6 - Wyodrębnij do kontrolki użytkownika

Narzędzia Refactoring uwzględnione w programie Visual Studio, takich jak wyodrębnianie fragmentu kodu do funkcji, są wspaniałymi funkcjami, które ułatwiają poprawy i refaktoryzacji istniejącego kodu. Odpowiednikiem stron ASP.NET jest wyodrębniania kodu HTML do kontrolki użytkownika. To zrobić ręcznie wymagałoby kilku czynności, takie jak tworzenie nowej kontrolki użytkownika, przenoszenie sekcji kodu do kontrolki użytkownika, rejestrowanie prefiksu tagu kontrolki użytkownika i, koniec uruchamianiu formantu użytkownika na stronach. Teraz, nowe *Wyodrębnij do kontrolki użytkownika* narzędzie automatycznie wykonuje te kroki dla Ciebie.

To zadanie będzie używał nowego wyodrębniania operacji kontekstowe kontrolki użytkownika do generowania nową kontrolkę użytkownika z zaznaczonym kodzie.

1. Na **Default.aspx** wybierz pozycję **H2** i **audio** elementów.
2. Kliknij prawym przyciskiem myszy i wybierz **Wyodrębnij do kontrolki użytkownika**.

    ![Wyodrębnij do kontrolki użytkownika menu opcji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Wyodrębnij do kontrolki użytkownika opcji menu")

    *Wyodrębnij do kontrolki użytkownika opcji menu*
3. Wpisz nazwę nowego formantu użytkownika. Na przykład **Jukebox.ascx**, a następnie kliknij przycisk **OK**.

    ![Zapisywanie kontrolki użytkownika wyodrębnionego](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "zapisywania kontrolki wyodrębnionego użytkownika")

    *Zapisywanie kontrolki wyodrębnionego użytkownika*
4. Zauważyć, że zaznaczony kod został wyodrębniony do kontrolki użytkownika, a Pierwotna lokalizacja zaznaczony kod został zastąpiony wystąpienia nowe kontrolki użytkownika.

    ![Strona automatycznie aktualizowane do użycia nowej kontrolki użytkownika](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "strony są automatycznie aktualizowane do użycia nowej kontrolki użytkownika")

    *Strona automatycznie aktualizowane do użycia nowej kontrolki użytkownika*
5. Naciśnij klawisz **F5** do uruchomienia strony i sprawdź, czy formant działa.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Ćwiczenie 3: Nowości w edytorze kodu JavaScript

Zapisywanie lub edycji kodu JavaScript nie jest łatwym zadaniem, szczególnie, gdy aplikacja rozpoczyna się zwiększa się rozmiar i okaże się, że zajmowanie długie pliki oraz setki funkcji. Zwykle programiści skryptu muszą wykonywania dodatkowej pracy, aby zachować czytelność kodu i przechodzić między plików. Obejmująca biblioteki języka JavaScript, takich jak jQuery nawigacji skryptu stał się żądanie się z powodu długość kodu.

Visual Studio ma odnowiony edytora kodu JavaScript z promise dokonanie tryb kodu, dostępnych i organizowane. Wiele funkcji programu Visual Studio, które istniały w językach C# i VB edytory są teraz zaimplementowane w edytorze kodu JavaScript: Przejdź do definicji, automatyczne wcięcia, dokumentacji i sprawdzania poprawności podczas pisania. Z listy IntelliSense odnowionego będziesz mieć katalogu funkcji JavaScript w zasięgu ręki.

W tym ćwiczeniu dowiesz się, niektóre nowe funkcje i ulepszenia edytora kodu JavaScript. Zostanie przeglądanie przykładowych plików i odnajdywanie wszystkich nowych cech, które spowoduje, że Twoje programowania w języku JavaScript większą wydajność w programie Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Zadanie 1 — nowe funkcje edytora JavaScript

To zadanie przedstawiono niektóre nowe funkcje edytora JavaScript, które skupić się na przechowywanie kodu i dostarczają lepsze środowisko pracy użytkownika.

1. Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.
2. Naciśnij klawisz **F5** do uruchomienia aplikacji, a następnie kliknij łącze JavaScript na pasku nawigacyjnym. Odśwież stronę kilka razy i sprawdź, jak zwiększa licznik.

    ![Licznik strony](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "licznika na stronie")

    *Licznik na stronie*
3. Zamknij przeglądarkę i przejdź wstecz do programu Visual Studio.
4. Otwórz **JavaScript.aspx** strony i Znajdź **&lt;skryptu&gt;** bloku (pokazana poniżej).

    W poniższym kodzie użyto HTML5 lokalnego magazynu do przechowywania *pageLoadCount* zmiennej, która przechowuje numer odwiedza strony przez bieżącego użytkownika. Magazyn lokalny jest bazą danych klucz wartość po stronie klienta wprowadzone ze standardem HTML5. Dane są zapisywane na komputerze lokalnym, w przeglądarce.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Upewnij się, że DOCTYPE ustawiono XHTML5 przed wykonaniem dalszych kroków.
5. Edytuj kod i zwróć uwagę, że IntelliSense dla JavaScript zawiera funkcje HTML5, takie jak i ich metod wewnętrzny magazyn lokalny.

    ![Funkcje HTML5 JavaScript w języku JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "funkcji HTML5 JavaScript w języku JavaScript")

    *Funkcje HTML5 JavaScript w języku JavaScript*
6. Kliknij żadnych nawiasu otwierającego (**{**) z skryptów kodu i zwróć uwagę, że nawiasy są wyróżnione.

    ![Nawiasy kwadratowe są wyróżnione](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "nawiasy kwadratowe są wyróżnione")

    *Nawiasy kwadratowe są wyróżnione*
7. Usuń znaczniki komentarza funkcji **testAutoAlign()** (Wybierz trzy wiersze i można użyć **CTRL** + **K**; **CTRL** + **U**) i Znajdź kursora wewnątrz kodu funkcji. Naciśnij klawisz enter, aby można było dodać drugi wiersz. Należy zauważyć, że kod jest teraz **wyrównane** i **wcięty automatycznie**.

    ![Kod JavaScript jest automatycznie wyrównane](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "kod JavaScript jest automatycznie wyrównane")

    *Kod JavaScript jest automatycznie wyrównane*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Zadanie 2 — Sprawdzanie poprawności kodu JavaScript

W tym zadaniu odnajdzie nowy walidacji JavaScript dla standardowej ECMAScript5. Ta funkcja będzie ułatwia pisanie zgodne kod JavaScript, podczas zapobiega zagadnienia dotyczące skryptów przed wdrożeniem witryny.

> [!NOTE]
> Program Visual Studio 2010 zaimplementowana zgodności ECMAStript3 programu Visual Studio 2012 zapewnia zgodność ECMAScript5 jednocześnie.


1. Otwórz **ECMA5script5.js** znajduje się w obszarze **Scripts\custom** folderu projektu. Teraz zostanie testów sprawdzania poprawności dla ECMAScript5 standardowe.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Można wyewidencjonować &quot; **używaj z ograniczeniami** &quot; kierunek, w pierwszym wierszu pliku, który umożliwia ECMAScript5 **tryb z ograniczeniami**. W tym trybie składa się z podzbioru języka, który wyjaśnia niejednoznaczności z poprzednich wersji i dodaje nowe funkcje, takie jak metody pobierające i ustawiające, obsługa bibliotek dla formatu JSON, a dokładniejsza odbicia właściwości obiektu.
2. Otwórz **listy błędów** Jeśli nie jest jeszcze otwarty (**widoku** menu | **Listy błędów**). Powiadomienie **funkcja** deklaracji jest podkreślony. Jest to spowodowane w ECMA5 standardowych funkcji nie mogą być zagnieżdżone wewnątrz struktury języka. W dzienniku błędów poniżej możesz pojawią się szczegóły ostrzeżenie.

    ![Komunikat o błędzie weryfikacji JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "komunikat o błędzie weryfikacji JavaScript")

    *Komunikat o błędzie weryfikacji JavaScript*
3. Komentarz **&quot;używaj z ograniczeniami&quot;** kierunek i zwróć uwagę, że błędy znikają, przy zachowaniu ostrzeżenia.
4. W ostatnim wierszu pliku, należy zapisać dowolny ciąg, takich jak **&quot;test&quot;** (należy używać znaków cudzysłowu, aby wskazać ją jako ciąg). Zapis okresu obok ciąg do wyświetlania na liście IntelliSense i wybierz **trim** opcji.

    W standardzie ECMAScript5 wartości parametrów i zmiennych również mieć zdefiniowane, takich jak trim, wielkie litery, wyszukiwania i zamieniania metod ciągów.

    ![Lista IntelliSense w języku JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "listy IntelliSense w języku JavaScript")

    *Lista IntelliSense w języku JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Zadanie 3. plik dokumentacji XML JavaScript

W tym zadaniu zostanie Poznaj funkcje programu Visual Studio dla dokumentacji XML w języku JavaScript. Zobaczysz, że lista IntelliSense dla JavaScript zawiera teraz dokumentacji XML każdej funkcji. Ponadto możesz zauważyć funkcję nawigacji w języku JavaScript.

1. Otwórz **XMLDoc.js** plik znajdujący się w **skryptów/niestandardowa** folderu projektu. Ten plik zawiera dokumentacja XML na każdej z tych funkcji JavaScript.

    ![Plik dokumentacji JavaScript XML zintegrowany z IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "dokumentacji JavaScript XML zintegrowany z IntelliSense")

    *Plik dokumentacji JavaScript XML zintegrowany z IntelliSense*
2. Poniżej **dodać** działać w **XMLDoc.js** plików, Utwórz nową funkcję o nazwie **testu**.
3. W **test** funkcji, należy wywołać **mnożenia** funkcja, która otrzymuje dwa parametry. Zwróć uwagę, pole tooltip jest pokazywany **mnożenia** funkcji dokumentacji.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Plik dokumentacji XML dla funkcji JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "dokumentacji XML dla funkcji JavaScript")

    *Plik dokumentacji XML dla funkcji JavaScript*
4. Zakończenie instrukcji call funkcji i typ *kropka* aby otworzyć listę IntelliSense w zwracanej wartości. Należy zauważyć, że program Visual Studio jest wykrywanie **zwracać** wartość w dokumentacji, traktując wartości jako liczby.

    ![Plik dokumentacji XML dla zwracanych typów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "dokumentacji XML dla zwracane typy")

    *Plik dokumentacji XML dla zwracane typy*
5. Teraz Wstawianie wywołania do add — funkcja. Zwróć uwagę, że edytor JavaScript obsługuje teraz przeciążenia funkcji. Podczas pisania nazwę funkcji, można wybrać dowolny z dostępnych przeciążeń określony w dokumentacji.

    ![Dokumentacja XML do przeciążenia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "dokumentacji XML dla przeciążenia")

    *Dokumentacja XML do przeciążenia*
6. Otwórz **GotoDefinition.js** plików i Znajdź **$().html()** wywołania funkcji. Zlokalizuj kursor na **html**.
7. Naciśnij klawisz **F12** i przejdź do definicji. Powiadomienia można teraz uzyskać dostępu i przeglądać kod JavaScript bez użycia **znaleźć** narzędzia.
8. Zlokalizuj kursor w instrukcji jQuery przed blok podpisu w dolnej części pliku kodu. Naciśnij klawisz **F12**. Nastąpi przeniesienie do pliku biblioteki jQuery. Zwróć uwagę, można także przechodzić między pliki jQuery przy użyciu **F12**.

    ![Przechodzenie do definicji jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "przejść do definicji jQuery")

    *Przechodzenie do definicji jQuery*

> [!NOTE]
> Upewnij się, że GotoDefinition.js nie zawiera składnię błędów przed zapisaniem pliku.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Ćwiczenie 4: Tworzenie pakietów i minimalizowanie

Ile razy witryny sieci Web ma więcej niż jednego języka JavaScript lub CSS pliku? Jest to bardzo typowy scenariusz, w którym tworzenie pakietów i minimalizowanie może pomóc zmniejszyć rozmiar pliku i witryna szybsze. Nowa funkcja powiązanego w programie ASP.NET 4.5 pakiety zestawu plików JS i CSS do pojedynczego elementu i zmniejsza rozmiar przez zminimalizowania liczby zawartości (tj. usuwanie spacje nie są wymagane, usuwanie komentarzy, zmniejszając identyfikatory).

Tworzenie pakietów i minimalizowanie w programie ASP.NET 4.5 jest wykonywane w czasie wykonywania, aby proces identyfikowania agent użytkownika (na przykład programu Internet Explorer, Mozilla itp.), a w związku z tym poprawy kompresji, wybierając w przeglądarce użytkownika (na przykład usuwania rzeczy będący Mozilla określonych Jeśli żądanie pochodzi z programu Internet Explorer).

W tym ćwiczeniu dowiesz się, jak włączyć i korzystać z różnego rodzaju tworzenie pakietów i minimalizowanie w programie ASP.NET 4.5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Zadanie 1 — Instalowanie tworzenie pakietów i minimalizowanie pakiet NuGet

1. Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.
2. Otwórz konsolę Menedżera pakietów NuGet. Aby to zrobić, użyj menu **widoku** | **inne okna** | **Konsola Menedżera pakietów**.

    ![Otwieranie pakietu file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole Menedżera](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "otwarcie konsoli Menedżera pakietów")

    *Otwieranie konsoli Menedżera pakietów*
3. W **Konsola Menedżera pakietów,** typu **Microsoft.Web.Optimization Install-Package** i naciśnij klawisz **ENTER**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Zadanie 2 — domyślne pakiety

Najprostszym sposobem użycia tworzenie pakietów i minimalizowanie jest umożliwienie pakiety z domyślnej. Ta metoda używa konwencji, aby można było odwołać powiązane i zminimalizowany wersji JS i CSS pliki w folderze.

W tym zadaniu dowiesz się, jak włączyć i powiązane i zminimalizowany plików JS i CSS i wyświetlić dane wyjściowe.

1. Jeśli nie jest jeszcze otwarty, uruchom **programu Visual Studio** , a następnie otwórz **WhatsNewASPNET.sln** rozwiązania, znajdujących się w **Source\WhatsNewASPNET** folder tego laboratorium.
2. W **Eksploratora rozwiązań**, rozwiń węzeł **style**, **Scripts\custom** i **Scripts\bundle** folderów.

    Zwróć uwagę, że aplikacja używa pliku więcej niż jeden CSS i JS.

    ![Pliki wielu arkusze stylów i JavaScript w aplikacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "pliki wielu arkusze stylów i JavaScript w aplikacji")

    *Wiele plików arkusze stylów i JavaScript w aplikacji*
3. Otwórz **Global.asax.cs** pliku.

    Zwróć uwagę, że nowe **Microsoft.Web.Optimization** przestrzeni nazw jest oznaczone jako komentarz na początku pliku. Usuń znaczniki komentarza użycie dyrektywy w celu włączenia funkcji Tworzenie pakietów i minimalizowanie.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Zlokalizuj **aplikacji\_Start** metody.

    W przypadku tej metody usuń znaczniki komentarza wywołania EnableDefaultBundles, jak pokazano w poniższy fragment. Pozwala na odwołanie zbiór powiązane pliki CSS w folderze przy użyciu ścieżki do tego folderu i &quot;CSS&quot; lub &quot;JS&quot; sufiks.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Otwórz **Optimization.aspx** pliku, a następnie zlokalizuj formant zawartości dla **HeadContent**.

    Zwróć uwagę, pliki CSS i JS zawierać jeden tag do którego istnieje odwołanie.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Ten kod jest dla celów demonstracyjnych. W idealnym przypadku będzie odwoływać pakietów w pliku Site.Master. W ten przykładowy kod można znaleźć czy niektóre pliki w pakiecie są również odwołuje pliku Site.Master odniesienia tego ostatniego nadmiarowe.
6. Należy zauważyć, że łącza z powiązanego konwencje w **href** atrybutu Pobierz pliki CSS i Javascript z style i Scripts\custom folderu odpowiednio.

    Można użyć ścieżki **skryptów/niestandardowe/JS** przedstawioną poniżej połączyć w paczkę i zminimalizowania wszystkie pliki JS wewnątrz **skryptów/niestandardowa** folderu. Jest to domyślne zachowanie z pakietami domyślne.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Otwórz **Styles\Site.css** pliku.

    Zwróć uwagę, że oryginalnego pliku CSS zawiera kod wcięta, spacje i komentarze, które powiększyć plik. (Również JavaScript zawiera spacje i komentarze).

    ![Jeden z oryginalnego CSS pliki w folderze skryptów](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "jedną z oryginalnego CSS pliki w folderze skryptów")

    *Jeden z oryginalnych plików CSS w folderze skryptów*
8. Naciśnij klawisz **F5** do uruchamiania aplikacji i przejdź do **optymalizacji** strony.
9. Polecenie **pakietu CSS** łącze aby pobrać i otworzyć plik.

    Wyewidencjonuj plik powiązane zminimalizowany. Można zauważyć, że wszystkie spacje, komentarze i wcięcia znaków zostały usunięte, generowanie mniejszy plik.

    ![Powiązane pliki CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "pliki CSS powiązane")

    *Powiązane pliki CSS*
10. Teraz kliknij **pakietu JS** łącze, aby otworzyć plik JavaScript powiązane. Można bezpiecznie zignorować explorer ostrzeżenie. Zwróć uwagę, pliki JavaScript w **niestandardowych** folderu są również powiązane i zminimalizowany.

    ![Powiązane pliki JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "pliki pakietu języka JavaScript")

    *Powiązane pliki JavaScript*

    Włączanie kompresji plików CSS i Javascript w poprzedniej wersji programu ASP.NET została znacznie bardziej skomplikowane. Teraz, jak już wspomniano, wystarczy dodać jeden wiersz w *Global.asax* plików, aby włączyć funkcję tworzenia pakietów i odwoływanie powiązane pliki z witryny.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Zadanie 3 — statyczne pakietów

Metody statyczne pakietu umożliwia dostosowanie zestawu plików do pakietu, odwołanie i metody minimalizację, który będzie używany.

W tym zadaniu skonfiguruj statyczny pakietu do definiowania określonego zestawu plików pakietu do zminimalizowania.

1. Zamknij przeglądarkę.
2. Otwórz **Global.asax.cs** plików i Znajdź **aplikacji\_Start** metody.
3. Usuń komentarz kodu pakietu statycznych, jak pokazano w poniższym kodzie.

    Definiowania statycznego pakiet, który zostanie dodane odwołanie z &quot; **~/StaticBundle** &quot; ścieżki wirtualnej i użyj **JsMinify** dla minimalizację wszystkie określone pliki z **AddFile** metody. Ponadto w przypadku dodawania statycznych pakietu do **BundleTable** i włączenie go.

    Zwróć uwagę, że pliki nie znajdują się w tym samym miejscu; jest to możliwości innego niż domyślny, tworzenie pakietów.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Otwórz **Optimization.aspx** pliku.

    Zwróć uwagę, że łącze do **statycznych pakietu JS** jest przy użyciu ścieżki zadeklarowaniu podczas konfigurowania statycznego pakietu w pliku Global.asax.cs: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Naciśnij klawisz **F5** do uruchomienia aplikacji, a następnie przejdź do **optymalizacji** strony.
6. Polecenie **statycznych pakietu JS** łącze, aby otworzyć plik.

    Powiadomienie zminimalizowany powiązane plik JavaScript jest wyjściem dla wszystkich plików JavaScript skonfigurowany w pliku statycznego pakietu w ścieżce &quot;/StaticBundle&quot;.

    ![Statyczne pakietu plików JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "pakietu plików statycznych JavaScript")

    *Pliki statyczne JavaScript pakietu*
7. Zamknij przeglądarkę i powrócić do programu Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Zadanie 4 — pakietów dynamicznych folderu

W tym zadaniu dowiesz się, sposobu konfigurowania pakietów dynamicznych folderu. Moc dynamiczne tworzenie pakietów jest obejmują statycznych JavaScript, a także inne pliki w językach, które kompiluje na język JavaScript, a w związku z tym wymagają niektórych przetwarzania przed wykonaniem tworzenie pakietów.

W w tym przykładzie przedstawiono sposób użycia **DynamicFolderBundle** klasa do tworzenia dynamicznych pakietu plików napisana CofeeScript. CofeeScript jest język programowania, który kompiluje się na język JavaScript i zapewnia prostszy składni pisanie kodu JavaScript, pominiemy i czytelność języka JavaScript.

1. Otwórz **Global.asax.cs** plików i Znajdź **aplikacji\_Start** metody.
2. Usuń komentarz kodu dynamiczne pakietu, jak pokazano w poniższym kodzie.

    Definiujesz pakietu dynamiczne folderu, który będzie używany przez **CoffeeMinify** procesora minimalizację niestandardowego, który będzie dotyczyć tylko pliki z &quot; **.coffee** &quot; (rozszerzenia Pliki języka CoffeeScript). Powiadomienia, której można wzorzec wyszukiwania, aby wybrać pliki pakietów w folderze, takie jak "\*.coffee".

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Otwórz konsolę Menedżera pakietów NuGet. Aby to zrobić, użyj menu **widoku** | **inne okna** | **Konsola Menedżera pakietów**.
4. W **Konsola Menedżera pakietów,** typu **CoffeeSharp Install-Package** i naciśnij klawisz **ENTER**.
5. Kliknij przycisk **Pokaż wszystkie pliki** przycisk **Eksploratora rozwiązań** okna

    ![Wyświetlanie wszystkich plików](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "wyświetlanie wszystkich plików")

    *Wyświetlanie wszystkich plików*
6. Kliknij prawym przyciskiem myszy **CoffeeMinify.cs** w pliku **Eksploratora rozwiązań** i wybierz **Include w projekcie**

    ![W projekcie umieścić plik CoffeeMinify.cs](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "dołączyć plik CoffeeMinify.cs w projekcie")

    *Dołączenie pliku CoffeeMinify.cs w projekcie*
7. Otwórz **CoffeeMinify.cs** pliku.

    Ta klasa dziedziczy JsMinify do zminimalizowania dane wyjściowe JavaScript wynikające z CoffeeScript kompilacji kodu. Wywołuje kompilatora języka CoffeeScript do generowania kodu JavaScript najpierw, a następnie wysyła go do metody JsMinify.Process w celu zminimalizowania wynikowy kod.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Otwórz **Script1.coffee** i **Script2.coffee** plików ze **skryptów/pakietu** folderu.

    Te pliki zostaną uwzględnione kodu CoffeScript ma być kompilowana podczas wykonywania, tworzenie pakietów przy użyciu klasy CoffeeMinify.

    Dla uproszczenia CoffeeScript pliki udostępniane są tylko tym CoffeeScript przykładowy kod. Komentarze są wyłączone przez proces JsMinify.

    ![Pliki języka CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "pliki języka CoffeeScript")

    *Pliki języka CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) zapewnia prostszy składni pisanie kodu JavaScript, skrócenia języka JavaScript i czytelność, a także dodawanie innych funkcji, takich jak tablicy zrozumienia i dopasowywanie do wzorca.
9. Otwórz **Optimization.aspx** plików i Znajdź łącza pakietu.

    Zwróć uwagę, że łącze do **dynamiczne pakietu JS** odwołuje się do **skryptów/pakietu** folder przy użyciu **/kawy** sufiks skonfigurowany dla pakietu folderu dynamicznych.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Naciśnij klawisz **F5** do uruchomienia aplikacji, a następnie przejdź do **optymalizacji** strony.
11. Polecenie **dynamiczne pakietu JS** łącze, aby otworzyć wygenerowanego pliku.

    Należy zauważyć, że zawiera tylko zawartość, która została uwzględniona w tym pakiecie **.coffee** plików. Można również sprawdzić, czy kod języka CoffeeScript został skompilowany dla JavaScript i linie poza komentarzem został usunięty.

    ![Dynamiczne pliki JS pakietu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "JS dynamiczne pliki pakietu")

    *Dynamiczne pakietu plików JS*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym laboratorium pomaga zrozumieć, co jest nowego w programie ASP.NET i aplikacji sieci Web w programie Visual Studio 2012 oraz sposób korzystać z różnego rodzaju rozszerzeń w programie Visual Studio 2012.

Wykonując tego laboratorium Hands-On ma doświadczeń sposób użycia nowe funkcje i ulepszenia w Visual Studio 2012 edytory CSS, JavaScript i HTML. Ponadto zostały wcześniej, jak Visual Studio 2012 implementuje wbudowanych tworzenie pakietów i minimalizowanie.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Instalowanie programu Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "instalacji programu Visual Studio Express")

    *Instalowanie programu Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express for Web kafelka*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure

1. Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu. Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Zaloguj się do portalu Windows Azure")

    *Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*
2. Kliknij przycisk **nowy** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczeniowe** | **witryny sieci Web**. Następnie wybierz **szybkie tworzenie** opcji. Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.

    > [!NOTE]
    > Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj na nowe **witryny sieci Web** jest tworzony.
5. Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny. Sprawdź, czy działa nowej witryny sieci Web.

    ![Przeglądanie do nowej witryny sieci web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "przeglądanie do nowej witryny sieci web")

    *Przeglądanie do nowej witryny sieci web*

    ![Witryna sieci Web działa](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "uruchamiania witryny sieci Web")

    *Witryna sieci Web uruchomiona*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie stron witryny sieci web zarządzania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie stron zarządzania witryny sieci Web*
7. W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.

    > [!NOTE]
    > *Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.

    ![Pobieranie witryny sieci web profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz profil publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.

    ![Zapisywanie pliku profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji. Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**. Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania. Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.

    ![Pulpit nawigacyjny serwera bazy danych SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny serwera bazy danych SQL*
2. W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pól tekstowych. Wprowadź nazwę reguły, a następnie kliknij przycisk ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) przycisku.

    ![Dodawanie adresu IP klienta](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.

    ![Potwierdzenie zmian](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Potwierdzenie zmian*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.

    ![Publikowanie aplikacji](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.

    ![Importowanie profilu publikowania](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **Weryfikacja połączenia z**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.

    ![Sprawdzanie poprawności połączenia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.
   - W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.
   - W **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.

     ![Konfigurowanie parametrów połączenia z lokalizacją docelową](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")

     *Konfigurowanie parametrów połączenia z lokalizacją docelową*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.

    ![Tworzenie bazy danych](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "tworzenie parametry bazy danych")

    *Tworzenie bazy danych*
7. Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie. Następnie kliknij przycisk **Dalej**.

    ![Parametry połączenia wskazujące bazę danych SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "ciąg połączenia wskazujące bazę danych SQL")

    *Parametry połączenia wskazujące bazę danych SQL*
8. W **Podgląd** kliknij przycisk **publikowania**.

    ![Publikowanie aplikacji sieci web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

    ![Aplikacja opublikowana w systemie Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "aplikacji publikowanych w systemie Windows Azure")

    *Aplikacji publikowanych w systemie Windows Azure*
