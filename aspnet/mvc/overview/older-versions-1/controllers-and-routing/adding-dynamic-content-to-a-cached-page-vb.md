---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Zawartości dynamicznej buforowana strona (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak mieszać zawartości dynamicznej i pamięci podręcznej w tej samej stronie. Podstawianie po pamięci podręcznej umożliwia wyświetlanie zawartości dynamicznej, takie jak transparent anonsów o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 89421b4bec2170e408ded87ccc918a7a16844a98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a><span data-ttu-id="88634-104">Zawartości dynamicznej buforowana strona (VB)</span><span class="sxs-lookup"><span data-stu-id="88634-104">Adding Dynamic Content to a Cached Page (VB)</span></span>
====================
<span data-ttu-id="88634-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="88634-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="88634-106">Dowiedz się, jak mieszać zawartości dynamicznej i pamięci podręcznej w tej samej stronie.</span><span class="sxs-lookup"><span data-stu-id="88634-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="88634-107">Podstawianie po pamięci podręcznej umożliwia wyświetlanie zawartości dynamicznej, takie jak Anonse transparent lub wiadomości, na stronie zostały wyjściowych w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="88634-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="88634-108">Dzięki wykorzystaniu buforowanie danych wyjściowych, może znacznie poprawić wydajność aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="88634-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="88634-109">Zamiast ponownego generowania strony poszczególnych czas, żądanej strony, strony umożliwia generowanie raz i są przechowywane w pamięci dla wielu użytkowników.</span><span class="sxs-lookup"><span data-stu-id="88634-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="88634-110">Występuje problem.</span><span class="sxs-lookup"><span data-stu-id="88634-110">But there is a problem.</span></span> <span data-ttu-id="88634-111">Co zrobić, jeśli należy wyświetlić zawartość dynamiczną, na stronie?</span><span class="sxs-lookup"><span data-stu-id="88634-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="88634-112">Załóżmy na przykład chcesz wyświetlić anonsu transparencie na stronie.</span><span class="sxs-lookup"><span data-stu-id="88634-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="88634-113">Nie chcesz anonsu transparent można buforować, dzięki czemu każdy użytkownik będzie widział anonsu tej samej.</span><span class="sxs-lookup"><span data-stu-id="88634-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="88634-114">Nie należy pieniędzy w ten sposób!</span><span class="sxs-lookup"><span data-stu-id="88634-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="88634-115">Na szczęście jest łatwe rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="88634-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="88634-116">Można skorzystać z funkcji platformy ASP.NET o nazwie *po pamięci podręcznej podstawienia*.</span><span class="sxs-lookup"><span data-stu-id="88634-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="88634-117">Podstawianie po pamięci podręcznej umożliwia Zastąp zawartość dynamiczną, na stronie, które ma w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="88634-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="88634-118">Zwykle podczas wyprowadzania pamięci podręcznej strony za pomocą &lt;OutputCache&gt; atrybutu, strony są buforowane na serwerze i kliencie (przeglądarki sieci web).</span><span class="sxs-lookup"><span data-stu-id="88634-118">Normally, when you output cache a page by using the &lt;OutputCache&gt; attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="88634-119">Użycie pamięci podręcznej po podstawienia, strony są buforowane tylko na serwerze.</span><span class="sxs-lookup"><span data-stu-id="88634-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="88634-120">Using Post-Cache Substitution</span><span class="sxs-lookup"><span data-stu-id="88634-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="88634-121">Przy użyciu pamięci podręcznej po podstawienia wymaga wykonania dwóch kroków.</span><span class="sxs-lookup"><span data-stu-id="88634-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="88634-122">Najpierw należy zdefiniować metody, która zwraca ciąg reprezentujący zawartość dynamiczną, które mają być wyświetlane na stronie pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="88634-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="88634-123">Następnie należy wywołać metodę HttpResponse.WriteSubstitution() iniekcję zawartość dynamiczną do strony.</span><span class="sxs-lookup"><span data-stu-id="88634-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="88634-124">Załóżmy, na przykład, że mają losowo w celu wyświetlenia wiadomości różnych elementów stronę z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="88634-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="88634-125">Klasa w 1 lista przedstawia jedną metodę o nazwie RenderNews(), który losowo zwraca jeden element wiadomości z listy elementów trzy wiadomości.</span><span class="sxs-lookup"><span data-stu-id="88634-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="88634-126">**Wyświetlanie listy 1 – Models\News.vb**</span><span class="sxs-lookup"><span data-stu-id="88634-126">**Listing 1 – Models\News.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

<span data-ttu-id="88634-127">Aby móc korzystać z pamięci podręcznej po podstawienia, należy wywołać metodę HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="88634-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="88634-128">Metoda WriteSubstitution() ustawia kodu Zamień obszaru buforowana strona zawartości dynamicznej.</span><span class="sxs-lookup"><span data-stu-id="88634-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="88634-129">Metoda WriteSubstitution() jest używana do wyświetlania elementu losowe wiadomości w widoku wyświetlania 2.</span><span class="sxs-lookup"><span data-stu-id="88634-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="88634-130">**Wyświetlanie listy 2 — Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="88634-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

<span data-ttu-id="88634-131">Metoda RenderNews jest przekazywany do metody WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="88634-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="88634-132">Zwróć uwagę, nie wywołano metody RenderNews.</span><span class="sxs-lookup"><span data-stu-id="88634-132">Notice that the RenderNews method is not called.</span></span> <span data-ttu-id="88634-133">Zamiast tego odwołania do metody jest przekazywany do WriteSubstitution() za pomocą operatora AddressOf.</span><span class="sxs-lookup"><span data-stu-id="88634-133">Instead a reference to the method is passed to WriteSubstitution() with the help of the AddressOf operator.</span></span>

<span data-ttu-id="88634-134">Widok indeksu są buforowane.</span><span class="sxs-lookup"><span data-stu-id="88634-134">The Index view is cached.</span></span> <span data-ttu-id="88634-135">Widok jest zwracana przez kontroler w 3 wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="88634-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="88634-136">Należy zauważyć, że akcja indeks() zostanie nadany &lt;OutputCache&gt; atrybut, który powoduje, że widok indeksu można buforować przez 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="88634-136">Notice that the Index() action is decorated with an &lt;OutputCache&gt; attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="88634-137">**Wyświetlanie listy 3 — Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="88634-137">**Listing 3 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

<span data-ttu-id="88634-138">Mimo że widoku indeksu są buforowane, różnych losowe wiadomości są wyświetlane podczas żądania strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="88634-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="88634-139">Podczas żądania strony indeksu, wyświetlany przez stronę czas nie ulega zmianie przez 60 sekund (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="88634-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="88634-140">Fakt, że nie zmienia czas potwierdza, że strony są buforowane.</span><span class="sxs-lookup"><span data-stu-id="88634-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="88634-141">Jednak zawartość wstrzyknięte przez zmiany metody — element losowe wiadomości — WriteSubstitution() z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="88634-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="88634-142">**Rysunek 1 — wstrzyknięcie elementów dynamicznych wiadomości w stronę z pamięci podręcznej**</span><span class="sxs-lookup"><span data-stu-id="88634-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="88634-144">Przy użyciu pamięci podręcznej po podstawienia w metody pomocnicze</span><span class="sxs-lookup"><span data-stu-id="88634-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="88634-145">Łatwiejszy sposób, aby móc korzystać z pamięci podręcznej po podstawienia jest Hermetyzowanie wywołanie do metody WriteSubstitution() w metodzie niestandardowego elementu pomocniczego.</span><span class="sxs-lookup"><span data-stu-id="88634-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="88634-146">Takie podejście jest zilustrowane w listę 4 metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="88634-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="88634-147">**Wyświetlanie listy 4 — Helpers\AdHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="88634-147">**Listing 4 – Helpers\AdHelper.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

<span data-ttu-id="88634-148">Wyświetlanie listy 4 zawiera moduł Visual Basic, który udostępnia dwie metody: RenderBanner() i RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="88634-148">Listing 4 contains a Visual Basic module that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="88634-149">Metoda RenderBanner() reprezentuje metodę pomocnika rzeczywistych.</span><span class="sxs-lookup"><span data-stu-id="88634-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="88634-150">Ta metoda jest rozszerzeniem standardowe klasy ASP.NET MVC HtmlHelper, dzięki czemu można wywołać Html.RenderBanner() w widoku, podobnie jak inne metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="88634-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="88634-151">Metoda RenderBanner() wywołuje metodę HttpResponse.WriteSubstitution() przekazywanie metody RenderBannerInternal() do metody WriteSubsitution().</span><span class="sxs-lookup"><span data-stu-id="88634-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="88634-152">Metoda RenderBannerInternal() jest metoda prywatna.</span><span class="sxs-lookup"><span data-stu-id="88634-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="88634-153">Ta metoda nie będą widoczne jako metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="88634-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="88634-154">Metoda RenderBannerInternal() losowo zwraca jeden obraz anonsu transparentu z listy trzy obrazy anonsu transparent.</span><span class="sxs-lookup"><span data-stu-id="88634-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="88634-155">Zmodyfikowany widok indeksu listę 5 przedstawiono, jak można użyć metody pomocniczej RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="88634-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="88634-156">Zwróć uwagę, że dodatkowe &lt;% @ importu %&gt; dyrektywy znajduje się w górnej części Widok do zaimportowania MvcApplication1.Helpers przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="88634-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="88634-157">Pominięcie zaimportować tej przestrzeni nazw, metoda RenderBanner() nie będą wyświetlane jako metoda we właściwości Html.</span><span class="sxs-lookup"><span data-stu-id="88634-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="88634-158">**Wyświetlanie listy 5 — Views\Home\Index.aspx (za pomocą metody RenderBanner())**</span><span class="sxs-lookup"><span data-stu-id="88634-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

<span data-ttu-id="88634-159">W przypadku żądania strony renderowany przez widok w listę 5, anons różnych transparentu jest wyświetlany przy każdym żądaniu (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="88634-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="88634-160">Strony są buforowane, ale anonsu transparent jest dynamicznie wstrzyknięte przez metodę pomocnika RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="88634-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="88634-161">**Rysunek 2 — widok indeksu wyświetlanie anonsu losowe transparentu**</span><span class="sxs-lookup"><span data-stu-id="88634-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="88634-163">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="88634-163">Summary</span></span>

<span data-ttu-id="88634-164">W tym samouczku wyjaśniono, jak dynamicznie Aktualizuj zawartość w pamięci podręcznej strony.</span><span class="sxs-lookup"><span data-stu-id="88634-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="88634-165">Przedstawiono sposób użycia metody HttpResponse.WriteSubstitution(), aby umożliwić dynamiczne zawartość można wprowadzić w stronę z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="88634-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="88634-166">Przedstawiono również sposób Hermetyzowanie wywołanie do metody WriteSubstitution() wewnątrz metody pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="88634-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="88634-167">Korzystać z pamięci podręcznej, jeśli to możliwe — go może mieć znaczący wpływ na wydajność aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="88634-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="88634-168">Zgodnie z objaśnieniem w tym samouczku, możesz korzystać z pamięci podręcznej, nawet wtedy, gdy konieczne jest wyświetlenie zawartość dynamiczna na swoich stronach.</span><span class="sxs-lookup"><span data-stu-id="88634-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="88634-169">[Poprzednie](improving-performance-with-output-caching-vb.md)
> [dalej](creating-a-controller-vb.md)</span><span class="sxs-lookup"><span data-stu-id="88634-169">[Previous](improving-performance-with-output-caching-vb.md)
[Next](creating-a-controller-vb.md)</span></span>
