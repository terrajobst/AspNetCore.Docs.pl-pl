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
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566738"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="fe4e0-102">What's New in ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="fe4e0-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="fe4e0-103">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="fe4e0-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="fe4e0-104">W tym temacie opisano, co jest nowego w programie ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="fe4e0-105">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="fe4e0-105">Download</span></span>](#download)
- [<span data-ttu-id="fe4e0-106">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="fe4e0-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="fe4e0-107">Biblioteki podstawowego OData</span><span class="sxs-lookup"><span data-stu-id="fe4e0-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="fe4e0-108">Nowe funkcje</span><span class="sxs-lookup"><span data-stu-id="fe4e0-108">New Features</span></span>](#newf)
- [<span data-ttu-id="fe4e0-109">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="fe4e0-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="fe4e0-110">Poprawki błędów</span><span class="sxs-lookup"><span data-stu-id="fe4e0-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="fe4e0-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="fe4e0-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="fe4e0-112">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="fe4e0-112">Download</span></span>

<span data-ttu-id="fe4e0-113">Funkcje środowiska uruchomieniowego są wydawane jako pakietów NuGet w galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="fe4e0-114">Można zainstalować lub zaktualizować do pakietów NuGet wydanych przy użyciu konsoli Menedżera pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="fe4e0-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="fe4e0-115">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="fe4e0-115">Documentation</span></span>

<span data-ttu-id="fe4e0-116">Można znaleźć samouczki i innych dokumentację dotyczącą ASP.NET Web API OData w [witryny sieci web ASP.NET](../odata-support-in-aspnet-web-api/index.md).</span><span class="sxs-lookup"><span data-stu-id="fe4e0-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="fe4e0-117">Biblioteki podstawowego OData</span><span class="sxs-lookup"><span data-stu-id="fe4e0-117">OData Core Libraries</span></span>

<span data-ttu-id="fe4e0-118">Dla protokołu OData v4 interfejsu API sieci Web używa teraz ODataLib wersji 6.5.0</span><span class="sxs-lookup"><span data-stu-id="fe4e0-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="fe4e0-119">Nowe funkcje w programie ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="fe4e0-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="fe4e0-120">Rozwiń węzeł Obsługa $levels w $</span><span class="sxs-lookup"><span data-stu-id="fe4e0-120">Support for $levels in $expand</span></span>

<span data-ttu-id="fe4e0-121">Można użyć $levels zapytania opcję $ Rozwiń węzeł zapytania.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="fe4e0-122">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fe4e0-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="fe4e0-123">To zapytanie jest odpowiednikiem:</span><span class="sxs-lookup"><span data-stu-id="fe4e0-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="fe4e0-124">Obsługa typów jednostek otwarte</span><span class="sxs-lookup"><span data-stu-id="fe4e0-124">Support for Open Entity Types</span></span>

<span data-ttu-id="fe4e0-125">*Otwartym typem* jest typu stuctured, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="fe4e0-126">Otwórz typy umożliwiają bardziej elastyczne modeli danych.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="fe4e0-127">Aby uzyskać więcej informacji zobacz xxxx.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="fe4e0-128">Obsługa dynamicznego kolekcji właściwości w typach Otwórz</span><span class="sxs-lookup"><span data-stu-id="fe4e0-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="fe4e0-129">Wcześniej musiały być pojedynczą wartość właściwości dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="fe4e0-130">W 5.3 właściwości dynamicznych może mieć wartości kolekcji.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="fe4e0-131">Na przykład w następujących ładunek JSON `Emails` właściwość jest właściwością dynamiczne i kolekcji typu string:</span><span class="sxs-lookup"><span data-stu-id="fe4e0-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="fe4e0-132">Obsługa dziedziczenia dla typów złożonych</span><span class="sxs-lookup"><span data-stu-id="fe4e0-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="fe4e0-133">Obecnie typy złożone może dziedziczyć z typu podstawowego.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="fe4e0-134">Na przykład usługi OData może zdefiniować następujące typy złożone:</span><span class="sxs-lookup"><span data-stu-id="fe4e0-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="fe4e0-135">Oto EDM w tym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="fe4e0-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="fe4e0-136">Aby uzyskać więcej informacji, zobacz [próbki dziedziczenia typu złożonego OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="fe4e0-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="fe4e0-137">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="fe4e0-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="fe4e0-138">W tej sekcji opisano znane problemy i fundamentalne zmiany w programie ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="fe4e0-139">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="fe4e0-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="fe4e0-140">Opcje zapytania</span><span class="sxs-lookup"><span data-stu-id="fe4e0-140">Query Options</span></span>

<span data-ttu-id="fe4e0-141">Problem: Przy użyciu zagnieżdżonych $rozwiń z $levels = powoduje maksymalną głębokość rozszerzenia niepoprawne.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="fe4e0-142">Na przykład podać następujące żądania:</span><span class="sxs-lookup"><span data-stu-id="fe4e0-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="fe4e0-143">Jeśli `MaxExpansionDepth` wynosi 5, to zapytanie spowoduje głębokość rozszerzenia 6.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="fe4e0-144">Poprawki i aktualizacje funkcji pomocniczych</span><span class="sxs-lookup"><span data-stu-id="fe4e0-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="fe4e0-145">Ta wersja zawiera również kilka poprawek usterek i funkcji pomocniczych aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="fe4e0-146">Pełną listę można znaleźć:</span><span class="sxs-lookup"><span data-stu-id="fe4e0-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="fe4e0-147">Poprawki błędów</span><span class="sxs-lookup"><span data-stu-id="fe4e0-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="fe4e0-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="fe4e0-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="fe4e0-149">W tej wersji wprowadziliśmy [Poprawka usterki](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) do niektórych typów wyliczeniowych AllowedFunctions.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="fe4e0-150">Ta wersja nie ma innych poprawki i nowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="fe4e0-150">This release doesn't have any other bug fixes or new features.</span></span>
