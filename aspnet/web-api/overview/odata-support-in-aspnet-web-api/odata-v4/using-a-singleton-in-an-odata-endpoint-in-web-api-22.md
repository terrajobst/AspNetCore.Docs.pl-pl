---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Tworzenie pojedynczego wystąpienia w protokole OData v4 używanie składnika Web API 2.2 | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym temacie przedstawiono sposób definiowania wzorzec singleton w punktu końcowego OData w sieci Web API 2.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 3ae9b23fae356e387a011a190119d760dc46d022
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390930"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Tworzenie pojedynczego wystąpienia w protokole OData v4 używanie składnika Web API 2.2
====================
przez Zoe Luo

> Tradycyjnie z jednostki może oceniony jedynie go zostały hermetyzowany wewnątrz zestawu jednostek. Jednak protokół OData 4 udostępnia dwie dodatkowe opcje pojedyncze i zawierania, które obsługuje WebAPI 2.2.


W tym artykule przedstawiono sposób definiowania wzorzec singleton w punktu końcowego OData w sieci Web API 2.2. Aby uzyskać informacji o jakie pojedyncze wystąpienie znajduje i jakie korzyści z korzystania z niego, zobacz [przy użyciu pojedynczego do definiowania jednostka specjalne](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Aby utworzyć punkt końcowy protokołu OData V4 w interfejsie API sieci Web, zobacz [tworzenia protokołu OData v4 punktu końcowego Używanie wzorca ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md). 

Utworzymy wzorzec singleton w projekcie interfejsu API sieci Web przy użyciu następujących modelu danych:

![Model danych](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Wzorzec singleton o nazwie `Umbrella` będzie można zdefiniować w zależności od typu `Company`oraz zestaw nazwanych jednostek `Employees` będzie można zdefiniować w zależności od typu `Employee`.

Rozwiązanie używane w ramach tego samouczka można pobrać z [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Zdefiniowanie modelu danych

1. Definiowanie typów CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Generowanie modelu EDM na podstawie typów CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    W tym miejscu `builder.Singleton<Company>("Umbrella")` informuje konstruktora modeli do utworzenia pojedynczego o nazwie `Umbrella` w modelu EDM.

    Wygenerowany metadanych będzie wyglądać następująco:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Z metadanych widać, że właściwość nawigacji `Company` w `Employees` powiązany zestaw jednostek w wzorzec singleton `Umbrella`. Powiązanie jest wykonywane automatycznie przez `ODataConventionModelBuilder`, ponieważ tylko `Umbrella` ma `Company` typu. Jeśli istnieje niejednoznaczność modelu, można użyć `HasSingletonBinding` można jawnie powiązać właściwość nawigacji z pojedynczym; `HasSingletonBinding` ma taki sam skutek jak użycie `Singleton` atrybutu w definicji typu CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Zdefiniuj kontrolera pojedynczego wystąpienia

Takich jak kontroler obiektu EntitySet kontrolera pojedyncze dziedziczy `ODataController`, i nazwy kontrolera singleton powinna być `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Aby można było obsługiwać różne rodzaje żądań, akcje muszą być wstępnie zdefiniowane w kontrolerze. **Atrybut routingu** jest domyślnie włączone w WebApi 2.2. Na przykład, aby zdefiniować akcję w celu obsługi zapytań `Revenue` z `Company` przy użyciu atrybutu routingu, należy użyć następującego:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Jeśli nie jesteś gotowy do definiowania atrybutów dla każdej akcji, wystarczy zdefiniować swoje działania zgodnie z [konwencje routingu OData](../odata-routing-conventions.md). Ponieważ klucz nie jest wymagane do wykonywania zapytań wzorzec singleton, akcji określonych w kontrolerze pojedyncze są nieco inne niż akcji określonych w kontrolerze obiektu entityset.

Podpisy metod dla każdej definicji akcji w kontrolerze pojedyncze odwołanie, znajduje się poniżej.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Zasadniczo jest to wszystko, co należy zrobić po stronie usługi. [Przykładowy projekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) zawiera cały kod dla rozwiązania i klient OData, który pokazuje, jak użyć wzorca singleton. Klient jest wbudowana, wykonując kroki opisane w [tworzenie aplikacji klienckiej OData v4](create-an-odata-v4-client-app.md).

. 

*Dzięki rozłożeniu Leo Hu oryginalnej zawartości w tym artykule.*
