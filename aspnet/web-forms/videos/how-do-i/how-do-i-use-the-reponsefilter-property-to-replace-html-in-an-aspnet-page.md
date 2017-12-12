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
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Jak i.] Aby zastąpić HTML strony ASP.NET należy użyć właściwości Reponse.Filter
====================
przez [Pels Krzysztof](https://twitter.com/chrispels)

W tym wideo Pels Krzysztof przedstawia sposób użycia właściwości Reponse.Filter przechwycenia i alter są wysyłane do strony HTML. Najpierw przykładową stronę jest tworzony z niektórych zwykły tekst. Niestandardowej klasy strumień jest tworzona, która służy jako strumień zastąpienia dla bieżącego strumienia wysyłany do przeglądarki użytkownika. W tej klasie niestandardowych strumieni zawartość strony są pobierane z strumienia, zmienić, a następnie zapisywane do strumienia odpowiedzi. W tej niestandardowej klasy strumienia metody zapisu jest dostosowana i zastąpić HTML w bazowy strumieniu odpowiedzi, a tym samym zmiany, co jest wysyłany do przeglądarki użytkownika. Ponadto nowa klasa strumień jest przypisany do właściwości Response.Filter na stronie\_zdarzeniem ładowania, w tym samym, podając mechanizm zmiany zawartości strony.

[&#9654; Obejrzyj klip wideo (minuty 13)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
