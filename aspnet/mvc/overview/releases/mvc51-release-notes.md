---
uid: mvc/overview/releases/mvc51-release-notes
title: Co nowego we wzorcu ASP.NET MVC 5.1 | Dokumentacja firmy Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 2b63ca8868bbf31be569e2eb2edb51141b9f4330
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362765"
---
<a name="whats-new-in-aspnet-mvc-51"></a>Co nowego we wzorcu ASP.NET MVC 5.1
====================
przez [firmy Microsoft](https://github.com/microsoft)

W tym temacie opisano what's new for ASP.NET sieci Web MVC 5.1.

- [Wymagania dotyczące oprogramowania](#SoftwareRequirements)
- [Pobieranie](#download)
- [Dokumentacja](#documentation)
- [Nowe funkcje we wzorcu ASP.NET MVC 5.1](#new-features)

    - [Atrybut ulepszenia routingu](#AttributeRouting)
    - [Obsługa ładowania początkowego szablony Edytora](#Bootstrap)
    - [Obsługa typu wyliczeniowego w widokach](#Enum)
    - [Dyskretny kod weryfikacji dla atrybutów MinLength/MaxLength.](#Unobtrusive)
    - [Obsługa kontekstu "this" w dyskretnego kodu Ajax](#thisContext)
- [Znane problemy i przełomowe zmiany](#KnownBreakingChanges)- [poprawki błędów](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Wymagania programowe

- Program Visual Studio 2012: Pobierz [ASP.NET and Web Tools 2013.1 dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Program Visual Studio 2013: Pobierz [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064). Ta aktualizacja jest potrzebne do edycji widoki ASP.NET MVC 5.1 Razor.

<a id="download"></a>
## <a name="download"></a>Pobieranie

Funkcje środowiska uruchomieniowego są wydawane jako pakiety NuGet w galerii NuGet. Wykonaj wszystkie pakiety środowiska uruchomieniowego [Semantic Versioning](http://semver.org/) specyfikacji. Najnowszy pakiet RTM programu ASP.NET MVC 5.1 ma następującą wersję: "5.1.2". Możesz zainstalować lub zaktualizować te pakiety przy użyciu [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Wersja ta oferuje również odpowiedni zlokalizowanych pakietów dla narzędzia NuGet.

Możesz zainstalować lub zaktualizować do wydanych pakietów NuGet za pomocą konsoli Menedżera pakietów NuGet:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentacja

W samouczkach i innych informacji na temat RTM programu ASP.NET MVC 5.1 są dostępne w witrynie sieci web platformy ASP.NET ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Nowe funkcje we wzorcu ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Atrybut ulepszenia routingu

 Routing teraz obsługuje ograniczenia, umożliwiające przechowywanie wersji i nagłówek atrybutów na podstawie wyboru trasy. Wiele aspektów tras atrybutów są teraz można dostosowywać za pomocą `IDirectRouteFactory` interfejsu i `RouteFactoryAttribute` klasy. Prefiks trasy jest teraz rozszerzalny za pośrednictwem `IRoutePrefix` interfejsu i `RoutePrefixAttribute` klasy. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Obsługa typu wyliczeniowego w widokach

1. Nowe `@Html.EnumDropDownListFor()` metody pomocnika. Powinny one być używane najbardziej pomocników HTML, ale należy pamiętać, że wyrażenia musi być [wyliczenia](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu lub [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) gdzie `T` jest [wyliczenia](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu. Użyj `EnumHelper.IsValidForEnumHelper()` do sprawdzenia tych wymagań.
2. Nowe `EnumHelper.GetSelectList()` metod, które zwracają `IList<SelectListItem>`. Jest to przydatne, gdy zachodzi potrzeba manipulowania listy wyboru przed wywołaniem, na przykład `@Html.DropDownListFor()`, lub gdy chcesz wyświetlić nazwy który `@Html.EnumDropDownListFor()` pokazuje.

Poniższy kod przedstawia te interfejsy API.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Możesz zobaczyć kompletny przykład [tutaj](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Obsługa ładowania początkowego szablony Edytora

Obecnie zezwalamy na przekazywanie w atrybutach HTML w [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) jako [obiekt anonimowy](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Na przykład:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Sprawdzanie poprawności dyskretnego kodu MinLengthAttribute i MaxLengthAttribute

Dla właściwości ozdobione będzie teraz obsługiwane weryfikacji po stronie klienta dla typu ciąg i tablica [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) i [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atrybutów.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Obsługa kontekstu "this" w dyskretnego kodu Ajax

Funkcje wywołania zwrotnego (`OnBegin, OnComplete, OnFailure, OnSuccess`) będą teraz mogli zlokalizować wywołania elementu za pomocą `this` kontekstu. Na przykład:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

### <a name="attribute-routing"></a>Routing atrybutów

Teraz będzie zgłaszać niejasności w atrybucie dopasowania routingu, błąd, a nie wybierając pierwsze dopasowanie.

Tras atrybutów mogą używać tychże `{controller}` parametru i przy użyciu `{action}` parametru na trasach umieścić w akcji. Korzysta z tych parametrów najprawdopodobniej doprowadzi do niejednoznaczności. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Tworzenie szkieletów MVC/interfejsu API sieci Web do projektu przy użyciu 5.1 pakietów skutkuje 5.0 pakietów dla tych, które jeszcze nie istnieją w projekcie

Trwa aktualizowanie pakietów NuGet dla platformy ASP.NET MVC 5.1 RTM nie powoduje aktualizacji narzędzi programu Visual Studio, takich jak funkcja tworzenia szkieletu ASP.NET lub szablon projektu aplikacji sieci Web ASP.NET. Używają poprzedniej wersji pakiety środowiska uruchomieniowego platformy ASP.NET (5.0.0.0). Co w efekcie funkcja tworzenia szkieletu ASP.NET zainstaluje starszą wersję (5.0.0.0) wymagane pakiety, jeśli nie są jeszcze dostępne w projektach. Funkcja tworzenia szkieletu ASP.NET w Visual Studio 2013 RTM i Update 1 nie zastąpi jednak najnowszych pakietów w swoich projektach. Jeśli używasz funkcja tworzenia szkieletu ASP.NET po zaktualizowaniu pakietów projektów sieci Web API 2.1 lub platformy ASP.NET MVC 5.1, upewnij się, że wersje interfejsu API sieci Web platformy ASP.NET MVC są spójne. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Wyróżnianie składni Razor widoków w programie Visual Studio 2013

Po aktualizacji do RTM programu ASP.NET MVC 5.1 bez aktualizowania programu Visual Studio 2013 nie uzyskasz pomoc techniczna do edytora programu Visual Studio do wyróżniania składni podczas edytowania widokami Razor. Będzie konieczne zaktualizowanie programu Visual Studio 2013 można pobrać tę obsługę. 

### <a name="type-renames"></a>Zmienia nazwę typu

Niektóre typy używane do rozszerzalności routingu atrybutu są zmieniane w wersji 5.1 RTM.

| **Stara nazwa typu (5.1 RC)** | **Nowy typ nazwy (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Poprawki błędów

Ta wersja zawiera także kilka poprawek usterek. Pełną listę można znaleźć:

- [5.1.0 pakiet](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 pakiet](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 pakiet zawiera aktualizacje funkcji IntelliSense, ale nie poprawki.
