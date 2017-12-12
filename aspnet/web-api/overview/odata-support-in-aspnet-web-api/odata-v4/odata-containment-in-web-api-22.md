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
<a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="55ec8-104">Zawierania w protokołu OData v4 używanie składnika Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="55ec8-104">Containment in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="55ec8-105">przez Jinfu Tan</span><span class="sxs-lookup"><span data-stu-id="55ec8-105">by Jinfu Tan</span></span>

> <span data-ttu-id="55ec8-106">Zazwyczaj z jednostką można uzyskać tylko go zostały hermetyzowany wewnątrz zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="55ec8-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="55ec8-107">Jednak protokołu OData v4 zapewnia dwie dodatkowe opcje Singleton i zawierania, które obsługuje WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="55ec8-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="55ec8-108">W tym temacie przedstawiono sposób definiowania relacji zawierania w punktu końcowego OData w wersji 2.2 WebApi.</span><span class="sxs-lookup"><span data-stu-id="55ec8-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="55ec8-109">Aby uzyskać więcej informacji na temat zawierania, zobacz [zawierania pochodzi z protokołu OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="55ec8-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="55ec8-110">Aby utworzyć punkt końcowy protokołu OData V4 w składniku Web API, zobacz [utworzyć OData v4 punktu końcowego Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="55ec8-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="55ec8-111">Najpierw utworzymy model domeny zawierania usługi OData za pomocą tego modelu danych:</span><span class="sxs-lookup"><span data-stu-id="55ec8-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Model danych](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="55ec8-113">Konto zawiera wiele PaymentInstruments (PI), ale nie zdefiniowano zestaw jednostek dla PI.</span><span class="sxs-lookup"><span data-stu-id="55ec8-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="55ec8-114">Zamiast tego instrukcje przetwarzania można uzyskać tylko za pomocą konta.</span><span class="sxs-lookup"><span data-stu-id="55ec8-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="55ec8-115">Możesz pobrać rozwiązanie używane w tym temacie z [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="55ec8-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="55ec8-116">Definiowanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="55ec8-116">Defining the data model</span></span>

1. <span data-ttu-id="55ec8-117">Definiowanie typów CLR.</span><span class="sxs-lookup"><span data-stu-id="55ec8-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="55ec8-118">`Contained` Atrybut jest używany w przypadku właściwości nawigacji zawierania.</span><span class="sxs-lookup"><span data-stu-id="55ec8-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="55ec8-119">Generowanie modelu EDM na podstawie typów CLR.</span><span class="sxs-lookup"><span data-stu-id="55ec8-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="55ec8-120">`ODataConventionModelBuilder` Obsłuży Budowanie modelu EDM, jeśli `Contained` odpowiednią właściwość nawigacji jest dodawany atrybut.</span><span class="sxs-lookup"><span data-stu-id="55ec8-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="55ec8-121">Jeśli właściwość jest typem kolekcji `GetCount(string NameContains)` funkcji zostanie również utworzona.</span><span class="sxs-lookup"><span data-stu-id="55ec8-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="55ec8-122">Wygenerowany metadanych będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="55ec8-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="55ec8-123">`ContainsTarget` Atrybut wskazuje, że właściwość nawigacji jest relacją zawierania.</span><span class="sxs-lookup"><span data-stu-id="55ec8-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="55ec8-124">Zdefiniuj kontrolera zestawu zawierającego jednostki</span><span class="sxs-lookup"><span data-stu-id="55ec8-124">Define the containing entity set controller</span></span>

<span data-ttu-id="55ec8-125">Zawarte nie mają własnych kontrolera; Akcja jest zdefiniowany w kontrolerze zawierający zestaw jednostek.</span><span class="sxs-lookup"><span data-stu-id="55ec8-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="55ec8-126">W tym przykładzie jest AccountsController, ale nie PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="55ec8-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="55ec8-127">Ścieżki OData w przypadku 4 lub więcej segmentów, tylko atrybut routingu działania, takie jak `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` w kontrolerze powyżej.</span><span class="sxs-lookup"><span data-stu-id="55ec8-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="55ec8-128">W przeciwnym razie wartość atrybutu i routingu z konwencjonalnej działa: na przykład `GetPayInPIs(int key)` odpowiada `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="55ec8-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="55ec8-129">*Dzięki użyciu Leo Hu oryginalnej zawartości w tym artykule.*</span><span class="sxs-lookup"><span data-stu-id="55ec8-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
