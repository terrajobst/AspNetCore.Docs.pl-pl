---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: What's New in ASP.NET Web API OData 5.3 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: c185e7ef9bfe6e21601ab61c418c63c8a81b680a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827032"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>What's New in ASP.NET Web API OData 5.3
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano, co jest nowego w składniku ASP.NET Web API OData 5.3.

- [Pobieranie](#download)
- [Dokumentacja](#documentation)
- [Podstawowe OData biblioteki](#corelib)
- [Nowe funkcje](#newf)
- [Znane problemy i fundamentalne zmiany](#known-issues)
- [Poprawki błędów](#bug-fixes)
- [ASP.NET Web API OData 5.3.1 beta](#OD)

<a id="download"></a>
## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakiety NuGet w galerii NuGet. Możesz zainstalować lub zaktualizować do wydanych pakietów NuGet za pomocą konsoli Menedżera pakietów NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

Możesz znaleźć, samouczki i inne dokumenty dotyczące platformy ASP.NET Web API OData na [witryny sieci web platformy ASP.NET](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>Podstawowe OData biblioteki

Dla protokołu OData v4 interfejsu API sieci Web używa teraz ODataLib wersji 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Nowe funkcje w programie ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>Rozwiń węzeł Obsługa $levels w $

Możesz użyć $levels zapytania opcji $ Rozwiń węzeł zapytania. Na przykład:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

To zapytanie jest równoważne:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Obsługa typów jednostek Otwórz

*Otwórz typ* jest typem stuctured, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu. Typy otwarte umożliwiają bardziej elastyczne modeli danych. Aby uzyskać więcej informacji zobacz xxxx.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Obsługa właściwości kolekcji dynamicznych w typach Otwórz

Wcześniej właściwość dynamiczna musiały być pojedynczą wartość. 5.3 właściwości dynamiczne mogą mieć wartości kolekcji. Na przykład w następujących ładunek w formacie JSON `Emails` właściwość jest właściwością dynamiczne i jest kolekcji typu string:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Obsługa dziedziczenia dla typów złożonych

Teraz złożonych typów może dziedziczyć z typu podstawowego. Na przykład usługi OData można zdefiniować następujące typy złożone:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Oto EDM w tym przykładzie:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Aby uzyskać więcej informacji, zobacz [przykładowe dziedziczenia typu złożonego OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

W tej sekcji opisano znane problemy i przełomowe zmiany w programie ASP.NET Web API OData 5.3.

### <a name="odata-v4"></a>Protokołu OData v4

#### <a name="query-options"></a>Opcje zapytania

Problem: Przy użyciu ciągu $ zagnieżdżonych rozwiń z $levels = powoduje maksymalną głębokość rozszerzenia niepoprawne.

Na przykład biorąc pod uwagę następujące żądanie:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Jeśli `MaxExpansionDepth` wynosi 5, to zapytanie spowoduje głębokość rozszerzenia 6.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Poprawki i aktualizacje funkcji pomocniczych

Ta wersja zawiera także kilka poprawek usterek i funkcji drobne aktualizacje. Pełną listę można znaleźć:

- [Poprawki błędów](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1 beta

W tej wersji wprowadziliśmy [poprawki](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) do niektórych typów wyliczeniowych AllowedFunctions. Tej wersji nie zawarto żadnych poprawek ani nowych funkcji.
