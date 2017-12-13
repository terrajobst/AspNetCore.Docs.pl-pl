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
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a><span data-ttu-id="e0f49-103">Struktura Knockout.js MVVM w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0f49-103">Knockout.js MVVM Framework in ASP.NET Core</span></span>

<span data-ttu-id="e0f49-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e0f49-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e0f49-105">Knockout jest popularnych biblioteka języka JavaScript, które ułatwia tworzenie interfejsów złożonych oparte na danych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e0f49-105">Knockout is a popular JavaScript library that simplifies the creation of complex data-based user interfaces.</span></span> <span data-ttu-id="e0f49-106">Można go samodzielnie lub z innych bibliotek, np. jQuery.</span><span class="sxs-lookup"><span data-stu-id="e0f49-106">It can be used alone or with other libraries, such as jQuery.</span></span> <span data-ttu-id="e0f49-107">Jego podstawowym celem jest powiązać odpowiedni model danych zdefiniowany jako obiekt JavaScript elementy interfejsu użytkownika tak, aby podczas wprowadzania zmian do interfejsu użytkownika, model jest aktualizowany i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="e0f49-107">Its primary purpose is to bind UI elements to an underlying data model defined as a JavaScript object, such that when changes are made to the UI, the model is updated, and vice versa.</span></span> <span data-ttu-id="e0f49-108">Odcinania ułatwia korzystanie ze wzorca Model-View-ViewModel (MVVM) w aplikacji sieci web po stronie klienta zachowanie.</span><span class="sxs-lookup"><span data-stu-id="e0f49-108">Knockout facilitates the use of a Model-View-ViewModel (MVVM) pattern in a web application's client-side behavior.</span></span> <span data-ttu-id="e0f49-109">Dwa główne pojęcia, które jedną muszą dowiedzieć się, podczas pracy z implementacją MVVM odcinania w są dostrzegalne elementy i powiązania.</span><span class="sxs-lookup"><span data-stu-id="e0f49-109">The two main concepts one must learn when working with Knockout's MVVM implementation are Observables and Bindings.</span></span>

## <a name="getting-started"></a><span data-ttu-id="e0f49-110">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="e0f49-110">Getting started</span></span>

<span data-ttu-id="e0f49-111">Knockout jest wdrożona jako pojedynczy plik JavaScript, więc instalowania i korzystania z niego jest bardzo prosta przy użyciu [bower](bower.md).</span><span class="sxs-lookup"><span data-stu-id="e0f49-111">Knockout is deployed as a single JavaScript file, so installing and using it is very straightforward using [bower](bower.md).</span></span> <span data-ttu-id="e0f49-112">Zakładając, że masz już [bower](bower.md) i [system gulp](using-gulp.md) skonfigurowane, otwórz *bower.json* w Twojej platformy ASP.NET Core projektu i Dodawanie zależności odcinania, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="e0f49-112">Assuming you already have [bower](bower.md) and [gulp](using-gulp.md) configured, open *bower.json* in your ASP.NET Core project and add the knockout dependency as shown here:</span></span>

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

<span data-ttu-id="e0f49-113">Z tym w miejscu mogą być następnie ręcznie uruchomić bower, otwierając Eksploratora modułu uruchamiającego zadania (w widoku ‣ ‣ inne okna Eksploratora modułu uruchamiającego zadania), a następnie w obszarze zadania kliknij prawym przyciskiem myszy bower i wybierz polecenie Uruchom.</span><span class="sxs-lookup"><span data-stu-id="e0f49-113">With this in place, you can then manually run bower by opening the Task Runner Explorer (under View ‣ Other Windows ‣ Task Runner Explorer) and then under Tasks, right-click on bower and select Run.</span></span> <span data-ttu-id="e0f49-114">Wynik powinien być podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="e0f49-114">The result should appear similar to this:</span></span>

![bower odcinania uruchomionej w Eksploratora modułu uruchamiającego zadania](knockout/_static/bower-knockout.png)

<span data-ttu-id="e0f49-116">Teraz można spojrzeć na projekt `wwwroot` folderu, powinny pojawić się odcinania zainstalowany w folderze lib.</span><span class="sxs-lookup"><span data-stu-id="e0f49-116">Now if you look in your project's `wwwroot` folder, you should see knockout installed under the lib folder.</span></span>

![odcinania zainstalowany w folderze lib](knockout/_static/wwwroot-knockout.png)

<span data-ttu-id="e0f49-118">Zaleca się w środowisku produkcyjnym odwoływania odcinania za pośrednictwem sieci dostarczania zawartości lub CDN, ponieważ zwiększa prawdopodobieństwo, że użytkownicy mają już pamięci podręcznej kopię pliku i w związku z tym nie należy pobrać go w ogóle.</span><span class="sxs-lookup"><span data-stu-id="e0f49-118">It's recommended that in your production environment you reference knockout via a Content Delivery Network, or CDN, as this increases the likelihood that your users will already have a cached copy of the file and thus will not need to download it at all.</span></span> <span data-ttu-id="e0f49-119">Knockout jest dostępna w kilku CDN, w tym Microsoft Ajax CDN, w tym miejscu:</span><span class="sxs-lookup"><span data-stu-id="e0f49-119">Knockout is available on several CDNs, including the Microsoft Ajax CDN, here:</span></span>

[<span data-ttu-id="e0f49-120">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="e0f49-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

<span data-ttu-id="e0f49-121">Aby uwzględnić odcinania na stronie, który zostanie użyty, po prostu Dodaj `<script>` element odwołuje się do pliku z wszędzie tam, gdzie można będzie obsługiwać go (z aplikacją lub za pośrednictwem CDN):</span><span class="sxs-lookup"><span data-stu-id="e0f49-121">To include Knockout on a page that will use it, simply add a `<script>` element referencing the file from wherever you will be hosting it (with your application, or via a CDN):</span></span>

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a><span data-ttu-id="e0f49-122">Dostrzegalne elementy, ViewModels i proste powiązanie</span><span class="sxs-lookup"><span data-stu-id="e0f49-122">Observables, ViewModels, and simple binding</span></span>

<span data-ttu-id="e0f49-123">Mogą być już znasz przy użyciu języka JavaScript do modyfikowania elementów na stronie sieci web za pomocą bezpośredniego dostępu do modelu DOM lub biblioteki, takich jak jQuery.</span><span class="sxs-lookup"><span data-stu-id="e0f49-123">You may already be familiar with using JavaScript to manipulate elements on a web page, either via direct access to the DOM or using a library like jQuery.</span></span> <span data-ttu-id="e0f49-124">Zwykle zachowanie tego typu uzyskuje się poprzez pisanie kodu bezpośrednio ustawić wartości elementów w odpowiedzi na pewne akcje użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e0f49-124">Typically this kind of behavior is achieved by writing code to directly set element values in response to certain user actions.</span></span> <span data-ttu-id="e0f49-125">W przypadku odcinania deklaratywne podejście jest pobierana, za pomocą którego elementy na stronie są powiązane z właściwości obiektu.</span><span class="sxs-lookup"><span data-stu-id="e0f49-125">With Knockout, a declarative approach is taken instead, through which elements on the page are bound to properties on an object.</span></span> <span data-ttu-id="e0f49-126">Zamiast pisania kodu do manipulowania elementy modelu DOM, akcje użytkownika, po prostu interakcji z obiektu ViewModel i odcinania odpowiada on za zapewnienie, że elementy na stronie są synchronizowane.</span><span class="sxs-lookup"><span data-stu-id="e0f49-126">Instead of writing code to manipulate DOM elements, user actions simply interact with the ViewModel object, and Knockout takes care of ensuring the page elements are synchronized.</span></span>

<span data-ttu-id="e0f49-127">Jako przykład prostego należy wziąć pod uwagę na poniższej liście strony.</span><span class="sxs-lookup"><span data-stu-id="e0f49-127">As a simple example, consider the page list below.</span></span> <span data-ttu-id="e0f49-128">Obejmuje on `<span>` element z `data-bind` atrybut wskazujący, że NazwaAutora powinna być powiązana zawartości tekstowej.</span><span class="sxs-lookup"><span data-stu-id="e0f49-128">It includes a `<span>` element with a `data-bind` attribute indicating that the text content should be bound to authorName.</span></span> <span data-ttu-id="e0f49-129">Następnie w bloku kodu JavaScript viewModel zmiennej jest zdefiniowana z tylko jedną właściwość `authorName`, niektóre wartość.</span><span class="sxs-lookup"><span data-stu-id="e0f49-129">Next, in a JavaScript block a variable viewModel is defined with a single property, `authorName`, set to some value.</span></span> <span data-ttu-id="e0f49-130">Na koniec wywołania `ko.applyBindings` następuje, przekazując tę zmienną viewModel.</span><span class="sxs-lookup"><span data-stu-id="e0f49-130">Finally, a call to `ko.applyBindings` is made, passing in this viewModel variable.</span></span>

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

<span data-ttu-id="e0f49-131">Podczas wyświetlania w przeglądarce, zawartość <span> zastępuje element o wartości w zmiennej viewModel:</span><span class="sxs-lookup"><span data-stu-id="e0f49-131">When viewed in the browser, the content of the <span> element is replaced with the value in the viewModel variable:</span></span>

![proste powiązanie odcinania](knockout/_static/simple-binding-screenshot.png)

<span data-ttu-id="e0f49-133">Mamy teraz pracy proste powiązania jednokierunkowe.</span><span class="sxs-lookup"><span data-stu-id="e0f49-133">We now have simple one-way binding working.</span></span> <span data-ttu-id="e0f49-134">Zwróć uwagę, że daleko w kodzie została możemy zapisu JavaScript można przypisać wartości do zawartości zakresu.</span><span class="sxs-lookup"><span data-stu-id="e0f49-134">Notice that nowhere in the code did we write JavaScript to assign a value to the span's contents.</span></span> <span data-ttu-id="e0f49-135">Jeśli chcemy manipulowania ViewModel możemy podejmowanie ten krok i Dodaj pole tekstowe do wprowadzania HTML i powiązać jego wartość tak samo, jak tak:</span><span class="sxs-lookup"><span data-stu-id="e0f49-135">If we want to manipulate the ViewModel, we can take this a step further and add an HTML input textbox, and bind to its value, like so:</span></span>

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

<span data-ttu-id="e0f49-136">Ponowne ładowanie strony, widzimy, że ta wartość jest w rzeczywistości powiązane z polem wejściowych:</span><span class="sxs-lookup"><span data-stu-id="e0f49-136">Reloading the page, we see that this value is indeed bound to the input box:</span></span>

![powiązania wejściowego odcinania](knockout/_static/input-binding-screenshot.png)

<span data-ttu-id="e0f49-138">Jednak zmiana wartość w polu tekstowym, odpowiednie wartości w `<span>` elementu nie ulega zmianie.</span><span class="sxs-lookup"><span data-stu-id="e0f49-138">However, if we change the value in the textbox, the corresponding value in the `<span>` element doesn't change.</span></span> <span data-ttu-id="e0f49-139">Dlaczego nie?</span><span class="sxs-lookup"><span data-stu-id="e0f49-139">Why not?</span></span>

<span data-ttu-id="e0f49-140">Problem polega na niczego powiadomienie `<span>` wymaganej w aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="e0f49-140">The issue is that nothing notified the `<span>` that it needed to be updated.</span></span> <span data-ttu-id="e0f49-141">Po prostu aktualizowanie ViewModel nie zostanie wystarczające, chyba że ViewModel właściwości są ujęte w specjalny typ.</span><span class="sxs-lookup"><span data-stu-id="e0f49-141">Simply updating the ViewModel isn't by itself sufficient, unless the ViewModel's properties are wrapped in a special type.</span></span> <span data-ttu-id="e0f49-142">Należy użyć **dostrzegalne elementy** w ViewModel na wszystkie właściwości, które wymagają zmiany automatycznie aktualizowane w miarę ich występowania.</span><span class="sxs-lookup"><span data-stu-id="e0f49-142">We need to use **observables** in the ViewModel for any properties that need to have changes automatically updated as they occur.</span></span> <span data-ttu-id="e0f49-143">Zmieniając ViewModel do użycia `ko.observable("value")` zamiast tylko "value", ViewModel spowoduje zaktualizowanie elementów HTML, powiązane z jego wartości w każdym przypadku, gdy nastąpi zmiana.</span><span class="sxs-lookup"><span data-stu-id="e0f49-143">By changing the ViewModel to use `ko.observable("value")` instead of just "value", the ViewModel will update any HTML elements that are bound to its value whenever a change occurs.</span></span> <span data-ttu-id="e0f49-144">Należy pamiętać, że pola wejściowego nie zaktualizować ich wartości, dopóki nie utracą fokus, więc zobaczą zmiany powiązane elementy podczas pisania.</span><span class="sxs-lookup"><span data-stu-id="e0f49-144">Note that input boxes don't update their value until they lose focus, so you won't see changes to bound elements as you type.</span></span>

> [!NOTE]
> <span data-ttu-id="e0f49-145">Dodawanie obsługi aktualizowanie na żywo po każdej keypress polega po prostu na dodawania `valueUpdate: "afterkeydown"` do `data-bind` zawartości atrybutu.</span><span class="sxs-lookup"><span data-stu-id="e0f49-145">Adding support for live updating after each keypress is simply a matter of adding `valueUpdate: "afterkeydown"` to the `data-bind` attribute's contents.</span></span> <span data-ttu-id="e0f49-146">To zachowanie można również uzyskać za pomocą `data-bind="textInput: authorName"` Aby pobrać aktualizacje błyskawicznych wartości.</span><span class="sxs-lookup"><span data-stu-id="e0f49-146">You can also get this behavior by using `data-bind="textInput: authorName"` to get instant updates of values.</span></span> 

<span data-ttu-id="e0f49-147">Nasze viewModel po zaktualizowaniu go do użycia ko.observable:</span><span class="sxs-lookup"><span data-stu-id="e0f49-147">Our viewModel, after updating it to use ko.observable:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="e0f49-148">Odcinania obsługuje wiele różnych rodzajów powiązania.</span><span class="sxs-lookup"><span data-stu-id="e0f49-148">Knockout supports a number of different kinds of bindings.</span></span> <span data-ttu-id="e0f49-149">Firma Microsoft do tej pory przedstawiono sposób powiązania `text` i `value`.</span><span class="sxs-lookup"><span data-stu-id="e0f49-149">So far we've seen how to bind to `text` and to `value`.</span></span> <span data-ttu-id="e0f49-150">Można także powiązać danego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="e0f49-150">You can also bind to any given attribute.</span></span> <span data-ttu-id="e0f49-151">Na przykład, aby utworzyć hiperłącze z tag kotwicy `src` atrybut może być powiązana z viewModel.</span><span class="sxs-lookup"><span data-stu-id="e0f49-151">For instance, to create a hyperlink with an anchor tag, the `src` attribute can be bound to the viewModel.</span></span> <span data-ttu-id="e0f49-152">Odcinania obsługuje również powiązania funkcji.</span><span class="sxs-lookup"><span data-stu-id="e0f49-152">Knockout also supports binding to functions.</span></span> <span data-ttu-id="e0f49-153">Aby zademonstrować, to, umożliwia zaktualizuj viewModel uwzględnienie dojścia twitter autora i wyświetlić uchwyt twitter jako łącze do strony usługi twitter autora.</span><span class="sxs-lookup"><span data-stu-id="e0f49-153">To demonstrate this, let's update the viewModel to include the author's twitter handle, and display the twitter handle as a link to the author's twitter page.</span></span> <span data-ttu-id="e0f49-154">Firma Microsoft będzie w tym w trzech etapach.</span><span class="sxs-lookup"><span data-stu-id="e0f49-154">We'll do this in three stages.</span></span>

<span data-ttu-id="e0f49-155">Najpierw dodaj HTML do wyświetlenia hiperłącze, które zostanie omówiony w nawiasach po nazwisko autora:</span><span class="sxs-lookup"><span data-stu-id="e0f49-155">First, add the HTML to display the hyperlink, which we'll show in parentheses after the author's name:</span></span>

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

<span data-ttu-id="e0f49-156">Następnie zaktualizuj viewModel twitterUrl i twitterAlias właściwościami:</span><span class="sxs-lookup"><span data-stu-id="e0f49-156">Next, update the viewModel to include the twitterUrl and twitterAlias properties:</span></span>

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

<span data-ttu-id="e0f49-157">Zwróć uwagę, że na tym etapie firma Microsoft nie zostały jeszcze zaktualizowane twitterUrl, aby przejść do poprawny adres URL dla tego aliasu twitter — wystarczy wskazuje na twitter.com. Także zauważyć, że firma Microsoft korzysta z nowych funkcji odcinania, `computed`, dla twitterUrl.</span><span class="sxs-lookup"><span data-stu-id="e0f49-157">Notice that at this point we haven't yet updated the twitterUrl to go to the correct URL for this twitter alias – it's just pointing at twitter.com. Also notice that we're using a new Knockout function, `computed`, for twitterUrl.</span></span> <span data-ttu-id="e0f49-158">Jest to zauważalne funkcji, który powiadomi elementów interfejsu użytkownika, jeśli zmieni się.</span><span class="sxs-lookup"><span data-stu-id="e0f49-158">This is an observable function that will notify any UI elements if it changes.</span></span> <span data-ttu-id="e0f49-159">Jednak dla niego w celu zapewnienia dostępu do innych właściwości viewModel musimy zmienić sposób tworzymy viewModel, tak, aby każda właściwość własnych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="e0f49-159">However, for it to have access to other properties in the viewModel, we need to change how we are creating the viewModel, so that each property is its own statement.</span></span>

<span data-ttu-id="e0f49-160">Poniżej przedstawiono deklaracji poprawione viewModel.</span><span class="sxs-lookup"><span data-stu-id="e0f49-160">The revised viewModel declaration is shown below.</span></span> <span data-ttu-id="e0f49-161">Teraz jest ona zadeklarowana jako funkcja.</span><span class="sxs-lookup"><span data-stu-id="e0f49-161">It is now declared as a function.</span></span> <span data-ttu-id="e0f49-162">Zauważ, że każda właściwość jest własnych instrukcji teraz, kończąc średnikiem.</span><span class="sxs-lookup"><span data-stu-id="e0f49-162">Notice that each property is its own statement now, ending with a semicolon.</span></span> <span data-ttu-id="e0f49-163">Ponadto, czy do uzyskania dostępu do wartości właściwości twitterAlias, należy wykonać, aby () zawiera odwołanie.</span><span class="sxs-lookup"><span data-stu-id="e0f49-163">Also notice that to access the twitterAlias property value, we need to execute it, so its reference includes ().</span></span>

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

<span data-ttu-id="e0f49-164">Wynik działa zgodnie z oczekiwaniami w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="e0f49-164">The result works as expected in the browser:</span></span>

![hyperlink odcinania](knockout/_static/hyperlink-screenshot.png)

<span data-ttu-id="e0f49-166">Odcinania obsługuje również powiązania na określone zdarzenia element interfejsu użytkownika, takich jak zdarzenia kliknięcia.</span><span class="sxs-lookup"><span data-stu-id="e0f49-166">Knockout also supports binding to certain UI element events, such as the click event.</span></span> <span data-ttu-id="e0f49-167">Dzięki temu można łatwo i deklaratywnie powiązać elementy interfejsu użytkownika funkcji w ramach viewModel aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e0f49-167">This allows you to easily and declaratively bind UI elements to functions within the application's viewModel.</span></span> <span data-ttu-id="e0f49-168">Jako prosty przykład możemy dodać przycisk, po kliknięciu modyfikuje twitterAlias autora jako wersalików.</span><span class="sxs-lookup"><span data-stu-id="e0f49-168">As a simple example, we can add a button that, when clicked, modifies the author's twitterAlias to be all caps.</span></span>

<span data-ttu-id="e0f49-169">Najpierw dodamy przycisku powiązania przycisku kliknij pozycję zdarzeń i odwołuje się do nazwy funkcji, którą spróbujemy do dodania do viewModel:</span><span class="sxs-lookup"><span data-stu-id="e0f49-169">First, we add the button, binding to the button's click event, and referencing the function name we're going to add to the viewModel:</span></span>

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

<span data-ttu-id="e0f49-170">Następnie należy dodać funkcję do viewModel i połączenie go do modyfikowania stanu viewModel.</span><span class="sxs-lookup"><span data-stu-id="e0f49-170">Then, add the function to the viewModel, and wire it up to modify the viewModel's state.</span></span> <span data-ttu-id="e0f49-171">Zwróć uwagę, że aby ustawić nową wartość dla właściwości twitterAlias, możemy wywołać ją jako metodę i podaj nową wartość.</span><span class="sxs-lookup"><span data-stu-id="e0f49-171">Notice that to set a new value to the twitterAlias property, we call it as a method and pass in the new value.</span></span>

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

<span data-ttu-id="e0f49-172">Wykonywanie kodu i klikając przycisk modyfikuje łącze wyświetlane zgodnie z oczekiwaniami:</span><span class="sxs-lookup"><span data-stu-id="e0f49-172">Running the code and clicking the button modifies the displayed link as expected:</span></span>

![HYPERLINK pisane wielkimi literami.](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a><span data-ttu-id="e0f49-174">Przepływ sterowania</span><span class="sxs-lookup"><span data-stu-id="e0f49-174">Control flow</span></span>

<span data-ttu-id="e0f49-175">Odcinania obejmuje powiązania, które mogą wykonywać operacje warunkowe i pętli.</span><span class="sxs-lookup"><span data-stu-id="e0f49-175">Knockout includes bindings that can perform conditional and looping operations.</span></span> <span data-ttu-id="e0f49-176">Operacje pętli są szczególnie przydatne w przypadku powiązania listy danych interfejsu użytkownika list, menu, siatki i tabele.</span><span class="sxs-lookup"><span data-stu-id="e0f49-176">Looping operations are especially useful for binding lists of data to UI lists, menus, and grids or tables.</span></span> <span data-ttu-id="e0f49-177">Powiązanie foreach będzie iteracja tablicy.</span><span class="sxs-lookup"><span data-stu-id="e0f49-177">The foreach binding will iterate over an array.</span></span> <span data-ttu-id="e0f49-178">W przypadku użycia z tablicą zauważalne, automatycznie aktualizuje elementy interfejsu użytkownika, gdy elementy są dodawane lub usuwane z tablicy, bez konieczności ponownego tworzenia każdego elementu w drzewie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e0f49-178">When used with an observable array, it will automatically update the UI elements when items are added or removed from the array, without re-creating every element in the UI tree.</span></span> <span data-ttu-id="e0f49-179">W poniższym przykładzie użyto nowego viewModel, w tym tablicę zauważalne gier wyników.</span><span class="sxs-lookup"><span data-stu-id="e0f49-179">The following example uses a new viewModel which includes an observable array of game results.</span></span> <span data-ttu-id="e0f49-180">Jest on powiązany z prostą tabelę z kolumnami przy użyciu `foreach` wiążących `<tbody>` elementu.</span><span class="sxs-lookup"><span data-stu-id="e0f49-180">It is bound to a simple table with two columns using a `foreach` binding on the `<tbody>` element.</span></span> <span data-ttu-id="e0f49-181">Każdy `<tr>` w elemencie `<tbody>` zostanie powiązany z elementem kolekcji gameResults.</span><span class="sxs-lookup"><span data-stu-id="e0f49-181">Each `<tr>` element within `<tbody>` will be bound to an element of the gameResults collection.</span></span>

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

<span data-ttu-id="e0f49-182">Należy zauważyć, że teraz używamy ViewModel z litera "V" ponieważ do utworzenia go za pomocą "new" (w wywołaniu applyBindings).</span><span class="sxs-lookup"><span data-stu-id="e0f49-182">Notice that this time we're using ViewModel with a capital “V" because we expect to construct it using “new" (in the applyBindings call).</span></span> <span data-ttu-id="e0f49-183">Podczas wykonywania strony powoduje zwrócenie następujących danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="e0f49-183">When executed, the page results in the following output:</span></span>

![odcinania modelu widoku rekordu](knockout/_static/record-screenshot.png)

<span data-ttu-id="e0f49-185">Aby zademonstrować, czy działa zauważalne kolekcji, Dodajmy trochę więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="e0f49-185">To demonstrate that the observable collection is working, let's add a bit more functionality.</span></span> <span data-ttu-id="e0f49-186">Firma Microsoft obejmują możliwość zapisywania wyników gry do ViewModel, a następnie dodać przycisk i niektóre interfejsu użytkownika do pracy z tej nowej funkcji.</span><span class="sxs-lookup"><span data-stu-id="e0f49-186">We can include the ability to record the results of another game to the ViewModel, and then add a button and some UI to work with this new function.</span></span>  <span data-ttu-id="e0f49-187">Najpierw utwórz metody addResult:</span><span class="sxs-lookup"><span data-stu-id="e0f49-187">First, let's create the addResult method:</span></span>

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

<span data-ttu-id="e0f49-188">Powiązać tej metody za pomocą przycisku `click` powiązania:</span><span class="sxs-lookup"><span data-stu-id="e0f49-188">Bind this method to a button using the `click` binding:</span></span>

```html
<button data-bind="click: addResult">Add New Result</button>
```

<span data-ttu-id="e0f49-189">Otwórz stronę w przeglądarce, a następnie kliknij przycisk kilka razy, co w nowym wierszu tabeli każdego kliknięciem:</span><span class="sxs-lookup"><span data-stu-id="e0f49-189">Open the page in the browser and click the button a couple of times, resulting in a new table row with each click:</span></span>

![Dodaj wyników](knockout/_static/record-addresult-screenshot.png)

<span data-ttu-id="e0f49-191">Istnieje kilka sposobów umożliwiających dodawanie nowych rekordów w interfejsie użytkownika, zwykle albo wbudowanego lub postać osobnym.</span><span class="sxs-lookup"><span data-stu-id="e0f49-191">There are a few ways to support adding new records in the UI, typically either inline or in a separate form.</span></span> <span data-ttu-id="e0f49-192">Firma Microsoft można łatwo zmodyfikować tabeli używanej pola tekstowe i dropdownlists, dzięki czemu wszystko można edytować.</span><span class="sxs-lookup"><span data-stu-id="e0f49-192">We can easily modify the table to use textboxes and dropdownlists so that the whole thing is editable.</span></span> <span data-ttu-id="e0f49-193">Można zmienić `<tr>` element, jak pokazano:</span><span class="sxs-lookup"><span data-stu-id="e0f49-193">Just change the `<tr>` element as shown:</span></span>

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

<span data-ttu-id="e0f49-194">Należy pamiętać, że `$root` odwołuje się do katalogu głównego ViewModel, czyli gdzie możliwe opcje są dostępne.</span><span class="sxs-lookup"><span data-stu-id="e0f49-194">Note that `$root` refers to the root ViewModel, which is where the possible choices are exposed.</span></span> <span data-ttu-id="e0f49-195">`$data`odnosi się do niezależnie od bieżącego modelu znajduje się w danym kontekście — w tym przypadku odwołuje się do pojedynczego elementu tablicy resultChoices, z których każdy jest prosty ciąg znaków.</span><span class="sxs-lookup"><span data-stu-id="e0f49-195">`$data` refers to whatever the current model is within a given context - in this case it refers to an individual element of the resultChoices array, each of which is a simple string.</span></span>

<span data-ttu-id="e0f49-196">Dzięki tej zmianie całej siatki będzie można edytować:</span><span class="sxs-lookup"><span data-stu-id="e0f49-196">With this change, the entire grid becomes editable:</span></span>

![Można edytować siatki](knockout/_static/editable-grid-screenshot.png)

<span data-ttu-id="e0f49-198">Jeśli firma Microsoft nie zostały przy użyciu odcinania, firma Microsoft może osiągnąć wszystko to przy użyciu jQuery, ale najprawdopodobniej może nie być niemal tak wydajna.</span><span class="sxs-lookup"><span data-stu-id="e0f49-198">If we weren't using Knockout, we could achieve all of this using jQuery, but most likely it would not be nearly as efficient.</span></span> <span data-ttu-id="e0f49-199">Odcinania śledzi powiązane dane, które odpowiadają elementów w ViewModel które elementy interfejsu użytkownika i aktualizację tylko tych elementów, które muszą być dodane, usunięte lub zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="e0f49-199">Knockout tracks which bound data items in the ViewModel correspond to which UI elements, and only updates those elements that need to be added, removed, or updated.</span></span> <span data-ttu-id="e0f49-200">Wymagałoby znaczących nakładu pracy nad przy użyciu jQuery lub bezpośredniej DOM można to osiągnąć, a nawet wówczas Jeśli następnie możemy do wyświetlenia wyników agregacji (na przykład rekord / utraconych) na podstawie danych z tabeli, czy musimy ponownie pętli go i przeanalizować Elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="e0f49-200">It would take significant effort to achieve this ourselves using jQuery or direct DOM manipulation, and even then if we then wanted to display aggregate results (such as a win-loss record) based on the table's data, we would need to once more loop through it and parse the HTML elements.</span></span>  <span data-ttu-id="e0f49-201">Z odcinania wyświetlanie rekordu / utraconych jest proste.</span><span class="sxs-lookup"><span data-stu-id="e0f49-201">With Knockout, displaying the win-loss record is trivial.</span></span> <span data-ttu-id="e0f49-202">Firma Microsoft wykonywania obliczeń w ViewModel, sama i wyświetl ją z powiązaniem prosty tekst i `<span>`.</span><span class="sxs-lookup"><span data-stu-id="e0f49-202">We can perform the calculations within the ViewModel itself, and then display it with a simple text binding and a `<span>`.</span></span>

<span data-ttu-id="e0f49-203">Do tworzenia ciągu rekordu / utraconych, możemy użyć obliczoną według.</span><span class="sxs-lookup"><span data-stu-id="e0f49-203">To build the win-loss record string, we can use a computed observable.</span></span> <span data-ttu-id="e0f49-204">Należy pamiętać, że odwołuje się do właściwości zauważalne w ramach ViewModel musi być wywołania funkcji, w przeciwnym razie ich nie wybierze wartość według (tj. `gameResults()` nie `gameResults` w kodzie pokazanym):</span><span class="sxs-lookup"><span data-stu-id="e0f49-204">Note that references to observable properties within the ViewModel must be function calls, otherwise they will not retrieve the value of the observable (i.e. `gameResults()` not `gameResults` in the code shown):</span></span>

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

<span data-ttu-id="e0f49-205">Tej funkcji należy powiązać zakresu w ramach `<h1>` elementu w górnej części strony:</span><span class="sxs-lookup"><span data-stu-id="e0f49-205">Bind this function to a span within the `<h1>` element at the top of the page:</span></span>

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

<span data-ttu-id="e0f49-206">Wynik:</span><span class="sxs-lookup"><span data-stu-id="e0f49-206">The result:</span></span>

![/ Utraconych](knockout/_static/record-winloss-screenshot.png)

<span data-ttu-id="e0f49-208">Dodawanie wierszy i modyfikowanie wybranego elementu w kolumnie wynik każdego wiersza spowoduje zaktualizowanie rekordu wyświetlany w górnej części okna.</span><span class="sxs-lookup"><span data-stu-id="e0f49-208">Adding rows or modifying the selected element in any row's Result column will update the record shown at the top of the window.</span></span>

<span data-ttu-id="e0f49-209">Oprócz powiązania wartości, można również użyć prawie każdego prawne wyrażenia JavaScript w powiązaniu.</span><span class="sxs-lookup"><span data-stu-id="e0f49-209">In addition to binding to values, you can also use almost any legal JavaScript expression within a binding.</span></span> <span data-ttu-id="e0f49-210">Na przykład jeśli element interfejsu użytkownika ma być wyświetlane tylko w niektórych warunkach, np. gdy wartość przekroczy określony próg, można określić to logicznie wewnątrz wyrażenia powiązania:</span><span class="sxs-lookup"><span data-stu-id="e0f49-210">For example, if a UI element should only appear under certain conditions, such as when a value exceeds a certain threshold, you can specify this logically within the binding expression:</span></span>

```html
<div data-bind="visible: customerValue > 100"></div>
```

<span data-ttu-id="e0f49-211">To `<div>` będą widoczne tylko w przypadku customerValue ponad 100.</span><span class="sxs-lookup"><span data-stu-id="e0f49-211">This `<div>` will only be visible when the customerValue is over 100.</span></span>

## <a name="templates"></a><span data-ttu-id="e0f49-212">Szablony</span><span class="sxs-lookup"><span data-stu-id="e0f49-212">Templates</span></span>

<span data-ttu-id="e0f49-213">Odcinania ma obsługę szablony, można łatwo interfejsu użytkownika z innej niż Twoje zachowanie lub przyrostowo Załaduj elementy interfejsu użytkownika do dużych aplikacji na żądanie.</span><span class="sxs-lookup"><span data-stu-id="e0f49-213">Knockout has support for templates, so that you can easily separate your UI from your behavior, or incrementally load UI elements into a large application on demand.</span></span> <span data-ttu-id="e0f49-214">Aktualizujemy nasze poprzednim przykładzie, aby każdy wiersz własny szablon po prostu limit ściąganie HTML do szablonu i określanie szablonu według nazwy w wywołaniu wiązania danych na `<tbody>`.</span><span class="sxs-lookup"><span data-stu-id="e0f49-214">We can update our previous example to make each row its own template by simply pulling the HTML out into a template and specifying the template by name in the data-bind call on `<tbody>`.</span></span>

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

<span data-ttu-id="e0f49-215">Odcinania obsługuje również innych aparatów tworzenia szablonów, takiej jak biblioteka jQuery.tmpl i jego Underscore.js aparatu tworzenia szablonów.</span><span class="sxs-lookup"><span data-stu-id="e0f49-215">Knockout also supports other templating engines, such as the jQuery.tmpl library and Underscore.js's templating engine.</span></span>

## <a name="components"></a><span data-ttu-id="e0f49-216">Składniki</span><span class="sxs-lookup"><span data-stu-id="e0f49-216">Components</span></span>

<span data-ttu-id="e0f49-217">Składniki umożliwiają organizowanie i ponowne użycie kodu interfejsu użytkownika, zwykle razem z danych ViewModel, od którego zależy kodu interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e0f49-217">Components allow you to organize and reuse UI code, usually along with the ViewModel data on which the UI code depends.</span></span> <span data-ttu-id="e0f49-218">Aby utworzyć składnik, należy po prostu określić jego szablonu i jego viewModel i nadaj mu nazwę.</span><span class="sxs-lookup"><span data-stu-id="e0f49-218">To create a component, you simply need to specify its template and its viewModel, and give it a name.</span></span> <span data-ttu-id="e0f49-219">Jest to realizowane przez wywołanie `ko.components.register()`.</span><span class="sxs-lookup"><span data-stu-id="e0f49-219">This is done by calling `ko.components.register()`.</span></span> <span data-ttu-id="e0f49-220">Oprócz Definiowanie szablonów i wbudowanego viewmodel, mogą być załadowany z zewnętrznych plików za pomocą biblioteki, takich jak *require.js*, co w kodzie bardzo czyste i wydajne.</span><span class="sxs-lookup"><span data-stu-id="e0f49-220">In addition to defining the templates and viewmodel inline, they can be loaded from external files using a library like *require.js*, resulting in very clean and efficient code.</span></span>

## <a name="communicating-with-apis"></a><span data-ttu-id="e0f49-221">Podczas komunikacji z interfejsów API</span><span class="sxs-lookup"><span data-stu-id="e0f49-221">Communicating with APIs</span></span>

<span data-ttu-id="e0f49-222">Odcinania może współpracować z żadnych danych w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="e0f49-222">Knockout can work with any data in JSON format.</span></span> <span data-ttu-id="e0f49-223">Typowym sposobem pobrania i zapisywanie danych przy użyciu odcinania jest z jQuery, która obsługuje `$.getJSON()` funkcji, aby pobrać dane i `$.post()` metody do przesyłania danych za pomocą przeglądarki do punktu końcowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="e0f49-223">A common way to retrieve and save data using Knockout is with jQuery, which supports the `$.getJSON()` function to retrieve data, and the `$.post()` method to send data from the browser to an API endpoint.</span></span> <span data-ttu-id="e0f49-224">Oczywiście jeśli wolisz inny sposób, aby wysyłać i odbierać dane JSON odcinania zostanie z nim również działać.</span><span class="sxs-lookup"><span data-stu-id="e0f49-224">Of course, if you prefer a different way to send and receive JSON data, Knockout will work with it as well.</span></span>

## <a name="summary"></a><span data-ttu-id="e0f49-225">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="e0f49-225">Summary</span></span>

<span data-ttu-id="e0f49-226">Odcinania zapewnia prosty, elegancki sposób powiązania elementów interfejsu użytkownika do bieżącego stanu aplikacji klienckiej, zdefiniowane w ViewModel.</span><span class="sxs-lookup"><span data-stu-id="e0f49-226">Knockout provides a simple, elegant way to bind UI elements to the current state of the client application, defined in a ViewModel.</span></span> <span data-ttu-id="e0f49-227">Składnia wiązania w odcinania używa atrybutu data-bind, stosowane do elementów HTML, które mają być przetwarzane.</span><span class="sxs-lookup"><span data-stu-id="e0f49-227">Knockout's binding syntax uses the data-bind attribute, applied to HTML elements that are to be processed.</span></span> <span data-ttu-id="e0f49-228">Odcinania jest w stanie wydajnie renderowania i zaktualizuj dużych zestawów danych, śledzenie elementów interfejsu użytkownika i przetwarza tylko zmiany w wpływ na elementy.</span><span class="sxs-lookup"><span data-stu-id="e0f49-228">Knockout is able to efficiently render and update large data sets by tracking UI elements and only processing changes to affected elements.</span></span> <span data-ttu-id="e0f49-229">Dużych aplikacji można podzielić logika interfejsu użytkownika przy użyciu szablonów i składników, które mogą być ładowane na żądanie z plików zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="e0f49-229">Large applications can break up UI logic using templates and components, which can be loaded on demand from external files.</span></span> <span data-ttu-id="e0f49-230">Obecnie w wersji 3, odcinania jest stabilna biblioteka języka JavaScript, które może poprawić aplikacji sieci web, które wymagają interakcji wzbogaconego klienta.</span><span class="sxs-lookup"><span data-stu-id="e0f49-230">Currently version 3, Knockout is a stable JavaScript library that can improve web applications that require rich client interactivity.</span></span>
