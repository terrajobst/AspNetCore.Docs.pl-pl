---
uid: mvc/overview/releases/mvc51-release-notes
title: Co to jest nowe w programie ASP.NET MVC 5.1 | Dokumentacja firmy Microsoft
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-51"></a>Co to jest nowe w programie ASP.NET MVC 5.1
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano nowe dla 5.1 MVC sieci Web ASP.NET.

- [Wymagania dotyczące oprogramowania](#SoftwareRequirements)
- [Pobieranie](#download)
- [Dokumentacja](#documentation)
- [Nowe funkcje w programie ASP.NET MVC 5.1](#new-features)

    - [Atrybut ulepszenia routingu](#AttributeRouting)
    - [Obsługa inicjowania szablony Edytora](#Bootstrap)
    - [Obsługa wyliczenia w widokach](#Enum)
    - [Dyskretny kod weryfikacji dla atrybutów MinLength/element MaxLength](#Unobtrusive)
    - [Obsługa kontekstu "this" w dyskretnego kodu Ajax](#thisContext)
- [Znane problemy i fundamentalne zmiany](#KnownBreakingChanges)- [poprawki błędów](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Wymagania programowe

- Program Visual Studio 2012: Pobierz [ASP.NET i sieć Web narzędzi 2013.1 dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Programu Visual Studio 2013: Pobieranie [programu Visual Studio 2013, aktualizacja 1](https://go.microsoft.com/fwlink/?LinkId=390064). Ta aktualizacja jest potrzebne do edycji widoki ASP.NET MVC 5.1 Razor.

<a id="download"></a>
## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakietów NuGet w galerii NuGet. Wykonaj wszystkie pakiety środowiska uruchomieniowego [Wersjonowania semantycznego](http://semver.org/) specyfikacji. Najnowszy pakiet RTM programu ASP.NET MVC 5.1 ma następującą wersję: "5.1.2". Można zainstalować lub zaktualizować te pakiety za pośrednictwem [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Wydanie obejmuje również odpowiedniego zlokalizowane pakiety na NuGet.

Można zainstalować lub zaktualizować do pakietów NuGet wydanych przy użyciu konsoli Menedżera pakietów NuGet:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

Samouczki i inne informacje o RTM programu ASP.NET MVC 5.1 są dostępne w witrynie sieci web platformy ASP.NET (https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Nowe funkcje w programie ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Atrybut ulepszenia routingu

 Atrybut routingu teraz obsługuje ograniczenia, włączanie wersji i nagłówek na podstawie wyboru trasy. Wiele aspektów tras atrybutów są teraz można dostosować za pomocą `IDirectRouteFactory` interfejsu i `RouteFactoryAttribute` klasy. Prefiks trasy jest teraz rozszerzalny za pośrednictwem `IRoutePrefix` interfejsu i `RoutePrefixAttribute` klasy. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Obsługa wyliczenia w widokach

1. Nowe `@Html.EnumDropDownListFor()` metody pomocnicze. Powinny być one używane jak najbardziej pomocników HTML, ale należy pamiętać, że wyrażenia musi być [wyliczenia](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu lub [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) gdzie `T` jest [wyliczenia](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu. Użyj `EnumHelper.IsValidForEnumHelper()` Aby sprawdzić te wymagania.
2. Nowy `EnumHelper.GetSelectList()` metody zwracające `IList<SelectListItem>`. Jest to przydatne, gdy potrzebne do modyfikowania listy wyboru przed wywołaniem, na przykład `@Html.DropDownListFor()`, lub gdy chcesz wyświetlić nazwy którego `@Html.EnumDropDownListFor()` zawiera.

Poniższy kod przedstawia te interfejsy API.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Widać pełny przykład [tutaj](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Obsługa inicjowania szablony Edytora

Mamy teraz możliwe przekazywanie w atrybutach HTML w [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) jako [obiekt anonimowy](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Na przykład:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Sprawdzanie poprawności dyskretnego kodu MinLengthAttribute i MaxLengthAttribute

Dla właściwości ozdobione będą teraz obsługiwane weryfikacji po stronie klienta dla typów ciąg i tablica [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) i [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atrybutów.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Obsługa kontekstu "this" w dyskretnego kodu Ajax

Funkcje wywołania zwrotnego (`OnBegin, OnComplete, OnFailure, OnSuccess`) teraz będzie mógł zlokalizować wywoływanie elementu za pomocą `this` kontekstu. Na przykład:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

### <a name="attribute-routing"></a>Atrybut routingu

Niejednoznaczności w dopasowań routingu atrybutu teraz będzie zgłaszać błąd, a nie wybranie pierwszego dopasowania.

Tras atrybutów mogą używać tychże `{controller}` parametru i korzystania z `{action}` parametru na trasach dotyczącymi akcji. Korzysta z tych parametrów najprawdopodobniej doprowadzi do niejednoznaczności. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Funkcja szkieletów MVC/interfejs API sieci Web do projektu z 5.1 wynikiem pakiety w wersji 5.0 pakiety dla tych, które już nie istnieje w projekcie

Aktualizowanie pakietów NuGet dla programu ASP.NET MVC 5.1 RTM nie powoduje aktualizacji narzędzi programu Visual Studio, takich jak ASP.NET rusztowania lub szablonu projektu aplikacji sieci Web ASP.NET. Korzystają z poprzedniej wersji pakietów środowiska uruchomieniowego ASP.NET (5.0.0.0). W związku z tym szkieletów ASP.NET zainstaluje poprzedniej wersji wymaganych pakietów (5.0.0.0), jeśli nie są one już dostępne w projektach. Jednak funkcja szkieletów ASP.NET w programie Visual Studio 2013 RTM lub Update 1 nie powoduje zastąpienia najnowszych pakietów w projektach. Jeśli używasz szkieletów ASP.NET po zaktualizowaniu pakietów projektów sieci Web interfejsu API 2.1 lub ASP.NET MVC w wersji 5.1, upewnij się, że wersje interfejsu API sieci Web i ASP.NET MVC są spójne. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Wyróżnianie składni Razor widoków w programie Visual Studio 2013

Po aktualizacji do RTM programu ASP.NET MVC 5.1 bez aktualizacji programu Visual Studio 2013 nie zapewni obsługę edytora programu Visual Studio dla wyróżnianie składni podczas edycji widokami Razor. Należy zaktualizować programu Visual Studio 2013, aby uzyskać tę obsługę. 

### <a name="type-renames"></a>Zmienia nazwę typu

Niektóre typy używane dla atrybutu rozszerzalności routingu zostały zmienione w wersji 5.1 RTM.

| **Stara nazwa typu (5.1 RC)** | **Nowy typ Name (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Poprawki błędów

Ta wersja zawiera również kilka poprawek. Pełną listę można znaleźć:

- [5.1.0 pakiet](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pakietu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 pakiet zawiera aktualizacje funkcji IntelliSense, ale nie poprawki.
