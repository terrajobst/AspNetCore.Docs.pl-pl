---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 'Jak: utworzyć pomocnika niestandardowy kod HTML dla aplikacji MVC | Microsoft Docs'
author: rick-anderson
description: W tym wideo Pels Krzysztof przedstawiono sposób tworzenia niestandardowych HtmlHelper, która nie jest dostępna w zestawie standardowe w aplikacji MVC. Pierwszy, tawienia aplikacji MVC próbki...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/11/2009
ms.topic: article
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 92faa04e1eefec0852604d51987ddaa9ee58838a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a>Jak: utworzyć pomocnika niestandardowy kod HTML dla aplikacji MVC
====================
przez [Pels Krzysztof](https://twitter.com/chrispels)

W tym wideo Pels Krzysztof przedstawiono sposób tworzenia niestandardowych HtmlHelper, która nie jest dostępna w zestawie standardowe w aplikacji MVC. Po pierwsze przykładowej aplikacji MVC jest tworzony z pokaz kontrolera i widoku, aby sprawdzić HtmlHelper niestandardowych. Następnie moduł jest tworzony z publicznego funkcja, która jest metodą rozszerzenia, który reprezentuje implementację HtmlHelper niestandardowych. Niestandardowe pomocnika służy do tworzenia `<img>` znaczników na stronie i odbiera kilka parametrów dla ruchu przychodzącego, takich jak identyfikator, adres url i tekst alternatywny obrazu znacznika. Logika jest dodawane do funkcji dla zwracania ukończonej `<img>` tagu z określonymi informacjami. Następnie HtmlHelper niestandardowych na stronie pokaz służy do wyświetlania obrazu. Na koniec niestandardowych HtmlHelper jest rozszerzona w celu uwzględnienia wielu zastąpienia konstruktora, które zapewniają elastyczność więcej łatwe tworzenie różnych `<img>` tagów.

[&#9654;Obejrzyj klip wideo (minuty 18)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> [Poprzednie](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [dalej](how-do-i-work-with-model-binders-in-an-mvc-application.md)
