---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Tworzenie niestandardowego ograniczenia trasy (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Autor: Stephen Walther pokazuje, jak można utworzyć ograniczenia trasy niestandardowej. Wdrożymy prosty niestandardowe ograniczenia, które uniemożliwia trasę dopasowywane w...'
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 0a0b6b706fdb212a745346ffaefc118e85c2a245
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752462"
---
<a name="creating-a-custom-route-constraint-c"></a>Tworzenie niestandardowego ograniczenia trasy (C#)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> Autor: Stephen Walther pokazuje, jak można utworzyć ograniczenia trasy niestandardowej. Firma Microsoft zaimplementowania proste ograniczenie niestandardowych, które uniemożliwia trasy są dopasowywane, gdy żądanie przeglądarki na komputerze zdalnym.


Celem tego samouczka jest pokazują, jak można utworzyć ograniczenia trasy niestandardowej. Ograniczenia trasy niestandardowej można uniemożliwić trasy są dopasowywane, chyba że jakiś warunek niestandardowy jest zgodny.

W tym samouczku utworzymy Localhost ograniczenia trasy. Ograniczenia trasy Localhost jest zgodny tylko żądań wysyłanych z komputera lokalnego. Żądania zdalne z przez Internet nie zostały dopasowane.

Implementując interfejs IRouteConstraint implementowania ograniczenia trasy niestandardowej. Jest to bardzo prosty interfejs, który opisuje pojedynczej metody:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

Metoda zwraca wartość logiczną. Po powrocie do false trasy skojarzonych z ograniczeniem nie będzie zgodne żądanie przeglądarki.

Ograniczenie Localhost znajduje się w ofercie 1.

**Wyświetlanie listy 1 - LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

Ograniczenia w ofercie 1 korzysta z właściwości IsLocal udostępnianych przez klasy HttpRequest. Ta właściwość zwraca wartość true, gdy adres IP żądania jest albo 127.0.0.1 lub adres IP żądania jest taka sama jak adres IP serwera.

Możesz użyć niestandardowego ograniczenia w obrębie trasy zdefiniowanej w pliku Global.asax. Plik Global.asax w ofercie 2 używa ograniczenie Localhost, aby uniemożliwić osobom żądania na stronie administratora, chyba że dokonają żądania z serwera lokalnego. Na przykład żądanie /Admin/DeleteAll zakończy się niepowodzeniem, gdy na serwerze zdalnym.

**Wyświetlanie listy 2 - w pliku Global.asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

Ograniczenie Localhost jest używane w definicji trasy administratora. Ta trasa nie będzie można dopasować do podwyrażenia żądania zdalnego przeglądarki. Należy pamiętać, jednak czy innych tras zdefiniowanych w pliku Global.asax może pasuje do tego samego żądania. Jest ważne dowiedzieć się, że ograniczenie zapobiega określonej trasy pasujące żądanie i nie wszystkie trasy zdefiniowanej w pliku Global.asax.

Należy zauważyć, że domyślna trasa zostały oznaczone komentarzami w pliku Global.asax w ofercie 2. Jeśli uwzględniony trasa domyślna trasa domyślna umożliwi dopasowanie żądania dla kontrolera administratora. W takim przypadku użytkownicy zdalni nadal może wywołać akcji kontrolera administratora, nawet jeśli ich żądania nie odpowiada trasy administratora.

> [!div class="step-by-step"]
> [Poprzednie](creating-a-route-constraint-cs.md)
> [dalej](asp-net-mvc-controller-overview-vb.md)
