---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: "[Jak i.] Aby zastąpić HTML strony ASP.NET należy użyć właściwości Reponse.Filter | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym wideo Pels Krzysztof przedstawia sposób użycia właściwości Reponse.Filter przechwycenia i alter są wysyłane do strony HTML. Po pierwsze przykładową stronę jest tworzony w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="8c552-104">[Jak i.] Aby zastąpić HTML strony ASP.NET należy użyć właściwości Reponse.Filter</span><span class="sxs-lookup"><span data-stu-id="8c552-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="8c552-105">przez [Pels Krzysztof](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="8c552-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="8c552-106">W tym wideo Pels Krzysztof przedstawia sposób użycia właściwości Reponse.Filter przechwycenia i alter są wysyłane do strony HTML.</span><span class="sxs-lookup"><span data-stu-id="8c552-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="8c552-107">Najpierw przykładową stronę jest tworzony z niektórych zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="8c552-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="8c552-108">Niestandardowej klasy strumień jest tworzona, która służy jako strumień zastąpienia dla bieżącego strumienia wysyłany do przeglądarki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8c552-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="8c552-109">W tej klasie niestandardowych strumieni zawartość strony są pobierane z strumienia, zmienić, a następnie zapisywane do strumienia odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8c552-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="8c552-110">W tej niestandardowej klasy strumienia metody zapisu jest dostosowana i zastąpić HTML w bazowy strumieniu odpowiedzi, a tym samym zmiany, co jest wysyłany do przeglądarki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8c552-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="8c552-111">Ponadto nowa klasa strumień jest przypisany do właściwości Response.Filter na stronie\_zdarzeniem ładowania, w tym samym, podając mechanizm zmiany zawartości strony.</span><span class="sxs-lookup"><span data-stu-id="8c552-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="8c552-112">&#9654; Obejrzyj klip wideo (minuty 13)</span><span class="sxs-lookup"><span data-stu-id="8c552-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
