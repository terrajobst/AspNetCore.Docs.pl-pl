---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Za pomocą narzędzia Page Inspector we wzorcu ASP.NET MVC | Dokumentacja firmy Microsoft
author: rick-anderson
description: Narzędzie Page Inspector w programie Visual Studio 2012 jest narzędziem do projektowania sieci web za pomocą zintegrowanej przeglądarki. Wybierz dowolny element w zintegrowanej przeglądarki i narzędzie Page Inspector i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 032be78e744bdf2c74337c72afb4efb7471ae4f9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399146"
---
<a name="using-page-inspector-in-aspnet-mvc"></a>Za pomocą narzędzia Page Inspector we wzorcu ASP.NET MVC
====================
przez Tim Ammann

> Narzędzie Page Inspector w programie Visual Studio 2012 jest narzędziem do projektowania sieci web za pomocą zintegrowanej przeglądarki. Wybranie dowolnego elementu w zintegrowanej przeglądarki i narzędzie Page Inspector natychmiast wyróżnia elementu źródłowego i arkusze CSS. Można przeglądać dowolny widok MVC, szybkie znalezienie źródła renderowanego kodu znaczników i za pomocą narzędzi przeglądarki bezpośrednio w środowisku Visual Studio.
> 
> [Obejrzyj wideo](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> W tym samouczku pokazano, jak włączyć tryb inspekcji, a następnie szybko Znajdź i Edytuj znaczniki i CSS w projekcie sieci web. W tym samouczku użyto projektu MVC, ale można również użyć narzędzia Page Inspector dla [formularzy sieci Web](https://go.microsoft.com/?linkid=9802001) i inne aplikacje platformy ASP.NET.
> 
> Samouczek zawiera następujące sekcje:
> 
> - [Wymagania wstępne](#_1_prerequisites)
> - [Tworzenie aplikacji sieci Web](#_2_creating_a)
> - [Inspektor stron do przejdź do widoku](#_3_using_page)
> - [Włącz tryb inspekcji](#_4_inspection_mode)
> - [Inspektor stron umożliwia wprowadzać zmiany znaczników](#_5_using_page)
> - [Tryb inspekcji i w oknie kodu HTML](#_6_inspection_mode)
> - [Zmiany arkusza CSS (wersja zapoznawcza) w oknie style](#_7_previewing_css)
> - [Automatyczna synchronizacja z CSS](#css_auto_sync)
> - [Za pomocą selektora kolorów CSS](#css_color_picker)
> - [Mapowanie elementów strony dynamicznej języka JavaScript](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

- [Program Visual Studio 2012](https://www.microsoft.com/visualstudio/11) lub [programu Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Aby uzyskać najnowszą wersję narzędzia Page Inspector, użyj [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=255386) zainstalować zestaw Windows Azure SDK dla platformy .NET w wersji 2.0.


Narzędzie Page Inspector jest umieszczany w pakietach za pomocą narzędzia Microsoft Web Developer Tools. Najnowsza wersja to 1.3. Aby sprawdzić, która wersja mają, uruchom program Visual Studio i wybierz **Microsoft Visual Studio** z **pomocy** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Tworzenie aplikacji sieci Web

Najpierw należy utworzyć aplikacji sieci web, którego będziesz używać narzędzia Page Inspector za pomocą. W programie Visual Studio, wybierz **pliku** &gt; **nowy projekt**. Po lewej stronie, rozwiń węzeł **Visual C#**, wybierz opcję **Web**, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC4**.

![Nowa aplikacja platformy ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Kliknij przycisk **OK**.

W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej**. Pozostaw **Razor** jako domyślny aparat widoku.

![Nowy projekt ASP.NET MVC — aplikacji internetowej](using-page-inspector-in-aspnet-mvc/_static/image4.png)

Aplikacja zostanie otwarta w **źródła** widoku.

![Nowa aplikacja platformy ASP.NET MVC w widoku źródła](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Teraz, gdy masz już aplikację, chcesz pracować, można użyć narzędzia Page Inspector do zbadania i zmodyfikowania go.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Inspektor stron do przejdź do widoku

W programie Visual Studio 2012, możesz kliknąć prawym przyciskiem myszy dowolny widok w projekcie, wybierz opcję **widoku w narzędzia Page Inspector**, i narzędzie Page Inspector będzie ustalenie trasy i wyświetlić stronę.

W **Eksploratora rozwiązań**, rozwiń węzeł **widoków** folder i następnie **Home** folderu. Plik Index.cshtml kliknij prawym przyciskiem myszy i wybierz polecenie **widoku w narzędzia Page Inspector**.

![Wyświetl Index.cshtml w narzędzia Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Domyślnie narzędzie Page Inspector jest zadokowany jako okno po lewej stronie w środowisku Visual Studio. Jeśli wolisz, możesz go zadokować w innym miejscu lub oddokować okno. Zobacz [porady: Aranżowanie i dokowanie Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).

Górne okienko okna narzędzia Page Inspector pokazuje bieżącej strony w oknie przeglądarki. Dolne okienko wyświetla stronę w kod znaczników HTML, wraz z niektórych kart, które pozwalają przeprowadzać inspekcje różne aspekty strony. Dolne okienko jest podobny do [narzędzi deweloperskich F12](https://msdn.microsoft.com/ie/aa740478) w programie Internet Explorer.

![Aplikacja platformy ASP.NET MVC w narzędzia Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image10.png)

W tym samouczku użyjesz **HTML** i **style** karty, aby szybko i wprowadzać zmiany w aplikacji.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Tryb EnableInspection

Aby ustawić narzędzia Page Inspector trybu inspekcji, kliknij przycisk **Sprawdź** przycisku. W trybie inspekcji Jeśli przytrzymasz wskaźnik myszy nad dowolną część renderowanej strony odpowiedni kod źródłowy lub kod zostanie wyróżniona.

![Przełącz tryb inspekcji](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Teraz umieść wskaźnik myszy nad różnych części strony, w ramach narzędzia Page Inspector. To zrobisz, kursor zmienia się na znak plus w dużych, a jest wyróżniony element poniżej:

![Kursor div.content otoki](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Podczas przesuwania wskaźnika myszy, Visual Studio wyróżnia odpowiedniej składni Razor w pliku źródłowym. Jeśli HTML element pochodzi z innego pliku źródłowego, Visual Studio automatycznie otwiera plik.

W narzędzia Page Inspector **HTML** karta przedstawia HTML, który został wygenerowany z użyciem składni Razor. Podczas przesuwania wskaźnika myszy, są wyróżnione elementów HTML. **Style** karta zawiera reguły CSS dla elementu.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Inspektor stron umożliwia wprowadzać zmiany znaczników

Narzędzie Page Inspector umożliwia znajdowanie znaczników, którego lokalizacja może nie być oczywista. Następnie można zmodyfikować znaczników i wyświetlić wynikowe zmiany.

Aby to zobaczyć, kliknij przycisk **Sprawdź** a następnie przewiń do dołu strony, w oknie narzędzia Page Inspector.

Gdy przesuniesz wskaźnik myszy w obszarze stopki, zostanie otwarte narzędzie Page Inspector \_plik Layout.cshtml i wyróżnienie części strony układu, który wybrano. Jak widać, czy stopka jest zdefiniowana w pliku układu, a nie samego widoku.

![Stopka](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Teraz umieść wskaźnik myszy nad wierszem o prawach autorskich <a id="a"> </a>Zwróć uwagę. W \_Layout.cshtml stronie odpowiednim wierszu zostanie wyróżniony.

![Stopka wiersz o prawach autorskich wyróżniony](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Dodaj jakiś tekst do końca wiersza w \_plik Layout.cshtml.

&lt;p&gt;&amp;kopiowania; @DateTime.Now.Year — Moja aplikacja platformy ASP.NET MVC zmieni! &lt;/p&gt;

Teraz naciśnij klawisze Ctrl + Alt + Enter lub kliknij pasek aktualizacji, aby wyświetlić wyniki w oknie przeglądarki, narzędzie Page Inspector.

![Moje ASP.NET aplikacji Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Może mieć uważasz, czy stopka zdefiniowany w Index.cshtml, ale okazała się on znajdować się w \_Layout.cshtml i narzędzie Page Inspector odnaleziona automatycznie.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Tryb inspekcji i w oknie kodu HTML

Następnie będziesz mieć rzut oka na oknie HTML oraz sposób mapowania elementów dla Ciebie.

Kliknij przycisk **Sprawdź** umieścić narzędzie Page Inspector w trybie inspekcji.

Kliknij w górnej części strony, który jest wyświetlany komunikat "logohere". To badanie konkretnego elementu bardziej szczegółowo, aby wyświetlana w oknie przeglądarki nie jest już zmienia się po przesunięciu wskaźnika myszy.

Teraz umieść kursor myszy na **HTML** okna. Podczas przesuwania wskaźnika myszy, narzędzie Page Inspector przedstawia element w obrębie **HTML** okna i odpowiadający mu element w oknie przeglądarki.

![Okno w formacie HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Jak wcześniej, zostanie otwarte narzędzie Page Inspector \_plik Layout.cshtml dla Ciebie w karcie tymczasowej. Kliknij przycisk \_Layout.cshtml tymczasowe kartę, a odpowiedni kod znaczników zostaną wyróżnione w &lt;nagłówka&gt; sekcji możesz:

![Wyróżnione znaczników](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Zmiany arkusza CSS (wersja zapoznawcza) w oknie style

Następnie, użyjesz narzędzia Page Inspector **style** okna, aby wyświetlić podgląd zmian do arkusza CSS.

Kliknij przycisk **Sprawdź** umieścić narzędzie Page Inspector w trybie inspekcji.

W oknie przeglądarki, narzędzie Page Inspector, umieść kursor myszy na sekcję "Home Page" do momentu **div.content otoki** etykieta jest wyświetlana.

![Kursor div.content otoki](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Kliknij wewnątrz sekcji otoki div.content jeden raz, a następnie przesuń wskaźnik myszy na **style** okna. **— Style** okno pokazuje wszystkie reguły CSS dla tego elementu. Przewiń w dół do selektora klasy Znajdź .featured .content otoki. Teraz usuń zaznaczenie pola wyboru dla właściwości kolor tła.

![Kolor tła wyczyść](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Zwróć uwagę, jak zmiany zawiera wersje zapoznawcze natychmiast w oknie przeglądarki, narzędzie Page Inspector.

Ponownie zaznacz pole wyboru, a następnie kliknij dwukrotnie wartość właściwości i zmienić ją na czerwono. Zmiana pokazuje natychmiast:

![Kolor tła czerwony](using-page-inspector-in-aspnet-mvc/_static/image30.png)

**Style** sprawia, że okna ułatwiają przetestuj i Wyświetl podgląd CSS zmian, zanim zatwierdzenia zmian w stylu arkusz sam.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Automatyczna synchronizacja z CSS

> [!NOTE]
> Ta funkcja wymaga w wersji 1.3 narzędzia Page Inspector.


Funkcja automatycznej synchronizacji CSS służy do bezpośredniego edytowania pliku CSS i zobaczyć zmiany bezpośrednio w przeglądarce narzędzia Page Inspector.

Kliknij przycisk **Sprawdź** umieścić narzędzie Page Inspector w trybie inspekcji.

W przeglądarce narzędzia Page Inspector, umieść kursor myszy na sekcję "Home Page" do momentu **div.content otoki** etykieta jest wyświetlana. Kliknij raz, aby wybrać ten element.

**— Style** okno pokazuje wszystkie reguły CSS dla tego elementu. Przewiń w dół do selektora klasy Znajdź .featured .content otoki. Kliknij pozycję "opakowanie .content .featured". Narzędzie Page Inspector otwiera się w pliku CSS, która definiuje ten styl (Site.css) i zaznacza odpowiednie style CSS.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Teraz Zmień wartość `background-color` do "red". Zmiana pojawi się natychmiast w przeglądarce narzędzia Page Inspector.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Za pomocą selektora kolorów CSS

Edytor CSS w programie Visual Studio 2012 zawiera selektor kolorów, która pozwala łatwo wybrać i Wstaw kolorów. Selektor kolorów zawiera standardowe paletę kolorów, obsługuje nazwy kolor standardowy, kody skrótów, kolorów RGB, RGBA, HSL i HSLA i utrzymuje listę kolorów, które zostały ostatnio użyte w dokumencie.

W poprzedniej sekcji, możesz zmienić wartość `background-color` właściwości. Aby wywołać selektor kolorów, umieść kursor po danej nazwy właściwości oraz typ **#** lub **rgb (**.

![Pasek wyboru koloru CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Kliknij kolor, aby go zaznaczyć, lub naciśnij klawisz strzałki w dół, a następnie użyj klawiszy strzałek w lewo i w prawo do przechodzenia kolorów. Gdy użytkownik odwiedza kolor, jest przeglądany odpowiadająca wartość szesnastkowa:

![wartość właściwości kolor tła podglądu](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Jeśli pasek koloru nie ma dokładnie odpowiedni kolor, można użyć selektora kolorów w dół pop. Aby go otworzyć, kliknij cudzysłów ostrokątny double na prawym końcu paska koloru, lub jeden lub dwa razy klawisz Strzałka w dół na klawiaturze.

![Selektor koloru CSS Pop — szczegółów](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Kliknij przycisk koloru z pionowy pasek po prawej stronie. W oknie głównym ten pokazuje gradientu dla tego koloru. Wybierz kolor, bezpośrednio z kreska pionowa, naciskając klawisz Enter lub kliknij dowolnego punktu w głównym oknie, aby wybrać z większą dokładnością.

Jeśli jest kolorem na ekranie komputera, którego chcesz używać (nie musi być w interfejsie użytkownika programu Visual Studio), jego wartość można przechwycić za pomocą narzędzia służącego do pobierania w prawym dolnym rogu.

Możesz również zmienić przezroczystość koloru za pomocą suwaka u dołu selektora kolorów. Robi zmiany kolor wartości do wartości RGBA, ponieważ RGBA format może reprezentować nieprzezroczystości.

Po wybraniu koloru, naciśnij klawisz Enter, a następnie wpisz średnikiem, aby ukończyć kolor tła wejścia w *Site.css* pliku.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Na pasku aktualizacji Inspektor strony

Narzędzie Page Inspector natychmiast wykrywa zmiany *Site.css* pliku i wyświetla alert w pasku aktualizacji.

![Pasek aktualizacji](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Aby zapisać wszystkie pliki i odświeżyć przeglądarkę, narzędzie Page Inspector, naciśnij klawisze Ctrl + Alt + Enter lub kliknij przycisk na pasku aktualizacji. Zmień kolor wyróżnienia pojawia się w przeglądarce.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Mapowanie elementów strony dynamicznej języka JavaScript

W nowoczesnych aplikacji sieci web elementy na stronie są często generowane dynamicznie przy użyciu języka JavaScript. Oznacza to, że nie ma żadnych znaczników statyczne (HTML lub Razor), która odnosi się do tych elementów strony.

W wersji 1.3 narzędzie Page Inspector można teraz Mapowanie elementów, które zostały dynamicznie dodawane do strony do odpowiedniego kodu JavaScript. Aby zademonstrować tę funkcję, użyjemy [szablonu jednej strony aplikacji (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> Szablon SPA wymaga [platformy ASP.NET i Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) aktualizacji.


W programie Visual Studio, wybierz **pliku** &gt; **nowy projekt**. Po lewej stronie, rozwiń węzeł **Visual C#**, wybierz opcję **Web**, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC4**. Kliknij przycisk **OK**.

W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji jednostronicowej**.

W Eksploratorze rozwiązań rozwiń **widoków** folder i następnie **Home** folderu. Plik Index.cshtml kliknij prawym przyciskiem myszy i wybierz polecenie **widoku w narzędzia Page Inspector**.

Po pierwsze, czyli wyświetlonej w przeglądarce narzędzia Page Inspector jest strona logowania. Kliknij przycisk "Zarejestruj", a następnie utwórz nazwę użytkownika i hasło. Po zarejestrowaniu się aplikacji loguje się użytkownik i tworzy listę zadań do wykonania z niektórymi elementami próbki.

Kliknij przycisk **Sprawdź** umieścić narzędzie Page Inspector w trybie inspekcji. W przeglądarce narzędzia Page Inspector kliknij jeden z elementów do wykonania. Należy zauważyć, że zamiast są wyróżnione na niebiesko, elementu jest wyróżniony kolorem pomarańczowym za pomocą "JS" obok nazwy elementu. Oznacza to, że element został utworzony dynamicznie za pośrednictwem skryptu.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Ponadto pomarańczowy podkreślenie pojawia się na **stos wywołań** kartę. Oznacza to, że **stos wywołań** okienko zawiera więcej informacji na temat elementu.

Kliknij pozycję **stos wywołań** kartę. **Stos wywołań** okienko zawiera stos wywołań dla wywołania języka JavaScript, utworzony element. Wywołuje w zewnętrznych bibliotekach np. jQuery są zwinięte, dzięki czemu można łatwo zobaczyć, wywołuje skrypt aplikacji.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Aby wyświetlić pełny stos, w tym wywołania zewnętrznych bibliotek można rozwinąć węzły z etykietą "Biblioteki zewnętrznej":

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Kliknięcie elementu w stosie wywołań programu Visual Studio otwiera plik kodu i wyróżnienie odpowiadający jej skrypt.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Zobacz też

[Wprowadzenie do platformy ASP.NET MVC 4 w programie Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (witryny sieci Web platformy ASP.net)

[Wprowadzenie do narzędzia Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (wideo Channel 9)

[Komunikaty o błędach narzędzia Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)
