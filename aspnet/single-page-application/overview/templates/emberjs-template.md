---
uid: single-page-application/overview/templates/emberjs-template
title: Szablon EmberJS | Dokumentacja firmy Microsoft
author: xqiu
description: Szablon EmberJS
ms.author: aspnetcontent
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 2488e9c10550bd9b11c675572c70618f6ca4ac05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823780"
---
<a name="emberjs-template"></a><span data-ttu-id="7d62c-103">Szablon EmberJS</span><span class="sxs-lookup"><span data-stu-id="7d62c-103">EmberJS template</span></span>
====================
<span data-ttu-id="7d62c-104">przez [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="7d62c-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="7d62c-105">Szablon EmberJS MVC są zapisywane przez Nathana Totten, Thiago Santos i Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="7d62c-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="7d62c-106">Pobierz szablon EmberJS MVC</span><span class="sxs-lookup"><span data-stu-id="7d62c-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="7d62c-107">Szablon EmberJS SPA został zaprojektowany ułatwią Ci rozpoczęcie pracy szybkiego tworzenia aplikacji sieci web interactive po stronie klienta przy użyciu EmberJS.</span><span class="sxs-lookup"><span data-stu-id="7d62c-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="7d62c-108">"Aplikacja jednostronicowa" (SPA) jest ogólnym terminem dla aplikacji sieci web, która ładuje z pojedynczą stroną HTML, a następnie aktualizuje stronę dynamicznie, zamiast ładowanie nowych stron.</span><span class="sxs-lookup"><span data-stu-id="7d62c-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="7d62c-109">Po załadowaniu strony początkowej SPA komunikuje się z serwerem za pośrednictwem żądań AJAX.</span><span class="sxs-lookup"><span data-stu-id="7d62c-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="7d62c-110">AJAX to nic nowego, ale obecnie ma platformy JavaScript, które ułatwiają tworzenie i zarządzanie nimi dużej zaawansowanych aplikacji SPA.</span><span class="sxs-lookup"><span data-stu-id="7d62c-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="7d62c-111">Ponadto HTML 5 i CSS3 są ułatwia tworzenie rozbudowanych interfejsów użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7d62c-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="7d62c-112">Szablon EmberJS SPA używa [użyciu](http://emberjs.com/) biblioteki JavaScript do obsługi aktualizacji stron AJAX żądań.</span><span class="sxs-lookup"><span data-stu-id="7d62c-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="7d62c-113">Ember.js używa powiązanie danych w celu synchronizowania strony przy użyciu najnowszych danych.</span><span class="sxs-lookup"><span data-stu-id="7d62c-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="7d62c-114">Dzięki temu nie trzeba pisać kodu, który przeprowadzi dane JSON i aktualizuje DOM.</span><span class="sxs-lookup"><span data-stu-id="7d62c-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="7d62c-115">Zamiast tego należy umieścić deklaratywne atrybuty w formacie HTML, informacje Ember.js sposobu prezentowania danych.</span><span class="sxs-lookup"><span data-stu-id="7d62c-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="7d62c-116">Po stronie serwera jest niemal identyczny szablon EmberJS [szablon KnockoutJS SPA](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="7d62c-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="7d62c-117">Używa platformy ASP.NET MVC do udostępniania dokumentów HTML i Web API platformy ASP.NET do obsługi żądań AJAX od klienta.</span><span class="sxs-lookup"><span data-stu-id="7d62c-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="7d62c-118">Aby uzyskać więcej informacji na temat tych aspektów szablonu dotyczą [szablon KnockoutJS](../introduction/knockoutjs-template.md) dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="7d62c-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="7d62c-119">Ten temat koncentruje się na temat różnic między szablon Knockout i szablon EmberJS.</span><span class="sxs-lookup"><span data-stu-id="7d62c-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="7d62c-120">Tworzenie szablonu projektu SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="7d62c-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="7d62c-121">Pobierz i zainstaluj szablon, klikając przycisk Pobierz powyżej.</span><span class="sxs-lookup"><span data-stu-id="7d62c-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="7d62c-122">Może być konieczne ponowne uruchomienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d62c-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="7d62c-123">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="7d62c-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="7d62c-124">W obszarze **Visual C#**, wybierz opcję **Web**.</span><span class="sxs-lookup"><span data-stu-id="7d62c-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="7d62c-125">Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="7d62c-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="7d62c-126">Nadaj projektowi nazwę, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d62c-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="7d62c-127">W **nowy projekt** kreatora wybierz **projekt SPA Ember.js**.</span><span class="sxs-lookup"><span data-stu-id="7d62c-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="7d62c-128">Omówienie szablonu SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="7d62c-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="7d62c-129">Szablon EmberJS używa kombinacji jQuery, Ember.js, Handlebars.js, aby utworzyć płynne interaktywnego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7d62c-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="7d62c-130">Ember.js to biblioteka języka JavaScript, który korzysta ze wzorca MVC po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="7d62c-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="7d62c-131">A *szablonu*robaków napisanych w języku szablonów Handlebars, w tym artykule opisano interfejs użytkownika aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d62c-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="7d62c-132">W trybie wydania [kompilatora Handlebars](https://github.com/Myslik/csharp-ember-handlebars) służy do pakietu i skompilować szablon handlebars.</span><span class="sxs-lookup"><span data-stu-id="7d62c-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="7d62c-133">A *modelu* przechowuje dane aplikacji, która otrzymuje od serwera (listy zadań do wykonania i zadania do wykonania).</span><span class="sxs-lookup"><span data-stu-id="7d62c-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="7d62c-134">A *kontrolera* zapisuje stan aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d62c-134">A *controller* stores application state.</span></span> <span data-ttu-id="7d62c-135">Kontrolery przedstawiają często model danych do odpowiedniego szablonów.</span><span class="sxs-lookup"><span data-stu-id="7d62c-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="7d62c-136">A *widoku* przekształca pierwotne zdarzeń z aplikacji i przekazuje je do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7d62c-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="7d62c-137">A *routera* zarządza stan aplikacji, synchronizacja adresy URL i szablony.</span><span class="sxs-lookup"><span data-stu-id="7d62c-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="7d62c-138">Ponadto biblioteka danych o użyciu może służyć do synchronizowania obiektów JSON (uzyskana z serwera za pomocą interfejsu API RESTful) i modeli klienta.</span><span class="sxs-lookup"><span data-stu-id="7d62c-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="7d62c-139">Szablon EmberJS SPA organizuje skrypty w ośmiu warstwy:</span><span class="sxs-lookup"><span data-stu-id="7d62c-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="7d62c-140">webapi\_adapter.js, webapi\_serializer.js: rozszerza biblioteki danych o użyciu do pracy z interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7d62c-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="7d62c-141">Scripts/helpers.js: Definiuje nowy pomocników Handlebars użyciu.</span><span class="sxs-lookup"><span data-stu-id="7d62c-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="7d62c-142">Scripts/App.js: Utworzy aplikację i konfiguruje karty i serializatora.</span><span class="sxs-lookup"><span data-stu-id="7d62c-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="7d62c-143">Modeleskrypty/aplikacji/\*js: definiuje modeli.</span><span class="sxs-lookup"><span data-stu-id="7d62c-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="7d62c-144">Skrypty/app/widoki/\*js: zawiera definicje widoków.</span><span class="sxs-lookup"><span data-stu-id="7d62c-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="7d62c-145">Skrypty/app/kontrolerów/\*js: definiuje kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="7d62c-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="7d62c-146">Skrypty/app/tras, Scripts/app/router.js: Definiuje trasy.</span><span class="sxs-lookup"><span data-stu-id="7d62c-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="7d62c-147">Szablony /\*.hbs: definiuje szablony handlebars.</span><span class="sxs-lookup"><span data-stu-id="7d62c-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="7d62c-148">Spójrzmy na niektóre z tych skryptów bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="7d62c-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="7d62c-149">Modele</span><span class="sxs-lookup"><span data-stu-id="7d62c-149">Models</span></span>

<span data-ttu-id="7d62c-150">Modele są definiowane w folderze skrypty/app/modeli.</span><span class="sxs-lookup"><span data-stu-id="7d62c-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="7d62c-151">Istnieją dwa pliki modelu: todoItem.js i todoList.js.</span><span class="sxs-lookup"><span data-stu-id="7d62c-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="7d62c-152">**TODO.model.js** definiuje modeli po stronie klienta (przeglądarka) do listy zadań do wykonania.</span><span class="sxs-lookup"><span data-stu-id="7d62c-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="7d62c-153">Istnieją dwie klasy modelu: todoItem i listy zadań.</span><span class="sxs-lookup"><span data-stu-id="7d62c-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="7d62c-154">W użyciu modele są podklasy DS. Model.</span><span class="sxs-lookup"><span data-stu-id="7d62c-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="7d62c-155">Model może mieć właściwości, za pomocą atrybutów:</span><span class="sxs-lookup"><span data-stu-id="7d62c-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="7d62c-156">Modele można zdefiniować relacje z innymi modelami:</span><span class="sxs-lookup"><span data-stu-id="7d62c-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="7d62c-157">Modele mogą mieć obliczane właściwości, powiązanych do innych właściwości:</span><span class="sxs-lookup"><span data-stu-id="7d62c-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="7d62c-158">Modele mogą mieć obserwatora funkcji, które są wywoływane, gdy zmieni się właściwość obserwacji:</span><span class="sxs-lookup"><span data-stu-id="7d62c-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="7d62c-159">Widoki</span><span class="sxs-lookup"><span data-stu-id="7d62c-159">Views</span></span>

<span data-ttu-id="7d62c-160">Widoki są definiowane w folderze skrypty/app/widoków.</span><span class="sxs-lookup"><span data-stu-id="7d62c-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="7d62c-161">Widok dokonuje translacji zdarzeń z aplikacji interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7d62c-161">A view translates events from the application UI.</span></span> <span data-ttu-id="7d62c-162">Program obsługi zdarzeń można wywołania zwrotnego do funkcji kontrolera lub po prostu bezpośrednio wywoływać kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="7d62c-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="7d62c-163">Na przykład następujący kod pochodzi z views/TodoItemEditView.js.</span><span class="sxs-lookup"><span data-stu-id="7d62c-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="7d62c-164">Definiuje zdarzenia obsługi dla pola wprowadzania tekstu.</span><span class="sxs-lookup"><span data-stu-id="7d62c-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="7d62c-165">Kontroler</span><span class="sxs-lookup"><span data-stu-id="7d62c-165">Controller</span></span>

<span data-ttu-id="7d62c-166">Kontrolery są definiowane w folderze skrypty/app/kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="7d62c-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="7d62c-167">Do reprezentowania jednego modelu, należy rozszerzyć `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="7d62c-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="7d62c-168">Kontroler może również reprezentować kolekcję modeli, rozszerzając `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="7d62c-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="7d62c-169">Na przykład TodoListController reprezentuje tablicę `todoList` obiektów.</span><span class="sxs-lookup"><span data-stu-id="7d62c-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="7d62c-170">Kontroler sortowane według Identyfikatora listy zadań, w kolejności malejącej:</span><span class="sxs-lookup"><span data-stu-id="7d62c-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="7d62c-171">Kontroler definiuje funkcję o nazwie `addTodoList`, który tworzy nowe listy zadań i dodaje go do tablicy.</span><span class="sxs-lookup"><span data-stu-id="7d62c-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="7d62c-172">Aby zobaczyć, jak ta funkcja jest wywoływana, otwórz plik szablonu o nazwie todoListTemplate.html w folderze szablonów.</span><span class="sxs-lookup"><span data-stu-id="7d62c-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="7d62c-173">Poniższy kod szablonu wiąże przycisk, aby `addTodoList` funkcji:</span><span class="sxs-lookup"><span data-stu-id="7d62c-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="7d62c-174">Zawiera także kontrolera `error` właściwość, która zawiera komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="7d62c-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="7d62c-175">Poniżej przedstawiono kod szablonu, aby wyświetlić komunikat o błędzie (również w todoListTemplate.html):</span><span class="sxs-lookup"><span data-stu-id="7d62c-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="7d62c-176">Trasy</span><span class="sxs-lookup"><span data-stu-id="7d62c-176">Routes</span></span>

<span data-ttu-id="7d62c-177">Router.js definiuje trasy i szablonu domyślnego, aby wyświetlić, ustawia stan aplikacji oraz odpowiada adresy URL do tras:</span><span class="sxs-lookup"><span data-stu-id="7d62c-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="7d62c-178">TodoListRoute.js powoduje załadowanie danych do TodoListRoute przez przesłonięcie funkcji setupController:</span><span class="sxs-lookup"><span data-stu-id="7d62c-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="7d62c-179">Użyciu używa konwencji nazewnictwa w celu dopasowania adresów URL, nazwy tras, kontrolerów i szablony.</span><span class="sxs-lookup"><span data-stu-id="7d62c-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="7d62c-180">Aby uzyskać więcej informacji, zobacz [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS dokumentację.</span><span class="sxs-lookup"><span data-stu-id="7d62c-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="7d62c-181">Szablony</span><span class="sxs-lookup"><span data-stu-id="7d62c-181">Templates</span></span>

<span data-ttu-id="7d62c-182">Folder szablonów zawiera cztery szablony:</span><span class="sxs-lookup"><span data-stu-id="7d62c-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="7d62c-183">Application.HBS: domyślny szablon, który jest renderowany po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d62c-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="7d62c-184">About.HBS: szablonu trasy "/ informacje".</span><span class="sxs-lookup"><span data-stu-id="7d62c-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="7d62c-185">index.HBS: szablon główny trasy "/".</span><span class="sxs-lookup"><span data-stu-id="7d62c-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="7d62c-186">todoList.hbs: szablonu dla "/ zadań do wykonania" trasy.</span><span class="sxs-lookup"><span data-stu-id="7d62c-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="7d62c-187">\_navbar.HBS: szablon definiuje menu nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7d62c-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="7d62c-188">Szablon aplikacji działa jak strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="7d62c-188">The application template acts like a master page.</span></span> <span data-ttu-id="7d62c-189">Zawiera nagłówek, stopka i "{{ujścia}}" do wstawienia innych szablonów, w zależności od trasy.</span><span class="sxs-lookup"><span data-stu-id="7d62c-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="7d62c-190">Aby uzyskać więcej informacji na temat szablonów aplikacji w użyciu, zobacz [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="7d62c-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="7d62c-191">"/ TodoList" szablon zawiera dwa wyrażenia pętli.</span><span class="sxs-lookup"><span data-stu-id="7d62c-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="7d62c-192">Poza pętla jest `{{#each controller}}`oraz wewnątrz pętli jest `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="7d62c-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="7d62c-193">Poniższy kod przedstawia wbudowaną `Ember.Checkbox` wyświetlić dostosowany `App.TodoItemEditView`i łącze z `deleteTodo` akcji.</span><span class="sxs-lookup"><span data-stu-id="7d62c-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="7d62c-194">`HtmlHelperExtensions` Klasy, zdefiniowanej w Controllers/HtmlHelperExensions.cs, definiuje pomocnika pliki funkcji do pamięci podręcznej i Wstaw szablon, gdy **debugowania** jest ustawiona na **true** w pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="7d62c-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="7d62c-195">Ta funkcja jest wywoływana z pliku widok ASP.NET MVC, które są zdefiniowane w Views/Home/App.cshtml:</span><span class="sxs-lookup"><span data-stu-id="7d62c-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="7d62c-196">Wywołać bez argumentów, funkcja powoduje wyświetlenie wszystkich plików szablonu w folderze szablonów.</span><span class="sxs-lookup"><span data-stu-id="7d62c-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="7d62c-197">Można również określić podfolder lub pliku określonego szablonu.</span><span class="sxs-lookup"><span data-stu-id="7d62c-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="7d62c-198">Gdy **debugowania** jest **false** w pliku Web.config, aplikacja zawiera element pakietu "~/bundles/templates".</span><span class="sxs-lookup"><span data-stu-id="7d62c-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="7d62c-199">Ten element pakietu zostanie dodany w BundleConfig.cs, za pomocą biblioteki kompilatora Handlebars:</span><span class="sxs-lookup"><span data-stu-id="7d62c-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
