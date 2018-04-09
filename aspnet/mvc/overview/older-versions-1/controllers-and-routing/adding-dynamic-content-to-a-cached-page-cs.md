---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Zawartości dynamicznej buforowana strona (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak mieszać zawartości dynamicznej i pamięci podręcznej w tej samej stronie. Podstawianie po pamięci podręcznej umożliwia wyświetlanie zawartości dynamicznej, takie jak transparent anonsów o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 9f91cc07bc531cfb3edf577ab79e91fd94a57a3c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a>Zawartości dynamicznej buforowana strona (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak mieszać zawartości dynamicznej i pamięci podręcznej w tej samej stronie. Podstawianie po pamięci podręcznej umożliwia wyświetlanie zawartości dynamicznej, takie jak Anonse transparent lub wiadomości, na stronie zostały wyjściowych w pamięci podręcznej.


Dzięki wykorzystaniu buforowanie danych wyjściowych, może znacznie poprawić wydajność aplikacji platformy ASP.NET MVC. Zamiast ponownego generowania strony poszczególnych czas, żądanej strony, strony umożliwia generowanie raz i są przechowywane w pamięci dla wielu użytkowników.

Występuje problem. Co zrobić, jeśli należy wyświetlić zawartość dynamiczną, na stronie? Załóżmy na przykład chcesz wyświetlić anonsu transparencie na stronie. Nie chcesz anonsu transparent można buforować, dzięki czemu każdy użytkownik będzie widział anonsu tej samej. Nie należy pieniędzy w ten sposób!

Na szczęście jest łatwe rozwiązania. Można skorzystać z funkcji platformy ASP.NET o nazwie *po pamięci podręcznej podstawienia*. Podstawianie po pamięci podręcznej umożliwia Zastąp zawartość dynamiczną, na stronie, które ma w pamięci podręcznej.


Zwykle gdy przy użyciu atrybutu [OutputCache] dane wyjściowe pamięci podręcznej strony, strony są buforowane na zarówno na serwerze i kliencie (przeglądarki sieci web). Użycie pamięci podręcznej po podstawienia, strony są buforowane tylko na serwerze.


#### <a name="using-post-cache-substitution"></a>Using Post-Cache Substitution

Przy użyciu pamięci podręcznej po podstawienia wymaga wykonania dwóch kroków. Najpierw należy zdefiniować metody, która zwraca ciąg reprezentujący zawartość dynamiczną, które mają być wyświetlane na stronie pamięci podręcznej. Następnie należy wywołać metodę HttpResponse.WriteSubstitution() iniekcję zawartość dynamiczną do strony.

Załóżmy, na przykład, że mają losowo w celu wyświetlenia wiadomości różnych elementów stronę z pamięci podręcznej. Klasa w 1 lista przedstawia jedną metodę o nazwie RenderNews(), który losowo zwraca jeden element wiadomości z listy elementów trzy wiadomości.

**Wyświetlanie listy 1 – Models\News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Aby móc korzystać z pamięci podręcznej po podstawienia, należy wywołać metodę HttpResponse.WriteSubstitution(). Metoda WriteSubstitution() ustawia kodu Zamień obszaru buforowana strona zawartości dynamicznej. Metoda WriteSubstitution() jest używana do wyświetlania elementu losowe wiadomości w widoku wyświetlania 2.

**Wyświetlanie listy 2 — Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

Metoda RenderNews jest przekazywany do metody WriteSubstitution(). Zwróć uwagę, nie wywołano metody RenderNews (istnieją nie nawiasów). Zamiast tego odwołania do metody jest przekazywany do WriteSubstitution().

Widok indeksu są buforowane. Widok jest zwracana przez kontroler w 3 wyświetlania. Zwróć uwagę, że akcja indeks() zostanie nadany atrybut [OutputCache], który powoduje, że widok indeksu można buforować przez 60 sekund.

**Wyświetlanie listy 3 — Controllers\HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Mimo że widoku indeksu są buforowane, różnych losowe wiadomości są wyświetlane podczas żądania strony indeksu. Podczas żądania strony indeksu, wyświetlany przez stronę czas nie ulega zmianie przez 60 sekund (zobacz rysunek 1). Fakt, że nie zmienia czas potwierdza, że strony są buforowane. Jednak zawartość wstrzyknięte przez zmiany metody — element losowe wiadomości — WriteSubstitution() z każdym żądaniem.

**Rysunek 1 — wstrzyknięcie elementów dynamicznych wiadomości w stronę z pamięci podręcznej**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Przy użyciu pamięci podręcznej po podstawienia w metody pomocnicze

Łatwiejszy sposób, aby móc korzystać z pamięci podręcznej po podstawienia jest Hermetyzowanie wywołanie do metody WriteSubstitution() w metodzie niestandardowego elementu pomocniczego. Takie podejście jest zilustrowane w listę 4 metody pomocnika.

**Wyświetlanie listy 4 — AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

Wyświetlanie listy 4 zawiera statycznej klasy, która udostępnia dwie metody: RenderBanner() i RenderBannerInternal(). Metoda RenderBanner() reprezentuje metodę pomocnika rzeczywistych. Ta metoda jest rozszerzeniem standardowe klasy ASP.NET MVC HtmlHelper, dzięki czemu można wywołać Html.RenderBanner() w widoku, podobnie jak inne metody pomocnika.

Metoda RenderBanner() wywołuje metodę HttpResponse.WriteSubstitution() przekazywanie metody RenderBannerInternal() do metody WriteSubsitution().

Metoda RenderBannerInternal() jest metoda prywatna. Ta metoda nie będą widoczne jako metody pomocnika. Metoda RenderBannerInternal() losowo zwraca jeden obraz anonsu transparentu z listy trzy obrazy anonsu transparent.

Zmodyfikowany widok indeksu listę 5 przedstawiono, jak można użyć metody pomocniczej RenderBanner(). Zwróć uwagę, że dodatkowe &lt;% @ importu %&gt; dyrektywy znajduje się w górnej części Widok do zaimportowania MvcApplication1.Helpers przestrzeni nazw. Pominięcie zaimportować tej przestrzeni nazw, metoda RenderBanner() nie będą wyświetlane jako metoda we właściwości Html.

**Wyświetlanie listy 5 — Views\Home\Index.aspx (za pomocą metody RenderBanner())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

W przypadku żądania strony renderowany przez widok w listę 5, anons różnych transparentu jest wyświetlany przy każdym żądaniu (patrz rysunek 2). Strony są buforowane, ale anonsu transparent jest dynamicznie wstrzyknięte przez metodę pomocnika RenderBanner().

**Rysunek 2 — widok indeksu wyświetlanie anonsu losowe transparentu**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Podsumowanie

W tym samouczku wyjaśniono, jak dynamicznie Aktualizuj zawartość w pamięci podręcznej strony. Przedstawiono sposób użycia metody HttpResponse.WriteSubstitution(), aby umożliwić dynamiczne zawartość można wprowadzić w stronę z pamięci podręcznej. Przedstawiono również sposób Hermetyzowanie wywołanie do metody WriteSubstitution() wewnątrz metody pomocnika kodu HTML.

Korzystać z pamięci podręcznej, jeśli to możliwe — go może mieć znaczący wpływ na wydajność aplikacji sieci web. Zgodnie z objaśnieniem w tym samouczku, możesz korzystać z pamięci podręcznej, nawet wtedy, gdy konieczne jest wyświetlenie zawartość dynamiczna na swoich stronach.

## 

## 

> [!div class="step-by-step"]
> [Poprzednie](improving-performance-with-output-caching-cs.md)
> [dalej](creating-a-controller-cs.md)
