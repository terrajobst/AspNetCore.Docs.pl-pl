---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Tworzenie trasy niestandardowe (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać trasy niestandardowe do aplikacji platformy ASP.NET MVC. W tym samouczku Dowiedz się jak zmodyfikować domyślną tabelę routingu w pliku Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: e827725ab7ce54c86ae9f4193d0a8a7ef4af8512
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-routes-vb"></a>Tworzenie trasy niestandardowe (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak dodać trasy niestandardowe do aplikacji platformy ASP.NET MVC. W tym samouczku Dowiedz się jak zmodyfikować domyślną tabelę routingu w pliku Global.asax.


W tym samouczku Dowiedz się jak dodać niestandardowe trasy do aplikacji platformy ASP.NET MVC. Jak zmodyfikować tabeli trasy domyślnej w pliku Global.asax z tras niestandardowych.

W aplikacjach ASP.NET MVC domyślne tabeli tras będzie działać bez problemu. Jednak może się okazać, że ma specjalizowany potrzeb routingu. W takim przypadku można utworzyć tras niestandardowych.

Załóżmy, na przykład tworzysz aplikację blogu. Można obsłużyć żądań przychodzących, które wyglądają następująco:

/ Archiwum 12-25-2009

Gdy użytkownik wprowadza to żądanie, chcesz przywrócić wpis w blogu, która odpowiada Data 2009-12/25. Aby obsługiwać ten typ żądania, należy utworzyć niestandardowe trasy.

Plik Global.asax 1 Lista zawiera nowe niestandardowe trasy, o nazwie blogu w żądaniach dojść, które mają postać /Archive/*Data wprowadzenia*.

**Wyświetlanie listy 1 - Global.asax (z tras niestandardowych)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

Kolejność trasy, które można dodać do tabeli tras jest ważna. Nasze nowe niestandardowe trasy blogu zostanie dodany przed istniejącą trasę domyślną. Jeśli odwrócić kolejność, następnie trasy domyślnej zawsze będzie uzyskać nazwę zamiast tras niestandardowych.

Niestandardowe trasy blogu odpowiada każde żądanie, która rozpoczyna się od/archiwum /. Tak jest on zgodny wszystkich następujących adresów URL:

- / Archiwum 12-25-2009

- -Archiwum 10-6-2004

- / Archiwum/firmy apple

Tras niestandardowych mapuje przychodzące żądanie do kontrolera o nazwie archiwum i wywołuje akcję Entry(). Gdy wywoływana jest metoda Entry(), daty rozpoczęcia jest przekazywana jako parametr o nazwie entryDate.

Można użyć tras niestandardowych blogu kontroler w wyświetlania 2.

**Wyświetlanie listy 2 - ArchiveController.vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Zwróć uwagę, że metoda Entry() wyświetlania 2 przyjmuje parametr typu DateTime. Struktura MVC jest inteligentne przekonwertować datę wejścia z adresu URL na wartość daty i godziny automatycznie. Jeśli parametr Data wpisu z adresu URL nie można przekonwertować na wartość typu DateTime, występuje błąd (zobacz rysunek 1).

**Rysunek 1 — błąd podczas konwersji parametru**


[![Okno dialogowe nowego projektu](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Rysunek 01**: błąd podczas konwersji parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-routes-vb/_static/image2.png))


## <a name="summary"></a>Podsumowanie

Celem tego samouczka było wykazać, sposób tworzenia tras niestandardowych. Przedstawiono sposób dodawania tras niestandardowych do tabeli tras w pliku Global.asax, który reprezentuje wpisy. Omówiono sposób mapowania żądania do wpisów w kontrolerze o nazwie ArchiveController i o nazwie Entry() akcji kontrolera.

> [!div class="step-by-step"]
> [Poprzednie](asp-net-mvc-controller-overview-vb.md)
> [dalej](creating-a-route-constraint-vb.md)
