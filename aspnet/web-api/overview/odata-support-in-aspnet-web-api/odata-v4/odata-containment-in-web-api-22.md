---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: "Zawierania w protokołu OData v4 używanie składnika Web API 2.2 | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Zazwyczaj z jednostką można uzyskać tylko go zostały hermetyzowany wewnątrz zestawu jednostek. Ale protokołu OData v4 zawiera dwie dodatkowe opcje Singleton i Con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Zawierania w protokołu OData v4 używanie składnika Web API 2.2
====================
przez Jinfu Tan

> Zazwyczaj z jednostką można uzyskać tylko go zostały hermetyzowany wewnątrz zestawu jednostek. Jednak protokołu OData v4 zapewnia dwie dodatkowe opcje Singleton i zawierania, które obsługuje WebAPI 2.2.


W tym temacie przedstawiono sposób definiowania relacji zawierania w punktu końcowego OData w wersji 2.2 WebApi. Aby uzyskać więcej informacji na temat zawierania, zobacz [zawierania pochodzi z protokołu OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Aby utworzyć punkt końcowy protokołu OData V4 w składniku Web API, zobacz [utworzyć OData v4 punktu końcowego Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).

Najpierw utworzymy model domeny zawierania usługi OData za pomocą tego modelu danych:

![Model danych](odata-containment-in-web-api-22/_static/image1.png)

Konto zawiera wiele PaymentInstruments (PI), ale nie zdefiniowano zestaw jednostek dla PI. Zamiast tego instrukcje przetwarzania można uzyskać tylko za pomocą konta.

Możesz pobrać rozwiązanie używane w tym temacie z [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definiowanie modelu danych

1. Definiowanie typów CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained` Atrybut jest używany w przypadku właściwości nawigacji zawierania.
2. Generowanie modelu EDM na podstawie typów CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder` Obsłuży Budowanie modelu EDM, jeśli `Contained` odpowiednią właściwość nawigacji jest dodawany atrybut. Jeśli właściwość jest typem kolekcji `GetCount(string NameContains)` funkcji zostanie również utworzona.

    Wygenerowany metadanych będzie wyglądać następująco:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget` Atrybut wskazuje, że właściwość nawigacji jest relacją zawierania.

## <a name="define-the-containing-entity-set-controller"></a>Zdefiniuj kontrolera zestawu zawierającego jednostki

Zawarte nie mają własnych kontrolera; Akcja jest zdefiniowany w kontrolerze zawierający zestaw jednostek. W tym przykładzie jest AccountsController, ale nie PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Ścieżki OData w przypadku 4 lub więcej segmentów, tylko atrybut routingu działania, takie jak `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` w kontrolerze powyżej. W przeciwnym razie wartość atrybutu i routingu z konwencjonalnej działa: na przykład `GetPayInPIs(int key)` odpowiada `GET ~/Accounts(1)/PayinPIs`.

*Dzięki użyciu Leo Hu oryginalnej zawartości w tym artykule.*
