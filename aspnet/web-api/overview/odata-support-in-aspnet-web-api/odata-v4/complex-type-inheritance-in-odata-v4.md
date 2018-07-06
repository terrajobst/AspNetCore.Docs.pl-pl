---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Dziedziczenie typu złożonego w protokole OData v4 Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: microsoft
description: Zgodnie ze specyfikacją OData v4 typ złożony może dziedziczyć z innego typu złożonego. (Typ złożony jest typem strukturalnym bez klucza). Interfejs API sieci Web...
ms.author: aspnetcontent
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d295a6ae20f5771ae1f4f28166f7e651b6ec5c58
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824033"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="9e33a-104">Dziedziczenie typu złożonego w protokole OData v4 Web API platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9e33a-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="9e33a-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9e33a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="9e33a-106">Zgodnie z protokołu OData v4 [specyfikacji](http://www.odata.org/documentation/odata-version-4-0/), typ złożony mogą dziedziczyć z innego typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="9e33a-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="9e33a-107">(A *złożonych* typ jest typem strukturalnym bez klucza.) Składnik Web API OData 5.3 obsługuje dziedziczenie typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="9e33a-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="9e33a-108">W tym temacie przedstawiono sposób tworzenia modelu entity data model (EDM struktury) z typów złożonych dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="9e33a-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="9e33a-109">Aby uzyskać pełnego kodu źródłowego, zobacz [przykładowe dziedziczenia typu złożonego OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="9e33a-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9e33a-110">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="9e33a-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9e33a-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="9e33a-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="9e33a-112">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="9e33a-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="9e33a-113">Hierarchia modeli</span><span class="sxs-lookup"><span data-stu-id="9e33a-113">Model Hierarchy</span></span>

<span data-ttu-id="9e33a-114">Aby zilustrować dziedziczenie typu złożonego, użyjemy następujących hierarchii klas.</span><span class="sxs-lookup"><span data-stu-id="9e33a-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="9e33a-115">`Shape` jest typem abstrakcyjnym złożone.</span><span class="sxs-lookup"><span data-stu-id="9e33a-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="9e33a-116">`Rectangle`, `Triangle`, i `Circle` typy złożone pochodne `Shape`, i `RoundRectangle` pochodzi od klasy `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="9e33a-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="9e33a-117">`Window` jest typem jednostki i zawiera `Shape` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="9e33a-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="9e33a-118">Poniżej przedstawiono klasy CLR, które definiują tych typów.</span><span class="sxs-lookup"><span data-stu-id="9e33a-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="9e33a-119">Budowanie modelu EDM</span><span class="sxs-lookup"><span data-stu-id="9e33a-119">Build the EDM Model</span></span>

<span data-ttu-id="9e33a-120">Aby utworzyć EDM, można użyć **ODataConventionModelBuilder**, którego wnioskuje relacje dziedziczenia z typami CLR.</span><span class="sxs-lookup"><span data-stu-id="9e33a-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="9e33a-121">Możesz również tworzyć EDM jawnie, przy użyciu **element ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="9e33a-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="9e33a-122">Trwa więcej kodu, ale daje większą kontrolę nad EDM.</span><span class="sxs-lookup"><span data-stu-id="9e33a-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="9e33a-123">Dwa poniższe przykłady utworzyć ten sam schemat EDM.</span><span class="sxs-lookup"><span data-stu-id="9e33a-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="9e33a-124">Dokument metadanych</span><span class="sxs-lookup"><span data-stu-id="9e33a-124">Metadata Document</span></span>

<span data-ttu-id="9e33a-125">Oto dokument metadanych OData, przedstawiający dziedziczenie typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="9e33a-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="9e33a-126">Z dokumentu metadanych możesz zobaczyć, który:</span><span class="sxs-lookup"><span data-stu-id="9e33a-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="9e33a-127">`Shape` Typu złożonego jest abstrakcyjna.</span><span class="sxs-lookup"><span data-stu-id="9e33a-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="9e33a-128">`Rectangle`, `Triangle`, I `Circle` typ złożony ma typ podstawowy `Shape`.</span><span class="sxs-lookup"><span data-stu-id="9e33a-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="9e33a-129">`RoundRectangle` Typ ma podstawowy typ `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="9e33a-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="9e33a-130">Rzutowanie typów złożonych</span><span class="sxs-lookup"><span data-stu-id="9e33a-130">Casting Complex Types</span></span>

<span data-ttu-id="9e33a-131">Rzutowanie na typy złożone jest teraz obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="9e33a-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="9e33a-132">Na przykład poniższe zapytanie rzutowania `Shape` do `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="9e33a-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="9e33a-133">Oto ładunek odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="9e33a-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
