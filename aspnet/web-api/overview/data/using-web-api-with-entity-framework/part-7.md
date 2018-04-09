---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Tworzenie widoku (UI) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 5052d7cca4a5c12a9ea56eb929d4794b19e82603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="create-the-view-ui"></a>Tworzenie widoku (UI)
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji rozpocznie zdefiniuj kod HTML dla aplikacji, a następnie Dodaj powiązanie danych pomiędzy HTML a model widoku.

Otwórz plik Views/Home/Index.cshtml. Zamień całą zawartość tego pliku poniżej.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Większość `div` elementy są związane z [Bootstrap](http://getbootstrap.com/) style. Ważne elementy są pokazane z `data-bind` atrybutów. Ten atrybut łączy HTML na model widoku.

Na przykład:

[!code-html[Main](part-7/samples/sample2.html)]

W tym przykładzie &quot; `text` &quot; powiązanie przyczyny `<p>` element, aby wyświetlić wartość `error` właściwość z modelu widoku. Odwołania, który `error` został zadeklarowany jako `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Gdy nowa wartość jest przypisywana do `error`, odcinania aktualizuje tekst w `<p>` elementu.

`foreach` Powiązania informuje odcinania pętli zawartość `books` tablicy. Dla każdego elementu w tablicy, tworzy nową Knockout &lt;li&gt; elementu. Powiązania w kontekście `foreach` odwoływać się do właściwości w elemencie tablicy. Na przykład:

[!code-html[Main](part-7/samples/sample4.html)]

W tym miejscu `text` powiązania odczytuje właściwość Autor każdego książki.

Jeśli uruchomisz aplikację teraz powinien wyglądać następująco:

![](part-7/_static/image1.png)

Lista książek ładuje asynchronicznie, po pobraniu strony. Teraz &quot;szczegóły&quot; łącza nie działają. Ta funkcja zostanie dodany w następnej sekcji.

> [!div class="step-by-step"]
> [Poprzednie](part-6.md)
> [dalej](part-8.md)
