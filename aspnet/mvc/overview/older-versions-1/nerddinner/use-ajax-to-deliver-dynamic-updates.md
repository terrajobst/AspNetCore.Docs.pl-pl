---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Korzystanie z technologii AJAX w celu dostarczania aktualizacji dynamicznych | Dokumentacja firmy Microsoft
author: microsoft
description: Implementuje kroku 10 obsługuje zalogowanym użytkownikom RSVP zainteresowanie udziałowi obiad, przy użyciu podejścia opartego na technologii Ajax, zintegrowane w ramach szczegółów obiad...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 9f11c4c15c0ac9bab8d53b18a4e07be4b864b2c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825204"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="a2ed7-103">Korzystanie z technologii AJAX w celu dostarczania aktualizacji dynamicznych</span><span class="sxs-lookup"><span data-stu-id="a2ed7-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="a2ed7-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a2ed7-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a2ed7-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="a2ed7-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="a2ed7-106">Jest to krok 10 bezpłatnych [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="a2ed7-107">Implementuje kroku 10 obsługuje zalogowanym użytkownikom RSVP zainteresowanie obiad, przy użyciu podejścia opartego na technologii Ajax, zintegrowane w ramach strony szczegółów obiad bierzesz udział w wydarzeniu.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="a2ed7-108">Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="a2ed7-109">NerdDinner krok 10: Akceptuje AJAX, umożliwiając zbędne</span><span class="sxs-lookup"><span data-stu-id="a2ed7-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="a2ed7-110">Przejdźmy teraz zaimplementować obsługę zalogowanym użytkownikom RSVP zainteresowanie udziałowi obiad.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="a2ed7-111">Firma Microsoft będzie to umożliwić przy użyciu podejścia opartego na technologii AJAX, zintegrowane w ramach strony szczegółów obiad.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="a2ed7-112">Wskazującą, czy użytkownik jest RSVP'd</span><span class="sxs-lookup"><span data-stu-id="a2ed7-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="a2ed7-113">Użytkownicy mogą odwiedzać */Dinners/szczegóły / [identyfikator*] adres URL, aby zobaczyć szczegółowe informacje dotyczące konkretnego obiad:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="a2ed7-114">Details() metody akcji jest zaimplementowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="a2ed7-115">Naszym pierwszym krokiem do zaimplementowania obsługi RSVP zostaną dodane z metody pomocnika "IsUserRegistered(username)" nasze obiektowi obiad (w ramach Dinner.cs klasy częściowej których wcześniej skompilowany).</span><span class="sxs-lookup"><span data-stu-id="a2ed7-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="a2ed7-116">Ta metoda pomocnika zwraca wartość PRAWDA lub FAŁSZ w zależności od tego, czy użytkownik jest aktualnie RSVP'd na obiad:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="a2ed7-117">Firma Microsoft następnie dodaj następujący kod, aby szablon widok Details.aspx na wyświetli odpowiedni komunikat wskazujący, czy użytkownik jest zarejestrowany lub nie dla zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="a2ed7-118">A teraz, gdy użytkownik odwiedzi obiad, są rejestrowane dla zobaczą ten komunikat:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="a2ed7-119">I podczas odwiedzania obiad, nie są zarejestrowane dla zobaczą poniżej komunikat:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="a2ed7-120">Implementacja metody akcji rejestru</span><span class="sxs-lookup"><span data-stu-id="a2ed7-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="a2ed7-121">Teraz Dodajmy funkcje niezbędne do obsługi użytkowników do RSVP na obiad na stronie szczegółów.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="a2ed7-122">Aby wdrożyć to rozwiązanie, utworzymy nową klasę "RSVPController", kliknij prawym przyciskiem myszy w katalogu \Controllers i wybierając Add -&gt;polecenia menu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="a2ed7-123">Firma Microsoft będzie zaimplementować metodę akcji "Register" w nowej klasie RSVPController, która ma identyfikator na obiad jako argument, pobiera odpowiedniego obiektu obiad sprawdza, jeśli zalogowany użytkownik jest obecnie na liście użytkowników, którzy zarejestrowali się go, a nie dodaje obiekt RSVP dla nich:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="a2ed7-124">Zwróć uwagę na powyżej, jak firma Microsoft jest zwracany prostego ciągu jako dane wyjściowe metody akcji.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="a2ed7-125">Firma Microsoft może zostały osadzone tego komunikatu w ramach Wyświetl szablon, ale ponieważ tak mały, po prostu użyjemy Content() metody pomocniczej kontrolera klasy bazowej i zwracany komunikat w formacie ciągu, takich jak powyżej.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="a2ed7-126">Wywoływanie metody akcji RSVPForEvent za pomocą interfejsu AJAX</span><span class="sxs-lookup"><span data-stu-id="a2ed7-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="a2ed7-127">Użyjemy AJAX do wywołania metody akcji rejestru z naszych widok szczegółów.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="a2ed7-128">Zaimplementowanie tego jest całkiem proste.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="a2ed7-129">Najpierw dodamy dwa odwołania do biblioteki skryptu:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="a2ed7-130">Biblioteka pierwszego odwołuje się do podstawowej biblioteki skryptu po stronie klienta ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="a2ed7-131">Ten plik jest około 24k rozmiar (skompresować) i zawiera podstawowe funkcje AJAX po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="a2ed7-132">Drugi biblioteka zawiera funkcje narzędzia, które integrują się z platformy ASP.NET MVC wbudowanych AJAX metody pomocnicze (które będziemy używać wkrótce).</span><span class="sxs-lookup"><span data-stu-id="a2ed7-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="a2ed7-133">Firma Microsoft może, a następnie Wyświetl kod szablonu, które dodaliśmy wcześniej, aby zamiast outputing komunikat "Nie zarejestrowano Cię do tego zdarzenia", firma Microsoft zamiast renderować łącze wypchnięcie aktualizacji wykonuje wywołanie AJAX, która wywołuje naszych RSVPForEvent metody akcji kontrolera RSVP i RSVPs użytkownika:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="a2ed7-134">Metoda pomocnika Ajax.ActionLink() powyżej jest wbudowana w program ASP.NET MVC i jest podobna do metody pomocnika Html.ActionLink(), z tą różnicą, że zamiast wykonywania standardowych nawigacji to sprawia, że wywołanie AJAX do metody akcji po kliknięciu łącza.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="a2ed7-135">Jesteśmy powyżej wywołaniem metody akcji "Zarejestruj" na kontrolerze "RSVP" i przekazując DinnerID jako parametr "id" do niego.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="a2ed7-136">Ostatni parametr AjaxOptions możemy kończy się sukcesem wskazuje, czy chcesz pobrać zawartości, zwracany przez metodę akcji i zaktualizuj kod HTML &lt;div&gt; elementu na stronie, w których identyfikator jest "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="a2ed7-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="a2ed7-137">A teraz gdy użytkownik przegląda obiad nie są one zarejestrowane dla, ale zobaczą łącze do RSVP dla niego:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="a2ed7-138">Po kliknięciu linku "RSVP dla tego zdarzenia" dokonają będzie wywołanie AJAX do metody akcji, zarejestruj się na kontrolerze RSVP i po jej zakończeniu użytkownik zobaczy komunikat zaktualizowane, takich jak poniżej:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="a2ed7-139">Przepustowość sieci i związanych z wywołania AJAX ruch jest bardzo uproszczone.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="a2ed7-140">Gdy użytkownik kliknie link "RSVP dla tego zdarzenia", małe POST protokołu HTTP sieci żądań do */Dinners/Register/1* adresu URL, który wygląda jak poniżej w sieci:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="a2ed7-141">I odpowiedź z naszej rejestru metody akcji jest po prostu:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="a2ed7-142">To wywołanie uproszczone jest szybkie i będzie działać nawet w przypadku wolnej sieci.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="a2ed7-143">Dodawanie jQuery animacji</span><span class="sxs-lookup"><span data-stu-id="a2ed7-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="a2ed7-144">Funkcji interfejsu AJAX, którą wdrożyliśmy działa dobrze i szybkie.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="a2ed7-145">Czasami może się zdarzyć tak szybko, że użytkownik może nie zostać zauważony czy RSVP link został zastąpiony nowym tekstem.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="a2ed7-146">Aby w wyniku nieco bardziej oczywisty możemy dodać prostej animacji, aby zwrócić uwagę na komunikat aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="a2ed7-147">Domyślny szablon projektu platformy ASP.NET MVC zawiera jQuery — Biblioteka JavaScript doskonałą (i bardzo popularna) typu open source, który także jest obsługiwany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="a2ed7-148">jQuery oferuje pewną liczbę funkcji, takich jak nieuprzywilejowany HTML DOM wybór i efekty bibliotekę.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="a2ed7-149">Aby użyć jQuery najpierw dodamy skryptu odwołanie do niej.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="a2ed7-150">Ponieważ firma Microsoft zamierza się przy użyciu jQuery w różnych miejscach w naszej witrynie, dodamy odwołanie do skryptu w ramach naszych plik strony głównej Site.master tak, aby go używać wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="a2ed7-151">*Porada: Upewnij się, że została zainstalowana poprawka funkcji intellisense języka JavaScript dla programu VS 2008 z dodatkiem SP1, która umożliwia bogatszych obsługę funkcji intellisense dla plików JavaScript (w tym jQuery). Możesz pobrać go z: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="a2ed7-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="a2ed7-152">Kod napisany za pomocą technologii JQuery często używa globalnego "$ ()" metody JavaScript, która pobiera jeden lub więcej elementów HTML za pomocą selektora CSS.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="a2ed7-153">Na przykład <em>$("#rsvpmsg")</em> wybiera dowolnego elementu HTML o identyfikatorze rsvpmsg, podczas gdy <em>$(".something")</em> wybrać wszystkie elementy o coś, co"CSS nazwy klasy.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="a2ed7-154">Można także napisać bardziej zaawansowanych zapytań, takich jak "return wszystkie przyciski radiowe zaznaczone" przy użyciu selektora zapytania takiego jak: <em>$("dane wejściowe [@type= radio] [@checked]")</em>.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="a2ed7-155">Po wybraniu elementów może wywoływać metody na nich podjęcia działania, takie jak ich: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="a2ed7-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="a2ed7-156">W naszym scenariuszu RSVP zdefiniujemy prostą funkcję języka JavaScript o nazwie "AnimateRSVPMessage", który wybiera "rsvpmsg" &lt;div&gt; i animuje rozmiar jego zawartości tekstowej.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="a2ed7-157">Poniższego kodu, rozpoczyna się mały tekst, a następnie powoduje, że jego zwiększenie w czasu 400 milisekund:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="a2ed7-158">Firma Microsoft może następnie o komunikacji sieciowej w górę ta funkcja JavaScript do wywołania po naszym wywołanie AJAX pomyślnym ukończeniu przez przekazanie nazwy do naszych metody pomocnika Ajax.ActionLink() (za pośrednictwem AjaxOptions "OnSuccess" właściwości zdarzenia):</span><span class="sxs-lookup"><span data-stu-id="a2ed7-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="a2ed7-159">A teraz po kliknięciu łącza "RSVP dla tego zdarzenia" i naszych wywołanie AJAX zakończy się pomyślnie, zawartości komunikat wysłany wstecz będzie animować i powiększać dużych:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="a2ed7-160">Oprócz zapewniania zdarzenie "OnSuccess", obiekt AjaxOptions udostępnia zdarzenia OnBegin, OnFailure i OnComplete, które może obsłużyć (wraz z różnych innych właściwości i użyteczne opcje).</span><span class="sxs-lookup"><span data-stu-id="a2ed7-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="a2ed7-161">Oczyszczanie — Zrefaktoryzuj się widok częściowy Odpowiedz na zaproszenie</span><span class="sxs-lookup"><span data-stu-id="a2ed7-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="a2ed7-162">Szablon widoku szczegółów rozpoczyna pobieranie nieco długie, które nadgodzinach, spowoduje to nieco trudniejsze do zrozumienia.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="a2ed7-163">Aby poprawić czytelność kodu, umożliwia zakończenie się przez utworzenie widoku częściowego — RSVPStatus.ascx — która hermetyzacji wszystkich RSVP Wyświetl kod dla naszej stronie szczegółów.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="a2ed7-164">Możemy to zrobić przez kliknięcie prawym przyciskiem myszy \Views\Dinners folder, a następnie wybierając polecenie Add -&gt;wyświetlać polecenia menu.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="a2ed7-165">Odpowiemy od niego obiekt obiad jako jego silnie typizowane ViewModel.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="a2ed7-166">Firma Microsoft może następnie kopiowania/wklejania zawartości RSVP z naszych widok Details.aspx tę sytuację.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="a2ed7-167">Gdy dotychczasowej, również Utwórz innego widoku częściowego — EditAndDeleteLinks.ascx —, który hermetyzuje naszych edytowania i usuwania łącze Wyświetl kod.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="a2ed7-168">Zostanie też przeprowadzona pobrać obiekt obiad, jako jego ViewModel silnie typizowane i kopiowania/wklejania logiki edytowanie i usuwanie z naszych widok Details.aspx tę sytuację.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="a2ed7-169">Nasze Szczegóły szablonu można wyświetlić, a następnie po prostu Dołącz dwa wywołania metody Html.RenderPartial() u dołu:</span><span class="sxs-lookup"><span data-stu-id="a2ed7-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="a2ed7-170">To sprawia, że kod czyszczenia do odczytywania i obsługa.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="a2ed7-171">Następny krok</span><span class="sxs-lookup"><span data-stu-id="a2ed7-171">Next Step</span></span>

<span data-ttu-id="a2ed7-172">Teraz Przyjrzyjmy jak możemy dalsze korzystanie z technologii AJAX i dodanie obsługi interaktywnych mapowania do naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a2ed7-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a2ed7-173">[Poprzednie](secure-applications-using-authentication-and-authorization.md)
> [dalej](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="a2ed7-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
