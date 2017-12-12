---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: "Część 5: Tworzenie dynamicznego interfejsu użytkownika z Knockout.js | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20ebdb1b8ba710e0fbc6040f7cd4064b44658c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Część 5: Tworzenie dynamicznego interfejsu użytkownika z Knockout.js
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Tworzenie dynamicznego interfejsu użytkownika z Knockout.js

W tej sekcji użyjemy Knockout.js Dodawanie funkcji do widoku administracyjnej.

[Knockout.js](http://knockoutjs.com/) biblioteka języka Javascript, która ułatwia powiązanie kontrolek HTML z danymi. Knockout.js korzysta ze wzorca Model-View-ViewModel (MVVM).

- *Modelu* jest po stronie serwera reprezentację danych w domenie business (w przypadku, produktów i zamówienia).
- *Widoku* to warstwa prezentacji (HTML).
- *Model widoku* jest obiektem Javascript, która przechowuje dane modelu. Model widoku jest abstrakcji kodu interfejsu użytkownika. Go nie ma informacji o reprezentacji w formacie HTML. Zamiast tego reprezentuje funkcje abstrakcyjne widoku, takie jak "listę elementów".

Widok jest powiązany z danymi model widoku. Aktualizacje na model widoku są automatycznie odzwierciedlane w widoku. Model widoku również pobiera zdarzenia w widoku, takie jak kliknięcia przycisków i wykonuje operacje na model, takich jak tworzenie zamówienia.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Najpierw zdefiniujemy model widoku. Po tym firma Microsoft będzie powiązać kod znaczników HTML na model widoku.

Dodaj poniższą sekcję Razor do Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

W tej sekcji można dodać dowolne miejsce w pliku. Podczas renderowania widoku sekcji pojawi się w dolnej części strony HTML, kliknij prawym przyciskiem myszy przed tagiem zamykającym &lt;/body&gt; tagu.

Wszystkie skryptu ta strona zostanie umieszczona wewnątrz tagu skryptu wskazywanym przez komentarz:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Najpierw należy zdefiniować klasę modelu widoku:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** jest specjalnym rodzajem obiektu w odcinania o nazwie *według*. Z [dokumentacji Knockout.js](http://knockoutjs.com/documentation/observables.html): zauważalny jest "JavaScript obiekt, który może powiadomić subskrybentów o zmianach." Po zmianie zawartości zauważalny, widok jest automatycznie aktualizowany do dopasowania.

Aby wypełnić `products` tablicy, przesyłania żądania AJAX do interfejsu API sieci web. Odwołaj możemy przechowywane podstawowy identyfikator URI dla interfejsu API w zbiorze widoku (zobacz [część 4](using-web-api-with-entity-framework-part-4.md) samouczka).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Następnie dodaj funkcje do modelu widoku do tworzenia, aktualizacji i usuwania produktów. Te funkcje przesyłania wywołania AJAX do interfejsu API sieci web i użyć wyników, aby zaktualizować model widoku.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Teraz najważniejszy element: gdy DOM jest fulled załadowany, wywołanie **ko.applyBindings** funkcji i podaj nowe wystąpienie klasy `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings** metody aktywuje Knockout i tworzącej powiązania modelu widoku do widoku.

Teraz, gdy mamy model widoku, można utworzyć powiązania. W Knockout.js, możesz to zrobić przez dodanie `data-bind` atrybutów elementów HTML. Na przykład, aby powiązać listy HTML do tablicy, należy użyć `foreach` powiązania:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach` Powiązanie iteruje tablicy i tworzy podrzędne elementy dla każdego obiektu w tablicy. Wiązania na elementy podrzędne mogą odwoływać się do właściwości tablicy obiektów.

Dodaj następujące powiązania do listy "aktualizacji produktów":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>` Element występuje w zakresie **foreach** powiązania. Czy oznacza odcinania będzie renderować element raz dla każdego produktu w `products` tablicy. Wszystkie powiązania w ramach `<li>` element się odwoływać do tego wystąpienia produktu. Na przykład `$data.Name` odwołuje się do `Name` właściwości dla produktu.

Aby ustawić wartości danych wejściowych tekstu, należy użyć `value` powiązania. Przyciski są powiązane z funkcji w widoku modelu przy użyciu `click` powiązania. Wystąpienie produktu są przekazywane jako parametr do każdej funkcji. Aby uzyskać więcej informacji [dokumentacji Knockout.js](http://knockoutjs.com/documentation/observables.html) ma dobrej opisy różnych powiązań.

Następnie Dodaj powiązanie dla **przesłać** zdarzenia w formularzu Dodawanie produktu:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Wymaga to powiązanie `create` funkcja na model widoku do tworzenia nowych produktów.

W tym miejscu jest kompletny kod dla widoku administracyjnej:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Uruchom aplikację, zaloguj się przy użyciu konta administratora, a następnie kliknij łącze "Admin". Należy zapoznać się z listą produktów i można tworzyć, aktualizować lub usuwać produktów.

>[!div class="step-by-step"]
[Poprzednie](using-web-api-with-entity-framework-part-4.md)
[dalej](using-web-api-with-entity-framework-part-6.md)
