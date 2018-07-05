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
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="c1100-103">Tworzenie pojedynczego wystąpienia w protokole OData v4 używanie składnika Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="c1100-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="c1100-104">przez Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="c1100-104">by Zoe Luo</span></span>

> <span data-ttu-id="c1100-105">Tradycyjnie z jednostki może oceniony jedynie go zostały hermetyzowany wewnątrz zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="c1100-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="c1100-106">Jednak protokół OData 4 udostępnia dwie dodatkowe opcje pojedyncze i zawierania, które obsługuje WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="c1100-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="c1100-107">W tym artykule przedstawiono sposób definiowania wzorzec singleton w punktu końcowego OData w sieci Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="c1100-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="c1100-108">Aby uzyskać informacji o jakie pojedyncze wystąpienie znajduje i jakie korzyści z korzystania z niego, zobacz [przy użyciu pojedynczego do definiowania jednostka specjalne](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1100-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="c1100-109">Aby utworzyć punkt końcowy protokołu OData V4 w interfejsie API sieci Web, zobacz [tworzenia protokołu OData v4 punktu końcowego Używanie wzorca ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="c1100-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="c1100-110">Utworzymy wzorzec singleton w projekcie interfejsu API sieci Web przy użyciu następujących modelu danych:</span><span class="sxs-lookup"><span data-stu-id="c1100-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Model danych](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="c1100-112">Wzorzec singleton o nazwie `Umbrella` będzie można zdefiniować w zależności od typu `Company`oraz zestaw nazwanych jednostek `Employees` będzie można zdefiniować w zależności od typu `Employee`.</span><span class="sxs-lookup"><span data-stu-id="c1100-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="c1100-113">Rozwiązanie używane w ramach tego samouczka można pobrać z [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="c1100-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="c1100-114">Zdefiniowanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="c1100-114">Define the data model</span></span>

1. <span data-ttu-id="c1100-115">Definiowanie typów CLR.</span><span class="sxs-lookup"><span data-stu-id="c1100-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="c1100-116">Generowanie modelu EDM na podstawie typów CLR.</span><span class="sxs-lookup"><span data-stu-id="c1100-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="c1100-117">W tym miejscu `builder.Singleton<Company>("Umbrella")` informuje konstruktora modeli do utworzenia pojedynczego o nazwie `Umbrella` w modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="c1100-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="c1100-118">Wygenerowany metadanych będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="c1100-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="c1100-119">Z metadanych widać, że właściwość nawigacji `Company` w `Employees` powiązany zestaw jednostek w wzorzec singleton `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="c1100-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="c1100-120">Powiązanie jest wykonywane automatycznie przez `ODataConventionModelBuilder`, ponieważ tylko `Umbrella` ma `Company` typu.</span><span class="sxs-lookup"><span data-stu-id="c1100-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="c1100-121">Jeśli istnieje niejednoznaczność modelu, można użyć `HasSingletonBinding` można jawnie powiązać właściwość nawigacji z pojedynczym; `HasSingletonBinding` ma taki sam skutek jak użycie `Singleton` atrybutu w definicji typu CLR:</span><span class="sxs-lookup"><span data-stu-id="c1100-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="c1100-122">Zdefiniuj kontrolera pojedynczego wystąpienia</span><span class="sxs-lookup"><span data-stu-id="c1100-122">Define the singleton controller</span></span>

<span data-ttu-id="c1100-123">Takich jak kontroler obiektu EntitySet kontrolera pojedyncze dziedziczy `ODataController`, i nazwy kontrolera singleton powinna być `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="c1100-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="c1100-124">Aby można było obsługiwać różne rodzaje żądań, akcje muszą być wstępnie zdefiniowane w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="c1100-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="c1100-125">**Atrybut routingu** jest domyślnie włączone w WebApi 2.2.</span><span class="sxs-lookup"><span data-stu-id="c1100-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="c1100-126">Na przykład, aby zdefiniować akcję w celu obsługi zapytań `Revenue` z `Company` przy użyciu atrybutu routingu, należy użyć następującego:</span><span class="sxs-lookup"><span data-stu-id="c1100-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="c1100-127">Jeśli nie jesteś gotowy do definiowania atrybutów dla każdej akcji, wystarczy zdefiniować swoje działania zgodnie z [konwencje routingu OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="c1100-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="c1100-128">Ponieważ klucz nie jest wymagane do wykonywania zapytań wzorzec singleton, akcji określonych w kontrolerze pojedyncze są nieco inne niż akcji określonych w kontrolerze obiektu entityset.</span><span class="sxs-lookup"><span data-stu-id="c1100-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="c1100-129">Podpisy metod dla każdej definicji akcji w kontrolerze pojedyncze odwołanie, znajduje się poniżej.</span><span class="sxs-lookup"><span data-stu-id="c1100-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="c1100-130">Zasadniczo jest to wszystko, co należy zrobić po stronie usługi.</span><span class="sxs-lookup"><span data-stu-id="c1100-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="c1100-131">[Przykładowy projekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) zawiera cały kod dla rozwiązania i klient OData, który pokazuje, jak użyć wzorca singleton.</span><span class="sxs-lookup"><span data-stu-id="c1100-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="c1100-132">Klient jest wbudowana, wykonując kroki opisane w [tworzenie aplikacji klienckiej OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="c1100-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="c1100-133">.</span><span class="sxs-lookup"><span data-stu-id="c1100-133">.</span></span> 

<span data-ttu-id="c1100-134">*Dzięki rozłożeniu Leo Hu oryginalnej zawartości w tym artykule.*</span><span class="sxs-lookup"><span data-stu-id="c1100-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
