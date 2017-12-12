---
title: "Przy użyciu AngularJS dla aplikacji jednej strony (źródła)"
author: rick-anderson
description: "Dowiedz się, jak utworzyć aplikację ASP.NET SPA stylu przy użyciu AngularJS"
keywords: JEDNOSTRONICOWEJ platformy ASP.NET Core AngularJS,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ccdf1625cdaf2400780500ac5ab86f41537964a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a>Przy użyciu AngularJS dla aplikacji jednej strony (źródła) z platformy ASP.NET Core


Przez [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) i [Scott Addie](https://scottaddie.com)

W tym artykule dowiesz się, jak utworzyć aplikację ASP.NET SPA stylu przy użyciu AngularJS.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-angularjs"></a>Co to jest AngularJS?

[AngularJS](https://angularjs.org/) jest nowoczesne architektury JavaScript z Google najczęściej używanych do pracy z jednej strony aplikacji (źródła). AngularJS jest otwarty powierzając jej ich konserwację MIT licencji, a postęp programowanie AngularJS może występować [repozytorium GitHub](https://github.com/angular/angular.js). Biblioteka jest nazywany kątową, ponieważ HTML używa nawiasów kątowego w kształcie.

AngularJS nie jest biblioteką manipulowania modelu DOM, takich jak jQuery, ale używa podzbiór o nazwie jQLite jQuery. AngularJS opiera się głównie na deklaratywne atrybuty HTML, które można dodać do tagów HTML. Możesz spróbować AngularJS w przeglądarce za pomocą [służbowe kodu witryny sieci Web](https://www.codeschool.com/courses/shaping-up-with-angularjs) lub [W3Schools witryny sieci Web](https://www.w3schools.com/angular/).

Ten artykuł skupia się na AngularJS, niektóre uwagi o której kątową jest nagłówkiem.

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć korzystanie z AngularJS w aplikacji platformy ASP.NET, musi ją zainstalować w ramach projektu lub Przywołaj ją z sieci dostarczania zawartości (CDN).

### <a name="installation"></a>Instalacja

Istnieje kilka sposobów, aby dodać AngularJS do aplikacji. Jeśli zaczynasz nowej aplikacji sieci web platformy ASP.NET Core w programie Visual Studio, możesz dodać AngularJS przy użyciu wbudowanych [Bower](bower.md) obsługuje. Otwórz *bower.json*i dodać wpis do `dependencies` właściwości:

<a name="angular-bower-json"></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

Po zapisaniu *bower.json* plik, kątową zostanie zainstalowany na projekt *wwwroot/lib* folderu. Ponadto, taka informacja znajdzie się w obrębie `Dependencies/Bower` folderu. Zobacz poniższy zrzut ekranu.

![Eksplorator rozwiązań z projektem AngularJS](angular/_static/angular-solution-explorer.png)

Następnie dodaj `<script>` odwołania do dołu `<body>` sekcji strony HTML lub *_Layout.cshtml* plików, jak pokazano poniżej:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

Zaleca się, że aplikacje produkcyjne wykorzystywać CDN wspólnych bibliotek, takich jak AngularJS. AngularJS można odwoływać się do jednego z kilku CDN, taką jak:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

Po uzyskaniu odwołania do *angular.js* pliku skryptu, wszystko jest gotowe do rozpoczęcia korzystania AngularJS na stronach sieci web.

## <a name="key-components"></a>Najważniejsze składniki

AngularJS zawiera szereg głównych składników, takich jak *dyrektywy*, *szablony*, *wzmacniaki*, *modułów*,  *kontrolery*, *składniki*, *router składnika* itd. Przeanalizujmy, jak te składniki współpracują, aby dodać zachowanie do stron sieci web.

### <a name="directives"></a>Dyrektyw

Używa AngularJS [dyrektywy](https://docs.angularjs.org/guide/directive) rozszerzenie z niestandardowych atrybutów i elementów HTML. Dyrektywy AngularJS są definiowane za pomocą `data-ng-*` lub `ng-*` prefiksy (`ng` jest skrót kątowego). Istnieją dwa typy dyrektywy AngularJS:

   1. **Dyrektywy pierwotnych**: tych wstępnie zdefiniowanych przez zespół kątowego i są częścią struktury AngularJS.

   2. **Niestandardowe dyrektywy**: są to dyrektywy niestandardowych, które można określić.

Jedną z pierwotnego dyrektyw używane we wszystkich aplikacjach AngularJS jest `ng-app` dyrektywa, która używa do ładowania aplikacji AngularJS. Ta dyrektywa może odnosić się do `<body>` tag lub elementu podrzędnego treści. Zobaczmy w akcji. Zakładając, że jesteś w projekcie platformy ASP.NET, można dodać do pliku HTML `wwwroot` folderze, lub Dodaj nowe akcji kontrolera i skojarzonego widoku. W takim przypadku zostały dodane nowe `Directives` metody akcji, aby `HomeController.cs`. Skojarzony widok jest następujący:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

Aby zachować te przykłady od siebie niezależne, nie używam pliku udostępnionego układu. Widoczny czy możemy dekorowane tag treść z `ng-app` dyrektywy, aby wskazać, ta strona jest aplikacją AngularJS. `{{2+2}}` Jest wyrażenie powiązania kątowego danych, które będzie więcej informacji na temat za chwilę. Po uruchomieniu tej aplikacji, w tym miejscu jest wynikiem:

![Proste dyrektywy Angular](angular/_static/simple-directive.png)

Inne dyrektywy pierwotnych w AngularJS obejmują:

`ng-controller`Określa, który kontroler JavaScript jest powiązany z widoku.

`ng-model`Określa model, z którym związane są wartości właściwości elementu HTML.

`ng-init`Używane do zainicjowania danych aplikacji w postaci wyrażenia dla bieżącego zakresu.

`ng-if`Usuwa lub odtwarza danego elementu HTML w modelu DOM, oparte na truthiness z dostarczonego wyrażenia.

`ng-repeat`Powtarza danym bloku kodu HTML dla zestawu danych.

`ng-show`Wyświetlenie lub ukrycie oparte na dostarczonego wyrażenia danego elementu HTML.

Aby uzyskać pełną listę wszystkich pierwotnych dyrektyw obsługiwane w AngularJS, zapoznaj się [sekcji dyrektywy dokumentacji w witrynie sieci Web z dokumentacją AngularJS](https://docs.angularjs.org/api/ng/directive).

### <a name="data-binding"></a>Powiązanie danych

Udostępnia AngularJS [wiązania z danymi](https://docs.angularjs.org/guide/databinding) obsługuje out-of--box za pomocą `ng-bind` dyrektywy lub danych, takich jak powiązania składni wyrażenia `{{expression}}`. AngularJS obsługuje powiązanie danych dwukierunkowe miejsca przechowywania danych z modelu podczas synchronizacji z szablonu widoku przez cały czas. Zmiany wprowadzone w widoku są automatycznie odzwierciedlane w modelu. Podobnie wszystkie zmiany w modelu są uwzględniane w widoku.

Utwórz plik HTML lub akcji kontrolera z towarzyszącym widok o nazwie `Databinding`. W widoku, są następujące:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

Powiadomienie, że można wyświetlić za pomocą powiązania dyrektywy lub dane wartości modelu (`ng-bind`). Wynikowa strona powinna wyglądać następująco:

![Proste powiązania danych](angular/_static/simple-databinding.png)

### <a name="templates"></a>Szablony

[Szablony](https://docs.angularjs.org/guide/templates) w AngularJS są tylko zwykły stron HTML ozdobione dyrektywy AngularJS i artefaktów. Szablon AngularJS jest mieszaniną dyrektywy, wyrażeń filtrów i formantów z HTML w celu utworzenia widoku.

Dodaj inny widok, aby zademonstrować szablony i dodać następujące:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

Szablon ma dyrektywy AngularJS, takich jak `ng-app`, `ng-init`, `ng-model` i składnia wyrażenia wiązania danych do powiązania `personName` właściwości. Uruchomiona w przeglądarce, widok wygląda jak na poniższym zrzucie ekranu:

![Prosty przykład szablony 1](angular/_static/simple-templates-1.png)

Jeśli zmieniasz nazwę pola, wpisując polecenie w polu wejściowym zobaczysz tekst obok pola wejściowego dynamicznie aktualizacji przedstawiający kątowego dwukierunkowe danych powiązania w akcji.

![Prosty przykład szablony 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a>Wyrażenia

[Wyrażenia](https://docs.angularjs.org/guide/expression) w AngularJS są wstawki kodu notacji języka JavaScript, które są zapisywane w `{{ expression }}` składni. Dane z tych wyrażeń jest powiązany z HTML w taki sam sposób jak `ng-bind` dyrektywy. Podstawowa różnica między AngularJS wyrażeń i wyrażeń regularnych języka JavaScript jest tym AngularJS wyrażenia są obliczane względem `$scope` obiektu w AngularJS.

Wyrażenia AngularJS w przykładzie poniżej bind `personName` i proste JavaScript obliczane wyrażenie:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

Przykład uruchomiony w przeglądarce Wyświetla `personName` danych oraz wynikiem obliczenia:

![Proste wyrażenia](angular/_static/simple-expressions.png)

### <a name="repeaters"></a>Wzmacniaki

Powtarzanie w AngularJS odbywa się za pośrednictwem pierwotnych dyrektywy o nazwie `ng-repeat`. `ng-repeat` Dyrektywy powtarza danego elementu HTML w widoku na długości tablicy identycznych danych. Wzmacniaki w AngularJS można powtarzać w tablicy ciągów lub obiektów. Oto przykładowe użycie powtórzonych na tablicę ciągów:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

[Repeat — dyrektywa](https://docs.angularjs.org/api/ng/directive/ngRepeat) generuje szereg elementów listy w nieuporządkowaną listę, jak widać w narzędziach developer pokazano tego zrzutu ekranu:

![Przykład elementu powtarzanego](angular/_static/repeater.png)

Oto przykład, który powtarza na tablicę obiektów. `ng-init` Dyrektywa określa `names` tablicy, której każdy element jest obiekt zawierający najpierw i nazwiska. `ng-repeat` Przypisania, `name in names`, danych wyjściowych elementu każdy element tablicy.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

W takim przypadku dane wyjściowe jest taka sama, jak w poprzednim przykładzie.

Kątową zawiera niektóre dodatkowe dyrektywy, które pomaga zapewnić działanie pętli w przypadku jego wykonywania.

`$index`

Użyj `$index` w `ng-repeat` pętli, aby określić indeks pozycji z pętli obecnie znajduje się na.

`$even`i`$odd`

Użyj `$even` w `ng-repeat` pętli w celu ustalenia, czy bieżącego indeksu w Twojej pętli jest nawet indeksowanego wiersza. Podobnie, użyj `$odd` do ustalenia, czy bieżący indeks jest nieparzysta indeksowanego wiersza.

`$first`i`$last`

Użyj `$first` w `ng-repeat` pętli, aby ustalić, czy bieżący indeks w pętli z pierwszego wiersza. Podobnie, użyj `$last` do ustalenia, czy bieżący indeks jest ostatni wiersz.

Poniżej znajduje się przykład pokazujący `$index`, `$even`, `$odd`, `$first`, i `$last` w akcji:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

Poniżej przedstawiono dane wyjściowe:

![Przykład elementu powtarzanego 2](angular/_static/repeaters2.png)

### <a name="scope"></a>$scope

`$scope`jest obiektem JavaScript, który działa jako sklejki między widokiem (szablonu) i kontrolera (co omówiono poniżej). Szablon widoku AngularJS zna tylko wartości dołączony do `$scope` obiektu w kontrolerze.

> [!NOTE]
> W świecie MVVM `$scope` obiektu w AngularJS często jest zdefiniowany jako ViewModel. Zespół AngularJS odwołuje się do `$scope` obiektu jako modelu danych. [Dowiedz się więcej na temat zakresów w AngularJS](https://docs.angularjs.org/guide/scope).

Poniżej przedstawiono prosty przykład pokazujący sposób ustawić właściwości `$scope` w osobnym pliku JavaScript, *scope.js*:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

Obserwować `$scope` parametr przekazany do kontrolera na wiersz 2. Ten obiekt jest widok wie o. W wierszu 3 konfigurujemy ustawienia właściwość o nazwie "name" na "Joanna Magdalena".

Co się stanie, gdy nie znaleziono określonej właściwości przez widok? Wyświetlanie określonych poniżej odwołuje się do właściwości "name" i "wiek":

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

Zwróć uwagę, w wierszu 9, że prosimy kątową, aby wyświetlić właściwości "name", używając składni wyrażenia. Następnie wiersz 10 dotyczy "wiek", właściwość, która nie istnieje. Uruchomione przykładzie nazwa ustawioną "Joanna Magdalena" i nic wieku. Brak właściwości są ignorowane.

![Przykład zakresu](angular/_static/scope.png)

### <a name="modules"></a>Moduły

A [modułu](https://docs.angularjs.org/guide/module) w AngularJS to kolekcja kontrolerów, usługi, dyrektywy itd. `angular.module()` Wywołanie funkcji służy do tworzenia, rejestrowania i pobranie modułów w AngularJS. Wszystkie moduły, w tym dostarczonym przez AngularJS zespołu i bibliotek innych firm, powinny być rejestrowane za pomocą `angular.module()` funkcji.

Poniżej przedstawiono fragment kodu, który pokazuje, jak utworzyć nowy moduł w AngularJS. Pierwszy parametr jest nazwa modułu. Drugi parametr określa zależności w innych modułach. W dalszej części tego artykułu, firma Microsoft można pokazujący sposób przekazywania te zależności, aby `angular.module()` wywołania metody.

```javascript
var personApp = angular.module('personApp', []);
```

Użyj `ng-app` dyrektywy do reprezentowania moduł AngularJS, na stronie. Używaj modułu TPM, Przypisz nazwę modułu, `personApp` w tym przykładzie do `ng-app` dyrektywy w naszym szablonu.

```html
<body ng-app="personApp">
```

### <a name="controllers"></a>Kontrolery

[Kontrolery](https://docs.angularjs.org/guide/controller) w AngularJS są pierwszy punkt wejścia dla kodu. `<module name>.controller()` Wywołanie funkcji jest używany do tworzenia i zarejestrować kontrolerów w AngularJS. `ng-controller` Dyrektywa jest używana do reprezentowania kontroler AngularJS, na stronie HTML. Rolę kontrolera kątową jest ustalenie stanu i zachowanie modelu danych (`$scope`). Nie można używać kontrolery do manipulowania w modelu DOM bezpośrednio.

Poniżej znajduje się fragment kodu, który rejestruje nowy kontroler. `personApp` Kątowego moduł, który jest zdefiniowany w wierszu 2 odwołuje się do zmiennej we fragmencie.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

W widoku przy użyciu `ng-controller` dyrektywy przypisuje nazwę kontrolera:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

Na stronie znajdują się "Maria" oraz "Magdalena", który odpowiada `firstName` i `lastName` właściwości dołączonych do `$scope` obiektu:

![Przykład kontrolera](angular/_static/controllers.png)

### <a name="components"></a>Składniki

[Składniki](https://docs.angularjs.org/guide/component) na kątową 1.5.x umożliwia hermetyzację oraz możliwość tworzenia poszczególnych elementów HTML. W 1.4.x kątowego można osiągnąć tej samej funkcji przy użyciu metody .directive().

Przy użyciu metody .component() programowanie upraszcza uzyskanie funkcji dyrektywy i kontrolera. Inne zalety; zakres izolacji, najlepsze rozwiązania są związane i migracji do 2 kątowego staje się łatwiejsze zadań. `<module name>.component()` Wywołanie funkcji jest używany do tworzenia i zarejestrować komponenty w AngularJS.

Poniżej znajduje się fragment kodu, który rejestruje nowy składnik. `personApp` Kątowego moduł, który jest zdefiniowany w wierszu 2 odwołuje się do zmiennej we fragmencie.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

Widok, w którym są wyświetlane niestandardowego elementu HTML.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

Skojarzony szablon używany przez składnik:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

Na stronie znajdują się "Aftab" i "Ansari", który odpowiada `firstName` i `lastName` właściwości dołączonych do `vm` obiektu:

![Przykład składników](angular/_static/components.png)

### <a name="services"></a>Usługi

[Usługi](https://docs.angularjs.org/guide/services) w AngularJS są często używane do udostępnionego kodu, który jest niedostępny pobieranej do pliku, którego można użyć w okresie istnienia kątowego aplikacji. Usługi opóźnieniem są tworzone, co oznacza, że nie będzie wystąpienia usługi, chyba że jest używany przez składnik zależy od usługi. Fabryki są przykładem usługi używany w aplikacjach AngularJS. Fabryki są tworzone przy użyciu `myApp.factory()` wywołania funkcji, których `myApp` jest moduł.

Poniżej znajduje się przykład przedstawia sposób użycia fabryki w AngularJS:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

Aby wywołać tę fabrykę z kontrolera, Przekaż `personFactory` jako parametr `controller` funkcji:

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a>Za pomocą usług do komunikowania się punkt końcowy REST

Poniżej przedstawiono przykład end-to-end używa usług w AngularJS wchodzić w interakcje z punktem końcowym interfejsu API platformy ASP.NET Core sieci Web. Przykład pobiera dane z interfejsu API sieci Web i wyświetla dane w szablonie widoku. Zacznijmy od widoku najpierw:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

W tym widoku mamy kątowego modułu o nazwie `PersonsApp` i wywołać kontrolera `personController`. Używamy `ng-repeat` do wykonywania iteracji listy osób. Odwołuje się trzy niestandardowe pliki JavaScript w wierszach 17-19.

*PersonApp.js* plik jest używany do rejestrowania `PersonsApp` modułu; i składnia jest podobny do poprzednich przykładach. Używamy `angular.module` funkcji do utworzenia nowego wystąpienia modułu, w którym firma Microsoft będzie działać z.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

Spójrzmy na *personFactory.js*, poniżej. Wywołania modułu `factory` metodę w celu utworzenia do ustawień fabrycznych. Wiersz 12 zawiera wbudowane kątową `$http` usługi podczas pobierania informacji osób z usługą sieci web.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

W *personController.js*, wywołania modułu `controller` metodę w celu utworzenia kontrolera. `$scope` Obiektu `people` właściwości przypisano z danymi zwróconymi z personFactory (wiersz 13).

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

Spójrzmy szybkiego interfejsu API sieci Web i modelu za nią. `Person` Model jest POCO (zwykły stary obiekt CLR) z `Id`, `FirstName`, i `LastName` właściwości:

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

`Person` Kontrolera zwraca listę formacie JSON `Person` obiektów:

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

Zobaczmy, aplikację w akcji:

![Wyświetlanie wyników REST kontrolera](angular/_static/rest-bound.png)

Możesz [wyświetlanie struktury aplikacji w witrynie GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

> [!NOTE]
> Aby uzyskać więcej informacji na temat struktury aplikacji AngularJS, zobacz [Papa Jan kątowego Przewodnik po stylu](https://github.com/johnpapa/angular-styleguide)

&nbsp;

> [!NOTE]
> Utworzyć moduł AngularJS, kontroler, fabryki, pliki dyrektywy i widoku można łatwo, sprawdź Sayed Hashimi [SideWaffle szablonu dodatkiem Service pack dla programu Visual Studio](http://sidewaffle.com/). Sayed Hashimi jest starszy Menedżer programu Visual Studio zespołu sieci Web firmy Microsoft i szablonów SideWaffle są traktowane jako standard złota. W momencie pisania tego SideWaffle jest dostępna dla programu Visual Studio 2012 2013 i 2015.

### <a name="routing-and-multiple-views"></a>Routing i wiele widoków

AngularJS ma dostawcę wbudowanych trasy do obsługi nawigacji na podstawie SPA (jednej strony aplikacji). Aby pracować z routingiem w AngularJS, należy dodać `angular-route` biblioteki za pomocą rozwiązania Bower. Można zobaczyć w [bower.json](#angular-bower-json) pliku, do których odwołuje się na początku tego artykułu, że firma Microsoft już odwołuje się on w naszym projektu.

Po zainstalowaniu pakietu, Dodaj odwołanie do skryptu (*kątowego route.js*) do widoku.

Teraz Przyjrzyjmy aplikacji osoby możemy zostały tworzenia i Dodaj do niej nawigacji. Najpierw, możemy utworzyć kopię aplikację przez utworzenie nowej `PeopleController` akcji o nazwie `Spa` i odpowiadające mu `Spa.cshtml` widoku przez skopiowanie widok Index.cshtml `People` folderu. Dodaj odwołanie do skryptu `angular-route` (patrz wiersz 11). Również dodać `div` oznaczonej jako `ng-view` — dyrektywa (patrz wiersz 6) jako symbol zastępczy umieścić widoków. Zamierzamy korzystać z kilku dodatkowych *js* pliki, które są przywoływane w wierszach 13-16.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

Spójrzmy na *personModule.js* pliku, aby zobaczyć, jak są uruchamianiu modułu routingu. Firma Microsoft przekazywane `ngRoute` jako biblioteki do modułu. Ten moduł obsługuje routing w naszej aplikacji.

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

*PersonRoutes.js* pliku poniżej definiuje trasy w oparciu o dostawcy trasy. Wiersze 4-7 definiują nawigacji skutecznie mówiąc, gdy adres URL z `/persons` jest wymagane, użyj szablonu o nazwie `partials/personlist` pracy za pośrednictwem `personListController`. Linie 8-11 wskazują strony szczegółów z parametrem trasy `personId`. Jeśli adres URL nie pasuje do jednej z wzorców, kątową domyślnie `/persons` widoku.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

`personlist.html` Plik jest zawiera tylko niezbędne do wyświetlania listy osób HTML widoku częściowego.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

Kontroler jest zdefiniowany za pomocą modułu `controller` działać w *personListController.js*.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

Jeśli firma Microsoft uruchomić tę aplikację i przejdź do `people/spa#/persons` adres URL, zostanie wyświetlone:

![Widok listy osób](angular/_static/spa-persons.png)

Jeśli firma Microsoft, przejdź do strony szczegółów, na przykład `people/spa#/persons/2`, zostanie wyświetlone widoku częściowego szczegółów:

![Widok szczegółów osoby](angular/_static/spa-persons-2.png)

Można wyświetlić pełną źródła i wszystkie pliki, które nie są wyświetlane w tym artykule, na [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

### <a name="event-handlers"></a>Programy obsługi zdarzeń

Istnieje wiele dyrektyw w AngularJS, która dodanie funkcji obsługi zdarzeń do elementów wejściowych w Twojej HTML modelu DOM. Poniżej znajduje się lista zdarzeń, które są wbudowane w AngularJS.

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> Można dodać własne programy obsługi zdarzeń za pomocą [dyrektywy niestandardowej funkcji w AngularJS](https://docs.angularjs.org/guide/directive).

Zobaczmy, jak `ng-click` przewodowej zdarzeń w górę. Utwórz nowy plik JavaScript o nazwie *eventHandlerController.js*i Dodaj do niej następujące:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

Zwróć uwagę, nowe `sayName` działać w `eventHandlerController` w wierszu 5 powyżej. Wykonuje wszystkie metody dla teraz jest przedstawiający ostrzeżenie JavaScript do użytkownika wiadomość powitalna.

Poniższy widok wiąże funkcja kontroler AngularJS zdarzenia. Wiersz 9 zawiera przycisk, na którym `ng-click` dyrektywy Angular zostały zastosowane. Wywołuje naszych `sayName` funkcji, która jest dołączona do `$scope` obiekt przekazany do tego widoku.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

Przykład uruchomionych wykaże, że kontrolera `sayName` funkcja jest wywoływana automatycznie, gdy przycisk zostanie kliknięty.

![Zdarzenie kliknięcia](angular/_static/events.png)

Aby uzyskać więcej szczegółów na dyrektywy programu obsługi zdarzeń wbudowanych AngularJS, upewnij się, head [witryny sieci Web w dokumentacji](https://docs.angularjs.org/api/ng/directive/ngClick) z AngularJS.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja dyrektywy angular](https://docs.angularjs.org)

* [Info 2 dyrektywy angular](https://angular.io/)
