---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: What's New in ASP.NET Web Pages 3.2 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 80421018e0508d430b6142cd3cee1727d1d17b7e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896397"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Nowości w produkcie ASP.NET Web Pages 3.2
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano, jakie nowości 3.2 stron sieci Web ASP.NET, stron sieci Web 3.2.2 i [stron sieci Web 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>Strony ASP.NET Web Pages 3.2

Ta wersja poprawki błędów i wprowadza jedna nowa funkcja.

## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakietów NuGet w galerii NuGet. Wykonaj wszystkie pakiety środowiska uruchomieniowego [Wersjonowania semantycznego](http://semver.org/) specyfikacji. Pakiet ASP.NET Web Pages 3.2 ma następującą wersję: &ldquo;3.2.0&rdquo;. Można zainstalować lub zaktualizować te pakiety za pośrednictwem [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). Wydanie obejmuje również odpowiedniego zlokalizowane pakiety na NuGet.

Można zainstalować lub zaktualizować do pakietów NuGet wydanych przy użyciu konsoli Menedżera pakietów NuGet:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Nowa funkcja i popraw błąd

Firma Microsoft stałej jeden błąd a co umożliwiło funkcji pomocniczych w tej wersji. Można znaleźć zapytania dla tego samego [tutaj](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>ASP.NET Web Pages 3.2.2

Ta wersja przedstawia up zmiany [wydania Beta stron ASP.NET Web Pages 3.2.1](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) co umożliwia zwiększenie wydajności znaczących w renderowania stron razor dużych. Zobacz[problem witrynie Codeplex 585](https://aspnetwebstack.codeplex.com/workitem/585). Ta wersja wyrównana z MVC 5.2.2 pakiety, do których będzie teraz są zależne od tej wersji.

Firma Microsoft we współpracy z zespołu MSN na renderowanie dużych stron. Gdy wyświetlane ponad 80 kilobajtów danych, możemy końcowa obiektów na stercie dużego obiektu. Użycie wielu warstw układów mnożona można w tym celu.

Wynik na serwerze jest dodatkowy użycie procesora CPU, dłużej przechowywania w pamięci, a nawet długa jest wstrzymywana podczas [oczyszczania Gen 2](https://msdn.microsoft.com/en-us/library/ms973837.aspx) w moduł garbage collector.

Poniżej znajduje się tabela prezentacja wyniki analizy [narzędzia perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) dla przebiegu. Procesor jest stała o 68% podczas renderowania dużych stron. W tabeli pokazano, że prawie całkowicie wyeliminować liczby kolekcji generacji 2, a wynik jest wyższym żądania i znaczny spadek wstrzymuje działanie z powodu wyrzucanie elementów bezużytecznych.

| **Obszar** | **Przed (3.2)** | **Po (3.2.1)** | **Delta %** |
| --- | --- | --- | --- |
| Całkowita liczba żądań (licznik) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Śledzenie czasu trwania (w sekundach) | 196.20 | 198.60 | 1.20% |
| Żądania na sekundę | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Obciążenie procesora CPU | 68.80% | 68.50% |  -0.40% |
| GC CPU próbek | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Całkowita liczba alokacji (licznik) | 55,357,146 | 57,222,949 | 3.40% |
| Całkowite wstrzymywania GC (przykłady) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC (licznik) | 403 | 1,216 | 201.70% |
| Gen1 GC (licznik) | 290 | 367 | 26.60% |
| Gen2 GC (licznik) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| Procesor CPU / żądań (przykłady/req) | 19.73 | 16.47 | -16.50% |

| Kolor kodowanie: | <font style="background-color: #00ff00">Poprawa Core</font> | <font style="background-color: #4bacc6">Pozytywny wpływ na wydajność</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Ta wersja zawiera tylko poprawki błędów. Można użyć [to zapytanie](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) lista problemy rozwiązane w tej wersji.
