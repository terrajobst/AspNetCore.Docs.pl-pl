---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamiczne v. Silnie Typizowane widoki | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 941cb24b81721eb75a8f7150ddb17acf71287da3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822185"
---
<a name="dynamic-v-strongly-typed-views"></a>Dynamiczne v. Silnie Typizowane widoki
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

Istnieją trzy sposoby przekazywania informacji z kontrolera do widoku w ASP.NET MVC 3:

1. Jako obiekt silnie typizowany model.
2. Jako typu dynamicznego (przy użyciu @model dynamiczny)
3. Za pomocą obiekt ViewBag

Moje recenzje prostą aplikację MVC 3 pierwszych blogu do Porównaj i zestaw widoki dynamiczne i silnie typizowane. Kontroler rozpoczyna się od prostego listę blogów:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Kliknij prawym przyciskiem myszy w metodzie IndexNotStonglyTyped() i Dodawanie widoku Razor.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Upewnij się, że **utworzyć widok silnie typizowane** pole nie jest zaznaczone. Wynikowym widoku nie zawiera wiele:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Ponieważ używamy dynamiczne i silnie typizowanego widoku, funkcja intellisense nie pomagają nam. Kompletny kod jest pokazany poniżej:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Teraz dodamy silnie typizowanego widoku. Dodaj następujący kod na kontrolerze:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Zwróć uwagę, że jest dokładnie ten sam zwracany View(topBlogs); Wywołaj jako — silnie typizowanego widoku. Kliknij prawym przyciskiem myszy wewnątrz *StonglyTypedIndex()* i wybierz **Dodaj widok**. Tym razem wybierz pozycję **Blog** klasa modelu, a następnie wybierz pozycję **listy** jako szablon szkieletu.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

Wewnątrz szablonu widoku uzyskujemy obsługę funkcji intellisense.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Projekt c# można pobrać [tutaj](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
