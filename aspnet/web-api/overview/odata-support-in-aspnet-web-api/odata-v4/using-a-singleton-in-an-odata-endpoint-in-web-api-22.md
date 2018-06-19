---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Utwórz wzorzec Singleton w protokołu OData v4 używanie składnika Web API 2.2 | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym temacie przedstawiono sposób definiowania pojedynczą w punktu końcowego OData w wersji 2.2 interfejsu API sieci Web.
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
ms.locfileid: "26566768"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="572b4-103">Utwórz wzorzec Singleton w protokołu OData v4 używanie składnika Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="572b4-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>
====================
<span data-ttu-id="572b4-104">przez Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="572b4-104">by Zoe Luo</span></span>

> <span data-ttu-id="572b4-105">Zazwyczaj z jednostką można uzyskać tylko go zostały hermetyzowany wewnątrz zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="572b4-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="572b4-106">Jednak protokołu OData v4 zapewnia dwie dodatkowe opcje Singleton i zawierania, które obsługuje WebAPI 2.2.</span><span class="sxs-lookup"><span data-stu-id="572b4-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>


<span data-ttu-id="572b4-107">W tym artykule przedstawiono sposób definiowania pojedynczą w punktu końcowego OData w wersji 2.2 interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="572b4-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="572b4-108">Aby uzyskać informacje na jakie singleton jest i jak mogą korzystać z nim, zobacz [przy użyciu pojedynczego, aby zdefiniować szczególne jednostki](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="572b4-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="572b4-109">Aby utworzyć punkt końcowy protokołu OData V4 w składniku Web API, zobacz [utworzyć OData v4 punktu końcowego Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="572b4-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="572b4-110">Utworzymy pojedynczą w projekcie interfejsu API sieci Web przy użyciu następującego modelu danych:</span><span class="sxs-lookup"><span data-stu-id="572b4-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Model danych](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="572b4-112">Pojedyncza o nazwie `Umbrella` będą zdefiniowane na podstawie typu `Company`i zestaw o nazwie jednostek `Employees` będzie można zdefiniować w zależności od typu `Employee`.</span><span class="sxs-lookup"><span data-stu-id="572b4-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="572b4-113">Rozwiązanie używane w tym samouczku można pobrać z [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="572b4-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="572b4-114">Zdefiniowanie modelu danych</span><span class="sxs-lookup"><span data-stu-id="572b4-114">Define the data model</span></span>

1. <span data-ttu-id="572b4-115">Definiowanie typów CLR.</span><span class="sxs-lookup"><span data-stu-id="572b4-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="572b4-116">Generowanie modelu EDM na podstawie typów CLR.</span><span class="sxs-lookup"><span data-stu-id="572b4-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="572b4-117">W tym miejscu `builder.Singleton<Company>("Umbrella")` informuje Konstruktor modelu, aby utworzyć pojedynczą o nazwie `Umbrella` w modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="572b4-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="572b4-118">Wygenerowany metadanych będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="572b4-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="572b4-119">Z metadanych możemy stwierdzić, że właściwość nawigacji `Company` w `Employees` zestawu jednostek jest powiązany z singleton `Umbrella`.</span><span class="sxs-lookup"><span data-stu-id="572b4-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="572b4-120">Powiązanie odbywa się automatycznie przez `ODataConventionModelBuilder`, ponieważ tylko `Umbrella` ma `Company` typu.</span><span class="sxs-lookup"><span data-stu-id="572b4-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="572b4-121">Jeśli istnieje niejednoznaczność modelu, możesz użyć `HasSingletonBinding` jawnie powiązać właściwość nawigacji z pojedynczym; `HasSingletonBinding` działa tak samo jak przy użyciu `Singleton` atrybutu w definicji typu CLR:</span><span class="sxs-lookup"><span data-stu-id="572b4-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="572b4-122">Zdefiniuj kontrolera pojedynczego wystąpienia</span><span class="sxs-lookup"><span data-stu-id="572b4-122">Define the singleton controller</span></span>

<span data-ttu-id="572b4-123">Jak kontroler EntitySet kontrolera singleton dziedziczy `ODataController`, oraz nazwy kontrolera singleton powinna być `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="572b4-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="572b4-124">Aby można było obsługiwać różne rodzaje żądań, akcje muszą być wstępnie zdefiniowane w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="572b4-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="572b4-125">**Atrybut routingu** jest domyślnie włączona w wersji 2.2 WebApi.</span><span class="sxs-lookup"><span data-stu-id="572b4-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="572b4-126">Na przykład, aby zdefiniować akcję do obsługi zapytań `Revenue` z `Company` przy użyciu atrybutu routingu, należy użyć następującego:</span><span class="sxs-lookup"><span data-stu-id="572b4-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="572b4-127">Jeśli nie jest gotowa do definiowania atrybutów dla każdej akcji, wystarczy zdefiniować akcji po [Konwencji routingu OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="572b4-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="572b4-128">Ponieważ klucz nie jest wymagane do wykonywania zapytań w pojedynczą, działania zdefiniowane w kontrolerze pojedynczego wystąpienia są nieco inne niż akcji określonych w kontrolerze obiektu entityset.</span><span class="sxs-lookup"><span data-stu-id="572b4-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="572b4-129">Odwołania podpisy metod dla każdej definicji akcji w kontrolerze pojedynczego wystąpienia są wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="572b4-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="572b4-130">Zasadniczo to wszystko, co należy zrobić po stronie usługi.</span><span class="sxs-lookup"><span data-stu-id="572b4-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="572b4-131">[Przykładowy projekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) zawiera wszystkie kodu dla rozwiązania i klienta OData, który przedstawia sposób użycia wzorca singleton.</span><span class="sxs-lookup"><span data-stu-id="572b4-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="572b4-132">Klient jest konstruowany przez zgodnie z krokami w [tworzenie aplikacji klienckich OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="572b4-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="572b4-133">.</span><span class="sxs-lookup"><span data-stu-id="572b4-133">.</span></span> 

<span data-ttu-id="572b4-134">*Dzięki użyciu Leo Hu oryginalnej zawartości w tym artykule.*</span><span class="sxs-lookup"><span data-stu-id="572b4-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
