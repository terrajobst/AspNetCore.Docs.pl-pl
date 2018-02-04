---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: "Jak: utworzyć pomocnika niestandardowy kod HTML dla aplikacji MVC | Microsoft Docs"
author: rick-anderson
description: "W tym wideo Pels Krzysztof przedstawiono sposób tworzenia niestandardowych HtmlHelper, która nie jest dostępna w zestawie standardowe w aplikacji MVC. Pierwszy, tawienia aplikacji MVC próbki..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/11/2009
ms.topic: article
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 50a03799336636a8ba622b4ee3e8da99dcbc2708
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/03/2018
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="acf7f-105">Jak: utworzyć pomocnika niestandardowy kod HTML dla aplikacji MVC</span><span class="sxs-lookup"><span data-stu-id="acf7f-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="acf7f-106">przez [Pels Krzysztof](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="acf7f-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="acf7f-107">W tym wideo Pels Krzysztof przedstawiono sposób tworzenia niestandardowych HtmlHelper, która nie jest dostępna w zestawie standardowe w aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="acf7f-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="acf7f-108">Po pierwsze przykładowej aplikacji MVC jest tworzony z pokaz kontrolera i widoku, aby sprawdzić HtmlHelper niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="acf7f-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="acf7f-109">Następnie moduł jest tworzony z publicznego funkcja, która jest metodą rozszerzenia, który reprezentuje implementację HtmlHelper niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="acf7f-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="acf7f-110">Niestandardowe pomocnika służy do tworzenia `<img>` znaczników na stronie i odbiera kilka parametrów dla ruchu przychodzącego, takich jak identyfikator, adres url i tekst alternatywny obrazu znacznika.</span><span class="sxs-lookup"><span data-stu-id="acf7f-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="acf7f-111">Logika jest dodawane do funkcji dla zwracania ukończonej `<img>` tagu z określonymi informacjami.</span><span class="sxs-lookup"><span data-stu-id="acf7f-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="acf7f-112">Następnie HtmlHelper niestandardowych na stronie pokaz służy do wyświetlania obrazu.</span><span class="sxs-lookup"><span data-stu-id="acf7f-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="acf7f-113">Na koniec niestandardowych HtmlHelper jest rozszerzona w celu uwzględnienia wielu zastąpienia konstruktora, które zapewniają elastyczność więcej łatwe tworzenie różnych `<img>` tagów.</span><span class="sxs-lookup"><span data-stu-id="acf7f-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="acf7f-114">&#9654; Obejrzyj klip wideo (minuty 18)</span><span class="sxs-lookup"><span data-stu-id="acf7f-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

>[!div class="step-by-step"]
<span data-ttu-id="acf7f-115">[Poprzednie](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[dalej](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="acf7f-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
