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
<a name="whats-new-in-aspnet-web-api-odata-53"></a>What's New in ASP.NET Web API OData 5.3
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano, co jest nowego w programie ASP.NET Web API OData 5.3.

- [Pobieranie](#download)
- [Dokumentacja](#documentation)
- [Biblioteki podstawowego OData](#corelib)
- [Nowe funkcje](#newf)
- [Znane problemy i fundamentalne zmiany](#known-issues)
- [Poprawki błędów](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakietów NuGet w galerii NuGet. Można zainstalować lub zaktualizować do pakietów NuGet wydanych przy użyciu konsoli Menedżera pakietów NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

Można znaleźć samouczki i innych dokumentację dotyczącą ASP.NET Web API OData w [witryny sieci web ASP.NET](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>Biblioteki podstawowego OData

Dla protokołu OData v4 interfejsu API sieci Web używa teraz ODataLib wersji 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Nowe funkcje w programie ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>Rozwiń węzeł Obsługa $levels w $

Można użyć $levels zapytania opcję $ Rozwiń węzeł zapytania. Na przykład:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

To zapytanie jest odpowiednikiem:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Obsługa typów jednostek otwarte

*Otwartym typem* jest typu stuctured, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu. Otwórz typy umożliwiają bardziej elastyczne modeli danych. Aby uzyskać więcej informacji zobacz xxxx.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Obsługa dynamicznego kolekcji właściwości w typach Otwórz

Wcześniej musiały być pojedynczą wartość właściwości dynamicznych. W 5.3 właściwości dynamicznych może mieć wartości kolekcji. Na przykład w następujących ładunek JSON `Emails` właściwość jest właściwością dynamiczne i kolekcji typu string:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Obsługa dziedziczenia dla typów złożonych

Obecnie typy złożone może dziedziczyć z typu podstawowego. Na przykład usługi OData może zdefiniować następujące typy złożone:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Oto EDM w tym przykładzie:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Aby uzyskać więcej informacji, zobacz [próbki dziedziczenia typu złożonego OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

W tej sekcji opisano znane problemy i fundamentalne zmiany w programie ASP.NET Web API OData 5.3.

### <a name="odata-v4"></a>Protokołu OData v4

#### <a name="query-options"></a>Opcje zapytania

Problem: Przy użyciu zagnieżdżonych $rozwiń z $levels = powoduje maksymalną głębokość rozszerzenia niepoprawne.

Na przykład podać następujące żądania:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Jeśli `MaxExpansionDepth` wynosi 5, to zapytanie spowoduje głębokość rozszerzenia 6.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Poprawki i aktualizacje funkcji pomocniczych

Ta wersja zawiera również kilka poprawek usterek i funkcji pomocniczych aktualizacji. Pełną listę można znaleźć:

- [Poprawki błędów](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

W tej wersji wprowadziliśmy [Poprawka usterki](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) do niektórych typów wyliczeniowych AllowedFunctions. Ta wersja nie ma innych poprawki i nowe funkcje.
