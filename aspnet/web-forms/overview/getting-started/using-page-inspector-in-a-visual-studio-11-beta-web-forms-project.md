---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Za pomocą narzędzia Page Inspector dla programu Visual Studio 2012 we wzorcu ASP.NET Web Forms | Dokumentacja firmy Microsoft
author: rick-anderson
description: Narzędzie Page Inspector dla programu Visual Studio 2012 jest narzędziem do projektowania sieci web za pomocą zintegrowanej przeglądarki. Wybierz dowolny element w zintegrowanej przeglądarki i narzędzie Page Inspector...
ms.author: aspnetcontent
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8e914b87305fa729659822ec1166e9d1947e59cb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806277"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Za pomocą narzędzia Page Inspector dla programu Visual Studio 2012 w formularzach sieci Web platformy ASP.NET
====================
przez Tim Ammann

> Narzędzie Page Inspector dla programu Visual Studio 2012 jest narzędziem do projektowania sieci web za pomocą zintegrowanej przeglądarki. Wybranie dowolnego elementu w zintegrowanej przeglądarki i narzędzie Page Inspector natychmiast wyróżnia elementu źródłowego i arkusze CSS. Można przeglądać dowolnej strony w aplikacji, szybkie znalezienie źródła renderowanego kodu znaczników i użyj narzędzia przeglądarki bezpośrednio w środowisku Visual Studio.
> 
> Ten samouczek shwos jak włączyć tryb inspekcji i następnie szybko zlokalizować i edytować reguły CSS i tekstu w projekcie sieci web. W tym samouczku użyto projektu aplikacji formularzy sieci Web, ale można również użyć narzędzia Page Inspector dla projektów witryny sieci Web i [MVC](https://go.microsoft.com/?linkid=9802002) aplikacji.
> 
> Samouczek zawiera następujące sekcje:
> 
> [Wymagania wstępne](#_1_prerequisites)
> 
> [Tworzenie aplikacji sieci Web](#_2_creating_a)
> 
> [Inspektor stron umożliwia wyświetlanie aplikacji](#_3_using_page)
> 
> [Włącz tryb inspekcji](#_4_inspection_mode)
> 
> [Inspektor stron umożliwia wprowadzać zmiany znaczników](#_5_using_page)
> 
> [Tryb inspekcji i w oknie kodu HTML](#_6_inspection_mode)
> 
> [Zmiany arkusza CSS (wersja zapoznawcza) w oknie style](#_7_previewing_css)
> 
> [Automatyczna synchronizacja z CSS](#css_auto_sync)
> 
> [Za pomocą selektora kolorów CSS](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

- [Program Visual Studio 2012](https://www.microsoft.com/visualstudio/11) lub [programu Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Aby uzyskać najnowszą wersję narzędzia Page Inspector, użyj [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=255386) do zainstalowania zestawu Azure SDK dla platformy .NET w wersji 2.0.


Narzędzie Page Inspector jest umieszczany w pakietach za pomocą narzędzia Microsoft Web Developer Tools. Najnowsza wersja to 1.3. Aby sprawdzić, która wersja mają, uruchom program Visual Studio i wybierz **Microsoft Visual Studio** z **pomocy** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Tworzenie aplikacji sieci Web

Najpierw należy utworzyć aplikacji sieci web, którego będziesz używać narzędzia Page Inspector za pomocą. W programie Visual Studio, wybierz **pliku** &gt; **nowy projekt**. Po lewej stronie, rozwiń węzeł **Visual C#**, wybierz opcję **Web**, a następnie wybierz pozycję **aplikacji formularzy sieci Web ASP.NET**.

![Nowa aplikacja formularzy sieci Web](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Kliknij przycisk **OK**.

Aplikacja zostanie otwarta w **źródła** widoku.

![Nowa aplikacja formularzy sieci Web w widoku źródła](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Teraz, gdy masz już aplikację, chcesz pracować, można użyć narzędzia Page Inspector do zbadania i zmodyfikowania go.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Inspektor stron umożliwia wyświetlanie aplikacji

Następnie zostaną wyświetlone aplikację za pomocą narzędzia Page Inspector. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie wybierz **widoku w narzędzia Page Inspector**.

![Wyświetl w narzędzia Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Domyślnie gdy narzędzie Page Inspector uruchamia się po raz pierwszy jest zadokowany jako wąskie okno po lewej stronie w środowisku Visual Studio. Pozostaw zadokowane po lewej stronie, a następnie ustaw szerokość, która jest wygodny dla Ciebie, albo zadokować razem w jednym z obszarów narzędzia na górze, dołu lub prawej strony:

![Narzędzie Page Inspector pozycjach.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Jeśli można oddokować okno narzędzia Page Inspector, należy go umieścić poza programem Visual Studio lub nawet na drugim monitorze Jeśli nie masz. Jednak aby klawisze ALT + TAB między narzędzia Page Inspector i programu Visual Studio podczas okna narzędzia Page Inspector jest zadokowany, przejdź do **narzędzia** &gt; **opcje** &gt;  **Środowiska** &gt; **karty i Windows**, a następnie w obszarze **kartę dobrze**, wyczyść pole wyboru o nazwie **zawsze w górnej części okna przestawne Okno główne**:

![Wyczyść zmiennoprzecinkowy narzędzie windows pola wyboru ALT + TAB między Visual Studio i niezadokowane okna narzędzia Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Górne okienko okna narzędzia Page Inspector pokazuje bieżącej strony w oknie przeglądarki. Dolne okienko zawiera strony w kod znaczników HTML po lewej stronie, a niektóre karty po prawej stronie, dzięki którym możesz sprawdzić różne aspekty strony. Dolne okienko jest podobny do [narzędzi deweloperskich F12](https://msdn.microsoft.com/ie/aa740478) w programie Internet Explorer. (Jednak w przeciwieństwie do narzędzia dla deweloperów, możesz użyć narzędzia Page Inspector bezpośrednio w programie Visual Studio.)

![Inspektor strony](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

W tym samouczku użyjesz narzędzia Page Inspector okienku przeglądarki i **HTML** i **style** kart, aby ułatwić szybkie nawigowanie i wprowadzać zmiany w aplikacji.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Włącz tryb inspekcji

Następnie zobaczysz, jak działa narzędzie Page Inspector tryb inspekcji. Kliknij w oknie narzędzia Page Inspector **Sprawdź** przycisku.

![Zbadaj Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Aby wyświetlić tryb inspekcji w działaniu, przesuń mysz, za pośrednictwem różnych części strony w oknie przeglądarki narzędzia Page Inspector. To zrobisz, kursor zmienia się na znak plus w dużych, a jest wyróżniony element poniżej:

![Kursor div.content otoki](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Podczas przesuwania wskaźnika myszy, należy pamiętać, że

- Zawartość **źródła** wyświetlić zmiany, aby pokazać znaczników odpowiadający wybranego elementu na stronie. Odpowiednie znaczników jest wyróżniona. Jeśli źródło jest w innym pliku, ten plik jest otwarty w widoku źródła ze znacznikami odpowiednich wyróżnione.

- Znaczniki wyświetlane w **HTML** karcie narzędzie Page Inspector zmienia także odnoszą się do wybranego elementu na stronie. W **HTML** karcie zawarto istotne znaczników.

- **Style** karta zawiera reguły CSS odpowiednie do bieżącego zaznaczenia.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Inspektor stron umożliwia wprowadzać zmiany znaczników

Teraz zobaczysz, jak można użyć narzędzia Page Inspector można znaleźć i wprowadzać zmiany znaczników lub tekst, którego lokalizacja może nie być od razu oczywista.

Umieść narzędzie Page Inspector w trybie inspekcji, a następnie przewiń do dolnej części strony głównej.

Zaraz po wprowadzeniu stopka obszaru, zostanie otwarte narzędzie Page Inspector *Site.Master* pliku układu **źródła** widoku w karcie tymczasowej na prawo od innych kart i wyróżnienie sekcji wzorca strony, wybrane. To pokazuje jak narzędzie Page Inspector można znaleźć i wyświetlić zawartość na stronie, która faktycznie mogą pochodzić z różnych plików niż ten, który pierwotnie został otwarty.

![Najważniejsze stopki w trybie inspekcji](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

W oknie przeglądarki, narzędzie Page Inspector, umieść wskaźnik myszy nad wierszem o prawach autorskich <a id="a"> </a>Zwróć uwagę.

W *Site.Master* stronie odpowiednim wierszu zostanie wyróżniony.

![Stopka wiersz o prawach autorskich wyróżniony](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Dodaj jakiś tekst do końca wiersza w *Site.Master* pliku.

&lt;p&gt;&amp;kopiowania; &lt;%: DateTime.Now.Year %&gt; — Moje Rocks aplikacji ASP.NET!&lt; /p&gt;

Teraz naciśnij klawisze Ctrl + Alt + Enter lub kliknij pasek aktualizacji, aby wyświetlić wyniki w oknie przeglądarki, narzędzie Page Inspector.

![Moje ASP.NET aplikacji Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Może mieć uważasz, czy stopka została na *Default.aspx* strony, ale okazała się na stronie układem głównym i narzędzie Page Inspector uznała, że dla Ciebie.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Tryb inspekcji i w oknie kodu HTML

Następnie będziesz mieć rzut oka na oknie HTML oraz sposób mapowania elementów dla Ciebie.

Narzędzie Page Inspector należy umieścić w trybie inspekcji.

![Zbadaj Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Kliknij w górnej części strony, który jest wyświetlany komunikat "Twoje logo". To badanie konkretnego elementu bardziej szczegółowo, aby wyświetlana w oknie przeglądarki nie jest już zmienia się po przesunięciu wskaźnika myszy.

Teraz umieść kursor myszy na **HTML** okna. Podczas przesuwania wskaźnika myszy, narzędzie Page Inspector przedstawia element w obrębie **HTML** okna i odpowiadający mu element w oknie przeglądarki.

![Okno w formacie HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Jak wcześniej, zostanie otwarte narzędzie Page Inspector *Site.Master* plik w karcie tymczasowej. Kliknij kartę Site.Master i odpowiedni kod znaczników jest wyróżniony na &lt;nagłówka&gt; sekcji:

![Wyróżnione znaczników](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Zmiany arkusza CSS (wersja zapoznawcza) w oknie style

Następnie zobaczysz, jak można użyć narzędzia Page Inspector **style** okna, aby wyświetlić podgląd zmian do arkusza CSS.

Kliknij przycisk **Sprawdź** przycisk, aby umieścić narzędzie Page Inspector w trybie inspekcji.

W oknie przeglądarki, narzędzie Page Inspector, umieść kursor myszy na sekcję "Home Page" do momentu **div.content otoki** etykieta jest wyświetlana.

![Przenosząc kursor myszy nad elementami](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Kliknij wewnątrz sekcji otoki div.content jeden raz, a następnie przesuń wskaźnik myszy na **style** okna. W obszarze selektora klasy otoki .content .featured Wyczyść, a następnie zaznacz pole wyboru dla właściwości kolor tła.

![Kolor tła wyczyść](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Zwróć uwagę, jak zmiany zawiera wersje zapoznawcze natychmiast w oknie przeglądarki, narzędzie Page Inspector.

Ponownie zaznacz pole wyboru, a następnie kliknij dwukrotnie wartość właściwości i zmień ją na `red`. Zmiana pokazuje natychmiast:

![Kolor tła czerwony](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

**Style** sprawia, że okna ułatwiają przetestuj i Wyświetl podgląd CSS zmian, zanim zatwierdzenia zmian w stylu arkusz sam.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Automatyczna synchronizacja z CSS

> [!NOTE]
> Ta funkcja wymaga w wersji 1.3 narzędzia Page Inspector.


Funkcja automatycznej synchronizacji CSS służy do bezpośredniego edytowania pliku CSS i zobaczyć zmiany bezpośrednio w przeglądarce narzędzia Page Inspector.

Kliknij przycisk **Sprawdź** umieścić narzędzie Page Inspector w trybie inspekcji.

W przeglądarce narzędzia Page Inspector, umieść kursor myszy na sekcję "Home Page" do momentu **div.content otoki** etykieta jest wyświetlana. Kliknij raz, aby wybrać ten element.

**— Style** okno pokazuje wszystkie reguły CSS dla tego elementu. Przewiń w dół do selektora klasy Znajdź .featured .content otoki. Kliknij pozycję "opakowanie .content .featured". Narzędzie Page Inspector otwiera się w pliku CSS, która definiuje ten styl (Site.css) i zaznacza odpowiednie style CSS.

![Plik CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Teraz Zmień wartość `background-color` do "red". Zmiana pojawi się natychmiast w przeglądarce narzędzia Page Inspector.

![Narzędzie Page Inspector przeglądarki](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Za pomocą selektora kolorów CSS

Następnie dowiesz się, jak używać narzędzia Page Inspector umożliwia szybkie znajdowanie i zmienianie CSS dla wyróżnionego tekstu w domyślnej aplikacji. W tym przykładzie decydujesz nie niebieskie wyróżnienie, takich jak i chcesz je zmienić na inny kolor.

Kliknij przycisk **Sprawdź** przycisku.

![Zbadaj Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

W oknie przeglądarki, narzędzie Page Inspector, umieść kursor myszy nad wyróżnionego "filmów wideo, samouczki i przykłady" tekst tak, aby etykieta CSS "Oznacz" pojawia się.

![Przenosząc kursor myszy nad elementem znacznika](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Kliknij tekst, aby go zaznaczyć. Odpowiedni selektor CSS w znaku, który pojawia się w dolnej części **style** okna.

![Selektor znaku w oknie style](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Kliknij selektor znacznika. Spowoduje to otwarcie *Site.css* pliku dla aplikacji sieci web. Kliknij kartę Site.css i odpowiednie CSS selektora jest wyróżniona:

![Znacznik wyboru w arkuszu stylów](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Wybierz i Usuń wiersz z właściwością kolor tła.

Teraz użyjesz próbnika kolorów programu Visual Studio 2012 CSS wybierz nowy kolor dla **oznaczyć** właściwość kolor tła.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Za pomocą programu Visual Studio 2012 CSS próbnika

Edytor CSS w programie Visual Studio 2012 zawiera selektor kolorów, która pozwala łatwo wybrać i Wstaw kolorów. Ma proste pasek koloru i selektor "pop-down", który zapewnia bardziej precyzyjną kontrolę.

Selektor kolorów zawiera standardowe paletę kolorów, obsługuje nazwy kolor standardowy, kody skrótów, kolorów RGB, RGBA, HSL i HSLA i utrzymuje listę kolorów, które zostały ostatnio użyte w dokumencie.

W wierszu, gdzie właściwość kolor tła była wpisz "bc", a jeden raz naciśnij strzałkę w dół.

Po wpisaniu pierwszym znakiem każdego wyrazu we właściwości łącznikami, takich jak "background-color" IntelliSense filtrów na liście pokazywać tylko właściwości, które odpowiadają:

![Funkcja IntelliSense filtrowane wartości](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Teraz wpisz dwukropek. Po wykonaniu, jest wstawiany nazwę właściwości pełny kolor tła. Typ **#** lub **rgb (**, i zostanie wyświetlony pasek selektora kolorów:

![Pasek wyboru koloru CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Aby zobaczyć, jak działa paska selektora kolorów, kliknij jego kolorów, umieść wskaźnik myszy lub naciśnij klawisz strzałki w dół i użyj klawiszy strzałek lewy i prawy przechodzenia kolorów. Gdy użytkownik odwiedza kolor, jest przeglądany odpowiednie wartości dla właściwości kolor tła:

![wartość właściwości kolor tła podglądu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

W tym momencie można naciśnij klawisz Enter, aby wybrać wartość, a następnie średnikami (;), Zakończ wprowadzanie CSS. Teraz przejdź do następnej sekcji, aby zobaczyć, jak działa selektor kolorów w dół pop.

#### <a name="using-the-color-picker-pop-down"></a>Za pomocą selektora kolorów w dół Pop

Gdy pasek koloru nie dokładny kolor, którego szukasz, można użyć selektora kolorów punktów pop w dół.

Aby go otworzyć, kliknij cudzysłów ostrokątny double na prawym końcu paska koloru, lub jeden lub dwa razy klawisz Strzałka w dół na klawiaturze.

![Selektor koloru CSS Pop — szczegółów](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Kliknij przycisk koloru z pionowy pasek po prawej stronie. W oknie głównym ten pokazuje gradientu dla tego koloru. Wybierz kolor, bezpośrednio z kreska pionowa, naciskając klawisz Enter lub kliknij dowolnego punktu w głównym oknie, aby wybrać z większą dokładnością.

Jeśli jest kolorem na ekranie komputera, którego chcesz używać (nie musi być w interfejsie użytkownika programu Visual Studio), jego wartość można przechwycić za pomocą narzędzia służącego do pobierania w prawym dolnym rogu.

Możesz również zmienić przezroczystość koloru za pomocą suwaka u dołu selektora kolorów. Robi zmiany kolor wartości z RGBA, ponieważ RGBA format może reprezentować nieprzezroczystości.

Po wybraniu koloru, naciśnij klawisz Enter, a następnie wpisz średnikiem, aby ukończyć kolor tła wejścia w *Site.css* pliku.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Na pasku aktualizacji Inspektor strony

Narzędzie Page Inspector natychmiast wykrywa zmiany *Site.css* pliku (lub dowolnego pliku w aplikacji) i wyświetla alert w pasku aktualizacji.

![Pasek aktualizacji](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Aby zapisać wszystkie pliki i odświeżyć przeglądarkę, narzędzie Page Inspector, naciśnij klawisze Ctrl + Alt + Enter lub kliknij przycisk na pasku aktualizacji. Zmień kolor wyróżnienia pojawia się w przeglądarce:

![Zmienić kolor wyróżnienia](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Zwróć uwagę, wygodnie odświeżyć przeglądarkę narzędzia Page Inspector bezpośrednio z programu w środowisku Visual Studio. Za pomocą narzędzia Page Inspector zamiast zewnętrznej przeglądarki umożliwia pozostają w edytorze podczas opracowywania aplikacji sieci web.

## <a name="see-also"></a>Zobacz też

[Wprowadzenie do narzędzia Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (wideo Channel 9)

[Komunikaty o błędach narzędzia Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)
