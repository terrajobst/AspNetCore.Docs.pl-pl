---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: Co nowego we wzorcu ASP.NET Web Pages 3.2 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 06/30/2014
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: a55c01c1430b983fbe34654cc2c00de05e941d71
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802254"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Co nowego we wzorcu ASP.NET Web Pages 3.2
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano what's new for ASP.NET Web Pages 3.2, stron sieci Web 3.2.2 i [Web Pages 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web Pages 3.2

Ta wersja naprawia błąd i wprowadza jedną nową funkcję.

## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakiety NuGet w galerii NuGet. Wykonaj wszystkie pakiety środowiska uruchomieniowego [Semantic Versioning](http://semver.org/) specyfikacji. Pakiet ASP.NET Web Pages 3.2 ma następującą wersję: &ldquo;3.2.0&rdquo;. Możesz zainstalować lub zaktualizować te pakiety przy użyciu [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). Wersja ta oferuje również odpowiedni zlokalizowanych pakietów dla narzędzia NuGet.

Możesz zainstalować lub zaktualizować do wydanych pakietów NuGet za pomocą konsoli Menedżera pakietów NuGet:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Nowa funkcja i naprawienie usterki

Naprawiliśmy usterki jednego i wprowadzonych ulepszeniem funkcji pomocniczej w tej wersji. Można znaleźć zapytania dla tego samego [tutaj](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>3.2.2 strony sieci Web platformy ASP.NET

W tej wersji ustala telefoniczny zmiany [wydania Beta stron ASP.NET Web Pages 3.2.1](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) zapewniającą poprawę istotnie poprawiającą wydajność renderowania stron razor dużych. Zobacz[problem Codeplex 585](https://aspnetwebstack.codeplex.com/workitem/585). Ta wersja jest wyrównywany MVC 5.2.2 pakiety, które teraz będzie zależeć od tej wersji.

Poprawiono z zespołem usługi MSN renderowania dużych stron. W przypadku stron renderować ponad 80 kilobajtów danych, firma Microsoft skończyć z obiektów na stosie dużego obiektu. Gdy jest używanych wiele warstw układów można mnożona ten efekt.

Wynik na serwerze jest dodatkowe użycie procesora CPU, dłuższe przechowywanie w pamięci, a nawet długie wstrzymywana podczas [oczyszczania Gen 2](https://msdn.microsoft.com/en-us/library/ms973837.aspx) w moduł odśmiecania pamięci.

Poniżej znajduje się tabela ukazujące wyniki analizy [narzędzia perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) dotyczące przebiegu. Procesor jest przechowywany w stałym poziomie około 68% podczas renderowania dużych stron. W tabeli przedstawiono, że prawie całkowicie wyeliminować liczby kolekcji generacji 2, a wynik jest wyższa liczba żądań i znaczną redukcję wstrzymuje działanie z powodu wyrzucania elementów bezużytecznych.

| **Obszar** | **Przed (3.2)** | **Po (3.2.1)** | **Delta %** |
| --- | --- | --- | --- |
| Łączna liczba żądań (licznik) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Śledzenie czasu trwania (w sekundach) | 196.20 | 198.60 | 1.20% |
| Żądania na sekundę | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Obciążenie procesora CPU | 68.80% | 68.50% |  -0.40% |
| Próbki użycia Procesora GC | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Łączna liczba alokacji (licznik) | 55,357,146 | 57,222,949 | 3.40% |
| Łączna liczba Wstrzymaj GC (przykłady) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Gen0 GC (licznik) | 403 | 1,216 | 201.70% |
| Gen1 GC (licznik) | 290 | 367 | 26.60% |
| Gen2 GC (licznik) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| Procesor CPU / żądania (przykłady/req) | 19.73 | 16.47 | -16.50% |

| Kodowanie kolorami: | <font style="background-color: #00ff00">Poprawa Core</font> | <font style="background-color: #4bacc6">Pozytywny wpływ na wydajność</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Ta wersja zawiera tylko poprawki. Możesz użyć [to zapytanie](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) Aby wyświetlić listę problemów rozwiązanych w tej wersji.
