---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: "Dziedziczenie typu złożonego w protokołu OData v4 z interfejsu API sieci Web programu ASP.NET | Dokumentacja firmy Microsoft"
author: microsoft
description: "Zgodnie ze specyfikacją OData v4 typu złożonego może dziedziczyć z innego typu złożonego. (Typ złożony jest typem strukturalnym bez klucza). Interfejs API sieci Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Dziedziczenie typu złożonego w protokołu OData v4 z interfejsu API sieci Web ASP.NET
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Zgodnie z protokołu OData v4 [specyfikacji](http://www.odata.org/documentation/odata-version-4-0/), typu złożonego może dziedziczyć z innego typu złożonego. (A *złożonych* typ jest typem strukturalnym bez klucza.) Interfejs API sieci Web OData 5.3 obsługuje dziedziczenia typu złożonego.
> 
> W tym temacie przedstawiono sposób tworzenia modelu entity data model (EDM) z typów złożonych dziedziczenia. Dla kodu źródłowego pełną, zobacz [próbki dziedziczenia typu złożonego OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Web API OData 5.3
> - Protokołu OData v4


## <a name="model-hierarchy"></a>Model hierarchii

Aby zilustrować dziedziczenia typu złożonego, użyjemy poniższych hierarchii klas.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`jest typem abstrakcyjnym złożonych. `Rectangle`, `Triangle`, i `Circle` typy złożone pochodne `Shape`, i `RoundRectangle` pochodną `Rectangle`. `Window`jest typem jednostki i zawiera `Shape` wystąpienia.

Poniżej przedstawiono klasy CLR, które definiują tych typów.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Tworzenie modelu EDM

Aby utworzyć EDM, można użyć **ODataConventionModelBuilder**, którego wnioskuje relacji dziedziczenia z typów CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Można również tworzyć EDM jawnie, przy użyciu **element ODataModelBuilder**. Więcej kodu, ale zapewnia większą kontrolę nad EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Te dwa przykłady tworzenia tego samego schematu modelu EDM.

## <a name="metadata-document"></a>Dokument metadanych

Oto dokument metadanych OData, przedstawiający dziedziczenia typu złożonego.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Z dokumentu metadanych można stwierdzić, że:

- `Shape` Typu złożonego jest abstrakcyjny.
- `Rectangle`, `Triangle`, I `Circle` typ podstawowy ma typ złożony `Shape`.
- `RoundRectangle` Typ ma typ podstawowy `Rectangle`.

## <a name="casting-complex-types"></a>Rzutowanie typów złożonych

Rzutowanie na typy złożone obecnie jest obsługiwany. Na przykład następujące zapytanie rzutowania `Shape` do `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Oto ładunku odpowiedzi:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
