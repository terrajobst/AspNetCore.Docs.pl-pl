---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Za pomocą narzędzia Page Inspector dla formularzy programu Visual Studio 2012 w sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Narzędzie Page Inspector dla programu Visual Studio 2012 to narzędzie do projektowania sieci web za pomocą zintegrowanego przeglądarki. Wybierz dowolny element w zintegrowanej przeglądarki i narzędzie Page Inspector...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Za pomocą narzędzia Page Inspector dla programu Visual Studio 2012 w formularzach sieci Web ASP.NET
====================
przez Ammann Timowi

> Narzędzie Page Inspector dla programu Visual Studio 2012 to narzędzie do projektowania sieci web za pomocą zintegrowanego przeglądarki. Wybierz dowolny element w zintegrowanej przeglądarki i narzędzie Page Inspector natychmiast wyróżnia elementu źródłowego i CSS. Można wybrać dowolną stronę w aplikacji, szybkie znajdowanie źródeł renderowanego kodu znaczników i użyj narzędzia przeglądarki w środowisku Visual Studio.
> 
> Ten samouczek shwos jak włączyć tryb inspekcji, a następnie szybko zlokalizować i edytować reguły CSS i tekst w ramach projektu sieci web. W samouczku projekt aplikacji formularzy sieci Web, ale można również użyć narzędzie Page Inspector dla projektów witryny sieci Web i [MVC](https://go.microsoft.com/?linkid=9802002) aplikacji.
> 
> Samouczek zawiera następujące sekcje:
> 
> [Wymagania wstępne](#_1_prerequisites)
> 
> [Tworzenie aplikacji sieci Web](#_2_creating_a)
> 
> [Narzędzie Page Inspector umożliwia wyświetlanie aplikacji](#_3_using_page)
> 
> [Włącz tryb inspekcji](#_4_inspection_mode)
> 
> [Narzędzie Page Inspector umożliwia zmianę znaczników](#_5_using_page)
> 
> [Tryb inspekcji i okno HTML](#_6_inspection_mode)
> 
> [Podgląd zmian CSS w oknie style](#_7_previewing_css)
> 
> [Automatyczna synchronizacja z CSS](#css_auto_sync)
> 
> [Za pomocą selektora kolorów CSS](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

- [Program Visual Studio 2012](https://www.microsoft.com/visualstudio/11) lub [programu Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Aby uzyskać najnowszą wersję narzędzia Page Inspector, należy użyć [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=255386) zainstalować zestaw Azure SDK dla .NET 2.0.


Narzędzie Page Inspector jest dołączany do narzędzia Microsoft Web Developer Tools. Najnowsza wersja to 1.3. Aby sprawdzić, która wersja ma, uruchom Visual Studio i wybierz **Microsoft Visual Studio** z **pomocy** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Tworzenie aplikacji sieci Web

Najpierw należy utworzyć aplikacji sieci web, która będzie używana narzędzie Page Inspector z. W programie Visual Studio, wybierz **pliku** &gt; **nowy projekt**. Po lewej stronie rozwiń węzeł **Visual C#**, wybierz pozycję **Web**, a następnie wybierz **aplikacji formularzy sieci Web ASP.NET**.

![Nowa aplikacja formularzy sieci Web](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Kliknij przycisk **OK**.

Aplikacja zostanie otwarta w **źródła** widoku.

![Nowej aplikacji formularzy sieci Web w widoku źródła](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Teraz, po aplikacji do pracy z służy narzędzie Page Inspector do zbadania i zmodyfikowania go.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Narzędzie Page Inspector umożliwia wyświetlanie aplikacji

Następnie zostaną wyświetlone aplikacji z narzędziem Page Inspector. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **widoku w narzędzie Page Inspector**.

![Widok w narzędzie Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Domyślnie gdy narzędzie Page Inspector uruchamia się po raz pierwszy jest zadokowany wąskie okna po lewej stronie środowiska Visual Studio. Pozostaw zadokowane po lewej stronie i Ustaw szerokość jest wygodne dla Ciebie lub dock w jednym z obszarów narzędzia na górze, dołu lub prawej:

![Narzędzie Page Inspector pozycjach](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Jeśli Oddokuj okna narzędzia Page Inspector, możesz go umieścić poza programem Visual Studio lub nawet na drugim monitorze Jeśli masz. Jednak aby klawisze ALT + TAB między narzędzie Page Inspector i Visual Studio podczas okna narzędzia Page Inspector jest zadokowany, przejdź do **narzędzia** &gt; **opcje** &gt;  **Środowiska** &gt; **zakładek i okien**i w obszarze **kartę również**, wyczyść pole wyboru o nazwie **zawsze nad przestawne okna narzędzi Okno główne**:

![Wyczyść pole wyboru przestawne windows narzędzia do ALT + TAB między Visual Studio i niezadokowane okna narzędzia Page Inspector](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Górne okienko okna narzędzia Page Inspector pokazuje bieżącą stronę w przeglądarce. Dolne okienko zawiera strony w kod znaczników HTML po lewej stronie, a niektóre karty po prawej stronie, które pozwalają sprawdzić różnych aspektów strony. Dolne okienko jest podobny do [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) w programie Internet Explorer. (Jednak w przeciwieństwie do narzędzia developer służy narzędzie Page Inspector bezpośrednio w programie Visual Studio.)

![Inspektor strony](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

W tym samouczku użyjesz okienku przeglądarkę narzędzia Page Inspector i **HTML** i **style** karty, aby szybko ułatwiają nawigowanie i wprowadzenia zmian w aplikacji.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Włącz tryb inspekcji

Następnie pojawi się, jak działa tryb inspekcji Page Inspector. Kliknij w oknie narzędzia Page Inspector **inspekcję** przycisku.

![Sprawdź Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Aby zobaczyć, tryb inspekcji w akcji, myszą różnych części strony w oknie przeglądarki narzędzie Page Inspector. Jak, kursor zmienia się na znak plus dużych i jest wyróżniony element poniżej:

![Ustawiając kursor nad div.content otoki](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Pamiętaj, że podczas przesuwania wskaźnika myszy

- Zawartość **źródła** zmian do wyświetlenia znaczników odpowiadający wybranego elementu na stronie widoku. Zostanie wyróżniona odpowiednich znaczników. Jeśli źródło jest w innym pliku, czy plik jest otwarty w widoku źródła z odpowiednich wyróżniony kod znaczników.

- Kod znaczników wyświetlanych w **HTML** kartę w narzędzie Page Inspector zmienia także odpowiadają wybranego elementu na stronie. W **HTML** karcie przedstawiono istotne znaczników.

- **Style** karta zawiera reguły CSS odpowiednie do bieżącego zaznaczenia.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Narzędzie Page Inspector umożliwia zmianę znaczników

Teraz będą widzieć, jak można użyć narzędzia Page Inspector można znaleźć i wprowadzać zmiany znaczników lub tekst, którego lokalizacja może nie być od razu widoczne.

Umieść narzędzie Page Inspector w trybie inspekcji, a następnie przewiń w dół strony głównej.

Jak wprowadzasz obszaru stopki zostanie otwarte narzędzie Page Inspector *Site.Master* pliku układu **źródła** widok na karcie tymczasowego na prawo od drugiej karty i zaznacza sekcji wzorca strony, które zostały wybrane. To pokazuje sposób narzędzie Page Inspector można znaleźć i wyświetlić zawartość na stronie, która może faktycznie pochodzą z innego pliku niż ten, który je otworzył.

![Najważniejsze funkcje stopki w trybie inspekcji](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

W oknie przeglądarki narzędzie Page Inspector Przesuń wskaźnik myszy nad wiersza o prawach autorskich <a id="a"> </a>zauważyć.

W *Site.Master* zostanie wyróżniona strony, odpowiednim wierszu.

![Stopka wiersza o prawach autorskich wyróżnione](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Dodaj tekst do końca wiersza w *Site.Master* pliku.

&lt;p&gt;&amp;kopiowania; &lt;%: DateTime.Now.Year %&gt; -Moje skały aplikacji ASP.NET!&lt; /p&gt;

Teraz naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij przycisk paska aktualizacji, aby wyświetlić wyniki w oknie przeglądarki narzędzie Page Inspector.

![Moje skały aplikacji ASP.NET!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Może mieć uważasz, że stopki został na *Default.aspx* strony, ale wyłączone w strony wzorcowej układu i narzędzie Page Inspector uznała, że dla Ciebie.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Tryb inspekcji i okno HTML

Następnie należy krótki przegląd okno HTML i jak mapuje elementy dla Ciebie.

Narzędzie Page Inspector należy umieścić w trybie inspekcji.

![Sprawdź Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Kliknij w górnej części strony, stwierdzający "tutaj znak logo". Są badanie dany element bardziej szczegółowo dzięki wyświetlana w oknie przeglądarki już zmiany podczas przesuwania wskaźnika myszy.

Teraz umieść wskaźnik myszy **HTML** okna. Podczas przesuwania wskaźnika myszy narzędzie Page Inspector przedstawiono w elemencie **HTML** okna i zaznacza odpowiadający mu element w oknie przeglądarki.

![Okno HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Jak wcześniej, zostanie otwarte narzędzie Page Inspector *Site.Master* pliku dla Ciebie na karcie tymczasowego. Kliknij kartę Site.Master i odpowiedni kod znaczników jest wyróżniona w &lt;nagłówka&gt; sekcji:

![Wyróżnione znaczników](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Podgląd zmian CSS w oknie style

Następnie zostanie wyświetlony, jak używasz narzędzia Page Inspector **style** okno, aby wyświetlić podgląd zmian do arkusza CSS.

Kliknij przycisk **inspekcję** przycisk, aby włączyć tryb inspekcji narzędzie Page Inspector.

W oknie przeglądarki narzędzie Page Inspector, przenieś wskaźnik myszy w sekcji "Strona główna" do **div.content otoki** jest wyświetlana etykieta.

![Ustawiając kursor nad elementów](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Kliknij wewnątrz sekcji otoki div.content, a następnie przesuń wskaźnik myszy do **style** okna. W obszarze selektor klasy otoki .content .featured wyczyść i zaznacz pole wyboru dla właściwości kolor tła.

![Kolor tła wyczyść](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Zwróć uwagę, jak zmiana Wyświetla podgląd natychmiast w oknie przeglądarki narzędzie Page Inspector.

Zaznacz pole wyboru ponownie, kliknij dwukrotnie wartość właściwości i zmień go na `red`. Zmiana przedstawia natychmiast:

![Kolor tła czerwony](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

**Style** sprawia, że okno łatwe do testowania i Podgląd CSS zmienia przed dokonaniem zmiany stylu arkusza samej siebie.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>CSS Auto Sync

> [!NOTE]
> Ta funkcja wymaga wersji 1.3 narzędzie Page Inspector.


Funkcja automatycznej synchronizacji CSS służy do bezpośredniego edytowania pliku CSS i zobaczyć zmiany bezpośrednio w przeglądarce narzędzie Page Inspector.

Kliknij przycisk **inspekcję** chcesz włączyć tryb inspekcji narzędzie Page Inspector.

W przeglądarce narzędzie Page Inspector, przenieś wskaźnik myszy w sekcji "Strona główna" do **div.content otoki** jest wyświetlana etykieta. Kliknij raz, aby wybrać tego elementu.

**Syles** oknie wyświetlane są wszystkie reguły CSS dla tego elementu. Przewiń w dół do selektora klasy Znajdź .featured .content otoki. Kliknij pozycję ".featured .content otoki". Narzędzie Page Inspector otwiera plik kodu CSS, która definiuje ten styl (Site.css) i zaznacza odpowiednie stylu CSS.

![Pliku CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Teraz Zmień wartość atrybutu `background-color` do "red". W przeglądarce narzędzie Page Inspector zmiana pojawi się natychmiast.

![Narzędzie Page Inspector przeglądarki](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Za pomocą selektora kolorów CSS

Następnie dowiesz się, jak używać narzędzia Page Inspector można szybko znaleźć i zmienić CSS dla wyróżnionego tekstu w domyślnej aplikacji. W tym przykładzie decydujesz się nie niebieskie wyróżnienie, takich jak i chcesz je zmienić kolor na inny.

Kliknij przycisk **inspekcję** przycisku.

![Sprawdź Element](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

W oknie przeglądarki narzędzie Page Inspector Przesuń wskaźnik myszy nad wyróżnionego "wideo, samouczki i przykłady" tak, aby CSS "znak" etykiety tekstu.

![Kursor myszy nad elementem znaku](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Kliknij tekst, aby go wybrać. Zostanie wyświetlony odpowiedni selektor CSS w znaku w dolnej części **style** okna.

![Selektor znacznika w oknie style](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Kliknij znacznik wyboru. Spowoduje to otwarcie *Site.css* plików dla aplikacji sieci web. Kliknij kartę Site.css i odpowiednie CSS selektora zostanie wyróżniona:

![Selektor znacznika w arkuszu stylów](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Wybierz i Usuń wiersz z właściwością kolor tła.

Teraz użyje próbnika kolorów programu Visual Studio 2012 CSS, aby wybrać nowy kolor **oznaczyć** właściwości kolor tła.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Za pomocą programu Visual Studio 2012 CSS próbnika

Edytor CSS w programie Visual Studio 2012 zawiera próbnika kolorów, który można łatwo wybrać i Wstaw kolorów. Ma prosty pasek koloru i selektor "pop rozwijanej", który zapewnia bardziej precyzyjną kontrolę.

Próbnika kolorów zawiera standardowe paletę kolorów, obsługuje standardowe kolor nazwy i skrótu, kolorów RGB, RGBA HSL i HSLA i przechowuje listę kolorów używanych ostatnio w dokumencie.

Gdy właściwość kolor tła była wierszu "bc" i naciśnij klawisz strzałki w dół raz.

Po wpisaniu pierwszych znaków każdego wyrazu we właściwości rozdzielone łącznikiem, takich jak "background-color" IntelliSense filtry na liście pokazywać tylko właściwości, które odpowiadają:

![IntelliSense filtrowane wartości](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Teraz wpisz dwukropkiem. Po wykonaniu czynności są wstawiane nazwę właściwości pełny kolor tła. Typ **#** lub **rgb (**, i zostanie wyświetlony pasek selektora kolorów:

![Na pasku próbnika kolorów CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Aby zobaczyć, jak działa na pasku próbnika kolorów, kliknij jego kolorów przy użyciu wskaźnika myszy lub naciśnij klawisz strzałki w dół, a następnie użyj klawiszy strzałek w lewo i prawo przechodzenia kolorów. Podczas odwiedzania kolor odpowiednie wartości dla właściwości kolor tła jest przeglądany:

![wartość właściwości kolor tła podglądu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

W tym momencie można nacisnąć klawisz Enter Wybierz wartość, a następnie średnikiem (;) do ukończenia wpis CSS. Teraz przejdź do następnej sekcji, aby mogli przeglądać, jak działa próbnika kolorów w dół pop.

#### <a name="using-the-color-picker-pop-down"></a>Za pomocą selektora kolorów w dół Pop

Przy pasku kolorów nie ma dokładnie kolor, którego szukasz, można użyć pop rozwijanej próbnika kolorów.

Aby go otworzyć, kliknij przycisk ostrokątny na prawym końcu paska koloru, lub jeden lub dwa razy klawisz strzałki w dół na klawiaturze.

![Selektor koloru CSS Pop dół](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Kliknij przycisk koloru z pionowy pasek po prawej stronie. W oknie głównym wskazuje gradientu dla tego koloru. Wybierz kolor bezpośrednio z pionowy pasek, naciskając klawisz Enter lub kliknij przycisk dowolnego punktu w oknie głównym, aby wybrać o większej dokładności.

Jeśli na ekranie komputera, który ma być używany jest kolor (nie musi on być w interfejsie użytkownika programu Visual Studio), jego wartość można przechwycić przy użyciu narzędzia służącego do pobierania w prawej dolnej.

Możesz również zmienić przezroczystość koloru za pomocą suwaka w dolnej części próbnika kolorów. Grozi to zmiany wartości z RGBA kolorów, ponieważ RGBA format może reprezentować nieprzezroczystość.

Po wybraniu opcji koloru, naciśnij klawisz Enter, a następnie wpisz średnik przeprowadzenie wpisu kolor tła w *Site.css* pliku.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Na pasku aktualizacji inspektora strony

Narzędzie Page Inspector natychmiast wykrywa zmiany *Site.css* pliku (lub dowolny plik w aplikacji) i wyświetla alert w pasku aktualizacji.

![Pasek aktualizacji](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Aby zapisać wszystkie pliki i odświeżyć przeglądarkę narzędzia Page Inspector, naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij przycisk paska aktualizacji. Zmiana koloru wyróżnienia zostanie wyświetlona w przeglądarce:

![Zmienić kolor zaznaczenia](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Zwróć uwagę, wygodnie odświeżyć przeglądarkę narzędzia Page Inspector bezpośrednio w w środowisku Visual Studio. Za pomocą narzędzia Page Inspector zamiast zewnętrznej przeglądarki umożliwia pozostanie w edytorze podczas opracowywania aplikacji sieci web.

## <a name="see-also"></a>Zobacz też

[Wprowadzenie do narzędzia Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (wideo z witryny Channel 9)

[Komunikaty o błędach narzędzia Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)
