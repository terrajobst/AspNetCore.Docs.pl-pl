---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Jak mogę] Zastąp HTML na stronie ASP.NET za pomocą właściwości Reponse.Filter | Dokumentacja firmy Microsoft'
author: rick-anderson
description: W tym wideo pikseli Chris pokazuje, jak za pomocą właściwości Reponse.Filter przechwycenia i alter HTML są wysyłane do strony. Po pierwsze przykładowa strona jest tworzony w...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 0e8ae8b62bbb4ac1fc7e942cf3dd0438383cb510
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754353"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="96eb9-104">[Jak mogę] Zastąp HTML na stronie ASP.NET za pomocą właściwości Reponse.Filter</span><span class="sxs-lookup"><span data-stu-id="96eb9-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="96eb9-105">przez [Chris pikseli](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="96eb9-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="96eb9-106">W tym wideo pikseli Chris pokazuje, jak za pomocą właściwości Reponse.Filter przechwycenia i alter HTML są wysyłane do strony.</span><span class="sxs-lookup"><span data-stu-id="96eb9-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="96eb9-107">Przykładowa strona jest utworzenia z tekstem proste.</span><span class="sxs-lookup"><span data-stu-id="96eb9-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="96eb9-108">Następnie niestandardowej klasy Stream jest tworzony, który służy jako strumień zastąpienia dla bieżącego strumienia, które są wysyłane do przeglądarki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="96eb9-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="96eb9-109">W tej klasie niestandardowe strumienia zawartość strony są pobierane ze strumienia, zmieniona, a następnie zapisywane do strumienia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="96eb9-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="96eb9-110">W tym niestandardowej klasy Stream metody zapisu jest dostosowany do Zastąp kod HTML w strumienia odpowiedzi podstawowego, a tym samym zmiany, co jest wysyłane do przeglądarki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="96eb9-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="96eb9-111">Na koniec nową klasę strumienia jest przypisany do właściwości Response.Filter na stronie\_zdarzeniem ładowania, a tym samym, zapewniając mechanizm zmiany zawartości strony.</span><span class="sxs-lookup"><span data-stu-id="96eb9-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="96eb9-112">&#9654;Obejrzyj film wideo (minuty 13)</span><span class="sxs-lookup"><span data-stu-id="96eb9-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
