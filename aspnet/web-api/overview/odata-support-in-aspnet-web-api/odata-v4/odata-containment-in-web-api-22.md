---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Zawieranie w protokole OData v4 używanie składnika Web API 2.2 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Tradycyjnie z jednostki może oceniony jedynie go zostały hermetyzowany wewnątrz zestawu jednostek. Jednak protokół OData 4 udostępnia dwie dodatkowe opcje pojedyncze i Con...
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 56e550b56e9ad237dbf4fab04f2bd545164ee90a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824915"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Zawieranie w protokole OData v4 używanie składnika Web API 2.2
====================
przez Jinfu Tan

> Tradycyjnie z jednostki może oceniony jedynie go zostały hermetyzowany wewnątrz zestawu jednostek. Jednak protokół OData 4 udostępnia dwie dodatkowe opcje pojedyncze i zawierania, które obsługuje WebAPI 2.2.


W tym temacie pokazano, jak zdefiniować zawierania w punktu końcowego OData w WebApi 2.2. Aby uzyskać więcej informacji na temat relacji zawierania zobacz [zawierania pochodzi z protokołu OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Aby utworzyć punkt końcowy protokołu OData V4 w interfejsie API sieci Web, zobacz [tworzenia protokołu OData v4 punktu końcowego Używanie wzorca ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).

Najpierw utworzymy model domeny zawierania usługi OData przy użyciu tego modelu danych:

![Model danych](odata-containment-in-web-api-22/_static/image1.png)

Konto zawiera wiele PaymentInstruments (PI), ale nie zdefiniowano zestaw jednostek dla PI. Zamiast tego instrukcje przetwarzania może zostać oceniony jedynie za pośrednictwem konta.

Możesz pobrać rozwiązanie używane w tym temacie z [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Zdefiniowanie modelu danych

1. Definiowanie typów CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained` Atrybut jest używany dla właściwości nawigacji zawierania.
2. Generowanie modelu EDM na podstawie typów CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder` Będzie obsługiwać Budowanie modelu EDM, jeśli `Contained` odpowiednią właściwość nawigacji jest dodawany atrybut. Jeśli właściwość jest typem kolekcji `GetCount(string NameContains)` funkcji zostanie również utworzony.

    Wygenerowany metadanych będzie wyglądać następująco:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget` Atrybut wskazuje, że właściwość nawigacji jest relacją zawierania.

## <a name="define-the-containing-entity-set-controller"></a>Zdefiniuj zawierający kontroler zestaw jednostek

Jednostki zawarte nie mają własne kontroler; Akcja jest zdefiniowany w kontrolerze zawierający zestaw jednostek. W tym przykładzie ma AccountsController, ale nie PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Ścieżki OData w przypadku 4 lub więcej segmentów, tylko atrybut routingu dzieła, takie jak `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` w kontrolerze powyżej. W przeciwnym razie działa atrybut i routing konwencjonalne: na przykład `GetPayInPIs(int key)` odpowiada `GET ~/Accounts(1)/PayinPIs`.

*Dzięki rozłożeniu Leo Hu oryginalnej zawartości w tym artykule.*
