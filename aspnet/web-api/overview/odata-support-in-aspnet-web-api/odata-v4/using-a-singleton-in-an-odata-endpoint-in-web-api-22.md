---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: "Utwórz wzorzec Singleton w protokołu OData v4 używanie składnika Web API 2.2 | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym temacie przedstawiono sposób definiowania pojedynczą w punktu końcowego OData w wersji 2.2 interfejsu API sieci Web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Utwórz wzorzec Singleton w protokołu OData v4 używanie składnika Web API 2.2
====================
przez Zoe Luo

> Zazwyczaj z jednostką można uzyskać tylko go zostały hermetyzowany wewnątrz zestawu jednostek. Jednak protokołu OData v4 zapewnia dwie dodatkowe opcje Singleton i zawierania, które obsługuje WebAPI 2.2.


W tym artykule przedstawiono sposób definiowania pojedynczą w punktu końcowego OData w wersji 2.2 interfejsu API sieci Web. Aby uzyskać informacje na jakie singleton jest i jak mogą korzystać z nim, zobacz [przy użyciu pojedynczego, aby zdefiniować szczególne jednostki](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Aby utworzyć punkt końcowy protokołu OData V4 w składniku Web API, zobacz [utworzyć OData v4 punktu końcowego Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md). 

Utworzymy pojedynczą w projekcie interfejsu API sieci Web przy użyciu następującego modelu danych:

![Model danych](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Pojedyncza o nazwie `Umbrella` będą zdefiniowane na podstawie typu `Company`i zestaw o nazwie jednostek `Employees` będzie można zdefiniować w zależności od typu `Employee`.

Rozwiązanie używane w tym samouczku można pobrać z [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Zdefiniowanie modelu danych

1. Definiowanie typów CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Generowanie modelu EDM na podstawie typów CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    W tym miejscu `builder.Singleton<Company>("Umbrella")` informuje Konstruktor modelu, aby utworzyć pojedynczą o nazwie `Umbrella` w modelu EDM.

    Wygenerowany metadanych będzie wyglądać następująco:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Z metadanych możemy stwierdzić, że właściwość nawigacji `Company` w `Employees` zestawu jednostek jest powiązany z singleton `Umbrella`. Powiązanie odbywa się automatycznie przez `ODataConventionModelBuilder`, ponieważ tylko `Umbrella` ma `Company` typu. Jeśli istnieje niejednoznaczność modelu, możesz użyć `HasSingletonBinding` jawnie powiązać właściwość nawigacji z pojedynczym; `HasSingletonBinding` działa tak samo jak przy użyciu `Singleton` atrybutu w definicji typu CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Zdefiniuj kontrolera pojedynczego wystąpienia

Jak kontroler EntitySet kontrolera singleton dziedziczy `ODataController`, oraz nazwy kontrolera singleton powinna być `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Aby można było obsługiwać różne rodzaje żądań, akcje muszą być wstępnie zdefiniowane w kontrolerze. **Atrybut routingu** jest domyślnie włączona w wersji 2.2 WebApi. Na przykład, aby zdefiniować akcję do obsługi zapytań `Revenue` z `Company` przy użyciu atrybutu routingu, należy użyć następującego:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Jeśli nie jest gotowa do definiowania atrybutów dla każdej akcji, wystarczy zdefiniować akcji po [Konwencji routingu OData](../odata-routing-conventions.md). Ponieważ klucz nie jest wymagane do wykonywania zapytań w pojedynczą, działania zdefiniowane w kontrolerze pojedynczego wystąpienia są nieco inne niż akcji określonych w kontrolerze obiektu entityset.

Odwołania podpisy metod dla każdej definicji akcji w kontrolerze pojedynczego wystąpienia są wymienione poniżej.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Zasadniczo to wszystko, co należy zrobić po stronie usługi. [Przykładowy projekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) zawiera wszystkie kodu dla rozwiązania i klienta OData, który przedstawia sposób użycia wzorca singleton. Klient jest konstruowany przez zgodnie z krokami w [tworzenie aplikacji klienckich OData v4](create-an-odata-v4-client-app.md).

. 

*Dzięki użyciu Leo Hu oryginalnej zawartości w tym artykule.*
