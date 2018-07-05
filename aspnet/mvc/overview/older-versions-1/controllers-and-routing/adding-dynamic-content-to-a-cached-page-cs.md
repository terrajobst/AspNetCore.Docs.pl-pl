---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Dodawanie zawartości dynamicznej do buforowanej strony (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak łączyć zawartości dynamicznej i pamięci podręcznej w tej samej stronie. Podstawianie po pamięci podręcznej umożliwia wyświetlanie zawartości dynamicznej, takiej jak transparent anonsów o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 08f680e8d057f47a3f2802b1136edfb00634637d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364148"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a>Dodawanie zawartości dynamicznej do buforowanej strony (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak łączyć zawartości dynamicznej i pamięci podręcznej w tej samej stronie. Podstawianie po pamięci podręcznej umożliwia wyświetlanie zawartości dynamicznej, takie jak Anonse transparentu lub elementy wiadomości, w ramach strony została wyjściowych w pamięci podręcznej.


Korzystając z buforowania danych wyjściowych, można znacznie zwiększyć wydajność aplikacji ASP.NET MVC. Zamiast ponownego generowania strony razem każdym żądaniu strony, strony można generowane raz i są przechowywane w pamięci dla wielu użytkowników.

Występuje problem. Co zrobić, jeśli należy wyświetlić zawartość dynamiczna na tej stronie? Załóżmy na przykład, chcesz wyświetlić anonsu banerze na stronie. Nie chcesz, aby anonsu transparent buforowane, dzięki czemu każdy użytkownik będzie widział tej samej anonsu. W takich sytuacjach przydałaby upewnij pieniędzy w ten sposób.

Na szczęście jest prostemu rozwiązaniu. Możesz korzystać z zalet funkcji struktury programu ASP.NET o nazwie *po utworzeniu pamięci podręcznej podstawienia*. Podstawianie po pamięci podręcznej umożliwia Zastąp zawartość dynamiczna, na stronie, która ma w pamięci podręcznej.


Zwykle gdy danych wyjściowych strony w pamięci podręcznej przy użyciu atrybutu [OutputCache] strony są buforowane na serwera i klienta (przeglądarki sieci web). Gdy używasz podstawienia po pamięci podręcznej, jest ona buforowana tylko na serwerze.


#### <a name="using-post-cache-substitution"></a>Using Post-Cache Substitution

Korzystanie z pamięci podręcznej po podstawienia wymaga dwóch kroków. Najpierw należy zdefiniować metodę, która zwraca ciąg reprezentujący zawartości dynamicznej, która ma być wyświetlany na stronie pamięci podręcznej. Następnie możesz wywołać metodę HttpResponse.WriteSubstitution() iniekcję zawartości dynamicznej do strony.

Wyobraź sobie, na przykład chcesz losowo wyświetlać elementy inne kanały aktualności w stronę z pamięci podręcznej. Klasa w ofercie 1 przedstawia jedną metodę o nazwie RenderNews(), który losowo zwraca jeden element wiadomości z listy elementów trzy wiadomości.

**Wyświetlanie listy 1 – Models\News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Aby móc korzystać z pamięci podręcznej po podstawienia, należy wywołać metodę HttpResponse.WriteSubstitution(). Metoda WriteSubstitution() konfiguruje kod, aby zastąpić region buforowane strony z zawartością dynamiczną. Metoda WriteSubstitution() jest używana do wyświetlania elementu losowego wiadomości w widoku w ofercie 2.

**Wyświetlanie listy 2 — Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

Metoda RenderNews jest przekazywany do metody WriteSubstitution(). Należy zauważyć, że nie jest wywoływana metoda RenderNews (nie ma żadnych nawiasów). Zamiast tego odwołania do metody jest przekazywany do WriteSubstitution().

Widok indeksu są buforowane. Widok jest zwracany przez kontroler w ofercie 3. Należy zauważyć, że akcja indeks() zostanie nadany atrybut [OutputCache], który powoduje, że widok indeksu przechowywanie w pamięci podręcznej przez 60 sekund.

**Wyświetlanie listy 3 — Controllers\HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Mimo że widoku indeksu są buforowane, inne kanały aktualności losowego mają być wyświetlane w przypadku żądania strony indeksu. W przypadku żądania strony indeksu, wyświetlany przez stronę czas nie zmienia się 60 sekund (patrz rysunek 1). Fakt, że nie zmienia czas okazuje się, że strona jest buforowana. Jednakże zawartość wstrzyknięte przez zmiany metody — element losowy wiadomości — WriteSubstitution() z każdym żądaniem.

**Rysunek 1 — wprowadzanie elementów dynamicznych wiadomości w stronę z pamięci podręcznej**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Przy użyciu pamięci podręcznej po podstawienia w metody pomocnika

Jest łatwiejszy sposób, aby móc korzystać z pamięci podręcznej po podstawienia do hermetyzacji wywołanie metody WriteSubstitution() wewnątrz metody niestandardowego elementu pomocniczego. To podejście jest zilustrowany przez metody pomocniczej w ofercie 4.

**Wyświetlanie listy 4 — AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

Wyświetlanie listy 4 zawiera klasy statycznej, która udostępnia dwie metody: RenderBanner() i RenderBannerInternal(). Metoda RenderBanner() reprezentuje metodę pomocnika rzeczywistych. Ta metoda jest rozszerzeniem standardowa klasy ASP.NET MVC HtmlHelper dzięki czemu można wywołać Html.RenderBanner() w widoku, podobnie jak każda inna metoda pomocnika.

Metoda RenderBanner() wywołuje metodę HttpResponse.WriteSubstitution(), przekazując RenderBannerInternal() metody do metody WriteSubsitution().

Metoda RenderBannerInternal() jest metody prywatnej. Ta metoda nie będzie widoczne jako metody pomocnika. Metoda RenderBannerInternal() losowo zwraca jeden obraz anonsu transparent z listy trzy obrazy anonsu baner.

Zmodyfikowany widok indeksu w ofercie 5 ilustruje, jak można użyć metody pomocniczej RenderBanner(). Należy zauważyć, że dodatkowy &lt;% @ % importu&gt; dyrektywa jest uwzględniony w górnej części Widok do zaimportowania MvcApplication1.Helpers przestrzeni nazw. Pominięcie można zaimportować tej przestrzeni nazw, metoda RenderBanner() nie są wyświetlane jako metodę dla właściwości Html.

**Wyświetlanie listy 5 — Views\Home\Index.aspx (przy użyciu metody RenderBanner())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Żądając strony renderowany przez widok w ofercie 5 anonsu różnych transparent jest wyświetlany z każdym żądaniem (patrz rysunek 2). Strony są buforowane, ale reklama transparencie są wstrzykiwane dynamicznie przez metodę pomocnika RenderBanner().

**Rysunek 2 — widok indeksu, wyświetlając ogłoszenie losowe transparentu**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Podsumowanie

W tym samouczku wyjaśniono, jak może dynamicznie aktualizować zawartość w pamięci podręcznej strony. Pokazaliśmy ci, jak użyć metody HttpResponse.WriteSubstitution() Włączanie zawartości dynamicznej ich wstrzyknięcie w stronę z pamięci podręcznej. Przedstawiono również sposób hermetyzacji wywołanie metody WriteSubstitution() wewnątrz metody pomocnika kodu HTML.

Skorzystaj z zalet pamięci podręcznej, jeśli to możliwe — może mieć znaczący wpływ na wydajność aplikacji sieci web. Jak wyjaśniono w ramach tego samouczka, możesz korzystać z pamięci podręcznej, nawet wtedy, gdy trzeba wyświetlić zawartość dynamiczna na stronach sieci.

## 

## 

> [!div class="step-by-step"]
> [Poprzednie](improving-performance-with-output-caching-cs.md)
> [dalej](creating-a-controller-cs.md)
