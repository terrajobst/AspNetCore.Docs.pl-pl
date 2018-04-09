---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Użyć AJAX w celu dostarczenia aktualizacji dynamicznych | Dokumentacja firmy Microsoft
author: microsoft
description: Implementuje kroku 10 obsługuje zalogowany użytkownikom RSVP zainteresowanie uczestniczący obiad, opartych na technologii Ajax podejście zintegrowane w ramach szczegółów obiad...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7cea3ee2ec52261521941efac484e91a53f6310b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="f762f-103">Użyj AJAX, aby dostarczać aktualizacje dynamiczne</span><span class="sxs-lookup"><span data-stu-id="f762f-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="f762f-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f762f-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="f762f-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="f762f-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="f762f-106">Jest to krok 10 bezpłatnych ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="f762f-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="f762f-107">Implementuje kroku 10 obsługuje zalogowany użytkownikom RSVP zainteresowanie uczestniczący obiad, zintegrowane na stronie szczegółów obiad podejście opartych na technologii Ajax.</span><span class="sxs-lookup"><span data-stu-id="f762f-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="f762f-108">Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.</span><span class="sxs-lookup"><span data-stu-id="f762f-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="f762f-109">NerdDinner krok 10: Akceptuje AJAX włączenie zbędne</span><span class="sxs-lookup"><span data-stu-id="f762f-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="f762f-110">Umożliwia teraz implementuje obsługę zalogowanych użytkowników do RSVP zainteresowanie uczestniczący obiad.</span><span class="sxs-lookup"><span data-stu-id="f762f-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="f762f-111">Firma Microsoft będzie to umożliwić przy użyciu zintegrowanych na stronie szczegółów obiad podejście opartych na technologii AJAX.</span><span class="sxs-lookup"><span data-stu-id="f762f-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="f762f-112">Wskazującą, czy użytkownik jest RSVP'd</span><span class="sxs-lookup"><span data-stu-id="f762f-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="f762f-113">Użytkownicy mogą odwiedzać */Dinners/szczegóły / [identyfikator*] adres URL, aby zobaczyć szczegółowe informacje dotyczące konkretnego obiad:</span><span class="sxs-lookup"><span data-stu-id="f762f-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="f762f-114">Details() metody akcji jest zaimplementowana w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f762f-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="f762f-115">Naszym pierwszym krokiem Obsługa RSVP będzie Dodaj metodę pomocnika "IsUserRegistered(username)" do naszej obiad obiektu (w ramach klasy częściowe Dinner.cs, którą wcześniej budujemy).</span><span class="sxs-lookup"><span data-stu-id="f762f-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="f762f-116">Ta metoda pomocnika zwraca wartość PRAWDA lub FAŁSZ w zależności od tego, czy użytkownik jest obecnie RSVP'd na obiad:</span><span class="sxs-lookup"><span data-stu-id="f762f-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="f762f-117">Firma Microsoft następnie dodaj następujący kod do naszych Details.aspx widoku szablonu, aby wyświetlić odpowiedni komunikat wskazujący, czy użytkownik jest zarejestrowany lub nie dla zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="f762f-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="f762f-118">I teraz, gdy użytkownik odwiedza obiad, są zarejestrowane dla one wyświetlony ten komunikat:</span><span class="sxs-lookup"><span data-stu-id="f762f-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="f762f-119">I po odwiedzeniu obiad, nie są zarejestrowane dla one wyświetlone poniżej komunikat:</span><span class="sxs-lookup"><span data-stu-id="f762f-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="f762f-120">Implementacja metody akcji rejestru</span><span class="sxs-lookup"><span data-stu-id="f762f-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="f762f-121">Teraz Dodajmy funkcje niezbędne do obsługi użytkowników do RSVP na obiad na stronie szczegółów.</span><span class="sxs-lookup"><span data-stu-id="f762f-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="f762f-122">Aby zaimplementować to, utworzymy nową klasę "RSVPController" katalogu \Controllers prawym przyciskiem myszy i wybierając Add -&gt;polecenia menu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f762f-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="f762f-123">Firma Microsoft będzie zaimplementować metodę akcji "Register" w ramach nowej klasy RSVPController, który ma identyfikator na obiad jako argument, pobiera odpowiedni obiekt obiad sprawdza, czy zalogowany użytkownik jest obecnie na liście użytkowników, którzy mają zarejestrowany, i czy nie dodaje obiekt RSVP dla nich:</span><span class="sxs-lookup"><span data-stu-id="f762f-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="f762f-124">Zwróć uwagę, nad jak możemy zwróconego prostego ciągu jako dane wyjściowe metody akcji.</span><span class="sxs-lookup"><span data-stu-id="f762f-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="f762f-125">Firma Microsoft może osadzania tego komunikatu w wyświetlanie szablonu, ale ponieważ jest to mała właśnie użyjemy Content() metody pomocniczej kontrolera klasy podstawowej i zwracany ciąg komunikatu, takich jak powyżej.</span><span class="sxs-lookup"><span data-stu-id="f762f-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="f762f-126">Wywoływanie metody akcji RSVPForEvent za pomocą interfejsu AJAX</span><span class="sxs-lookup"><span data-stu-id="f762f-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="f762f-127">Użyjemy AJAX do wywołania metody akcji rejestrowania z naszych widok szczegółów.</span><span class="sxs-lookup"><span data-stu-id="f762f-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="f762f-128">To wdrożenie jest dość proste.</span><span class="sxs-lookup"><span data-stu-id="f762f-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="f762f-129">Najpierw dodamy dwa odwołania do biblioteki skryptu:</span><span class="sxs-lookup"><span data-stu-id="f762f-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="f762f-130">Biblioteka pierwszym odwołuje się do podstawowej biblioteki skryptu po stronie klienta ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="f762f-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="f762f-131">Ten plik jest około 24k rozmiaru (skompresowane) i zawiera podstawowe funkcje AJAX po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="f762f-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="f762f-132">Drugi biblioteka zawiera funkcje narzędzia, które zintegrować z platformy ASP.NET MVC AJAX pomocnika metod wbudowanych (które użyjemy wkrótce).</span><span class="sxs-lookup"><span data-stu-id="f762f-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="f762f-133">Możemy zaktualizować kodu szablonu widoku, które wcześniej dodano tak, aby zamiast outputing komunikat "Użytkownik nie są zarejestrowane dla tego zdarzenia", firma Microsoft zamiast renderować łącze po naciśnięciu wykona wywołanie AJAX, która wywołuje metodę akcji naszych RSVPForEvent na kontrolera RSVP i RSVPs użytkownika:</span><span class="sxs-lookup"><span data-stu-id="f762f-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="f762f-134">Metodę pomocnika Ajax.ActionLink() powyżej jest zbudowany na platformie ASP.NET MVC i jest podobna do metody pomocniczej Html.ActionLink() z tą różnicą, że zamiast wykonywania standardowych nawigacji powoduje wywołanie AJAX do metody akcji po kliknięciu łącza.</span><span class="sxs-lookup"><span data-stu-id="f762f-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="f762f-135">Jesteśmy powyżej wywołanie metody akcji "Register" controller "RSVP" i przekazywanie DinnerID jako parametru "id" do niego.</span><span class="sxs-lookup"><span data-stu-id="f762f-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="f762f-136">Ostatni parametr AjaxOptions możemy przekazywane wskazuje chęć pobrać zawartości, zwracany przez metodę akcji i zaktualizować HTML &lt;div&gt; elementu na stronie, których identyfikator jest "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="f762f-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="f762f-137">A teraz, gdy użytkownik przechodzi do obiad nie są one zarejestrowane dla jeszcze one wyświetlone łącze do RSVP dla niej:</span><span class="sxs-lookup"><span data-stu-id="f762f-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="f762f-138">Jeśli odbiorca kliknie link "RSVP dla tego zdarzenia" finalizowania będzie wywołanie AJAX do metody akcji rejestru na kontrolerze RSVP i po ukończeniu będzie zobaczy komunikat zaktualizowane, takich jak poniżej:</span><span class="sxs-lookup"><span data-stu-id="f762f-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="f762f-139">Przepustowość sieci i związane z wykonaniem tego wywołania AJAX jest naprawdę lekkie ruchu.</span><span class="sxs-lookup"><span data-stu-id="f762f-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="f762f-140">Gdy użytkownik kliknie łącze "RSVP dla tego zdarzenia", mała POST protokołu HTTP sieci żądań do */Dinners/Register/1* adres URL, który wygląda jak poniżej w sieci:</span><span class="sxs-lookup"><span data-stu-id="f762f-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="f762f-141">I odpowiedzi z naszych rejestru metody akcji jest po prostu:</span><span class="sxs-lookup"><span data-stu-id="f762f-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="f762f-142">To wywołanie lekkie jest szybkie i będzie działać nawet w przypadku wolnej sieci.</span><span class="sxs-lookup"><span data-stu-id="f762f-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="f762f-143">Dodawanie jQuery animacji</span><span class="sxs-lookup"><span data-stu-id="f762f-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="f762f-144">Funkcje AJAX, które wprowadziliśmy działa dobrze i szybko.</span><span class="sxs-lookup"><span data-stu-id="f762f-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="f762f-145">Czasami może się zdarzyć tak szybko, że użytkownik nie zauważyć, że łącze RSVP został zastąpiony nowym tekstem.</span><span class="sxs-lookup"><span data-stu-id="f762f-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="f762f-146">Aby można było nieco bardziej oczywistymi wyniku dodamy prostych animacji, aby zwrócić uwagę na komunikat aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="f762f-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="f762f-147">Domyślny szablon projektu programu ASP.NET MVC zawiera jQuery — biblioteka języka JavaScript znakomity (i bardzo popularny) typu open source, który także jest obsługiwany przez firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f762f-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="f762f-148">jQuery oferuje następujące funkcje, takie jak nieuprzywilejowany HTML DOM wybór i efekty biblioteki.</span><span class="sxs-lookup"><span data-stu-id="f762f-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="f762f-149">Aby użyć jQuery najpierw dodamy skryptu odwołanie do niej.</span><span class="sxs-lookup"><span data-stu-id="f762f-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="f762f-150">Ponieważ zamierzamy się przy użyciu jQuery w różnych miejscach w naszej witrynie, dodamy odwołanie do skryptu w ramach naszych plik strony głównej Site.master tak, aby wszystkie strony może być używany.</span><span class="sxs-lookup"><span data-stu-id="f762f-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="f762f-151">*Porada: Upewnij się, że zainstalowano poprawkę intellisense języka JavaScript dla wersji programu VS 2008 z dodatkiem SP1, która umożliwia bardziej rozbudowane obsługę funkcji intellisense dla JavaScript plików (w tym jQuery). Możesz pobrać go z: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="f762f-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="f762f-152">Kod napisany za pomocą JQuery często używa globalnej "$ ()" metody JavaScript, która pobiera jeden lub więcej elementów HTML za pomocą selektora CSS.</span><span class="sxs-lookup"><span data-stu-id="f762f-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="f762f-153">Na przykład <em>$("#rsvpmsg")</em> wybiera dowolnego elementu HTML z identyfikatorem rsvpmsg, podczas gdy <em>$(".something")</em> wybrać wszystkie elementy z coś, co"CSS nazwę klasy.</span><span class="sxs-lookup"><span data-stu-id="f762f-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="f762f-154">Można również napisać bardziej zaawansowanych zapytań, takie jak "return wszystkie przyciski zaznaczenia opcji" przy użyciu selektora zapytania takie jak: <em>$("dane wejściowe [@type= radiowych] [@checked]")</em>.</span><span class="sxs-lookup"><span data-stu-id="f762f-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="f762f-155">Po wybraniu elementów można wywoływać metod na podjęcie działań, takich jak ich: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="f762f-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="f762f-156">W naszym scenariuszu RSVP zdefiniujemy prostych funkcji JavaScript o nazwie "AnimateRSVPMessage", który wybiera "rsvpmsg" &lt;div&gt; i animuje rozmiar jego zawartości tekstowej.</span><span class="sxs-lookup"><span data-stu-id="f762f-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="f762f-157">Poniższego kodu, rozpoczyna się tekst małe, a następnie przyczyny go zwiększa się w milisekundach 400 przedział czasu:</span><span class="sxs-lookup"><span data-stu-id="f762f-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="f762f-158">Firma Microsoft może następnie przewodowy w górę tej funkcji JavaScript do wywołania po pomyślnym przez przekazanie jej nazwę metody pomocnika naszych Ajax.ActionLink() naszych wywołanie AJAX (za pośrednictwem AjaxOptions "OnSuccess" właściwości zdarzenia):</span><span class="sxs-lookup"><span data-stu-id="f762f-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="f762f-159">I po kliknięciu łącza "RSVP dla tego zdarzenia" i naszych wywołanie AJAX zostało ukończone pomyślnie, zawartości komunikat wysłany wstecz będzie animować wzrostu dużych:</span><span class="sxs-lookup"><span data-stu-id="f762f-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="f762f-160">Oprócz zapewnienia zdarzenie "OnSuccess", obiekt AjaxOptions udostępnia OnBegin, OnFailure i OnComplete zdarzenia, które może obsłużyć (wraz z różnych innych właściwości i opcje przydatne).</span><span class="sxs-lookup"><span data-stu-id="f762f-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="f762f-161">Oczyszczanie - Zrefaktoryzuj limit RSVP widok częściowy</span><span class="sxs-lookup"><span data-stu-id="f762f-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="f762f-162">Nasze szablon widoku szczegółów rozpoczyna uzyskać nieco długi, które nadgodzinach będzie nieco trudniejsze do zrozumienia.</span><span class="sxs-lookup"><span data-stu-id="f762f-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="f762f-163">Aby poprawić czytelność kodu, możemy zakończyć, tworzenie widoku częściowego — RSVPStatus.ascx — które hermetyzują cały kod RSVP widoku dla strony szczegółów.</span><span class="sxs-lookup"><span data-stu-id="f762f-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="f762f-164">Firma Microsoft można to zrobić, klikając prawym przyciskiem myszy w folderze \Views\Dinners, a następnie wybierając polecenie Add -&gt;wyświetlić polecenia menu.</span><span class="sxs-lookup"><span data-stu-id="f762f-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="f762f-165">Firma Microsoft będziesz mieć ona potrwać obiektu obiad jako jego jednoznacznie ViewModel.</span><span class="sxs-lookup"><span data-stu-id="f762f-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="f762f-166">Firma Microsoft może następnie skopiuj/Wklej zawartość RSVP z naszych widok Details.aspx do niego.</span><span class="sxs-lookup"><span data-stu-id="f762f-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="f762f-167">Gdy firma Microsoft to również Utwórzmy innego widoku częściowego — EditAndDeleteLinks.ascx - hermetyzujący naszego edytowanie i usuwanie link widoku kodu.</span><span class="sxs-lookup"><span data-stu-id="f762f-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="f762f-168">Firma Microsoft będzie także dostępna go zająć obiektu obiad jako jego ViewModel jednoznacznie i kopiowania/wklejania logiki edytowanie i usuwanie z naszych widok Details.aspx do niego.</span><span class="sxs-lookup"><span data-stu-id="f762f-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="f762f-169">Szczegóły naszych wyświetlić szablonu, a następnie po prostu obejmują dwa wywołania metody Html.RenderPartial() u dołu:</span><span class="sxs-lookup"><span data-stu-id="f762f-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="f762f-170">Dzięki temu kod czyszczący do odczytu i obsługi.</span><span class="sxs-lookup"><span data-stu-id="f762f-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="f762f-171">Następny krok</span><span class="sxs-lookup"><span data-stu-id="f762f-171">Next Step</span></span>

<span data-ttu-id="f762f-172">Teraz Przyjrzyjmy się jak możemy jeszcze bardziej używać technologii AJAX i dodawanie interaktywnych mapowania obsługi do naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f762f-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f762f-173">[Poprzednie](secure-applications-using-authentication-and-authorization.md)
> [dalej](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="f762f-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
