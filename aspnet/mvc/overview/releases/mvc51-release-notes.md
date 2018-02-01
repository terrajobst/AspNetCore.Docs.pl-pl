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
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="178ae-102">Co to jest nowe w programie ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="178ae-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="178ae-103">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="178ae-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="178ae-104">W tym temacie opisano nowe dla 5.1 MVC sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="178ae-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="178ae-105">Wymagania dotyczące oprogramowania</span><span class="sxs-lookup"><span data-stu-id="178ae-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="178ae-106">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="178ae-106">Download</span></span>](#download)
- [<span data-ttu-id="178ae-107">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="178ae-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="178ae-108">Nowe funkcje w programie ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="178ae-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="178ae-109">Atrybut ulepszenia routingu</span><span class="sxs-lookup"><span data-stu-id="178ae-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="178ae-110">Obsługa inicjowania szablony Edytora</span><span class="sxs-lookup"><span data-stu-id="178ae-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="178ae-111">Obsługa wyliczenia w widokach</span><span class="sxs-lookup"><span data-stu-id="178ae-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="178ae-112">Dyskretny kod weryfikacji dla atrybutów MinLength/element MaxLength</span><span class="sxs-lookup"><span data-stu-id="178ae-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="178ae-113">Obsługa kontekstu "this" w dyskretnego kodu Ajax</span><span class="sxs-lookup"><span data-stu-id="178ae-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="178ae-114">[Znane problemy i fundamentalne zmiany](#KnownBreakingChanges)- [poprawki błędów](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="178ae-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="178ae-115">Wymagania programowe</span><span class="sxs-lookup"><span data-stu-id="178ae-115">Software Requirements</span></span>

- <span data-ttu-id="178ae-116">Program Visual Studio 2012: Pobierz [ASP.NET i sieć Web narzędzi 2013.1 dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="178ae-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="178ae-117">Programu Visual Studio 2013: Pobieranie [programu Visual Studio 2013, aktualizacja 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="178ae-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="178ae-118">Ta aktualizacja jest potrzebne do edycji widoki ASP.NET MVC 5.1 Razor.</span><span class="sxs-lookup"><span data-stu-id="178ae-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="178ae-119">Pobieranie</span><span class="sxs-lookup"><span data-stu-id="178ae-119">Download</span></span>

<span data-ttu-id="178ae-120">Funkcje środowiska uruchomieniowego są wydawane jako pakietów NuGet w galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="178ae-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="178ae-121">Wykonaj wszystkie pakiety środowiska uruchomieniowego [Wersjonowania semantycznego](http://semver.org/) specyfikacji.</span><span class="sxs-lookup"><span data-stu-id="178ae-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="178ae-122">Najnowszy pakiet RTM programu ASP.NET MVC 5.1 ma następującą wersję: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="178ae-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="178ae-123">Można zainstalować lub zaktualizować te pakiety za pośrednictwem [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="178ae-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="178ae-124">Wydanie obejmuje również odpowiedniego zlokalizowane pakiety na NuGet.</span><span class="sxs-lookup"><span data-stu-id="178ae-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="178ae-125">Można zainstalować lub zaktualizować do pakietów NuGet wydanych przy użyciu konsoli Menedżera pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="178ae-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="178ae-126">Dokumentacja</span><span class="sxs-lookup"><span data-stu-id="178ae-126">Documentation</span></span>

<span data-ttu-id="178ae-127">Samouczki i inne informacje o RTM programu ASP.NET MVC 5.1 są dostępne w witrynie sieci web platformy ASP.NET (https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="178ae-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="178ae-128">Nowe funkcje w programie ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="178ae-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="178ae-129">Atrybut ulepszenia routingu</span><span class="sxs-lookup"><span data-stu-id="178ae-129">Attribute routing improvements</span></span>

 <span data-ttu-id="178ae-130">Atrybut routingu teraz obsługuje ograniczenia, włączanie wersji i nagłówek na podstawie wyboru trasy.</span><span class="sxs-lookup"><span data-stu-id="178ae-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="178ae-131">Wiele aspektów tras atrybutów są teraz można dostosować za pomocą `IDirectRouteFactory` interfejsu i `RouteFactoryAttribute` klasy.</span><span class="sxs-lookup"><span data-stu-id="178ae-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="178ae-132">Prefiks trasy jest teraz rozszerzalny za pośrednictwem `IRoutePrefix` interfejsu i `RoutePrefixAttribute` klasy.</span><span class="sxs-lookup"><span data-stu-id="178ae-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="178ae-133">Obsługa wyliczenia w widokach</span><span class="sxs-lookup"><span data-stu-id="178ae-133">Enum support in views</span></span>

1. <span data-ttu-id="178ae-134">Nowe `@Html.EnumDropDownListFor()` metody pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="178ae-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="178ae-135">Powinny być one używane jak najbardziej pomocników HTML, ale należy pamiętać, że wyrażenia musi być [wyliczenia](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu lub [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) gdzie `T` jest [wyliczenia](https://msdn.microsoft.com/en-us/library/cc138362.aspx) typu.</span><span class="sxs-lookup"><span data-stu-id="178ae-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="178ae-136">Użyj `EnumHelper.IsValidForEnumHelper()` Aby sprawdzić te wymagania.</span><span class="sxs-lookup"><span data-stu-id="178ae-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="178ae-137">Nowy `EnumHelper.GetSelectList()` metody zwracające `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="178ae-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="178ae-138">Jest to przydatne, gdy potrzebne do modyfikowania listy wyboru przed wywołaniem, na przykład `@Html.DropDownListFor()`, lub gdy chcesz wyświetlić nazwy którego `@Html.EnumDropDownListFor()` zawiera.</span><span class="sxs-lookup"><span data-stu-id="178ae-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="178ae-139">Poniższy kod przedstawia te interfejsy API.</span><span class="sxs-lookup"><span data-stu-id="178ae-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="178ae-140">Widać pełny przykład [tutaj](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="178ae-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="178ae-141">Obsługa inicjowania szablony Edytora</span><span class="sxs-lookup"><span data-stu-id="178ae-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="178ae-142">Mamy teraz możliwe przekazywanie w atrybutach HTML w [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) jako [obiekt anonimowy](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="178ae-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="178ae-143">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="178ae-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="178ae-144">Sprawdzanie poprawności dyskretnego kodu MinLengthAttribute i MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="178ae-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="178ae-145">Dla właściwości ozdobione będą teraz obsługiwane weryfikacji po stronie klienta dla typów ciąg i tablica [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) i [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) atrybutów.</span><span class="sxs-lookup"><span data-stu-id="178ae-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="178ae-146">Obsługa kontekstu "this" w dyskretnego kodu Ajax</span><span class="sxs-lookup"><span data-stu-id="178ae-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="178ae-147">Funkcje wywołania zwrotnego (`OnBegin, OnComplete, OnFailure, OnSuccess`) teraz będzie mógł zlokalizować wywoływanie elementu za pomocą `this` kontekstu.</span><span class="sxs-lookup"><span data-stu-id="178ae-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="178ae-148">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="178ae-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="178ae-149">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="178ae-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="178ae-150">Atrybut routingu</span><span class="sxs-lookup"><span data-stu-id="178ae-150">Attribute Routing</span></span>

<span data-ttu-id="178ae-151">Niejednoznaczności w dopasowań routingu atrybutu teraz będzie zgłaszać błąd, a nie wybranie pierwszego dopasowania.</span><span class="sxs-lookup"><span data-stu-id="178ae-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="178ae-152">Tras atrybutów mogą używać tychże `{controller}` parametru i korzystania z `{action}` parametru na trasach dotyczącymi akcji.</span><span class="sxs-lookup"><span data-stu-id="178ae-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="178ae-153">Korzysta z tych parametrów najprawdopodobniej doprowadzi do niejednoznaczności.</span><span class="sxs-lookup"><span data-stu-id="178ae-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="178ae-154">Funkcja szkieletów MVC/interfejs API sieci Web do projektu z 5.1 wynikiem pakiety w wersji 5.0 pakiety dla tych, które już nie istnieje w projekcie</span><span class="sxs-lookup"><span data-stu-id="178ae-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="178ae-155">Aktualizowanie pakietów NuGet dla programu ASP.NET MVC 5.1 RTM nie powoduje aktualizacji narzędzi programu Visual Studio, takich jak ASP.NET rusztowania lub szablonu projektu aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="178ae-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="178ae-156">Korzystają z poprzedniej wersji pakietów środowiska uruchomieniowego ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="178ae-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="178ae-157">W związku z tym szkieletów ASP.NET zainstaluje poprzedniej wersji wymaganych pakietów (5.0.0.0), jeśli nie są one już dostępne w projektach.</span><span class="sxs-lookup"><span data-stu-id="178ae-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="178ae-158">Jednak funkcja szkieletów ASP.NET w programie Visual Studio 2013 RTM lub Update 1 nie powoduje zastąpienia najnowszych pakietów w projektach.</span><span class="sxs-lookup"><span data-stu-id="178ae-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="178ae-159">Jeśli używasz szkieletów ASP.NET po zaktualizowaniu pakietów projektów sieci Web interfejsu API 2.1 lub ASP.NET MVC w wersji 5.1, upewnij się, że wersje interfejsu API sieci Web i ASP.NET MVC są spójne.</span><span class="sxs-lookup"><span data-stu-id="178ae-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="178ae-160">Wyróżnianie składni Razor widoków w programie Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="178ae-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="178ae-161">Po aktualizacji do RTM programu ASP.NET MVC 5.1 bez aktualizacji programu Visual Studio 2013 nie zapewni obsługę edytora programu Visual Studio dla wyróżnianie składni podczas edycji widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="178ae-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="178ae-162">Należy zaktualizować programu Visual Studio 2013, aby uzyskać tę obsługę.</span><span class="sxs-lookup"><span data-stu-id="178ae-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="178ae-163">Zmienia nazwę typu</span><span class="sxs-lookup"><span data-stu-id="178ae-163">Type Renames</span></span>

<span data-ttu-id="178ae-164">Niektóre typy używane dla atrybutu rozszerzalności routingu zostały zmienione w wersji 5.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="178ae-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="178ae-165">**Stara nazwa typu (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="178ae-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="178ae-166">**Nowy typ Name (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="178ae-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="178ae-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="178ae-167">IDirectRouteProvider</span></span> | <span data-ttu-id="178ae-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="178ae-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="178ae-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="178ae-169">RouteProviderAttribute</span></span> | <span data-ttu-id="178ae-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="178ae-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="178ae-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="178ae-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="178ae-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="178ae-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="178ae-173">Poprawki błędów</span><span class="sxs-lookup"><span data-stu-id="178ae-173">Bug Fixes</span></span>

<span data-ttu-id="178ae-174">Ta wersja zawiera również kilka poprawek.</span><span class="sxs-lookup"><span data-stu-id="178ae-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="178ae-175">Pełną listę można znaleźć:</span><span class="sxs-lookup"><span data-stu-id="178ae-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="178ae-176">5.1.0 pakiet</span><span class="sxs-lookup"><span data-stu-id="178ae-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="178ae-177">5.1.1 pakietu</span><span class="sxs-lookup"><span data-stu-id="178ae-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="178ae-178">5.1.2 pakiet zawiera aktualizacje funkcji IntelliSense, ale nie poprawki.</span><span class="sxs-lookup"><span data-stu-id="178ae-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
