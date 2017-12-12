---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: "Buforowanie danych w sieci Web ASP.NET stron witryny (Razor) w celu poprawy wydajności | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Można przyspieszyć witryny sieci Web przez go przechowywać — to znaczy pamięci podręcznej - wyników dane, które zwykle może zająć wiele czasu pobierania lub przetwarzania..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: c747fef33a6d1db19f09fd0303c47d689b956687
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>Buforowanie danych w lokacji (Razor) stron sieci Web ASP.NET w celu poprawy wydajności
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób użycia Pomocnika do pamięci podręcznej informacji, aby zwiększyć wydajność w witrynie sieci Web platformy ASP.NET Web Pages (Razor). Można przyspieszyć witryny sieci Web, konfigurując go sklepu &#8212; oznacza to, że pamięć podręczna &#8212; wyniki danych, które zwykle może zająć wiele czasu można pobrać lub przetworzyć i nie zmienia często.
> 
> **Zawartość:** 
> 
> - Jak używać buforowania poprawę czasu reakcji witryny sieci Web.
> 
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzone w artykule:
> 
> - `WebCache` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 3
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2.


Za każdym razem, gdy ktoś zażąda strony z lokacji, serwer sieci web ma wykonania dodatkowych czynności w celu spełnienia żądania. W przypadku niektórych stron serwer może być konieczne wykonywanie zadań, które długo trwać, (stosunkowo), takie jak pobieranie danych z bazy danych. Nawet jeśli te zadania nie wykonać długo w liczbach bezwzględnych, witryny napotyka dużą ilość ruchu, cały szereg poszczególnych żądań, które powodują serwera sieci web do wykonywania zadań skomplikowany lub powolne można sumują się do dużo pracy. Ostatecznie to wpływ na wydajność lokacji.

Jednym ze sposobów poprawy wydajności witryny sieci Web w sytuacjach, takich jak to jest danych w pamięci podręcznej. Jeśli lokacji pobiera ponownego żądania te same informacje, a informacje nie musi zostać zmodyfikowany dla każdej osoby i nie jest to czas poufnych, zamiast ponownego pobierania lub ponownego obliczania, można pobrać danych raz i następnie zapisać wyniki. Przy następnym dotrze żądanie, tym informacji, możesz po prostu pobrać go z pamięci podręcznej.

Ogólnie rzecz biorąc można w pamięci podręcznej informacje, które nie zmieniają się często. Umieszczanie informacji w pamięci podręcznej, jest przechowywany w pamięci na serwerze sieci web. Można określić, jak długo go powinno być buforowane, od czasu w sekundach dni. Po wygaśnięciu okresu buforowania informacji zostanie automatycznie usunięta z pamięci podręcznej.

> [!NOTE]
> Wpisy w pamięci podręcznej mogą zostać usunięte ze względu na inny niż czy wygasną. Na przykład serwer sieci web może tymczasowo zaczyna brakować pamięci i jednym mógł odzyskać pamięć jest zgłaszanie wpisów z pamięci podręcznej. Jak można zauważyć, nawet jeśli została umieść informacje w pamięci podręcznej, masz upewnij się, że jest on jeszcze w razie konieczności.


Załóżmy, że strona, wyświetlająca bieżącego temperatury i prognozie pogody witryny sieci Web. Aby uzyskać informacje tego typu, może wysłać żądania do zewnętrznej usługi. Ponieważ te informacje nie zmieniają się znacznie (w okresie dwóch godzin czasu, na przykład), a ponieważ połączeniami zewnętrznymi wymaga czasu i przepustowości, są odpowiednimi kandydatami do buforowania.

## <a name="adding-caching-to-a-page"></a>Dodawanie buforowanie na stronę

Program ASP.NET zawiera `WebCache` pomocnika, która ułatwia dodawanie buforowanie do swojej witryny i dodać dane do pamięci podręcznej. W tej procedurze utworzysz strony, który buforuje bieżącego czasu. Nie jest to przykład rzeczywistych, ponieważ bieżąca godzina jest coś, które zmieniają się często, że ponadto nie jest skomplikowane obliczenia. Jednak jest to dobry sposób na ilustrują buforowanie w akcji.

1. Dodaj nową stronę o nazwie *WebCache.cshtml* witryny sieci Web.
2. Dodaj następujący kod i znaczników do strony:

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Podczas buforowania danych, do których został dodany do pamięci podręcznej przy użyciu nazwy to jest unikatowy w całej witryny sieci Web. W takim przypadku użyjesz wpis pamięci podręcznej o nazwie `CachedTime`. Jest to `cacheItemKey` pokazano w przykładzie kodu.

    Kod najpierw odczytuje `CachedTime` wpisu pamięci podręcznej. Jeśli wartość jest zwracana (to znaczy, jeśli wpis pamięci podręcznej nie jest pusta), kod tylko ustawia wartość zmiennej czasu na danych z pamięci podręcznej.

    Jednak jeśli wpis pamięci podręcznej nie istnieje (to znaczy, że jest ona pusta), kod ustawia wartość czasu, dodaje go do pamięci podręcznej i ustawia wartość dla wygaśnięcia na jedną minutę. Po minucie wpis pamięci podręcznej zostanie usunięty. (Domyślna wartość wygaśnięcia elementu pamięci podręcznej jest 20 minut). Polecenie `WebCache.Set(cacheItemKey, time, 1, false)` pokazano, jak dodać bieżącą wartość czasu do pamięci podręcznej i ustaw jego wygaśnięcia na 1 minutę. Ustawienie *slidingExpiration* parametr `false` oznacza, że czas wygaśnięcia nie zostanie odnowiony zawsze żądanie. Wygaśnięcia dokładnie 1 minuta po pierwotnie został dodany do pamięci podręcznej. Jeśli ta wartość jest ustawiona na `true` 1 minuta czas wygaśnięcia jest resetowany zawsze wartość zostanie wywołana z pamięci podręcznej.

    Ten kod ilustruje wzorzec, który zawsze należy używać podczas buforowania danych. Zanim będzie można pobrać coś z pamięci podręcznej zawsze sprawdzić w pierwszej kolejności czy `WebCache.Get` metoda zwróciła wartość null. Należy pamiętać, że wpis pamięci podręcznej mogły wygasnąć lub mógł zostać usunięty innego powodu, więc każdy wpis danego nigdy nie jest gwarantowana w pamięci podręcznej.
3. Uruchom *WebCache.cshtml* w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.) Podczas pierwszego żądania strony danych czasu nie znajduje się w pamięci podręcznej i kod musi dodać wartość czasu do pamięci podręcznej.

    ![pamięć podręczna-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Odśwież *WebCache.cshtml* w przeglądarce. Teraz, przy zbieraniu danych jest w pamięci podręcznej. Zwróć uwagę, że czas nie zmieniła się od czasu ostatniego wyświetlania strony.

    ![pamięć podręczna 2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Zaczekaj minutę dla można opróżnić pamięć podręczną, a następnie odśwież stronę. Strona ponownie wskazuje, że przy zbieraniu danych nie został znaleziony w pamięci podręcznej i zaktualizowane razem jest dodawany do pamięci podręcznej.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


- [Wyświetlanie danych na wykresie](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Dokumentacja interfejsu API WebCache](https://msdn.microsoft.com/en-us/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
