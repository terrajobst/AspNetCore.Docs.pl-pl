---
title: Tworzenie pięknych, interaktywnych witryn za pomocą narzędzia Bootstrap i ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać narzędzia Bootstrap umożliwiający projektowanie responsywne aplikacje internetowe z platformą ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: client-side/bootstrap
ms.openlocfilehash: 1ccf33b299739f5aa963a53feb70b44b290443ca
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753640"
---
# <a name="build-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>Tworzenie pięknych, interaktywnych witryn za pomocą narzędzia Bootstrap i ASP.NET Core

<a name="bootstrap-index"></a>

Przez [Steve Smith](https://ardalis.com/)

Usługa ładowania początkowego jest obecnie najpopularniejsze struktura sieci web do tworzenia aplikacji sieci web odpowiada. Czy jesteś Początkujący frontonu projektowania i programowania lub ekspertem, oferuje wiele funkcji i korzyści, które może poprawić komfort użytkowania witryny sieci web. Usługa ładowania początkowego jest wdrażany jako zestaw plików CSS i JavaScript i został zaprojektowany w celu skalowania witryny sieci Web lub aplikacji wydajnie z telefonów tabletach do komputerów stacjonarnych.

## <a name="get-started"></a>Wprowadzenie

Istnieje kilka sposobów na rozpoczęcie pracy za pomocą narzędzi Bootstrap. Jeśli zaczynasz nowej aplikacji sieci web w programie Visual Studio, można wybrać domyślny początkowy szablon dla platformy ASP.NET Core, w którym wielkość Bootstrap są zainstalowane:

![Uruchomienie w widoku rozwiązania szablonu starter](bootstrap/_static/bootstrap-in-starter-template.png)

Dodawanie Bootstrap do platformy ASP.NET Core projekt jest po prostu kwestią dodaniem jej do *bower.json* jako zależność:

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

Jest to zalecany sposób dodawania Bootstrap do projektu programu ASP.NET Core.

Można także zainstalować narzędzia bootstrap przy użyciu jednej z kilku Menedżery pakietów, takich jak Bower, npm i NuGet. W każdym przypadku proces jest zasadniczo taki sam:

### <a name="bower"></a>Program bower

```console
bower install bootstrap
```

### <a name="npm"></a>npm

```console
npm install bootstrap
```

### <a name="nuget"></a>NuGet

```console
Install-Package bootstrap
```

> [!NOTE]
> Zalecanym sposobem instalowania zależności po stronie klienta, jak za pomocą rozwiązania Bower Bootstrap w programie ASP.NET Core (przy użyciu *bower.json*, jak pokazano powyżej). Użyj programu npm/NuGet są wyświetlane, aby zademonstrować, jak łatwo można dodać Bootstrap do innych typów aplikacji sieci web, w tym wcześniejsze wersje programu ASP.NET.

Masz odwołujące się do własnego lokalne wersje Bootstrap, należy odwołać je w dowolnej strony, używające go. W środowisku produkcyjnym powinny odwoływać się bootstrap korzystanie z sieci CDN. W szablonie domyślnego platformy ASP.NET Core lokacji *_Layout.cshtml* pliku więc następująco:

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Jeśli zamierzasz korzystać z żadnego z wtyczek jQuery Bootstrap firmy, należy także odwoływać się do technologii jQuery.

## <a name="basic-templates-and-features"></a>Szablony podstawowe i funkcje

Najbardziej podstawowa szablonu Bootstrap wygląda bardzo podobnie *_Layout.cshtml* pliku pokazano powyżej i po prostu obejmuje podstawowe menu nawigacji i miejsce do renderowania pozostałej części strony.

### <a name="basic-navigation"></a>Podstawowa Nawigacja

Domyślny szablon korzysta z zestawu `<div>` elementy do renderowania w górnym pasku nawigacyjnym i głównej części strony. Jeśli używasz języków HTML5, możesz zastąpić pierwszy `<div>` oznaczyć za pomocą `<nav>` tag, aby uzyskać ten sam efekt, ale z semantyką bardziej precyzyjne. W tym pierwszym `<div>` widać, istnieje kilka innych. Po pierwsze, `<div>` z klasą "container", a następnie w ramach, dwa kolejne `<div>` elementów: "navbar-header" i "navbar Zwiń". Div pasek nawigacyjny Nagłówek zawiera przycisk, który będzie wyświetlany, gdy ekran jest poniżej niektóre minimalne szerokości, pokazujący 3 poziome linie (tak zwane "typu" hamburger "ikonę"). Ikona jest renderowany przy użyciu czystego kodu HTML i CSS; Brak obrazu jest wymagana. Jest to kod, który wyświetla ikonę, z których każdy <span> jednego ze słupków białe renderowania tagów:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Obejmuje również nazwę aplikacji, która jest wyświetlana w lewym górnym rogu. Główne menu nawigacji jest renderowany przy `<ul>` elementu w drugim div oraz zawiera łącza do główna o i skontaktuj się z pomocą. Poniżej nawigacji, główną część każdej strony jest renderowany w innym `<div>`, jest oznaczona za pomocą klasy "container" i "treść". W prostym, domyślnym \_plik układu tutaj pokazany, zawartość strony są renderowane w określonym widoku powiązanych ze strony i prostego `<footer>` zostanie dodany na końcu `<div>` elementu. Możesz zobaczyć, jak wbudowane o stronie pojawi się przy użyciu tego szablonu:

![Informacje o stronie](bootstrap/_static/about-page-wide.png)

Zwinięty pasek nawigacyjny za pomocą przycisku "typu" hamburger "" w prawym górnym rogu, jest wyświetlany, gdy okno nie spadnie poniżej określonej granicy:

![informacje o stronie za pomocą menu "hamburger"](bootstrap/_static/about-page-hamburger.png)

Kliknięcie ikony, co spowoduje wyświetlenie elementów menu w szufladzie pionowy, który slajdy w dół od górnej części strony:

![informacje o stronie przy użyciu rozwinięte menu typu "hamburger"](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Typografia i łączy

Usługa ładowania początkowego konfiguruje typografii podstawowe witryny, kolory i łącze formatowania w pliku CSS. Ten plik CSS zawiera domyślne style dla tabel, przycisków, elementów formularza i obrazów ([więcej](http://getbootstrap.com/css/)). Jedna z funkcji szczególnie przydatne jest system układu siatki, objęte obok.

### <a name="grids"></a>Siatki

Jedną z najbardziej popularnych funkcji ładowania początkowego jest jego system układu siatki. Nowoczesnych aplikacji sieci web należy unikać używania `<table>` tag w przypadku układu, zamiast tego ograniczania użycia tego elementu do rzeczywistych danych tabelarycznych. Zamiast tego kolumn i wierszy może być rozmieszczony przy użyciu szeregu `<div>` elementów i odpowiednich klas CSS. Ma kilka zalet tego podejścia, łącznie z możliwością dostosowania układu siatki, aby wyświetlić w pionie na wąskich ekranach, takich jak na telefonach.

[System układu siatki w bootstrap](http://getbootstrap.com/css/#grid) opiera się na 12 kolumn. Ten numer został wybrany, ponieważ można je podzielić równomiernie na 1, 2, 3 lub 4 kolumny i szerokości kolumn mogą się różnić się w ciągu 1/12 szerokości pionowa ekranu. Aby rozpocząć korzystanie z systemu układów siatki, należy najpierw kontener `<div>` , a następnie dodaj wiersz `<div>`, jak pokazano poniżej:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Następnie dodaj dodatkowe `<div>` elementów dla każdej kolumny, a następnie określ liczbę kolumn, `<div>` powinien zajmować (poza 12) jako część klasy CSS, rozpoczynając od "kolumna - md-". Na przykład jeśli chcesz po prostu ma dwie kolumny w taki sam rozmiar, należy użyć klasy "kolumna md-6" dla każdej z nich. W tym przypadku "md" to skrót od "średnia" i odwołuje się do standardowych wymiarach komputer stacjonarny formaty wyświetlania. Istnieją cztery różne opcje, których mogą wybierać, a każda stosowanych w odniesieniu do wyższych szerokości Jeśli zastąpiona (więc chcąc układu można usunąć, niezależnie od tego, szerokość ekranu, można określić tylko klasy xs).

Prefiks klasy CSS | Warstwa urządzenia | Szerokość
:---: | :---: | :---:
Kolumna-xs - | Telefony | < 768px
Kolumna-sm - | Tablety | >= 768px
Kolumna-md - | Komputery stacjonarne | >= 992px
Kolumna-lg - | Większe stacjonarnym Wyświetla | >= 1200px

Podczas określania dwie kolumny zarówno z "kolumna md-6" układ wynikowy będzie dwie kolumny w rozdzielczościach pulpitu, ale tymi dwiema kolumnami stosu w pionie podczas renderowania w mniejszych urządzeniach (lub mniejszą niż okno przeglądarki na pulpicie), dzięki czemu użytkownicy mogą łatwo wyświetlić zawartość, ale nie trzeba być przewijane w poziomie.

Usługa ładowania początkowego zawsze domyślnie układ jednokolumnową, wystarczy tylko określić kolumny, gdy więcej niż jedną kolumnę. Jedyną sytuacją, należy jawnie określić, że `<div>` podejmowanie wszystkich 12 kolumn będzie zastąpić zachowanie większa warstwa urządzenia. Podczas określania wielu klas warstwa urządzenia, może być konieczne resetowania renderowania kolumny w niektórych punktach. Dodawanie div clearfix, będzie on widoczny w obrębie niektórych okienka ekranu można to osiągnąć, jak pokazano poniżej:

![Siatka wąskie i dwubajtowe okienka ekranu](bootstrap/_static/narrow-and-wide-viewport-grid.png)

W powyższym przykładzie 1 i 2 udostępnianie wierszy w układzie "md", podczas gdy drugi i trzeci udostępnianie wierszy w układzie "xs". Bez clearfix `<div>`, dwoma i trzema nie są wyświetlane poprawnie w widoku "xs" (Pamiętaj, że są wyświetlane tylko jeden, 4 i 5):

![Siatka bez użycia clearfix](bootstrap/_static/grid-without-clearfix.png)

W tym przykładzie, tylko jeden wiersz `<div>` było używane, i nadal Bootstrap przede wszystkim postąpiły w odniesieniu do układu i zestawianie kolumn. Zazwyczaj należy określić wiersz `<div>` dla każdego wiersza w poziomie wymaga układu i oczywiście można zagnieżdżać Bootstrap siatki w obrębie siebie nawzajem. Po wykonaniu każdej siatce zagnieżdżonych zajmują 100% szerokości elementu, w którym znajduje się, następnie można podzielić przy użyciu klas kolumny.

### <a name="jumbotron"></a>Jumbotron

Jeśli używano domyślne szablony ASP.NET Core MVC w programie Visual Studio 2012 lub 2013, prawdopodobnie przedstawiono Jumbotron w działaniu. Odwołuje się on duży sekcją pełnej szerokości strony, który może służyć do wyświetlania obrazu tła dużych, wywołanie akcji, rotator lub podobnych elementów. Aby dodać jumbotron do strony, po prostu Dodaj `<div>` i nadaj jej klasę "jumbotron", a następnie umieść kontener `<div>` wewnątrz i Dodaj zawartość. Firma Microsoft można łatwo dostosować standardowe informacje o stronie na potrzeby jumbotron główne nagłówki, które są wyświetlane:

![przykład jumbotron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Przyciski

Na poniższej ilustracji przedstawiono klasy przycisk domyślny i ich kolorów.

![przyciski motywów](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Wskaźniki

Wskaźniki dotyczą małe, zwykle numerycznych objaśnienia obok elementu nawigacji. Pokazują liczbę komunikatów lub powiadomienia oczekiwania lub obecności aktualizacji. Określenie takie identyfikatory jest proste i polega na dodaniu `<span>` zawierający tekst, z klasą "badge":

![wskaźniki motywów](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Alerty

Może być konieczne wyświetlenie pewnego rodzaju powiadomienia, alertu lub komunikat o błędzie użytkownicy twojej aplikacji. To, gdzie przydają się standardowych klas alertu. Istnieją cztery różne poziomy ważności ze schematami skojarzone kolorów:

![alerty motywów](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Navbars i menu

Nasze układ zawiera już standardowy pasek nawigacyjny, ale w motywie Bootstrap obsługuje opcje dodatkowymi stylami. Firma Microsoft może również łatwo zoptymalizowany pod kątem do wyświetlania na pasku nawigacyjnym w pionie, a nie w poziomie Jeśli która zawiera preferowane, elementy również jak dodanie podrzędnych nawigacji w menu wysuwanych zapewniających. Menu nawigacji prosty, takich jak paski karty są wbudowane w górnej części `<ul>` elementów. Można je utworzyć bardzo — wystarczy po prostu dostarczanie im klas CSS "nav" i "nawigacji kart":

![tabstrips motywów](bootstrap/_static/theme-tabstrips.png)

Navbars są wbudowane w podobny sposób, ale są nieco bardziej skomplikowane. Wszystko zaczyna `<nav>` lub `<div>` z klasą "pasek nawigacyjny" w ramach którego div kontener zawiera pozostałe elementy. Nasza strona zawiera paska nawigacyjnego w jej nagłówku już — przedstawionego poniżej po prostu rozszerza się na tym, dodanie obsługi menu rozwijanego:

![navbars motywów](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Dodatkowe elementy

Motyw domyślny również może służyć do prezentowania tabel HTML w stylu dobrze sformatowana, łącznie z obsługą rozłożone widoków. Brak etykiet przy użyciu stylów, które są podobne do tych przycisków. Można tworzyć niestandardowe menu rozwijanych, które obsługują opcje dodatkowymi stylami poza standardowe HTML `<select>` elementu, wraz z Navbars, takiego jak już używa naszej witryny początkowej domyślnej. Jeśli potrzebujesz pasek postępu, istnieje kilka stylów do wyboru, a także wyświetlanie listy grup i paneli, które obejmują tytuł i zawartości. Poznaj opcje dodatkowe w standardowych motywie Bootstrap tutaj:

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Więcej motywów

Standardowa motywie Bootstrap można rozszerzyć przez zastąpienie niektórych lub wszystkich jego CSS dostosowanie kolorów i styl zgodnie z potrzebami własną aplikację. Jeśli chcesz rozpocząć od gotowych do użycia motywu, istnieje kilka motywów galerii online, specjalizują się w Bootstrap motywy, takich jak WrapBootstrap.com (który ma komercyjnych kompozycje) i Bootswatch.com (co oferuje bezpłatna motywy). Dużym stopniem funkcjonalność w oparciu o podstawowe motywie Bootstrap, takich jak rozbudowaną obsługę administracyjne menu i pulpitów nawigacyjnych niektóre z dostępnych szablonów płatnych Udostępnij rozbudowane wykresów i mierników. Przykładem popularnych szablonów płatnych jest Inspinia, który jest pokazany na poniższym zrzucie ekranu:

![Przykład motywu inspinia](bootstrap/_static/theme-inspinia.png)

Jeśli chcesz zmienić swoje motywie Bootstrap, umieść *bootstrap.css* pliku motywu w **wwwroot/css** folder i zmień odwołania w *_Layout.cshtml* Aby wskazać. Zmień łącza dla wszystkich środowisk:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Jeśli chcesz utworzyć własny pulpit nawigacyjny, można uruchomić z przykładu bezpłatna, dostępna tutaj: [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Składniki

Oprócz tych elementów, już omówione Bootstrap obejmuje obsługę różnych [wbudowane składniki interfejsu użytkownika](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Usługa ładowania początkowego obejmuje zestawy ikon z Glyphicons ([http://glyphicons.com](http://glyphicons.com)), z ponad 200 ikonami dostępne do użycia w aplikacji sieci web z obsługą ładowania początkowego. Poniżej przedstawiono tylko niewielką próbkę:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>Grupy danych wejściowych

Danych wejściowych grupy Zezwalaj na paczki dodatkowy tekst lub przyciski z elementu input, zapewniając użytkownikowi bardziej intuicyjne środowisko pracy:

![grupy danych wejściowych](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Linki do stron nadrzędnych

Linki do stron nadrzędnych są typowe składnik interfejsu użytkownika używane do wyświetlania użytkownikowi ich niedawną historię lub głębokość hierarchii nawigacji w witrynie sieci. Dodaj je łatwo, stosując dowolnej klasy "łączy do stron nadrzędnych" `<ol>` elementu listy. Dołączanie wbudowana obsługa dzielenia na strony za pomocą klasy "podział na strony" na `<ul>` elemencie `<nav>`. Dodawanie interaktywnych osadzone pokazy slajdów i wideo za pomocą `<iframe>`, `<embed>`, `<video>`, lub `<object>` elementy, które Bootstrap, automatycznie nadawania stylu. Za pomocą określonej klasy, takie jak "osadzić — elastyczna — 16by9", należy określić określonej proporcji.

## <a name="javascript-support"></a>Obsługa języka JavaScript

Biblioteka języka JavaScript w bootstrap obsługuje interfejs API uwzględnione składniki, dzięki czemu możesz kontrolować swoje zachowanie programowo w aplikacji. Ponadto *bootstrap.js* obejmuje ponad tuzina jQuery niestandardowych wtyczek, zapewniając dodatkowe funkcje, takie jak przejścia, dialogów modalnych przewiń wykrywania (Aktualizowanie style, na której użytkownik ma być przewijane w dokumencie podstawie), Zwiń zachowanie, karuzeli i umieszczanie menu do okna, dlatego nie przewinięcie ekranu. Nie ma wystarczającej ilości miejsca do obejmujące wszystkie dodatki JavaScript wbudowaną Bootstrap — Aby dowiedzieć się więcej, odwiedź [ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Podsumowanie

Usługa ładowania początkowego zapewnia platforma sieci web, która pozwala szybko i wydajnie układu i stylu szerokiej gamy witryn internetowych i aplikacji. Jego podstawowe typografii i style zapewniają przyjemny wygląd i działanie może łatwo operować dzięki obsłudze niestandardowy motyw, specjalnie ręcznie lub kupić komercyjnego punktu widzenia. Obsługuje ona hostem składników sieci web, które będzie wymagana kosztowne formanty innych firm w celu obsługi standardów sieci web nowoczesnych i Otwórz w przeszłości.
