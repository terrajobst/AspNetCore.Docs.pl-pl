---
title: "Mniejsza, Sass i czcionki świetny w platformy ASP.NET Core"
author: ardalis
description: "Dowiedz się, jak używać mniej Sass i świetny czcionki w aplikacjach ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/less-sass-fa
ms.openlocfilehash: 979f5639e382560d952df45ba6e0b8af3b132c2d
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a>Wprowadzenie do aplikacji stylów jest mniejsza, Sass i czcionki świetny w ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Użytkownicy aplikacji sieci web ma coraz wysokiej oczekiwania po przejściu do nadawania stylu i doświadczenia, ogólne. Nowoczesnych aplikacji sieci web często korzystaj z zaawansowanych narzędzi i platform do definiowania i zarządzanie ich wyglądu i działania w sposób ciągły. Struktury, takich jak [Bootstrap](http://getbootstrap.com/) można zdecydowanie kierunku Definiowanie ze wspólnego zestawu style i opcje układu dla witryn sieci web. Jednak w większości witryn nieuproszczony również korzystać z można skutecznie zdefiniuj oraz obsługa style i plików kaskadowych (CSS) arkuszy stylów, a także łatwy dostęp do innych niż obraz ikony, które sprawić, że bardziej intuicyjnego interfejsu witryny. To miejsce języków i narzędzi, które obsługują [mniej](http://lesscss.org/) i [Sass](http://sass-lang.com/), i biblioteki, takich jak [czcionki świetny](http://fontawesome.io/), są dostępne w.

## <a name="css-preprocessor-languages"></a>Języki preprocesora CSS

Języki, które są skompilowane w innych językach, aby poprawić komfort pracy z podstawowej języka, są określane jako preprocessors. Istnieją dwa popularne preprocessors CSS: mniej i Sass.  Te preprocessors dodawania funkcji do arkusza CSS, takie jak obsługa zmiennych i zagnieżdżone reguły, które poprawy utrzymanie dużych, złożonych arkuszy stylów. CSS jako język jest bardzo proste, brak obsługę nawet coś najprostszą zmienne, i to sprawia pliki CSS powtarzających się i przeglądarek. Dodawanie rzeczywistym programowania funkcji języka za pomocą preprocessors może pomóc pozwalają ograniczyć dublowania i zapewnienia lepszej organizacji reguły stylów. Program Visual Studio udostępnia wbudowaną obsługę zarówno mniej i Sass, jak również rozszerzeń, które można jeszcze bardziej poprawić środowisko programistyczne podczas pracy z tych języków.

Jako prosty przykład sposobu preprocessors może poprawić czytelność i łatwości konserwacji informacji o stylu należy wziąć pod uwagę to CSS:

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

Przy użyciu mniejsza, to można ponownego napisania wyeliminowanie wszystkich duplikatów, za pomocą *domieszki* (o nazwie tak, ponieważ umożliwia "mieszać" właściwości z jednej klasy lub zestawu reguł do innego):

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>mniej

Uruchamia preprocesora mniej CSS przy użyciu środowiska Node.js. Aby zainstalować mniejsza, należy użyć Node Package Manager (npm) z wiersza polecenia (-g oznacza "globalne"):

```console
npm install -g less
```

Jeśli używasz programu Visual Studio, mogą Rozpoczynanie pracy z mniej przez dodanie jednego lub kilku mniej plików do projektu, a następnie konfigurując Gulp (lub Grunt) ich przetworzyć w czasie kompilacji. Dodaj *style* folderu do projektu, a następnie dodaj nową mniej plik o nazwie *main.less* do tego folderu.

![Dodaj plik mniej](less-sass-fa/_static/add-less-file.png)

Po dodaniu strukturę folderu powinien wyglądać następująco:

![Struktura folderów](less-sass-fa/_static/folder-structure.png)

Teraz można dodać niektóre podstawowe stylów do pliku, który zostanie skompilowany w CSS i wdrożony wwwroot folder przez Gulp.

Modyfikowanie *main.less* uwzględnienie następujących zawartość, która tworzy paletę kolorów prostego na podstawie pojedynczego koloru podstawowego.

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base` a druga @-prefixed zmienne są elementy. Każdy z nich reprezentuje kolor. Z wyjątkiem `@base`, są ustawione przy użyciu funkcji kolorów: rozjaśnić Ciemniej i pokrętła. Rozjaśnić i przyciemnić oczekiwaniami pomogą czy; SPIN dopasowuje odcień koloru liczba stopni (około koło kolorów). Inteligentne zignorować zmiennych, które nie są używane, tak aby zademonstrować, jak działają te zmienne, należy użyć je gdzieś jest mniej procesora. Klasy `.baseColor`, itp. zostaną przedstawione obliczone wartości poszczególnych zmiennych w utworzonym pliku CSS.

### <a name="get-started"></a>Wprowadzenie

Utwórz **plik konfiguracji programu npm** (*package.json*) w folderze projektu i edytować go, aby odwołać `gulp` i `gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

Zainstaluj zależności, albo w wierszu polecenia w folderze projektu lub w programie Visual Studio **Eksploratora rozwiązań** (**zależności > npm > przywracania pakietów**).

```console
npm install
```

![VS przywracania pakietów](less-sass-fa/_static/restore-packages.png)

W folderze projektu Utwórz **system Gulp pliku konfiguracyjnego** (*gulpfile.js*) do definiowania zautomatyzowanego procesu.  Dodawanie zmiennej w górnej części pliku do reprezentowania mniej i mniej uruchomienie zadania:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Otwórz **Eksploratora modułu uruchamiającego zadania** (**Widok > innych okien > Eksploratora modułu uruchamiającego zadania**). Wśród zadań, powinny pojawić się nowe zadanie o nazwie `less`. Może być konieczne Odśwież okno programu.

Uruchom `less` zadań, a wyświetlone dane wyjściowe podobne do co to jest następujący:

![mniej modułu uruchamiającego zadania](less-sass-fa/_static/less-task-runner.png)

*Wwwroot/css* folder zawiera teraz nowy plik *main.css*:

![główne css utworzone](less-sass-fa/_static/main-css-created.png)

Otwórz *main.css* i zostanie wyświetlony ekran podobny do następujących:

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

Dodaj prostą stronę HTML do *wwwroot* folderu, a odwołanie *main.css* wyświetlić palety kolorów w akcji.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

Widać, że 180 stopni pokrętła na `@base` użyta do wyprodukowania `@background` spowodowała koło kolorów przeciwne kolor `@base`:

![przykład mniej testowych](less-sass-fa/_static/less-test-screenshot.png)

Mniejsze także zapewnia obsługę zagnieżdżonych reguły, a także zapytaniami multimediów zagnieżdżonych. Na przykład definiującego zagnieżdżone hierarchie jak menu może spowodować pełne reguły CSS, takich jak te:

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

W idealnym przypadku wszystkie reguły stylu pokrewne zostaną umieszczone razem w pliku CSS, ale w praktyce nie ma nic wymuszania tej reguły, z wyjątkiem Konwencji i prawdopodobnie komentarze bloku.

Definiowanie te reguły tego samego przy użyciu mniej wygląda następująco:

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

Należy pamiętać, że w przypadku wszystkich elementów podrzędnych elementu `nav` są zawarte w swoim zakresie. Nie ma żadnych powtarzania elementy nadrzędne (`nav`, `li`, `a`), a także spadła liczba całkowita liczba wierszy (chociaż niektóre który jest wynikiem wprowadzanie wartości w tej samej linii w drugim przykładzie). Może być bardzo przydatne, organizacji, aby wyświetlić wszystkie reguły dla danego elementu interfejsu użytkownika w zakresie ograniczonego jawnie, w tym przypadku ustawieniem od pozostałej części pliku nawiasów klamrowych.

`&` Składni jest mniej funkcji selektor z & reprezentujący bieżącego elementu nadrzędnego selektora. Tak, w ramach {...} blok, `&` reprezentuje `a` tag, a tym samym `&:link` jest odpowiednikiem `a:link`.

Zapytaniami multimediów bardzo przydatne podczas tworzenia projektów reakcji, można także współtworzyć silnie powtarzania i złożoność w kodzie CSS. Mniejsze umożliwia zapytania nośnika, aby być zagnieżdżone w obrębie klasy, dzięki czemu definicji klasy całego nie trzeba powtarzać się w różnych najwyższego poziomu `@media` elementów. Na przykład w tym miejscu jest CSS menu dynamiczne:

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

Może to być lepiej określone w czasie krótszym jako:

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

Funkcja innego mniejszą niż już widzieliśmy jest jego obsługę operacji matematycznych, umożliwiając atrybutów stylu, które mają zostać utworzone na podstawie wstępnie zdefiniowanych zmiennych. Dzięki temu aktualizacji powiązanych stylów znacznie łatwiejsze, ponieważ podstawowej zmiennej można modyfikować i wszystkie wartości zależnych automatycznie zmienić.

Pliki CSS, zwłaszcza w przypadku dużych witryn (i zwłaszcza, jeśli są używane zapytaniami multimediów), często można uzyskać dość duży wraz z upływem czasu, co niewygodna pracy z nimi. Mniejsze pliki można zdefiniować oddzielnie, następnie pobierane ze sobą przy użyciu `@import` dyrektywy. Mniejsze można również zaimportować poszczególnych plików CSS, a także w razie potrzeby.

*Mixins* może akceptować parametry, a mniejsza obsługuje logikę warunkową w formie domieszki osłony, zapewniających deklaratywne definiowanie, kiedy niektórych mixins zostały wprowadzone. Użycia osłony domieszki jest aby dostosować kolory, jak światła na podstawie lub ciemny koloru źródłowego. Podana domieszki, który akceptuje parametr koloru, strażnik domieszki może służyć do modyfikowania domieszki na podstawie tego koloru:

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

Podane nasze bieżące `@base` wartość `#663333`, ten mniej skrypt utworzy następujące CSS:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Mniejsze udostępnia szereg dodatkowych funkcji, ale to należy przedstawić mocy niniejszej przetwarzania wstępnego języka.

## <a name="sass"></a>Sass

Sass jest podobny do mniej, zapewniając obsługę wielu te same funkcje, ale ze składnią nieco inne. Jest zbudowany przy użyciu Ruby zamiast JavaScript, a więc ma wymagania dotyczące różnych konfiguracji. Język Sass oryginalny nie użyto nawiasów klamrowych lub średników, ale zamiast tego zdefiniowanego zakresu przy użyciu biały znak i wcięcia. W wersji 3 Sass wprowadzono nowej składni, **SCSS** ("Sassy CSS"). SCSS jest podobny do CSS, ponieważ ignoruje poziomów wcięć i spacja i użyje średnikami i nawiasów klamrowych.

Aby zainstalować Sass, zwykle można będzie najpierw zainstalować Ruby (wstępnie zainstalowanych na macOS), a następnie uruchom:

```console
gem install sass
```

Jednak jeśli korzystasz z programu Visual Studio, możesz rozpocząć pracę z Sass w taki tak samo jak w przypadku jest mniejsza. Otwórz *package.json* i Dodaj pakiet "gulp-sass" `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

Następnie należy zmodyfikować *gulpfile.js* , aby dodać zmienną sass i zadania do kompilowania plików Sass i umieścić w folderze wwwroot wyniki:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Teraz można dodać pliku Sass *main2.scss* do *style* folderu w katalogu głównym projektu:

![Dodaj plik scss](less-sass-fa/_static/add-scss-file.png)

Otwórz *main2.scss* i Dodaj następujący kod:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Zapisz wszystkie pliki. Teraz po odświeżeniu **Eksploratora modułu uruchamiającego zadania**, zostanie wyświetlony `sass` zadań. Uruchom go i Znajdź */wwwroot/css* folderu. Obecnie *main2.css* pliku z tych zawartości:

```css
body {
    background-color: #CC0000;
}
```

Sass obsługuje zagnieżdżenia w większości takie same zakończonych, że mniej tak, świadczenie podobne. Pliki można podzielić przez funkcję i uwzględniane przy użyciu `@import` dyrektywy:

```sass
@import 'anotherfile';
```

Sass obsługuje również mixins przy użyciu `@mixin` — słowo kluczowe definiowania ich i `@include` aby je uwzględnić, jak w poniższym przykładzie z [sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Oprócz mixins Sass umożliwia koncepcja dziedziczenia, dzięki czemu jedną klasę do innego rozszerzenia. Jest on podobny do domieszki, ale powoduje mniejsza ilość kodu CSS. Odbywa się przy użyciu `@extend` — słowo kluczowe. Aby wypróbować mixins, Dodaj następujący kod do Twojej *main2.scss* pliku:

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Sprawdź dane wyjściowe w *main2.css* po uruchomieniu `sass` zadań w **Eksploratora modułu uruchamiającego zadania**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Zwróć uwagę, wszystkie wspólne właściwości alertu domieszki są powtarzane w każdej klasie. Mixin — jak dobrej zadania Pomoc wyeliminować dublowanie w czasie opracowywania, ale nadal jest tworzenie CSS z dużą ilością duplikatów, co przekracza niezbędne pliki CSS - potencjalny problem z wydajnością.

Teraz Zastąp alertu domieszki z `.alert` klasy i zmień `@include` do `@extend` (Pamiętaj o tym rozszerzyć `.alert`, a nie `alert`):

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Ponownie uruchom Sass i zbadać wynikowy CSS:

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Teraz właściwości są definiowane tylko jako liczbę razy i lepiej jest generowany CSS.

Sass obejmuje również funkcje i operacje logikę warunkową, podobnie jak mniejsze. W rzeczywistości możliwości dwóch języków są bardzo podobne.

## <a name="less-or-sass"></a>Mniej lub Sass?

Nadal jest konsensu nie określa, czy jest zazwyczaj lepiej jest mniejsze lub Sass (lub nawet czy preferować oryginalnego Sass lub nowsza składnia SCSS w Sass). Prawdopodobnie jest najważniejszych decyzji **Użyj jednego z tych narzędzi**, w przeciwieństwie do właśnie programowania ręcznego plików CSS. Po dokonaniu który decyzja zarówno mniej i Sass powinno się.

## <a name="font-awesome"></a>Świetny czcionki

Oprócz CSS preprocessors innego doskonałą dla stylów nowoczesnych aplikacji sieci web jest świetne czcionki. Specjalna czcionki to pakiet narzędzi umożliwiający zawiera ponad 500 ikony wektor skalowalne, które mogą służyć za darmo w aplikacji sieci web. Pierwotnie został zaprojektowany do współpracy z Bootstrap, ale ma nie zależności na tym framework lub żadnych bibliotek JavaScript.

Najłatwiejszym sposobem na szybkie wprowadzenie do czcionki świetny jest można dodać odwołania do niego, przy użyciu jego publicznego dostarczania zawartości (CDN) lokalizacji:

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

Użytkownik może też dodać do projektu programu Visual Studio przez dodanie go do "zależności" w *bower.json*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

Po utworzeniu odwołania do czcionki świetny na stronie, stosując czcionki świetny klas, zwykle prefiksem "fa-", aby Twoje elementów śródwierszowych HTML można dodać ikony aplikacji (takich jak `<span>` lub `<i>`).  Na przykład można dodać ikony do prostej listy i menu przy użyciu kodu w następujący sposób:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

To spowoduje utworzenie następujących w przeglądarce — należy zwrócić uwagę ikona obok każdego elementu:

![listy ikon](less-sass-fa/_static/list-icons-screenshot.png)

Można wyświetlić listę wszystkich dostępnych ikon:

http://fontawesome.io/icons/

## <a name="summary"></a>Podsumowanie

Nowoczesnych aplikacji sieci web wymaga coraz bardziej elastyczny, płynne projekty, które są czyste, intuicyjne i łatwe w użyciu z różnych urządzeń. Zarządzanie złożonością arkusze stylów CSS wymagane na osiągnięcie tych celów najlepiej odbywa się przy użyciu notacji preprocesora mniej lub Sass. Ponadto zestaw narzędzi, takich jak czcionki świetny szybko zapewnić dobrze znane ikony do menu nawigacji tekstową i przyciski użytkownika ogólnej poprawy środowisko aplikacji.
