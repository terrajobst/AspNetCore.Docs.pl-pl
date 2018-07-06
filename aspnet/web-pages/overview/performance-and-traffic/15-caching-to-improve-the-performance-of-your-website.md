---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: Buforowanie danych we wzorcu ASP.NET Web Pages lokacji (Razor) zapewnienia lepszej wydajności | Dokumentacja firmy Microsoft
author: tfitzmac
description: Można przyspieszyć witryny sieci Web przez on przechowywać — czyli pamięci podręcznej — wyniki danych, który normalnie zajęłoby znaczną ilość czasu pobierania lub przetwarzać...
ms.author: aspnetcontent
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 28be9194bbd95e896311700ddcf89379a82ee636
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805206"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Buforowanie danych w witrynie ASP.NET Web Pages (Razor) w celu zapewnienia lepszej wydajności
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak użyć pomocnika do informacji o pamięci podręcznej, aby zwiększyć wydajność w witrynie internetowej ASP.NET Web Pages (Razor). Można przyspieszyć witryny sieci Web, konfigurując go przechowywać &#8212; oznacza to, że w pamięci podręcznej &#8212; wyników danych, który normalnie zajęłoby znaczną ilość czasu, można pobrać lub przetwarzania, a która nie zmienia się często.
> 
> **Zawartość:** 
> 
> - Jak używać buforowania, można zwiększyć szybkość reakcji witryny sieci Web.
> 
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzona w artykule:
> 
> - `WebCache` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.


Za każdym razem, gdy ktoś zgłasza żądanie strony z lokacji, serwer sieci web ma wykonania dodatkowych czynności w celu spełnienia żądania. W przypadku niektórych stron serwer może być konieczne przeprowadzenie zadań, które długo (stosunkowo), takie jak pobieranie danych z bazy danych. Nawet jeśli te zadania nie wykona długo w liczbach bezwzględnych, jeśli wiele ruchu witryny, całego szeregu poszczególnych żądań, które powodują serwer sieci web do wykonania zadania skomplikowany lub powolne możliwe jest dodanie do sporego nakładu pracy. Ostatecznie może to wpłynąć na wydajność lokacji.

Jednym ze sposobów, aby zwiększyć wydajność witryny sieci Web w sytuacjach, takich jak to jest danych w pamięci podręcznej. Jeśli lokacji pobiera ponownych żądań tych samych informacji i informacji nie musi zostać zmodyfikowany dla każdej osoby, a nie jest to czas uwzględniana wielkość liter, zamiast ponownie podczas pobierania lub ponowne obliczanie, możesz pobrać dane jeden raz i następnie zapisać wyniki. Przy następnym dotrze żądanie, tym informacji, po prostu jego uzyskaniu z pamięci podręcznej.

Ogólnie rzecz biorąc należy w pamięci podręcznej informacje, które nie zmieniają się często. Kiedy wprowadzasz informacje w pamięci podręcznej znajduje się w pamięci na serwerze sieci web. Można określić, jak długo go powinno być buforowane, od sekund do dni. Po wygaśnięciu okresu buforowania informacji zostanie automatycznie usunięty z pamięci podręcznej.

> [!NOTE]
> Wpisy w pamięci podręcznej mogą zostać usunięte powodów innych niż, po wygaśnięciu. Na przykład serwer sieci web może być tymczasowo brakować pamięci i jednym ze sposobów jej odzyskania pamięci jest zgłaszanie wpisów z pamięci podręcznej. Jak można zauważyć, nawet jeśli została utworzona informacje w pamięci podręcznej, masz upewnij się, że jest on jeszcze gdy ich potrzebujesz.


Załóżmy, że witryna ma stronę, która zawiera aktualną temperaturę i prognozy pogody. Aby uzyskać informacje tego typu, może wysyłać żądania do usługi zewnętrznej. Ponieważ te informacje nie zmienia się wiele (w ciągu dwóch godzin okresu, na przykład), a ponieważ połączeniami zewnętrznymi wymaga czasu i przepustowość, jest dobrym kandydatem do buforowania.

## <a name="adding-caching-to-a-page"></a>Dodanie do strony

Program ASP.NET zawiera `WebCache` pomocnika, która ułatwia dodawanie funkcji buforowania do swojej witryny i dodać dane do pamięci podręcznej. W tej procedurze utworzysz strona, która zapisuje bieżący czas w pamięci podręcznej. Jest to przykład rzeczywistych, ponieważ bieżąca godzina to coś, który zmienia się często, a ponadto nie jest to skomplikowane obliczenia. Jednak jest dobrym sposobem ilustrowania buforowania.

1. Dodaj nową stronę o nazwie *WebCache.cshtml* do witryny sieci Web.
2. Na stronie, Dodaj następujący kod i znaczników:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Po użytkownik dane z pamięci podręcznej, został on umieszczony w pamięci podręcznej przy użyciu nazwy, to jest unikatowa w obrębie witryny sieci Web. W tym przypadku użyjemy wpis pamięci podręcznej o nazwie `CachedTime`. Jest to `cacheItemKey` pokazano w przykładzie kodu.

    Kod najpierw odczytuje `CachedTime` wpisu pamięci podręcznej. Jeśli wartość jest zwracana (to znaczy, jeśli wpis pamięci podręcznej nie jest null), kod po prostu ustawia wartość zmiennej czasu buforowania danych.

    Jednak jeśli nie istnieje wpis pamięci podręcznej (czyli jest wartość null), ten kod ustawia wartość czasu, dodaje ją do pamięci podręcznej i ustawia wartość dla wygaśnięcia na jedną minutę. Po jednej minucie wpisu pamięci podręcznej jest odrzucany. (Wartość domyślna wygaśnięcie elementu w pamięci podręcznej jest 20 minut). Polecenie `WebCache.Set(cacheItemKey, time, 1, false)` pokazuje, jak dodać wartość bieżącej godziny do pamięci podręcznej i ustawić jej wygaśnięcia na 1 minutę. Ustawienie *slidingExpiration* parametr `false` oznacza, że czas wygaśnięcia nie zostanie odnowiony każdorazowo żądanie. Wygaśnięcia dokładnie 1 minuta po pierwotnie został dodany do pamięci podręcznej. Jeśli ta wartość jest ustawiona na `true` każdego czas żądania wartość z pamięci podręcznej jest resetowany przy 1 minuta czasu wygaśnięcia.

    Ten kod ilustruje wzorzec, który należy zawsze używać podczas buforowania danych. Przed zagłębieniem się coś z pamięci podręcznej, zawsze najpierw sprawdzić czy `WebCache.Get` metoda zwróciła wartość null. Należy pamiętać, że wpis pamięci podręcznej wygasła lub mógł zostać usunięty innego powodu, aby dany wpis nigdy nie musi znajdować się w pamięci podręcznej.
3. Uruchom *WebCache.cshtml* w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.) Żądanie strony, po raz pierwszy danych czasowych nie znajduje się w pamięci podręcznej, a kod musi dodać wartości w czasie do pamięci podręcznej.

    ![pamięć podręczna-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Odśwież *WebCache.cshtml* w przeglądarce. Tym razem danych czasowych jest w pamięci podręcznej. Należy zauważyć, że czas nie zmieniła się od czasu ostatniego przeglądać strony.

    ![pamięć podręczna 2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Poczekaj jedną minutę na można opróżnić pamięć podręczną, a następnie odśwież stronę. Strona ponownie wskazuje, że danych czasowych nie został znaleziony w pamięci podręcznej i czas aktualizacji jest dodawany do pamięci podręcznej.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


- [Wyświetlanie danych na wykresie](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Dokumentacja interfejsu API WebCache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
