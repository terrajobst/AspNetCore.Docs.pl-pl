---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Utworzenie ograniczenia trasy niestandardowe (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: Stephen Walther przedstawiono sposób tworzenia tras niestandardowych ograniczenia. Możemy wdrożyć prosty ograniczenie niestandardowych, które zapobiega trasę dopasowywane w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c120a102b117433b6774f2ea7800f1c4a609f8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874346"
---
<a name="creating-a-custom-route-constraint-c"></a>Utworzenie ograniczenia trasy niestandardowe (C#)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther przedstawiono sposób tworzenia tras niestandardowych ograniczenia. Wdrożymy proste ograniczenie niestandardowych, które zapobiega trasę filtrowanego, gdy żądanie przeglądarki na komputerze zdalnym.


Celem tego samouczka jest aby zademonstrować, jak można utworzyć ograniczenia tras niestandardowych. Ograniczenia trasy niestandardowe umożliwia uniemożliwić filtrowanego, chyba że niektóre warunek niestandardowy jest dopasowywany trasy.

W tym samouczku utworzymy ograniczenia trasy Localhost. Ograniczenia trasy Localhost zgodny tylko żądań z komputera lokalnego. Żądania zdalne z Internetu są niezgodne.

Ograniczenia trasy niestandardowe można wdrożyć przy implementującej interfejs interfejs IRouteConstraint. Jest to bardzo proste interfejsu, który opisuje metodę pojedynczego:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

Metoda zwraca wartość logiczną. Jeśli wartość false, skojarzonych z ograniczeniem trasy nie pasuje do żądania przeglądarki.

Ograniczenie Localhost znajduje się w 1 wyświetlania.

**Wyświetlanie listy 1 - LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

Ograniczenia w 1 wyświetlania korzysta z właściwości IsLocal udostępniane przez klasę HttpRequest. Ta właściwość zwraca wartość true, gdy adres IP żądania jest albo 127.0.0.1 lub adres IP żądania jest taki sam jak adres IP serwera.

Możesz użyć niestandardowego ograniczenia w obrębie trasy zdefiniowane w pliku Global.asax. Plik Global.asax wyświetlania 2 używa ograniczenia Localhost aby uniemożliwić osobom żądania strony administratora, chyba że finalizowania żądania z lokalnego serwera. Na przykład żądania /Admin/DeleteAll zakończy się niepowodzeniem, gdy na serwerze zdalnym.

**Wyświetlanie listy 2 - pliku Global.asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

Warunek ograniczający Localhost jest używany w definicji trasy administratora. Nie można dopasować tej trasy przez żądanie przeglądarki zdalnego. Należy pamiętać, jednak, że innych tras zdefiniowanych w pliku Global.asax może pasuje do tego samego żądania. Należy zrozumieć ograniczenie uniemożliwia określonej trasy dopasowywanie na żądanie, a nie wszystkich tras określonych w pliku Global.asax.

Zwróć uwagę, że trasy domyślnej zostały oznaczone komentarzami z pliku Global.asax wyświetlania 2. Jeśli dołączysz trasa domyślna trasa domyślna umożliwi dopasowanie żądania dla administratora kontrolera. W takim przypadku użytkownicy zdalni nadal może wywołać akcji kontrolera administratora, nawet jeśli ich żądania nie pasuje trasy administratora.

> [!div class="step-by-step"]
> [Poprzednie](creating-a-route-constraint-cs.md)
> [dalej](asp-net-mvc-controller-overview-vb.md)
