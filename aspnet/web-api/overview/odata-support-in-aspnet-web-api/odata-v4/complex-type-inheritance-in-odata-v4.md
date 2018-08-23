---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Dziedziczenie typu złożonego w protokole OData v4 Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: microsoft
description: Zgodnie ze specyfikacją OData v4 typ złożony może dziedziczyć z innego typu złożonego. (Typ złożony jest typem strukturalnym bez klucza). Interfejs API sieci Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 8dbf7dc4cfc70e1ea1ed6f72ffc0751a56809c09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756049"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Dziedziczenie typu złożonego w protokole OData v4 Web API platformy ASP.NET
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Zgodnie z protokołu OData v4 [specyfikacji](http://www.odata.org/documentation/odata-version-4-0/), typ złożony mogą dziedziczyć z innego typu złożonego. (A *złożonych* typ jest typem strukturalnym bez klucza.) Składnik Web API OData 5.3 obsługuje dziedziczenie typu złożonego.
> 
> W tym temacie przedstawiono sposób tworzenia modelu entity data model (EDM struktury) z typów złożonych dziedziczenia. Aby uzyskać pełnego kodu źródłowego, zobacz [przykładowe dziedziczenia typu złożonego OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Web API OData 5.3
> - Protokołu OData v4


## <a name="model-hierarchy"></a>Hierarchia modeli

Aby zilustrować dziedziczenie typu złożonego, użyjemy następujących hierarchii klas.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` jest typem abstrakcyjnym złożone. `Rectangle`, `Triangle`, i `Circle` typy złożone pochodne `Shape`, i `RoundRectangle` pochodzi od klasy `Rectangle`. `Window` jest typem jednostki i zawiera `Shape` wystąpienia.

Poniżej przedstawiono klasy CLR, które definiują tych typów.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Budowanie modelu EDM

Aby utworzyć EDM, można użyć **ODataConventionModelBuilder**, którego wnioskuje relacje dziedziczenia z typami CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Możesz również tworzyć EDM jawnie, przy użyciu **element ODataModelBuilder**. Trwa więcej kodu, ale daje większą kontrolę nad EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Dwa poniższe przykłady utworzyć ten sam schemat EDM.

## <a name="metadata-document"></a>Dokument metadanych

Oto dokument metadanych OData, przedstawiający dziedziczenie typu złożonego.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Z dokumentu metadanych możesz zobaczyć, który:

- `Shape` Typu złożonego jest abstrakcyjna.
- `Rectangle`, `Triangle`, I `Circle` typ złożony ma typ podstawowy `Shape`.
- `RoundRectangle` Typ ma podstawowy typ `Rectangle`.

## <a name="casting-complex-types"></a>Rzutowanie typów złożonych

Rzutowanie na typy złożone jest teraz obsługiwane. Na przykład poniższe zapytanie rzutowania `Shape` do `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Oto ładunek odpowiedzi:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
