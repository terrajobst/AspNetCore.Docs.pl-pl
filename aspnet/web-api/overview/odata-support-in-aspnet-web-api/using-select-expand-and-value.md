---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Za pomocą $select, $expand and $value w programie ASP.NET Web API 2 OData | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/11/2013
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: d198ecf40155cba36204bc0810f4735aae6b100b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754696"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Za pomocą $select, $expand and $value w programie ASP.NET Web API 2 OData
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Składnik Web API 2 dodaje obsługę dla $expand, $select i opcje $value w protokole OData. Te opcje umożliwiają klientowi do kontrolowania reprezentacji, która go ponownie z serwera.

- **$expand** powoduje, że z powiązanymi obiektami być wbudowane w odpowiedzi.
- **$select** wybierze podzbiór właściwości do uwzględnienia w odpowiedzi.
- **$value** pobiera nieprzetworzonej wartości właściwości.

## <a name="example-schema"></a>Przykład schematu

W tym artykule, będziesz używać usługi OData, który definiuje trzy jednostki: produktów, dostawcy i kategorii. Każdy produkt ma jedną kategorię i jednego dostawcy.

![](using-select-expand-and-value/_static/image1.png)

Poniżej przedstawiono klasy C#, które definiują modeli jednostki:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Należy zauważyć, że `Product` klasy definiuje właściwości nawigacji dla `Supplier` i `Category`. `Category` Klasa definiuje właściwość nawigacji dla produktów w każdej kategorii.

Aby utworzyć punkt końcowy OData dla tego schematu, użyj szkieletu programu Visual Studio 2013, zgodnie z opisem w [Tworzenie punktu końcowego OData w interfejsie API sieci Web platformy ASP.NET](odata-v3/creating-an-odata-endpoint.md). Dodaj kontrolery oddzielny produkt, kategoria i dostawcy.

## <a name="enabling-expand-and-select"></a>Włączanie $Rozwiń i $select

W programie Visual Studio 2013 rusztowania Web API OData tworzy kontroler, automatycznie obsługuje $expand i $select. Odwołanie, poniżej przedstawiono wymagania dotyczące obsługi $Rozwiń i $select w kontrolerze.

Dla kolekcji, kontroler firmy `Get` metoda musi zwracać **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

W przypadku pojedynczych jednostek zwracają **SingleResult&lt;T&gt;**, gdzie T jest **IQueryable** zawierający zero lub jedną jednostkę.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Ponadto dekoracji swoje `Get` metod z **[Queryable]** atrybutu, jak pokazano w poprzednich fragmentach kodu. Alternatywnie wywołać **EnableQuerySupport** na **HttpConfiguration** obiektu podczas uruchamiania. (Aby uzyskać więcej informacji, zobacz [włączenie opcji zapytania OData](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Rozwiń przy użyciu ciągu $

Kiedy wykonujesz zapytanie OData jednostki lub kolekcji, domyślną odpowiedź nie zawiera powiązanych jednostek. Na przykład poniżej przedstawiono odpowiedź domyślną dla zestawu jednostek kategorie:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Jak widać, odpowiedź nie obejmuje wszystkie produkty, nawet jeśli jednostka kategoria zawiera łącze nawigacyjne produktów. Jednak klient może używać $Rozwiń, aby uzyskać listę produktów dla każdej kategorii. $Expand opcja znajduje się w ciągu zapytania żądania:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Teraz serwer będzie zawierać produktów dla każdej kategorii, wbudowany z kategorii. Oto ładunek odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Należy zauważyć, że każdy wpis w tablicy "wartość" zawiera listę produktów.

$Rozwiń opcję przyjmuje rozdzielonych przecinkami lista właściwości nawigacyjne do rozszerzenia. Następujące żądanie rozwija kategorii i dostawcy dla produktu.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Oto treści odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Możesz rozwinąć więcej niż jednego poziomu właściwości nawigacji. Poniższy przykład obejmuje wszystkie produkty dla kategorii, a także dostawcy dla każdego produktu.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Oto treści odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

Domyślnie interfejs API sieci Web ogranicza głębokość rozszerzenia maksymalną na 2. Która zapobiega wysyłaniu złożonych żądań, takich jak przez klienta `$expand=Orders/OrderDetails/Product/Supplier/Region`, który może być nieefektywne w celu wykonywania zapytań i tworzenia dużych odpowiedzi. Aby zastąpić domyślne, należy ustawić **MaxExpansionDepth** właściwość **[Queryable]** atrybutu.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Aby uzyskać więcej informacji na temat $ rozwiń węzeł opcji, zobacz [rozwiń System stosowanie opcji zapytania ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) w oficjalnej dokumentacji OData.

## <a name="using-select"></a>Za pomocą $select

Opcja $select określa podzbiór właściwości do uwzględnienia w treści odpowiedzi. Na przykład można pobrać tylko nazwa i cena każdego produktu, należy użyć następującej kwerendy:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Oto treści odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

Można połączyć $select i $expand w jednym zapytaniu. Upewnij się, że będą zawierać właściwości rozszerzonej w opcji $select. Na przykład następujące żądanie pobiera nazwę produktu i dostawcy.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Oto treści odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

Możesz również wybrać właściwości w ramach właściwości rozszerzonej. Następujące żądanie rozwija produktów i wybiera nazwę kategorii, a także nazwę produktu.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Oto treści odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Aby uzyskać więcej informacji na temat opcji $select, zobacz [wybierz opcję zapytania systemu ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) w oficjalnej dokumentacji OData.

## <a name="getting-individual-properties-of-an-entity-value"></a>Pobieranie właściwości poszczególne jednostki ($value)

Istnieją dwa sposoby dla klienta OData można pobrać wybranej właściwości z jednostką. Klienta można uzyskać wartość w formacie OData lub uzyskaj nieprzetworzonej wartości właściwości.

Następujące żądanie pobiera właściwości w formacie OData.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Poniżej przedstawiono przykładową odpowiedź w formacie JSON:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Aby uzyskać nieprzetworzonej wartości właściwości, należy dołączyć $value do identyfikatora URI:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Poniżej przedstawiono odpowiedzi. Należy zauważyć, że typ zawartości "text/plain" nie jest kodem JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Aby obsługiwać te zapytania w kontrolerze OData, Dodaj metodę o nazwie `GetProperty`, gdzie `Property` jest nazwą właściwości. Na przykład, czy nazwę metody pobierania właściwości Name `GetName`. Metoda powinna zwrócić wartość tej właściwości:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
