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
<a name="display-item-details"></a><span data-ttu-id="82b81-102">Wyświetl szczegóły elementu</span><span class="sxs-lookup"><span data-stu-id="82b81-102">Display Item Details</span></span>
====================
<span data-ttu-id="82b81-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="82b81-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="82b81-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="82b81-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="82b81-105">W tej sekcji dodasz możliwość wyświetlania szczegółów dla każdej księgi.</span><span class="sxs-lookup"><span data-stu-id="82b81-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="82b81-106">W app.js Dodaj następujący kod służący do modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="82b81-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="82b81-107">W Views/Home/Index.cshtml Dodaj element wiązania danych do łącza Szczegóły:</span><span class="sxs-lookup"><span data-stu-id="82b81-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="82b81-108">Powoduje to powiązanie obsługi kliknięcia dla &lt;&gt; elementu `getBookDetail` funkcja na model widoku.</span><span class="sxs-lookup"><span data-stu-id="82b81-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="82b81-109">W tym samym pliku Zastąp następujące wycinka:</span><span class="sxs-lookup"><span data-stu-id="82b81-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="82b81-110">na kod:</span><span class="sxs-lookup"><span data-stu-id="82b81-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="82b81-111">Ten kod znaczników tworzy tabelę, która jest powiązany z danymi właściwości `detail` widocznych w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="82b81-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="82b81-112">"&lt;!--Ko -&gt; &quot; składni pozwala umieścić powiązanie odcinania poza elementem modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="82b81-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="82b81-113">W takim przypadku `if` powiązania spowoduje, że ta sekcja znaczników, który będzie wyświetlany tylko wtedy, gdy `details` jest różna od null.</span><span class="sxs-lookup"><span data-stu-id="82b81-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="82b81-114">Teraz, jeśli Uruchom aplikację i kliknij jeden z &quot;szczegółów&quot; łącza, aplikacji będą wyświetlane szczegóły książki.</span><span class="sxs-lookup"><span data-stu-id="82b81-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

>[!div class="step-by-step"]
<span data-ttu-id="82b81-115">[Poprzednie](part-7.md)
[dalej](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="82b81-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
