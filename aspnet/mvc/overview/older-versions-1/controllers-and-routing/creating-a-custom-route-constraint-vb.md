---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Tworzenie niestandardowego ograniczenia trasy (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Autor: Stephen Walther pokazuje, jak można utworzyć ograniczenia trasy niestandardowej. Wdrożymy prosty niestandardowe ograniczenia, które uniemożliwia trasę dopasowywane w...'
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 72389d11467cbf7baea4cc9452266edb8ab81125
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840478"
---
<a name="creating-a-custom-route-constraint-vb"></a>Tworzenie niestandardowego ograniczenia trasy (VB)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> Autor: Stephen Walther pokazuje, jak można utworzyć ograniczenia trasy niestandardowej. Firma Microsoft zaimplementowania proste ograniczenie niestandardowych, które uniemożliwia trasy są dopasowywane, gdy żądanie przeglądarki na komputerze zdalnym.


Celem tego samouczka jest pokazują, jak można utworzyć ograniczenia trasy niestandardowej. Ograniczenia trasy niestandardowej można uniemożliwić trasy są dopasowywane, chyba że jakiś warunek niestandardowy jest zgodny.

W tym samouczku utworzymy Localhost ograniczenia trasy. Ograniczenia trasy Localhost jest zgodny tylko żądań wysyłanych z komputera lokalnego. Żądania zdalne z przez Internet nie zostały dopasowane.

Implementując interfejs IRouteConstraint implementowania ograniczenia trasy niestandardowej. Jest to bardzo prosty interfejs, który opisuje pojedynczej metody:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Metoda zwraca wartość logiczną. Po powrocie False trasy skojarzonych z ograniczeniem nie będzie zgodne żądanie przeglądarki.

Ograniczenie Localhost znajduje się w ofercie 1.

**Listing 1 - LocalhostConstraint.vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

Ograniczenia w ofercie 1 korzysta z właściwości IsLocal udostępnianych przez klasy HttpRequest. Ta właściwość zwraca wartość true, gdy adres IP żądania jest albo 127.0.0.1 lub adres IP żądania jest taka sama jak adres IP serwera.

Możesz użyć niestandardowego ograniczenia w obrębie trasy zdefiniowanej w pliku Global.asax. Plik Global.asax w ofercie 2 używa ograniczenie Localhost, aby uniemożliwić osobom żądania na stronie administratora, chyba że dokonają żądania z serwera lokalnego. Na przykład żądanie /Admin/DeleteAll zakończy się niepowodzeniem, gdy na serwerze zdalnym.

**Wyświetlanie listy 2 - w pliku Global.asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Ograniczenie Localhost jest używane w definicji trasy administratora. Ta trasa nie będzie można dopasować do podwyrażenia żądania zdalnego przeglądarki. Należy pamiętać, jednak czy innych tras zdefiniowanych w pliku Global.asax może pasuje do tego samego żądania. Jest ważne dowiedzieć się, że ograniczenie zapobiega określonej trasy pasujące żądanie i nie wszystkie trasy zdefiniowanej w pliku Global.asax.

Należy zauważyć, że domyślna trasa zostały oznaczone komentarzami w pliku Global.asax w ofercie 2. Jeśli uwzględniony trasa domyślna trasa domyślna umożliwi dopasowanie żądania dla kontrolera administratora. W takim przypadku użytkownicy zdalni nadal może wywołać akcji kontrolera administratora, nawet jeśli ich żądania nie odpowiada trasy administratora.

> [!div class="step-by-step"]
> [Poprzednie](creating-a-route-constraint-vb.md)
