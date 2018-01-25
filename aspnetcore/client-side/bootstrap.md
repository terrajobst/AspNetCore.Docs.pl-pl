---
title: "Tworzenie doskonałych, dynamiczne witryny z ładowania początkowego"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bootstrap
ms.openlocfilehash: 4332dce4c341cb8e5c241a67547048301ae9a8b1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="building-beautiful-responsive-sites-with-bootstrap"></a>Tworzenie doskonałych, dynamiczne witryny z ładowania początkowego

<a name="bootstrap-index"></a>

Przez [Steve Smith](https://ardalis.com/)

Ładowania początkowego jest obecnie najpopularniejsze struktura sieci web do tworzenia aplikacji sieci web odpowiada. Czy jesteś Początkujący frontonu projektu i rozwoju lub eksperta oferuje funkcje i korzyści, które może poprawić środowisko użytkowników z witryny sieci web. Ładowania początkowego jest wdrożona jako zestawu plików CSS i JavaScript i ma na celu skali z witryny sieci Web lub aplikacji z telefonów do tabletów do pulpitów wydajnie.

## <a name="getting-started"></a>Wprowadzenie

Istnieje kilka sposobów, aby rozpocząć korzystanie z ładowania początkowego. Jeśli zaczynasz nowej aplikacji sieci web w programie Visual Studio, można wybrać domyślne początkowy szablon dla platformy ASP.NET Core, w których wielkość Bootstrap są zainstalowane:

![Wykryto w widoku solution starter szablonu](bootstrap/_static/bootstrap-in-starter-template.png)

Dodawanie Bootstrap do platformy ASP.NET Core projektu jest po prostu dodanie go do *bower.json* jako zależność:

[!code-json[Main](../common/samples/WebApplication1/bower.json?highlight=5)]

Jest to zalecany sposób dodać Bootstrap do projektu platformy ASP.NET Core.

Można również zainstalować przy użyciu jednej z kilku menedżerów pakietu, takie jak Bower, npm lub NuGet ładowania początkowego. W każdym przypadku proces jest zasadniczo taki sam:

### <a name="bower"></a>Bower

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
> Zalecanym sposobem instalowania zależności po stronie klienta, jak za pomocą rozwiązania Bower Bootstrap w ASP.NET Core (przy użyciu *bower.json*, jak pokazano powyżej). Korzystanie z programu npm/NuGet są wyświetlane pokazują, jak łatwo można dodać ładowania początkowego do innych typów aplikacji sieci web, w tym wcześniejsze wersje programu ASP.NET.

W przypadku odwołania do własnych lokalnych wersji Bootstrap, należy odwoływać wszystkich stron, które będą używać go. W środowisku produkcyjnym należy odwoływać się przy użyciu CDN ładowania początkowego. W szablonie witryny ASP.NET domyślne *_Layout.cshtml* plik tak jak to:

[!code-html[Main](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Jeśli użytkownik chce się przy użyciu dowolnego ładowania początkowego na jQuery wtyczki, należy również odwołanie jQuery.

## <a name="basic-templates-and-features"></a>Szablony podstawowe i funkcje

Najbardziej podstawowa szablonu Bootstrap wygląda bardzo podobnie *_Layout.cshtml* pliku pokazano powyżej i po prostu zawiera podstawowe menu nawigacji i miejsce do renderowania pozostałej części strony.

### <a name="basic-navigation"></a>Podstawowe nawigacji

Domyślny szablon używa zestawu `<div>` elementy do renderowania górny pasek nawigacyjny i głównej części strony. Jeśli używasz HTML5, możesz zastąpić pierwszy `<div>` tagu z `<nav>` tag, aby uzyskać ten sam efekt, ale semantyka bardziej dokładne. W tym pierwszym `<div>` widać, istnieje kilka innych. Najpierw `<div>` z klasą "kontener", a następnie w tym, dwa więcej `<div>` elementów: "navbar-header" i "navbar-collapse". Div pasek nawigacyjny Nagłówek zawiera przycisk, który będzie wyświetlany, gdy ekran jest poniżej niektórych minimalnej szerokości, pokazujący 3 poziome linie (tak zwane "hamburger ikonę"). Ikona jest renderowany przy użyciu czysty HTML i CSS; obraz nie jest wymagane. To jest kod, który wyświetla ikonę z poszczególnymi <span> znaczników renderowania jeden z pasków białe:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Zawiera również nazwę aplikacji, która pojawia się w lewej górnej części. Menu główne nawigacji jest renderowana przez `<ul>` element div drugiego oraz linki do Home około i skontaktuj się z pomocą. Linki do dodatkowych rejestru i logowania są dodawane przez _LoginPartial wiersz w wierszu 29. Poniżej nawigacji, główną każdej strony jest renderowany w innym `<div>`, oznaczone przy klasy "treść" i "kontenera". W pliku _Layout domyślne proste pokazane, zawartość strony są renderowany przez widok określonych skojarzone z strony, a następnie prostą `<footer>` zostanie dodany na końcu `<div>` elementu. Zobacz temat jak wbudowane o stronie pojawia się przy użyciu tego szablonu:

![Strona — informacje](bootstrap/_static/about-page-wide.png)

Pasek nawigacyjny zwinięte, klikając przycisk "hamburger" w górnym rogu, jest wyświetlany, gdy okno spadnie poniżej określonej granicy:

![Strona z hamburger menu — informacje](bootstrap/_static/about-page-hamburger.png)

Kliknięcie ikony ujawnia elementów menu w szufladzie pionowy, który slajdów w dół od górnej części strony:

![Strona z menu hamburger rozwinięty — informacje](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Typografii i łącza

Bootstrap konfiguruje podstawowe typografii witryny, kolory i łącze formatowania w pliku CSS. Ten plik CSS zawiera domyślne style dla tabel, przycisków, elementów formularza i obrazów ([więcej](http://getbootstrap.com/css/)). Jedną funkcję szczególnie przydatne jest system układu siatki, obok objętych usługą.

### <a name="grids"></a>Siatki

Jedną z najbardziej popularnych funkcji ładowania początkowego jest jego siatki układu systemu. Nowoczesnych aplikacji sieci web należy unikać używania `<table>` tag układu, zamiast tego ograniczenie użycia tego elementu, do rzeczywistych danych tabelarycznych. Zamiast tego, kolumn i wierszy może zostać uwzględnione przy użyciu serii `<div>` elementy oraz odpowiednie klasy CSS. Istnieje kilka zalety tego podejścia, w tym możliwość dostosowania układu siatki mają być wyświetlane w pionie na wąskich ekranach, takich jak na telefonach.

[Jego ładowania początkowego siatki układu systemu](http://getbootstrap.com/css/#grid) opiera się na dwunastu kolumn. Ta liczba została wybrana, ponieważ można ją podzielić równomiernie na 1, 2, 3 lub 4 kolumn i szerokości kolumn mogą się różnić się w ciągu 1/12 pionowy szerokości ekranu. Aby rozpocząć korzystanie z systemu układu siatki, należy uruchomić z kontenerem `<div>` , a następnie dodaj wiersz `<div>`, jak pokazano poniżej:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Następnie dodaj dodatkowe `<div>` elementy dla każdej kolumny i określ liczbę kolumn który `<div>` powinny zajmować (poza 12) jako część klasy CSS, począwszy od "kolumna - md-". Na przykład jeśli chcesz mieć dwie kolumny w taki sam rozmiar, należy użyć klasy "kolumna md-6" dla każdej z nich. W takim przypadku "md" to skrót od "średnia" i odwołuje się do rozmiarów wyświetlania o komputera stacjonarnego. Istnieją cztery różne opcje, które są dostępne, a każda będzie służyć do wyższych szerokości Jeśli zastąpiona (więc jeśli chcesz układ należy naprawić niezależnie od szerokości ekranu, wystarczy podać xs klasy).

Prefiks klasy CSS | Warstwa urządzenia | Szerokość
:---: | :---: | :---:
Kolumna-xs - | Telefony | < 768px
Kolumna-ZS - | Tablety | >= 768px
Kolumna-md - | Komputery stacjonarne | >= 992px
Kolumna-lg - | Większe Wyświetla pulpitu | >= 1200px

Podczas określania dwie kolumny zarówno z "kolumna md-6" układu wynikowy będzie dwie kolumny o rozdzielczościach pulpitu, ale tymi dwiema kolumnami stosu pionowo podczas renderowania na mniejsze urządzenia (lub mniejszą niż okno przeglądarki na pulpicie), dzięki czemu użytkownicy mogą łatwo wyświetlić zawartość bez konieczności przewijane w poziomie.

Ładowania początkowego jest zawsze domyślnie układ pojedynczej kolumny, dlatego trzeba określić kolumny, gdy ma więcej niż jedną kolumnę. Tylko wtedy, należy jawnie określić, że `<div>` podejmowanie wszystkie kolumny 12 byłoby zastąpienie zachowania większych warstwy urządzenia. Podczas określania wielu klas warstwy urządzeń, może być konieczne zresetowanie renderowania kolumny w niektórych punktach. Dodawanie div clearfix, które są tylko widoczne w niektórych okienka ekranu można to osiągnąć, jak pokazano poniżej:

![Siatka wąskie i dwubajtowe okienka ekranu.](bootstrap/_static/narrow-and-wide-viewport-grid.png)

W powyższym przykładzie 1 i 2 udziału wiersza w układzie "md", podczas gdy drugi i trzeci wiersz w układzie "xs" udostępnianie. Bez clearfix `<div>`, 2 i 3 nie są wyświetlane poprawnie w widoku "xs" (należy pamiętać, że są wyświetlane tylko jeden, 4 i 5):

![Siatka bez użycia clearfix](bootstrap/_static/grid-without-clearfix.png)

W tym przykładzie tylko pojedynczy wiersz `<div>` był używany, a nadal Bootstrap przede wszystkim została co układ i układania kolumn. Zazwyczaj należy określić wiersz `<div>` dla każdego wiersza w poziomie wymaga układ i oczywiście można zagnieżdżać ładowania początkowego siatki wewnątrz innych. Po wykonaniu każdego zagnieżdżonych siatki zajmie 100% szerokości elementu, w której znajduje się następnie mogą być podzielone przy użyciu klasy kolumny.

### <a name="jumbotron"></a>Jumbotron

Jeśli używano domyślne szablony ASP.NET MVC w programie Visual Studio 2012 lub 2013, prawdopodobnie przedstawiono Jumbotron w akcji. Odnosi się do dużych sekcji pełnej szerokości strony, który może służyć do wyświetlania obrazu tła dużych, wywołanie akcji, rotator lub podobnych elementów. Aby dodać jumbotron do strony, po prostu Dodaj `<div>` i nadaj klasie "jumbotron", a następnie umieść kontener `<div>` wewnątrz i dodać zawartość. Firma Microsoft może łatwo dostosować standard o służą jumbotron główne nagłówki, które są wyświetlane:

![przykład jumbotron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Przyciski

Na poniższej ilustracji przedstawiono domyślne przycisk klasy i ich kolorów.

![przyciski motywów](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Identyfikatory

Identyfikatory odwoływać się do krótkich, zwykle numerycznych objaśnienia obok elementu nawigacji. Pokazują liczbę komunikatów lub powiadomień o oczekiwanie lub obecności aktualizacji. Określenie takie identyfikatory jest tak proste, jak dodawanie <span> zawierającej tekst, w przypadku klasy "wskaźnik":

![identyfikatory motywów](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Alerty

Konieczne może być wyświetlany rodzaj powiadomień, alertu lub komunikat o błędzie z użytkownikami aplikacji. To, gdzie standardowe klasy alertów są przydatne. Istnieją cztery różne poziomy ważności ze schematami skojarzone kolorów:

![alerty motywów](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Navbars i menu

Nasze układ już zawiera standardowe pasek nawigacyjny, ale Bootstrap motywu obsługuje opcje dodatkowymi stylami. Możemy również łatwo można wybrać opcję Wyświetl pasek nawigacyjny pionowo zamiast poziomie Jeśli która zawiera preferowane, elementów oraz jak dodanie podrzędnego nawigacji w menu wysuwane. Menu nawigacji proste, takie jak karta pasków, są tworzone na <ul> elementy. Można je utworzyć bardzo prosty przez zapewnienie im tylko z klas CSS "nav" i "Nawiguj do karty":

![tabstrips motywów](bootstrap/_static/theme-tabstrips.png)

Navbars są wbudowane w podobny sposób, ale nieco bardziej skomplikowane. Zaczynają `<nav>` lub `<div>` z klasą "navbar", w którym div kontenera przechowuje pozostałe elementy. Naszą stronę obejmuje pasek nawigacyjny w nagłówku już — przedstawionego poniżej po prostu rozszerza się na tym, dodanie obsługi menu rozwijane:

![navbars motywów](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Dodatkowe elementy

Motyw domyślny mogą służyć do prezentowania tabel HTML w stylu dobrze sformatowana, w tym obsługi widoków rozłożone. Brak etykiety style, które są podobne do tych przycisków. Można tworzyć niestandardowe menu rozwijanych obsługujących opcje dodatkowymi stylami poza standardowe HTML `<select>` element wraz z Navbars jak już używa naszego domyślnej witryny początkowej. Jeśli potrzebujesz pasek postępu, istnieje kilka stylów do wyboru, a także wyświetlanie listy grup i panele, które obejmują tytuł i zawartości. Poznaj dodatkowe opcje w standardowe motywu Bootstrap tutaj:

[http://getbootstrap.com/Examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Więcej kompozycji

Standardowa motywu ładowania początkowego można rozszerzyć przez zastąpienie niektórych lub wszystkich jego CSS dostosowywania kolorów i styl stosownie do potrzeb swojej aplikacji. Jeśli chcesz rozpocząć od gotowych motywu, istnieje kilka motywu galerii online który specialize ładowania początkowego kompozycje, takich jak WrapBootstrap.com (mającej kompozycje komercyjnych) i Bootswatch.com (który oferuje wolnego motywów). Niektóre z dostępnych szablonów płatną zapewnić dużą funkcjonalność rozszerzającą podstawowe motywu Bootstrap, takie jak obsługa zaawansowanych menu administracyjnych i pulpity nawigacyjne funkcje sformatowanego wykresów i mierników. Przykład popularnych płatnej szablonu jest Inspinia, obecnie do sprzedaży dla $18, w tym szablonem ASP.NET MVC5 oprócz AngularJS i statyczne wersje HTML. Poniżej przedstawiono przykład zrzutu ekranu.

![Przykład inspinia motywu](bootstrap/_static/theme-inspinia.png)

Jeśli chcesz zmienić kompozycję ładowania początkowego *bootstrap.css* pliku kompozycji w **wwwroot/css** folder i zmień odwołania w *_Layout.cshtml* Aby go wskazać. Zmień łącza dla wszystkich środowisk:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Jeśli chcesz utworzyć własne pulpitu nawigacyjnego, można uruchomić z wolne przykładzie dostępna tutaj: [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Składniki

Oprócz tych elementów już omówione Bootstrap obejmuje obsługę szerokiej gamy [wbudowanych składników interfejsu użytkownika](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Bootstrap obejmuje zestawy ikon z Glyphicons ([http://glyphicons.com](http://glyphicons.com)), z ponad 200 ikonami bezpłatnie do użytku w aplikacji sieci web z obsługą ładowania początkowego. Oto małej przykładowej:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>wejściowy grup

Wejściowy grupy umożliwiają paczki dodatkowy tekst lub przycisków z elementu input udostępnia użytkownikowi bardziej intuicyjne środowisko:

![wejściowy grup](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Obszar nawigacji

Obszar nawigacji są typowe składnik interfejsu użytkownika używane do wyświetlania użytkownikowi ich niedawną historię lub głębokość hierarchii nawigacji witryny sieci. Dodaj je łatwo przez zastosowanie klasy "nawigacją" do dowolnego `<ol>` elementu listy. Dołączenie wbudowaną obsługę podział na strony za pomocą klasy "podział na strony" na `<ul>` w elemencie `<nav>`. Dodawanie reakcji osadzonych pokazy slajdów i wideo za pomocą `<iframe>`, `<embed>`, `<video>`, lub `<object>` elementów, które Bootstrap nadawania stylu automatycznie. Określ określonej proporcji przy użyciu określonych klas takich jak "Osadź reakcji 16by9".

## <a name="javascript-support"></a>Obsługa języka JavaScript

Biblioteka języka JavaScript dla początkowego obsługuje API dołączone składniki, dzięki czemu można kontrolować działanie programowo w aplikacji. Ponadto *bootstrap.js* zawiera ponad dwanaście wtyczek niestandardowe jQuery, zapewniając dodatkowe funkcje, takie jak przejścia, modalne okna dialogowe, przewiń wykrywania (Aktualizowanie style, na którym użytkownik jest przewijane w dokumencie podstawie), Zwiń zachowanie, carousels i umieszczanie menu do okna, więc nie przewinięcie ekranu. Nie jest wystarczające miejsca, aby zaspokajać wszystkie dodatki JavaScript wbudowane ładowania początkowego — Aby dowiedzieć się więcej, odwiedź [http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Podsumowanie

Bootstrap zapewnia platforma sieci web, która może służyć do szybkiego i efektywnie układ i styl szerokiej gamy witryn internetowych i aplikacji. Jego podstawowe typografii i style zapewniają przyjemne wygląd i działanie może łatwo operować dzięki obsłudze motyw niestandardowy, można ręcznie, co lub zakupionych komercyjnie. Hosta składników sieci web, które w przeszłości czy wymagana jest kosztowne formanty innych firm do wykonania podczas obsługi standardów sieci web nowoczesnych i otwórz go obsługuje.
