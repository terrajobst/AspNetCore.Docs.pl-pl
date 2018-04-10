---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: ASP.NET MVC widoków — omówienie (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: Co to jest widok ASP.NET MVC i jak go różni się od strony HTML? W tym samouczku Stephen Walther stanowi wprowadzenie do widoków i pokazuje, jak można t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5217994168ebac32a4a9754ae09e63e120804813
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-views-overview-c"></a><span data-ttu-id="587e7-104">ASP.NET MVC widoków — omówienie (C#)</span><span class="sxs-lookup"><span data-stu-id="587e7-104">ASP.NET MVC Views Overview (C#)</span></span>
====================
<span data-ttu-id="587e7-105">przez [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="587e7-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="587e7-106">Co to jest widok ASP.NET MVC i jak go różni się od strony HTML?</span><span class="sxs-lookup"><span data-stu-id="587e7-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="587e7-107">W tym samouczku Stephen Walther stanowi wprowadzenie do widoków i pokazuje, jak można korzystać z danych widoku i pomocników HTML w widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="587e7-108">Celem tego samouczka jest dostarczają krótkie wprowadzenie do platformy ASP.NET MVC widoków danych widoku i pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="587e7-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="587e7-109">Na koniec tego samouczka należy wiedzieć, jak utworzyć nowe widoki, przekazać dane z kontrolera do widoku i używać pomocników HTML do generowania zawartości w widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="587e7-110">Opis widoków</span><span class="sxs-lookup"><span data-stu-id="587e7-110">Understanding Views</span></span>

<span data-ttu-id="587e7-111">Dla programu ASP.NET lub Active Server Pages platformy ASP.NET MVC nie zawiera wszystko, co odpowiada bezpośrednio do strony.</span><span class="sxs-lookup"><span data-stu-id="587e7-111">For ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="587e7-112">W aplikacji platformy ASP.NET MVC nie istnieje strona na dysku, który odpowiada ścieżki w adresie URL można wpisać w pasku adresu przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="587e7-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="587e7-113">Najbliższy co do strony w aplikacji platformy ASP.NET MVC to element o nazwie *widoku*.</span><span class="sxs-lookup"><span data-stu-id="587e7-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="587e7-114">Aplikacja przeglądarki przychodzące żądanie jest mapowane na akcji kontrolera ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="587e7-114">ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="587e7-115">Akcja kontrolera może zwrócić widok.</span><span class="sxs-lookup"><span data-stu-id="587e7-115">A controller action might return a view.</span></span> <span data-ttu-id="587e7-116">Jednak akcji kontrolera może wykonywać innego typu akcji, takie jak przekierowywanie należy do innej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="587e7-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="587e7-117">Wyświetlanie listy 1 zawiera proste kontrolerze o nazwie HomeController.</span><span class="sxs-lookup"><span data-stu-id="587e7-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="587e7-118">HomeController przedstawia o nazwie indeks() i Details() dwóch akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="587e7-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="587e7-119">**Wyświetlanie listy 1 - HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="587e7-119">**Listing 1 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

<span data-ttu-id="587e7-120">Wprowadź następujący adres URL na pasku adresu przeglądarki, można wywołać pierwszą akcją akcji indeks():</span><span class="sxs-lookup"><span data-stu-id="587e7-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="587e7-121">/ Głównej/indeksu</span><span class="sxs-lookup"><span data-stu-id="587e7-121">/Home/Index</span></span>

<span data-ttu-id="587e7-122">Drugą akcję, akcja Details() można wywoływać, wpisując ten adres w przeglądarce:</span><span class="sxs-lookup"><span data-stu-id="587e7-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="587e7-123">/ Głównej/szczegółów</span><span class="sxs-lookup"><span data-stu-id="587e7-123">/Home/Details</span></span>

<span data-ttu-id="587e7-124">Akcja indeks() zwraca widok.</span><span class="sxs-lookup"><span data-stu-id="587e7-124">The Index() action returns a view.</span></span> <span data-ttu-id="587e7-125">Większość akcji, które możesz utworzyć zwróci widoków.</span><span class="sxs-lookup"><span data-stu-id="587e7-125">Most actions that you create will return views.</span></span> <span data-ttu-id="587e7-126">Jednak akcja może zwrócić innych typów wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="587e7-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="587e7-127">Na przykład akcja Details() zwraca RedirectToActionResult, który przekierowuje żądania przychodzące do działania indeks().</span><span class="sxs-lookup"><span data-stu-id="587e7-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="587e7-128">Akcja indeks() zawiera jednym następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="587e7-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="587e7-129">View();</span><span class="sxs-lookup"><span data-stu-id="587e7-129">View();</span></span>

<span data-ttu-id="587e7-130">Ten wiersz kodu zwraca widok, który musi znajdować się w następującej ścieżce na serwerze sieci web:</span><span class="sxs-lookup"><span data-stu-id="587e7-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="587e7-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="587e7-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="587e7-132">Ścieżka do widoku jest wywnioskowany na podstawie nazwy kontrolera i nazwy akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="587e7-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="587e7-133">Jeśli wolisz, może być jawne dotyczące widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="587e7-134">Następujący wiersz kodu zwraca widok o nazwie Krzysztof:</span><span class="sxs-lookup"><span data-stu-id="587e7-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="587e7-135">Widok (Krzysztof);</span><span class="sxs-lookup"><span data-stu-id="587e7-135">View( Fred );</span></span>

<span data-ttu-id="587e7-136">Po wykonaniu ten wiersz kodu widoku jest zwracana z następującej ścieżki:</span><span class="sxs-lookup"><span data-stu-id="587e7-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="587e7-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="587e7-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="587e7-138">Jeśli planujesz tworzenia testów jednostkowych dla aplikacji ASP.NET MVC istnieje dobrze jawne o nazwy widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="587e7-139">W ten sposób można utworzyć testu jednostkowego, aby sprawdzić widok oczekiwanego zwróciło akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="587e7-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="587e7-140">Dodawanie zawartości do widoku</span><span class="sxs-lookup"><span data-stu-id="587e7-140">Adding Content to a View</span></span>

<span data-ttu-id="587e7-141">Widok jest standardem (dokument HTML, który może zawierać skryptów X).</span><span class="sxs-lookup"><span data-stu-id="587e7-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="587e7-142">Skrypty umożliwia dodanie do widoku zawartości dynamicznej.</span><span class="sxs-lookup"><span data-stu-id="587e7-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="587e7-143">Na przykład widok wyświetlania 2 Wyświetla bieżącą datę i godzinę.</span><span class="sxs-lookup"><span data-stu-id="587e7-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="587e7-144">**Wyświetlanie listy 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="587e7-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

<span data-ttu-id="587e7-145">Zwróć uwagę, że strony HTML w wyświetlania 2 zawiera następujący skrypt:</span><span class="sxs-lookup"><span data-stu-id="587e7-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="587e7-146">&lt;% Response.Write(DateTime.Now);%&gt;</span><span class="sxs-lookup"><span data-stu-id="587e7-146">&lt;% Response.Write(DateTime.Now);%&gt;</span></span>

<span data-ttu-id="587e7-147">Użyj ograniczników skryptu &lt;% i %&gt; aby oznaczyć początek i koniec skryptu.</span><span class="sxs-lookup"><span data-stu-id="587e7-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="587e7-148">Ten skrypt jest napisany w języku C#.</span><span class="sxs-lookup"><span data-stu-id="587e7-148">This script is written in C#.</span></span> <span data-ttu-id="587e7-149">Wyświetla bieżącą datę i godzinę, wywołując metodę metody Response.Write() do renderowania zawartości w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="587e7-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="587e7-150">Ograniczniki skryptu &lt;% i %&gt; może służyć do wykonywania instrukcji jeden lub więcej.</span><span class="sxs-lookup"><span data-stu-id="587e7-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="587e7-151">Ponieważ często wywołania metody Response.Write(), firma Microsoft umożliwia skrót dla wywołania metody Response.Write() metody.</span><span class="sxs-lookup"><span data-stu-id="587e7-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="587e7-152">Widok w 3 wyświetlania używa ograniczniki &lt;% = i %&gt; jako skrót do wywołania metody Response.Write().</span><span class="sxs-lookup"><span data-stu-id="587e7-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="587e7-153">**Wyświetlanie listy 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="587e7-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

<span data-ttu-id="587e7-154">Można użyć dowolnego języka .NET do generowania zawartości dynamicznej w widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="587e7-155">Zwykle pojawi Użyj Visual Basic .NET lub C# do zapisu, widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="587e7-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="587e7-156">Wyświetl zawartość przy użyciu pomocników HTML</span><span class="sxs-lookup"><span data-stu-id="587e7-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="587e7-157">Aby ułatwić dodawanie zawartości do widoku, można korzystać z coś o nazwie *pomocnika kodu HTML*.</span><span class="sxs-lookup"><span data-stu-id="587e7-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="587e7-158">Pomocnika kodu HTML, zazwyczaj jest to metoda, która generuje ciąg.</span><span class="sxs-lookup"><span data-stu-id="587e7-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="587e7-159">Pomocników HTML służy do generowania standardowych elementów HTML, takich jak pola tekstowe, łącza, listy rozwijane i pola listy.</span><span class="sxs-lookup"><span data-stu-id="587e7-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="587e7-160">Na przykład widok listę 4 wykorzystuje trzy pomocników HTML — pomocników BeginForm(), TextBox() i Password() — można wygenerować identyfikatora logowania formularza (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="587e7-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="587e7-161">**Wyświetlanie listy 4 – \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="587e7-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


<span data-ttu-id="587e7-162">[![Okno dialogowe nowego projektu](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="587e7-162">[![The New Project dialog box](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="587e7-163">**Rysunek 01**: standardowy formularz logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-views-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="587e7-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="587e7-164">Wszystkie metody pomocników HTML są nazywane we właściwości Html widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="587e7-165">Na przykład że pole tekstowe przez wywołanie metody Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="587e7-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="587e7-166">Powiadomienie, że używasz ograniczników skryptu &lt;% = i %&gt; podczas wywoływania metody pomocników Html.TextBox() i Html.Password().</span><span class="sxs-lookup"><span data-stu-id="587e7-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="587e7-167">Te pomocników po prostu zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="587e7-167">These helpers simply return a string.</span></span> <span data-ttu-id="587e7-168">Należy wywołać metody Response.Write() do renderowania ciąg do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="587e7-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="587e7-169">Przy użyciu metody pomocnika kodu HTML jest opcjonalna.</span><span class="sxs-lookup"><span data-stu-id="587e7-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="587e7-170">One ułatwiać pracę dzięki zmniejszeniu ilości HTML i skryptu, który należy napisać.</span><span class="sxs-lookup"><span data-stu-id="587e7-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="587e7-171">Wyświetl listę 5 renderuje dokładnie tego samego formularza jako widok na listę 4 bez użycia pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="587e7-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="587e7-172">**Wyświetlanie listy 5 — \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="587e7-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

<span data-ttu-id="587e7-173">Istnieje również możliwość utworzenia własnych pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="587e7-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="587e7-174">Na przykład można utworzyć metody pomocnika GridView(), która automatycznie wyświetla zestaw rekordów bazy danych w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="587e7-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="587e7-175">W tym temacie firma Microsoft Eksplorowanie w samouczku **Tworzenie niestandardowych pomocników HTML**.</span><span class="sxs-lookup"><span data-stu-id="587e7-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="587e7-176">Przy użyciu widoku danych do przekazywania danych do widoku</span><span class="sxs-lookup"><span data-stu-id="587e7-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="587e7-177">Wyświetlanie danych służy do przekazywania danych z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="587e7-178">Należy traktować danych widoku, takich jak pakiet, który możesz wysłać pocztą.</span><span class="sxs-lookup"><span data-stu-id="587e7-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="587e7-179">Należy wysłać wszystkie dane przekazane z kontrolera do widoku przy użyciu tego pakietu.</span><span class="sxs-lookup"><span data-stu-id="587e7-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="587e7-180">Na przykład kontroler na wyświetlanie 6 dodaje komunikat, aby wyświetlić dane.</span><span class="sxs-lookup"><span data-stu-id="587e7-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="587e7-181">**Wyświetlanie listy 6 - ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="587e7-181">**Listing 6 - ProductController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

<span data-ttu-id="587e7-182">Kontroler właściwości ViewData reprezentuje kolekcję par nazw i wartości.</span><span class="sxs-lookup"><span data-stu-id="587e7-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="587e7-183">Wyświetlanie listy 6 metody indeks() dodaje element do widoku zbierania danych o nazwie komunikat z wartością Hello World!.</span><span class="sxs-lookup"><span data-stu-id="587e7-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="587e7-184">Gdy widok jest zwracany przez metodę indeks(), dane widoku są automatycznie przekazywane do widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="587e7-185">Widok w wyświetlania 7 pobiera wiadomość z danymi widoku i renderuje komunikat do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="587e7-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="587e7-186">**Wyświetlanie listy 7 — \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="587e7-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

<span data-ttu-id="587e7-187">Należy zauważyć, że widok wykorzystuje metody pomocnika kodu HTML Html.Encode() podczas renderowania komunikatu.</span><span class="sxs-lookup"><span data-stu-id="587e7-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="587e7-188">Pomocnik kodu HTML Html.Encode() koduje znaki specjalne, takie jak &lt; i &gt; na znaki, które są bezpieczne wyświetlić na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="587e7-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="587e7-189">Zawsze, gdy renderowania zawartości, którą użytkownik prześle do witryny sieci Web, należy zakodować zawartości, aby zapobiec atakom iniekcji JavaScript.</span><span class="sxs-lookup"><span data-stu-id="587e7-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="587e7-190">(Ponieważ utworzone komunikat nad ProductController, możemy ADAM t naprawdę potrzebne do kodowania komunikatu.</span><span class="sxs-lookup"><span data-stu-id="587e7-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="587e7-191">Jednak jest dobrym nawyk zawsze wywołać metody Html.Encode(), podczas wyświetlania zawartości pobranej z danych widoku w widoku).</span><span class="sxs-lookup"><span data-stu-id="587e7-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="587e7-192">Wyświetlanie listy 7 Wybraliśmy zaletą danych widoku do przekazywania wiadomości prostego ciągu z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="587e7-193">Również umożliwia wyświetlanie danych przekazywania innych typów danych, takich jak kolekcja rekordów bazy danych, z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="587e7-194">Na przykład jeśli chcesz wyświetlenia zawartości tabeli Produkty bazy danych w widoku, a następnie przejdzie kolekcji bazy danych rejestruje w widoku danych.</span><span class="sxs-lookup"><span data-stu-id="587e7-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="587e7-195">Istnieje również opcja przekazywania danych silnie typizowanego widoku z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="587e7-196">W tym temacie firma Microsoft Eksplorowanie w samouczku **opis silnie Typizowanej danych widoku i widoki**.</span><span class="sxs-lookup"><span data-stu-id="587e7-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="587e7-197">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="587e7-197">Summary</span></span>

<span data-ttu-id="587e7-198">W tym samouczku podać krótkie wprowadzenie do platformy ASP.NET MVC widoków danych widoku i pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="587e7-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="587e7-199">W pierwszej sekcji przedstawiono sposób dodawania nowych widoków do projektu.</span><span class="sxs-lookup"><span data-stu-id="587e7-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="587e7-200">Wiesz, że należy dodać widok do prawidłowego folderu celu wywołania go z danego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="587e7-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="587e7-201">Następnie Rozmawialiśmy temat pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="587e7-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="587e7-202">Przedstawiono sposób pomocników HTML umożliwiają łatwe generowanie standardowe zawartość HTML.</span><span class="sxs-lookup"><span data-stu-id="587e7-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="587e7-203">Ponadto przedstawiono sposób korzystać z danych widoku do przekazywania danych z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="587e7-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="587e7-204">Next</span><span class="sxs-lookup"><span data-stu-id="587e7-204">Next</span></span>](creating-custom-html-helpers-cs.md)
