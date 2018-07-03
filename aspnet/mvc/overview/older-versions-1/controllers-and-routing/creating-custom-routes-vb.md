---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Tworzenie tras niestandardowych (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać trasy niestandardowe do aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak zmodyfikować tabela routingu domyślnego w pliku Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: b2bca002434274e334509d21f6857e094d391198
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362021"
---
<a name="creating-custom-routes-vb"></a>Tworzenie tras niestandardowych (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak dodać trasy niestandardowe do aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak zmodyfikować tabela routingu domyślnego w pliku Global.asax.


W tym samouczku dowiesz się, jak dodać trasy niestandardowej do aplikacji ASP.NET MVC. Dowiesz się, jak zmodyfikować tabela routingu domyślnego w pliku Global.asax z tras niestandardowych.

W aplikacjach ASP.NET MVC tabela routingu domyślnego będzie działać prawidłowo. Jednak może się okazać, że ma wyspecjalizowany potrzeb routingu. W takim przypadku można utworzyć trasy niestandardowej.

Wyobraź sobie, na przykład tworzysz aplikację blogu. Możesz chcieć obsłużyć żądań przychodzących, które wyglądają następująco:

/ Archiwum/12-25-2009

Kiedy użytkownik wprowadzi tego żądania, mają być zwracane wpis w blogu, który odnosi się do wartości typu date 12/25/2009. Aby umożliwić obsługę tego typu żądania, musisz utworzyć tras niestandardowych.

Plik Global.asax w ofercie 1 zawiera nowe trasy niestandardowej o nazwie blogu, które obsługuje żądania, które mają postać /Archive/*Data wpisu*.

**Wyświetlanie listy 1 - Global.asax (przy użyciu tras niestandardowych)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

Kolejność trasy, które można dodać do tabeli tras jest ważna. Nasz nowy trasy niestandardowej blogu zostanie dodany przed istniejącą trasę domyślną. Czy to zamówienie, jest wycofywany, a następnie trasy domyślnej zawsze ma zostać wywołana zamiast tras niestandardowych.

Każde żądanie, które zaczyna się od/archiwum/pasuje do trasy niestandardowej blogu. Tak jest on zgodny wszystkie następujące adresy URL:

- / Archiwum/12-25-2009

- / Archiwum/10-6-2004

- / Archiwum/firmy apple

Trasy niestandardowej mapy przychodzące żądanie do kontrolera, o nazwie archiwum i wywołuje akcję Entry(). Gdy wywoływana jest metoda Entry(), daty rozpoczęcia jest przekazywany jako parametr o nazwie entryDate.

Za pomocą tras niestandardowych blogu kontrolera w ofercie 2.

**Wyświetlanie listy 2 - ArchiveController.vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Należy zauważyć, że metoda Entry() w ofercie 2 akceptuje parametr typu Data/Godzina. Struktura MVC jest inteligentnego przekonwertować datę wejścia z adresu URL na wartość typu DateTime automatycznie. Jeśli parametr daty wpis z adresu URL nie można przekonwertować na wartość typu DateTime, zgłaszany jest błąd (patrz rysunek 1).

**Rysunek 1. Błąd konwersji parametru**


[![Okno dialogowe Nowy projekt](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Rysunek 01**: błąd konwersji parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-routes-vb/_static/image2.png))


## <a name="summary"></a>Podsumowanie

Celem tego samouczka było pokazują, jak można utworzyć trasy niestandardowej. Pokazaliśmy ci, jak dodać niestandardowe trasy w tabeli tras w pliku Global.asax, który reprezentuje wpisy w blogu. Omówiliśmy sposób mapowania żądania dotyczące wpisów w blogu o nazwie ArchiveController kontrolera i akcji kontrolera, o nazwie Entry().

> [!div class="step-by-step"]
> [Poprzednie](asp-net-mvc-controller-overview-vb.md)
> [dalej](creating-a-route-constraint-vb.md)
