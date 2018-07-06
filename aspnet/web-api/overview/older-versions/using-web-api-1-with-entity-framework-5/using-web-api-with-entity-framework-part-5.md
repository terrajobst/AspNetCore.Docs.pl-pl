---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Część 5: Tworzenie dynamicznego interfejsu użytkownika z użyciem Knockout.js | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 7d8bd4732ecf73c14251ceebfd667310f6e197b4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801696"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Część 5: Tworzenie dynamicznego interfejsu użytkownika z użyciem Knockout.js
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Tworzenie dynamicznego interfejsu użytkownika z użyciem Knockout.js

W tej sekcji użyjemy struktura Knockout.js Dodawanie funkcji do widoku administratora.

[Struktura Knockout.js](http://knockoutjs.com/) to biblioteka języka Javascript, która ułatwia powiązania kontrolek HTML do danych. Struktura Knockout.js używa wzorca Model-View-ViewModel (MVVM).

- *Modelu* jest po stronie serwera reprezentacja danych w domenie firmy (w naszym przypadku, produkty i zamówienia).
- *Widoku* to warstwa prezentacji (HTML).
- *Model widoku* jest obiekt Javascript, która przechowuje dane modelu. Model widoku jest abstrakcji kodu interfejsu użytkownika. Go nie zna reprezentacji w formacie HTML. Zamiast tego reprezentuje abstrakcyjnej funkcji widoku, takie jak "Lista elementów".

Widok jest powiązany z danymi model widoku. Model widoku aktualizacje są automatycznie odzwierciedlane w widoku. Model widoku również pobiera zdarzenia z widoku, takich jak kliknięcia przycisków i wykonuje operacje na modelu, na przykład utworzyć zamówienie.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Najpierw zdefiniujemy model widoku. Po tym firma Microsoft będzie powiązać kod znaczników HTML model widoku.

Dodaj następującą sekcję Razor do Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

W tej sekcji można dodać w dowolnym miejscu w pliku. Podczas renderowania widoku sekcji pojawi się w dolnej części strony HTML bezpośrednio przed zamykającym &lt;/body&gt; tagu.

Wszystkie skrypt na tej stronie zaczną się wewnątrz tagu wskazywanym przez komentarz:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Najpierw należy zdefiniować klasę modelu widoku:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** to specjalny rodzaj obiektu w Knockout, o nazwie *obserwowalnymi*. Z [dokumentacji struktura Knockout.js](http://knockoutjs.com/documentation/observables.html): zauważalny jest "JavaScript obiekt, który może generować powiadomienia subskrybentów o zmianach." Po zmianie zawartości możliwość obserwowania, widok jest automatycznie aktualizowana do dopasowania.

Aby wypełnić `products` tablicy, przesyłania żądania AJAX do internetowego interfejsu API. Odwołaj, firma Microsoft przechowywane podstawowy identyfikator URI dla interfejsu API w zbiorze widoku (zobacz [część 4](using-web-api-with-entity-framework-part-4.md) tego samouczka).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Następnie dodaj funkcje do modelu widoku na tworzenie, aktualizowanie i usuwanie produktów. Te funkcje przesyłania wywołania AJAX do internetowego interfejsu API i użyj wyników, aby zaktualizować model widoku.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Teraz najważniejszy element: gdy DOM jest fulled załadowany, wywołanie **ko.applyBindings** działać, a następnie przekazać nowe wystąpienie klasy `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings** metoda aktywuje Knockout i tworzącej powiązania modelu widoku do widoku.

Teraz, gdy model widoku, możemy utworzyć powiązania. W struktura Knockout.js, możesz to zrobić, dodając `data-bind` atrybutów elementów HTML. Na przykład, aby powiązać lista HTML do tablicy, należy użyć `foreach` powiązania:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach` Powiązanie dokonuje iteracji tablicy i tworzy elementy dla każdego obiektu podrzędnego w tablicy. Wiązania na elementy podrzędne mogą odwoływać się do właściwości obiektów w tablicy.

Dodaj następujące powiązania do listy "update produkty":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>` Element występuje zakresie **foreach** powiązania. Czy oznacza, że będą Knockout renderować element raz dla każdego produktu w `products` tablicy. Wszystkie powiązania w ramach `<li>` element odwołują się do tego wystąpienia produktu. Na przykład `$data.Name` odwołuje się do `Name` właściwość nad produktem.

Aby ustawić wartości tekstowe dane wejściowe, użyj `value` powiązania. Przyciski są powiązane z funkcji na widok modelu przy użyciu `click` powiązania. Wystąpienie produktu jest przekazywany jako parametr do każdej funkcji. Aby uzyskać więcej informacji [dokumentacji struktura Knockout.js](http://knockoutjs.com/documentation/observables.html) ma dobrą opisy różnych powiązania.

Następnie Dodaj powiązanie dla **przesłać** zdarzeń w formularzu Dodawanie produktu:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Wywołuje to powiązanie `create` funkcji na podstawie modelu widoku, aby utworzyć nowy produkt.

Oto kompletny kod dla widoku administratora:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Uruchom aplikację, zaloguj się przy użyciu konta administratora, a następnie kliknij link "Admin". Należy zapoznać się z listą produktów i mieć możliwość utworzenia, aktualizacji lub usunięcia produktów.

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-4.md)
> [dalej](using-web-api-with-entity-framework-part-6.md)
