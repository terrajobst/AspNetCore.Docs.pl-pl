---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: "W programie ASP.NET MVC przy użyciu narzędzia Page Inspector | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Narzędzie Page Inspector w programie Visual Studio 2012 to narzędzie do projektowania sieci web za pomocą zintegrowanego przeglądarki. Wybierz dowolny element w zintegrowanej przeglądarki i narzędzie Page Inspector i..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6aa9f16f166ecf5529ae33a17951eb5ea425e7af
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-in-aspnet-mvc"></a>Za pomocą narzędzia Page Inspector na platformie ASP.NET MVC
====================
przez Ammann Timowi

> Narzędzie Page Inspector w programie Visual Studio 2012 to narzędzie do projektowania sieci web za pomocą zintegrowanego przeglądarki. Wybierz dowolny element w zintegrowanej przeglądarki i narzędzie Page Inspector natychmiast wyróżnia elementu źródłowego i CSS. Można wybrać dowolny widok MVC, szybkie znajdowanie źródeł renderowanego kodu znaczników i użyj narzędzia przeglądarki w środowisku Visual Studio.
> 
> [Obejrzyj film](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> W tym samouczku pokazano, jak włączyć tryb inspekcji, a następnie szybko Znajdź i edytowanie znaczników i CSS w ramach projektu sieci web. W samouczku projektu MVC, ale można również użyć narzędzia Page Inspector dla [formularzy sieci Web](https://go.microsoft.com/?linkid=9802001) i innych aplikacji ASP.NET.
> 
> Samouczek zawiera następujące sekcje:
> 
> - [Wymagania wstępne](#_1_prerequisites)
> - [Tworzenie aplikacji sieci Web](#_2_creating_a)
> - [Użyj narzędzia Page Inspector do przejdź do widoku](#_3_using_page)
> - [Włącz tryb inspekcji](#_4_inspection_mode)
> - [Narzędzie Page Inspector umożliwia zmianę znaczników](#_5_using_page)
> - [Tryb inspekcji i okno HTML](#_6_inspection_mode)
> - [Podgląd zmian CSS w oknie style](#_7_previewing_css)
> - [Automatyczna synchronizacja z CSS](#css_auto_sync)
> - [Za pomocą selektora kolorów CSS](#css_color_picker)
> - [Elementy strony dynamiczne mapowania JavaScript](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

- [Program Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us) lub [programu Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).

> [!NOTE]
> Aby uzyskać najnowszą wersję narzędzia Page Inspector, należy użyć [Instalatora platformy sieci Web](https://go.microsoft.com/fwlink/?LinkId=255386) zainstalować zestaw Windows Azure SDK dla platformy .NET 2.0.


Narzędzie Page Inspector jest dołączany do narzędzia Microsoft Web Developer Tools. Najnowsza wersja to 1.3. Aby sprawdzić, która wersja ma, uruchom Visual Studio i wybierz **Microsoft Visual Studio** z **pomocy** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Tworzenie aplikacji sieci Web

Najpierw należy utworzyć aplikacji sieci web, która będzie używana narzędzie Page Inspector z. W programie Visual Studio, wybierz **pliku** &gt; **nowy projekt**. Po lewej stronie rozwiń węzeł **Visual C#**, wybierz pozycję **Web**, a następnie wybierz **aplikacji sieci Web ASP.NET MVC4**.

![Nowa aplikacja platformy ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Kliknij przycisk **OK**.

W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej**. Pozostaw **Razor** jako domyślny aparat widoku.

![Nowy projekt ASP.NET MVC — aplikacji internetowej](using-page-inspector-in-aspnet-mvc/_static/image4.png)

Aplikacja zostanie otwarta w **źródła** widoku.

![Nowa aplikacja platformy ASP.NET MVC w widoku źródła](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Teraz, po aplikacji do pracy z służy narzędzie Page Inspector do zbadania i zmodyfikowania go.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Użyj narzędzia Page Inspector do przejdź do widoku

W programie Visual Studio 2012, należy kliknąć prawym przyciskiem myszy dowolny widok w projekcie, wybierz opcję **widoku w narzędzie Page Inspector**, i narzędzie Page Inspector będzie ustalić trasy i wyświetli tej strony.

W **Eksploratora rozwiązań**, rozwiń węzeł **widoków** folder, a następnie **Home** folderu. Kliknij prawym przyciskiem myszy plik Index.cshtml i wybierz polecenie **widoku w narzędzie Page Inspector**.

![Wyświetl Index.cshtml w narzędzie Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Domyślnie narzędzie Page Inspector jest zadokowany jako okno po lewej stronie środowiska Visual Studio. Jeśli wolisz, możesz dock go w innym miejscu lub Oddokuj okna. Zobacz [porady: Aranżowanie i dokowanie okien](https://msdn.microsoft.com/en-us/library/z4y0hsax.aspx).

Górne okienko okna narzędzia Page Inspector pokazuje bieżącą stronę w przeglądarce. Dolne okienko zawiera strony w kod znaczników HTML, wraz z niektórych kart, które pozwalają sprawdzić różnych aspektów strony. Dolne okienko jest podobny do [F12 Developer Tools](https://msdn.microsoft.com/en-us/ie/aa740478) w programie Internet Explorer.

![Aplikacji ASP.NET MVC w narzędzie Page Inspector](using-page-inspector-in-aspnet-mvc/_static/image10.png)

W tym samouczku użyjesz **HTML** i **style** karty, aby szybko przejść i wprowadzenia zmian w aplikacji.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Tryb EnableInspection

Aby uruchomić narzędzie Page Inspector w trybie inspekcji, kliknij przycisk **inspekcję** przycisku. W trybie inspekcji gdy kursora myszy nad dowolną część renderowanej strony odpowiedni kod źródłowy lub kod zostanie wyróżniona.

![Przełącz tryb inspekcji](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Teraz myszą różnych części strony w narzędzie Page Inspector. Jak, kursor zmienia się na znak plus dużych i jest wyróżniony element poniżej:

![Ustawiając kursor nad div.content otoki](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Podczas przesuwania wskaźnika myszy Visual Studio prezentuje odpowiedniej składni Razor w pliku źródłowym. Jeśli HTML element pochodzą z innego pliku źródłowego, Visual Studio automatycznie otwiera plik.

W narzędzie Page Inspector **HTML** karta zawiera kod HTML, który został wygenerowany z użyciem składni Razor. Podczas przesuwania wskaźnika myszy, są wyróżnione elementów HTML. **Style** karta zawiera reguły CSS dla elementu.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Narzędzie Page Inspector umożliwia zmianę znaczników

Narzędzie Page Inspector umożliwia znaleźć znaczników, którego lokalizacja może nie być oczywista. Następnie można zmodyfikować kod znaczników i wyświetlić wynikowe zmiany.

Aby zobaczyć, kliknij przycisk **inspekcję** , a następnie przewiń w dół strony w oknie narzędzia Page Inspector.

Podczas przesuwania wskaźnika myszy do obszaru stopki zostanie otwarte narzędzie Page Inspector \_plik Layout.cshtml i zaznacza sekcji wybranej strony układu. Jak widać, czy stopka jest zdefiniowany w pliku układu, a nie samego widoku.

![Stopki](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Teraz umieść wskaźnik myszy na linii o prawach autorskich <a id="a"> </a>zauważyć. W \_zostanie wyróżniona Layout.cshtml strony, odpowiednim wierszu.

![Stopka wiersza o prawach autorskich wyróżnione](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Dodaj tekst do końca wiersza w \_plik Layout.cshtml.

&lt;p&gt;&amp;kopiowania; @DateTime.Now.Year — Moja aplikacja platformy ASP.NET MVC zmieni! &lt;/p&gt;

Teraz naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij przycisk paska aktualizacji, aby wyświetlić wyniki w oknie przeglądarki narzędzie Page Inspector.

![Moje skały aplikacji ASP.NET!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Może mieć uważasz stopki zdefiniowane w Index.cshtml, że włączone, w \_Layout.cshtml i narzędzie Page Inspector odnaleziona automatycznie.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Tryb inspekcji i okno HTML

Następnie należy krótki przegląd okno HTML i jak mapuje elementy dla Ciebie.

Kliknij przycisk **inspekcję** chcesz włączyć tryb inspekcji narzędzie Page Inspector.

Kliknij w górnej części strony, stwierdzający "logohere". Są badanie dany element bardziej szczegółowo dzięki wyświetlana w oknie przeglądarki już zmiany podczas przesuwania wskaźnika myszy.

Teraz umieść wskaźnik myszy **HTML** okna. Podczas przesuwania wskaźnika myszy narzędzie Page Inspector przedstawiono w elemencie **HTML** okna i zaznacza odpowiadający mu element w oknie przeglądarki.

![Okno HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Jak wcześniej, zostanie otwarte narzędzie Page Inspector \_plik Layout.cshtml dla Ciebie na karcie tymczasowego. Kliknij przycisk \_Layout.cshtml kartę tymczasowego i odpowiednie znaczników zostanie podświetlony w &lt;nagłówka&gt; sekcji można:

![Wyróżnione znaczników](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Podgląd zmian CSS w oknie style

Następnie użyje narzędzie Page Inspector **style** okno, aby wyświetlić podgląd zmian do arkusza CSS.

Kliknij przycisk **inspekcję** chcesz włączyć tryb inspekcji narzędzie Page Inspector.

W oknie przeglądarki narzędzie Page Inspector, przenieś wskaźnik myszy w sekcji "Strona główna" do **div.content otoki** jest wyświetlana etykieta.

![Ustawiając kursor nad div.content otoki](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Kliknij wewnątrz sekcji otoki div.content, a następnie przesuń wskaźnik myszy do **style** okna. **Syles** oknie wyświetlane są wszystkie reguły CSS dla tego elementu. Przewiń w dół do selektora klasy Znajdź .featured .content otoki. Teraz, wyczyść pole wyboru dla właściwości kolor tła.

![Kolor tła wyczyść](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Zwróć uwagę, jak zmiana Wyświetla podgląd natychmiast w oknie przeglądarki narzędzie Page Inspector.

Zaznacz pole wyboru ponownie, kliknij dwukrotnie wartość właściwości i zmień ją na czerwony. Zmiana przedstawia natychmiast:

![Kolor tła czerwony](using-page-inspector-in-aspnet-mvc/_static/image30.png)

**Style** sprawia, że okno łatwe do testowania i Podgląd CSS zmienia przed dokonaniem zmiany stylu arkusza samej siebie.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Automatyczna synchronizacja z CSS

> [!NOTE]
> Ta funkcja wymaga wersji 1.3 narzędzie Page Inspector.


Funkcja automatycznej synchronizacji CSS służy do bezpośredniego edytowania pliku CSS i zobaczyć zmiany bezpośrednio w przeglądarce narzędzie Page Inspector.

Kliknij przycisk **inspekcję** chcesz włączyć tryb inspekcji narzędzie Page Inspector.

W przeglądarce narzędzie Page Inspector, przenieś wskaźnik myszy w sekcji "Strona główna" do **div.content otoki** jest wyświetlana etykieta. Kliknij raz, aby wybrać tego elementu.

**Syles** oknie wyświetlane są wszystkie reguły CSS dla tego elementu. Przewiń w dół do selektora klasy Znajdź .featured .content otoki. Kliknij pozycję ".featured .content otoki". Narzędzie Page Inspector otwiera plik kodu CSS, która definiuje ten styl (Site.css) i zaznacza odpowiednie stylu CSS.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Teraz Zmień wartość atrybutu `background-color` do "red". W przeglądarce narzędzie Page Inspector zmiana pojawi się natychmiast.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Za pomocą selektora kolorów CSS

Edytor CSS w programie Visual Studio 2012 zawiera próbnika kolorów, który można łatwo wybrać i Wstaw kolorów. Próbnika kolorów zawiera standardowe paletę kolorów, obsługuje standardowe kolor nazwy i skrótu, kolorów RGB, RGBA HSL i HSLA i przechowuje listę kolorów używanych ostatnio w dokumencie.

W poprzedniej sekcji, należy zmienić wartość `background-color` właściwości. Aby wywołać próbnika kolorów, umieść kursor po nazwie właściwości i typ  **#**  lub **rgb (**.

![Na pasku próbnika kolorów CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Kliknij kolor, który będzie zaznacz ją, a następnie naciśnij klawisz strzałki w dół, a następnie użyj klawiszy strzałek w lewo i w prawo do przechodzenia kolorów. Podczas odwiedzania kolor jest przeglądany odpowiedniej wartości szesnastkowych:

![wartość właściwości kolor tła podglądu](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Jeśli pasek koloru nie ma dokładnie kolor, który ma, można użyć selektora kolorów w dół pop. Aby go otworzyć, kliknij przycisk ostrokątny na prawym końcu paska koloru, lub jeden lub dwa razy klawisz strzałki w dół na klawiaturze.

![Selektor koloru CSS Pop dół](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Kliknij przycisk koloru z pionowy pasek po prawej stronie. W oknie głównym wskazuje gradientu dla tego koloru. Wybierz kolor bezpośrednio z pionowy pasek, naciskając klawisz Enter lub kliknij przycisk dowolnego punktu w oknie głównym, aby wybrać o większej dokładności.

Jeśli na ekranie komputera, który ma być używany jest kolor (nie musi on być w interfejsie użytkownika programu Visual Studio), jego wartość można przechwycić przy użyciu narzędzia służącego do pobierania w prawej dolnej.

Możesz również zmienić przezroczystość koloru za pomocą suwaka w dolnej części próbnika kolorów. Grozi to zmiany kolor wartości z RGBA, ponieważ RGBA format może reprezentować nieprzezroczystość.

Po wybraniu opcji koloru, naciśnij klawisz Enter, a następnie wpisz średnik przeprowadzenie wpisu kolor tła w *Site.css* pliku.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Na pasku aktualizacji inspektora strony

Narzędzie Page Inspector natychmiast wykrywa zmiany *Site.css* plików i wyświetla alert w pasku aktualizacji.

![Pasek aktualizacji](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Aby zapisać wszystkie pliki i odświeżyć przeglądarkę narzędzia Page Inspector, naciśnij kombinację klawiszy Ctrl + Alt + Enter lub kliknij przycisk paska aktualizacji. Zmiana koloru wyróżnienia zostanie wyświetlona w przeglądarce.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Elementy strony dynamiczne mapowania JavaScript

W nowoczesnych aplikacji sieci web elementy na stronie są często generowane dynamicznie z użyciem języka JavaScript. Oznacza to, że nie istnieje żadne statycznych kod znaczników (HTML lub Razor) umożliwiająca te elementy na stronie.

W wersji 1.3 narzędzie Page Inspector można teraz mapować elementy, które były dodawane dynamicznie do strony do odpowiedniego kodu JavaScript. Aby zademonstrować tę funkcję, użyjemy [szablonu jednej strony aplikacji JEDNOSTRONICOWEJ](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> Szablon SPA wymaga [ASP.NET i 2012.2 narzędzia sieci Web](https://go.microsoft.com/fwlink/?LinkId=282650) aktualizacji.


W programie Visual Studio, wybierz **pliku** &gt; **nowy projekt**. Po lewej stronie rozwiń węzeł **Visual C#**, wybierz pozycję **Web**, a następnie wybierz **aplikacji sieci Web ASP.NET MVC4**. Kliknij przycisk **OK**.

W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **jednej strony aplikacji**.

W Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **Home** folderu. Kliknij prawym przyciskiem myszy plik Index.cshtml i wybierz polecenie **widoku w narzędzie Page Inspector**.

Po pierwsze jest wyświetlany w przeglądarce narzędzie Page Inspector jest strony logowania. Kliknij przycisk "Utwórz konto", a następnie utwórz nazwę użytkownika i hasło. Po zarejestrowaniu się aplikacja loguje się użytkownik i tworzy listę zadań do wykonania z niektórych przykładowych elementów.

Kliknij przycisk **inspekcję** chcesz włączyć tryb inspekcji narzędzie Page Inspector. W przeglądarce narzędzie Page Inspector kliknij jeden z elementów do wykonania. Zwróć uwagę, że zamiast są zaznaczone na niebiesko, element jest wyróżniona w kolorze pomarańczowym z "JS" obok nazwy elementu. Oznacza to, że element został utworzony dynamicznie za pośrednictwem skryptu.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Ponadto pomarańczowy podkreślenie pojawia się na **stos wywołań** kartę. Oznacza to, że **stos wywołań** okienko zawiera więcej informacji o elemencie.

Polecenie **stos wywołań** kartę. **Stos wywołań** w okienku zostaną wyświetlone stosu wywołań utworzony element wywołania języka JavaScript. Wywołuje do zewnętrznej biblioteki np. jQuery jest zwinięte, dzięki czemu można łatwo Zobacz wywołania do skryptu aplikacji.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Aby wyświetlić pełną stosu, w tym wywołania do zewnętrznej biblioteki, można rozwinąć węzły z etykietą "Biblioteki zewnętrznej":

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Po kliknięciu elementu w stosie wywołań Visual Studio otworzy plik kodu i zaznacza odpowiadający jej skrypt.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Zobacz też

[Wprowadzenie do platformy ASP.NET MVC 4 z programem Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (witryny sieci Web programu ASP.net)

[Wprowadzenie do narzędzia Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (wideo z witryny Channel 9)

[Komunikaty o błędach narzędzia Page Inspector](https://go.microsoft.com/?linkid=9813062) (MSDN)
