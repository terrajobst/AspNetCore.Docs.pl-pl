---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Dodawanie zawartości dynamicznej do buforowanej strony (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak łączyć zawartości dynamicznej i pamięci podręcznej w tej samej stronie. Podstawianie po pamięci podręcznej umożliwia wyświetlanie zawartości dynamicznej, takiej jak transparent anonsów o...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 5c6d5a223a465a00a08f58a188a16cbf3c310431
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805584"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a><span data-ttu-id="339f5-104">Dodawanie zawartości dynamicznej do buforowanej strony (C#)</span><span class="sxs-lookup"><span data-stu-id="339f5-104">Adding Dynamic Content to a Cached Page (C#)</span></span>
====================
<span data-ttu-id="339f5-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="339f5-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="339f5-106">Dowiedz się, jak łączyć zawartości dynamicznej i pamięci podręcznej w tej samej stronie.</span><span class="sxs-lookup"><span data-stu-id="339f5-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="339f5-107">Podstawianie po pamięci podręcznej umożliwia wyświetlanie zawartości dynamicznej, takie jak Anonse transparentu lub elementy wiadomości, w ramach strony została wyjściowych w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="339f5-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="339f5-108">Korzystając z buforowania danych wyjściowych, można znacznie zwiększyć wydajność aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="339f5-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="339f5-109">Zamiast ponownego generowania strony razem każdym żądaniu strony, strony można generowane raz i są przechowywane w pamięci dla wielu użytkowników.</span><span class="sxs-lookup"><span data-stu-id="339f5-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="339f5-110">Występuje problem.</span><span class="sxs-lookup"><span data-stu-id="339f5-110">But there is a problem.</span></span> <span data-ttu-id="339f5-111">Co zrobić, jeśli należy wyświetlić zawartość dynamiczna na tej stronie?</span><span class="sxs-lookup"><span data-stu-id="339f5-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="339f5-112">Załóżmy na przykład, chcesz wyświetlić anonsu banerze na stronie.</span><span class="sxs-lookup"><span data-stu-id="339f5-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="339f5-113">Nie chcesz, aby anonsu transparent buforowane, dzięki czemu każdy użytkownik będzie widział tej samej anonsu.</span><span class="sxs-lookup"><span data-stu-id="339f5-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="339f5-114">W takich sytuacjach przydałaby upewnij pieniędzy w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="339f5-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="339f5-115">Na szczęście jest prostemu rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="339f5-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="339f5-116">Możesz korzystać z zalet funkcji struktury programu ASP.NET o nazwie *po utworzeniu pamięci podręcznej podstawienia*.</span><span class="sxs-lookup"><span data-stu-id="339f5-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="339f5-117">Podstawianie po pamięci podręcznej umożliwia Zastąp zawartość dynamiczna, na stronie, która ma w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="339f5-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="339f5-118">Zwykle gdy danych wyjściowych strony w pamięci podręcznej przy użyciu atrybutu [OutputCache] strony są buforowane na serwera i klienta (przeglądarki sieci web).</span><span class="sxs-lookup"><span data-stu-id="339f5-118">Normally, when you output cache a page by using the [OutputCache] attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="339f5-119">Gdy używasz podstawienia po pamięci podręcznej, jest ona buforowana tylko na serwerze.</span><span class="sxs-lookup"><span data-stu-id="339f5-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="339f5-120">Using Post-Cache Substitution</span><span class="sxs-lookup"><span data-stu-id="339f5-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="339f5-121">Korzystanie z pamięci podręcznej po podstawienia wymaga dwóch kroków.</span><span class="sxs-lookup"><span data-stu-id="339f5-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="339f5-122">Najpierw należy zdefiniować metodę, która zwraca ciąg reprezentujący zawartości dynamicznej, która ma być wyświetlany na stronie pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="339f5-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="339f5-123">Następnie możesz wywołać metodę HttpResponse.WriteSubstitution() iniekcję zawartości dynamicznej do strony.</span><span class="sxs-lookup"><span data-stu-id="339f5-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="339f5-124">Wyobraź sobie, na przykład chcesz losowo wyświetlać elementy inne kanały aktualności w stronę z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="339f5-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="339f5-125">Klasa w ofercie 1 przedstawia jedną metodę o nazwie RenderNews(), który losowo zwraca jeden element wiadomości z listy elementów trzy wiadomości.</span><span class="sxs-lookup"><span data-stu-id="339f5-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="339f5-126">**Wyświetlanie listy 1 – Models\News.cs**</span><span class="sxs-lookup"><span data-stu-id="339f5-126">**Listing 1 – Models\News.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

<span data-ttu-id="339f5-127">Aby móc korzystać z pamięci podręcznej po podstawienia, należy wywołać metodę HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="339f5-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="339f5-128">Metoda WriteSubstitution() konfiguruje kod, aby zastąpić region buforowane strony z zawartością dynamiczną.</span><span class="sxs-lookup"><span data-stu-id="339f5-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="339f5-129">Metoda WriteSubstitution() jest używana do wyświetlania elementu losowego wiadomości w widoku w ofercie 2.</span><span class="sxs-lookup"><span data-stu-id="339f5-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="339f5-130">**Wyświetlanie listy 2 — Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="339f5-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

<span data-ttu-id="339f5-131">Metoda RenderNews jest przekazywany do metody WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="339f5-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="339f5-132">Należy zauważyć, że nie jest wywoływana metoda RenderNews (nie ma żadnych nawiasów).</span><span class="sxs-lookup"><span data-stu-id="339f5-132">Notice that the RenderNews method is not called (there are no parentheses).</span></span> <span data-ttu-id="339f5-133">Zamiast tego odwołania do metody jest przekazywany do WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="339f5-133">Instead a reference to the method is passed to WriteSubstitution().</span></span>

<span data-ttu-id="339f5-134">Widok indeksu są buforowane.</span><span class="sxs-lookup"><span data-stu-id="339f5-134">The Index view is cached.</span></span> <span data-ttu-id="339f5-135">Widok jest zwracany przez kontroler w ofercie 3.</span><span class="sxs-lookup"><span data-stu-id="339f5-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="339f5-136">Należy zauważyć, że akcja indeks() zostanie nadany atrybut [OutputCache], który powoduje, że widok indeksu przechowywanie w pamięci podręcznej przez 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="339f5-136">Notice that the Index() action is decorated with an [OutputCache] attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="339f5-137">**Wyświetlanie listy 3 — Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="339f5-137">**Listing 3 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

<span data-ttu-id="339f5-138">Mimo że widoku indeksu są buforowane, inne kanały aktualności losowego mają być wyświetlane w przypadku żądania strony indeksu.</span><span class="sxs-lookup"><span data-stu-id="339f5-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="339f5-139">W przypadku żądania strony indeksu, wyświetlany przez stronę czas nie zmienia się 60 sekund (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="339f5-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="339f5-140">Fakt, że nie zmienia czas okazuje się, że strona jest buforowana.</span><span class="sxs-lookup"><span data-stu-id="339f5-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="339f5-141">Jednakże zawartość wstrzyknięte przez zmiany metody — element losowy wiadomości — WriteSubstitution() z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="339f5-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="339f5-142">**Rysunek 1 — wprowadzanie elementów dynamicznych wiadomości w stronę z pamięci podręcznej**</span><span class="sxs-lookup"><span data-stu-id="339f5-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="339f5-144">Przy użyciu pamięci podręcznej po podstawienia w metody pomocnika</span><span class="sxs-lookup"><span data-stu-id="339f5-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="339f5-145">Jest łatwiejszy sposób, aby móc korzystać z pamięci podręcznej po podstawienia do hermetyzacji wywołanie metody WriteSubstitution() wewnątrz metody niestandardowego elementu pomocniczego.</span><span class="sxs-lookup"><span data-stu-id="339f5-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="339f5-146">To podejście jest zilustrowany przez metody pomocniczej w ofercie 4.</span><span class="sxs-lookup"><span data-stu-id="339f5-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="339f5-147">**Wyświetlanie listy 4 — AdHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="339f5-147">**Listing 4 – AdHelper.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

<span data-ttu-id="339f5-148">Wyświetlanie listy 4 zawiera klasy statycznej, która udostępnia dwie metody: RenderBanner() i RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="339f5-148">Listing 4 contains a static class that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="339f5-149">Metoda RenderBanner() reprezentuje metodę pomocnika rzeczywistych.</span><span class="sxs-lookup"><span data-stu-id="339f5-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="339f5-150">Ta metoda jest rozszerzeniem standardowa klasy ASP.NET MVC HtmlHelper dzięki czemu można wywołać Html.RenderBanner() w widoku, podobnie jak każda inna metoda pomocnika.</span><span class="sxs-lookup"><span data-stu-id="339f5-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="339f5-151">Metoda RenderBanner() wywołuje metodę HttpResponse.WriteSubstitution(), przekazując RenderBannerInternal() metody do metody WriteSubsitution().</span><span class="sxs-lookup"><span data-stu-id="339f5-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="339f5-152">Metoda RenderBannerInternal() jest metody prywatnej.</span><span class="sxs-lookup"><span data-stu-id="339f5-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="339f5-153">Ta metoda nie będzie widoczne jako metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="339f5-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="339f5-154">Metoda RenderBannerInternal() losowo zwraca jeden obraz anonsu transparent z listy trzy obrazy anonsu baner.</span><span class="sxs-lookup"><span data-stu-id="339f5-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="339f5-155">Zmodyfikowany widok indeksu w ofercie 5 ilustruje, jak można użyć metody pomocniczej RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="339f5-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="339f5-156">Należy zauważyć, że dodatkowy &lt;% @ % importu&gt; dyrektywa jest uwzględniony w górnej części Widok do zaimportowania MvcApplication1.Helpers przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="339f5-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="339f5-157">Pominięcie można zaimportować tej przestrzeni nazw, metoda RenderBanner() nie są wyświetlane jako metodę dla właściwości Html.</span><span class="sxs-lookup"><span data-stu-id="339f5-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="339f5-158">**Wyświetlanie listy 5 — Views\Home\Index.aspx (przy użyciu metody RenderBanner())**</span><span class="sxs-lookup"><span data-stu-id="339f5-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

<span data-ttu-id="339f5-159">Żądając strony renderowany przez widok w ofercie 5 anonsu różnych transparent jest wyświetlany z każdym żądaniem (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="339f5-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="339f5-160">Strony są buforowane, ale reklama transparencie są wstrzykiwane dynamicznie przez metodę pomocnika RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="339f5-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="339f5-161">**Rysunek 2 — widok indeksu, wyświetlając ogłoszenie losowe transparentu**</span><span class="sxs-lookup"><span data-stu-id="339f5-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="339f5-163">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="339f5-163">Summary</span></span>

<span data-ttu-id="339f5-164">W tym samouczku wyjaśniono, jak może dynamicznie aktualizować zawartość w pamięci podręcznej strony.</span><span class="sxs-lookup"><span data-stu-id="339f5-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="339f5-165">Pokazaliśmy ci, jak użyć metody HttpResponse.WriteSubstitution() Włączanie zawartości dynamicznej ich wstrzyknięcie w stronę z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="339f5-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="339f5-166">Przedstawiono również sposób hermetyzacji wywołanie metody WriteSubstitution() wewnątrz metody pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="339f5-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="339f5-167">Skorzystaj z zalet pamięci podręcznej, jeśli to możliwe — może mieć znaczący wpływ na wydajność aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="339f5-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="339f5-168">Jak wyjaśniono w ramach tego samouczka, możesz korzystać z pamięci podręcznej, nawet wtedy, gdy trzeba wyświetlić zawartość dynamiczna na stronach sieci.</span><span class="sxs-lookup"><span data-stu-id="339f5-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

## 

## 

> [!div class="step-by-step"]
> <span data-ttu-id="339f5-169">[Poprzednie](improving-performance-with-output-caching-cs.md)
> [dalej](creating-a-controller-cs.md)</span><span class="sxs-lookup"><span data-stu-id="339f5-169">[Previous](improving-performance-with-output-caching-cs.md)
[Next](creating-a-controller-cs.md)</span></span>
