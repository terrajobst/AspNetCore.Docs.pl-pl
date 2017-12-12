---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Utworzenie ograniczenia trasy niestandardowe (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: "Stephen Walther przedstawiono sposób tworzenia tras niestandardowych ograniczenia. Możemy wdrożyć prosty ograniczenie niestandardowych, które zapobiega trasę dopasowywane w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 41b04c1fea267e7ee9f8a0b1c2f0d4fe4bb96d15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-route-constraint-vb"></a>Utworzenie ograniczenia trasy niestandardowe (VB)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther przedstawiono sposób tworzenia tras niestandardowych ograniczenia. Wdrożymy proste ograniczenie niestandardowych, które zapobiega trasę filtrowanego, gdy żądanie przeglądarki na komputerze zdalnym.


Celem tego samouczka jest aby zademonstrować, jak można utworzyć ograniczenia tras niestandardowych. Ograniczenia trasy niestandardowe umożliwia uniemożliwić filtrowanego, chyba że niektóre warunek niestandardowy jest dopasowywany trasy.

W tym samouczku utworzymy ograniczenia trasy Localhost. Ograniczenia trasy Localhost zgodny tylko żądań z komputera lokalnego. Żądania zdalne z Internetu są niezgodne.

Ograniczenia trasy niestandardowe można wdrożyć przy implementującej interfejs interfejs IRouteConstraint. Jest to bardzo proste interfejsu, który opisuje metodę pojedynczego:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Metoda zwraca wartość logiczną. Jeśli zwróci wartość False, skojarzonych z ograniczeniem trasy nie pasuje do żądania przeglądarki.

Ograniczenie Localhost znajduje się w 1 wyświetlania.

**Wyświetlanie listy 1 - LocalhostConstraint.vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

Ograniczenia w 1 wyświetlania korzysta z właściwości IsLocal udostępniane przez klasę HttpRequest. Ta właściwość zwraca wartość true, gdy adres IP żądania jest albo 127.0.0.1 lub adres IP żądania jest taki sam jak adres IP serwera.

Możesz użyć niestandardowego ograniczenia w obrębie trasy zdefiniowane w pliku Global.asax. Plik Global.asax wyświetlania 2 używa ograniczenia Localhost aby uniemożliwić osobom żądania strony administratora, chyba że finalizowania żądania z lokalnego serwera. Na przykład żądania /Admin/DeleteAll zakończy się niepowodzeniem, gdy na serwerze zdalnym.

**Wyświetlanie listy 2 - pliku Global.asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Warunek ograniczający Localhost jest używany w definicji trasy administratora. Nie można dopasować tej trasy przez żądanie przeglądarki zdalnego. Należy pamiętać, jednak, że innych tras zdefiniowanych w pliku Global.asax może pasuje do tego samego żądania. Należy zrozumieć ograniczenie uniemożliwia określonej trasy dopasowywanie na żądanie, a nie wszystkich tras określonych w pliku Global.asax.

Zwróć uwagę, że trasy domyślnej zostały oznaczone komentarzami z pliku Global.asax wyświetlania 2. Jeśli dołączysz trasa domyślna trasa domyślna umożliwi dopasowanie żądania dla administratora kontrolera. W takim przypadku użytkownicy zdalni nadal może wywołać akcji kontrolera administratora, nawet jeśli ich żądania nie pasuje trasy administratora.

>[!div class="step-by-step"]
[Poprzednie](creating-a-route-constraint-vb.md)
