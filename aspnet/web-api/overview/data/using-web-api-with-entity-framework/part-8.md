---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Wyświetl szczegóły elementu | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 268c44f842cc2beb32a0a3e4c74b83b7ca9fd787
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375189"
---
<a name="display-item-details"></a>Wyświetlanie szczegółów elementu
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji dodasz możliwości, aby wyświetlić szczegóły dotyczące każdej książki. W app.js Dodaj poniższy kod do modelu widoku:

[!code-javascript[Main](part-8/samples/sample1.js)]

W Views/Home/Index.cshtml Dodaj element wiązania danych do łącza Szczegóły:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Powoduje to powiązanie programu obsługi kliknij &lt;&gt; elementu `getBookDetail` funkcji na model widoku.

W tym samym pliku Zamień następujące wycinka:

[!code-html[Main](part-8/samples/sample3.html)]

na kod:

[!code-html[Main](part-8/samples/sample4.html)]

Ten kod znaczników tworzy tabelę, która jest powiązany z danymi właściwości `detail` zauważalne w modelu widoku.

"&lt;!--Ko —&gt; &quot; składni pozwala objąć powiązanie Knockout poza elementu DOM. W tym przypadku `if` powiązania powoduje, że ta część znaczników, które mają być wyświetlane tylko wtedy, gdy `details` jest różna od null.

[!code-html[Main](part-8/samples/sample5.html)]

Teraz uruchom aplikację i kliknij jeden z &quot;szczegółów&quot; łącza, aplikacji spowoduje wyświetlenie szczegółów książki.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Poprzednie](part-7.md)
> [dalej](part-9.md)
