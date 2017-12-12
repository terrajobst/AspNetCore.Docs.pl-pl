---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: "Wyświetl szczegóły elementu | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 0b6ae9384843712cae824ea662b984a40f021e57
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="display-item-details"></a>Wyświetl szczegóły elementu
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji dodasz możliwość wyświetlania szczegółów dla każdej księgi. W app.js Dodaj następujący kod służący do modelu widoku:

[!code-javascript[Main](part-8/samples/sample1.js)]

W Views/Home/Index.cshtml Dodaj element wiązania danych do łącza Szczegóły:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Powoduje to powiązanie obsługi kliknięcia dla &lt;&gt; elementu `getBookDetail` funkcja na model widoku.

W tym samym pliku Zastąp następujące wycinka:

[!code-html[Main](part-8/samples/sample3.html)]

na kod:

[!code-html[Main](part-8/samples/sample4.html)]

Ten kod znaczników tworzy tabelę, która jest powiązany z danymi właściwości `detail` widocznych w modelu widoku.

"&lt;!--Ko -&gt; &quot; składni pozwala umieścić powiązanie odcinania poza elementem modelu DOM. W takim przypadku `if` powiązania spowoduje, że ta sekcja znaczników, który będzie wyświetlany tylko wtedy, gdy `details` jest różna od null.

[!code-html[Main](part-8/samples/sample5.html)]

Teraz, jeśli Uruchom aplikację i kliknij jeden z &quot;szczegółów&quot; łącza, aplikacji będą wyświetlane szczegóły książki.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

>[!div class="step-by-step"]
[Poprzednie](part-7.md)
[dalej](part-9.md)
