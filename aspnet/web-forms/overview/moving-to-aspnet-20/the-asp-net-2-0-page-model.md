---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Model 2.0 strony ASP.NET | Dokumentacja firmy Microsoft
author: microsoft
description: W programie ASP.NET 1.x, deweloperzy ma wybór między kodu wbudowanego i modelu kodu związane z kodem. Związane z kodem można zaimplementować za pomocą Src attr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: fda85ec03f845cafa7720382bf85652937932c44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="the-aspnet-20-page-model"></a><span data-ttu-id="3217c-104">Model 2.0 strony ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3217c-104">The ASP.NET 2.0 Page Model</span></span>
====================
<span data-ttu-id="3217c-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="3217c-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="3217c-106">W programie ASP.NET 1.x, deweloperzy ma wybór między kodu wbudowanego i modelu kodu związane z kodem.</span><span class="sxs-lookup"><span data-stu-id="3217c-106">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="3217c-107">Związane z kodem można zaimplementować za pomocą atrybutu Src lub atrybut plik CodeBehind @Page dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="3217c-107">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="3217c-108">W programie ASP.NET 2.0 deweloperzy nadal mają wybór między kodu wbudowanego i związane z kodem, ale było istotne ulepszenia modelu związane z kodem.</span><span class="sxs-lookup"><span data-stu-id="3217c-108">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>


<span data-ttu-id="3217c-109">W programie ASP.NET 1.x, deweloperzy ma wybór między kodu wbudowanego i modelu kodu związane z kodem.</span><span class="sxs-lookup"><span data-stu-id="3217c-109">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="3217c-110">Związane z kodem można zaimplementować za pomocą atrybutu Src lub atrybut plik CodeBehind @Page dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="3217c-110">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="3217c-111">W programie ASP.NET 2.0 deweloperzy nadal mają wybór między kodu wbudowanego i związane z kodem, ale było istotne ulepszenia modelu związane z kodem.</span><span class="sxs-lookup"><span data-stu-id="3217c-111">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>

## <a name="improvements-in-the-code-behind-model"></a><span data-ttu-id="3217c-112">Ulepszenia w modelu związane z kodem</span><span class="sxs-lookup"><span data-stu-id="3217c-112">Improvements in the Code-Behind Model</span></span>

<span data-ttu-id="3217c-113">Aby lepiej zrozumieć zmian w modelu kodu powiązanego w programie ASP.NET 2.0, zaleca się szybko przejrzeć modelu w postaci, w jakiej były dostępne w programie ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="3217c-113">In order to fully understand the changes in the code-behind model in ASP.NET 2.0, its best to quickly review the model as it existed in ASP.NET 1.x.</span></span>

## <a name="the-code-behind-model-in-aspnet-1x"></a><span data-ttu-id="3217c-114">Model kodu powiązanego w programie ASP.NET 1.x</span><span class="sxs-lookup"><span data-stu-id="3217c-114">The Code-Behind Model in ASP.NET 1.x</span></span>

<span data-ttu-id="3217c-115">W programie ASP.NET 1.x, modelu CodeBehind składa się z plikiem ASPX (formularza sieci Web) i plik CodeBehind kodem programowania.</span><span class="sxs-lookup"><span data-stu-id="3217c-115">In ASP.NET 1.x, the code-behind model consisted of an ASPX file (the Webform) and a code-behind file containing programming code.</span></span> <span data-ttu-id="3217c-116">Dwa pliki zostały połączone za pomocą @Page dyrektywy w pliku ASPX.</span><span class="sxs-lookup"><span data-stu-id="3217c-116">The two files were connected using the @Page directive in the ASPX file.</span></span> <span data-ttu-id="3217c-117">Na stronie ASPX każdego formantu ma deklarację odpowiednie w pliku CodeBehind jako zmienna wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="3217c-117">Each control on the ASPX page had a corresponding declaration in the code-behind file as an instance variable.</span></span> <span data-ttu-id="3217c-118">Plik CodeBehind również zawiera kod dla wiązania zdarzeń i wygenerowanego kodu niezbędne do projektanta programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3217c-118">The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer.</span></span> <span data-ttu-id="3217c-119">Ten model stosunkowo dobrze działa, ale ponieważ ASP.NET, co element na stronie ASPX wymagane odpowiedni kod w pliku związanym z kodem, nie było żadnych true oddzielenie kodu i zawartości.</span><span class="sxs-lookup"><span data-stu-id="3217c-119">This model worked fairly well, but because every ASP.NET element in the ASPX page required corresponding code in the code-behind file, there was no true separation of code and content.</span></span> <span data-ttu-id="3217c-120">Na przykład jeśli projektant dodany nowy formant serwera z plikiem ASPX poza środowiska IDE programu Visual Studio, aplikacji spowoduje przerwanie z powodu braku deklaracji dla tego formantu w pliku CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="3217c-120">For example, if a designer added a new server control to an ASPX file outside of the Visual Studio IDE, the application would break due to the absence of a declaration for that control in the code-behind file.</span></span>

## <a name="the-code-behind-model-in-aspnet-20"></a><span data-ttu-id="3217c-121">Model kodu powiązanego w programie ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="3217c-121">The Code-Behind Model in ASP.NET 2.0</span></span>

<span data-ttu-id="3217c-122">Platforma ASP.NET 2.0 znacznie poprawia tego modelu.</span><span class="sxs-lookup"><span data-stu-id="3217c-122">ASP.NET 2.0 greatly improves upon this model.</span></span> <span data-ttu-id="3217c-123">W programie ASP.NET 2.0, związane z kodem jest implementowane za pomocą nowej *klasy częściowe* dostępnych w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="3217c-123">In ASP.NET 2.0, code-behind is implemented using the new *partial classes* provided in ASP.NET 2.0.</span></span> <span data-ttu-id="3217c-124">Klasy związane z kodem w programie ASP.NET 2.0 jest definiowane jako częściowej klasy, co oznacza, że zawiera on tylko część definicji klasy.</span><span class="sxs-lookup"><span data-stu-id="3217c-124">The code-behind class in ASP.NET 2.0 is definied as a partial class meaning that it contains only part of the class definition.</span></span> <span data-ttu-id="3217c-125">Pozostała część definicji klasy dynamicznie jest generowany przez ASP.NET 2.0 za pomocą strony ASPX, w czasie wykonywania, lub gdy witryna sieci Web została wstępnie skompilowana.</span><span class="sxs-lookup"><span data-stu-id="3217c-125">The remaining part of the class definition is dynamically generated by ASP.NET 2.0 using the ASPX page at runtime or when the Web site is precompiled.</span></span> <span data-ttu-id="3217c-126">Skojarzenia między plik CodeBehind i strony ASPX nadal jest określana za pomocą dyrektywy @ Page.</span><span class="sxs-lookup"><span data-stu-id="3217c-126">The link between the code-behind file and the ASPX page is still established using the @ Page directive.</span></span> <span data-ttu-id="3217c-127">Jednak zamiast atrybutu plik CodeBehind lub Src, ASP.NET 2.0 teraz używa atrybut CodeFile.</span><span class="sxs-lookup"><span data-stu-id="3217c-127">However, instead of a CodeBehind or Src attribute, ASP.NET 2.0 now uses the CodeFile attribute.</span></span> <span data-ttu-id="3217c-128">Atrybut Inherits umożliwia również określić nazwę klasy dla strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-128">The Inherits attribute is also used to specify the class name for the page.</span></span>

<span data-ttu-id="3217c-129">Typowy dyrektywy @ Page może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="3217c-129">A typical @ Page directive might look like this:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

<span data-ttu-id="3217c-130">Definicję typowej klasy w pliku CodeBehind ASP.NET 2.0 może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="3217c-130">A typical class definition in an ASP.NET 2.0 code-behind file might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="3217c-131">C# i Visual Basic są tylko języki zarządzanych, które obsługuje obecnie klas częściowych.</span><span class="sxs-lookup"><span data-stu-id="3217c-131">C# and Visual Basic are the only managed languages that currently support partial classes.</span></span> <span data-ttu-id="3217c-132">W związku z tym deweloperzy przy użyciu J# nie będzie mógł używać modelu kodu powiązanego w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="3217c-132">Therefore, developers using J# will not be able to use the code-behind model in ASP.NET 2.0.</span></span>


<span data-ttu-id="3217c-133">Nowy model zwiększa modelu związane z kodem, ponieważ deweloperzy mają teraz pliki kodu, które zawierają tylko kod, który zostały one utworzone.</span><span class="sxs-lookup"><span data-stu-id="3217c-133">The new model enhances the code-behind model because developers will now have code files that contain only the code that they have created.</span></span> <span data-ttu-id="3217c-134">Zapewnia także true oddzielenie kodu i zawartości, ponieważ nie ma żadnych wystąpienia deklaracji zmiennych w pliku CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="3217c-134">It also provides for a true separation of code and content because there are no instance variable declarations in the code-behind file.</span></span>

> [!NOTE]
> <span data-ttu-id="3217c-135">Ponieważ częściowej klasy strony ASPX Jeśli powiązanie zdarzeń ma miejsce, deweloperów języka Visual Basic osiągnąć wzrost wydajności nieznaczne przy użyciu słowa kluczowego dojść w związane z kodem powiązać zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="3217c-135">Because the partial class for the ASPX page is where event binding takes place, Visual Basic developers can realize a slight performance increase by using the Handles keyword in code-behind to bind events.</span></span> <span data-ttu-id="3217c-136">C# ma nie równoważne — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="3217c-136">C# has no equivalent keyword.</span></span>


## <a name="new--page-directive-attributes"></a><span data-ttu-id="3217c-137">Nowe atrybuty @ Page — dyrektywa</span><span class="sxs-lookup"><span data-stu-id="3217c-137">New @ Page Directive Attributes</span></span>

<span data-ttu-id="3217c-138">Platforma ASP.NET 2.0 dodaje wiele nowych atrybutów do dyrektywy @ Page.</span><span class="sxs-lookup"><span data-stu-id="3217c-138">ASP.NET 2.0 adds many new attributes to the @ Page directive.</span></span> <span data-ttu-id="3217c-139">Następujące atrybuty są nowe w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="3217c-139">The following attributes are new in ASP.NET 2.0.</span></span>

## <a name="async"></a><span data-ttu-id="3217c-140">Async</span><span class="sxs-lookup"><span data-stu-id="3217c-140">Async</span></span>

<span data-ttu-id="3217c-141">Atrybut Async pozwala na skonfigurowanie strony, aby być wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="3217c-141">The Async attribute allows you to configure page to be executed asynchronously.</span></span> <span data-ttu-id="3217c-142">Obejmuje również asynchroniczne stron w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="3217c-142">Well cover asynchronous pages later in this module.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="3217c-143">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="3217c-143">AsyncTimeout</span></span>

<span data-ttu-id="3217c-144">Określony limit czasu asynchronicznego stron.</span><span class="sxs-lookup"><span data-stu-id="3217c-144">Specified the timeout for asynchronous pages.</span></span> <span data-ttu-id="3217c-145">Wartość domyślna wynosi 45 sekund.</span><span class="sxs-lookup"><span data-stu-id="3217c-145">The default is 45 seconds.</span></span>

## <a name="codefile"></a><span data-ttu-id="3217c-146">CodeFile</span><span class="sxs-lookup"><span data-stu-id="3217c-146">CodeFile</span></span>

<span data-ttu-id="3217c-147">Atrybut CodeFile zastępuje atrybutu plik CodeBehind w programie Visual Studio 2002/2003.</span><span class="sxs-lookup"><span data-stu-id="3217c-147">The CodeFile attribute is the replacement for the CodeBehind attribute in Visual Studio 2002/2003.</span></span>

### <a name="codefilebaseclass"></a><span data-ttu-id="3217c-148">CodeFileBaseClass</span><span class="sxs-lookup"><span data-stu-id="3217c-148">CodeFileBaseClass</span></span>

<span data-ttu-id="3217c-149">Atrybut CodeFileBaseClass jest używany w przypadkach, w którym ma wiele stron pochodzić z jednej klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="3217c-149">The CodeFileBaseClass attribute is used in cases where you want multiple pages to derive from a single base class.</span></span> <span data-ttu-id="3217c-150">Z powodu wykonania klas częściowych w ASP.NET, bez tego atrybutu klasę podstawową używaną do odwołują się do formantów zadeklarowany w strony ASPX współdzielonych pól wspólnej nie będzie działać prawidłowo ponieważ ASP. Aparat kompilacji sieci automatycznie utworzy nowe elementy członkowskie oparte na formanty na stronie.</span><span class="sxs-lookup"><span data-stu-id="3217c-150">Because of the implementation of partial classes in ASP.NET, without this attribute, a base class that uses shared common fields to reference controls declared in an ASPX page would not work properly because ASP.NETs compilation engine will automatically create new members based on controls in the page.</span></span> <span data-ttu-id="3217c-151">W związku z tym jeśli chcesz wspólna klasa podstawowa dla dwóch lub więcej stron w programie ASP.NET, należy zdefiniować Określ klasy podstawowej w atrybucie CodeFileBaseClass, a następnie pochodzi każdej klasy stron od tej klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="3217c-151">Therefore, if you want a common base class for two or more pages in ASP.NET, you will need to define specify your base class in the CodeFileBaseClass attribute and then derive each pages class from that base class.</span></span> <span data-ttu-id="3217c-152">Atrybut CodeFile jest również wymagany, gdy jest używany ten atrybut.</span><span class="sxs-lookup"><span data-stu-id="3217c-152">The CodeFile attribute is also required when this attribute is used.</span></span>

## <a name="compilationmode"></a><span data-ttu-id="3217c-153">właściwość compilationMode</span><span class="sxs-lookup"><span data-stu-id="3217c-153">CompilationMode</span></span>

<span data-ttu-id="3217c-154">Ten atrybut umożliwia należy ustawić właściwość CompilationMode strony ASPX.</span><span class="sxs-lookup"><span data-stu-id="3217c-154">This attribute allows you to set the CompilationMode property of the ASPX page.</span></span> <span data-ttu-id="3217c-155">Właściwość CompilationMode jest wyliczenie zawierające wartości **zawsze**, **automatycznie**, i **nigdy**.</span><span class="sxs-lookup"><span data-stu-id="3217c-155">The CompilationMode property is an enumeration containing the values **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="3217c-156">Wartość domyślna to **zawsze**.</span><span class="sxs-lookup"><span data-stu-id="3217c-156">The default is **Always**.</span></span> <span data-ttu-id="3217c-157">**Automatycznie** ustawienie uniemożliwi dynamicznie, jeśli to możliwe kompilowania strony ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3217c-157">The **Auto** setting will prevent ASP.NET from dynamically compiling the page if possible.</span></span> <span data-ttu-id="3217c-158">Zwiększa wydajność, z wyłączeniem stron z kompilacji dynamicznej.</span><span class="sxs-lookup"><span data-stu-id="3217c-158">Excluding pages from dynamic compilation increases performance.</span></span> <span data-ttu-id="3217c-159">Jednak jeśli strony, który jest wykluczony zawiera ten kod, który musi być skompilowany, będzie można zgłoszony błąd podczas przeglądania strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-159">However, if a page that is excluded contains that code that must be compiled, an error will be thrown when the page is browsed.</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="3217c-160">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="3217c-160">EnableEventValidation</span></span>

<span data-ttu-id="3217c-161">Ten atrybut określa, czy są weryfikowane zdarzenia odświeżania strony i wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="3217c-161">This attribute specifies whether or not postback and callback events are validated.</span></span> <span data-ttu-id="3217c-162">Gdy ta opcja jest włączona, argumenty odświeżania lub wywołania zwrotnego zdarzenia są sprawdzane w celu zapewnienia, że pochodzą z formantu serwera, który pierwotnie je odwzorował.</span><span class="sxs-lookup"><span data-stu-id="3217c-162">When this is enabled, arguments to postback or callback events are checked to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="3217c-163">Właściwość EnableTheming</span><span class="sxs-lookup"><span data-stu-id="3217c-163">EnableTheming</span></span>

<span data-ttu-id="3217c-164">Ten atrybut określa, czy kompozycji ASP.NET są używane na stronie.</span><span class="sxs-lookup"><span data-stu-id="3217c-164">This attribute specifies whether or not ASP.NET themes are used on a page.</span></span> <span data-ttu-id="3217c-165">Wartość domyślna to **false**.</span><span class="sxs-lookup"><span data-stu-id="3217c-165">The default is **false**.</span></span> <span data-ttu-id="3217c-166">Motywy ASP.NET są objęte [10 modułu](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="3217c-166">ASP.NET themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="linepragmas"></a><span data-ttu-id="3217c-167">LinePragmas</span><span class="sxs-lookup"><span data-stu-id="3217c-167">LinePragmas</span></span>

<span data-ttu-id="3217c-168">Ten atrybut określa, czy dyrektywy pragma linii powinny zostać dodane podczas kompilacji.</span><span class="sxs-lookup"><span data-stu-id="3217c-168">This attribute specifies whether line pragmas should be added during compilation.</span></span> <span data-ttu-id="3217c-169">Wyrażenia pragma wiersza są opcje używane przez debugery, aby oznaczyć określonych sekcji kodu.</span><span class="sxs-lookup"><span data-stu-id="3217c-169">Line pragmas are options used by debuggers to mark specific sections of code.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="3217c-170">MaintainScrollPositionOnPostback</span><span class="sxs-lookup"><span data-stu-id="3217c-170">MaintainScrollPositionOnPostback</span></span>

<span data-ttu-id="3217c-171">Ten atrybut określa, czy JavaScript jest wstrzykiwane do strony w celu utrzymania pozycji przewijania między odświeżeniami.</span><span class="sxs-lookup"><span data-stu-id="3217c-171">This attribute specifies whether or not JavaScript is injected into the page in order to maintain scroll position between postbacks.</span></span> <span data-ttu-id="3217c-172">Ten atrybut jest **false** domyślnie.</span><span class="sxs-lookup"><span data-stu-id="3217c-172">This attribute is **false** by default.</span></span>

<span data-ttu-id="3217c-173">Jeśli ten atrybut jest **true**, doda ASP.NET &lt;skryptu&gt; bloku odświeżania strony, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="3217c-173">When this attribute is **true**, ASP.NET will add a &lt;script&gt; block on postback that looks like this:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

<span data-ttu-id="3217c-174">Należy pamiętać, że src tego bloku skryptu WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="3217c-174">Note that the src for this script block is WebResource.axd.</span></span> <span data-ttu-id="3217c-175">Ten zasób nie jest ścieżką fizyczną.</span><span class="sxs-lookup"><span data-stu-id="3217c-175">This resource is not a physical path.</span></span> <span data-ttu-id="3217c-176">Po zażądaniu tego skryptu ASP.NET dynamicznie tworzy skrypt.</span><span class="sxs-lookup"><span data-stu-id="3217c-176">When this script is requested, ASP.NET dynamically builds the script.</span></span>

### <a name="masterpagefile"></a><span data-ttu-id="3217c-177">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="3217c-177">MasterPageFile</span></span>

<span data-ttu-id="3217c-178">Ten atrybut określa plik strony głównej dla bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-178">This attribute specifies the master page file for the current page.</span></span> <span data-ttu-id="3217c-179">Ścieżka może być względna lub bezwzględna.</span><span class="sxs-lookup"><span data-stu-id="3217c-179">The path can be relative or absolute.</span></span> <span data-ttu-id="3217c-180">Strony główne są objęte [modułu 4](master-pages.md).</span><span class="sxs-lookup"><span data-stu-id="3217c-180">Master pages are covered in [Module 4](master-pages.md).</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="3217c-181">Element StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="3217c-181">StyleSheetTheme</span></span>

<span data-ttu-id="3217c-182">Ten atrybut umożliwia zastąpienie interfejsu użytkownika wygląd właściwości zdefiniowane przez motyw programu ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="3217c-182">This attribute allows you to override user-interface appearance properties defined by an ASP.NET 2.0 theme.</span></span> <span data-ttu-id="3217c-183">Motywy są objęte [10 modułu](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="3217c-183">Themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="theme"></a><span data-ttu-id="3217c-184">Motyw</span><span class="sxs-lookup"><span data-stu-id="3217c-184">Theme</span></span>

<span data-ttu-id="3217c-185">Określa motyw dla strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-185">Specifies the theme for the page.</span></span> <span data-ttu-id="3217c-186">Jeśli nie określono wartości dla atrybutu element StyleSheetTheme, atrybutu motywu zastępuje wszystkie style stosowana do formantów na stronie.</span><span class="sxs-lookup"><span data-stu-id="3217c-186">If a value is not specified for the StyleSheetTheme attribute, the Theme attribute overrides all styles applied to controls on the page.</span></span>

## <a name="title"></a><span data-ttu-id="3217c-187">Tytuł</span><span class="sxs-lookup"><span data-stu-id="3217c-187">Title</span></span>

<span data-ttu-id="3217c-188">Ustawia tytuł strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-188">Sets the title for the page.</span></span> <span data-ttu-id="3217c-189">Wartość określona w tym miejscu będą widoczne w &lt;tytuł&gt; element renderowanej strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-189">The value specified here will appear in the &lt;title&gt; element of the rendered page.</span></span>

### <a name="viewstateencryptionmode"></a><span data-ttu-id="3217c-190">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="3217c-190">ViewStateEncryptionMode</span></span>

<span data-ttu-id="3217c-191">Ustawia wartość wyliczenia ViewStateEncryptionMode.</span><span class="sxs-lookup"><span data-stu-id="3217c-191">Sets the value for the ViewStateEncryptionMode enumeration.</span></span> <span data-ttu-id="3217c-192">Dostępne wartości to **zawsze**, **automatycznie**, i **nigdy**.</span><span class="sxs-lookup"><span data-stu-id="3217c-192">The available values are **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="3217c-193">Wartość domyślna to **automatycznie**. Jeśli ten atrybut ma ustawioną wartość **automatycznie**, są szyfrowane, stan widoku jest formant żądania przez wywołanie metody **RegisterRequiresViewStateEncryption** — metoda.</span><span class="sxs-lookup"><span data-stu-id="3217c-193">The default value is **Auto**. When this attribute is set to a value of **Auto**, viewstate is encrypted is a control requests it by calling the **RegisterRequiresViewStateEncryption** method.</span></span>

## <a name="setting-public-property-values-via-the--page-directive"></a><span data-ttu-id="3217c-194">Ustawianie wartości właściwości publicznej za pośrednictwem @ Page — dyrektywa</span><span class="sxs-lookup"><span data-stu-id="3217c-194">Setting Public Property Values via the @ Page Directive</span></span>

<span data-ttu-id="3217c-195">Kolejna nowa możliwość dyrektywy @ Page, w programie ASP.NET 2.0 jest możliwość określenia początkowej wartości właściwości publicznej klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="3217c-195">Another new capability of the @ Page directive in ASP.NET 2.0 is the ability to set the initial value of public properties of a base class.</span></span> <span data-ttu-id="3217c-196">Załóżmy na przykład, że masz właściwości publicznej o nazwie **SomeText** w klasie podstawowej i d podoba Ci się zostać zainicjowany do **Hello** po załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-196">Suppose, for example, that you have a public property called **SomeText** in your base class and you d like it to be initialized to **Hello** when a page is loaded.</span></span> <span data-ttu-id="3217c-197">Można to zrobić po prostu ustawiając wartość w dyrektywy @ Page w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3217c-197">You can accomplish this by simply setting the value in the @ Page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

<span data-ttu-id="3217c-198">**SomeText** atrybutu dyrektywy @ Page ustawia początkowa wartość właściwości SomeText w klasie podstawowej, aby *Witaj!*.</span><span class="sxs-lookup"><span data-stu-id="3217c-198">The **SomeText** attribute of the @ Page directive sets the initial value of the SomeText property in the base class to *Hello!*.</span></span> <span data-ttu-id="3217c-199">Poniższy klip wideo jest wskazówki ustawienia początkowej wartości właściwości publicznej w klasie podstawowej za pomocą dyrektywy @ Page.</span><span class="sxs-lookup"><span data-stu-id="3217c-199">The video below is a walkthrough of setting the initial value of a public property in a base class using the @ Page directive.</span></span>


![](the-asp-net-2-0-page-model/_static/image1.png)


[<span data-ttu-id="3217c-200">Otwórz wideo na pełnym ekranie</span><span class="sxs-lookup"><span data-stu-id="3217c-200">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a><span data-ttu-id="3217c-201">Nowe właściwości publicznej klasy strony</span><span class="sxs-lookup"><span data-stu-id="3217c-201">New Public Properties of the Page Class</span></span>

<span data-ttu-id="3217c-202">Następujące właściwości publiczne są nowe w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="3217c-202">The following public properties are new in ASP.NET 2.0.</span></span>

## <a name="apprelativetemplatesourcedirectory"></a><span data-ttu-id="3217c-203">AppRelativeTemplateSourceDirectory</span><span class="sxs-lookup"><span data-stu-id="3217c-203">AppRelativeTemplateSourceDirectory</span></span>

<span data-ttu-id="3217c-204">Zwraca ścieżkę względem aplikacji do strona lub kontrolka.</span><span class="sxs-lookup"><span data-stu-id="3217c-204">Returns the application-relative path to the page or control.</span></span> <span data-ttu-id="3217c-205">Na przykład dla strony znajduje się w http://app/folder/page.aspx, zwraca właściwość ~ / folderu.</span><span class="sxs-lookup"><span data-stu-id="3217c-205">For example, for a page located at http://app/folder/page.aspx, the property returns ~/folder/.</span></span>

## <a name="apprelativevirtualpath"></a><span data-ttu-id="3217c-206">AppRelativeVirtualPath</span><span class="sxs-lookup"><span data-stu-id="3217c-206">AppRelativeVirtualPath</span></span>

<span data-ttu-id="3217c-207">Zwraca ścieżka względna katalogu wirtualnego do strony lub formantu.</span><span class="sxs-lookup"><span data-stu-id="3217c-207">Returns the relative virtual directory path to the page or control.</span></span> <span data-ttu-id="3217c-208">Na przykład dla strony znajduje się w http://app/folder/page.aspx, zwraca właściwość ~ / folder/page.aspx.</span><span class="sxs-lookup"><span data-stu-id="3217c-208">For example for a page located at http://app/folder/page.aspx, the property returns ~/folder/page.aspx.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="3217c-209">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="3217c-209">AsyncTimeout</span></span>

<span data-ttu-id="3217c-210">Pobiera lub ustawia limit czasu używany do obsługi asynchroniczne strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-210">Gets or sets the timeout used for asynchronous page handling.</span></span> <span data-ttu-id="3217c-211">(Asynchroniczne strony zostaną uwzględnione w dalszej części tego modułu.)</span><span class="sxs-lookup"><span data-stu-id="3217c-211">(Asynchronous pages will be covered later in this module.)</span></span>

## <a name="clientquerystring"></a><span data-ttu-id="3217c-212">ClientQueryString</span><span class="sxs-lookup"><span data-stu-id="3217c-212">ClientQueryString</span></span>

<span data-ttu-id="3217c-213">Właściwość tylko do odczytu, która zwraca część ciągu zapytania żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="3217c-213">A read-only property that returns the query string portion of the requested URL.</span></span> <span data-ttu-id="3217c-214">Ta wartość jest zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="3217c-214">This value is URL encoded.</span></span> <span data-ttu-id="3217c-215">Metoda UrlDecode klasy HttpServerUtility umożliwia zdekodować.</span><span class="sxs-lookup"><span data-stu-id="3217c-215">You can use the UrlDecode method of the HttpServerUtility class to decode it.</span></span>

## <a name="clientscript"></a><span data-ttu-id="3217c-216">ClientScript</span><span class="sxs-lookup"><span data-stu-id="3217c-216">ClientScript</span></span>

<span data-ttu-id="3217c-217">Ta właściwość zwraca obiekt ClientScriptManager, który może służyć do zarządzania emisji ASP.NETs skryptu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3217c-217">This property returns a ClientScriptManager object that can be used to manage ASP.NETs emission of client-side script.</span></span> <span data-ttu-id="3217c-218">(Klasa ClientScriptManager jest uwzględnione w dalszej części tego modułu).</span><span class="sxs-lookup"><span data-stu-id="3217c-218">(The ClientScriptManager class is covered later in this module.)</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="3217c-219">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="3217c-219">EnableEventValidation</span></span>

<span data-ttu-id="3217c-220">Ta właściwość określa, czy sprawdzanie poprawności zdarzenia jest włączony dla zdarzeń ogłaszania zwrotnego i wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="3217c-220">This property controls whether or not event validation is enabled for postback and callback events.</span></span> <span data-ttu-id="3217c-221">Po włączeniu argumentów odświeżania lub wywołania zwrotnego zdarzenia są weryfikowane i upewnij się, że pochodzą z formantu serwera, który pierwotnie je odwzorował.</span><span class="sxs-lookup"><span data-stu-id="3217c-221">When enabled, arguments to postback or callback events are verified to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="3217c-222">Właściwość EnableTheming</span><span class="sxs-lookup"><span data-stu-id="3217c-222">EnableTheming</span></span>

<span data-ttu-id="3217c-223">Ta właściwość pobiera lub ustawia wartość logiczna określająca, czy motyw programu ASP.NET 2.0 jest stosowany do strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-223">This property gets or sets a Boolean that specifies whether or not an ASP.NET 2.0 theme applies to the page.</span></span>

## <a name="form"></a><span data-ttu-id="3217c-224">Formularz</span><span class="sxs-lookup"><span data-stu-id="3217c-224">Form</span></span>

<span data-ttu-id="3217c-225">Ta właściwość zwraca formularza HTML na stronie ASPX jako obiekt HtmlForm.</span><span class="sxs-lookup"><span data-stu-id="3217c-225">This property returns the HTML form on the ASPX page as an HtmlForm object.</span></span>

## <a name="header"></a><span data-ttu-id="3217c-226">nagłówek</span><span class="sxs-lookup"><span data-stu-id="3217c-226">Header</span></span>

<span data-ttu-id="3217c-227">Ta właściwość zwraca odwołanie do obiektu HtmlHead, który zawiera nagłówek strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-227">This property returns a reference to an HtmlHead object that contains the page header.</span></span> <span data-ttu-id="3217c-228">Zwrócony obiekt HtmlHead służy do pobierania/ustawiania arkusze stylów, tagi Meta itp.</span><span class="sxs-lookup"><span data-stu-id="3217c-228">You can use the returned HtmlHead object to get/set style sheets, Meta tags, etc.</span></span>

## <a name="idseparator"></a><span data-ttu-id="3217c-229">IdSeparator</span><span class="sxs-lookup"><span data-stu-id="3217c-229">IdSeparator</span></span>

<span data-ttu-id="3217c-230">Ta właściwość tylko do odczytu pobiera znak, który jest używany do oddzielania identyfikatory kontroli, gdy program ASP.NET tworzy unikatowy identyfikator dla formantów na stronie.</span><span class="sxs-lookup"><span data-stu-id="3217c-230">This read-only property gets the character that is used to separate control identifiers when ASP.NET is building a unique ID for controls on a page.</span></span> <span data-ttu-id="3217c-231">Nie jest on przeznaczony do użycia bezpośrednio w kodzie.</span><span class="sxs-lookup"><span data-stu-id="3217c-231">It is not intended to be used directly from your code.</span></span>

## <a name="isasync"></a><span data-ttu-id="3217c-232">IsAsync</span><span class="sxs-lookup"><span data-stu-id="3217c-232">IsAsync</span></span>

<span data-ttu-id="3217c-233">Ta właściwość umożliwia asynchroniczne stron.</span><span class="sxs-lookup"><span data-stu-id="3217c-233">This property allows for asynchronous pages.</span></span> <span data-ttu-id="3217c-234">Asynchroniczne stron zostały omówione w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="3217c-234">Asynchronous pages are discussed later in this module.</span></span>

## <a name="iscallback"></a><span data-ttu-id="3217c-235">IsCallback</span><span class="sxs-lookup"><span data-stu-id="3217c-235">IsCallback</span></span>

<span data-ttu-id="3217c-236">Ta właściwość tylko do odczytu zwraca **true** Jeśli strona jest wynikiem wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="3217c-236">This read-only property returns **true** if the page is the result of a call back.</span></span> <span data-ttu-id="3217c-237">Wywołań zwrotnych zostały omówione w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="3217c-237">Call backs are discussed later in this module.</span></span>

## <a name="iscrosspagepostback"></a><span data-ttu-id="3217c-238">IsCrossPagePostBack</span><span class="sxs-lookup"><span data-stu-id="3217c-238">IsCrossPagePostBack</span></span>

<span data-ttu-id="3217c-239">Ta właściwość tylko do odczytu zwraca **true** Jeśli strona jest częścią odświeżania strony między stronami.</span><span class="sxs-lookup"><span data-stu-id="3217c-239">This read-only property returns **true** if the page is part of a cross-page postback.</span></span> <span data-ttu-id="3217c-240">Między stronami ogłaszania zwrotnego są uwzględnione w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="3217c-240">Cross-page postbacks are covered later in this module.</span></span>

## <a name="items"></a><span data-ttu-id="3217c-241">Elementy</span><span class="sxs-lookup"><span data-stu-id="3217c-241">Items</span></span>

<span data-ttu-id="3217c-242">Zwraca odwołanie do wystąpienia IDictionary, który zawiera wszystkie obiekty przechowywane w kontekście strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-242">Returns a reference to an IDictionary instance that contains all objects stored in the pages context.</span></span> <span data-ttu-id="3217c-243">Można dodać elementy do tego obiektu IDictionary i będą one dostępne w okresie istnienia w kontekście.</span><span class="sxs-lookup"><span data-stu-id="3217c-243">You can add items to this IDictionary object and they will be available to you throughout the lifetime of the context.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="3217c-244">MaintainScrollPositionOnPostBack</span><span class="sxs-lookup"><span data-stu-id="3217c-244">MaintainScrollPositionOnPostBack</span></span>

<span data-ttu-id="3217c-245">Ta właściwość określa, czy program ASP.NET emituje JavaScript, który utrzymuje się, że strony przewiń pozycji w przeglądarce po wystąpieniu odświeżania strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-245">This property controls whether or not ASP.NET emits JavaScript that maintains the pages scroll position in the browser after a postback occurs.</span></span> <span data-ttu-id="3217c-246">(Szczegóły tej właściwości zostały opisanych wcześniej w tym module).</span><span class="sxs-lookup"><span data-stu-id="3217c-246">(Details of this property were discussed earlier in this module.)</span></span>

## <a name="master"></a><span data-ttu-id="3217c-247">wzorzec</span><span class="sxs-lookup"><span data-stu-id="3217c-247">Master</span></span>

<span data-ttu-id="3217c-248">Ta właściwość tylko do odczytu zwraca odwołanie do wystąpienia formantu MasterPage dla strony, do którego zastosowano strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="3217c-248">This read-only property returns a reference to the MasterPage instance for a page to which a master page has been applied.</span></span>

## <a name="masterpagefile"></a><span data-ttu-id="3217c-249">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="3217c-249">MasterPageFile</span></span>

<span data-ttu-id="3217c-250">Pobiera lub ustawia nazwę pliku strony wzorcowej dla strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-250">Gets or sets the master page filename for the page.</span></span> <span data-ttu-id="3217c-251">Tej właściwości można ustawić tylko w metodzie PreInit.</span><span class="sxs-lookup"><span data-stu-id="3217c-251">This property can only be set in the PreInit method.</span></span>

## <a name="maxpagestatefieldlength"></a><span data-ttu-id="3217c-252">MaxPageStateFieldLength</span><span class="sxs-lookup"><span data-stu-id="3217c-252">MaxPageStateFieldLength</span></span>

<span data-ttu-id="3217c-253">Ta właściwość pobiera lub ustawia maksymalną długość stanu stron w bajtach.</span><span class="sxs-lookup"><span data-stu-id="3217c-253">This property gets or sets the maximum length for the pages state in bytes.</span></span> <span data-ttu-id="3217c-254">Jeśli właściwość jest ustawiona na wartość dodatnią, stan widoku strony będzie być dzielone na wiele ukryte pola tak, aby nie przekraczała liczba bajtów określona.</span><span class="sxs-lookup"><span data-stu-id="3217c-254">If the property is set to a positive number, the pages view state will be broken up into multiple hidden fields so that it doesnt exceed the number of bytes specified.</span></span> <span data-ttu-id="3217c-255">Jeśli właściwość jest liczbą ujemną, stan widoku nie będzie dzielony na fragmenty.</span><span class="sxs-lookup"><span data-stu-id="3217c-255">If the property is a negative number, the view state will not be broken into chunks.</span></span>

## <a name="pageadapter"></a><span data-ttu-id="3217c-256">PageAdapter</span><span class="sxs-lookup"><span data-stu-id="3217c-256">PageAdapter</span></span>

<span data-ttu-id="3217c-257">Zwraca odwołanie do obiektu PageAdapter, który modyfikuje strony do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3217c-257">Returns a reference to the PageAdapter object that modifies the page for the requesting browser.</span></span>

## <a name="previouspage"></a><span data-ttu-id="3217c-258">PreviousPage</span><span class="sxs-lookup"><span data-stu-id="3217c-258">PreviousPage</span></span>

<span data-ttu-id="3217c-259">Zwraca odwołanie do poprzedniej strony w przypadku metody Server.Transfer lub odświeżenie strony między stronami.</span><span class="sxs-lookup"><span data-stu-id="3217c-259">Returns a reference to the previous page in cases of a Server.Transfer or a cross-page postback.</span></span>

## <a name="skinid"></a><span data-ttu-id="3217c-260">SkinID</span><span class="sxs-lookup"><span data-stu-id="3217c-260">SkinID</span></span>

<span data-ttu-id="3217c-261">Określa karnacji ASP.NET 2.0 do zastosowania do strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-261">Specifies the ASP.NET 2.0 skin to apply to the page.</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="3217c-262">Element StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="3217c-262">StyleSheetTheme</span></span>

<span data-ttu-id="3217c-263">Ta właściwość pobiera lub ustawia arkusza stylów, który jest stosowany do strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-263">This property gets or sets the style sheet that is applied to a page.</span></span>

## <a name="templatecontrol"></a><span data-ttu-id="3217c-264">TemplateControl</span><span class="sxs-lookup"><span data-stu-id="3217c-264">TemplateControl</span></span>

<span data-ttu-id="3217c-265">Zwraca odwołanie do formantu zawierającego strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-265">Returns a reference to the containing control for the page.</span></span>

## <a name="theme"></a><span data-ttu-id="3217c-266">Motyw</span><span class="sxs-lookup"><span data-stu-id="3217c-266">Theme</span></span>

<span data-ttu-id="3217c-267">Pobiera lub ustawia nazwę motywu ASP.NET 2.0 zastosowana do strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-267">Gets or sets the name of the ASP.NET 2.0 theme applied to the page.</span></span> <span data-ttu-id="3217c-268">Ta wartość musi być ustawiony przed metodą PreInit.</span><span class="sxs-lookup"><span data-stu-id="3217c-268">This value must be set prior to the PreInit method.</span></span>

## <a name="title"></a><span data-ttu-id="3217c-269">Tytuł</span><span class="sxs-lookup"><span data-stu-id="3217c-269">Title</span></span>

<span data-ttu-id="3217c-270">Ta właściwość pobiera lub ustawia tytuł strony uzyskany w nagłówku strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-270">This property gets or sets the title for the page as obtained from the pages header.</span></span>

## <a name="viewstateencryptionmode"></a><span data-ttu-id="3217c-271">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="3217c-271">ViewStateEncryptionMode</span></span>

<span data-ttu-id="3217c-272">Pobiera lub ustawia ViewStateEncryptionMode strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-272">Gets or sets the ViewStateEncryptionMode of the page.</span></span> <span data-ttu-id="3217c-273">Zobacz szczegółowe omówienie tej właściwości wcześniej w tym module.</span><span class="sxs-lookup"><span data-stu-id="3217c-273">See a detailed discussion of this property earlier in this module.</span></span>

## <a name="new-protected-properties-of-the-page-class"></a><span data-ttu-id="3217c-274">Nowe właściwości chronionego klasy strony</span><span class="sxs-lookup"><span data-stu-id="3217c-274">New Protected Properties of the Page Class</span></span>

<span data-ttu-id="3217c-275">Poniżej przedstawiono nowe właściwości chronionego klasy strony w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="3217c-275">The following are the new protected properties of the Page class in ASP.NET 2.0.</span></span>

## <a name="adapter"></a><span data-ttu-id="3217c-276">Adapter</span><span class="sxs-lookup"><span data-stu-id="3217c-276">Adapter</span></span>

<span data-ttu-id="3217c-277">Zwraca odwołanie do ControlAdapter, renderujący stronę na urządzeniu, które go zażądało.</span><span class="sxs-lookup"><span data-stu-id="3217c-277">Returns a reference to the ControlAdapter that renders the page on the device that requested it.</span></span>

## <a name="asyncmode"></a><span data-ttu-id="3217c-278">AsyncMode</span><span class="sxs-lookup"><span data-stu-id="3217c-278">AsyncMode</span></span>

<span data-ttu-id="3217c-279">Ta właściwość wskazuje, czy strona jest wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="3217c-279">This property indicates whether or not the page is processed asynchronously.</span></span> <span data-ttu-id="3217c-280">Służy do użytku przez środowisko uruchomieniowe i nie są bezpośrednio w kodzie.</span><span class="sxs-lookup"><span data-stu-id="3217c-280">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="clientidseparator"></a><span data-ttu-id="3217c-281">ClientIDSeparator</span><span class="sxs-lookup"><span data-stu-id="3217c-281">ClientIDSeparator</span></span>

<span data-ttu-id="3217c-282">Ta właściwość zwraca znak używany jako separator podczas tworzenia unikatowych klientów identyfikatorów dla formantów.</span><span class="sxs-lookup"><span data-stu-id="3217c-282">This property returns the character used as a separator when creating unique client IDs for controls.</span></span> <span data-ttu-id="3217c-283">Służy do użytku przez środowisko uruchomieniowe i nie są bezpośrednio w kodzie.</span><span class="sxs-lookup"><span data-stu-id="3217c-283">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="pagestatepersister"></a><span data-ttu-id="3217c-284">PageStatePersister</span><span class="sxs-lookup"><span data-stu-id="3217c-284">PageStatePersister</span></span>

<span data-ttu-id="3217c-285">Ta właściwość zwraca obiekt PageStatePersister dla strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-285">This property returns the PageStatePersister object for the page.</span></span> <span data-ttu-id="3217c-286">Ta właściwość jest używana głównie przez deweloperów platformy ASP.NET formantu.</span><span class="sxs-lookup"><span data-stu-id="3217c-286">This property is primarily used by ASP.NET control developers.</span></span>

## <a name="uniquefilepathsuffix"></a><span data-ttu-id="3217c-287">UniqueFilePathSuffix</span><span class="sxs-lookup"><span data-stu-id="3217c-287">UniqueFilePathSuffix</span></span>

<span data-ttu-id="3217c-288">Ta właściwość Zwraca unikatowy suffic, dołączany do ścieżki pliku dla pamięci podręcznej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3217c-288">This property returns a unique suffic that is appended to the file path for caching browsers.</span></span> <span data-ttu-id="3217c-289">Wartość domyślna to \_ \_ufps = i 6-cyfrowy numer.</span><span class="sxs-lookup"><span data-stu-id="3217c-289">The default value is \_\_ufps= and a 6-digit number.</span></span>

## <a name="new-public-methods-for-the-page-class"></a><span data-ttu-id="3217c-290">Nowych metod publicznych klasy strony</span><span class="sxs-lookup"><span data-stu-id="3217c-290">New Public Methods for the Page Class</span></span>

<span data-ttu-id="3217c-291">Następujących metod publicznych dopiero zaczynasz korzystać z klasy strony w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="3217c-291">The following public methods are new to the Page class in ASP.NET 2.0.</span></span>

## <a name="addonprerendercompleteasync"></a><span data-ttu-id="3217c-292">AddOnPreRenderCompleteAsync</span><span class="sxs-lookup"><span data-stu-id="3217c-292">AddOnPreRenderCompleteAsync</span></span>

<span data-ttu-id="3217c-293">Ta metoda rejestruje delegatów obsługi zdarzeń do wykonywania asynchronicznych strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-293">This method registers event handler delegates for asynchronous page execution.</span></span> <span data-ttu-id="3217c-294">Asynchroniczne stron zostały omówione w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="3217c-294">Asynchronous pages are discussed later in this module.</span></span>

## <a name="applystylesheetskin"></a><span data-ttu-id="3217c-295">ApplyStyleSheetSkin</span><span class="sxs-lookup"><span data-stu-id="3217c-295">ApplyStyleSheetSkin</span></span>

<span data-ttu-id="3217c-296">Zastosowanie właściwości w arkuszu stylów strony do strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-296">Applies the properties in a pages style sheet to the page.</span></span>

## <a name="executeregisteredasynctasks"></a><span data-ttu-id="3217c-297">ExecuteRegisteredAsyncTasks</span><span class="sxs-lookup"><span data-stu-id="3217c-297">ExecuteRegisteredAsyncTasks</span></span>

<span data-ttu-id="3217c-298">Ta metoda istot zadanie asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="3217c-298">This method beings an asynchronous task.</span></span>

### <a name="getvalidators"></a><span data-ttu-id="3217c-299">GetValidators</span><span class="sxs-lookup"><span data-stu-id="3217c-299">GetValidators</span></span>

<span data-ttu-id="3217c-300">Zwraca kolekcję moduły weryfikacji dla grupy sprawdzania poprawności określonego lub domyślnego sprawdzania poprawności, jeśli nie jest określona.</span><span class="sxs-lookup"><span data-stu-id="3217c-300">Returns a collection of validators for the specified validation group or the default validation group if none is specified.</span></span>

## <a name="registerasynctask"></a><span data-ttu-id="3217c-301">RegisterAsyncTask</span><span class="sxs-lookup"><span data-stu-id="3217c-301">RegisterAsyncTask</span></span>

<span data-ttu-id="3217c-302">Ta metoda rejestruje nowe zadanie asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="3217c-302">This method registers a new async task.</span></span> <span data-ttu-id="3217c-303">Asynchroniczne strony są uwzględnione w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="3217c-303">Asynchronous pages are covered later in this module.</span></span>

## <a name="registerrequirescontrolstate"></a><span data-ttu-id="3217c-304">RegisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="3217c-304">RegisterRequiresControlState</span></span>

<span data-ttu-id="3217c-305">Ta metoda określa, że ASP.NET musi zostać utrwalony stanu kontroli stron.</span><span class="sxs-lookup"><span data-stu-id="3217c-305">This method tells ASP.NET that the pages control state must be persisted.</span></span>

## <a name="registerrequiresviewstateencryption"></a><span data-ttu-id="3217c-306">RegisterRequiresViewStateEncryption</span><span class="sxs-lookup"><span data-stu-id="3217c-306">RegisterRequiresViewStateEncryption</span></span>

<span data-ttu-id="3217c-307">Ta metoda informuje ASP.NET, czy stan wyświetlania stron wymaga szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="3217c-307">This method tells ASP.NET that the pages viewstate requires encryption.</span></span>

## <a name="resolveclienturl"></a><span data-ttu-id="3217c-308">ResolveClientUrl</span><span class="sxs-lookup"><span data-stu-id="3217c-308">ResolveClientUrl</span></span>

<span data-ttu-id="3217c-309">Zwraca względny adres URL, który może służyć do obsługi żądań klientów dla obrazów itd.</span><span class="sxs-lookup"><span data-stu-id="3217c-309">Returns a relative URL that can be used for client requests for images, etc.</span></span>

## <a name="setfocus"></a><span data-ttu-id="3217c-310">Funkcja SetFocus</span><span class="sxs-lookup"><span data-stu-id="3217c-310">SetFocus</span></span>

<span data-ttu-id="3217c-311">Ta metoda ustawi fokus do formantu, który jest określony, podczas ładowania strony do.</span><span class="sxs-lookup"><span data-stu-id="3217c-311">This method will set the focus to the control that is specified when the page is initially loaded.</span></span>

## <a name="unregisterrequirescontrolstate"></a><span data-ttu-id="3217c-312">UnregisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="3217c-312">UnregisterRequiresControlState</span></span>

<span data-ttu-id="3217c-313">Ta metoda wyrejestruje formant, który jest przekazywany do niego jako już wymagające trwałość stanu formantu.</span><span class="sxs-lookup"><span data-stu-id="3217c-313">This method will unregister the control that is passed to it as no longer requiring control state persistence.</span></span>

## <a name="changes-to-the-page-lifecycle"></a><span data-ttu-id="3217c-314">Zmiany w cyklu życia strony</span><span class="sxs-lookup"><span data-stu-id="3217c-314">Changes to the Page Lifecycle</span></span>

<span data-ttu-id="3217c-315">Cyklu życia strony w programie ASP.NET 2.0 nie zmieniła się znacznie, ale istnieją pewne nowych metod, które należy zwrócić uwagę.</span><span class="sxs-lookup"><span data-stu-id="3217c-315">The page lifecycle in ASP.NET 2.0 hasnt changed dramatically, but there are some new methods that you should be aware of.</span></span> <span data-ttu-id="3217c-316">Cyklu życia strony ASP.NET 2.0 jest opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="3217c-316">The ASP.NET 2.0 page lifecycle is outlined below.</span></span>

## <a name="preinit-new-in-aspnet-20"></a><span data-ttu-id="3217c-317">PreInit (Nowość w programie ASP.NET 2.0)</span><span class="sxs-lookup"><span data-stu-id="3217c-317">PreInit (New in ASP.NET 2.0)</span></span>

<span data-ttu-id="3217c-318">Zdarzenie PreInit jest najwcześniejszym etapie w cyklu życia, w których deweloper może uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="3217c-318">The PreInit event is the earliest stage in the lifecycle that a developer can access.</span></span> <span data-ttu-id="3217c-319">Dodanie to zdarzenie umożliwia programowo zmienić kompozycji ASP.NET 2.0, stron wzorcowych, dostępu do właściwości profilu platformy ASP.NET 2.0 itp. Jeśli jesteś w stanie odświeżania strony, jego zdawać sobie sprawę, że stan widoku nie ma jeszcze zastosowane do formantów w tym momencie w cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="3217c-319">The addition of this event makes it possible to programmatically change ASP.NET 2.0 themes, master pages, access properties for an ASP.NET 2.0 profile, etc. If you are in a postback state, its important to realize that Viewstate has not yet been applied to controls at this point in the lifecycle.</span></span> <span data-ttu-id="3217c-320">W związku z tym jeśli dewelopera właściwości formantu zmieni się na tym etapie, zostanie prawdopodobnie on zastąpiony później w cyklu życia strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-320">Therefore, if a developer changes a property of a control at this stage, it will likely be overwritten later in the pages lifecycle.</span></span>

## <a name="init"></a><span data-ttu-id="3217c-321">Init</span><span class="sxs-lookup"><span data-stu-id="3217c-321">Init</span></span>

<span data-ttu-id="3217c-322">Zdarzeniu Init nie zmienił się z ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="3217c-322">The Init event has not changed from ASP.NET 1.x.</span></span> <span data-ttu-id="3217c-323">Jest to miejscu do odczytu lub zainicjować właściwości formantów na stronie.</span><span class="sxs-lookup"><span data-stu-id="3217c-323">This is where you would want to read or initialize properties of controls on your page.</span></span> <span data-ttu-id="3217c-324">Ten etap, stron wzorcowych, kompozycje itp. już są stosowane do strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-324">At this stage, master pages, themes, etc. are already applied to the page.</span></span>

## <a name="initcomplete-new-in-20"></a><span data-ttu-id="3217c-325">InitComplete (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="3217c-325">InitComplete (New in 2.0)</span></span>

<span data-ttu-id="3217c-326">Zdarzenie InitComplete jest wywoływane po zakończeniu etapu inicjowania strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-326">The InitComplete event is called at the end of the pages initialization stage.</span></span> <span data-ttu-id="3217c-327">W tym momencie cały cykl życia, dostępne kontrolki na stronie, ale nie została jeszcze wypełniona ich stan.</span><span class="sxs-lookup"><span data-stu-id="3217c-327">At this point in the lifecycle, you can access controls on the page, but their state has not yet been populated.</span></span>

## <a name="preload-new-in-20"></a><span data-ttu-id="3217c-328">Wstępne ładowanie (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="3217c-328">PreLoad (New in 2.0)</span></span>

<span data-ttu-id="3217c-329">To zdarzenie jest wywoływane po zastosowaniu wszystkich danych odświeżania strony i wcześniejszy niż strony\_obciążenia.</span><span class="sxs-lookup"><span data-stu-id="3217c-329">This event is called after all postback data has been applied and just prior to Page\_Load.</span></span>

## <a name="load"></a><span data-ttu-id="3217c-330">Ładowanie</span><span class="sxs-lookup"><span data-stu-id="3217c-330">Load</span></span>

<span data-ttu-id="3217c-331">Zdarzenie obciążenia nie zmienił się z ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="3217c-331">The Load event has not changed from ASP.NET 1.x.</span></span>

## <a name="loadcomplete-new-in-20"></a><span data-ttu-id="3217c-332">LoadComplete (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="3217c-332">LoadComplete (New in 2.0)</span></span>

<span data-ttu-id="3217c-333">Zdarzenie LoadComplete jest ostatnie zdarzenie w fazie ładowania stron.</span><span class="sxs-lookup"><span data-stu-id="3217c-333">The LoadComplete event is the last event in the pages load stage.</span></span> <span data-ttu-id="3217c-334">Na tym etapie wszystkie dane ogłaszania zwrotnego i stanu wyświetlania zostały zastosowane do strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-334">At this stage, all postback and viewstate data has been applied to the page.</span></span>

## <a name="prerender"></a><span data-ttu-id="3217c-335">PreRender</span><span class="sxs-lookup"><span data-stu-id="3217c-335">PreRender</span></span>

<span data-ttu-id="3217c-336">Jeśli chcesz viewstate prawidłowo utrzymania dla formantów, które są dodawane dynamicznie do strony, zdarzenie PreRender jest to ostatnia możliwość je dodać.</span><span class="sxs-lookup"><span data-stu-id="3217c-336">If you would like for viewstate to be properly maintained for controls that are added to the page dynamically, the PreRender event is the last opportunity to add them.</span></span>

## <a name="prerendercomplete-new-in-20"></a><span data-ttu-id="3217c-337">PreRenderComplete (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="3217c-337">PreRenderComplete (New in 2.0)</span></span>

<span data-ttu-id="3217c-338">Na etapie PreRenderComplete zostały dodane wszystkie formanty do strony i jest gotowy do renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-338">At the PreRenderComplete stage, all controls have been added to the page and the page is ready to be rendered.</span></span> <span data-ttu-id="3217c-339">Zdarzenie PreRenderComplete jest ostatnie zdarzenie wywoływane przed zapisaniem stanu widoku strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-339">The PreRenderComplete event is the last event raised before the pages viewstate is saved.</span></span>

## <a name="savestatecomplete-new-in-20"></a><span data-ttu-id="3217c-340">SaveStateComplete (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="3217c-340">SaveStateComplete (New in 2.0)</span></span>

<span data-ttu-id="3217c-341">Zdarzenie SaveStateComplete jest wywoływana natychmiast po zapisaniu wszystkie strony viewstate i kontroli stanu.</span><span class="sxs-lookup"><span data-stu-id="3217c-341">The SaveStateComplete event is called immediately after all page viewstate and control state has been saved.</span></span> <span data-ttu-id="3217c-342">Jest to ostatnie zdarzenie przed wyświetleniem strony faktycznie w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="3217c-342">This is the last event before the page is actually rendered to the browser.</span></span>

## <a name="render"></a><span data-ttu-id="3217c-343">Renderowanie</span><span class="sxs-lookup"><span data-stu-id="3217c-343">Render</span></span>

<span data-ttu-id="3217c-344">Metoda renderowania nie zmienił się od ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="3217c-344">The Render method has not changed since ASP.NET 1.x.</span></span> <span data-ttu-id="3217c-345">Jest to której zainicjowano HtmlTextWriter i realizacją strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="3217c-345">This is where the HtmlTextWriter is initialized and the page is rendered to the browser.</span></span>

## <a name="cross-page-postback-in-aspnet-20"></a><span data-ttu-id="3217c-346">Ogłaszania zwrotnego między stronami w programie ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="3217c-346">Cross-Page Postback in ASP.NET 2.0</span></span>

<span data-ttu-id="3217c-347">W programie ASP.NET 1.x, ogłaszania zwrotnego były wymagane do wysłania do tej samej stronie.</span><span class="sxs-lookup"><span data-stu-id="3217c-347">In ASP.NET 1.x, postbacks were required to post to the same page.</span></span> <span data-ttu-id="3217c-348">Między stronami ogłaszania zwrotnego są niedozwolone.</span><span class="sxs-lookup"><span data-stu-id="3217c-348">Cross-page postbacks were not allowed.</span></span> <span data-ttu-id="3217c-349">Platforma ASP.NET 2.0 dodaje możliwość ogłosić innej strony za pomocą interfejsu IButtonControl.</span><span class="sxs-lookup"><span data-stu-id="3217c-349">ASP.NET 2.0 adds the ability to post back to a different page via the IButtonControl interface.</span></span> <span data-ttu-id="3217c-350">Żadnego formantu, który implementuje interfejs IButtonControl nowe (przycisk, LinkButton i ImageButton oprócz innych firm formantów niestandardowych) można korzystać z tej nowej funkcji przez użycie atrybutu PostBackUrl.</span><span class="sxs-lookup"><span data-stu-id="3217c-350">Any control that implements the new IButtonControl interface (Button, LinkButton, and ImageButton in addition to third-party custom controls) can take advantage of this new functionality via the use of the PostBackUrl attribute.</span></span> <span data-ttu-id="3217c-351">Poniższy kod przedstawia kontrolkę przycisku, który dokonuje ogłoszenia drugiej stronie.</span><span class="sxs-lookup"><span data-stu-id="3217c-351">The following code shows a Button control that posts back to a second page.</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

<span data-ttu-id="3217c-352">Jeśli strona jest przesyłana z powrotem, który inicjuje ogłaszania zwrotnego strony jest dostępny za pośrednictwem właściwości PreviousPage na drugiej stronie.</span><span class="sxs-lookup"><span data-stu-id="3217c-352">When the page is posted back, the Page that initiates the postback is accessible via the PreviousPage property on the second page.</span></span> <span data-ttu-id="3217c-353">Ta funkcja jest implementowany za pośrednictwem nowego formularza sieci Web\_DoPostBackWithOptions po stronie klienta funkcji, która renderuje ASP.NET 2.0 do strony, gdy formant dokonuje ogłoszenia innej strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-353">This functionality is implemented via the new WebForm\_DoPostBackWithOptions client-side function that ASP.NET 2.0 renders to the page when a control posts back to a different page.</span></span> <span data-ttu-id="3217c-354">Ta funkcja JavaScript jest dostarczana przez nowy program obsługi WebResource.axd, który emituje skryptu do klienta.</span><span class="sxs-lookup"><span data-stu-id="3217c-354">This JavaScript function is provided by the new WebResource.axd handler which emits script to the client.</span></span>

<span data-ttu-id="3217c-355">Poniższy klip wideo jest przewodnik odświeżania strony między stronami.</span><span class="sxs-lookup"><span data-stu-id="3217c-355">The video below is a walkthrough of a cross-page postback.</span></span>


![](the-asp-net-2-0-page-model/_static/image2.png)


[<span data-ttu-id="3217c-356">Otwórz wideo na pełnym ekranie</span><span class="sxs-lookup"><span data-stu-id="3217c-356">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a><span data-ttu-id="3217c-357">Więcej informacji na temat ogłaszania zwrotnego strony między</span><span class="sxs-lookup"><span data-stu-id="3217c-357">More Details on Cross-Page Postbacks</span></span>

### <a name="viewstate"></a><span data-ttu-id="3217c-358">Stan widoku</span><span class="sxs-lookup"><span data-stu-id="3217c-358">Viewstate</span></span>

<span data-ttu-id="3217c-359">Może zażądano samodzielnie już o co stanie się stan od pierwszej strony w scenariuszu odświeżania strony między stronami.</span><span class="sxs-lookup"><span data-stu-id="3217c-359">You may have asked yourself already about what happens to the viewstate from the first page in a cross-page postback scenario.</span></span> <span data-ttu-id="3217c-360">Po wszystkich żadnego formantu, który nie implementuje element IPostBackDataHandler pozostanie stanu za pośrednictwem elementu viewstate, więc dostęp do właściwości tego formantu na drugiej stronie odświeżania strony między stronami, musisz mieć dostęp do stanu widoku strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-360">After all, any control that does not implement IPostBackDataHandler will persist its state via viewstate, so to have access to the properties of that control on the second page of a cross-page postback, you must have access to the viewstate for the page.</span></span> <span data-ttu-id="3217c-361">Platforma ASP.NET 2.0 zajmuje się ten scenariusz przy użyciu nowego pola ukryte na drugiej stronie o nazwie \_ \_PREVIOUSPAGE.</span><span class="sxs-lookup"><span data-stu-id="3217c-361">ASP.NET 2.0 takes care of this scenario using a new hidden field in the second page called \_\_PREVIOUSPAGE.</span></span> <span data-ttu-id="3217c-362">\_ \_PREVIOUSPAGE pole formularza zawiera stan dla pierwszej strony, dzięki czemu mają dostęp do właściwości wszystkich kontrolek na drugiej stronie.</span><span class="sxs-lookup"><span data-stu-id="3217c-362">The \_\_PREVIOUSPAGE form field contains the viewstate for the first page so that you can have access to the properties of all controls in the second page.</span></span>

### <a name="circumventing-findcontrol"></a><span data-ttu-id="3217c-363">Obejście metody FindControl</span><span class="sxs-lookup"><span data-stu-id="3217c-363">Circumventing FindControl</span></span>

<span data-ttu-id="3217c-364">Przewodnik wideo odświeżania strony między stronami I użyć metody metody FindControl Aby pobrać odwołanie do formantu TextBox na pierwszej stronie.</span><span class="sxs-lookup"><span data-stu-id="3217c-364">In the video walkthrough of a cross-page postback, I used the FindControl method to get a reference to the TextBox control on the first page.</span></span> <span data-ttu-id="3217c-365">Ta metoda sprawdza się w przypadku tego celu, ale metody FindControl jest kosztowne i wymaga zapisywania dodatkowy kod.</span><span class="sxs-lookup"><span data-stu-id="3217c-365">That method works well for that purpose, but FindControl is expensive and it requires writing additional code.</span></span> <span data-ttu-id="3217c-366">Na szczęście program ASP.NET 2.0 stanowi alternatywę dla metody FindControl dla tego celu, które będą działać w wielu scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="3217c-366">Fortunately, ASP.NET 2.0 provides an alternative to FindControl for this purpose that will work in many scenarios.</span></span> <span data-ttu-id="3217c-367">Dyrektywa PreviousPageType umożliwia zawierają jednoznacznie odwołanie do poprzedniej strony za pomocą TypeName lub VirtualPath atrybutu.</span><span class="sxs-lookup"><span data-stu-id="3217c-367">The PreviousPageType directive allows you to have a strongly-typed reference to the previous page by using either the TypeName or the VirtualPath attribute.</span></span> <span data-ttu-id="3217c-368">Atrybut TypeName umożliwia określenie typu poprzedniej strony, gdy atrybut VirtualPath służy do odwoływania się do poprzedniej strony za pomocą ścieżki wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="3217c-368">The TypeName attribute allows you to specify the type of the previous page while the VirtualPath attribute allows you to refer to the previous page using a virtual path.</span></span> <span data-ttu-id="3217c-369">Po ustawieniu dyrektywy PreviousPageType następnie musi ujawniać formantów, itp., do której chcesz zezwolić na dostęp za pomocą właściwości publiczne.</span><span class="sxs-lookup"><span data-stu-id="3217c-369">After you've set the PreviousPageType directive, you must then expose the controls, etc. to which you want to allow access using public properties.</span></span>

## <a name="lab-1-cross-page-postback"></a><span data-ttu-id="3217c-370">Ogłaszania zwrotnego strony między laboratorium 1</span><span class="sxs-lookup"><span data-stu-id="3217c-370">Lab 1 Cross-Page Postback</span></span>

<span data-ttu-id="3217c-371">W tym laboratorium utworzysz aplikację, która korzysta z nowych funkcji odświeżania strony między stronami ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="3217c-371">In this lab, you will create an application that uses the new cross-page postback functionality of ASP.NET 2.0.</span></span>

1. <span data-ttu-id="3217c-372">Otwórz program Visual Studio 2005 i Utwórz nową witrynę sieci Web programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3217c-372">Open Visual Studio 2005 and create a new ASP.NET Web site.</span></span>
2. <span data-ttu-id="3217c-373">Dodaj nowy formularz sieci Web o nazwie page2.aspx.</span><span class="sxs-lookup"><span data-stu-id="3217c-373">Add a new Webform called page2.aspx.</span></span>
3. <span data-ttu-id="3217c-374">Otwórz plik Default.aspx w widoku projektu i Dodaj kontrolkę przycisku i kontrolki pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="3217c-374">Open the Default.aspx in Design view and add a Button control and a TextBox control.</span></span> 

    1. <span data-ttu-id="3217c-375">Nadaj identyfikator formantu przycisku **SubmitButton** i identyfikator dla formantu pole tekstowe **UserName**.</span><span class="sxs-lookup"><span data-stu-id="3217c-375">Give the Button control an ID of **SubmitButton** and the TextBox control an ID of **UserName**.</span></span>
    2. <span data-ttu-id="3217c-376">Ustaw właściwość PostBackUrl przycisku do page2.aspx.</span><span class="sxs-lookup"><span data-stu-id="3217c-376">Set the PostBackUrl property of the Button to page2.aspx.</span></span>
4. <span data-ttu-id="3217c-377">Otwórz page2.aspx w widoku źródła.</span><span class="sxs-lookup"><span data-stu-id="3217c-377">Open page2.aspx in Source view.</span></span>
5. <span data-ttu-id="3217c-378">Dodaj dyrektywy @ PreviousPageType, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="3217c-378">Add a @ PreviousPageType directive as shown below:</span></span>
6. <span data-ttu-id="3217c-379">Dodaj następujący kod do strony\_obciążenia na page2.aspx kodem:</span><span class="sxs-lookup"><span data-stu-id="3217c-379">Add the following code to the Page\_Load of page2.aspx's code-behind:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. <span data-ttu-id="3217c-380">Skompiluj projekt, klikając w menu kompilacji podczas kompilacji.</span><span class="sxs-lookup"><span data-stu-id="3217c-380">Build the project by clicking on Build on the Build menu.</span></span>
8. <span data-ttu-id="3217c-381">Dodaj następujący kod do kodem Default.aspx:</span><span class="sxs-lookup"><span data-stu-id="3217c-381">Add the following code to the code-behind for Default.aspx:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. <span data-ttu-id="3217c-382">Zmień stronę\_obciążenia w page2.aspx do następującego:</span><span class="sxs-lookup"><span data-stu-id="3217c-382">Change the Page\_Load in page2.aspx to the following:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. <span data-ttu-id="3217c-383">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="3217c-383">Build the project.</span></span>
11. <span data-ttu-id="3217c-384">Uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="3217c-384">Run the project.</span></span>
12. <span data-ttu-id="3217c-385">Wprowadź nazwę w polu tekstowym i kliknij przycisk.</span><span class="sxs-lookup"><span data-stu-id="3217c-385">Enter your name in the TextBox and click the button.</span></span>
13. <span data-ttu-id="3217c-386">Co to jest wynik?</span><span class="sxs-lookup"><span data-stu-id="3217c-386">What is the result?</span></span>

## <a name="asynchronous-pages-in-aspnet-20"></a><span data-ttu-id="3217c-387">Asynchroniczne stron w programie ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="3217c-387">Asynchronous Pages in ASP.NET 2.0</span></span>

<span data-ttu-id="3217c-388">Wiele rywalizacji problemów w programie ASP.NET są spowodowane opóźnienia połączeniami zewnętrznymi (np. wywołań usługi lub bazy danych w sieci Web), opóźnienia we/wy pliku itp. Po wysłaniu żądania w odniesieniu do aplikacji ASP.NET, program ASP.NET korzysta z jednego z jego wątków roboczych do obsłużenia tego żądania.</span><span class="sxs-lookup"><span data-stu-id="3217c-388">Many contention problems in ASP.NET are caused by latency of external calls (such as Web service or database calls), file IO latency, etc. When a request is made against an ASP.NET application, ASP.NET uses one of its worker threads to service that request.</span></span> <span data-ttu-id="3217c-389">To żądanie jest właścicielem tego wątku zakończenie żądanie i odpowiedź została wysłana.</span><span class="sxs-lookup"><span data-stu-id="3217c-389">That request owns that thread until the request is complete and the response has been sent.</span></span> <span data-ttu-id="3217c-390">Platforma ASP.NET 2.0 ma na celu rozwiązanie problemów opóźnienia z tego typu problemów przez dodanie możliwość wykonywania stron asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="3217c-390">ASP.NET 2.0 seeks to resolve latency issues with these types of issues by adding the capability to execute pages asynchronously.</span></span> <span data-ttu-id="3217c-391">Oznacza to, że wątku roboczego można uruchomić żądania i następnie przekazują wykonywania dodatkowych do innego wątku, w tym samym zwracać do puli wątków dostępnych szybko.</span><span class="sxs-lookup"><span data-stu-id="3217c-391">That means that a worker thread can start the request and then hand off additional execution to another thread, thereby returning to the available thread pool quickly.</span></span> <span data-ttu-id="3217c-392">Po ukończeniu operacji We/Wy pliku, wywołania bazy danych itp., nowego wątku są uzyskiwane z puli wątków, aby zakończyć żądania.</span><span class="sxs-lookup"><span data-stu-id="3217c-392">When the file IO, database call, etc. has completed, a new thread is obtained from the thread pool to finish the request.</span></span>

<span data-ttu-id="3217c-393">Pierwszym krokiem w podejmowaniu stronę asynchroniczne jest skonfigurowanie **Async** atrybutu dyrektywy strony w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3217c-393">The first step in making a page execute asynchronously is to set the **Async** attribute of the page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

<span data-ttu-id="3217c-394">Ten atrybut informuje program ASP.NET do zaimplementowania IHttpAsyncHandler dla strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-394">This attribute tells ASP.NET to implement the IHttpAsyncHandler for the page.</span></span>

<span data-ttu-id="3217c-395">Następnym krokiem jest, aby wywołać metodę AddOnPreRenderCompleteAsync w punkcie w cyklu życia strony przed zdarzeniem PreRender.</span><span class="sxs-lookup"><span data-stu-id="3217c-395">The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender.</span></span> <span data-ttu-id="3217c-396">(Ta metoda jest zazwyczaj wywoływana na stronie\_obciążenia.) Metoda AddOnPreRenderCompleteAsync przyjmuje dwa parametry; BeginEventHandler i EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="3217c-396">(This method is typically called in Page\_Load.) The AddOnPreRenderCompleteAsync method takes two parameters; a BeginEventHandler and an EndEventHandler.</span></span> <span data-ttu-id="3217c-397">BeginEventHandler zwraca obiekt IAsyncResult, które są następnie przekazywane jako parametr EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="3217c-397">The BeginEventHandler returns an IAsyncResult which is then passed as a parameter to the EndEventHandler.</span></span>

<span data-ttu-id="3217c-398">Obraz poniżej jest wskazówki żądania asynchroniczne strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-398">The video below is a walkthrough of an asynchronous page request.</span></span>


![](the-asp-net-2-0-page-model/_static/image3.png)


[<span data-ttu-id="3217c-399">Otwórz wideo na pełnym ekranie</span><span class="sxs-lookup"><span data-stu-id="3217c-399">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> <span data-ttu-id="3217c-400">Na stronie asynchronicznej nie renderowania w przeglądarce dopiero po ukończeniu EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="3217c-400">An async page does not render to the browser until the EndEventHandler has completed.</span></span> <span data-ttu-id="3217c-401">Bez wątpienia, ale niektórzy deweloperzy będzie traktować żądań asynchronicznych jako zbliżone do asynchronicznego wywołania zwrotne.</span><span class="sxs-lookup"><span data-stu-id="3217c-401">No doubt but that some developers will think of async requests as being similar to async callbacks.</span></span> <span data-ttu-id="3217c-402">Należy koniecznie należy pamiętać, że nie są one.</span><span class="sxs-lookup"><span data-stu-id="3217c-402">It's important to realize that they are not.</span></span> <span data-ttu-id="3217c-403">Korzyści żądań asynchronicznych jest, czy pierwszy wątku roboczego może być zwracany w puli wątków do obsługi nowych żądań, skracając rywalizacji z powodu jest powiązany we/wy, np.</span><span class="sxs-lookup"><span data-stu-id="3217c-403">The benefit to asynchronous requests is that the first worker thread can be returned to the thread pool to service new requests, thereby reducing contention due to being IO bound, etc.</span></span>


## <a name="script-callbacks-in-aspnet-20"></a><span data-ttu-id="3217c-404">Wywołania zwrotne skryptu w programie ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="3217c-404">Script Callbacks in ASP.NET 2.0</span></span>

<span data-ttu-id="3217c-405">Sposób można zapobiec migotanie skojarzonych z wywołaniem zwrotnym zawsze sprawdzono deweloperów sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3217c-405">Web developers have always looked for ways to prevent the flickering associated with a callback.</span></span> <span data-ttu-id="3217c-406">W programie ASP.NET 1.x, SmartNavigation był najbardziej typowa metoda uniknięcia migotanie, ale SmartNavigation przyczyną problemów dla deweloperów, niektóre ze względu na złożoność jej wdrożenia na kliencie.</span><span class="sxs-lookup"><span data-stu-id="3217c-406">In ASP.NET 1.x, SmartNavigation was the most common method for avoiding flickering, but SmartNavigation caused problems for some developers because of the complexity of its implementation on the client.</span></span> <span data-ttu-id="3217c-407">Platforma ASP.NET 2.0 rozwiązuje ten problem z wywołania zwrotne skryptu.</span><span class="sxs-lookup"><span data-stu-id="3217c-407">ASP.NET 2.0 addresses this issue with script callbacks.</span></span> <span data-ttu-id="3217c-408">Wywołania zwrotne skryptu wykorzystywać XMLHttp na wysyłanie żądań na serwerze sieci Web za pośrednictwem kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3217c-408">Script callbacks utilize XMLHttp to make requests against the Web server via JavaScript.</span></span> <span data-ttu-id="3217c-409">Żądanie XMLHttp zwraca dane XML, który następnie może manipulować za pośrednictwem przeglądarki modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="3217c-409">The XMLHttp request returns XML data that can then be manipulated via the browser's DOM.</span></span> <span data-ttu-id="3217c-410">Kod XMLHttp jest ukryty przez użytkownika przez nowy program obsługi WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="3217c-410">XMLHttp code is hidden from the user by the new WebResource.axd handler.</span></span>

<span data-ttu-id="3217c-411">Istnieje kilka czynności, które są niezbędne w celu skonfigurowania wywołanie zwrotne skryptu w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="3217c-411">There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.</span></span>

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a><span data-ttu-id="3217c-412">Krok 1: Implementowanie interfejsu modułu ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="3217c-412">Step 1 : Implement the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="3217c-413">Aby ASP.NET rozpoznał strony jako należący do wywołania zwrotnego skryptu musi implementować interfejs modułu ICallbackEventHandler.</span><span class="sxs-lookup"><span data-stu-id="3217c-413">In order for ASP.NET to recognize your page as participating in a script callback, you must implement the ICallbackEventHandler interface.</span></span> <span data-ttu-id="3217c-414">Można to zrobić w pliku CodeBehind w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3217c-414">You can do this in your code-behind file like so:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

<span data-ttu-id="3217c-415">Można też to zrobić przy użyciu podobnych dyrektywy @ implementuje tak:</span><span class="sxs-lookup"><span data-stu-id="3217c-415">You can also do this using the @ Implements directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

<span data-ttu-id="3217c-416">Dyrektywy @ Implements można skorzystać zazwyczaj, gdy przy użyciu kodu wbudowanego ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3217c-416">You would typically use the @ Implements directive when using inline ASP.NET code.</span></span>

## <a name="step-2--call-getcallbackeventreference"></a><span data-ttu-id="3217c-417">Krok 2: Wywołanie GetCallbackEventReference</span><span class="sxs-lookup"><span data-stu-id="3217c-417">Step 2 : Call GetCallbackEventReference</span></span>

<span data-ttu-id="3217c-418">Jak wspomniano wcześniej, wywołania XMLHttp jest hermetyzowany obsługi WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="3217c-418">As mentioned previously, the XMLHttp call is encapsulated in the WebResource.axd handler.</span></span> <span data-ttu-id="3217c-419">Podczas renderowania strony ASP.NET będzie Dodaj wywołanie do formularza sieci Web\_DoCallback, skrypt klientów zapewnianej przez WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="3217c-419">When your page is rendered, ASP.NET will add a call to WebForm\_DoCallback, a client script that is provided by WebResource.axd.</span></span> <span data-ttu-id="3217c-420">Formularz sieci Web\_DoCallback funkcja zastępuje \_ \_doPostBack funkcja wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="3217c-420">The WebForm\_DoCallback function replaces the \_\_doPostBack function for a callback.</span></span> <span data-ttu-id="3217c-421">Należy pamiętać, że \_ \_doPostBack programowo przesyła formularz na stronie.</span><span class="sxs-lookup"><span data-stu-id="3217c-421">Remember that \_\_doPostBack programmatically submits the form on the page.</span></span> <span data-ttu-id="3217c-422">W przypadku wywołania zwrotnego, chcesz uniemożliwić odświeżenie strony, dlatego \_ \_doPostBack nie wystarczy.</span><span class="sxs-lookup"><span data-stu-id="3217c-422">In a callback scenario, you want to prevent a postback, so \_\_doPostBack will not suffice.</span></span>

> [!NOTE]
> <span data-ttu-id="3217c-423">\_\_doPostBack jest nadal renderowany do strony w przypadku wywołania zwrotnego skryptu klienta.</span><span class="sxs-lookup"><span data-stu-id="3217c-423">\_\_doPostBack is still rendered to the page in a client script callback scenario.</span></span> <span data-ttu-id="3217c-424">Nie jest on jednak używany dla wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="3217c-424">However, it's not used for the callback.</span></span>


<span data-ttu-id="3217c-425">Argumenty dla formularza sieci Web\_DoCallback funkcji po stronie klienta są udostępniane za pośrednictwem funkcji po stronie serwera GetCallbackEventReference, która będzie wywoływana zwykle na stronie\_obciążenia.</span><span class="sxs-lookup"><span data-stu-id="3217c-425">The arguments for the WebForm\_DoCallback client-side function are provided via the server-side function GetCallbackEventReference which would normally be called in Page\_Load.</span></span> <span data-ttu-id="3217c-426">Typowe wywołania GetCallbackEventReference może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="3217c-426">A typical call to GetCallbackEventReference might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="3217c-427">W takim przypadku cm jest wystąpieniem ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="3217c-427">In this case, cm is an instance of ClientScriptManager.</span></span> <span data-ttu-id="3217c-428">Klasa ClientScriptManager zostanie omówiona w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="3217c-428">The ClientScriptManager class will be covered later in this module.</span></span>


<span data-ttu-id="3217c-429">Istnieje kilka wersji przeciążone GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="3217c-429">There are several overloaded versions of GetCallbackEventReference.</span></span> <span data-ttu-id="3217c-430">W takim przypadku argumenty są następujące:</span><span class="sxs-lookup"><span data-stu-id="3217c-430">In this case, the arguments are as follows:</span></span>

`this`

<span data-ttu-id="3217c-431">Odwołanie do formantu, gdy jest wywoływana GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="3217c-431">A reference to the control where GetCallbackEventReference is being called.</span></span> <span data-ttu-id="3217c-432">W takim przypadku jest samej strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-432">In this case, it's the page itself.</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

<span data-ttu-id="3217c-433">Argument ciągu, który zostanie przekazany z kodu po stronie klienta do zdarzenia po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="3217c-433">A string argument that will be passed from the client-side code to the server-side event.</span></span> <span data-ttu-id="3217c-434">W takim przypadku wiadomości błyskawicznych, przekazując wartość z listy rozwijanej o nazwie ddlCompany.</span><span class="sxs-lookup"><span data-stu-id="3217c-434">In this case, Im passing the value of a dropdown called ddlCompany.</span></span>

`ShowCompanyName`

<span data-ttu-id="3217c-435">Nazwa funkcji po stronie klienta, która przyjmuje wartość zwracaną (jako ciąg) ze zdarzenia wywołania zwrotnego po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="3217c-435">The name of the client-side function that will accept the return value (as string) from the server-side callback event.</span></span> <span data-ttu-id="3217c-436">Tę funkcję można wywołać tylko po pomyślnym zakończeniu operacji wywołania zwrotnego po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="3217c-436">This function will only be called when the server-side callback is successful.</span></span> <span data-ttu-id="3217c-437">W związku z tym dla niezawodności, ogólnie zaleca się użycie przeciążonej wersji GetCallbackEventReference, który przyjmuje argument dodatkowy ciąg określający nazwę funkcji do wykonania w przypadku wystąpienia błędu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3217c-437">Therefore, for the sake of robustness, it is generally recommended to use the overloaded version of GetCallbackEventReference that takes an additional string argument specifying the name of a client-side function to execute in the event of an error.</span></span>

`null`

<span data-ttu-id="3217c-438">Ciąg reprezentujący funkcji po stronie klienta, która zainicjowane przed wywołania zwrotnego na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3217c-438">A string representing a client-side function that it initiated before the callback to the server.</span></span> <span data-ttu-id="3217c-439">W takim przypadku nie żadnego skryptu więc argument ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="3217c-439">In this case, there is no such script, so the argument is null.</span></span>

`true`

<span data-ttu-id="3217c-440">Wartość logiczna określająca, czy należy przeprowadzić asynchronicznie metodę wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="3217c-440">A Boolean specifying whether or not to conduct the callback asynchronously.</span></span>

<span data-ttu-id="3217c-441">Wywołanie formularza sieci Web\_DoCallback na kliencie zostanie przekazany tych argumentów.</span><span class="sxs-lookup"><span data-stu-id="3217c-441">The call to WebForm\_DoCallback on the client will pass these arguments.</span></span> <span data-ttu-id="3217c-442">W związku z tym, gdy ta strona jest renderowany na kliencie, ten kod będzie wyglądać w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3217c-442">Therefore, when this page is rendered on the client, that code will look like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

<span data-ttu-id="3217c-443">Należy zauważyć, że podpis funkcji na kliencie jest nieco inny.</span><span class="sxs-lookup"><span data-stu-id="3217c-443">Notice that the signature of the function on the client is a bit different.</span></span> <span data-ttu-id="3217c-444">Funkcja klienta przekazuje ciągów 5 i wartość logiczną.</span><span class="sxs-lookup"><span data-stu-id="3217c-444">The client-side function passes 5 strings and a Boolean.</span></span> <span data-ttu-id="3217c-445">Dodatkowy ciąg znaków (czyli null w powyższym przykładzie) zawiera funkcję po stronie klienta, która będzie obsługiwać błędy zwrotne po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="3217c-445">The additional string (which is null in the above example) contains the client-side function that will handle any errors from the server-side callback.</span></span>

## <a name="step-3--hook-the-client-side-control-event"></a><span data-ttu-id="3217c-446">Krok 3: Utworzenie punktu zaczepienia zdarzenia formantu po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="3217c-446">Step 3 : Hook the Client-Side Control Event</span></span>

<span data-ttu-id="3217c-447">Należy zauważyć, że wartość zwracana GetCallbackEventReference powyżej został przypisany do zmiennej ciągu.</span><span class="sxs-lookup"><span data-stu-id="3217c-447">Notice that the return value of GetCallbackEventReference above was assigned to a string variable.</span></span> <span data-ttu-id="3217c-448">Ten ciąg jest używany do utworzenie punktu zaczepienia zdarzenie po stronie klienta dla formantu, który inicjuje wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="3217c-448">That string is used to hook a client-side event for the control that initiates the callback.</span></span> <span data-ttu-id="3217c-449">W tym przykładzie, wywołanie zwrotne jest inicjowane przez listy rozwijanej na stronie tak, chcę utworzenie punktu zaczepienia *OnChange* zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="3217c-449">In this example, the callback is initiated by a dropdown on the page, so I want to hook the *OnChange* event.</span></span>

<span data-ttu-id="3217c-450">Aby przyłączyć zdarzeń po stronie klienta, po prostu Dodaj program obsługi do znacznika po stronie klienta w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3217c-450">To hook the client-side event, simply add a handler to the client-side markup as follows:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

<span data-ttu-id="3217c-451">Odwołania, który *cbRef* jest wartość zwrotna z wywołania GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="3217c-451">Recall that *cbRef* is the return value from the call to GetCallbackEventReference.</span></span> <span data-ttu-id="3217c-452">Zawiera wywołanie formularza sieci Web\_DoCallback, który został przedstawionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="3217c-452">It contains the call to WebForm\_DoCallback that was shown above.</span></span>

## <a name="step-4--register-the-client-side-script"></a><span data-ttu-id="3217c-453">Krok 4: Zarejestruj skryptu po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="3217c-453">Step 4 : Register the Client-Side Script</span></span>

<span data-ttu-id="3217c-454">Odwołania, które wywołanie GetCallbackEventReference określone, czy skrypt po stronie klienta jest nazywany **ShowCompanyName** może być wykonywana w przypadku pomyślnego wywołania zwrotnego po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="3217c-454">Recall that the call to GetCallbackEventReference specified that a client-side script called **ShowCompanyName** would be executed when the server-side callback succeeds.</span></span> <span data-ttu-id="3217c-455">Czy skrypt musi zostać dodany do strony przy użyciu wystąpienia ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="3217c-455">That script needs to be added to the page using a ClientScriptManager instance.</span></span> <span data-ttu-id="3217c-456">(W klasie ClientScriptManager będzie dicussed w dalszej części tego modułu). Tak zrobisz tego typu:</span><span class="sxs-lookup"><span data-stu-id="3217c-456">(The ClientScriptManager class will be dicussed later in this module.) You do that like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a><span data-ttu-id="3217c-457">Krok 5: Wywoływać metod interfejsu modułu ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="3217c-457">Step 5 : Call the Methods of the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="3217c-458">Modułu ICallbackEventHandler zawiera dwie metody, które są potrzebne do zaimplementowania w kodzie.</span><span class="sxs-lookup"><span data-stu-id="3217c-458">The ICallbackEventHandler contains two methods that you need to implement in your code.</span></span> <span data-ttu-id="3217c-459">Są one **RaiseCallbackEvent** i **GetCallbackEvent**.</span><span class="sxs-lookup"><span data-stu-id="3217c-459">They are **RaiseCallbackEvent** and **GetCallbackEvent**.</span></span>

<span data-ttu-id="3217c-460">**RaiseCallbackEvent** jako argument ciąg znaków i nie zwraca żadnego.</span><span class="sxs-lookup"><span data-stu-id="3217c-460">**RaiseCallbackEvent** takes a string as an argument and returns nothing.</span></span> <span data-ttu-id="3217c-461">Argument ciągu jest przekazywany z wywołania po stronie klienta do formularza sieci Web\_DoCallback.</span><span class="sxs-lookup"><span data-stu-id="3217c-461">The string argument is passed from the client-side call to WebForm\_DoCallback.</span></span> <span data-ttu-id="3217c-462">W takim przypadku ta wartość jest *wartość* atrybutu o nazwie ddlCompany listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="3217c-462">In this case, that value is the *value* attribute of the dropdown called ddlCompany.</span></span> <span data-ttu-id="3217c-463">W metodzie RaiseCallbackEvent powinna zostać umieszczona kodu po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="3217c-463">Your server-side code should be placed in the RaiseCallbackEvent method.</span></span> <span data-ttu-id="3217c-464">Na przykład jeśli wywołanie zwrotne jest WebRequest przed zasób zewnętrzny, ten kod powinna zostać umieszczona w RaiseCallbackEvent.</span><span class="sxs-lookup"><span data-stu-id="3217c-464">For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.</span></span>

<span data-ttu-id="3217c-465">**GetCallbackEvent** jest odpowiedzialna za przetwarzanie zwracany wywołania zwrotnego do klienta.</span><span class="sxs-lookup"><span data-stu-id="3217c-465">**GetCallbackEvent** is responsible for processing the return of the callback to the client.</span></span> <span data-ttu-id="3217c-466">Go nie przyjmuje żadnych argumentów i zwraca wartość typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="3217c-466">It takes no arguments and returns a string.</span></span> <span data-ttu-id="3217c-467">Ciąg, który zwraca zostaną przekazane jako argument do funkcji po stronie klienta, w tym przypadku *ShowCompanyName*.</span><span class="sxs-lookup"><span data-stu-id="3217c-467">The string that it returns will be passed as an argument to the client-side function, in this case *ShowCompanyName*.</span></span>

<span data-ttu-id="3217c-468">Po wykonaniu powyższych czynności można przystąpić do wykonania wywołania zwrotnego skryptu w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="3217c-468">Once you have completed the above steps, you are ready to perform a script callback in ASP.NET 2.0.</span></span>


![](the-asp-net-2-0-page-model/_static/image4.png)


[<span data-ttu-id="3217c-469">Otwórz wideo na pełnym ekranie</span><span class="sxs-lookup"><span data-stu-id="3217c-469">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/callback1.wmv)


<span data-ttu-id="3217c-470">Wywołania zwrotne skryptu w programie ASP.NET są obsługiwane w dowolnej przeglądarki obsługującej wprowadzania wywołania XMLHttp.</span><span class="sxs-lookup"><span data-stu-id="3217c-470">Script callbacks in ASP.NET are supported in any browser that supports making XMLHttp calls.</span></span> <span data-ttu-id="3217c-471">Która obejmuje wszystkie nowoczesne przeglądarki w użyciu dzisiaj.</span><span class="sxs-lookup"><span data-stu-id="3217c-471">That includes all of the modern browsers in use today.</span></span> <span data-ttu-id="3217c-472">Program Internet Explorer używa obiektu XMLHttp ActiveX, podczas gdy obiekt wewnętrzny XMLHttp innych nowoczesnymi przeglądarkami (w tym nadchodzących IE 7).</span><span class="sxs-lookup"><span data-stu-id="3217c-472">Internet Explorer uses the XMLHttp ActiveX object while other modern browsers (including the upcoming IE 7) use an intrinsic XMLHttp object.</span></span> <span data-ttu-id="3217c-473">Aby określić programowo, jeśli przeglądarka obsługuje wywołań zwrotnych, możesz użyć **Request.Browser.SupportCallback** właściwości.</span><span class="sxs-lookup"><span data-stu-id="3217c-473">To programmatically determine if a browser supports callbacks, you can use the **Request.Browser.SupportCallback** property.</span></span> <span data-ttu-id="3217c-474">Ta właściwość będzie zwracać **true** Jeśli klienta obsługuje wywołania zwrotne skryptu.</span><span class="sxs-lookup"><span data-stu-id="3217c-474">This property will return **true** if the requesting client supports script callbacks.</span></span>

## <a name="working-with-client-script-in-aspnet-20"></a><span data-ttu-id="3217c-475">Praca z skrypt po stronie klienta w programie ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="3217c-475">Working with Client Script in ASP.NET 2.0</span></span>

<span data-ttu-id="3217c-476">Skrypty klienta w programie ASP.NET 2.0 są zarządzane przez użycie klasy ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="3217c-476">Client scripts in ASP.NET 2.0 are managed via the use of the ClientScriptManager class.</span></span> <span data-ttu-id="3217c-477">Klasa ClientScriptManager przechowuje informacje o skrypty klienta przy użyciu typu i nazwy.</span><span class="sxs-lookup"><span data-stu-id="3217c-477">The ClientScriptManager class keeps track of client scripts using a type and a name.</span></span> <span data-ttu-id="3217c-478">Zapobiega to programowo wstawiane na stronie więcej niż jeden raz przez tego samego skryptu.</span><span class="sxs-lookup"><span data-stu-id="3217c-478">This prevents the same script from being programmatically inserted on a page more than once.</span></span>

> [!NOTE]
> <span data-ttu-id="3217c-479">Po skrypt został pomyślnie zarejestrowany na stronie, kolejne próby zarejestrowania tego samego skryptu, po prostu spowoduje skryptu nie jest zarejestrowany na drugim.</span><span class="sxs-lookup"><span data-stu-id="3217c-479">After a script has been successfully registered on a page, any subsequent attempt to register the same script will simply result in the script not being registered a second time.</span></span> <span data-ttu-id="3217c-480">Żadnych zduplikowanych skryptów są dodawane i wystąpi żaden wyjątek.</span><span class="sxs-lookup"><span data-stu-id="3217c-480">No duplicate scripts are added and no exception occurs.</span></span> <span data-ttu-id="3217c-481">Aby uniknąć niepotrzebnych obliczeń, istnieją metody, których można użyć w celu ustalenia, czy skrypt jest już zarejestrowany, tak aby nie należy go zarejestrować więcej niż raz.</span><span class="sxs-lookup"><span data-stu-id="3217c-481">To avoid unnecessary computation, there are methods that you can use to determine if a script is already registered so that you do not attempt to register it more than once.</span></span>


<span data-ttu-id="3217c-482">Metody ClientScriptManager powinny być znane wszystkie bieżące deweloperów platformy ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="3217c-482">The methods of the ClientScriptManager should be familiar to all current ASP.NET developers:</span></span>

## <a name="registerclientscriptblock"></a><span data-ttu-id="3217c-483">RegisterClientScriptBlock</span><span class="sxs-lookup"><span data-stu-id="3217c-483">RegisterClientScriptBlock</span></span>

<span data-ttu-id="3217c-484">Ta metoda dodaje skrypt do góry renderowanej strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-484">This method adds a script to the top of the rendered page.</span></span> <span data-ttu-id="3217c-485">Jest to przydatne w przypadku dodawania funkcji, które będą wywoływane jawnie na kliencie.</span><span class="sxs-lookup"><span data-stu-id="3217c-485">This is useful for adding functions that will be explicitly called on the client.</span></span>

<span data-ttu-id="3217c-486">Istnieją dwie wersje przeciążenia tej metody.</span><span class="sxs-lookup"><span data-stu-id="3217c-486">There are two overloaded versions of this method.</span></span> <span data-ttu-id="3217c-487">Trzy z czterech argumentów występują między nimi.</span><span class="sxs-lookup"><span data-stu-id="3217c-487">Three of four arguments are common among them.</span></span> <span data-ttu-id="3217c-488">Są to:</span><span class="sxs-lookup"><span data-stu-id="3217c-488">They are:</span></span>

`type (string)`

<span data-ttu-id="3217c-489">***Typu*** argument określa typ skryptu.</span><span class="sxs-lookup"><span data-stu-id="3217c-489">The ***type*** argument identifies a type for the script.</span></span> <span data-ttu-id="3217c-490">Zazwyczaj jest dobrym pomysłem jest typ strony, (this. GetType()) dla typu.</span><span class="sxs-lookup"><span data-stu-id="3217c-490">It is generally a good idea to use the page's type (this.GetType()) for the type.</span></span>

`key (string)`

<span data-ttu-id="3217c-491">***Klucza*** argument jest zdefiniowane przez użytkownika klucza dla skryptu.</span><span class="sxs-lookup"><span data-stu-id="3217c-491">The ***key*** argument is a user-defined key for the script.</span></span> <span data-ttu-id="3217c-492">Powinny to być unikatowe dla każdego skryptu.</span><span class="sxs-lookup"><span data-stu-id="3217c-492">This should be unique for each script.</span></span> <span data-ttu-id="3217c-493">Jeśli próbujesz dodać skrypt za pomocą tego samego klucza i typ skryptu już dodany, nie można dodać.</span><span class="sxs-lookup"><span data-stu-id="3217c-493">If you attempt to add a script with the same key and type of an already added script, it will not be added.</span></span>

`script (string)`

<span data-ttu-id="3217c-494">***Skryptu*** argument jest ciąg zawierający faktyczny skrypt do dodania.</span><span class="sxs-lookup"><span data-stu-id="3217c-494">The ***script*** argument is a string containing the actual script to add.</span></span> <span data-ttu-id="3217c-495">Zaleca się używać StringBuilder utworzyć skrypt, a następnie użyć metodę ToString() na StringBuilder można przypisać ***skryptu*** argumentu.</span><span class="sxs-lookup"><span data-stu-id="3217c-495">It's recommended that you use a StringBuilder to create the script and then use the ToString() method on the StringBuilder to assign the ***script*** argument.</span></span>

<span data-ttu-id="3217c-496">Użycie przeciążonej RegisterClientScriptBlock, który przyjmuje tylko trzech argumentów, musi zawierać elementy skryptu (&lt;skryptu&gt; i &lt;/script&gt;) w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="3217c-496">If you use the overloaded RegisterClientScriptBlock that only takes three arguments, you must include script elements (&lt;script&gt; and &lt;/script&gt;) in your script.</span></span>

<span data-ttu-id="3217c-497">Możesz użyć przeciążenia RegisterClientScriptBlock, który pobiera czwarty argument.</span><span class="sxs-lookup"><span data-stu-id="3217c-497">You may choose to use the overload of RegisterClientScriptBlock that takes a fourth argument.</span></span> <span data-ttu-id="3217c-498">Czwarty argument jest wartość logiczna określająca, czy ASP.NET, należy dodać elementy skryptu dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="3217c-498">The fourth argument is a Boolean that specifies whether or not ASP.NET should add script elements for you.</span></span> <span data-ttu-id="3217c-499">Jeśli ten argument jest **true**, skrypt nie może zawierać elementy skryptu jawnie.</span><span class="sxs-lookup"><span data-stu-id="3217c-499">If this argument is **true**, your script should not include the script elements explicitly.</span></span>

<span data-ttu-id="3217c-500">Metoda IsClientScriptBlockRegistered ustalenie, jeśli skrypt został już zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="3217c-500">Use the IsClientScriptBlockRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="3217c-501">Dzięki temu można uniknąć próba ponownego zarejestrowania skrypt, który został już zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="3217c-501">This allows you to avoid an attempt to re-register a script that has already been registered.</span></span>

### <a name="registerclientscriptinclude-new-in-20"></a><span data-ttu-id="3217c-502">RegisterClientScriptInclude (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="3217c-502">RegisterClientScriptInclude (New in 2.0)</span></span>

<span data-ttu-id="3217c-503">RegisterClientScriptInclude tag tworzy blok skryptu, który stanowi łącze do zewnętrznego pliku skryptu.</span><span class="sxs-lookup"><span data-stu-id="3217c-503">The RegisterClientScriptInclude tag creates a script block that links to an external script file.</span></span> <span data-ttu-id="3217c-504">Ma on dwa przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="3217c-504">It has two overloads.</span></span> <span data-ttu-id="3217c-505">Wyższy klucza i adres URL.</span><span class="sxs-lookup"><span data-stu-id="3217c-505">One takes a key and a URL.</span></span> <span data-ttu-id="3217c-506">Drugi dodaje trzeci argument określenie typu.</span><span class="sxs-lookup"><span data-stu-id="3217c-506">The second adds a third argument specifying the type.</span></span>

<span data-ttu-id="3217c-507">Na przykład następujący kod generuje blok skryptu zawierającego łącze do jsfunctions.js w głównym folderze skryptów aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3217c-507">For example, the following code generates a script block that links to jsfunctions.js in the root of the scripts folder of the application:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

<span data-ttu-id="3217c-508">Ten kod tworzy następujący kod na renderowanej stronie:</span><span class="sxs-lookup"><span data-stu-id="3217c-508">This code produces the following code in the rendered page:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> <span data-ttu-id="3217c-509">Blok skryptu jest renderowany w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="3217c-509">The script block is rendered at the bottom of the page.</span></span>


<span data-ttu-id="3217c-510">Metoda IsClientScriptIncludeRegistered ustalenie, jeśli skrypt został już zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="3217c-510">Use the IsClientScriptIncludeRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="3217c-511">Dzięki temu można uniknąć próba ponownego zarejestrowania skryptu.</span><span class="sxs-lookup"><span data-stu-id="3217c-511">This allows you to avoid an attempt to re-register a script.</span></span>

## <a name="registerstartupscript"></a><span data-ttu-id="3217c-512">RegisterStartupScript</span><span class="sxs-lookup"><span data-stu-id="3217c-512">RegisterStartupScript</span></span>

<span data-ttu-id="3217c-513">Metoda RegisterStartupScript przyjmuje te same argumenty co metoda RegisterClientScriptBlock.</span><span class="sxs-lookup"><span data-stu-id="3217c-513">The RegisterStartupScript method takes the same arguments as the RegisterClientScriptBlock method.</span></span> <span data-ttu-id="3217c-514">Skrypt w zarejestrowany RegisterStartupScript wykonuje po pobraniu strony, ale przed zdarzeniem OnLoad po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3217c-514">A script registered with RegisterStartupScript executes after the page loads but before the OnLoad client-side event.</span></span> <span data-ttu-id="3217c-515">W 1.X, skrypty w zarejestrowany RegisterStartupScript zostały umieszczone bezpośrednio przed zamknięciem &lt;/form&gt; tagów, podczas gdy skryptów w zarejestrowany RegisterClientScriptBlock były umieszczane bezpośrednio po otwarciu &lt;formularza&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="3217c-515">In 1.X, scripts registered with RegisterStartupScript were placed just before the closing &lt;/form&gt; tag while scripts registered with RegisterClientScriptBlock were placed immediately after the opening &lt;form&gt; tag.</span></span> <span data-ttu-id="3217c-516">W programie ASP.NET 2.0, zarówno są umieszczane bezpośrednio przed tagiem zamykającym &lt;/form&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="3217c-516">In ASP.NET 2.0, both are placed immediately before the closing &lt;/form&gt; tag.</span></span>

> [!NOTE]
> <span data-ttu-id="3217c-517">Jeśli zarejestrowane przez funkcję RegisterStartupScript, ta funkcja nie zostanie wykonany, dopóki nie można jawnie wywołać w kodzie po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3217c-517">If you register a function with RegisterStartupScript, that function will not execute until you explicitly call it in client-side code.</span></span>


<span data-ttu-id="3217c-518">Użyj metody IsStartupScriptRegistered, aby ustalić, jeśli skrypt został już zarejestrowany i uniknąć próba ponownego zarejestrowania skryptu.</span><span class="sxs-lookup"><span data-stu-id="3217c-518">Use the IsStartupScriptRegistered method to determine if a script has already been registered and avoid an attempt to re-register a script.</span></span>

## <a name="other-clientscriptmanager-methods"></a><span data-ttu-id="3217c-519">Inne metody ClientScriptManager</span><span class="sxs-lookup"><span data-stu-id="3217c-519">Other ClientScriptManager Methods</span></span>

<span data-ttu-id="3217c-520">Oto niektóre przydatne metody klasy ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="3217c-520">Here are some of the other useful methods of the ClientScriptManager class.</span></span>


|  <span data-ttu-id="3217c-521"><strong>GetCallbackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="3217c-521"><strong>GetCallbackEventReference</strong></span></span>   |                                                 <span data-ttu-id="3217c-522">Zobacz wywołania zwrotne skryptu wcześniej w tym module.</span><span class="sxs-lookup"><span data-stu-id="3217c-522">See script callbacks earlier in this module.</span></span>                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <span data-ttu-id="3217c-523"><strong>GetPostBackClientHyperlink</strong></span><span class="sxs-lookup"><span data-stu-id="3217c-523"><strong>GetPostBackClientHyperlink</strong></span></span>  |                <span data-ttu-id="3217c-524">Pobiera odwołanie JavaScript (javascript:&lt;wywołać&gt;) można post z zdarzeń po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="3217c-524">Gets a JavaScript reference (javascript:&lt;call&gt;) that can be used to post back from a client-side event.</span></span>                 |
|  <span data-ttu-id="3217c-525"><strong>GetPostBackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="3217c-525"><strong>GetPostBackEventReference</strong></span></span>   |                                   <span data-ttu-id="3217c-526">Pobiera ciąg, który może służyć do zainicjowania post przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3217c-526">Gets a string that can be used to initiate a post back from the client.</span></span>                                    |
|      <span data-ttu-id="3217c-527"><strong>GetWebResourceUrl</strong></span><span class="sxs-lookup"><span data-stu-id="3217c-527"><strong>GetWebResourceUrl</strong></span></span>       | <span data-ttu-id="3217c-528">Zwraca adres URL do zasobu, który jest osadzony w zestawie.</span><span class="sxs-lookup"><span data-stu-id="3217c-528">Returns a URL to a resource that is embedded in an assembly.</span></span> <span data-ttu-id="3217c-529">Należy używać w połączeniu z <strong>RegisterClientScriptResource</strong>.</span><span class="sxs-lookup"><span data-stu-id="3217c-529">Must be used in conjunction with <strong>RegisterClientScriptResource</strong>.</span></span> |
| <span data-ttu-id="3217c-530"><strong>RegisterClientScriptResource</strong></span><span class="sxs-lookup"><span data-stu-id="3217c-530"><strong>RegisterClientScriptResource</strong></span></span> |     <span data-ttu-id="3217c-531">Rejestruje zasobu sieci Web ze stroną.</span><span class="sxs-lookup"><span data-stu-id="3217c-531">Registers a Web resource with the page.</span></span> <span data-ttu-id="3217c-532">Są to zasoby osadzone w zestawie i obsługiwane przez nowy program obsługi WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="3217c-532">These are resources embedded in an assembly and handled by the new WebResource.axd handler.</span></span>      |
|     <span data-ttu-id="3217c-533"><strong>RegisterHiddenField</strong></span><span class="sxs-lookup"><span data-stu-id="3217c-533"><strong>RegisterHiddenField</strong></span></span>      |                                                 <span data-ttu-id="3217c-534">Rejestruje ukryte pole formularza ze stroną.</span><span class="sxs-lookup"><span data-stu-id="3217c-534">Registers a hidden form field with the page.</span></span>                                                 |
|  <span data-ttu-id="3217c-535"><strong>RegisterOnSubmitStatement</strong></span><span class="sxs-lookup"><span data-stu-id="3217c-535"><strong>RegisterOnSubmitStatement</strong></span></span>   |                                  <span data-ttu-id="3217c-536">Rejestruje kod po stronie klienta, który jest wykonywany po przesłaniu formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="3217c-536">Registers client-side code that executes when the HTML form is submitted.</span></span>                                   |

