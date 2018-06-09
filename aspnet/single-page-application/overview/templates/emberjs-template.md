---
uid: single-page-application/overview/templates/emberjs-template
title: Szablon EmberJS | Dokumentacja firmy Microsoft
author: xqiu
description: Szablon EmberJS
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566345"
---
<a name="emberjs-template"></a><span data-ttu-id="26dc3-103">Szablon EmberJS</span><span class="sxs-lookup"><span data-stu-id="26dc3-103">EmberJS template</span></span>
====================
<span data-ttu-id="26dc3-104">przez [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="26dc3-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="26dc3-105">Szablon MVC EmberJS została napisana przez Nathan Totten, Thiago Santos i Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="26dc3-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="26dc3-106">Pobieranie szablonu EmberJS MVC</span><span class="sxs-lookup"><span data-stu-id="26dc3-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="26dc3-107">Szablon EmberJS SPA został zaprojektowany ułatwiających rozpoczęcie pracy szybkie opracowywanie aplikacji sieci web po stronie klienta interactive przy użyciu EmberJS.</span><span class="sxs-lookup"><span data-stu-id="26dc3-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="26dc3-108">"Jednostronicowej aplikacji" (SPA) jest ogólny termin dla aplikacji sieci web, który ładuje pojedynczej strony HTML, a następnie aktualizuje stronę dynamicznie, zamiast ładowania nowych stron.</span><span class="sxs-lookup"><span data-stu-id="26dc3-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="26dc3-109">Po załadowaniu stronę początkową aplikacja JEDNOSTRONICOWA komunikuje się z serwerem za pośrednictwem żądania AJAX.</span><span class="sxs-lookup"><span data-stu-id="26dc3-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="26dc3-110">AJAX ma nowych danych, ale obecnie istnieją struktury języka JavaScript, ułatwiające tworzenie i obsługa dużej aplikacji JEDNOSTRONICOWEJ zaawansowane.</span><span class="sxs-lookup"><span data-stu-id="26dc3-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="26dc3-111">Ponadto HTML 5 i CSS3 są ułatwiając tworzenie rozbudowanych UI.</span><span class="sxs-lookup"><span data-stu-id="26dc3-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="26dc3-112">Szablon SPA EmberJS używa [Członkowskimi](http://emberjs.com/) biblioteka języka JavaScript do obsługi aktualizacji strony z żądania AJAX.</span><span class="sxs-lookup"><span data-stu-id="26dc3-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="26dc3-113">Ember.js używa powiązanie danych w celu synchronizowania strony przy użyciu najnowszych danych.</span><span class="sxs-lookup"><span data-stu-id="26dc3-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="26dc3-114">Dzięki temu nie trzeba pisać kod, który przeprowadzi Cię przez dane JSON i aktualizuje modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="26dc3-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="26dc3-115">Zamiast tego należy umieścić deklaratywne atrybuty HTML, który poinformuje Ember.js jak przedstawiają dane.</span><span class="sxs-lookup"><span data-stu-id="26dc3-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="26dc3-116">Po stronie serwera jest niemal identyczny szablonu EmberJS [szablonu SPA elementami KnockoutJS](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="26dc3-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="26dc3-117">ASP.NET MVC jest używany do obsługi dokumentów HTML i ASP.NET Web API do obsługi żądań AJAX z klienta.</span><span class="sxs-lookup"><span data-stu-id="26dc3-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="26dc3-118">Aby uzyskać więcej informacji na temat tych aspektów szablonu dotyczą [szablonu elementami KnockoutJS](../introduction/knockoutjs-template.md) dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="26dc3-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="26dc3-119">Ten temat koncentruje się na temat różnic między szablon Knockout i szablon EmberJS.</span><span class="sxs-lookup"><span data-stu-id="26dc3-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="26dc3-120">Tworzenie projektu szablonu SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="26dc3-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="26dc3-121">Pobierz i zainstaluj szablonu, klikając przycisk Pobierz powyżej.</span><span class="sxs-lookup"><span data-stu-id="26dc3-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="26dc3-122">Może być konieczne ponowne uruchomienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="26dc3-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="26dc3-123">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="26dc3-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="26dc3-124">W obszarze **Visual C#**, wybierz pozycję **Web**.</span><span class="sxs-lookup"><span data-stu-id="26dc3-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="26dc3-125">Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="26dc3-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="26dc3-126">Nazwij projekt i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="26dc3-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="26dc3-127">W **nowy projekt** kreatora wybierz **Ember.js SPA projektu**.</span><span class="sxs-lookup"><span data-stu-id="26dc3-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="26dc3-128">Omówienie szablonu SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="26dc3-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="26dc3-129">Szablon EmberJS używa kombinacji jQuery, Ember.js, Handlebars.js, aby utworzyć smooth, interaktywnego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="26dc3-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="26dc3-130">Ember.js jest biblioteka języka JavaScript, który korzysta ze wzorca MVC po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="26dc3-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="26dc3-131">A *szablonu*robaków napisanych w języku tworzenia szablonów odległość, w tym artykule opisano interfejs użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="26dc3-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="26dc3-132">W trybie wersji [kompilatora odległość](https://github.com/Myslik/csharp-ember-handlebars) służy do pakietu i skompilować odległość szablonu.</span><span class="sxs-lookup"><span data-stu-id="26dc3-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="26dc3-133">A *modelu* przechowuje dane aplikacji, która otrzymuje od serwera (ToDo list i zadań do wykonania).</span><span class="sxs-lookup"><span data-stu-id="26dc3-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="26dc3-134">A *kontrolera* przechowuje stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="26dc3-134">A *controller* stores application state.</span></span> <span data-ttu-id="26dc3-135">Kontrolery często zawierają dane modelu do odpowiednich szablonów.</span><span class="sxs-lookup"><span data-stu-id="26dc3-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="26dc3-136">A *widoku* tłumaczy pierwotnych zdarzeń z aplikacji i przekazuje je do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="26dc3-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="26dc3-137">A *router* zarządza stanem aplikacji, synchronizacja szablony i adresy URL.</span><span class="sxs-lookup"><span data-stu-id="26dc3-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="26dc3-138">Ponadto biblioteka Członkowskimi danych można zsynchronizować obiektów JSON (uzyskane z serwerem za pośrednictwem interfejsu API RESTful) i modele klienta.</span><span class="sxs-lookup"><span data-stu-id="26dc3-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="26dc3-139">Szablon EmberJS SPA organizuje skrypty w ośmiu warstwy:</span><span class="sxs-lookup"><span data-stu-id="26dc3-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="26dc3-140">webapi\_adapter.js, webapi\_serializer.js: rozszerza biblioteki Członkowskimi danych do pracy z interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="26dc3-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="26dc3-141">Scripts/helpers.js: Określa nowe pomocników odległość Członkowskimi.</span><span class="sxs-lookup"><span data-stu-id="26dc3-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="26dc3-142">Scripts/App.js: Tworzenie aplikacji i konfiguruje serializator i karty.</span><span class="sxs-lookup"><span data-stu-id="26dc3-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="26dc3-143">Skrypty/app/modeli/\*js: definiuje modeli.</span><span class="sxs-lookup"><span data-stu-id="26dc3-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="26dc3-144">Widokówskryptów/app/\*js: zawiera definicje widoków.</span><span class="sxs-lookup"><span data-stu-id="26dc3-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="26dc3-145">Skrypty/app/kontrolerów/\*js: definiuje kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="26dc3-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="26dc3-146">Skrypty/app/tras, Scripts/app/router.js: Definiuje trasy.</span><span class="sxs-lookup"><span data-stu-id="26dc3-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="26dc3-147">Szablony /\*.hbs: definiuje szablony odległość.</span><span class="sxs-lookup"><span data-stu-id="26dc3-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="26dc3-148">Oto niektóre z tych skryptów bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="26dc3-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="26dc3-149">Modele</span><span class="sxs-lookup"><span data-stu-id="26dc3-149">Models</span></span>

<span data-ttu-id="26dc3-150">Modele definiuje się w folderze skryptów/app/modeli.</span><span class="sxs-lookup"><span data-stu-id="26dc3-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="26dc3-151">Istnieją dwa pliki modelu: todoItem.js i todoList.js.</span><span class="sxs-lookup"><span data-stu-id="26dc3-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="26dc3-152">**TODO.model.js** definiuje modeli po stronie klienta (przeglądarki) do listy zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="26dc3-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="26dc3-153">Istnieją dwie klasy modelu: todoItem i listy zadań.</span><span class="sxs-lookup"><span data-stu-id="26dc3-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="26dc3-154">W Członkowskimi modele są podklasy DS. Model.</span><span class="sxs-lookup"><span data-stu-id="26dc3-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="26dc3-155">Model może mieć właściwości z atrybutami:</span><span class="sxs-lookup"><span data-stu-id="26dc3-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="26dc3-156">Modele można zdefiniować relacji z innymi modelami:</span><span class="sxs-lookup"><span data-stu-id="26dc3-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="26dc3-157">Modele mogą mieć obliczana właściwości, które wiążą się do innych właściwości:</span><span class="sxs-lookup"><span data-stu-id="26dc3-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="26dc3-158">Modele może mieć obserwatora funkcji, które są wywoływane, gdy właściwość obserwowanych:</span><span class="sxs-lookup"><span data-stu-id="26dc3-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="26dc3-159">Widoki</span><span class="sxs-lookup"><span data-stu-id="26dc3-159">Views</span></span>

<span data-ttu-id="26dc3-160">Widoki są definiowane w folderze skryptów/app/widoków.</span><span class="sxs-lookup"><span data-stu-id="26dc3-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="26dc3-161">Widok tłumaczy zdarzeń z aplikacji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="26dc3-161">A view translates events from the application UI.</span></span> <span data-ttu-id="26dc3-162">Program obsługi zdarzeń można wywołania zwrotnego funkcje kontrolera lub po prostu bezpośrednio wywołać kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="26dc3-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="26dc3-163">Na przykład następujący kod jest z views/TodoItemEditView.js.</span><span class="sxs-lookup"><span data-stu-id="26dc3-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="26dc3-164">Definiuje zdarzenia obsługi dla pola wejściowego tekstu.</span><span class="sxs-lookup"><span data-stu-id="26dc3-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="26dc3-165">Kontrolera</span><span class="sxs-lookup"><span data-stu-id="26dc3-165">Controller</span></span>

<span data-ttu-id="26dc3-166">Kontrolery są definiowane w folderze skryptów/app/kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="26dc3-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="26dc3-167">Do reprezentowania pojedynczego modelu, należy rozszerzyć `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="26dc3-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="26dc3-168">Kontroler może również reprezentować kolekcji modeli przez rozszerzenie `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="26dc3-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="26dc3-169">Na przykład TodoListController reprezentuje tablicę `todoList` obiektów.</span><span class="sxs-lookup"><span data-stu-id="26dc3-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="26dc3-170">Kontroler sortowane według Identyfikatora listy zadań, w kolejności malejącej:</span><span class="sxs-lookup"><span data-stu-id="26dc3-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="26dc3-171">Kontroler definiuje funkcję o nazwie `addTodoList`, która tworzy nowy todoList i dodaje go do tablicy.</span><span class="sxs-lookup"><span data-stu-id="26dc3-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="26dc3-172">Aby zobaczyć, jak ta funkcja jest wywoływana, otwórz plik szablonu o nazwie todoListTemplate.html w folderze szablonów.</span><span class="sxs-lookup"><span data-stu-id="26dc3-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="26dc3-173">Poniższy kod szablonu wiąże przycisk, aby `addTodoList` funkcji:</span><span class="sxs-lookup"><span data-stu-id="26dc3-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="26dc3-174">Zawiera także kontrolerem `error` właściwość, która zawiera komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="26dc3-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="26dc3-175">Oto kod szablonu, aby wyświetlić komunikat o błędzie (również w todoListTemplate.html):</span><span class="sxs-lookup"><span data-stu-id="26dc3-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="26dc3-176">Trasy</span><span class="sxs-lookup"><span data-stu-id="26dc3-176">Routes</span></span>

<span data-ttu-id="26dc3-177">Router.js definiuje tras i domyślny szablon do wyświetlenia, ustawia stan aplikacji i pasuje do tras adresów URL:</span><span class="sxs-lookup"><span data-stu-id="26dc3-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="26dc3-178">TodoListRoute.js ładuje dane dla TodoListRoute przez zastąpienie funkcja setupController:</span><span class="sxs-lookup"><span data-stu-id="26dc3-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="26dc3-179">Członkowskimi używa konwencji nazewnictwa w celu dopasowania adresy URL, nazwy tras, kontrolerów i szablony.</span><span class="sxs-lookup"><span data-stu-id="26dc3-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="26dc3-180">Aby uzyskać więcej informacji, zobacz [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) w dokumentacji EmberJS.</span><span class="sxs-lookup"><span data-stu-id="26dc3-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="26dc3-181">Szablony</span><span class="sxs-lookup"><span data-stu-id="26dc3-181">Templates</span></span>

<span data-ttu-id="26dc3-182">Folder szablonów zawiera cztery szablony:</span><span class="sxs-lookup"><span data-stu-id="26dc3-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="26dc3-183">Application.HBS: domyślny szablon, który jest renderowany po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="26dc3-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="26dc3-184">About.HBS: szablonu trasy "/ około".</span><span class="sxs-lookup"><span data-stu-id="26dc3-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="26dc3-185">index.HBS: szablon dla głównego trasy "/".</span><span class="sxs-lookup"><span data-stu-id="26dc3-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="26dc3-186">todoList.hbs: szablonu dla "/ todo" trasy.</span><span class="sxs-lookup"><span data-stu-id="26dc3-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="26dc3-187">\_navbar.HBS: szablon definiuje menu nawigacji.</span><span class="sxs-lookup"><span data-stu-id="26dc3-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="26dc3-188">Szablon aplikacji działa jak strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="26dc3-188">The application template acts like a master page.</span></span> <span data-ttu-id="26dc3-189">Zawiera nagłówek, stopka i "{{gniazda}}" do wstawienia innych szablonów w zależności od trasy.</span><span class="sxs-lookup"><span data-stu-id="26dc3-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="26dc3-190">Aby uzyskać więcej informacji na temat szablonów aplikacji w Członkowskimi, zobacz [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="26dc3-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="26dc3-191">"/ TodoList" szablon zawiera dwóch wyrażeń pętli.</span><span class="sxs-lookup"><span data-stu-id="26dc3-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="26dc3-192">Pętla poza `{{#each controller}}`oraz wewnątrz pętli jest `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="26dc3-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="26dc3-193">Poniższy kod przedstawia wbudowaną `Ember.Checkbox` wyświetlić dostosowany `App.TodoItemEditView`i link `deleteTodo` akcji.</span><span class="sxs-lookup"><span data-stu-id="26dc3-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="26dc3-194">`HtmlHelperExtensions` Pomocnika definiuje klasa zdefiniowana w Controllers/HtmlHelperExensions.cs, funkcja do buforowania i Wstaw szablon pliki, kiedy **debugowania** ma ustawioną wartość **true** w pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="26dc3-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="26dc3-195">Ta funkcja jest wywoływana z pliku widoku programu ASP.NET MVC zdefiniowane w Views/Home/App.cshtml:</span><span class="sxs-lookup"><span data-stu-id="26dc3-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="26dc3-196">Wywołać bez argumentów funkcji renderuje wszystkie pliki szablonów w folderze szablonów.</span><span class="sxs-lookup"><span data-stu-id="26dc3-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="26dc3-197">Można również określić podfolderu lub pliku określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="26dc3-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="26dc3-198">Gdy **debugowania** jest **false** w pliku Web.config, aplikacja zawiera element pakietu "~/bundles/templates".</span><span class="sxs-lookup"><span data-stu-id="26dc3-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="26dc3-199">Ten element pakiet został dodany w BundleConfig.cs za pomocą biblioteki kompilatora odległość:</span><span class="sxs-lookup"><span data-stu-id="26dc3-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
