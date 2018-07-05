---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: What's New in ASP.NET Web API OData 5.3 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: aedc4e0dc1bf144239f6921a51eaef459a0907a6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386685"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="0c710-102">What's New in ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="0c710-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="0c710-103">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0c710-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="0c710-104">W tym temacie opisano, co jest nowego w składniku ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="0c710-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="0c710-105">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="0c710-105">Download</span></span>](#download)
- [<span data-ttu-id="0c710-106">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="0c710-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="0c710-107">Podstawowe OData biblioteki</span><span class="sxs-lookup"><span data-stu-id="0c710-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="0c710-108">Nowe funkcje</span><span class="sxs-lookup"><span data-stu-id="0c710-108">New Features</span></span>](#newf)
- [<span data-ttu-id="0c710-109">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="0c710-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="0c710-110">Poprawki błędów</span><span class="sxs-lookup"><span data-stu-id="0c710-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="0c710-111">ASP.NET Web API OData 5.3.1 beta</span><span class="sxs-lookup"><span data-stu-id="0c710-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="0c710-112">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="0c710-112">Download</span></span>

<span data-ttu-id="0c710-113">Funkcje środowiska uruchomieniowego są wydawane jako pakiety NuGet w galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="0c710-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="0c710-114">Możesz zainstalować lub zaktualizować do wydanych pakietów NuGet za pomocą konsoli Menedżera pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="0c710-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="0c710-115">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="0c710-115">Documentation</span></span>

<span data-ttu-id="0c710-116">Możesz znaleźć, samouczki i inne dokumenty dotyczące platformy ASP.NET Web API OData na [witryny sieci web platformy ASP.NET](../odata-support-in-aspnet-web-api/index.md).</span><span class="sxs-lookup"><span data-stu-id="0c710-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="0c710-117">Podstawowe OData biblioteki</span><span class="sxs-lookup"><span data-stu-id="0c710-117">OData Core Libraries</span></span>

<span data-ttu-id="0c710-118">Dla protokołu OData v4 interfejsu API sieci Web używa teraz ODataLib wersji 6.5.0</span><span class="sxs-lookup"><span data-stu-id="0c710-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="0c710-119">Nowe funkcje w programie ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="0c710-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="0c710-120">Rozwiń węzeł Obsługa $levels w $</span><span class="sxs-lookup"><span data-stu-id="0c710-120">Support for $levels in $expand</span></span>

<span data-ttu-id="0c710-121">Możesz użyć $levels zapytania opcji $ Rozwiń węzeł zapytania.</span><span class="sxs-lookup"><span data-stu-id="0c710-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="0c710-122">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="0c710-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="0c710-123">To zapytanie jest równoważne:</span><span class="sxs-lookup"><span data-stu-id="0c710-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="0c710-124">Obsługa typów jednostek Otwórz</span><span class="sxs-lookup"><span data-stu-id="0c710-124">Support for Open Entity Types</span></span>

<span data-ttu-id="0c710-125">*Otwórz typ* jest typem stuctured, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu.</span><span class="sxs-lookup"><span data-stu-id="0c710-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="0c710-126">Typy otwarte umożliwiają bardziej elastyczne modeli danych.</span><span class="sxs-lookup"><span data-stu-id="0c710-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="0c710-127">Aby uzyskać więcej informacji zobacz xxxx.</span><span class="sxs-lookup"><span data-stu-id="0c710-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="0c710-128">Obsługa właściwości kolekcji dynamicznych w typach Otwórz</span><span class="sxs-lookup"><span data-stu-id="0c710-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="0c710-129">Wcześniej właściwość dynamiczna musiały być pojedynczą wartość.</span><span class="sxs-lookup"><span data-stu-id="0c710-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="0c710-130">5.3 właściwości dynamiczne mogą mieć wartości kolekcji.</span><span class="sxs-lookup"><span data-stu-id="0c710-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="0c710-131">Na przykład w następujących ładunek w formacie JSON `Emails` właściwość jest właściwością dynamiczne i jest kolekcji typu string:</span><span class="sxs-lookup"><span data-stu-id="0c710-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="0c710-132">Obsługa dziedziczenia dla typów złożonych</span><span class="sxs-lookup"><span data-stu-id="0c710-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="0c710-133">Teraz złożonych typów może dziedziczyć z typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="0c710-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="0c710-134">Na przykład usługi OData można zdefiniować następujące typy złożone:</span><span class="sxs-lookup"><span data-stu-id="0c710-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="0c710-135">Oto EDM w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="0c710-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="0c710-136">Aby uzyskać więcej informacji, zobacz [przykładowe dziedziczenia typu złożonego OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="0c710-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="0c710-137">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="0c710-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="0c710-138">W tej sekcji opisano znane problemy i przełomowe zmiany w programie ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="0c710-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="0c710-139">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="0c710-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="0c710-140">Opcje zapytania</span><span class="sxs-lookup"><span data-stu-id="0c710-140">Query Options</span></span>

<span data-ttu-id="0c710-141">Problem: Przy użyciu ciągu $ zagnieżdżonych rozwiń z $levels = powoduje maksymalną głębokość rozszerzenia niepoprawne.</span><span class="sxs-lookup"><span data-stu-id="0c710-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="0c710-142">Na przykład biorąc pod uwagę następujące żądanie:</span><span class="sxs-lookup"><span data-stu-id="0c710-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="0c710-143">Jeśli `MaxExpansionDepth` wynosi 5, to zapytanie spowoduje głębokość rozszerzenia 6.</span><span class="sxs-lookup"><span data-stu-id="0c710-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="0c710-144">Poprawki i aktualizacje funkcji pomocniczych</span><span class="sxs-lookup"><span data-stu-id="0c710-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="0c710-145">Ta wersja zawiera także kilka poprawek usterek i funkcji drobne aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="0c710-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="0c710-146">Pełną listę można znaleźć:</span><span class="sxs-lookup"><span data-stu-id="0c710-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="0c710-147">Poprawki błędów</span><span class="sxs-lookup"><span data-stu-id="0c710-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="0c710-148">ASP.NET Web API OData 5.3.1 beta</span><span class="sxs-lookup"><span data-stu-id="0c710-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="0c710-149">W tej wersji wprowadziliśmy [poprawki](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) do niektórych typów wyliczeniowych AllowedFunctions.</span><span class="sxs-lookup"><span data-stu-id="0c710-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="0c710-150">Tej wersji nie zawarto żadnych poprawek ani nowych funkcji.</span><span class="sxs-lookup"><span data-stu-id="0c710-150">This release doesn't have any other bug fixes or new features.</span></span>
