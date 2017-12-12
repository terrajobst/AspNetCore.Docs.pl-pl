---
title: Struktura Knockout.js MVVM w platformy ASP.NET Core
author: ardalis
description: 
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: d1c5cbd430587b757bb550f8f04355e67f04eb54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a>Struktura Knockout.js MVVM w platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Knockout jest popularnych biblioteka języka JavaScript, które ułatwia tworzenie interfejsów złożonych oparte na danych użytkownika. Można go samodzielnie lub z innych bibliotek, np. jQuery. Jego podstawowym celem jest powiązać odpowiedni model danych zdefiniowany jako obiekt JavaScript elementy interfejsu użytkownika tak, aby podczas wprowadzania zmian do interfejsu użytkownika, model jest aktualizowany i na odwrót. Odcinania ułatwia korzystanie ze wzorca Model-View-ViewModel (MVVM) w aplikacji sieci web po stronie klienta zachowanie. Dwa główne pojęcia, które jedną muszą dowiedzieć się, podczas pracy z implementacją MVVM odcinania w są dostrzegalne elementy i powiązania.

## <a name="getting-started"></a>Wprowadzenie

Knockout jest wdrożona jako pojedynczy plik JavaScript, więc instalowania i korzystania z niego jest bardzo prosta przy użyciu [bower](bower.md). Zakładając, że masz już [bower](bower.md) i [system gulp](using-gulp.md) skonfigurowane, otwórz *bower.json* w Twojej platformy ASP.NET Core projektu i Dodawanie zależności odcinania, jak pokazano poniżej:

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

Z tym w miejscu mogą być następnie ręcznie uruchomić bower, otwierając Eksploratora modułu uruchamiającego zadania (w widoku ‣ ‣ inne okna Eksploratora modułu uruchamiającego zadania), a następnie w obszarze zadania kliknij prawym przyciskiem myszy bower i wybierz polecenie Uruchom. Wynik powinien być podobny do poniższego:

![bower odcinania uruchomionej w Eksploratora modułu uruchamiającego zadania](knockout/_static/bower-knockout.png)

Teraz można spojrzeć na projekt `wwwroot` folderu, powinny pojawić się odcinania zainstalowany w folderze lib.

![odcinania zainstalowany w folderze lib](knockout/_static/wwwroot-knockout.png)

Zaleca się w środowisku produkcyjnym odwoływania odcinania za pośrednictwem sieci dostarczania zawartości lub CDN, ponieważ zwiększa prawdopodobieństwo, że użytkownicy mają już pamięci podręcznej kopię pliku i w związku z tym nie należy pobrać go w ogóle. Knockout jest dostępna w kilku CDN, w tym Microsoft Ajax CDN, w tym miejscu:

[http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

Aby uwzględnić odcinania na stronie, który zostanie użyty, po prostu Dodaj `<script>` element odwołuje się do pliku z wszędzie tam, gdzie można będzie obsługiwać go (z aplikacją lub za pośrednictwem CDN):

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a>Dostrzegalne elementy, ViewModels i proste powiązanie

Mogą być już znasz przy użyciu języka JavaScript do modyfikowania elementów na stronie sieci web za pomocą bezpośredniego dostępu do modelu DOM lub biblioteki, takich jak jQuery. Zwykle zachowanie tego typu uzyskuje się poprzez pisanie kodu bezpośrednio ustawić wartości elementów w odpowiedzi na pewne akcje użytkownika. W przypadku odcinania deklaratywne podejście jest pobierana, za pomocą którego elementy na stronie są powiązane z właściwości obiektu. Zamiast pisania kodu do manipulowania elementy modelu DOM, akcje użytkownika, po prostu interakcji z obiektu ViewModel i odcinania odpowiada on za zapewnienie, że elementy na stronie są synchronizowane.

Jako przykład prostego należy wziąć pod uwagę na poniższej liście strony. Obejmuje on `<span>` element z `data-bind` atrybut wskazujący, że NazwaAutora powinna być powiązana zawartości tekstowej. Następnie w bloku kodu JavaScript viewModel zmiennej jest zdefiniowana z tylko jedną właściwość `authorName`, niektóre wartość. Na koniec wywołania `ko.applyBindings` następuje, przekazując tę zmienną viewModel.

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

Podczas wyświetlania w przeglądarce, zawartość <span> zastępuje element o wartości w zmiennej viewModel:

![proste powiązanie odcinania](knockout/_static/simple-binding-screenshot.png)

Mamy teraz pracy proste powiązania jednokierunkowe. Zwróć uwagę, że daleko w kodzie została możemy zapisu JavaScript można przypisać wartości do zawartości zakresu. Jeśli chcemy manipulowania ViewModel możemy podejmowanie ten krok i Dodaj pole tekstowe do wprowadzania HTML i powiązać jego wartość tak samo, jak tak:

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

Ponowne ładowanie strony, widzimy, że ta wartość jest w rzeczywistości powiązane z polem wejściowych:

![powiązania wejściowego odcinania](knockout/_static/input-binding-screenshot.png)

Jednak zmiana wartość w polu tekstowym, odpowiednie wartości w `<span>` elementu nie ulega zmianie. Dlaczego nie?

Problem polega na niczego powiadomienie `<span>` wymaganej w aktualizacji. Po prostu aktualizowanie ViewModel nie zostanie wystarczające, chyba że ViewModel właściwości są ujęte w specjalny typ. Należy użyć **dostrzegalne elementy** w ViewModel na wszystkie właściwości, które wymagają zmiany automatycznie aktualizowane w miarę ich występowania. Zmieniając ViewModel do użycia `ko.observable("value")` zamiast tylko "value", ViewModel spowoduje zaktualizowanie elementów HTML, powiązane z jego wartości w każdym przypadku, gdy nastąpi zmiana. Należy pamiętać, że pola wejściowego nie zaktualizować ich wartości, dopóki nie utracą fokus, więc zobaczą zmiany powiązane elementy podczas pisania.

> [!NOTE]
> Dodawanie obsługi aktualizowanie na żywo po każdej keypress polega po prostu na dodawania `valueUpdate: "afterkeydown"` do `data-bind` zawartości atrybutu. To zachowanie można również uzyskać za pomocą `data-bind="textInput: authorName"` Aby pobrać aktualizacje błyskawicznych wartości. 

Nasze viewModel po zaktualizowaniu go do użycia ko.observable:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

Odcinania obsługuje wiele różnych rodzajów powiązania. Firma Microsoft do tej pory przedstawiono sposób powiązania `text` i `value`. Można także powiązać danego atrybutu. Na przykład, aby utworzyć hiperłącze z tag kotwicy `src` atrybut może być powiązana z viewModel. Odcinania obsługuje również powiązania funkcji. Aby zademonstrować, to, umożliwia zaktualizuj viewModel uwzględnienie dojścia twitter autora i wyświetlić uchwyt twitter jako łącze do strony usługi twitter autora. Firma Microsoft będzie w tym w trzech etapach.

Najpierw dodaj HTML do wyświetlenia hiperłącze, które zostanie omówiony w nawiasach po nazwisko autora:

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

Następnie zaktualizuj viewModel twitterUrl i twitterAlias właściwościami:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

Zwróć uwagę, że na tym etapie firma Microsoft nie zostały jeszcze zaktualizowane twitterUrl, aby przejść do poprawny adres URL dla tego aliasu twitter — wystarczy wskazuje na twitter.com. Także zauważyć, że firma Microsoft korzysta z nowych funkcji odcinania, `computed`, dla twitterUrl. Jest to zauważalne funkcji, który powiadomi elementów interfejsu użytkownika, jeśli zmieni się. Jednak dla niego w celu zapewnienia dostępu do innych właściwości viewModel musimy zmienić sposób tworzymy viewModel, tak, aby każda właściwość własnych instrukcji.

Poniżej przedstawiono deklaracji poprawione viewModel. Teraz jest ona zadeklarowana jako funkcja. Zauważ, że każda właściwość jest własnych instrukcji teraz, kończąc średnikiem. Ponadto, czy do uzyskania dostępu do wartości właściwości twitterAlias, należy wykonać, aby () zawiera odwołanie.

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

Wynik działa zgodnie z oczekiwaniami w przeglądarce:

![hyperlink odcinania](knockout/_static/hyperlink-screenshot.png)

Odcinania obsługuje również powiązania na określone zdarzenia element interfejsu użytkownika, takich jak zdarzenia kliknięcia. Dzięki temu można łatwo i deklaratywnie powiązać elementy interfejsu użytkownika funkcji w ramach viewModel aplikacji. Jako prosty przykład możemy dodać przycisk, po kliknięciu modyfikuje twitterAlias autora jako wersalików.

Najpierw dodamy przycisku powiązania przycisku kliknij pozycję zdarzeń i odwołuje się do nazwy funkcji, którą spróbujemy do dodania do viewModel:

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

Następnie należy dodać funkcję do viewModel i połączenie go do modyfikowania stanu viewModel. Zwróć uwagę, że aby ustawić nową wartość dla właściwości twitterAlias, możemy wywołać ją jako metodę i podaj nową wartość.

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

Wykonywanie kodu i klikając przycisk modyfikuje łącze wyświetlane zgodnie z oczekiwaniami:

![HYPERLINK pisane wielkimi literami.](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a>Przepływ sterowania

Odcinania obejmuje powiązania, które mogą wykonywać operacje warunkowe i pętli. Operacje pętli są szczególnie przydatne w przypadku powiązania listy danych interfejsu użytkownika list, menu, siatki i tabele. Powiązanie foreach będzie iteracja tablicy. W przypadku użycia z tablicą zauważalne, automatycznie aktualizuje elementy interfejsu użytkownika, gdy elementy są dodawane lub usuwane z tablicy, bez konieczności ponownego tworzenia każdego elementu w drzewie interfejsu użytkownika. W poniższym przykładzie użyto nowego viewModel, w tym tablicę zauważalne gier wyników. Jest on powiązany z prostą tabelę z kolumnami przy użyciu `foreach` wiążących `<tbody>` elementu. Każdy `<tr>` w elemencie `<tbody>` zostanie powiązany z elementem kolekcji gameResults.

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

Należy zauważyć, że teraz używamy ViewModel z litera "V" ponieważ do utworzenia go za pomocą "new" (w wywołaniu applyBindings). Podczas wykonywania strony powoduje zwrócenie następujących danych wyjściowych:

![odcinania modelu widoku rekordu](knockout/_static/record-screenshot.png)

Aby zademonstrować, czy działa zauważalne kolekcji, Dodajmy trochę więcej funkcji. Firma Microsoft obejmują możliwość zapisywania wyników gry do ViewModel, a następnie dodać przycisk i niektóre interfejsu użytkownika do pracy z tej nowej funkcji.  Najpierw utwórz metody addResult:

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

Powiązać tej metody za pomocą przycisku `click` powiązania:

```html
<button data-bind="click: addResult">Add New Result</button>
```

Otwórz stronę w przeglądarce, a następnie kliknij przycisk kilka razy, co w nowym wierszu tabeli każdego kliknięciem:

![Dodaj wyników](knockout/_static/record-addresult-screenshot.png)

Istnieje kilka sposobów umożliwiających dodawanie nowych rekordów w interfejsie użytkownika, zwykle albo wbudowanego lub postać osobnym. Firma Microsoft można łatwo zmodyfikować tabeli używanej pola tekstowe i dropdownlists, dzięki czemu wszystko można edytować. Można zmienić `<tr>` element, jak pokazano:

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

Należy pamiętać, że `$root` odwołuje się do katalogu głównego ViewModel, czyli gdzie możliwe opcje są dostępne. `$data`odnosi się do niezależnie od bieżącego modelu znajduje się w danym kontekście — w tym przypadku odwołuje się do pojedynczego elementu tablicy resultChoices, z których każdy jest prosty ciąg znaków.

Dzięki tej zmianie całej siatki będzie można edytować:

![Można edytować siatki](knockout/_static/editable-grid-screenshot.png)

Jeśli firma Microsoft nie zostały przy użyciu odcinania, firma Microsoft może osiągnąć wszystko to przy użyciu jQuery, ale najprawdopodobniej może nie być niemal tak wydajna. Odcinania śledzi powiązane dane, które odpowiadają elementów w ViewModel które elementy interfejsu użytkownika i aktualizację tylko tych elementów, które muszą być dodane, usunięte lub zaktualizowane. Wymagałoby znaczących nakładu pracy nad przy użyciu jQuery lub bezpośredniej DOM można to osiągnąć, a nawet wówczas Jeśli następnie możemy do wyświetlenia wyników agregacji (na przykład rekord / utraconych) na podstawie danych z tabeli, czy musimy ponownie pętli go i przeanalizować Elementów HTML.  Z odcinania wyświetlanie rekordu / utraconych jest proste. Firma Microsoft wykonywania obliczeń w ViewModel, sama i wyświetl ją z powiązaniem prosty tekst i `<span>`.

Do tworzenia ciągu rekordu / utraconych, możemy użyć obliczoną według. Należy pamiętać, że odwołuje się do właściwości zauważalne w ramach ViewModel musi być wywołania funkcji, w przeciwnym razie ich nie wybierze wartość według (tj. `gameResults()` nie `gameResults` w kodzie pokazanym):

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

Tej funkcji należy powiązać zakresu w ramach `<h1>` elementu w górnej części strony:

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

Wynik:

![/ Utraconych](knockout/_static/record-winloss-screenshot.png)

Dodawanie wierszy i modyfikowanie wybranego elementu w kolumnie wynik każdego wiersza spowoduje zaktualizowanie rekordu wyświetlany w górnej części okna.

Oprócz powiązania wartości, można również użyć prawie każdego prawne wyrażenia JavaScript w powiązaniu. Na przykład jeśli element interfejsu użytkownika ma być wyświetlane tylko w niektórych warunkach, np. gdy wartość przekroczy określony próg, można określić to logicznie wewnątrz wyrażenia powiązania:

```html
<div data-bind="visible: customerValue > 100"></div>
```

To `<div>` będą widoczne tylko w przypadku customerValue ponad 100.

## <a name="templates"></a>Szablony

Odcinania ma obsługę szablony, można łatwo interfejsu użytkownika z innej niż Twoje zachowanie lub przyrostowo Załaduj elementy interfejsu użytkownika do dużych aplikacji na żądanie. Aktualizujemy nasze poprzednim przykładzie, aby każdy wiersz własny szablon po prostu limit ściąganie HTML do szablonu i określanie szablonu według nazwy w wywołaniu wiązania danych na `<tbody>`.

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

Odcinania obsługuje również innych aparatów tworzenia szablonów, takiej jak biblioteka jQuery.tmpl i jego Underscore.js aparatu tworzenia szablonów.

## <a name="components"></a>Składniki

Składniki umożliwiają organizowanie i ponowne użycie kodu interfejsu użytkownika, zwykle razem z danych ViewModel, od którego zależy kodu interfejsu użytkownika. Aby utworzyć składnik, należy po prostu określić jego szablonu i jego viewModel i nadaj mu nazwę. Jest to realizowane przez wywołanie `ko.components.register()`. Oprócz Definiowanie szablonów i wbudowanego viewmodel, mogą być załadowany z zewnętrznych plików za pomocą biblioteki, takich jak *require.js*, co w kodzie bardzo czyste i wydajne.

## <a name="communicating-with-apis"></a>Podczas komunikacji z interfejsów API

Odcinania może współpracować z żadnych danych w formacie JSON. Typowym sposobem pobrania i zapisywanie danych przy użyciu odcinania jest z jQuery, która obsługuje `$.getJSON()` funkcji, aby pobrać dane i `$.post()` metody do przesyłania danych za pomocą przeglądarki do punktu końcowego interfejsu API. Oczywiście jeśli wolisz inny sposób, aby wysyłać i odbierać dane JSON odcinania zostanie z nim również działać.

## <a name="summary"></a>Podsumowanie

Odcinania zapewnia prosty, elegancki sposób powiązania elementów interfejsu użytkownika do bieżącego stanu aplikacji klienckiej, zdefiniowane w ViewModel. Składnia wiązania w odcinania używa atrybutu data-bind, stosowane do elementów HTML, które mają być przetwarzane. Odcinania jest w stanie wydajnie renderowania i zaktualizuj dużych zestawów danych, śledzenie elementów interfejsu użytkownika i przetwarza tylko zmiany w wpływ na elementy. Dużych aplikacji można podzielić logika interfejsu użytkownika przy użyciu szablonów i składników, które mogą być ładowane na żądanie z plików zewnętrznych. Obecnie w wersji 3, odcinania jest stabilna biblioteka języka JavaScript, które może poprawić aplikacji sieci web, które wymagają interakcji wzbogaconego klienta.
