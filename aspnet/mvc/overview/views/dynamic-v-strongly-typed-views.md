---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamiczne v. Silnie Typizowane widoków | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "26565532"
---
<a name="dynamic-v-strongly-typed-views"></a>Dynamiczne v. Jednoznacznie widoków
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

Istnieją trzy sposoby przekazywania informacji z kontrolera do widoku w programie ASP.NET MVC 3:

1. Jako obiekt jednoznacznie modelu.
2. Jako typu dynamicznego (przy użyciu @model dynamiczny)
3. Przy użyciu obiekt ViewBag

Napisanych I prostą aplikację MVC 3 pierwszych blogu porównać i kontrastu widoki dynamiczne i silnie typizowaną. Kontroler rozpoczyna się od prostego listę blogów:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Kliknij prawym przyciskiem myszy w metodzie IndexNotStonglyTyped() i Dodawanie widoku Razor.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Upewnij się, że **utworzyć widok jednoznacznie** pole nie jest zaznaczone. Wynikowa widok nie zawiera wiele:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Ponieważ używamy dynamiczne i silnie typizowanego widoku intellisense nie pomoże nam. Kompletny kod jest pokazany poniżej:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Teraz dodamy silnie typizowanego widoku. Dodaj następujący kod do kontrolera:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Należy zauważyć, że jest dokładnie tego samego zwracany View(topBlogs); wywołanie jako-silnie typizowanego widoku. Kliknij prawym przyciskiem myszy wewnątrz *StonglyTypedIndex()* i wybierz **Dodaj widok**. Tym razem wybierz **Blog** klasa modelu, a następnie wybierz **listy** jako szablon szkieletu.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

Wewnątrz nowy szablon widoku uzyskujemy obsługę funkcji intellisense.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Można pobrać projektu c# [tutaj](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
