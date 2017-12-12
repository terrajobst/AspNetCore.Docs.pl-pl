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
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a><span data-ttu-id="3a15b-104">Przy użyciu AngularJS dla aplikacji jednej strony (źródła) z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3a15b-104">Using AngularJS for Single Page Applications (SPAs) with ASP.NET Core</span></span>


<span data-ttu-id="3a15b-105">Przez [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="3a15b-105">By [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="3a15b-106">W tym artykule dowiesz się, jak utworzyć aplikację ASP.NET SPA stylu przy użyciu AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-106">In this article, you will learn how to build a SPA-style ASP.NET application using AngularJS.</span></span>

<span data-ttu-id="3a15b-107">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3a15b-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-angularjs"></a><span data-ttu-id="3a15b-108">Co to jest AngularJS?</span><span class="sxs-lookup"><span data-stu-id="3a15b-108">What is AngularJS?</span></span>

<span data-ttu-id="3a15b-109">[AngularJS](https://angularjs.org/) jest nowoczesne architektury JavaScript z Google najczęściej używanych do pracy z jednej strony aplikacji (źródła).</span><span class="sxs-lookup"><span data-stu-id="3a15b-109">[AngularJS](https://angularjs.org/) is a modern JavaScript framework from Google commonly used to work with Single Page Applications (SPAs).</span></span> <span data-ttu-id="3a15b-110">AngularJS jest otwarty powierzając jej ich konserwację MIT licencji, a postęp programowanie AngularJS może występować [repozytorium GitHub](https://github.com/angular/angular.js).</span><span class="sxs-lookup"><span data-stu-id="3a15b-110">AngularJS is open sourced under MIT license, and the development progress of AngularJS can be followed on [its GitHub repository](https://github.com/angular/angular.js).</span></span> <span data-ttu-id="3a15b-111">Biblioteka jest nazywany kątową, ponieważ HTML używa nawiasów kątowego w kształcie.</span><span class="sxs-lookup"><span data-stu-id="3a15b-111">The library is called Angular because HTML uses angular-shaped brackets.</span></span>

<span data-ttu-id="3a15b-112">AngularJS nie jest biblioteką manipulowania modelu DOM, takich jak jQuery, ale używa podzbiór o nazwie jQLite jQuery.</span><span class="sxs-lookup"><span data-stu-id="3a15b-112">AngularJS is not a DOM manipulation library like jQuery, but it uses a subset of jQuery called jQLite.</span></span> <span data-ttu-id="3a15b-113">AngularJS opiera się głównie na deklaratywne atrybuty HTML, które można dodać do tagów HTML.</span><span class="sxs-lookup"><span data-stu-id="3a15b-113">AngularJS is primarily based on declarative HTML attributes that you can add to your HTML tags.</span></span> <span data-ttu-id="3a15b-114">Możesz spróbować AngularJS w przeglądarce za pomocą [służbowe kodu witryny sieci Web](https://www.codeschool.com/courses/shaping-up-with-angularjs) lub [W3Schools witryny sieci Web](https://www.w3schools.com/angular/).</span><span class="sxs-lookup"><span data-stu-id="3a15b-114">You can try AngularJS in your browser using the [Code School website](https://www.codeschool.com/courses/shaping-up-with-angularjs) or  [W3Schools website](https://www.w3schools.com/angular/).</span></span>

<span data-ttu-id="3a15b-115">Ten artykuł skupia się na AngularJS, niektóre uwagi o której kątową jest nagłówkiem.</span><span class="sxs-lookup"><span data-stu-id="3a15b-115">This article focuses on AngularJS with some notes on where Angular is heading.</span></span>

## <a name="getting-started"></a><span data-ttu-id="3a15b-116">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="3a15b-116">Getting started</span></span>

<span data-ttu-id="3a15b-117">Aby rozpocząć korzystanie z AngularJS w aplikacji platformy ASP.NET, musi ją zainstalować w ramach projektu lub Przywołaj ją z sieci dostarczania zawartości (CDN).</span><span class="sxs-lookup"><span data-stu-id="3a15b-117">To start using AngularJS in your ASP.NET application, you must either install it as part of your project, or reference it from a content delivery network (CDN).</span></span>

### <a name="installation"></a><span data-ttu-id="3a15b-118">Instalacja</span><span class="sxs-lookup"><span data-stu-id="3a15b-118">Installation</span></span>

<span data-ttu-id="3a15b-119">Istnieje kilka sposobów, aby dodać AngularJS do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3a15b-119">There are several ways to add AngularJS to your application.</span></span> <span data-ttu-id="3a15b-120">Jeśli zaczynasz nowej aplikacji sieci web platformy ASP.NET Core w programie Visual Studio, możesz dodać AngularJS przy użyciu wbudowanych [Bower](bower.md) obsługuje.</span><span class="sxs-lookup"><span data-stu-id="3a15b-120">If you’re starting a new ASP.NET Core web application in Visual Studio, you can add AngularJS using the built-in [Bower](bower.md) support.</span></span> <span data-ttu-id="3a15b-121">Otwórz *bower.json*i dodać wpis do `dependencies` właściwości:</span><span class="sxs-lookup"><span data-stu-id="3a15b-121">Open *bower.json*, and add an entry to the `dependencies` property:</span></span>

<a name="angular-bower-json"></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

<span data-ttu-id="3a15b-122">Po zapisaniu *bower.json* plik, kątową zostanie zainstalowany na projekt *wwwroot/lib* folderu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-122">Upon saving the *bower.json* file, Angular will be installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="3a15b-123">Ponadto, taka informacja znajdzie się w obrębie `Dependencies/Bower` folderu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-123">Additionally, it will be listed within the `Dependencies/Bower` folder.</span></span> <span data-ttu-id="3a15b-124">Zobacz poniższy zrzut ekranu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-124">See the screenshot below.</span></span>

![Eksplorator rozwiązań z projektem AngularJS](angular/_static/angular-solution-explorer.png)

<span data-ttu-id="3a15b-126">Następnie dodaj `<script>` odwołania do dołu `<body>` sekcji strony HTML lub *_Layout.cshtml* plików, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="3a15b-126">Next, add a `<script>` reference to the bottom of the `<body>` section of your HTML page or *_Layout.cshtml* file, as shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

<span data-ttu-id="3a15b-127">Zaleca się, że aplikacje produkcyjne wykorzystywać CDN wspólnych bibliotek, takich jak AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-127">It's recommended that production applications utilize CDNs for common libraries like AngularJS.</span></span> <span data-ttu-id="3a15b-128">AngularJS można odwoływać się do jednego z kilku CDN, taką jak:</span><span class="sxs-lookup"><span data-stu-id="3a15b-128">You can reference AngularJS from one of several CDNs, such as this one:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

<span data-ttu-id="3a15b-129">Po uzyskaniu odwołania do *angular.js* pliku skryptu, wszystko jest gotowe do rozpoczęcia korzystania AngularJS na stronach sieci web.</span><span class="sxs-lookup"><span data-stu-id="3a15b-129">Once you have a reference to the *angular.js* script file, you're ready to begin using AngularJS in your web pages.</span></span>

## <a name="key-components"></a><span data-ttu-id="3a15b-130">Najważniejsze składniki</span><span class="sxs-lookup"><span data-stu-id="3a15b-130">Key components</span></span>

<span data-ttu-id="3a15b-131">AngularJS zawiera szereg głównych składników, takich jak *dyrektywy*, *szablony*, *wzmacniaki*, *modułów*,  *kontrolery*, *składniki*, *router składnika* itd.</span><span class="sxs-lookup"><span data-stu-id="3a15b-131">AngularJS includes a number of major components, such as *directives*, *templates*, *repeaters*, *modules*, *controllers*, *components*, *component router* and more.</span></span> <span data-ttu-id="3a15b-132">Przeanalizujmy, jak te składniki współpracują, aby dodać zachowanie do stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="3a15b-132">Let's examine how these components work together to add behavior to your web pages.</span></span>

### <a name="directives"></a><span data-ttu-id="3a15b-133">Dyrektyw</span><span class="sxs-lookup"><span data-stu-id="3a15b-133">Directives</span></span>

<span data-ttu-id="3a15b-134">Używa AngularJS [dyrektywy](https://docs.angularjs.org/guide/directive) rozszerzenie z niestandardowych atrybutów i elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="3a15b-134">AngularJS uses [directives](https://docs.angularjs.org/guide/directive) to extend HTML with custom attributes and elements.</span></span> <span data-ttu-id="3a15b-135">Dyrektywy AngularJS są definiowane za pomocą `data-ng-*` lub `ng-*` prefiksy (`ng` jest skrót kątowego).</span><span class="sxs-lookup"><span data-stu-id="3a15b-135">AngularJS directives are defined via `data-ng-*` or `ng-*` prefixes (`ng` is short for angular).</span></span> <span data-ttu-id="3a15b-136">Istnieją dwa typy dyrektywy AngularJS:</span><span class="sxs-lookup"><span data-stu-id="3a15b-136">There are two types of AngularJS directives:</span></span>

   1. <span data-ttu-id="3a15b-137">**Dyrektywy pierwotnych**: tych wstępnie zdefiniowanych przez zespół kątowego i są częścią struktury AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-137">**Primitive Directives**: These are predefined by the Angular team and are part of the AngularJS framework.</span></span>

   2. <span data-ttu-id="3a15b-138">**Niestandardowe dyrektywy**: są to dyrektywy niestandardowych, które można określić.</span><span class="sxs-lookup"><span data-stu-id="3a15b-138">**Custom Directives**: These are custom directives that you can define.</span></span>

<span data-ttu-id="3a15b-139">Jedną z pierwotnego dyrektyw używane we wszystkich aplikacjach AngularJS jest `ng-app` dyrektywa, która używa do ładowania aplikacji AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-139">One of the primitive directives used in all AngularJS applications is the `ng-app` directive, which bootstraps the AngularJS application.</span></span> <span data-ttu-id="3a15b-140">Ta dyrektywa może odnosić się do `<body>` tag lub elementu podrzędnego treści.</span><span class="sxs-lookup"><span data-stu-id="3a15b-140">This directive can be applied to the `<body>` tag or to a child element of the body.</span></span> <span data-ttu-id="3a15b-141">Zobaczmy w akcji.</span><span class="sxs-lookup"><span data-stu-id="3a15b-141">Let's see an example in action.</span></span> <span data-ttu-id="3a15b-142">Zakładając, że jesteś w projekcie platformy ASP.NET, można dodać do pliku HTML `wwwroot` folderze, lub Dodaj nowe akcji kontrolera i skojarzonego widoku.</span><span class="sxs-lookup"><span data-stu-id="3a15b-142">Assuming you're in an ASP.NET project, you can either add an HTML file to the `wwwroot` folder, or add a new controller action and an associated view.</span></span> <span data-ttu-id="3a15b-143">W takim przypadku zostały dodane nowe `Directives` metody akcji, aby `HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="3a15b-143">In this case, I've added a new `Directives` action method to `HomeController.cs`.</span></span> <span data-ttu-id="3a15b-144">Skojarzony widok jest następujący:</span><span class="sxs-lookup"><span data-stu-id="3a15b-144">The associated view is shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

<span data-ttu-id="3a15b-145">Aby zachować te przykłady od siebie niezależne, nie używam pliku udostępnionego układu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-145">To keep these samples independent of one another, I'm not using the shared layout file.</span></span> <span data-ttu-id="3a15b-146">Widoczny czy możemy dekorowane tag treść z `ng-app` dyrektywy, aby wskazać, ta strona jest aplikacją AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-146">You can see that we decorated the body tag with the `ng-app` directive to indicate this page is an AngularJS application.</span></span> <span data-ttu-id="3a15b-147">`{{2+2}}` Jest wyrażenie powiązania kątowego danych, które będzie więcej informacji na temat za chwilę.</span><span class="sxs-lookup"><span data-stu-id="3a15b-147">The `{{2+2}}` is an Angular data binding expression that you will learn more about in a moment.</span></span> <span data-ttu-id="3a15b-148">Po uruchomieniu tej aplikacji, w tym miejscu jest wynikiem:</span><span class="sxs-lookup"><span data-stu-id="3a15b-148">Here is the result if you run this application:</span></span>

![Proste dyrektywy Angular](angular/_static/simple-directive.png)

<span data-ttu-id="3a15b-150">Inne dyrektywy pierwotnych w AngularJS obejmują:</span><span class="sxs-lookup"><span data-stu-id="3a15b-150">Other primitive directives in AngularJS include:</span></span>

<span data-ttu-id="3a15b-151">`ng-controller`Określa, który kontroler JavaScript jest powiązany z widoku.</span><span class="sxs-lookup"><span data-stu-id="3a15b-151">`ng-controller` Determines which JavaScript controller is bound to which view.</span></span>

<span data-ttu-id="3a15b-152">`ng-model`Określa model, z którym związane są wartości właściwości elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="3a15b-152">`ng-model` Determines the model to which the values of an HTML element's properties are bound.</span></span>

<span data-ttu-id="3a15b-153">`ng-init`Używane do zainicjowania danych aplikacji w postaci wyrażenia dla bieżącego zakresu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-153">`ng-init` Used to initialize the application data in the form of an expression for the current scope.</span></span>

<span data-ttu-id="3a15b-154">`ng-if`Usuwa lub odtwarza danego elementu HTML w modelu DOM, oparte na truthiness z dostarczonego wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="3a15b-154">`ng-if` Removes or recreates the given HTML element in the DOM based on the truthiness of the expression provided.</span></span>

<span data-ttu-id="3a15b-155">`ng-repeat`Powtarza danym bloku kodu HTML dla zestawu danych.</span><span class="sxs-lookup"><span data-stu-id="3a15b-155">`ng-repeat` Repeats a given block of HTML over a set of data.</span></span>

<span data-ttu-id="3a15b-156">`ng-show`Wyświetlenie lub ukrycie oparte na dostarczonego wyrażenia danego elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="3a15b-156">`ng-show` Shows or hides the given HTML element based on the expression provided.</span></span>

<span data-ttu-id="3a15b-157">Aby uzyskać pełną listę wszystkich pierwotnych dyrektyw obsługiwane w AngularJS, zapoznaj się [sekcji dyrektywy dokumentacji w witrynie sieci Web z dokumentacją AngularJS](https://docs.angularjs.org/api/ng/directive).</span><span class="sxs-lookup"><span data-stu-id="3a15b-157">For a full list of all primitive directives supported in AngularJS, please refer to the [directive documentation section on the AngularJS documentation website](https://docs.angularjs.org/api/ng/directive).</span></span>

### <a name="data-binding"></a><span data-ttu-id="3a15b-158">Powiązanie danych</span><span class="sxs-lookup"><span data-stu-id="3a15b-158">Data binding</span></span>

<span data-ttu-id="3a15b-159">Udostępnia AngularJS [wiązania z danymi](https://docs.angularjs.org/guide/databinding) obsługuje out-of--box za pomocą `ng-bind` dyrektywy lub danych, takich jak powiązania składni wyrażenia `{{expression}}`.</span><span class="sxs-lookup"><span data-stu-id="3a15b-159">AngularJS provides [data binding](https://docs.angularjs.org/guide/databinding) support out-of-the-box using either the `ng-bind` directive or a data binding expression syntax such as `{{expression}}`.</span></span> <span data-ttu-id="3a15b-160">AngularJS obsługuje powiązanie danych dwukierunkowe miejsca przechowywania danych z modelu podczas synchronizacji z szablonu widoku przez cały czas.</span><span class="sxs-lookup"><span data-stu-id="3a15b-160">AngularJS supports two-way data binding where data from a model is kept in synchronization with a view template at all times.</span></span> <span data-ttu-id="3a15b-161">Zmiany wprowadzone w widoku są automatycznie odzwierciedlane w modelu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-161">Any changes to the view are automatically reflected in the model.</span></span> <span data-ttu-id="3a15b-162">Podobnie wszystkie zmiany w modelu są uwzględniane w widoku.</span><span class="sxs-lookup"><span data-stu-id="3a15b-162">Likewise, any changes in the model are reflected in the view.</span></span>

<span data-ttu-id="3a15b-163">Utwórz plik HTML lub akcji kontrolera z towarzyszącym widok o nazwie `Databinding`.</span><span class="sxs-lookup"><span data-stu-id="3a15b-163">Create either an HTML file or a controller action with an accompanying view named `Databinding`.</span></span> <span data-ttu-id="3a15b-164">W widoku, są następujące:</span><span class="sxs-lookup"><span data-stu-id="3a15b-164">Include the following in the view:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

<span data-ttu-id="3a15b-165">Powiadomienie, że można wyświetlić za pomocą powiązania dyrektywy lub dane wartości modelu (`ng-bind`).</span><span class="sxs-lookup"><span data-stu-id="3a15b-165">Notice that you can display model values using either directives or data binding (`ng-bind`).</span></span> <span data-ttu-id="3a15b-166">Wynikowa strona powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="3a15b-166">The resulting page should look like this:</span></span>

![Proste powiązania danych](angular/_static/simple-databinding.png)

### <a name="templates"></a><span data-ttu-id="3a15b-168">Szablony</span><span class="sxs-lookup"><span data-stu-id="3a15b-168">Templates</span></span>

<span data-ttu-id="3a15b-169">[Szablony](https://docs.angularjs.org/guide/templates) w AngularJS są tylko zwykły stron HTML ozdobione dyrektywy AngularJS i artefaktów.</span><span class="sxs-lookup"><span data-stu-id="3a15b-169">[Templates](https://docs.angularjs.org/guide/templates) in AngularJS are just plain HTML pages decorated with AngularJS directives and artifacts.</span></span> <span data-ttu-id="3a15b-170">Szablon AngularJS jest mieszaniną dyrektywy, wyrażeń filtrów i formantów z HTML w celu utworzenia widoku.</span><span class="sxs-lookup"><span data-stu-id="3a15b-170">A template in AngularJS is a mixture of directives, expressions, filters, and controls that combine with HTML to form the view.</span></span>

<span data-ttu-id="3a15b-171">Dodaj inny widok, aby zademonstrować szablony i dodać następujące:</span><span class="sxs-lookup"><span data-stu-id="3a15b-171">Add another view to demonstrate templates, and add the following to it:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

<span data-ttu-id="3a15b-172">Szablon ma dyrektywy AngularJS, takich jak `ng-app`, `ng-init`, `ng-model` i składnia wyrażenia wiązania danych do powiązania `personName` właściwości.</span><span class="sxs-lookup"><span data-stu-id="3a15b-172">The template has AngularJS directives like `ng-app`, `ng-init`, `ng-model` and data binding expression syntax to bind the `personName` property.</span></span> <span data-ttu-id="3a15b-173">Uruchomiona w przeglądarce, widok wygląda jak na poniższym zrzucie ekranu:</span><span class="sxs-lookup"><span data-stu-id="3a15b-173">Running in the browser, the view looks like the screenshot below:</span></span>

![Prosty przykład szablony 1](angular/_static/simple-templates-1.png)

<span data-ttu-id="3a15b-175">Jeśli zmieniasz nazwę pola, wpisując polecenie w polu wejściowym zobaczysz tekst obok pola wejściowego dynamicznie aktualizacji przedstawiający kątowego dwukierunkowe danych powiązania w akcji.</span><span class="sxs-lookup"><span data-stu-id="3a15b-175">If you change the name by typing in the input field, you will see the text next to the input field dynamically update, showing Angular two-way data binding in action.</span></span>

![Prosty przykład szablony 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a><span data-ttu-id="3a15b-177">Wyrażenia</span><span class="sxs-lookup"><span data-stu-id="3a15b-177">Expressions</span></span>

<span data-ttu-id="3a15b-178">[Wyrażenia](https://docs.angularjs.org/guide/expression) w AngularJS są wstawki kodu notacji języka JavaScript, które są zapisywane w `{{ expression }}` składni.</span><span class="sxs-lookup"><span data-stu-id="3a15b-178">[Expressions](https://docs.angularjs.org/guide/expression) in AngularJS are JavaScript-like code snippets that are written inside the `{{ expression }}` syntax.</span></span> <span data-ttu-id="3a15b-179">Dane z tych wyrażeń jest powiązany z HTML w taki sam sposób jak `ng-bind` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="3a15b-179">The data from these expressions is bound to HTML the same way as `ng-bind` directives.</span></span> <span data-ttu-id="3a15b-180">Podstawowa różnica między AngularJS wyrażeń i wyrażeń regularnych języka JavaScript jest tym AngularJS wyrażenia są obliczane względem `$scope` obiektu w AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-180">The main difference between AngularJS expressions and regular JavaScript expressions is that AngularJS expressions are evaluated against the `$scope` object in AngularJS.</span></span>

<span data-ttu-id="3a15b-181">Wyrażenia AngularJS w przykładzie poniżej bind `personName` i proste JavaScript obliczane wyrażenie:</span><span class="sxs-lookup"><span data-stu-id="3a15b-181">The AngularJS expressions in the sample below bind `personName` and a simple JavaScript calculated expression:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

<span data-ttu-id="3a15b-182">Przykład uruchomiony w przeglądarce Wyświetla `personName` danych oraz wynikiem obliczenia:</span><span class="sxs-lookup"><span data-stu-id="3a15b-182">The example running in the browser displays the `personName` data and the results of the calculation:</span></span>

![Proste wyrażenia](angular/_static/simple-expressions.png)

### <a name="repeaters"></a><span data-ttu-id="3a15b-184">Wzmacniaki</span><span class="sxs-lookup"><span data-stu-id="3a15b-184">Repeaters</span></span>

<span data-ttu-id="3a15b-185">Powtarzanie w AngularJS odbywa się za pośrednictwem pierwotnych dyrektywy o nazwie `ng-repeat`.</span><span class="sxs-lookup"><span data-stu-id="3a15b-185">Repeating in AngularJS is done via a primitive directive called `ng-repeat`.</span></span> <span data-ttu-id="3a15b-186">`ng-repeat` Dyrektywy powtarza danego elementu HTML w widoku na długości tablicy identycznych danych.</span><span class="sxs-lookup"><span data-stu-id="3a15b-186">The `ng-repeat` directive repeats a given HTML element in a view over the length of a repeating data array.</span></span> <span data-ttu-id="3a15b-187">Wzmacniaki w AngularJS można powtarzać w tablicy ciągów lub obiektów.</span><span class="sxs-lookup"><span data-stu-id="3a15b-187">Repeaters in AngularJS can repeat over an array of strings or objects.</span></span> <span data-ttu-id="3a15b-188">Oto przykładowe użycie powtórzonych na tablicę ciągów:</span><span class="sxs-lookup"><span data-stu-id="3a15b-188">Here is a sample usage of repeating over an array of strings:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

<span data-ttu-id="3a15b-189">[Repeat — dyrektywa](https://docs.angularjs.org/api/ng/directive/ngRepeat) generuje szereg elementów listy w nieuporządkowaną listę, jak widać w narzędziach developer pokazano tego zrzutu ekranu:</span><span class="sxs-lookup"><span data-stu-id="3a15b-189">The [repeat directive](https://docs.angularjs.org/api/ng/directive/ngRepeat) outputs a series of list items in an unordered list, as you can see in the developer tools shown in this screenshot:</span></span>

![Przykład elementu powtarzanego](angular/_static/repeater.png)

<span data-ttu-id="3a15b-191">Oto przykład, który powtarza na tablicę obiektów.</span><span class="sxs-lookup"><span data-stu-id="3a15b-191">Here is an example that repeats over an array of objects.</span></span> <span data-ttu-id="3a15b-192">`ng-init` Dyrektywa określa `names` tablicy, której każdy element jest obiekt zawierający najpierw i nazwiska.</span><span class="sxs-lookup"><span data-stu-id="3a15b-192">The `ng-init` directive establishes a `names` array, where each element is an object containing first and last names.</span></span> <span data-ttu-id="3a15b-193">`ng-repeat` Przypisania, `name in names`, danych wyjściowych elementu każdy element tablicy.</span><span class="sxs-lookup"><span data-stu-id="3a15b-193">The `ng-repeat` assignment, `name in names`, outputs a list item for every array element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

<span data-ttu-id="3a15b-194">W takim przypadku dane wyjściowe jest taka sama, jak w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="3a15b-194">The output in this case is the same as in the previous example.</span></span>

<span data-ttu-id="3a15b-195">Kątową zawiera niektóre dodatkowe dyrektywy, które pomaga zapewnić działanie pętli w przypadku jego wykonywania.</span><span class="sxs-lookup"><span data-stu-id="3a15b-195">Angular provides some additional directives that can help provide behavior based on where the loop is in its execution.</span></span>

`$index`

<span data-ttu-id="3a15b-196">Użyj `$index` w `ng-repeat` pętli, aby określić indeks pozycji z pętli obecnie znajduje się na.</span><span class="sxs-lookup"><span data-stu-id="3a15b-196">Use `$index` in the `ng-repeat` loop to determine which index position your loop currently is on.</span></span>

<span data-ttu-id="3a15b-197">`$even`i`$odd`</span><span class="sxs-lookup"><span data-stu-id="3a15b-197">`$even` and `$odd`</span></span>

<span data-ttu-id="3a15b-198">Użyj `$even` w `ng-repeat` pętli w celu ustalenia, czy bieżącego indeksu w Twojej pętli jest nawet indeksowanego wiersza.</span><span class="sxs-lookup"><span data-stu-id="3a15b-198">Use `$even` in the `ng-repeat` loop to determine whether the current index in your loop is an even indexed row.</span></span> <span data-ttu-id="3a15b-199">Podobnie, użyj `$odd` do ustalenia, czy bieżący indeks jest nieparzysta indeksowanego wiersza.</span><span class="sxs-lookup"><span data-stu-id="3a15b-199">Similarly, use `$odd` to determine if the current index is an odd indexed row.</span></span>

<span data-ttu-id="3a15b-200">`$first`i`$last`</span><span class="sxs-lookup"><span data-stu-id="3a15b-200">`$first` and `$last`</span></span>

<span data-ttu-id="3a15b-201">Użyj `$first` w `ng-repeat` pętli, aby ustalić, czy bieżący indeks w pętli z pierwszego wiersza.</span><span class="sxs-lookup"><span data-stu-id="3a15b-201">Use `$first` in the `ng-repeat` loop to determine whether the current index in your loop is the first row.</span></span> <span data-ttu-id="3a15b-202">Podobnie, użyj `$last` do ustalenia, czy bieżący indeks jest ostatni wiersz.</span><span class="sxs-lookup"><span data-stu-id="3a15b-202">Similarly, use `$last` to determine if the current index is the last row.</span></span>

<span data-ttu-id="3a15b-203">Poniżej znajduje się przykład pokazujący `$index`, `$even`, `$odd`, `$first`, i `$last` w akcji:</span><span class="sxs-lookup"><span data-stu-id="3a15b-203">Below is a sample that shows `$index`, `$even`, `$odd`, `$first`, and `$last` in action:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

<span data-ttu-id="3a15b-204">Poniżej przedstawiono dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="3a15b-204">Here is the resulting output:</span></span>

![Przykład elementu powtarzanego 2](angular/_static/repeaters2.png)

### <a name="scope"></a><span data-ttu-id="3a15b-206">$scope</span><span class="sxs-lookup"><span data-stu-id="3a15b-206">$scope</span></span>

<span data-ttu-id="3a15b-207">`$scope`jest obiektem JavaScript, który działa jako sklejki między widokiem (szablonu) i kontrolera (co omówiono poniżej).</span><span class="sxs-lookup"><span data-stu-id="3a15b-207">`$scope` is a JavaScript object that acts as glue between the view (template) and the controller (explained below).</span></span> <span data-ttu-id="3a15b-208">Szablon widoku AngularJS zna tylko wartości dołączony do `$scope` obiektu w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="3a15b-208">A view template in AngularJS only knows about the values attached to the `$scope` object in the controller.</span></span>

> [!NOTE]
> <span data-ttu-id="3a15b-209">W świecie MVVM `$scope` obiektu w AngularJS często jest zdefiniowany jako ViewModel.</span><span class="sxs-lookup"><span data-stu-id="3a15b-209">In the MVVM world, the `$scope` object in AngularJS is often defined as the ViewModel.</span></span> <span data-ttu-id="3a15b-210">Zespół AngularJS odwołuje się do `$scope` obiektu jako modelu danych.</span><span class="sxs-lookup"><span data-stu-id="3a15b-210">The AngularJS team refers to the `$scope` object as the Data-Model.</span></span> <span data-ttu-id="3a15b-211">[Dowiedz się więcej na temat zakresów w AngularJS](https://docs.angularjs.org/guide/scope).</span><span class="sxs-lookup"><span data-stu-id="3a15b-211">[Learn more about Scopes in AngularJS](https://docs.angularjs.org/guide/scope).</span></span>

<span data-ttu-id="3a15b-212">Poniżej przedstawiono prosty przykład pokazujący sposób ustawić właściwości `$scope` w osobnym pliku JavaScript, *scope.js*:</span><span class="sxs-lookup"><span data-stu-id="3a15b-212">Below is a simple example showing how to set properties on `$scope` within a separate JavaScript file, *scope.js*:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

<span data-ttu-id="3a15b-213">Obserwować `$scope` parametr przekazany do kontrolera na wiersz 2.</span><span class="sxs-lookup"><span data-stu-id="3a15b-213">Observe the `$scope` parameter passed to the controller on line 2.</span></span> <span data-ttu-id="3a15b-214">Ten obiekt jest widok wie o.</span><span class="sxs-lookup"><span data-stu-id="3a15b-214">This object is what the view knows about.</span></span> <span data-ttu-id="3a15b-215">W wierszu 3 konfigurujemy ustawienia właściwość o nazwie "name" na "Joanna Magdalena".</span><span class="sxs-lookup"><span data-stu-id="3a15b-215">On line 3, we are setting a property called "name" to "Mary Jane".</span></span>

<span data-ttu-id="3a15b-216">Co się stanie, gdy nie znaleziono określonej właściwości przez widok?</span><span class="sxs-lookup"><span data-stu-id="3a15b-216">What happens when a particular property is not found by the view?</span></span> <span data-ttu-id="3a15b-217">Wyświetlanie określonych poniżej odwołuje się do właściwości "name" i "wiek":</span><span class="sxs-lookup"><span data-stu-id="3a15b-217">The view defined below refers to "name" and "age" properties:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

<span data-ttu-id="3a15b-218">Zwróć uwagę, w wierszu 9, że prosimy kątową, aby wyświetlić właściwości "name", używając składni wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="3a15b-218">Notice on line 9 that we are asking Angular to show the "name" property using expression syntax.</span></span> <span data-ttu-id="3a15b-219">Następnie wiersz 10 dotyczy "wiek", właściwość, która nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="3a15b-219">Line 10 then refers to "age", a property that does not exist.</span></span> <span data-ttu-id="3a15b-220">Uruchomione przykładzie nazwa ustawioną "Joanna Magdalena" i nic wieku.</span><span class="sxs-lookup"><span data-stu-id="3a15b-220">The running example shows the name set to "Mary Jane" and nothing for age.</span></span> <span data-ttu-id="3a15b-221">Brak właściwości są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="3a15b-221">Missing properties are ignored.</span></span>

![Przykład zakresu](angular/_static/scope.png)

### <a name="modules"></a><span data-ttu-id="3a15b-223">Moduły</span><span class="sxs-lookup"><span data-stu-id="3a15b-223">Modules</span></span>

<span data-ttu-id="3a15b-224">A [modułu](https://docs.angularjs.org/guide/module) w AngularJS to kolekcja kontrolerów, usługi, dyrektywy itd. `angular.module()` Wywołanie funkcji służy do tworzenia, rejestrowania i pobranie modułów w AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-224">A [module](https://docs.angularjs.org/guide/module) in AngularJS is a collection of controllers, services, directives, etc. The `angular.module()` function call is used to create, register, and retrieve modules in AngularJS.</span></span> <span data-ttu-id="3a15b-225">Wszystkie moduły, w tym dostarczonym przez AngularJS zespołu i bibliotek innych firm, powinny być rejestrowane za pomocą `angular.module()` funkcji.</span><span class="sxs-lookup"><span data-stu-id="3a15b-225">All modules, including those shipped by the AngularJS team and third party libraries, should be registered using the `angular.module()` function.</span></span>

<span data-ttu-id="3a15b-226">Poniżej przedstawiono fragment kodu, który pokazuje, jak utworzyć nowy moduł w AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-226">Below is a snippet of code that shows how to create a new module in AngularJS.</span></span> <span data-ttu-id="3a15b-227">Pierwszy parametr jest nazwa modułu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-227">The first parameter is the name of the module.</span></span> <span data-ttu-id="3a15b-228">Drugi parametr określa zależności w innych modułach.</span><span class="sxs-lookup"><span data-stu-id="3a15b-228">The second parameter defines dependencies on other modules.</span></span> <span data-ttu-id="3a15b-229">W dalszej części tego artykułu, firma Microsoft można pokazujący sposób przekazywania te zależności, aby `angular.module()` wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="3a15b-229">Later in this article, we will be showing how to pass these dependencies to an `angular.module()` method call.</span></span>

```javascript
var personApp = angular.module('personApp', []);
```

<span data-ttu-id="3a15b-230">Użyj `ng-app` dyrektywy do reprezentowania moduł AngularJS, na stronie.</span><span class="sxs-lookup"><span data-stu-id="3a15b-230">Use the `ng-app` directive to represent an AngularJS module on the page.</span></span> <span data-ttu-id="3a15b-231">Używaj modułu TPM, Przypisz nazwę modułu, `personApp` w tym przykładzie do `ng-app` dyrektywy w naszym szablonu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-231">To use a module, assign the name of the module, `personApp` in this example, to the `ng-app` directive in our template.</span></span>

```html
<body ng-app="personApp">
```

### <a name="controllers"></a><span data-ttu-id="3a15b-232">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="3a15b-232">Controllers</span></span>

<span data-ttu-id="3a15b-233">[Kontrolery](https://docs.angularjs.org/guide/controller) w AngularJS są pierwszy punkt wejścia dla kodu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-233">[Controllers](https://docs.angularjs.org/guide/controller) in AngularJS are the first point of entry for your code.</span></span> <span data-ttu-id="3a15b-234">`<module name>.controller()` Wywołanie funkcji jest używany do tworzenia i zarejestrować kontrolerów w AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-234">The `<module name>.controller()` function call is used to create and register controllers in AngularJS.</span></span> <span data-ttu-id="3a15b-235">`ng-controller` Dyrektywa jest używana do reprezentowania kontroler AngularJS, na stronie HTML.</span><span class="sxs-lookup"><span data-stu-id="3a15b-235">The `ng-controller` directive is used to represent an AngularJS controller on the HTML page.</span></span> <span data-ttu-id="3a15b-236">Rolę kontrolera kątową jest ustalenie stanu i zachowanie modelu danych (`$scope`).</span><span class="sxs-lookup"><span data-stu-id="3a15b-236">The role of the controller in Angular is to set state and behavior of the data model (`$scope`).</span></span> <span data-ttu-id="3a15b-237">Nie można używać kontrolery do manipulowania w modelu DOM bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="3a15b-237">Controllers should not be used to manipulate the DOM directly.</span></span>

<span data-ttu-id="3a15b-238">Poniżej znajduje się fragment kodu, który rejestruje nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="3a15b-238">Below is a snippet of code that registers a new controller.</span></span> <span data-ttu-id="3a15b-239">`personApp` Kątowego moduł, który jest zdefiniowany w wierszu 2 odwołuje się do zmiennej we fragmencie.</span><span class="sxs-lookup"><span data-stu-id="3a15b-239">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

<span data-ttu-id="3a15b-240">W widoku przy użyciu `ng-controller` dyrektywy przypisuje nazwę kontrolera:</span><span class="sxs-lookup"><span data-stu-id="3a15b-240">The view using the `ng-controller` directive assigns the controller name:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

<span data-ttu-id="3a15b-241">Na stronie znajdują się "Maria" oraz "Magdalena", który odpowiada `firstName` i `lastName` właściwości dołączonych do `$scope` obiektu:</span><span class="sxs-lookup"><span data-stu-id="3a15b-241">The page shows "Mary" and "Jane" that correspond to the `firstName` and `lastName` properties attached to the `$scope` object:</span></span>

![Przykład kontrolera](angular/_static/controllers.png)

### <a name="components"></a><span data-ttu-id="3a15b-243">Składniki</span><span class="sxs-lookup"><span data-stu-id="3a15b-243">Components</span></span>

<span data-ttu-id="3a15b-244">[Składniki](https://docs.angularjs.org/guide/component) na kątową 1.5.x umożliwia hermetyzację oraz możliwość tworzenia poszczególnych elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="3a15b-244">[Components](https://docs.angularjs.org/guide/component) in Angular 1.5.x allow for the encapsulation and capability of creating individual HTML elements.</span></span> <span data-ttu-id="3a15b-245">W 1.4.x kątowego można osiągnąć tej samej funkcji przy użyciu metody .directive().</span><span class="sxs-lookup"><span data-stu-id="3a15b-245">In Angular 1.4.x you could achieve the same feature using the .directive() method.</span></span>

<span data-ttu-id="3a15b-246">Przy użyciu metody .component() programowanie upraszcza uzyskanie funkcji dyrektywy i kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3a15b-246">By using the .component() method, development is simplified gaining the functionality of the directive and the controller.</span></span> <span data-ttu-id="3a15b-247">Inne zalety; zakres izolacji, najlepsze rozwiązania są związane i migracji do 2 kątowego staje się łatwiejsze zadań.</span><span class="sxs-lookup"><span data-stu-id="3a15b-247">Other benefits include; scope isolation, best practices are inherent, and migration to Angular 2 becomes an easier task.</span></span> <span data-ttu-id="3a15b-248">`<module name>.component()` Wywołanie funkcji jest używany do tworzenia i zarejestrować komponenty w AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-248">The `<module name>.component()` function call is used to create and register components in AngularJS.</span></span>

<span data-ttu-id="3a15b-249">Poniżej znajduje się fragment kodu, który rejestruje nowy składnik.</span><span class="sxs-lookup"><span data-stu-id="3a15b-249">Below is a snippet of code that registers a new component.</span></span> <span data-ttu-id="3a15b-250">`personApp` Kątowego moduł, który jest zdefiniowany w wierszu 2 odwołuje się do zmiennej we fragmencie.</span><span class="sxs-lookup"><span data-stu-id="3a15b-250">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

<span data-ttu-id="3a15b-251">Widok, w którym są wyświetlane niestandardowego elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="3a15b-251">The view where we are displaying the custom HTML element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

<span data-ttu-id="3a15b-252">Skojarzony szablon używany przez składnik:</span><span class="sxs-lookup"><span data-stu-id="3a15b-252">The associated template used by component:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

<span data-ttu-id="3a15b-253">Na stronie znajdują się "Aftab" i "Ansari", który odpowiada `firstName` i `lastName` właściwości dołączonych do `vm` obiektu:</span><span class="sxs-lookup"><span data-stu-id="3a15b-253">The page shows "Aftab" and "Ansari" that correspond to the `firstName` and `lastName` properties attached to the `vm` object:</span></span>

![Przykład składników](angular/_static/components.png)

### <a name="services"></a><span data-ttu-id="3a15b-255">Usługi</span><span class="sxs-lookup"><span data-stu-id="3a15b-255">Services</span></span>

<span data-ttu-id="3a15b-256">[Usługi](https://docs.angularjs.org/guide/services) w AngularJS są często używane do udostępnionego kodu, który jest niedostępny pobieranej do pliku, którego można użyć w okresie istnienia kątowego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3a15b-256">[Services](https://docs.angularjs.org/guide/services) in AngularJS are commonly used for shared code that is abstracted away into a file which can be used throughout the lifetime of an Angular application.</span></span> <span data-ttu-id="3a15b-257">Usługi opóźnieniem są tworzone, co oznacza, że nie będzie wystąpienia usługi, chyba że jest używany przez składnik zależy od usługi.</span><span class="sxs-lookup"><span data-stu-id="3a15b-257">Services are lazily instantiated, meaning that there will not be an instance of a service unless a component that depends on the service gets used.</span></span> <span data-ttu-id="3a15b-258">Fabryki są przykładem usługi używany w aplikacjach AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-258">Factories are an example of a service used in AngularJS applications.</span></span> <span data-ttu-id="3a15b-259">Fabryki są tworzone przy użyciu `myApp.factory()` wywołania funkcji, których `myApp` jest moduł.</span><span class="sxs-lookup"><span data-stu-id="3a15b-259">Factories are created using the `myApp.factory()` function call, where `myApp` is the module.</span></span>

<span data-ttu-id="3a15b-260">Poniżej znajduje się przykład przedstawia sposób użycia fabryki w AngularJS:</span><span class="sxs-lookup"><span data-stu-id="3a15b-260">Below is an example that shows how to use factories in AngularJS:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

<span data-ttu-id="3a15b-261">Aby wywołać tę fabrykę z kontrolera, Przekaż `personFactory` jako parametr `controller` funkcji:</span><span class="sxs-lookup"><span data-stu-id="3a15b-261">To call this factory from the controller, pass `personFactory` as a parameter to the `controller` function:</span></span>

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a><span data-ttu-id="3a15b-262">Za pomocą usług do komunikowania się punkt końcowy REST</span><span class="sxs-lookup"><span data-stu-id="3a15b-262">Using services to talk to a REST endpoint</span></span>

<span data-ttu-id="3a15b-263">Poniżej przedstawiono przykład end-to-end używa usług w AngularJS wchodzić w interakcje z punktem końcowym interfejsu API platformy ASP.NET Core sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3a15b-263">Below is an end-to-end example using services in AngularJS to interact with an ASP.NET Core Web API endpoint.</span></span> <span data-ttu-id="3a15b-264">Przykład pobiera dane z interfejsu API sieci Web i wyświetla dane w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="3a15b-264">The example gets data from the Web API and displays the data in a view template.</span></span> <span data-ttu-id="3a15b-265">Zacznijmy od widoku najpierw:</span><span class="sxs-lookup"><span data-stu-id="3a15b-265">Let's start with the view first:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

<span data-ttu-id="3a15b-266">W tym widoku mamy kątowego modułu o nazwie `PersonsApp` i wywołać kontrolera `personController`.</span><span class="sxs-lookup"><span data-stu-id="3a15b-266">In this view, we have an Angular module called `PersonsApp` and a controller called `personController`.</span></span> <span data-ttu-id="3a15b-267">Używamy `ng-repeat` do wykonywania iteracji listy osób.</span><span class="sxs-lookup"><span data-stu-id="3a15b-267">We are using `ng-repeat` to iterate over the list of persons.</span></span> <span data-ttu-id="3a15b-268">Odwołuje się trzy niestandardowe pliki JavaScript w wierszach 17-19.</span><span class="sxs-lookup"><span data-stu-id="3a15b-268">We are referencing three custom JavaScript files on lines 17-19.</span></span>

<span data-ttu-id="3a15b-269">*PersonApp.js* plik jest używany do rejestrowania `PersonsApp` modułu; i składnia jest podobny do poprzednich przykładach.</span><span class="sxs-lookup"><span data-stu-id="3a15b-269">The *personApp.js* file is used to register the `PersonsApp` module; and, the syntax is similar to previous examples.</span></span> <span data-ttu-id="3a15b-270">Używamy `angular.module` funkcji do utworzenia nowego wystąpienia modułu, w którym firma Microsoft będzie działać z.</span><span class="sxs-lookup"><span data-stu-id="3a15b-270">We are using the `angular.module` function to create a new instance of the module that we will be working with.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

<span data-ttu-id="3a15b-271">Spójrzmy na *personFactory.js*, poniżej.</span><span class="sxs-lookup"><span data-stu-id="3a15b-271">Let's take a look at *personFactory.js*, below.</span></span> <span data-ttu-id="3a15b-272">Wywołania modułu `factory` metodę w celu utworzenia do ustawień fabrycznych.</span><span class="sxs-lookup"><span data-stu-id="3a15b-272">We are calling the module’s `factory` method to create a factory.</span></span> <span data-ttu-id="3a15b-273">Wiersz 12 zawiera wbudowane kątową `$http` usługi podczas pobierania informacji osób z usługą sieci web.</span><span class="sxs-lookup"><span data-stu-id="3a15b-273">Line 12 shows the built-in Angular `$http` service retrieving people information from a web service.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

<span data-ttu-id="3a15b-274">W *personController.js*, wywołania modułu `controller` metodę w celu utworzenia kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3a15b-274">In *personController.js*, we are calling the module’s `controller` method to create the controller.</span></span> <span data-ttu-id="3a15b-275">`$scope` Obiektu `people` właściwości przypisano z danymi zwróconymi z personFactory (wiersz 13).</span><span class="sxs-lookup"><span data-stu-id="3a15b-275">The `$scope` object's `people` property is assigned the data returned from the personFactory (line 13).</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

<span data-ttu-id="3a15b-276">Spójrzmy szybkiego interfejsu API sieci Web i modelu za nią.</span><span class="sxs-lookup"><span data-stu-id="3a15b-276">Let's take a quick look at the Web API and the model behind it.</span></span> <span data-ttu-id="3a15b-277">`Person` Model jest POCO (zwykły stary obiekt CLR) z `Id`, `FirstName`, i `LastName` właściwości:</span><span class="sxs-lookup"><span data-stu-id="3a15b-277">The `Person` model is a POCO (Plain Old CLR Object) with `Id`, `FirstName`, and `LastName` properties:</span></span>

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

<span data-ttu-id="3a15b-278">`Person` Kontrolera zwraca listę formacie JSON `Person` obiektów:</span><span class="sxs-lookup"><span data-stu-id="3a15b-278">The `Person` controller returns a JSON-formatted list of `Person` objects:</span></span>

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

<span data-ttu-id="3a15b-279">Zobaczmy, aplikację w akcji:</span><span class="sxs-lookup"><span data-stu-id="3a15b-279">Let's see the application in action:</span></span>

![Wyświetlanie wyników REST kontrolera](angular/_static/rest-bound.png)

<span data-ttu-id="3a15b-281">Możesz [wyświetlanie struktury aplikacji w witrynie GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="3a15b-281">You can [view the application's structure on GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="3a15b-282">Aby uzyskać więcej informacji na temat struktury aplikacji AngularJS, zobacz [Papa Jan kątowego Przewodnik po stylu](https://github.com/johnpapa/angular-styleguide)</span><span class="sxs-lookup"><span data-stu-id="3a15b-282">For more on structuring AngularJS applications, see [John Papa's Angular Style Guide](https://github.com/johnpapa/angular-styleguide)</span></span>

&nbsp;

> [!NOTE]
> <span data-ttu-id="3a15b-283">Utworzyć moduł AngularJS, kontroler, fabryki, pliki dyrektywy i widoku można łatwo, sprawdź Sayed Hashimi [SideWaffle szablonu dodatkiem Service pack dla programu Visual Studio](http://sidewaffle.com/).</span><span class="sxs-lookup"><span data-stu-id="3a15b-283">To create AngularJS module, controller, factory, directive and view files easily, be sure to check out Sayed Hashimi's [SideWaffle template pack for Visual Studio](http://sidewaffle.com/).</span></span> <span data-ttu-id="3a15b-284">Sayed Hashimi jest starszy Menedżer programu Visual Studio zespołu sieci Web firmy Microsoft i szablonów SideWaffle są traktowane jako standard złota.</span><span class="sxs-lookup"><span data-stu-id="3a15b-284">Sayed Hashimi is a Senior Program Manager on the Visual Studio Web Team at Microsoft and SideWaffle templates are considered the gold standard.</span></span> <span data-ttu-id="3a15b-285">W momencie pisania tego SideWaffle jest dostępna dla programu Visual Studio 2012 2013 i 2015.</span><span class="sxs-lookup"><span data-stu-id="3a15b-285">At the time of this writing, SideWaffle is available for Visual Studio 2012, 2013, and 2015.</span></span>

### <a name="routing-and-multiple-views"></a><span data-ttu-id="3a15b-286">Routing i wiele widoków</span><span class="sxs-lookup"><span data-stu-id="3a15b-286">Routing and multiple views</span></span>

<span data-ttu-id="3a15b-287">AngularJS ma dostawcę wbudowanych trasy do obsługi nawigacji na podstawie SPA (jednej strony aplikacji).</span><span class="sxs-lookup"><span data-stu-id="3a15b-287">AngularJS has a built-in route provider to handle SPA (Single Page Application) based navigation.</span></span> <span data-ttu-id="3a15b-288">Aby pracować z routingiem w AngularJS, należy dodać `angular-route` biblioteki za pomocą rozwiązania Bower.</span><span class="sxs-lookup"><span data-stu-id="3a15b-288">To work with routing in AngularJS, you must add the `angular-route` library using Bower.</span></span> <span data-ttu-id="3a15b-289">Można zobaczyć w [bower.json](#angular-bower-json) pliku, do których odwołuje się na początku tego artykułu, że firma Microsoft już odwołuje się on w naszym projektu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-289">You can see in the [bower.json](#angular-bower-json) file referenced at the start of this article that we are already referencing it in our project.</span></span>

<span data-ttu-id="3a15b-290">Po zainstalowaniu pakietu, Dodaj odwołanie do skryptu (*kątowego route.js*) do widoku.</span><span class="sxs-lookup"><span data-stu-id="3a15b-290">After you install the package, add the script reference (*angular-route.js*) to your view.</span></span>

<span data-ttu-id="3a15b-291">Teraz Przyjrzyjmy aplikacji osoby możemy zostały tworzenia i Dodaj do niej nawigacji.</span><span class="sxs-lookup"><span data-stu-id="3a15b-291">Now let's take the Person App we have been building and add navigation to it.</span></span> <span data-ttu-id="3a15b-292">Najpierw, możemy utworzyć kopię aplikację przez utworzenie nowej `PeopleController` akcji o nazwie `Spa` i odpowiadające mu `Spa.cshtml` widoku przez skopiowanie widok Index.cshtml `People` folderu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-292">First, we will make a copy of the app by creating a new `PeopleController` action called `Spa` and a corresponding `Spa.cshtml` view by copying the Index.cshtml view in the `People` folder.</span></span> <span data-ttu-id="3a15b-293">Dodaj odwołanie do skryptu `angular-route` (patrz wiersz 11).</span><span class="sxs-lookup"><span data-stu-id="3a15b-293">Add a script reference to `angular-route` (see line 11).</span></span> <span data-ttu-id="3a15b-294">Również dodać `div` oznaczonej jako `ng-view` — dyrektywa (patrz wiersz 6) jako symbol zastępczy umieścić widoków.</span><span class="sxs-lookup"><span data-stu-id="3a15b-294">Also add a `div` marked with the `ng-view` directive (see line 6) as a placeholder to place views in.</span></span> <span data-ttu-id="3a15b-295">Zamierzamy korzystać z kilku dodatkowych *js* pliki, które są przywoływane w wierszach 13-16.</span><span class="sxs-lookup"><span data-stu-id="3a15b-295">We are going to be using several additional *.js* files which are referenced on lines 13-16.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

<span data-ttu-id="3a15b-296">Spójrzmy na *personModule.js* pliku, aby zobaczyć, jak są uruchamianiu modułu routingu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-296">Let's take a look at *personModule.js* file to see how we are instantiating the module with routing.</span></span> <span data-ttu-id="3a15b-297">Firma Microsoft przekazywane `ngRoute` jako biblioteki do modułu.</span><span class="sxs-lookup"><span data-stu-id="3a15b-297">We are passing `ngRoute` as a library into the module.</span></span> <span data-ttu-id="3a15b-298">Ten moduł obsługuje routing w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3a15b-298">This module handles routing in our application.</span></span>

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

<span data-ttu-id="3a15b-299">*PersonRoutes.js* pliku poniżej definiuje trasy w oparciu o dostawcy trasy.</span><span class="sxs-lookup"><span data-stu-id="3a15b-299">The *personRoutes.js* file, below, defines routes based on the route provider.</span></span> <span data-ttu-id="3a15b-300">Wiersze 4-7 definiują nawigacji skutecznie mówiąc, gdy adres URL z `/persons` jest wymagane, użyj szablonu o nazwie `partials/personlist` pracy za pośrednictwem `personListController`.</span><span class="sxs-lookup"><span data-stu-id="3a15b-300">Lines 4-7 define navigation by effectively saying, when a URL with `/persons` is requested, use a template called `partials/personlist` by working through `personListController`.</span></span> <span data-ttu-id="3a15b-301">Linie 8-11 wskazują strony szczegółów z parametrem trasy `personId`.</span><span class="sxs-lookup"><span data-stu-id="3a15b-301">Lines 8-11 indicate a detail page with a route parameter of `personId`.</span></span> <span data-ttu-id="3a15b-302">Jeśli adres URL nie pasuje do jednej z wzorców, kątową domyślnie `/persons` widoku.</span><span class="sxs-lookup"><span data-stu-id="3a15b-302">If the URL doesn't match one of the patterns, Angular defaults to the `/persons` view.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

<span data-ttu-id="3a15b-303">`personlist.html` Plik jest zawiera tylko niezbędne do wyświetlania listy osób HTML widoku częściowego.</span><span class="sxs-lookup"><span data-stu-id="3a15b-303">The `personlist.html` file is a partial view containing only the HTML needed to display person list.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

<span data-ttu-id="3a15b-304">Kontroler jest zdefiniowany za pomocą modułu `controller` działać w *personListController.js*.</span><span class="sxs-lookup"><span data-stu-id="3a15b-304">The controller is defined by using the module's `controller` function in *personListController.js*.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

<span data-ttu-id="3a15b-305">Jeśli firma Microsoft uruchomić tę aplikację i przejdź do `people/spa#/persons` adres URL, zostanie wyświetlone:</span><span class="sxs-lookup"><span data-stu-id="3a15b-305">If we run this application and navigate to the `people/spa#/persons` URL, we will see:</span></span>

![Widok listy osób](angular/_static/spa-persons.png)

<span data-ttu-id="3a15b-307">Jeśli firma Microsoft, przejdź do strony szczegółów, na przykład `people/spa#/persons/2`, zostanie wyświetlone widoku częściowego szczegółów:</span><span class="sxs-lookup"><span data-stu-id="3a15b-307">If we navigate to a detail page, for example `people/spa#/persons/2`, we will see the detail partial view:</span></span>

![Widok szczegółów osoby](angular/_static/spa-persons-2.png)

<span data-ttu-id="3a15b-309">Można wyświetlić pełną źródła i wszystkie pliki, które nie są wyświetlane w tym artykule, na [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="3a15b-309">You can view the full source and any files not shown in this article on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

### <a name="event-handlers"></a><span data-ttu-id="3a15b-310">Programy obsługi zdarzeń</span><span class="sxs-lookup"><span data-stu-id="3a15b-310">Event Handlers</span></span>

<span data-ttu-id="3a15b-311">Istnieje wiele dyrektyw w AngularJS, która dodanie funkcji obsługi zdarzeń do elementów wejściowych w Twojej HTML modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="3a15b-311">There are a number of directives in AngularJS that add event-handling capabilities to the input elements in your HTML DOM.</span></span> <span data-ttu-id="3a15b-312">Poniżej znajduje się lista zdarzeń, które są wbudowane w AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-312">Below is a list of the events that are built into AngularJS.</span></span>

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
> <span data-ttu-id="3a15b-313">Można dodać własne programy obsługi zdarzeń za pomocą [dyrektywy niestandardowej funkcji w AngularJS](https://docs.angularjs.org/guide/directive).</span><span class="sxs-lookup"><span data-stu-id="3a15b-313">You can add your own event handlers using the [custom directives feature in AngularJS](https://docs.angularjs.org/guide/directive).</span></span>

<span data-ttu-id="3a15b-314">Zobaczmy, jak `ng-click` przewodowej zdarzeń w górę.</span><span class="sxs-lookup"><span data-stu-id="3a15b-314">Let's look at how the `ng-click` event is wired up.</span></span> <span data-ttu-id="3a15b-315">Utwórz nowy plik JavaScript o nazwie *eventHandlerController.js*i Dodaj do niej następujące:</span><span class="sxs-lookup"><span data-stu-id="3a15b-315">Create a new JavaScript file named *eventHandlerController.js*, and add the following to it:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

<span data-ttu-id="3a15b-316">Zwróć uwagę, nowe `sayName` działać w `eventHandlerController` w wierszu 5 powyżej.</span><span class="sxs-lookup"><span data-stu-id="3a15b-316">Notice the new `sayName` function in `eventHandlerController` on line 5 above.</span></span> <span data-ttu-id="3a15b-317">Wykonuje wszystkie metody dla teraz jest przedstawiający ostrzeżenie JavaScript do użytkownika wiadomość powitalna.</span><span class="sxs-lookup"><span data-stu-id="3a15b-317">All the method is doing for now is showing a JavaScript alert to the user with a welcome message.</span></span>

<span data-ttu-id="3a15b-318">Poniższy widok wiąże funkcja kontroler AngularJS zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="3a15b-318">The view below binds a controller function to an AngularJS event.</span></span> <span data-ttu-id="3a15b-319">Wiersz 9 zawiera przycisk, na którym `ng-click` dyrektywy Angular zostały zastosowane.</span><span class="sxs-lookup"><span data-stu-id="3a15b-319">Line 9 has a button on which the `ng-click` Angular directive has been applied.</span></span> <span data-ttu-id="3a15b-320">Wywołuje naszych `sayName` funkcji, która jest dołączona do `$scope` obiekt przekazany do tego widoku.</span><span class="sxs-lookup"><span data-stu-id="3a15b-320">It calls our `sayName` function, which is attached to the `$scope` object passed to this view.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

<span data-ttu-id="3a15b-321">Przykład uruchomionych wykaże, że kontrolera `sayName` funkcja jest wywoływana automatycznie, gdy przycisk zostanie kliknięty.</span><span class="sxs-lookup"><span data-stu-id="3a15b-321">The running example demonstrates that the controller's `sayName` function is called automatically when the button is clicked.</span></span>

![Zdarzenie kliknięcia](angular/_static/events.png)

<span data-ttu-id="3a15b-323">Aby uzyskać więcej szczegółów na dyrektywy programu obsługi zdarzeń wbudowanych AngularJS, upewnij się, head [witryny sieci Web w dokumentacji](https://docs.angularjs.org/api/ng/directive/ngClick) z AngularJS.</span><span class="sxs-lookup"><span data-stu-id="3a15b-323">For more detail on AngularJS built-in event handler directives, be sure to head to the [documentation website](https://docs.angularjs.org/api/ng/directive/ngClick) of AngularJS.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3a15b-324">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3a15b-324">Additional resources</span></span>

* [<span data-ttu-id="3a15b-325">Dokumentacja dyrektywy angular</span><span class="sxs-lookup"><span data-stu-id="3a15b-325">Angular Docs</span></span>](https://docs.angularjs.org)

* [<span data-ttu-id="3a15b-326">Info 2 dyrektywy angular</span><span class="sxs-lookup"><span data-stu-id="3a15b-326">Angular 2 Info</span></span>](https://angular.io/)
