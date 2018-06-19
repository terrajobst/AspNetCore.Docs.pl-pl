---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Przy użyciu $select, rozwinąć $ i $value w programie ASP.NET Web API 2 OData | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566723"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Przy użyciu $select, rozwinąć $ i $value w programie ASP.NET Web API 2 OData
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Składnik Web API 2 dodaje obsługę dla $rozwiń $select i opcje OData $value. Te opcje Zezwalaj klientowi kontrolować reprezentacją go otrzymuje w odpowiedzi z serwera.

- **Rozwiń węzeł $** powoduje, że powiązanych jednostek jako wbudowany uwzględniony w odpowiedzi.
- **$select** wybiera podzbiór właściwości do uwzględnienia w odpowiedzi.
- **$value** pobiera nieprzetworzonej wartości właściwości.

## <a name="example-schema"></a>Przykład schematu

W tym artykule, można użyć usługi OData, który definiuje trzy jednostki: produktu, dostawca i kategorii. Każdy produkt ma jedną kategorię i jeden dostawca.

![](using-select-expand-and-value/_static/image1.png)

Poniżej przedstawiono definiujące modeli entity klas C#:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Zwróć uwagę, że `Product` klasa definiuje właściwości nawigacji dla `Supplier` i `Category`. `Category` Klasa definiuje właściwości nawigacji dla produktów w każdej kategorii.

Aby utworzyć punkt końcowy OData do tego schematu, użyj szkieletów programu Visual Studio 2013, zgodnie z opisem w [tworzenia punktu końcowego OData w ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md). Dodaj oddzielne kontrolerów produktu, kategorii i dostawcy.

## <a name="enabling-expand-and-select"></a>Włączanie $Rozwiń i $select

W programie Visual Studio 2013 rusztowania Web API OData tworzy kontrolerem tej automatycznie obsługuje Rozwiń $ i $select. Odwołania, poniżej przedstawiono wymagania dotyczące obsługi $Rozwiń i $select w kontrolerze.

Kolekcje kontrolera w `Get` metoda musi zwracać **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

W przypadku pojedynczych jednostek zwracają **SingleResult&lt;T&gt;**, gdzie T jest **IQueryable** zawierający zero lub jedną jednostkę.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Ponadto dekoracji Twojej `Get` metod **[Queryable]** atrybutu, jak pokazano w poprzednim wstawki kodu. Można również wywołać **EnableQuerySupport** na **HttpConfiguration** obiektu podczas uruchamiania. (Aby uzyskać więcej informacji, zobacz [włączenie opcji zapytania OData](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Przy użyciu $rozwiń

Określona w zapytaniu OData jednostkę lub kolekcji, domyślny nie ma powiązanych jednostek. Na przykład w tym miejscu jest domyślny dla zestawu jednostek kategorii:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Jak widać, odpowiedzi nie zawiera wszystkie produkty, nawet jeśli jednostka kategorii ma łącze nawigacyjne produktów. Jednak klient może używać $Rozwiń, aby uzyskać listę produktów dla każdej kategorii. Opcji $expand $ przechodzi w ciągu zapytania żądania:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Teraz serwer będzie zawierać produktów dla każdej kategorii, wbudowany z kategorii. Oto ładunku odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Zwróć uwagę, że każdy wpis w tablicy "wartość" zawiera listę produktów.

$Rozwiń opcję przyjmuje rozdzielone przecinkami lista właściwości nawigacyjne do rozszerzenia. Następujące żądania rozszerza zarówno kategorii i dostawcy dla produktu.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Oto treść odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Można rozwinąć więcej niż jeden poziom elementów właściwości nawigacji. Poniższy przykład zawiera wszystkich produktów dla kategorii, a także dostawcy dla każdego produktu.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Oto treść odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

Domyślnie interfejsu API sieci Web ogranicza głębokość rozszerzenia maksymalną 2. Który zapobiega wysyłaniu złożonych żądań, takich jak przez klienta `$expand=Orders/OrderDetails/Product/Supplier/Region`, może być nieefektywne do wykonywania kwerend i tworzenie dużych odpowiedzi. Aby zastąpić domyślną, ustaw **MaxExpansionDepth** właściwość **[Queryable]** atrybutu.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Aby uzyskać więcej informacji na temat $ opcji $expand, zobacz [rozwiń węzeł systemu opcji zapytania ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) w oficjalnej dokumentacji OData.

## <a name="using-select"></a>Przy użyciu $select

Opcja $select określa podzbiór właściwości, aby uwzględnić w treści odpowiedzi. Na przykład aby uzyskać tylko nazwę i ceny każdego produktu, należy użyć następującej kwerendy:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Oto treść odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

Możesz łączyć $select i $rozwiń w jednym zapytaniu. Upewnij się, że będą zawierać właściwości rozszerzonej w opcji $select. Na przykład następujące żądania pobiera nazwę produktu i dostawcy.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Oto treść odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

Możesz również wybrać właściwości w rozszerzonych właściwości. Następujące żądania rozszerza produktów i wybiera nazwy kategorii i nazwa produktu.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Oto treść odpowiedzi:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Aby uzyskać więcej informacji na temat opcji $select, zobacz [wybierz opcję zapytania systemu ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) w oficjalnej dokumentacji OData.

## <a name="getting-individual-properties-of-an-entity-value"></a>Pobieranie poszczególnych właściwości jednostki ($value)

Istnieją dwa sposoby dla klienta można pobrać wybranej właściwości z jednostką OData. Klienta można pobrać wartości w formacie OData lub pobrać nieprzetworzonej wartości właściwości.

Następujące żądania pobiera właściwości w formacie OData.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Oto przykład odpowiedzi w formacie JSON:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Aby uzyskać nieprzetworzonej wartości właściwości, Dołącz $value do identyfikatora URI:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Poniżej przedstawiono odpowiedzi. Zwróć uwagę, że typ zawartości "text/plain" nie jest JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Aby obsługiwać te zapytania w kontrolerze OData, Dodaj metodę o nazwie `GetProperty`, gdzie `Property` jest nazwą właściwości. Na przykład metoda get właściwości Name będą miały postać `GetName`. Metoda powinna zwrócić wartość tej właściwości:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
