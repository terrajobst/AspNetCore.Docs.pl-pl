---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Modelu programu ASP.NET 2.0 strony | Dokumentacja firmy Microsoft
author: microsoft
description: 'W programie ASP.NET: 1.x, deweloperzy ma wybór między wbudowany model kodu i model kodu związanego z kodem. Związane z kodem można zaimplementować przy użyciu Src attr...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 9d62aee5e0754b1910b923ad9ae501ebed91097e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379890"
---
<a name="the-aspnet-20-page-model"></a><span data-ttu-id="de0ad-104">Modelu programu ASP.NET 2.0 strony</span><span class="sxs-lookup"><span data-stu-id="de0ad-104">The ASP.NET 2.0 Page Model</span></span>
====================
<span data-ttu-id="de0ad-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="de0ad-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="de0ad-106">W programie ASP.NET: 1.x, deweloperzy ma wybór między wbudowany model kodu i model kodu związanego z kodem.</span><span class="sxs-lookup"><span data-stu-id="de0ad-106">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="de0ad-107">Związane z kodem można zaimplementować przy użyciu atrybutu Src lub atrybut CodeBehind @Page dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="de0ad-107">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="de0ad-108">W programie ASP.NET 2.0 deweloperzy nadal mają możliwość wyboru między wbudowany kod i związane z kodem, ale zostały znaczne ulepszenia do modelu związanym z kodem.</span><span class="sxs-lookup"><span data-stu-id="de0ad-108">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>


<span data-ttu-id="de0ad-109">W programie ASP.NET: 1.x, deweloperzy ma wybór między wbudowany model kodu i model kodu związanego z kodem.</span><span class="sxs-lookup"><span data-stu-id="de0ad-109">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="de0ad-110">Związane z kodem można zaimplementować przy użyciu atrybutu Src lub atrybut CodeBehind @Page dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="de0ad-110">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="de0ad-111">W programie ASP.NET 2.0 deweloperzy nadal mają możliwość wyboru między wbudowany kod i związane z kodem, ale zostały znaczne ulepszenia do modelu związanym z kodem.</span><span class="sxs-lookup"><span data-stu-id="de0ad-111">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>

## <a name="improvements-in-the-code-behind-model"></a><span data-ttu-id="de0ad-112">Ulepszenia w modelu związanym z kodem</span><span class="sxs-lookup"><span data-stu-id="de0ad-112">Improvements in the Code-Behind Model</span></span>

<span data-ttu-id="de0ad-113">Aby można było w pełni zapoznać się ze zmianami w modelu związanym z kodem w programie ASP.NET 2.0, jego najlepiej, aby szybko zapoznać się z modelu, ponieważ istniał w programie ASP.NET: 1.x.</span><span class="sxs-lookup"><span data-stu-id="de0ad-113">In order to fully understand the changes in the code-behind model in ASP.NET 2.0, its best to quickly review the model as it existed in ASP.NET 1.x.</span></span>

## <a name="the-code-behind-model-in-aspnet-1x"></a><span data-ttu-id="de0ad-114">Model związanym z kodem w programie ASP.NET: 1.x</span><span class="sxs-lookup"><span data-stu-id="de0ad-114">The Code-Behind Model in ASP.NET 1.x</span></span>

<span data-ttu-id="de0ad-115">W programie ASP.NET: 1.x, model związanym z kodem składa się z plikiem ASPX (formularz sieci Web) i pliku związanego z kodem, kodem programowania.</span><span class="sxs-lookup"><span data-stu-id="de0ad-115">In ASP.NET 1.x, the code-behind model consisted of an ASPX file (the Webform) and a code-behind file containing programming code.</span></span> <span data-ttu-id="de0ad-116">Dwa pliki zostały połączone za pomocą @Page dyrektywy w pliku ASPX.</span><span class="sxs-lookup"><span data-stu-id="de0ad-116">The two files were connected using the @Page directive in the ASPX file.</span></span> <span data-ttu-id="de0ad-117">Każdej kontrolki na stronie ASPX miały odpowiednich deklaracji w pliku związanym z kodem jako zmienna wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="de0ad-117">Each control on the ASPX page had a corresponding declaration in the code-behind file as an instance variable.</span></span> <span data-ttu-id="de0ad-118">Plik związany z kodem również zawiera kod dla powiązania zdarzeń i wygenerowanego kodu niezbędne do projektanta programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de0ad-118">The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer.</span></span> <span data-ttu-id="de0ad-119">Ten model jest dość dobrze działa, ale ponieważ wymaganego przez każdy element ASP.NET strony ASPX odpowiedni kod w pliku związanym z kodem wystąpił bez separacji true kodu i zawartości.</span><span class="sxs-lookup"><span data-stu-id="de0ad-119">This model worked fairly well, but because every ASP.NET element in the ASPX page required corresponding code in the code-behind file, there was no true separation of code and content.</span></span> <span data-ttu-id="de0ad-120">Na przykład jeśli projektant dodany nowy formant serwera do pliku ASPX poza programem Visual Studio IDE, aplikacja zaburzyłaby z powodu braku deklarację dla tego formantu w pliku związanym z kodem.</span><span class="sxs-lookup"><span data-stu-id="de0ad-120">For example, if a designer added a new server control to an ASPX file outside of the Visual Studio IDE, the application would break due to the absence of a declaration for that control in the code-behind file.</span></span>

## <a name="the-code-behind-model-in-aspnet-20"></a><span data-ttu-id="de0ad-121">Model związanym z kodem w programie ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="de0ad-121">The Code-Behind Model in ASP.NET 2.0</span></span>

<span data-ttu-id="de0ad-122">Program ASP.NET 2.0 znacznie poprawia przy tym modelu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-122">ASP.NET 2.0 greatly improves upon this model.</span></span> <span data-ttu-id="de0ad-123">W programie ASP.NET 2.0 związanym z kodem jest implementowany przy użyciu nowego *klas częściowych* dostarczane w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="de0ad-123">In ASP.NET 2.0, code-behind is implemented using the new *partial classes* provided in ASP.NET 2.0.</span></span> <span data-ttu-id="de0ad-124">Klasy związane z kodem w programie ASP.NET 2.0 jest definiowane jako klasę częściową, co oznacza, że zawiera on tylko część definicji klasy.</span><span class="sxs-lookup"><span data-stu-id="de0ad-124">The code-behind class in ASP.NET 2.0 is definied as a partial class meaning that it contains only part of the class definition.</span></span> <span data-ttu-id="de0ad-125">Pozostała część definicji klasy jest generowana dynamicznie przy ASP.NET 2.0 przy użyciu strony ASPX, w czasie wykonywania lub witryny sieci Web są wstępnie skompilowane.</span><span class="sxs-lookup"><span data-stu-id="de0ad-125">The remaining part of the class definition is dynamically generated by ASP.NET 2.0 using the ASPX page at runtime or when the Web site is precompiled.</span></span> <span data-ttu-id="de0ad-126">Łącze między plikiem CodeBehind i strony ASPX nadal zostanie nawiązane, przy użyciu dyrektywy @ Page.</span><span class="sxs-lookup"><span data-stu-id="de0ad-126">The link between the code-behind file and the ASPX page is still established using the @ Page directive.</span></span> <span data-ttu-id="de0ad-127">Jednak zamiast atrybutu CodeBehind lub Src ASP.NET 2.0 używa teraz atrybut CodeFile.</span><span class="sxs-lookup"><span data-stu-id="de0ad-127">However, instead of a CodeBehind or Src attribute, ASP.NET 2.0 now uses the CodeFile attribute.</span></span> <span data-ttu-id="de0ad-128">Atrybut Inherits umożliwia również określić nazwę klasy dla strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-128">The Inherits attribute is also used to specify the class name for the page.</span></span>

<span data-ttu-id="de0ad-129">Typowe dyrektywy @ Page może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="de0ad-129">A typical @ Page directive might look like this:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

<span data-ttu-id="de0ad-130">Definicję typowej klasy w pliku związanym z kodem programu ASP.NET 2.0 może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="de0ad-130">A typical class definition in an ASP.NET 2.0 code-behind file might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="de0ad-131">C# i Visual Basic są tylko języki zarządzane, obsługujące klas częściowych.</span><span class="sxs-lookup"><span data-stu-id="de0ad-131">C# and Visual Basic are the only managed languages that currently support partial classes.</span></span> <span data-ttu-id="de0ad-132">W związku z tym deweloperzy korzystający z języka J# nie będzie do korzystania z modelu związanym z kodem w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="de0ad-132">Therefore, developers using J# will not be able to use the code-behind model in ASP.NET 2.0.</span></span>


<span data-ttu-id="de0ad-133">Nowy model zwiększa modelu związanym z kodem, ponieważ deweloperzy będą teraz mieć plików kodu, które zawierają tylko kod, który zostały one utworzone.</span><span class="sxs-lookup"><span data-stu-id="de0ad-133">The new model enhances the code-behind model because developers will now have code files that contain only the code that they have created.</span></span> <span data-ttu-id="de0ad-134">Zapewnia także true separacja kodu i zawartości, ponieważ nie ma żadnych wystąpienia deklaracji zmiennych w pliku związanym z kodem.</span><span class="sxs-lookup"><span data-stu-id="de0ad-134">It also provides for a true separation of code and content because there are no instance variable declarations in the code-behind file.</span></span>

> [!NOTE]
> <span data-ttu-id="de0ad-135">Częściowe klasy dla strony ASPX jest, gdzie odbywa się powiązanie zdarzeń, deweloperów programu Visual Basic można weź pod uwagę wzrost niewielkim wzroście wydajności przy użyciu słowa kluczowego uchwytów w związanym z kodem można powiązać zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="de0ad-135">Because the partial class for the ASPX page is where event binding takes place, Visual Basic developers can realize a slight performance increase by using the Handles keyword in code-behind to bind events.</span></span> <span data-ttu-id="de0ad-136">C# ma nie równoważne słowa kluczowego.</span><span class="sxs-lookup"><span data-stu-id="de0ad-136">C# has no equivalent keyword.</span></span>


## <a name="new--page-directive-attributes"></a><span data-ttu-id="de0ad-137">Nowe atrybuty @ Page — dyrektywa</span><span class="sxs-lookup"><span data-stu-id="de0ad-137">New @ Page Directive Attributes</span></span>

<span data-ttu-id="de0ad-138">Program ASP.NET 2.0 dodaje wiele nowych atrybutów do dyrektywy @ Page.</span><span class="sxs-lookup"><span data-stu-id="de0ad-138">ASP.NET 2.0 adds many new attributes to the @ Page directive.</span></span> <span data-ttu-id="de0ad-139">Następujące atrybuty są nowością w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="de0ad-139">The following attributes are new in ASP.NET 2.0.</span></span>

## <a name="async"></a><span data-ttu-id="de0ad-140">Async</span><span class="sxs-lookup"><span data-stu-id="de0ad-140">Async</span></span>

<span data-ttu-id="de0ad-141">Atrybut Async pozwala skonfigurować stronę, aby być wykonywany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-141">The Async attribute allows you to configure page to be executed asynchronously.</span></span> <span data-ttu-id="de0ad-142">Obejmuje również asynchronicznego stron w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-142">Well cover asynchronous pages later in this module.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="de0ad-143">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="de0ad-143">AsyncTimeout</span></span>

<span data-ttu-id="de0ad-144">Określony limit czasu dla asynchronicznego stron.</span><span class="sxs-lookup"><span data-stu-id="de0ad-144">Specified the timeout for asynchronous pages.</span></span> <span data-ttu-id="de0ad-145">Wartość domyślna to 45 sekund.</span><span class="sxs-lookup"><span data-stu-id="de0ad-145">The default is 45 seconds.</span></span>

## <a name="codefile"></a><span data-ttu-id="de0ad-146">CodeFile</span><span class="sxs-lookup"><span data-stu-id="de0ad-146">CodeFile</span></span>

<span data-ttu-id="de0ad-147">Atrybut CodeFile jest zastępczych dla atrybutu pliku CodeBehind w programie Visual Studio 2002/2003.</span><span class="sxs-lookup"><span data-stu-id="de0ad-147">The CodeFile attribute is the replacement for the CodeBehind attribute in Visual Studio 2002/2003.</span></span>

### <a name="codefilebaseclass"></a><span data-ttu-id="de0ad-148">CodeFileBaseClass</span><span class="sxs-lookup"><span data-stu-id="de0ad-148">CodeFileBaseClass</span></span>

<span data-ttu-id="de0ad-149">Atrybut CodeFileBaseClass jest używany w przypadkach, w którym ma się wiele stron pochodzić z jednej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="de0ad-149">The CodeFileBaseClass attribute is used in cases where you want multiple pages to derive from a single base class.</span></span> <span data-ttu-id="de0ad-150">Ze względu na implementację klas częściowych w ASP.NET, bez tego atrybutu klasy bazowej, korzystającą z udostępnionych wspólnych pól w celu odwołują się do formantów zadeklarowanych w strony ASPX nie będzie działać poprawnie ponieważ ASP. Aparat kompilacji sieci automatycznie utworzy nowych elementów członkowskich na podstawie formantów na stronie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-150">Because of the implementation of partial classes in ASP.NET, without this attribute, a base class that uses shared common fields to reference controls declared in an ASPX page would not work properly because ASP.NETs compilation engine will automatically create new members based on controls in the page.</span></span> <span data-ttu-id="de0ad-151">W związku z tym, jeśli mają wspólną klasę bazową dla dwóch lub więcej stron w programie ASP.NET: należy zdefiniować Określ klasę bazową w atrybucie CodeFileBaseClass, a następnie dziedziczyć po każdej klasy strony tej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="de0ad-151">Therefore, if you want a common base class for two or more pages in ASP.NET, you will need to define specify your base class in the CodeFileBaseClass attribute and then derive each pages class from that base class.</span></span> <span data-ttu-id="de0ad-152">Atrybut CodeFile jest wymagany również w przypadku, gdy ten atrybut jest używany.</span><span class="sxs-lookup"><span data-stu-id="de0ad-152">The CodeFile attribute is also required when this attribute is used.</span></span>

## <a name="compilationmode"></a><span data-ttu-id="de0ad-153">CompilationMode</span><span class="sxs-lookup"><span data-stu-id="de0ad-153">CompilationMode</span></span>

<span data-ttu-id="de0ad-154">Ten atrybut umożliwia ustawienie właściwości CompilationMode strony ASPX.</span><span class="sxs-lookup"><span data-stu-id="de0ad-154">This attribute allows you to set the CompilationMode property of the ASPX page.</span></span> <span data-ttu-id="de0ad-155">Właściwość CompilationMode jest wyliczeniem, zawierające wartości **zawsze**, **automatycznie**, i **nigdy**.</span><span class="sxs-lookup"><span data-stu-id="de0ad-155">The CompilationMode property is an enumeration containing the values **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="de0ad-156">Wartość domyślna to **zawsze**.</span><span class="sxs-lookup"><span data-stu-id="de0ad-156">The default is **Always**.</span></span> <span data-ttu-id="de0ad-157">**Automatycznie** ustawienie uniemożliwi ASP.NET dynamicznie kompilowania strony, jeśli jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="de0ad-157">The **Auto** setting will prevent ASP.NET from dynamically compiling the page if possible.</span></span> <span data-ttu-id="de0ad-158">Wyłączanie stron z kompilacji dynamicznej zwiększa wydajność.</span><span class="sxs-lookup"><span data-stu-id="de0ad-158">Excluding pages from dynamic compilation increases performance.</span></span> <span data-ttu-id="de0ad-159">Jednakże, jeśli strona, która jest wykluczona zawiera ten kod, który musi być skompilowany, zostanie zwrócony błąd podczas przeglądania strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-159">However, if a page that is excluded contains that code that must be compiled, an error will be thrown when the page is browsed.</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="de0ad-160">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="de0ad-160">EnableEventValidation</span></span>

<span data-ttu-id="de0ad-161">Ten atrybut określa, czy zdarzenia odświeżenie strony i wywołań zwrotnych do sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="de0ad-161">This attribute specifies whether or not postback and callback events are validated.</span></span> <span data-ttu-id="de0ad-162">Gdy ta opcja jest włączona, argumenty odświeżania lub zdarzenia wywołania zwrotnego jest sprawdzany w celu upewnij się, że pochodzą formant serwera, który pierwotnie ich renderowania.</span><span class="sxs-lookup"><span data-stu-id="de0ad-162">When this is enabled, arguments to postback or callback events are checked to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="de0ad-163">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="de0ad-163">EnableTheming</span></span>

<span data-ttu-id="de0ad-164">Ten atrybut określa, czy motywów programu ASP.NET są używane na stronie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-164">This attribute specifies whether or not ASP.NET themes are used on a page.</span></span> <span data-ttu-id="de0ad-165">Wartość domyślna to **false**.</span><span class="sxs-lookup"><span data-stu-id="de0ad-165">The default is **false**.</span></span> <span data-ttu-id="de0ad-166">Motywy platformy ASP.NET zostały omówione w [10 modułu](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="de0ad-166">ASP.NET themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="linepragmas"></a><span data-ttu-id="de0ad-167">LinePragmas</span><span class="sxs-lookup"><span data-stu-id="de0ad-167">LinePragmas</span></span>

<span data-ttu-id="de0ad-168">Ten atrybut określa, czy podczas kompilacji należy dodać wyrażenia pragma wiersza.</span><span class="sxs-lookup"><span data-stu-id="de0ad-168">This attribute specifies whether line pragmas should be added during compilation.</span></span> <span data-ttu-id="de0ad-169">Wyrażenia pragma wiersza są opcje używane przez debugery, aby oznaczyć określonych sekcji kodu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-169">Line pragmas are options used by debuggers to mark specific sections of code.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="de0ad-170">MaintainScrollPositionOnPostback</span><span class="sxs-lookup"><span data-stu-id="de0ad-170">MaintainScrollPositionOnPostback</span></span>

<span data-ttu-id="de0ad-171">Ten atrybut określa, czy języka JavaScript są wstrzykiwane do strony, aby zachować położenie przewijania między ogłoszeniami wstecznymi.</span><span class="sxs-lookup"><span data-stu-id="de0ad-171">This attribute specifies whether or not JavaScript is injected into the page in order to maintain scroll position between postbacks.</span></span> <span data-ttu-id="de0ad-172">Ten atrybut jest **false** domyślnie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-172">This attribute is **false** by default.</span></span>

<span data-ttu-id="de0ad-173">Jeśli ten atrybut jest **true**, ASP.NET spowoduje to dodanie &lt;skryptu&gt; bloku na odświeżenie strony, która wygląda w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="de0ad-173">When this attribute is **true**, ASP.NET will add a &lt;script&gt; block on postback that looks like this:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

<span data-ttu-id="de0ad-174">Należy pamiętać, że src dla tego bloku skryptu WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="de0ad-174">Note that the src for this script block is WebResource.axd.</span></span> <span data-ttu-id="de0ad-175">Ten zasób nie jest ścieżką fizyczną.</span><span class="sxs-lookup"><span data-stu-id="de0ad-175">This resource is not a physical path.</span></span> <span data-ttu-id="de0ad-176">Zleconą ten skrypt ASP.NET dynamicznie tworzy skrypt.</span><span class="sxs-lookup"><span data-stu-id="de0ad-176">When this script is requested, ASP.NET dynamically builds the script.</span></span>

### <a name="masterpagefile"></a><span data-ttu-id="de0ad-177">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="de0ad-177">MasterPageFile</span></span>

<span data-ttu-id="de0ad-178">Ten atrybut określa plik strony głównej dla bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-178">This attribute specifies the master page file for the current page.</span></span> <span data-ttu-id="de0ad-179">Ścieżka może być względna lub bezwzględna.</span><span class="sxs-lookup"><span data-stu-id="de0ad-179">The path can be relative or absolute.</span></span> <span data-ttu-id="de0ad-180">Strony wzorcowe są objęte [modułu 4](master-pages.md).</span><span class="sxs-lookup"><span data-stu-id="de0ad-180">Master pages are covered in [Module 4](master-pages.md).</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="de0ad-181">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="de0ad-181">StyleSheetTheme</span></span>

<span data-ttu-id="de0ad-182">Ten atrybut umożliwia zastąpienie właściwości wygląd interfejsu użytkownika zdefiniowane przez motywów programu ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="de0ad-182">This attribute allows you to override user-interface appearance properties defined by an ASP.NET 2.0 theme.</span></span> <span data-ttu-id="de0ad-183">Motywy są objęte [10 modułu](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="de0ad-183">Themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="theme"></a><span data-ttu-id="de0ad-184">Motyw</span><span class="sxs-lookup"><span data-stu-id="de0ad-184">Theme</span></span>

<span data-ttu-id="de0ad-185">Określa motywu strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-185">Specifies the theme for the page.</span></span> <span data-ttu-id="de0ad-186">Jeśli nie określono wartości dla atrybutu StyleSheetTheme, atrybut motywu zastępuje wszystkie style, które dotyczą kontrolki na stronie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-186">If a value is not specified for the StyleSheetTheme attribute, the Theme attribute overrides all styles applied to controls on the page.</span></span>

## <a name="title"></a><span data-ttu-id="de0ad-187">Tytuł</span><span class="sxs-lookup"><span data-stu-id="de0ad-187">Title</span></span>

<span data-ttu-id="de0ad-188">Ustawia tytuł strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-188">Sets the title for the page.</span></span> <span data-ttu-id="de0ad-189">Wartość określona w tym miejscu będą wyświetlane w &lt;tytuł&gt; element renderowanej strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-189">The value specified here will appear in the &lt;title&gt; element of the rendered page.</span></span>

### <a name="viewstateencryptionmode"></a><span data-ttu-id="de0ad-190">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="de0ad-190">ViewStateEncryptionMode</span></span>

<span data-ttu-id="de0ad-191">Ustawia wartość wyliczenia ViewStateEncryptionMode.</span><span class="sxs-lookup"><span data-stu-id="de0ad-191">Sets the value for the ViewStateEncryptionMode enumeration.</span></span> <span data-ttu-id="de0ad-192">Dostępne wartości to **zawsze**, **automatycznie**, i **nigdy**.</span><span class="sxs-lookup"><span data-stu-id="de0ad-192">The available values are **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="de0ad-193">Wartość domyślna to **automatycznie**. Gdy ten atrybut jest ustawiony na wartość **automatycznie**, stan widoku jest szyfrowany jest kontrolki go przez wywołanie metody **RegisterRequiresViewStateEncryption** metody.</span><span class="sxs-lookup"><span data-stu-id="de0ad-193">The default value is **Auto**. When this attribute is set to a value of **Auto**, viewstate is encrypted is a control requests it by calling the **RegisterRequiresViewStateEncryption** method.</span></span>

## <a name="setting-public-property-values-via-the--page-directive"></a><span data-ttu-id="de0ad-194">Ustawianie wartości właściwości publicznej za pośrednictwem @ Page — dyrektywa</span><span class="sxs-lookup"><span data-stu-id="de0ad-194">Setting Public Property Values via the @ Page Directive</span></span>

<span data-ttu-id="de0ad-195">Kolejna nowa możliwość dyrektywy @ Page w programie ASP.NET 2.0 jest możliwość ustawiania początkowa wartość właściwości publiczne klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="de0ad-195">Another new capability of the @ Page directive in ASP.NET 2.0 is the ability to set the initial value of public properties of a base class.</span></span> <span data-ttu-id="de0ad-196">Załóżmy na przykład, że masz właściwość publiczną o nazwie **SomeText** w swojej klasy bazowej i d podoba Ci się zostać zainicjowana do **Hello** po załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-196">Suppose, for example, that you have a public property called **SomeText** in your base class and you d like it to be initialized to **Hello** when a page is loaded.</span></span> <span data-ttu-id="de0ad-197">Można to zrobić, po prostu ustawiając wartość w dyrektywy @ Page w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="de0ad-197">You can accomplish this by simply setting the value in the @ Page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

<span data-ttu-id="de0ad-198">**SomeText** atrybutu dyrektywy @ Page ustawia początkowe wartości właściwości SomeText w klasie bazowej, aby *Hello!*.</span><span class="sxs-lookup"><span data-stu-id="de0ad-198">The **SomeText** attribute of the @ Page directive sets the initial value of the SomeText property in the base class to *Hello!*.</span></span> <span data-ttu-id="de0ad-199">Poniższy klip wideo jest wskazówki dotyczące ustawiania wartości początkowej właściwość publiczna w klasie bazowej, przy użyciu dyrektywy @ Page.</span><span class="sxs-lookup"><span data-stu-id="de0ad-199">The video below is a walkthrough of setting the initial value of a public property in a base class using the @ Page directive.</span></span>


![](the-asp-net-2-0-page-model/_static/image1.png)


[<span data-ttu-id="de0ad-200">Otwórz wideo pełnego ekranu</span><span class="sxs-lookup"><span data-stu-id="de0ad-200">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a><span data-ttu-id="de0ad-201">Nowe właściwości publicznej klasy strony</span><span class="sxs-lookup"><span data-stu-id="de0ad-201">New Public Properties of the Page Class</span></span>

<span data-ttu-id="de0ad-202">Następujące właściwości publiczne są nowością w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="de0ad-202">The following public properties are new in ASP.NET 2.0.</span></span>

## <a name="apprelativetemplatesourcedirectory"></a><span data-ttu-id="de0ad-203">AppRelativeTemplateSourceDirectory</span><span class="sxs-lookup"><span data-stu-id="de0ad-203">AppRelativeTemplateSourceDirectory</span></span>

<span data-ttu-id="de0ad-204">Zwraca ścieżkę względem aplikacji do strony lub formant.</span><span class="sxs-lookup"><span data-stu-id="de0ad-204">Returns the application-relative path to the page or control.</span></span> <span data-ttu-id="de0ad-205">Na przykład strony znajduje się w http://app/folder/page.aspx, zwraca właściwości ~ / folder /.</span><span class="sxs-lookup"><span data-stu-id="de0ad-205">For example, for a page located at http://app/folder/page.aspx, the property returns ~/folder/.</span></span>

## <a name="apprelativevirtualpath"></a><span data-ttu-id="de0ad-206">AppRelativeVirtualPath</span><span class="sxs-lookup"><span data-stu-id="de0ad-206">AppRelativeVirtualPath</span></span>

<span data-ttu-id="de0ad-207">Zwraca ścieżkę względną katalogu wirtualnego do strona lub kontrolka.</span><span class="sxs-lookup"><span data-stu-id="de0ad-207">Returns the relative virtual directory path to the page or control.</span></span> <span data-ttu-id="de0ad-208">Na przykład w przypadku strony znajduje się w http://app/folder/page.aspx, zwraca właściwości ~ / folder/page.aspx.</span><span class="sxs-lookup"><span data-stu-id="de0ad-208">For example for a page located at http://app/folder/page.aspx, the property returns ~/folder/page.aspx.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="de0ad-209">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="de0ad-209">AsyncTimeout</span></span>

<span data-ttu-id="de0ad-210">Pobiera lub ustawia limit czasu, używane do obsługi asynchronicznego strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-210">Gets or sets the timeout used for asynchronous page handling.</span></span> <span data-ttu-id="de0ad-211">(Asynchronicznego strony będzie uwzględnione w dalszej części tego modułu).</span><span class="sxs-lookup"><span data-stu-id="de0ad-211">(Asynchronous pages will be covered later in this module.)</span></span>

## <a name="clientquerystring"></a><span data-ttu-id="de0ad-212">ClientQueryString</span><span class="sxs-lookup"><span data-stu-id="de0ad-212">ClientQueryString</span></span>

<span data-ttu-id="de0ad-213">Właściwość tylko do odczytu, która zwraca część ciągu zapytania żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="de0ad-213">A read-only property that returns the query string portion of the requested URL.</span></span> <span data-ttu-id="de0ad-214">Ta wartość jest zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="de0ad-214">This value is URL encoded.</span></span> <span data-ttu-id="de0ad-215">Metoda UrlDecode klasy HttpServerUtility służy do zdekodowania go.</span><span class="sxs-lookup"><span data-stu-id="de0ad-215">You can use the UrlDecode method of the HttpServerUtility class to decode it.</span></span>

## <a name="clientscript"></a><span data-ttu-id="de0ad-216">ClientScript</span><span class="sxs-lookup"><span data-stu-id="de0ad-216">ClientScript</span></span>

<span data-ttu-id="de0ad-217">Ta właściwość zwraca obiekt ClientScriptManager, który może służyć do zarządzania emisji ASP.NETs skryptu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="de0ad-217">This property returns a ClientScriptManager object that can be used to manage ASP.NETs emission of client-side script.</span></span> <span data-ttu-id="de0ad-218">(Klasy ClientScriptManager zostało opisane w dalszej części tego modułu).</span><span class="sxs-lookup"><span data-stu-id="de0ad-218">(The ClientScriptManager class is covered later in this module.)</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="de0ad-219">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="de0ad-219">EnableEventValidation</span></span>

<span data-ttu-id="de0ad-220">Ta właściwość określa, czy sprawdzanie poprawności zdarzenia jest włączony dla zdarzeń odświeżenie strony i wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="de0ad-220">This property controls whether or not event validation is enabled for postback and callback events.</span></span> <span data-ttu-id="de0ad-221">Po włączeniu argumentów odświeżania lub zdarzenia wywołania zwrotnego są weryfikowane i upewnij się, że pochodzą formant serwera, który pierwotnie ich renderowania.</span><span class="sxs-lookup"><span data-stu-id="de0ad-221">When enabled, arguments to postback or callback events are verified to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="de0ad-222">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="de0ad-222">EnableTheming</span></span>

<span data-ttu-id="de0ad-223">Tej właściwości pobiera lub ustawia wartość logiczna określająca, czy motywów programu ASP.NET 2.0 jest stosowany do strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-223">This property gets or sets a Boolean that specifies whether or not an ASP.NET 2.0 theme applies to the page.</span></span>

## <a name="form"></a><span data-ttu-id="de0ad-224">Formularz</span><span class="sxs-lookup"><span data-stu-id="de0ad-224">Form</span></span>

<span data-ttu-id="de0ad-225">Ta właściwość zwraca formularza HTML strony ASPX jako obiekt HtmlForm.</span><span class="sxs-lookup"><span data-stu-id="de0ad-225">This property returns the HTML form on the ASPX page as an HtmlForm object.</span></span>

## <a name="header"></a><span data-ttu-id="de0ad-226">nagłówek</span><span class="sxs-lookup"><span data-stu-id="de0ad-226">Header</span></span>

<span data-ttu-id="de0ad-227">Ta właściwość zwraca odwołanie do obiektu HtmlHead, która zawiera nagłówek strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-227">This property returns a reference to an HtmlHead object that contains the page header.</span></span> <span data-ttu-id="de0ad-228">Zwrócony obiekt HtmlHead służy do pobierania/ustawiania, arkusze stylów, tagów Meta itp.</span><span class="sxs-lookup"><span data-stu-id="de0ad-228">You can use the returned HtmlHead object to get/set style sheets, Meta tags, etc.</span></span>

## <a name="idseparator"></a><span data-ttu-id="de0ad-229">IdSeparator</span><span class="sxs-lookup"><span data-stu-id="de0ad-229">IdSeparator</span></span>

<span data-ttu-id="de0ad-230">Ta właściwość tylko do odczytu pobiera lub ustawia znak używany do oddzielania identyfikatory kontroli, kompilując ASP.NET jest unikatowy identyfikator dla formantów na stronie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-230">This read-only property gets the character that is used to separate control identifiers when ASP.NET is building a unique ID for controls on a page.</span></span> <span data-ttu-id="de0ad-231">Nie jest on przeznaczony do użycia bezpośrednio w kodzie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-231">It is not intended to be used directly from your code.</span></span>

## <a name="isasync"></a><span data-ttu-id="de0ad-232">IsAsync</span><span class="sxs-lookup"><span data-stu-id="de0ad-232">IsAsync</span></span>

<span data-ttu-id="de0ad-233">Ta właściwość umożliwia asynchroniczne stron.</span><span class="sxs-lookup"><span data-stu-id="de0ad-233">This property allows for asynchronous pages.</span></span> <span data-ttu-id="de0ad-234">Strony asynchroniczne zostały omówione w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-234">Asynchronous pages are discussed later in this module.</span></span>

## <a name="iscallback"></a><span data-ttu-id="de0ad-235">IsCallback</span><span class="sxs-lookup"><span data-stu-id="de0ad-235">IsCallback</span></span>

<span data-ttu-id="de0ad-236">Ta właściwość tylko do odczytu zwraca **true** Jeśli strona jest wynikiem wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="de0ad-236">This read-only property returns **true** if the page is the result of a call back.</span></span> <span data-ttu-id="de0ad-237">Oddzwanianie zostały omówione w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-237">Call backs are discussed later in this module.</span></span>

## <a name="iscrosspagepostback"></a><span data-ttu-id="de0ad-238">IsCrossPagePostBack</span><span class="sxs-lookup"><span data-stu-id="de0ad-238">IsCrossPagePostBack</span></span>

<span data-ttu-id="de0ad-239">Ta właściwość tylko do odczytu zwraca **true** Jeśli strona jest częścią ogłaszania zwrotnego między stronami.</span><span class="sxs-lookup"><span data-stu-id="de0ad-239">This read-only property returns **true** if the page is part of a cross-page postback.</span></span> <span data-ttu-id="de0ad-240">Ogłaszania zwrotnego między stronami zostały omówione w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-240">Cross-page postbacks are covered later in this module.</span></span>

## <a name="items"></a><span data-ttu-id="de0ad-241">Elementy</span><span class="sxs-lookup"><span data-stu-id="de0ad-241">Items</span></span>

<span data-ttu-id="de0ad-242">Zwraca odwołanie do wystąpienia element IDictionary zawierający wszystkie obiekty przechowywane w kontekście strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-242">Returns a reference to an IDictionary instance that contains all objects stored in the pages context.</span></span> <span data-ttu-id="de0ad-243">Można dodać elementy do tego obiektu IDictionary i będą one dostępne w okresie istnienia w kontekście.</span><span class="sxs-lookup"><span data-stu-id="de0ad-243">You can add items to this IDictionary object and they will be available to you throughout the lifetime of the context.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="de0ad-244">MaintainScrollPositionOnPostBack</span><span class="sxs-lookup"><span data-stu-id="de0ad-244">MaintainScrollPositionOnPostBack</span></span>

<span data-ttu-id="de0ad-245">Ta właściwość określa, czy program ASP.NET emituje języka JavaScript, który utrzymuje stron przewiń pozycji w przeglądarce, po wystąpieniu ogłaszania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="de0ad-245">This property controls whether or not ASP.NET emits JavaScript that maintains the pages scroll position in the browser after a postback occurs.</span></span> <span data-ttu-id="de0ad-246">(Szczegóły tej właściwości zostały omówione wcześniej w tym module).</span><span class="sxs-lookup"><span data-stu-id="de0ad-246">(Details of this property were discussed earlier in this module.)</span></span>

## <a name="master"></a><span data-ttu-id="de0ad-247">Wzorzec</span><span class="sxs-lookup"><span data-stu-id="de0ad-247">Master</span></span>

<span data-ttu-id="de0ad-248">Ta właściwość tylko do odczytu zwraca odwołanie do wystąpienia na nich dla strony, do którego zastosowano strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="de0ad-248">This read-only property returns a reference to the MasterPage instance for a page to which a master page has been applied.</span></span>

## <a name="masterpagefile"></a><span data-ttu-id="de0ad-249">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="de0ad-249">MasterPageFile</span></span>

<span data-ttu-id="de0ad-250">Pobiera lub ustawia nazwę pliku strony wzorcowej strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-250">Gets or sets the master page filename for the page.</span></span> <span data-ttu-id="de0ad-251">Tę właściwość można ustawić tylko w metodzie PreInit.</span><span class="sxs-lookup"><span data-stu-id="de0ad-251">This property can only be set in the PreInit method.</span></span>

## <a name="maxpagestatefieldlength"></a><span data-ttu-id="de0ad-252">MaxPageStateFieldLength</span><span class="sxs-lookup"><span data-stu-id="de0ad-252">MaxPageStateFieldLength</span></span>

<span data-ttu-id="de0ad-253">Tej właściwości pobiera lub ustawia maksymalną długość stanu stron w bajtach.</span><span class="sxs-lookup"><span data-stu-id="de0ad-253">This property gets or sets the maximum length for the pages state in bytes.</span></span> <span data-ttu-id="de0ad-254">Jeśli właściwość jest ustawiona na wartość dodatnią, stan widoku strony będzie być dzielone na wiele ukryte pola tak, aby nie przekracza rozmiaru liczbę bajtów określoną.</span><span class="sxs-lookup"><span data-stu-id="de0ad-254">If the property is set to a positive number, the pages view state will be broken up into multiple hidden fields so that it doesnt exceed the number of bytes specified.</span></span> <span data-ttu-id="de0ad-255">Jeśli właściwość jest liczbą ujemną, stan widoku nie można podzielić na fragmenty.</span><span class="sxs-lookup"><span data-stu-id="de0ad-255">If the property is a negative number, the view state will not be broken into chunks.</span></span>

## <a name="pageadapter"></a><span data-ttu-id="de0ad-256">PageAdapter</span><span class="sxs-lookup"><span data-stu-id="de0ad-256">PageAdapter</span></span>

<span data-ttu-id="de0ad-257">Zwraca odwołanie do obiektu PageAdapter, który modyfikuje strony do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="de0ad-257">Returns a reference to the PageAdapter object that modifies the page for the requesting browser.</span></span>

## <a name="previouspage"></a><span data-ttu-id="de0ad-258">PreviousPage</span><span class="sxs-lookup"><span data-stu-id="de0ad-258">PreviousPage</span></span>

<span data-ttu-id="de0ad-259">Zwraca odwołanie do poprzedniej strony w przypadku Server.Transfer lub ogłaszania zwrotnego między stronami.</span><span class="sxs-lookup"><span data-stu-id="de0ad-259">Returns a reference to the previous page in cases of a Server.Transfer or a cross-page postback.</span></span>

## <a name="skinid"></a><span data-ttu-id="de0ad-260">SkinID</span><span class="sxs-lookup"><span data-stu-id="de0ad-260">SkinID</span></span>

<span data-ttu-id="de0ad-261">Określa skórki ASP.NET 2.0 do zastosowania do strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-261">Specifies the ASP.NET 2.0 skin to apply to the page.</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="de0ad-262">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="de0ad-262">StyleSheetTheme</span></span>

<span data-ttu-id="de0ad-263">Tej właściwości pobiera lub ustawia arkusza stylów, która jest stosowana do strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-263">This property gets or sets the style sheet that is applied to a page.</span></span>

## <a name="templatecontrol"></a><span data-ttu-id="de0ad-264">TemplateControl</span><span class="sxs-lookup"><span data-stu-id="de0ad-264">TemplateControl</span></span>

<span data-ttu-id="de0ad-265">Zwraca odwołanie do formantu zawierającego dla strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-265">Returns a reference to the containing control for the page.</span></span>

## <a name="theme"></a><span data-ttu-id="de0ad-266">Motyw</span><span class="sxs-lookup"><span data-stu-id="de0ad-266">Theme</span></span>

<span data-ttu-id="de0ad-267">Pobiera lub ustawia nazwę motywu ASP.NET 2.0 zastosowana do strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-267">Gets or sets the name of the ASP.NET 2.0 theme applied to the page.</span></span> <span data-ttu-id="de0ad-268">Ta wartość musi być ustawiony przed metoda PreInit.</span><span class="sxs-lookup"><span data-stu-id="de0ad-268">This value must be set prior to the PreInit method.</span></span>

## <a name="title"></a><span data-ttu-id="de0ad-269">Tytuł</span><span class="sxs-lookup"><span data-stu-id="de0ad-269">Title</span></span>

<span data-ttu-id="de0ad-270">Tej właściwości pobiera lub ustawia tytuł strony, uzyskany w nagłówku strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-270">This property gets or sets the title for the page as obtained from the pages header.</span></span>

## <a name="viewstateencryptionmode"></a><span data-ttu-id="de0ad-271">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="de0ad-271">ViewStateEncryptionMode</span></span>

<span data-ttu-id="de0ad-272">Pobiera lub ustawia ViewStateEncryptionMode strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-272">Gets or sets the ViewStateEncryptionMode of the page.</span></span> <span data-ttu-id="de0ad-273">Zobacz szczegółowe omówienie tej właściwości we wcześniejszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-273">See a detailed discussion of this property earlier in this module.</span></span>

## <a name="new-protected-properties-of-the-page-class"></a><span data-ttu-id="de0ad-274">Nowe właściwości chronionego klasy strony</span><span class="sxs-lookup"><span data-stu-id="de0ad-274">New Protected Properties of the Page Class</span></span>

<span data-ttu-id="de0ad-275">Poniżej przedstawiono nowe właściwości chronionego klasy strony programu ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="de0ad-275">The following are the new protected properties of the Page class in ASP.NET 2.0.</span></span>

## <a name="adapter"></a><span data-ttu-id="de0ad-276">Adapter</span><span class="sxs-lookup"><span data-stu-id="de0ad-276">Adapter</span></span>

<span data-ttu-id="de0ad-277">Zwraca odwołanie do ControlAdapter, który powoduje wyświetlenie strony na urządzeniu żądane go.</span><span class="sxs-lookup"><span data-stu-id="de0ad-277">Returns a reference to the ControlAdapter that renders the page on the device that requested it.</span></span>

## <a name="asyncmode"></a><span data-ttu-id="de0ad-278">AsyncMode</span><span class="sxs-lookup"><span data-stu-id="de0ad-278">AsyncMode</span></span>

<span data-ttu-id="de0ad-279">Ta właściwość wskazuje, czy strony są przetwarzane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-279">This property indicates whether or not the page is processed asynchronously.</span></span> <span data-ttu-id="de0ad-280">Jest przeznaczony do użytku przez środowisko wykonawcze, a nie bezpośrednio w kodzie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-280">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="clientidseparator"></a><span data-ttu-id="de0ad-281">ClientIDSeparator</span><span class="sxs-lookup"><span data-stu-id="de0ad-281">ClientIDSeparator</span></span>

<span data-ttu-id="de0ad-282">Ta właściwość zwraca znak używany jako separator, podczas tworzenia unikatowych klientów identyfikatory dla formantów.</span><span class="sxs-lookup"><span data-stu-id="de0ad-282">This property returns the character used as a separator when creating unique client IDs for controls.</span></span> <span data-ttu-id="de0ad-283">Jest przeznaczony do użytku przez środowisko wykonawcze, a nie bezpośrednio w kodzie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-283">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="pagestatepersister"></a><span data-ttu-id="de0ad-284">PageStatePersister</span><span class="sxs-lookup"><span data-stu-id="de0ad-284">PageStatePersister</span></span>

<span data-ttu-id="de0ad-285">Ta właściwość zwraca obiekt PageStatePersister dla strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-285">This property returns the PageStatePersister object for the page.</span></span> <span data-ttu-id="de0ad-286">Ta właściwość jest głównie używana przez deweloperów kontroli platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="de0ad-286">This property is primarily used by ASP.NET control developers.</span></span>

## <a name="uniquefilepathsuffix"></a><span data-ttu-id="de0ad-287">UniqueFilePathSuffix</span><span class="sxs-lookup"><span data-stu-id="de0ad-287">UniqueFilePathSuffix</span></span>

<span data-ttu-id="de0ad-288">Ta właściwość Zwraca unikatowy suffic, która jest dołączana do ścieżki pliku do pamięci podręcznej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="de0ad-288">This property returns a unique suffic that is appended to the file path for caching browsers.</span></span> <span data-ttu-id="de0ad-289">Wartość domyślna to \_ \_ufps = 6-cyfrowy numer.</span><span class="sxs-lookup"><span data-stu-id="de0ad-289">The default value is \_\_ufps= and a 6-digit number.</span></span>

## <a name="new-public-methods-for-the-page-class"></a><span data-ttu-id="de0ad-290">Nowych metod publicznych klasy strony</span><span class="sxs-lookup"><span data-stu-id="de0ad-290">New Public Methods for the Page Class</span></span>

<span data-ttu-id="de0ad-291">Jesteś nowym użytkownikiem klasy strony programu ASP.NET 2.0 są następujące metody publiczne.</span><span class="sxs-lookup"><span data-stu-id="de0ad-291">The following public methods are new to the Page class in ASP.NET 2.0.</span></span>

## <a name="addonprerendercompleteasync"></a><span data-ttu-id="de0ad-292">AddOnPreRenderCompleteAsync</span><span class="sxs-lookup"><span data-stu-id="de0ad-292">AddOnPreRenderCompleteAsync</span></span>

<span data-ttu-id="de0ad-293">Ta metoda rejestruje delegatów obsługi zdarzeń do wykonania asynchronicznej strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-293">This method registers event handler delegates for asynchronous page execution.</span></span> <span data-ttu-id="de0ad-294">Strony asynchroniczne zostały omówione w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-294">Asynchronous pages are discussed later in this module.</span></span>

## <a name="applystylesheetskin"></a><span data-ttu-id="de0ad-295">ApplyStyleSheetSkin</span><span class="sxs-lookup"><span data-stu-id="de0ad-295">ApplyStyleSheetSkin</span></span>

<span data-ttu-id="de0ad-296">Zastosowanie właściwości w arkuszu stylów strony do strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-296">Applies the properties in a pages style sheet to the page.</span></span>

## <a name="executeregisteredasynctasks"></a><span data-ttu-id="de0ad-297">ExecuteRegisteredAsyncTasks</span><span class="sxs-lookup"><span data-stu-id="de0ad-297">ExecuteRegisteredAsyncTasks</span></span>

<span data-ttu-id="de0ad-298">Ta metoda żywych zadanie asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="de0ad-298">This method beings an asynchronous task.</span></span>

### <a name="getvalidators"></a><span data-ttu-id="de0ad-299">GetValidators</span><span class="sxs-lookup"><span data-stu-id="de0ad-299">GetValidators</span></span>

<span data-ttu-id="de0ad-300">Zwraca kolekcję moduły weryfikacji dla grupy sprawdzania poprawności określonego lub sprawdzania poprawności domyślnej grupy, jeśli nie określono.</span><span class="sxs-lookup"><span data-stu-id="de0ad-300">Returns a collection of validators for the specified validation group or the default validation group if none is specified.</span></span>

## <a name="registerasynctask"></a><span data-ttu-id="de0ad-301">RegisterAsyncTask</span><span class="sxs-lookup"><span data-stu-id="de0ad-301">RegisterAsyncTask</span></span>

<span data-ttu-id="de0ad-302">Ta metoda rejestruje nowe zadanie asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="de0ad-302">This method registers a new async task.</span></span> <span data-ttu-id="de0ad-303">Strony asynchroniczne zostały omówione w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-303">Asynchronous pages are covered later in this module.</span></span>

## <a name="registerrequirescontrolstate"></a><span data-ttu-id="de0ad-304">RegisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="de0ad-304">RegisterRequiresControlState</span></span>

<span data-ttu-id="de0ad-305">Ta metoda informuje ASP.NET stan formantu strony musi zostać utrwalone.</span><span class="sxs-lookup"><span data-stu-id="de0ad-305">This method tells ASP.NET that the pages control state must be persisted.</span></span>

## <a name="registerrequiresviewstateencryption"></a><span data-ttu-id="de0ad-306">RegisterRequiresViewStateEncryption</span><span class="sxs-lookup"><span data-stu-id="de0ad-306">RegisterRequiresViewStateEncryption</span></span>

<span data-ttu-id="de0ad-307">Ta metoda informuje ASP.NET, czy stan wyświetlania stron wymaga szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="de0ad-307">This method tells ASP.NET that the pages viewstate requires encryption.</span></span>

## <a name="resolveclienturl"></a><span data-ttu-id="de0ad-308">ResolveClientUrl</span><span class="sxs-lookup"><span data-stu-id="de0ad-308">ResolveClientUrl</span></span>

<span data-ttu-id="de0ad-309">Zwraca względny adres URL, który może służyć do obsługi żądań klientów, obrazy itp.</span><span class="sxs-lookup"><span data-stu-id="de0ad-309">Returns a relative URL that can be used for client requests for images, etc.</span></span>

## <a name="setfocus"></a><span data-ttu-id="de0ad-310">Funkcja SetFocus</span><span class="sxs-lookup"><span data-stu-id="de0ad-310">SetFocus</span></span>

<span data-ttu-id="de0ad-311">Ta metoda ustawi fokus do formantu, który jest określana podczas wczytywania strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-311">This method will set the focus to the control that is specified when the page is initially loaded.</span></span>

## <a name="unregisterrequirescontrolstate"></a><span data-ttu-id="de0ad-312">UnregisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="de0ad-312">UnregisterRequiresControlState</span></span>

<span data-ttu-id="de0ad-313">Ta metoda wyrejestruje formant, który jest przekazywany do niego jako już konieczności utrwalanie stanu kontrolki.</span><span class="sxs-lookup"><span data-stu-id="de0ad-313">This method will unregister the control that is passed to it as no longer requiring control state persistence.</span></span>

## <a name="changes-to-the-page-lifecycle"></a><span data-ttu-id="de0ad-314">Zmiany w cyklu życia strony</span><span class="sxs-lookup"><span data-stu-id="de0ad-314">Changes to the Page Lifecycle</span></span>

<span data-ttu-id="de0ad-315">Cyklu życia strony programu ASP.NET 2.0 nie zmieniło się drastycznie, ale istnieje kilka nowych metod, których należy wiedzieć.</span><span class="sxs-lookup"><span data-stu-id="de0ad-315">The page lifecycle in ASP.NET 2.0 hasnt changed dramatically, but there are some new methods that you should be aware of.</span></span> <span data-ttu-id="de0ad-316">Cyklu życia strony ASP.NET 2.0 jest opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="de0ad-316">The ASP.NET 2.0 page lifecycle is outlined below.</span></span>

## <a name="preinit-new-in-aspnet-20"></a><span data-ttu-id="de0ad-317">PreInit (Nowość w ASP.NET 2.0)</span><span class="sxs-lookup"><span data-stu-id="de0ad-317">PreInit (New in ASP.NET 2.0)</span></span>

<span data-ttu-id="de0ad-318">Zdarzenie PreInit jest najwcześniejszym etapie w cyklu życia, których deweloper może uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="de0ad-318">The PreInit event is the earliest stage in the lifecycle that a developer can access.</span></span> <span data-ttu-id="de0ad-319">Dodanie to zdarzenie umożliwia programowe Zmienianie motywów programu ASP.NET 2.0, strony wzorcowe, uzyskiwanie dostępu do właściwości profilu platformy ASP.NET 2.0 itp. Jeśli jesteś w stanie ogłaszania wstecznego, istotne jest, aby należy pamiętać, że stan widoku nie ma jeszcze zastosowane do kontrolek na tym etapie w cyklu życia.</span><span class="sxs-lookup"><span data-stu-id="de0ad-319">The addition of this event makes it possible to programmatically change ASP.NET 2.0 themes, master pages, access properties for an ASP.NET 2.0 profile, etc. If you are in a postback state, its important to realize that Viewstate has not yet been applied to controls at this point in the lifecycle.</span></span> <span data-ttu-id="de0ad-320">W związku z tym jeśli deweloper zmienia właściwość kontrolki na tym etapie, zostanie prawdopodobnie on zastąpiony w dalszej części cyklu życia strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-320">Therefore, if a developer changes a property of a control at this stage, it will likely be overwritten later in the pages lifecycle.</span></span>

## <a name="init"></a><span data-ttu-id="de0ad-321">Init</span><span class="sxs-lookup"><span data-stu-id="de0ad-321">Init</span></span>

<span data-ttu-id="de0ad-322">Zdarzenie inicjowania nie zmienił się z ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="de0ad-322">The Init event has not changed from ASP.NET 1.x.</span></span> <span data-ttu-id="de0ad-323">Jest to miejscu do odczytu lub zainicjować właściwości formantów na stronie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-323">This is where you would want to read or initialize properties of controls on your page.</span></span> <span data-ttu-id="de0ad-324">Na tym etapie, strony wzorcowe, motywy itp., są już stosowane do strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-324">At this stage, master pages, themes, etc. are already applied to the page.</span></span>

## <a name="initcomplete-new-in-20"></a><span data-ttu-id="de0ad-325">InitComplete (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="de0ad-325">InitComplete (New in 2.0)</span></span>

<span data-ttu-id="de0ad-326">Zdarzenie InitComplete jest wywoływane na końcu etapie inicjowania strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-326">The InitComplete event is called at the end of the pages initialization stage.</span></span> <span data-ttu-id="de0ad-327">Na tym etapie w cyklu życia, możesz uzyskać dostęp formantów na stronie, ale nie została jeszcze wypełniona ich stanu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-327">At this point in the lifecycle, you can access controls on the page, but their state has not yet been populated.</span></span>

## <a name="preload-new-in-20"></a><span data-ttu-id="de0ad-328">Wstępne ładowanie (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="de0ad-328">PreLoad (New in 2.0)</span></span>

<span data-ttu-id="de0ad-329">To zdarzenie jest wywoływane po zastosowaniu wszystkich danych ogłaszania wstecznego i tuż przed strony\_obciążenia.</span><span class="sxs-lookup"><span data-stu-id="de0ad-329">This event is called after all postback data has been applied and just prior to Page\_Load.</span></span>

## <a name="load"></a><span data-ttu-id="de0ad-330">Ładowanie</span><span class="sxs-lookup"><span data-stu-id="de0ad-330">Load</span></span>

<span data-ttu-id="de0ad-331">Załadowane zdarzenie nie zmienił się z ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="de0ad-331">The Load event has not changed from ASP.NET 1.x.</span></span>

## <a name="loadcomplete-new-in-20"></a><span data-ttu-id="de0ad-332">LoadComplete (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="de0ad-332">LoadComplete (New in 2.0)</span></span>

<span data-ttu-id="de0ad-333">Zdarzenie LoadComplete jest ostatnie zdarzenie w fazie ładowania stron.</span><span class="sxs-lookup"><span data-stu-id="de0ad-333">The LoadComplete event is the last event in the pages load stage.</span></span> <span data-ttu-id="de0ad-334">Na tym etapie wszystkie dane ogłaszania zwrotnego i stanu wyświetlania zostały doliczone do strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-334">At this stage, all postback and viewstate data has been applied to the page.</span></span>

## <a name="prerender"></a><span data-ttu-id="de0ad-335">PreRender</span><span class="sxs-lookup"><span data-stu-id="de0ad-335">PreRender</span></span>

<span data-ttu-id="de0ad-336">Jeśli chcesz uzyskać viewstate prawidłowo utrzymanie dla formantów, które są dodawane dynamicznie do strony, zdarzenie PreRender jest to ostatnia możliwość je dodać.</span><span class="sxs-lookup"><span data-stu-id="de0ad-336">If you would like for viewstate to be properly maintained for controls that are added to the page dynamically, the PreRender event is the last opportunity to add them.</span></span>

## <a name="prerendercomplete-new-in-20"></a><span data-ttu-id="de0ad-337">PreRenderComplete (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="de0ad-337">PreRenderComplete (New in 2.0)</span></span>

<span data-ttu-id="de0ad-338">Na etapie PreRenderComplete wszystkich kontrolek zostały dodane do strony, a strona jest gotowy do renderowania.</span><span class="sxs-lookup"><span data-stu-id="de0ad-338">At the PreRenderComplete stage, all controls have been added to the page and the page is ready to be rendered.</span></span> <span data-ttu-id="de0ad-339">Zdarzenie PreRenderComplete jest ostatnie zdarzenie zgłoszone przed zapisaniem stanu widoku strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-339">The PreRenderComplete event is the last event raised before the pages viewstate is saved.</span></span>

## <a name="savestatecomplete-new-in-20"></a><span data-ttu-id="de0ad-340">SaveStateComplete (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="de0ad-340">SaveStateComplete (New in 2.0)</span></span>

<span data-ttu-id="de0ad-341">Zdarzenie SaveStateComplete nosi nazwę natychmiast, po zapisaniu wszystkich stanów strony viewstate i kontroli.</span><span class="sxs-lookup"><span data-stu-id="de0ad-341">The SaveStateComplete event is called immediately after all page viewstate and control state has been saved.</span></span> <span data-ttu-id="de0ad-342">Jest to ostatnie zdarzenie przed wyświetleniem strony faktycznie w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="de0ad-342">This is the last event before the page is actually rendered to the browser.</span></span>

## <a name="render"></a><span data-ttu-id="de0ad-343">Renderowanie</span><span class="sxs-lookup"><span data-stu-id="de0ad-343">Render</span></span>

<span data-ttu-id="de0ad-344">Metody renderowania nie zmienił się od ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="de0ad-344">The Render method has not changed since ASP.NET 1.x.</span></span> <span data-ttu-id="de0ad-345">Jest to, gdzie HtmlTextWriter jest inicjowany i wyrenderowaniu strony do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="de0ad-345">This is where the HtmlTextWriter is initialized and the page is rendered to the browser.</span></span>

## <a name="cross-page-postback-in-aspnet-20"></a><span data-ttu-id="de0ad-346">Ogłaszania zwrotnego między stronami, w programie ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="de0ad-346">Cross-Page Postback in ASP.NET 2.0</span></span>

<span data-ttu-id="de0ad-347">W programie ASP.NET: 1.x, ogłaszania zwrotnego były wymagane do wysłania do tej samej stronie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-347">In ASP.NET 1.x, postbacks were required to post to the same page.</span></span> <span data-ttu-id="de0ad-348">Ogłaszania zwrotnego między stronami nie były dozwolone.</span><span class="sxs-lookup"><span data-stu-id="de0ad-348">Cross-page postbacks were not allowed.</span></span> <span data-ttu-id="de0ad-349">Program ASP.NET 2.0 dodaje możliwość Opublikuj innej strony za pomocą interfejsu IButtonControl.</span><span class="sxs-lookup"><span data-stu-id="de0ad-349">ASP.NET 2.0 adds the ability to post back to a different page via the IButtonControl interface.</span></span> <span data-ttu-id="de0ad-350">Żadnego formantu, który implementuje nowy interfejs IButtonControl (Button, element LinkButton i ImageButton oprócz niestandardowe formanty innych firm) mogą korzystać z tej nowej funkcji przez użycie atrybutu PostBackUrl.</span><span class="sxs-lookup"><span data-stu-id="de0ad-350">Any control that implements the new IButtonControl interface (Button, LinkButton, and ImageButton in addition to third-party custom controls) can take advantage of this new functionality via the use of the PostBackUrl attribute.</span></span> <span data-ttu-id="de0ad-351">Poniższy kod przedstawia formant przycisku, który publikuje drugiej strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-351">The following code shows a Button control that posts back to a second page.</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

<span data-ttu-id="de0ad-352">Gdy strona jest przesyłana z powrotem, Strona która inicjuje odświeżenie strony są dostępne przez właściwość PreviousPage na drugiej stronie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-352">When the page is posted back, the Page that initiates the postback is accessible via the PreviousPage property on the second page.</span></span> <span data-ttu-id="de0ad-353">Ta funkcja jest zaimplementowana za pomocą nowego formularza sieci Web\_DoPostBackWithOptions po stronie klienta funkcji, która ASP.NET 2.0 renderuje do strony, gdy formant publikuje do innej strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-353">This functionality is implemented via the new WebForm\_DoPostBackWithOptions client-side function that ASP.NET 2.0 renders to the page when a control posts back to a different page.</span></span> <span data-ttu-id="de0ad-354">Ta funkcja JavaScript jest zapewniana przez nowy program obsługi WebResource.axd, który emituje skryptu do klienta.</span><span class="sxs-lookup"><span data-stu-id="de0ad-354">This JavaScript function is provided by the new WebResource.axd handler which emits script to the client.</span></span>

<span data-ttu-id="de0ad-355">Poniższy klip wideo jest przewodnik ogłaszania zwrotnego między stronami.</span><span class="sxs-lookup"><span data-stu-id="de0ad-355">The video below is a walkthrough of a cross-page postback.</span></span>


![](the-asp-net-2-0-page-model/_static/image2.png)


[<span data-ttu-id="de0ad-356">Otwórz wideo pełnego ekranu</span><span class="sxs-lookup"><span data-stu-id="de0ad-356">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a><span data-ttu-id="de0ad-357">Szczegółowe informacje na temat ogłaszania zwrotnego między stronami</span><span class="sxs-lookup"><span data-stu-id="de0ad-357">More Details on Cross-Page Postbacks</span></span>

### <a name="viewstate"></a><span data-ttu-id="de0ad-358">Stan widoku</span><span class="sxs-lookup"><span data-stu-id="de0ad-358">Viewstate</span></span>

<span data-ttu-id="de0ad-359">Może mieć monit samodzielnie już o co się dzieje z viewstate od pierwszej strony w przypadku zwrotu między stronami.</span><span class="sxs-lookup"><span data-stu-id="de0ad-359">You may have asked yourself already about what happens to the viewstate from the first page in a cross-page postback scenario.</span></span> <span data-ttu-id="de0ad-360">Gdy wszystkie żadnego formantu, który nie implementuje element IPostBackDataHandler utrwali stanie za pośrednictwem stanu widoku, dlatego mają mieć dostęp do właściwości tej kontrolki na drugiej stronie ogłaszania zwrotnego między stronami, musi mieć dostęp do stanu widoku strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-360">After all, any control that does not implement IPostBackDataHandler will persist its state via viewstate, so to have access to the properties of that control on the second page of a cross-page postback, you must have access to the viewstate for the page.</span></span> <span data-ttu-id="de0ad-361">Program ASP.NET 2.0 zajmuje się ten scenariusz przy użyciu nowego pola ukryte na drugiej stronie o nazwie \_ \_PREVIOUSPAGE.</span><span class="sxs-lookup"><span data-stu-id="de0ad-361">ASP.NET 2.0 takes care of this scenario using a new hidden field in the second page called \_\_PREVIOUSPAGE.</span></span> <span data-ttu-id="de0ad-362">\_ \_PREVIOUSPAGE pole formularza zawiera viewstate dla pierwszej strony, dzięki czemu mają dostęp do właściwości wszystkich kontrolek na drugiej stronie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-362">The \_\_PREVIOUSPAGE form field contains the viewstate for the first page so that you can have access to the properties of all controls in the second page.</span></span>

### <a name="circumventing-findcontrol"></a><span data-ttu-id="de0ad-363">Obejście FindControl</span><span class="sxs-lookup"><span data-stu-id="de0ad-363">Circumventing FindControl</span></span>

<span data-ttu-id="de0ad-364">Przewodnik wideo ogłaszania zwrotnego między stronami czy używana metoda FindControl można pobrać odwołania do kontrolki pola tekstowego na pierwszej stronie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-364">In the video walkthrough of a cross-page postback, I used the FindControl method to get a reference to the TextBox control on the first page.</span></span> <span data-ttu-id="de0ad-365">Ta metoda sprawdza się w przypadku tego celu, ale FindControl jest kosztowne i wymaga on tworzenia dodatkowego kodu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-365">That method works well for that purpose, but FindControl is expensive and it requires writing additional code.</span></span> <span data-ttu-id="de0ad-366">Na szczęście ASP.NET 2.0 stanowi alternatywę dla FindControl w tym celu, który będzie działać w wielu scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="de0ad-366">Fortunately, ASP.NET 2.0 provides an alternative to FindControl for this purpose that will work in many scenarios.</span></span> <span data-ttu-id="de0ad-367">Dyrektywa PreviousPageType umożliwia silnie typizowane odwoływałby się do poprzedniej strony za pomocą TypeName lub atrybut VirtualPath.</span><span class="sxs-lookup"><span data-stu-id="de0ad-367">The PreviousPageType directive allows you to have a strongly-typed reference to the previous page by using either the TypeName or the VirtualPath attribute.</span></span> <span data-ttu-id="de0ad-368">Atrybut TypeName umożliwia określenie typu poprzedniej strony, gdy atrybut VirtualPath zezwala na odnoszenie się do poprzedniej strony, przy użyciu ścieżki wirtualnej.</span><span class="sxs-lookup"><span data-stu-id="de0ad-368">The TypeName attribute allows you to specify the type of the previous page while the VirtualPath attribute allows you to refer to the previous page using a virtual path.</span></span> <span data-ttu-id="de0ad-369">Po ustawieniu dyrektywy PreviousPageType, należy następnie udostępnić formantów, itp., do której chcesz zezwolić na dostęp przy użyciu właściwości publiczne.</span><span class="sxs-lookup"><span data-stu-id="de0ad-369">After you've set the PreviousPageType directive, you must then expose the controls, etc. to which you want to allow access using public properties.</span></span>

## <a name="lab-1-cross-page-postback"></a><span data-ttu-id="de0ad-370">Ogłaszania zwrotnego między stronami laboratorium 1</span><span class="sxs-lookup"><span data-stu-id="de0ad-370">Lab 1 Cross-Page Postback</span></span>

<span data-ttu-id="de0ad-371">W tym środowisku laboratoryjnym utworzysz aplikację, która korzysta z nowych funkcji zwrotu między stronami ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="de0ad-371">In this lab, you will create an application that uses the new cross-page postback functionality of ASP.NET 2.0.</span></span>

1. <span data-ttu-id="de0ad-372">Otwórz program Visual Studio 2005 i Utwórz nową witrynę sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="de0ad-372">Open Visual Studio 2005 and create a new ASP.NET Web site.</span></span>
2. <span data-ttu-id="de0ad-373">Dodaj nowy formularz sieci Web o nazwie page2.aspx.</span><span class="sxs-lookup"><span data-stu-id="de0ad-373">Add a new Webform called page2.aspx.</span></span>
3. <span data-ttu-id="de0ad-374">Otwórz Default.aspx w widoku Projekt i Dodaj formant przycisku i formant pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="de0ad-374">Open the Default.aspx in Design view and add a Button control and a TextBox control.</span></span> 

    1. <span data-ttu-id="de0ad-375">Nadaj formant przycisku o identyfikatorze **SubmitButton** i pole tekstowe kontrolować identyfikator **UserName**.</span><span class="sxs-lookup"><span data-stu-id="de0ad-375">Give the Button control an ID of **SubmitButton** and the TextBox control an ID of **UserName**.</span></span>
    2. <span data-ttu-id="de0ad-376">Ustaw właściwość PostBackUrl przycisku page2.aspx.</span><span class="sxs-lookup"><span data-stu-id="de0ad-376">Set the PostBackUrl property of the Button to page2.aspx.</span></span>
4. <span data-ttu-id="de0ad-377">Otwórz page2.aspx w widoku źródła.</span><span class="sxs-lookup"><span data-stu-id="de0ad-377">Open page2.aspx in Source view.</span></span>
5. <span data-ttu-id="de0ad-378">Dodaj dyrektywy @ PreviousPageType, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="de0ad-378">Add a @ PreviousPageType directive as shown below:</span></span>
6. <span data-ttu-id="de0ad-379">Dodaj następujący kod do strony\_obciążenia firmy page2.aspx związane z kodem:</span><span class="sxs-lookup"><span data-stu-id="de0ad-379">Add the following code to the Page\_Load of page2.aspx's code-behind:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. <span data-ttu-id="de0ad-380">Skompiluj projekt, klikając menu kompilacji podczas kompilacji.</span><span class="sxs-lookup"><span data-stu-id="de0ad-380">Build the project by clicking on Build on the Build menu.</span></span>
8. <span data-ttu-id="de0ad-381">Dodaj następujący kod do kodem Default.aspx:</span><span class="sxs-lookup"><span data-stu-id="de0ad-381">Add the following code to the code-behind for Default.aspx:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. <span data-ttu-id="de0ad-382">Zmiany strony\_obciążenia w page2.aspx do następującego:</span><span class="sxs-lookup"><span data-stu-id="de0ad-382">Change the Page\_Load in page2.aspx to the following:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. <span data-ttu-id="de0ad-383">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="de0ad-383">Build the project.</span></span>
11. <span data-ttu-id="de0ad-384">Uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="de0ad-384">Run the project.</span></span>
12. <span data-ttu-id="de0ad-385">Wprowadź nazwę w polu tekstowym, a następnie kliknij przycisk.</span><span class="sxs-lookup"><span data-stu-id="de0ad-385">Enter your name in the TextBox and click the button.</span></span>
13. <span data-ttu-id="de0ad-386">Co to jest działanie?</span><span class="sxs-lookup"><span data-stu-id="de0ad-386">What is the result?</span></span>

## <a name="asynchronous-pages-in-aspnet-20"></a><span data-ttu-id="de0ad-387">Asynchroniczne stron w programie ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="de0ad-387">Asynchronous Pages in ASP.NET 2.0</span></span>

<span data-ttu-id="de0ad-388">Wiele problemów rywalizacji o zasoby na platformie ASP.NET są spowodowane przez opóźnienia połączeniami zewnętrznymi (np. wywołań usługi lub bazy danych w sieci Web), czas oczekiwania operacji We/Wy plików itp. Po wysłaniu żądania w odniesieniu do aplikacji platformy ASP.NET, ASP.NET używa jednego z jego wątków roboczych do obsługi tego żądania.</span><span class="sxs-lookup"><span data-stu-id="de0ad-388">Many contention problems in ASP.NET are caused by latency of external calls (such as Web service or database calls), file IO latency, etc. When a request is made against an ASP.NET application, ASP.NET uses one of its worker threads to service that request.</span></span> <span data-ttu-id="de0ad-389">To żądanie jest właścicielem tego wątku zakończenie żądanie i odpowiedź została wysłana.</span><span class="sxs-lookup"><span data-stu-id="de0ad-389">That request owns that thread until the request is complete and the response has been sent.</span></span> <span data-ttu-id="de0ad-390">Program ASP.NET 2.0 ma na celu rozwiązania problemów z opóźnieniem przy użyciu tego rodzaju problemy, dodając możliwość strony są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-390">ASP.NET 2.0 seeks to resolve latency issues with these types of issues by adding the capability to execute pages asynchronously.</span></span> <span data-ttu-id="de0ad-391">Oznacza to, czy wątku roboczego można uruchomić żądania, a następnie przekazać dodatkowe wykonywania do innego wątku, w tym samym szybko zwracać do puli dostępnych wątków.</span><span class="sxs-lookup"><span data-stu-id="de0ad-391">That means that a worker thread can start the request and then hand off additional execution to another thread, thereby returning to the available thread pool quickly.</span></span> <span data-ttu-id="de0ad-392">Po ukończeniu operacji We/Wy w pliku, wywołanie bazy danych itp. nowy wątek jest uzyskiwana z puli wątków, aby zakończyć żądania.</span><span class="sxs-lookup"><span data-stu-id="de0ad-392">When the file IO, database call, etc. has completed, a new thread is obtained from the thread pool to finish the request.</span></span>

<span data-ttu-id="de0ad-393">Pierwszym krokiem w podejmowaniu strony są wykonywane asynchronicznie jest można ustawić **Async** atrybut strony dyrektyw w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="de0ad-393">The first step in making a page execute asynchronously is to set the **Async** attribute of the page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

<span data-ttu-id="de0ad-394">Ten atrybut informuje platformę ASP.NET w celu zaimplementowania IHttpAsyncHandler dla strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-394">This attribute tells ASP.NET to implement the IHttpAsyncHandler for the page.</span></span>

<span data-ttu-id="de0ad-395">Następnym krokiem jest wywołać metodę AddOnPreRenderCompleteAsync w punkcie w cyklu życia strony przed PreRender.</span><span class="sxs-lookup"><span data-stu-id="de0ad-395">The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender.</span></span> <span data-ttu-id="de0ad-396">(Ta metoda jest zwykle nazywany na stronie\_obciążenia.) Metoda AddOnPreRenderCompleteAsync przyjmuje dwa parametry; BeginEventHandler i EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="de0ad-396">(This method is typically called in Page\_Load.) The AddOnPreRenderCompleteAsync method takes two parameters; a BeginEventHandler and an EndEventHandler.</span></span> <span data-ttu-id="de0ad-397">BeginEventHandler zwraca obiekt IAsyncResult, który jest następnie przekazywany jako parametr EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="de0ad-397">The BeginEventHandler returns an IAsyncResult which is then passed as a parameter to the EndEventHandler.</span></span>

<span data-ttu-id="de0ad-398">Poniższy film jest przewodnik żądania asynchroniczne strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-398">The video below is a walkthrough of an asynchronous page request.</span></span>


![](the-asp-net-2-0-page-model/_static/image3.png)


[<span data-ttu-id="de0ad-399">Otwórz wideo pełnego ekranu</span><span class="sxs-lookup"><span data-stu-id="de0ad-399">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> <span data-ttu-id="de0ad-400">Na stronie async nie renderowane w przeglądarce, dopóki nie zakończy się EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="de0ad-400">An async page does not render to the browser until the EndEventHandler has completed.</span></span> <span data-ttu-id="de0ad-401">Bez wątpienia, ale niektórzy deweloperzy będą traktować żądań asynchronicznych jako zbliżone do wywołania zwrotne async.</span><span class="sxs-lookup"><span data-stu-id="de0ad-401">No doubt but that some developers will think of async requests as being similar to async callbacks.</span></span> <span data-ttu-id="de0ad-402">Należy weź pod uwagę, że nie są one.</span><span class="sxs-lookup"><span data-stu-id="de0ad-402">It's important to realize that they are not.</span></span> <span data-ttu-id="de0ad-403">Korzyścią dla żądań asynchronicznych jest, że pierwszym wątku roboczego mogą być zwrócone do puli wątków do obsługi nowych żądań, zmniejszając w ten sposób rywalizacji o zasoby z powodu trwa operacji We/Wy, powiązany, etc.</span><span class="sxs-lookup"><span data-stu-id="de0ad-403">The benefit to asynchronous requests is that the first worker thread can be returned to the thread pool to service new requests, thereby reducing contention due to being IO bound, etc.</span></span>


## <a name="script-callbacks-in-aspnet-20"></a><span data-ttu-id="de0ad-404">Wywołania zwrotne skryptu, na platformie ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="de0ad-404">Script Callbacks in ASP.NET 2.0</span></span>

<span data-ttu-id="de0ad-405">Deweloperzy sieci Web zawsze sprawdzono, sposobów zapobiec migotanie skojarzonych z wywołaniem zwrotnym.</span><span class="sxs-lookup"><span data-stu-id="de0ad-405">Web developers have always looked for ways to prevent the flickering associated with a callback.</span></span> <span data-ttu-id="de0ad-406">W programie ASP.NET: 1.x, SmartNavigation był najbardziej typowa metoda unikanie migotanie, ale SmartNavigation przyczyną problemów dla deweloperów, niektóre ze względu na złożoność implementacji na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="de0ad-406">In ASP.NET 1.x, SmartNavigation was the most common method for avoiding flickering, but SmartNavigation caused problems for some developers because of the complexity of its implementation on the client.</span></span> <span data-ttu-id="de0ad-407">Program ASP.NET 2.0 rozwiązuje ten problem za pomocą wywołania zwrotne skryptu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-407">ASP.NET 2.0 addresses this issue with script callbacks.</span></span> <span data-ttu-id="de0ad-408">Wywołania zwrotne skryptu wykorzystywać XMLHttp żądań na serwerze sieci Web przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="de0ad-408">Script callbacks utilize XMLHttp to make requests against the Web server via JavaScript.</span></span> <span data-ttu-id="de0ad-409">Żądanie XMLHttp zwraca dane XML, które następnie mogą być zmieniane za pomocą przeglądarki modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="de0ad-409">The XMLHttp request returns XML data that can then be manipulated via the browser's DOM.</span></span> <span data-ttu-id="de0ad-410">Kod XMLHttp są ukryte przed użytkownikiem przez nowy program obsługi WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="de0ad-410">XMLHttp code is hidden from the user by the new WebResource.axd handler.</span></span>

<span data-ttu-id="de0ad-411">Istnieje kilka kroków, które są niezbędne w celu skonfigurowania wywołanie zwrotne skryptu w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="de0ad-411">There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.</span></span>

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a><span data-ttu-id="de0ad-412">Krok 1: Implementowanie interfejsu ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="de0ad-412">Step 1 : Implement the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="de0ad-413">Aby ASP.NET rozpoznaje strony jako udział w wywołaniu zwrotnym skryptu musi implementować interfejs ICallbackEventHandler.</span><span class="sxs-lookup"><span data-stu-id="de0ad-413">In order for ASP.NET to recognize your page as participating in a script callback, you must implement the ICallbackEventHandler interface.</span></span> <span data-ttu-id="de0ad-414">Można to zrobić w pliku związanym z kodem w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="de0ad-414">You can do this in your code-behind file like so:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

<span data-ttu-id="de0ad-415">Możesz też to zrobić przy użyciu notacji dyrektywy @ implementuje, więc:</span><span class="sxs-lookup"><span data-stu-id="de0ad-415">You can also do this using the @ Implements directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

<span data-ttu-id="de0ad-416">Zazwyczaj używasz dyrektywa @ Implements, korzystając z wbudowanego kodu platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="de0ad-416">You would typically use the @ Implements directive when using inline ASP.NET code.</span></span>

## <a name="step-2--call-getcallbackeventreference"></a><span data-ttu-id="de0ad-417">Krok 2: Wywołanie GetCallbackEventReference</span><span class="sxs-lookup"><span data-stu-id="de0ad-417">Step 2 : Call GetCallbackEventReference</span></span>

<span data-ttu-id="de0ad-418">Jak wspomniano wcześniej, wywołanie XMLHttp jest hermetyzowany w obsłudze WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="de0ad-418">As mentioned previously, the XMLHttp call is encapsulated in the WebResource.axd handler.</span></span> <span data-ttu-id="de0ad-419">Po wyrenderowaniu strony ASP.NET spowoduje to dodanie wywołanie formularz sieci Web\_DoCallback, dostarczone przez WebResource.axd skryptu klienta.</span><span class="sxs-lookup"><span data-stu-id="de0ad-419">When your page is rendered, ASP.NET will add a call to WebForm\_DoCallback, a client script that is provided by WebResource.axd.</span></span> <span data-ttu-id="de0ad-420">Formularz sieci Web\_DoCallBack — funkcja zastępuje \_ \_doPostBack funkcji wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="de0ad-420">The WebForm\_DoCallback function replaces the \_\_doPostBack function for a callback.</span></span> <span data-ttu-id="de0ad-421">Należy pamiętać, że \_ \_doPostBack programowo przesyła formularz na stronie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-421">Remember that \_\_doPostBack programmatically submits the form on the page.</span></span> <span data-ttu-id="de0ad-422">W przypadku wywołania zwrotnego chcesz uniemożliwić odświeżenie strony, dlatego \_ \_doPostBack nie jest wystarczające.</span><span class="sxs-lookup"><span data-stu-id="de0ad-422">In a callback scenario, you want to prevent a postback, so \_\_doPostBack will not suffice.</span></span>

> [!NOTE]
> <span data-ttu-id="de0ad-423">\_\_doPostBack była nadal renderowana do strony w przypadku wywołania zwrotnego skryptu klienta.</span><span class="sxs-lookup"><span data-stu-id="de0ad-423">\_\_doPostBack is still rendered to the page in a client script callback scenario.</span></span> <span data-ttu-id="de0ad-424">Nie jest on jednak używany wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="de0ad-424">However, it's not used for the callback.</span></span>


<span data-ttu-id="de0ad-425">Argumenty dla formularza sieci Web\_DoCallback, funkcja po stronie klienta są udostępniane za pośrednictwem funkcji po stronie serwera GetCallbackEventReference, która zwykle będzie wywoływana na stronie\_obciążenia.</span><span class="sxs-lookup"><span data-stu-id="de0ad-425">The arguments for the WebForm\_DoCallback client-side function are provided via the server-side function GetCallbackEventReference which would normally be called in Page\_Load.</span></span> <span data-ttu-id="de0ad-426">Typowe wywołanie GetCallbackEventReference może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="de0ad-426">A typical call to GetCallbackEventReference might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="de0ad-427">W tym przypadku cm jest wystąpieniem ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="de0ad-427">In this case, cm is an instance of ClientScriptManager.</span></span> <span data-ttu-id="de0ad-428">Klasa ClientScriptManager zostały omówione w dalszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-428">The ClientScriptManager class will be covered later in this module.</span></span>


<span data-ttu-id="de0ad-429">Istnieje kilka przeciążone wersje GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="de0ad-429">There are several overloaded versions of GetCallbackEventReference.</span></span> <span data-ttu-id="de0ad-430">W tym przypadku argumenty są następujące:</span><span class="sxs-lookup"><span data-stu-id="de0ad-430">In this case, the arguments are as follows:</span></span>

`this`

<span data-ttu-id="de0ad-431">Odwołanie do formantu, gdzie jest wywoływana GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="de0ad-431">A reference to the control where GetCallbackEventReference is being called.</span></span> <span data-ttu-id="de0ad-432">W tym przypadku jest sama strona.</span><span class="sxs-lookup"><span data-stu-id="de0ad-432">In this case, it's the page itself.</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

<span data-ttu-id="de0ad-433">Argument ciągu, który zostanie przekazany z kodu po stronie klienta do zdarzenia po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="de0ad-433">A string argument that will be passed from the client-side code to the server-side event.</span></span> <span data-ttu-id="de0ad-434">W tym przypadku wiadomości błyskawicznych, przekazując wartość z listy rozwijanej o nazwie ddlCompany.</span><span class="sxs-lookup"><span data-stu-id="de0ad-434">In this case, Im passing the value of a dropdown called ddlCompany.</span></span>

`ShowCompanyName`

<span data-ttu-id="de0ad-435">Nazwa funkcji po stronie klienta, która będzie akceptować wartości zwracanej (jako ciąg) ze zdarzenia wywołania zwrotnego po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="de0ad-435">The name of the client-side function that will accept the return value (as string) from the server-side callback event.</span></span> <span data-ttu-id="de0ad-436">Ta funkcja będzie wywoływana tylko, po pomyślnym zakończeniu operacji wywołania zwrotnego po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="de0ad-436">This function will only be called when the server-side callback is successful.</span></span> <span data-ttu-id="de0ad-437">W związku ze względu na niezawodność, ogólnie zaleca się korzystać z przeciążoną wersją GetCallbackEventReference, która przyjmuje argument dodatkowy ciąg określający nazwę funkcję po stronie klienta do wykonania w przypadku wystąpienia błędu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-437">Therefore, for the sake of robustness, it is generally recommended to use the overloaded version of GetCallbackEventReference that takes an additional string argument specifying the name of a client-side function to execute in the event of an error.</span></span>

`null`

<span data-ttu-id="de0ad-438">Ciąg reprezentujący funkcji po stronie klienta, który je zainicjował przed wywołania zwrotnego do serwera.</span><span class="sxs-lookup"><span data-stu-id="de0ad-438">A string representing a client-side function that it initiated before the callback to the server.</span></span> <span data-ttu-id="de0ad-439">W tym przypadku nie jest bez skryptu, argument ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="de0ad-439">In this case, there is no such script, so the argument is null.</span></span>

`true`

<span data-ttu-id="de0ad-440">Wartość logiczna określająca, czy należy przeprowadzić asynchroniczne wywołanie zwrotne.</span><span class="sxs-lookup"><span data-stu-id="de0ad-440">A Boolean specifying whether or not to conduct the callback asynchronously.</span></span>

<span data-ttu-id="de0ad-441">Wywołanie metody formularza sieci Web\_DoCallback na komputerze klienckim przekazują tych argumentów.</span><span class="sxs-lookup"><span data-stu-id="de0ad-441">The call to WebForm\_DoCallback on the client will pass these arguments.</span></span> <span data-ttu-id="de0ad-442">W związku z tym, gdy ta strona jest wyświetlana na komputerze klienckim, ten kod będzie wyglądać w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="de0ad-442">Therefore, when this page is rendered on the client, that code will look like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

<span data-ttu-id="de0ad-443">Należy zauważyć, że podpis funkcji na komputerze klienckim jest nieco inna.</span><span class="sxs-lookup"><span data-stu-id="de0ad-443">Notice that the signature of the function on the client is a bit different.</span></span> <span data-ttu-id="de0ad-444">Funkcja klienta przekazuje 5 parametrów i wartość logiczną.</span><span class="sxs-lookup"><span data-stu-id="de0ad-444">The client-side function passes 5 strings and a Boolean.</span></span> <span data-ttu-id="de0ad-445">Dodatkowe parametry, (który ma wartość null w powyższym przykładzie) zawiera funkcję po stronie klienta, która będzie obsługiwać ewentualne błędy z wywołania zwrotnego po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="de0ad-445">The additional string (which is null in the above example) contains the client-side function that will handle any errors from the server-side callback.</span></span>

## <a name="step-3--hook-the-client-side-control-event"></a><span data-ttu-id="de0ad-446">Krok 3: Obsługiwać zdarzenia formantu po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="de0ad-446">Step 3 : Hook the Client-Side Control Event</span></span>

<span data-ttu-id="de0ad-447">Należy zauważyć, że wartość zwracana przez GetCallbackEventReference powyżej został przypisany do zmiennej ciągu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-447">Notice that the return value of GetCallbackEventReference above was assigned to a string variable.</span></span> <span data-ttu-id="de0ad-448">Ten ciąg jest używany do podłączania zdarzeń po stronie klienta, dla formantu, który inicjuje wywołanie zwrotne.</span><span class="sxs-lookup"><span data-stu-id="de0ad-448">That string is used to hook a client-side event for the control that initiates the callback.</span></span> <span data-ttu-id="de0ad-449">W tym przykładzie wywołanie zwrotne jest inicjowane przez listę rozwijaną na stronie tak, chcę dołączyć *OnChange* zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="de0ad-449">In this example, the callback is initiated by a dropdown on the page, so I want to hook the *OnChange* event.</span></span>

<span data-ttu-id="de0ad-450">Aby obsługiwać zdarzenia po stronie klienta, po prostu Dodaj program obsługi znaczników po stronie klienta w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="de0ad-450">To hook the client-side event, simply add a handler to the client-side markup as follows:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

<span data-ttu-id="de0ad-451">Pamiętamy *cbRef* jest wartością zwracaną z wywołania GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="de0ad-451">Recall that *cbRef* is the return value from the call to GetCallbackEventReference.</span></span> <span data-ttu-id="de0ad-452">Zawiera wywołanie metody formularza sieci Web\_DoCallback, który został wyświetlony powyżej.</span><span class="sxs-lookup"><span data-stu-id="de0ad-452">It contains the call to WebForm\_DoCallback that was shown above.</span></span>

## <a name="step-4--register-the-client-side-script"></a><span data-ttu-id="de0ad-453">Krok 4: Zarejestruj skryptu po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="de0ad-453">Step 4 : Register the Client-Side Script</span></span>

<span data-ttu-id="de0ad-454">Odwołania, które wywołanie GetCallbackEventReference określone, czy skrypt po stronie klienta o nazwie **ShowCompanyName** będzie wykonywany po pomyślnym zakończeniu wywołania zwrotnego po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="de0ad-454">Recall that the call to GetCallbackEventReference specified that a client-side script called **ShowCompanyName** would be executed when the server-side callback succeeds.</span></span> <span data-ttu-id="de0ad-455">Czy skrypt musi zostać dodane do strony, przy użyciu wystąpienia ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="de0ad-455">That script needs to be added to the page using a ClientScriptManager instance.</span></span> <span data-ttu-id="de0ad-456">(W klasie ClientScriptManager będzie dicussed w dalszej części tego modułu). Dlatego należy wykonać podobne do tego:</span><span class="sxs-lookup"><span data-stu-id="de0ad-456">(The ClientScriptManager class will be dicussed later in this module.) You do that like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a><span data-ttu-id="de0ad-457">Krok 5: Wywołanie metody interfejsu ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="de0ad-457">Step 5 : Call the Methods of the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="de0ad-458">ICallbackEventHandler zawiera dwie metody, które są potrzebne do zaimplementowania w kodzie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-458">The ICallbackEventHandler contains two methods that you need to implement in your code.</span></span> <span data-ttu-id="de0ad-459">Są one **RaiseCallbackEvent** i **GetCallbackEvent**.</span><span class="sxs-lookup"><span data-stu-id="de0ad-459">They are **RaiseCallbackEvent** and **GetCallbackEvent**.</span></span>

<span data-ttu-id="de0ad-460">**RaiseCallbackEvent** przyjmuje ciąg jako argument i zwraca nothing.</span><span class="sxs-lookup"><span data-stu-id="de0ad-460">**RaiseCallbackEvent** takes a string as an argument and returns nothing.</span></span> <span data-ttu-id="de0ad-461">Argument ciągu jest przekazywany z wywołania po stronie klienta, aby formularz sieci Web\_DoCallback.</span><span class="sxs-lookup"><span data-stu-id="de0ad-461">The string argument is passed from the client-side call to WebForm\_DoCallback.</span></span> <span data-ttu-id="de0ad-462">W takim przypadku ta wartość jest *wartość* atrybutu z listy rozwijanej o nazwie ddlCompany.</span><span class="sxs-lookup"><span data-stu-id="de0ad-462">In this case, that value is the *value* attribute of the dropdown called ddlCompany.</span></span> <span data-ttu-id="de0ad-463">Kod po stronie serwera, należy umieścić w metodzie RaiseCallbackEvent.</span><span class="sxs-lookup"><span data-stu-id="de0ad-463">Your server-side code should be placed in the RaiseCallbackEvent method.</span></span> <span data-ttu-id="de0ad-464">Na przykład jeśli wywołanie zwrotne WebRequest względem zewnętrznego zasobu, ten kod będzie umieszczona w RaiseCallbackEvent.</span><span class="sxs-lookup"><span data-stu-id="de0ad-464">For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.</span></span>

<span data-ttu-id="de0ad-465">**GetCallbackEvent** jest odpowiedzialna za przetwarzanie zwracana wywołania zwrotnego do klienta.</span><span class="sxs-lookup"><span data-stu-id="de0ad-465">**GetCallbackEvent** is responsible for processing the return of the callback to the client.</span></span> <span data-ttu-id="de0ad-466">Go nie przyjmuje żadnych argumentów i zwraca wartość typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="de0ad-466">It takes no arguments and returns a string.</span></span> <span data-ttu-id="de0ad-467">Ciąg, który zwraca zostaną przekazane jako argument do funkcji po stronie klienta, w tym przypadku *ShowCompanyName*.</span><span class="sxs-lookup"><span data-stu-id="de0ad-467">The string that it returns will be passed as an argument to the client-side function, in this case *ShowCompanyName*.</span></span>

<span data-ttu-id="de0ad-468">Po wykonaniu powyższych czynności można przystąpić do wykonania skryptu wywołania zwrotnego w programie ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="de0ad-468">Once you have completed the above steps, you are ready to perform a script callback in ASP.NET 2.0.</span></span>


![](the-asp-net-2-0-page-model/_static/image4.png)


[<span data-ttu-id="de0ad-469">Otwórz wideo pełnego ekranu</span><span class="sxs-lookup"><span data-stu-id="de0ad-469">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/callback1.wmv)


<span data-ttu-id="de0ad-470">Wywołania zwrotne skryptu, na platformie ASP.NET są obsługiwane w dowolnej przeglądarki obsługującej wprowadzania wywołania XMLHttp.</span><span class="sxs-lookup"><span data-stu-id="de0ad-470">Script callbacks in ASP.NET are supported in any browser that supports making XMLHttp calls.</span></span> <span data-ttu-id="de0ad-471">Obejmuje to wszystkie nowoczesne przeglądarki używana już dziś.</span><span class="sxs-lookup"><span data-stu-id="de0ad-471">That includes all of the modern browsers in use today.</span></span> <span data-ttu-id="de0ad-472">Program Internet Explorer używa obiektu XMLHttp ActiveX, podczas gdy inne nowoczesnych przeglądarek (w tym nadchodzących programu Internet Explorer 7) używają wewnętrzne obiektu XMLHttp.</span><span class="sxs-lookup"><span data-stu-id="de0ad-472">Internet Explorer uses the XMLHttp ActiveX object while other modern browsers (including the upcoming IE 7) use an intrinsic XMLHttp object.</span></span> <span data-ttu-id="de0ad-473">Aby programowo określić, jeśli przeglądarka obsługuje wywołania zwrotne, możesz użyć **Request.Browser.SupportCallback** właściwości.</span><span class="sxs-lookup"><span data-stu-id="de0ad-473">To programmatically determine if a browser supports callbacks, you can use the **Request.Browser.SupportCallback** property.</span></span> <span data-ttu-id="de0ad-474">Właściwość ta zwróci **true** Jeśli żądanie klienta obsługuje wywołania zwrotne skryptu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-474">This property will return **true** if the requesting client supports script callbacks.</span></span>

## <a name="working-with-client-script-in-aspnet-20"></a><span data-ttu-id="de0ad-475">Praca ze skryptem klienta w programie ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="de0ad-475">Working with Client Script in ASP.NET 2.0</span></span>

<span data-ttu-id="de0ad-476">Skrypty klienta w programie ASP.NET 2.0 są zarządzane przez użycie klasy ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="de0ad-476">Client scripts in ASP.NET 2.0 are managed via the use of the ClientScriptManager class.</span></span> <span data-ttu-id="de0ad-477">Klasa ClientScriptManager przechowuje informacje o skryptów klienta przy użyciu typu i nazwy.</span><span class="sxs-lookup"><span data-stu-id="de0ad-477">The ClientScriptManager class keeps track of client scripts using a type and a name.</span></span> <span data-ttu-id="de0ad-478">Zapobiega to programowo wstawiane na stronie więcej niż jeden raz przez tego samego skryptu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-478">This prevents the same script from being programmatically inserted on a page more than once.</span></span>

> [!NOTE]
> <span data-ttu-id="de0ad-479">Po skrypt został pomyślnie zarejestrowany na stronie, żadne kolejne próby, aby zarejestrować ten sam skrypt powoduje po prostu skrypt nie jest zarejestrowany po raz drugi.</span><span class="sxs-lookup"><span data-stu-id="de0ad-479">After a script has been successfully registered on a page, any subsequent attempt to register the same script will simply result in the script not being registered a second time.</span></span> <span data-ttu-id="de0ad-480">Nie zduplikowane skryptów są dodawane, a nie wystąpi wyjątek.</span><span class="sxs-lookup"><span data-stu-id="de0ad-480">No duplicate scripts are added and no exception occurs.</span></span> <span data-ttu-id="de0ad-481">Aby uniknąć niepotrzebnych obliczeń, istnieją metody, które można użyć w celu ustalenia, czy skrypt został już zarejestrowany, tak aby nie należy go zarejestrować więcej niż jeden raz.</span><span class="sxs-lookup"><span data-stu-id="de0ad-481">To avoid unnecessary computation, there are methods that you can use to determine if a script is already registered so that you do not attempt to register it more than once.</span></span>


<span data-ttu-id="de0ad-482">Metody ClientScriptManager powinny być znane wszystkie bieżące deweloperów platformy ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="de0ad-482">The methods of the ClientScriptManager should be familiar to all current ASP.NET developers:</span></span>

## <a name="registerclientscriptblock"></a><span data-ttu-id="de0ad-483">RegisterClientScriptBlock</span><span class="sxs-lookup"><span data-stu-id="de0ad-483">RegisterClientScriptBlock</span></span>

<span data-ttu-id="de0ad-484">Metoda ta umożliwia dodanie skryptu na początku renderowanej strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-484">This method adds a script to the top of the rendered page.</span></span> <span data-ttu-id="de0ad-485">Jest to przydatne, aby dodać funkcje, które będą wywoływane jawnie na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="de0ad-485">This is useful for adding functions that will be explicitly called on the client.</span></span>

<span data-ttu-id="de0ad-486">Istnieją dwie przeciążone wersje tej metody.</span><span class="sxs-lookup"><span data-stu-id="de0ad-486">There are two overloaded versions of this method.</span></span> <span data-ttu-id="de0ad-487">Trzy z czterech argumentów występują między nimi.</span><span class="sxs-lookup"><span data-stu-id="de0ad-487">Three of four arguments are common among them.</span></span> <span data-ttu-id="de0ad-488">Są to:</span><span class="sxs-lookup"><span data-stu-id="de0ad-488">They are:</span></span>

`type (string)`

<span data-ttu-id="de0ad-489">***Typu*** argument określa typ skryptu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-489">The ***type*** argument identifies a type for the script.</span></span> <span data-ttu-id="de0ad-490">Zazwyczaj jest to dobry pomysł, aby użyć typu strony, (to. GetType()) dla typu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-490">It is generally a good idea to use the page's type (this.GetType()) for the type.</span></span>

`key (string)`

<span data-ttu-id="de0ad-491">***Klucz*** argument jest zdefiniowane przez użytkownika klucza dla skryptu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-491">The ***key*** argument is a user-defined key for the script.</span></span> <span data-ttu-id="de0ad-492">Powinna to być unikatowy dla każdego skryptu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-492">This should be unique for each script.</span></span> <span data-ttu-id="de0ad-493">Jeśli spróbujesz dodać skrypt za pomocą tego samego klucza i typ skryptu dodane wcześniej, nie można dodać.</span><span class="sxs-lookup"><span data-stu-id="de0ad-493">If you attempt to add a script with the same key and type of an already added script, it will not be added.</span></span>

`script (string)`

<span data-ttu-id="de0ad-494">***Skryptu*** argument jest ciąg zawierający rzeczywiste skryptu, aby dodać.</span><span class="sxs-lookup"><span data-stu-id="de0ad-494">The ***script*** argument is a string containing the actual script to add.</span></span> <span data-ttu-id="de0ad-495">Zalecane jest, użyj klasy StringBuilder można utworzyć skrypt, a następnie zastosować metodę ToString() StringBuilder można przypisać ***skryptu*** argumentu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-495">It's recommended that you use a StringBuilder to create the script and then use the ToString() method on the StringBuilder to assign the ***script*** argument.</span></span>

<span data-ttu-id="de0ad-496">Jeśli używasz RegisterClientScriptBlock przeciążona, który tylko przyjmuje trzy argumenty, musi zawierać elementy skryptu (&lt;skryptu&gt; i &lt;/script&gt;) w skrypcie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-496">If you use the overloaded RegisterClientScriptBlock that only takes three arguments, you must include script elements (&lt;script&gt; and &lt;/script&gt;) in your script.</span></span>

<span data-ttu-id="de0ad-497">Można użyć przeciążenia RegisterClientScriptBlock, która przyjmuje czwarty argument.</span><span class="sxs-lookup"><span data-stu-id="de0ad-497">You may choose to use the overload of RegisterClientScriptBlock that takes a fourth argument.</span></span> <span data-ttu-id="de0ad-498">Czwarty argument jest wartość logiczna określająca, czy ASP.NET, należy dodać elementy skryptu dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-498">The fourth argument is a Boolean that specifies whether or not ASP.NET should add script elements for you.</span></span> <span data-ttu-id="de0ad-499">Jeśli ten argument jest **true**, skrypt nie może zawierać elementy skryptu jawnie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-499">If this argument is **true**, your script should not include the script elements explicitly.</span></span>

<span data-ttu-id="de0ad-500">Należy użyć metody IsClientScriptBlockRegistered, aby określić, jeśli skrypt został już zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="de0ad-500">Use the IsClientScriptBlockRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="de0ad-501">Dzięki temu można uniknąć próba ponownego zarejestrowania skrypt, który został już zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="de0ad-501">This allows you to avoid an attempt to re-register a script that has already been registered.</span></span>

### <a name="registerclientscriptinclude-new-in-20"></a><span data-ttu-id="de0ad-502">RegisterClientScriptInclude (Nowość w wersji 2.0)</span><span class="sxs-lookup"><span data-stu-id="de0ad-502">RegisterClientScriptInclude (New in 2.0)</span></span>

<span data-ttu-id="de0ad-503">RegisterClientScriptInclude tag tworzy blok skryptu, który stanowi łącze do zewnętrznego pliku skryptu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-503">The RegisterClientScriptInclude tag creates a script block that links to an external script file.</span></span> <span data-ttu-id="de0ad-504">Ma dwa przeciążenia.</span><span class="sxs-lookup"><span data-stu-id="de0ad-504">It has two overloads.</span></span> <span data-ttu-id="de0ad-505">Przyjmuje jeden klucz i adres URL.</span><span class="sxs-lookup"><span data-stu-id="de0ad-505">One takes a key and a URL.</span></span> <span data-ttu-id="de0ad-506">Drugi dodaje trzeci argument określenie typu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-506">The second adds a third argument specifying the type.</span></span>

<span data-ttu-id="de0ad-507">Na przykład, poniższy kod generuje blok skryptu zawierającego łącze do jsfunctions.js w katalogu głównym folderu skryptów aplikacji:</span><span class="sxs-lookup"><span data-stu-id="de0ad-507">For example, the following code generates a script block that links to jsfunctions.js in the root of the scripts folder of the application:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

<span data-ttu-id="de0ad-508">Ten kod tworzy następujący kod na renderowanej stronie:</span><span class="sxs-lookup"><span data-stu-id="de0ad-508">This code produces the following code in the rendered page:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> <span data-ttu-id="de0ad-509">Blok skryptu jest renderowany w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="de0ad-509">The script block is rendered at the bottom of the page.</span></span>


<span data-ttu-id="de0ad-510">Należy użyć metody IsClientScriptIncludeRegistered, aby określić, jeśli skrypt został już zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="de0ad-510">Use the IsClientScriptIncludeRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="de0ad-511">Dzięki temu można uniknąć próba ponownego zarejestrowania skrypt.</span><span class="sxs-lookup"><span data-stu-id="de0ad-511">This allows you to avoid an attempt to re-register a script.</span></span>

## <a name="registerstartupscript"></a><span data-ttu-id="de0ad-512">RegisterStartupScript</span><span class="sxs-lookup"><span data-stu-id="de0ad-512">RegisterStartupScript</span></span>

<span data-ttu-id="de0ad-513">Metoda RegisterStartupScript przyjmuje te same argumenty co metoda RegisterClientScriptBlock.</span><span class="sxs-lookup"><span data-stu-id="de0ad-513">The RegisterStartupScript method takes the same arguments as the RegisterClientScriptBlock method.</span></span> <span data-ttu-id="de0ad-514">Zarejestrowane w usłudze RegisterStartupScript skrypt wykonuje po załadowaniu strony, ale przed zdarzeniem OnLoad po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="de0ad-514">A script registered with RegisterStartupScript executes after the page loads but before the OnLoad client-side event.</span></span> <span data-ttu-id="de0ad-515">W 1.X, zarejestrowane w usłudze RegisterStartupScript skrypty zostały umieszczone bezpośrednio przed zamykającym &lt;/form&gt; tagów podczas zarejestrowane w usłudze RegisterClientScriptBlock skrypty zostały umieszczone bezpośrednio po otwarciu &lt;formularza&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-515">In 1.X, scripts registered with RegisterStartupScript were placed just before the closing &lt;/form&gt; tag while scripts registered with RegisterClientScriptBlock were placed immediately after the opening &lt;form&gt; tag.</span></span> <span data-ttu-id="de0ad-516">W programie ASP.NET 2.0, oba są umieszczane bezpośrednio przed tagiem zamykającym &lt;/form&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-516">In ASP.NET 2.0, both are placed immediately before the closing &lt;/form&gt; tag.</span></span>

> [!NOTE]
> <span data-ttu-id="de0ad-517">Jeśli zarejestrowane przez funkcję RegisterStartupScript, tej funkcji nie będą wykonywane, dopóki nie zostanie jawnie wywołana w kodzie po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="de0ad-517">If you register a function with RegisterStartupScript, that function will not execute until you explicitly call it in client-side code.</span></span>


<span data-ttu-id="de0ad-518">Aby określić, jeśli skrypt został już zarejestrowany i uniknąć próba ponownego zarejestrowania skrypt, należy użyć metody IsStartupScriptRegistered.</span><span class="sxs-lookup"><span data-stu-id="de0ad-518">Use the IsStartupScriptRegistered method to determine if a script has already been registered and avoid an attempt to re-register a script.</span></span>

## <a name="other-clientscriptmanager-methods"></a><span data-ttu-id="de0ad-519">Inne metody ClientScriptManager</span><span class="sxs-lookup"><span data-stu-id="de0ad-519">Other ClientScriptManager Methods</span></span>

<span data-ttu-id="de0ad-520">Poniżej przedstawiono niektóre inne użyteczne metody klasy ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="de0ad-520">Here are some of the other useful methods of the ClientScriptManager class.</span></span>


|  <span data-ttu-id="de0ad-521"><strong>GetCallbackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="de0ad-521"><strong>GetCallbackEventReference</strong></span></span>   |                                                 <span data-ttu-id="de0ad-522">Zobacz wywołania zwrotne skryptu, we wcześniejszej części tego modułu.</span><span class="sxs-lookup"><span data-stu-id="de0ad-522">See script callbacks earlier in this module.</span></span>                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <span data-ttu-id="de0ad-523"><strong>GetPostBackClientHyperlink</strong></span><span class="sxs-lookup"><span data-stu-id="de0ad-523"><strong>GetPostBackClientHyperlink</strong></span></span>  |                <span data-ttu-id="de0ad-524">Pobiera odniesienie JavaScript (javascript:&lt;wywołania&gt;) który może służyć do opublikowania powrót po awarii z zdarzenia po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="de0ad-524">Gets a JavaScript reference (javascript:&lt;call&gt;) that can be used to post back from a client-side event.</span></span>                 |
|  <span data-ttu-id="de0ad-525"><strong>GetPostBackEventReference</strong></span><span class="sxs-lookup"><span data-stu-id="de0ad-525"><strong>GetPostBackEventReference</strong></span></span>   |                                   <span data-ttu-id="de0ad-526">Pobiera ciąg, który może służyć do inicjowania wpis przez klienta.</span><span class="sxs-lookup"><span data-stu-id="de0ad-526">Gets a string that can be used to initiate a post back from the client.</span></span>                                    |
|      <span data-ttu-id="de0ad-527"><strong>GetWebResourceUrl</strong></span><span class="sxs-lookup"><span data-stu-id="de0ad-527"><strong>GetWebResourceUrl</strong></span></span>       | <span data-ttu-id="de0ad-528">Zwraca adres URL do zasobu, który jest osadzony w zestawie.</span><span class="sxs-lookup"><span data-stu-id="de0ad-528">Returns a URL to a resource that is embedded in an assembly.</span></span> <span data-ttu-id="de0ad-529">Należy używać w połączeniu z <strong>RegisterClientScriptResource</strong>.</span><span class="sxs-lookup"><span data-stu-id="de0ad-529">Must be used in conjunction with <strong>RegisterClientScriptResource</strong>.</span></span> |
| <span data-ttu-id="de0ad-530"><strong>RegisterClientScriptResource</strong></span><span class="sxs-lookup"><span data-stu-id="de0ad-530"><strong>RegisterClientScriptResource</strong></span></span> |     <span data-ttu-id="de0ad-531">Rejestruje zasobu sieci Web ze stroną.</span><span class="sxs-lookup"><span data-stu-id="de0ad-531">Registers a Web resource with the page.</span></span> <span data-ttu-id="de0ad-532">Są to zasoby osadzone w zestawie i obsługiwane przez nowy program obsługi WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="de0ad-532">These are resources embedded in an assembly and handled by the new WebResource.axd handler.</span></span>      |
|     <span data-ttu-id="de0ad-533"><strong>RegisterHiddenField</strong></span><span class="sxs-lookup"><span data-stu-id="de0ad-533"><strong>RegisterHiddenField</strong></span></span>      |                                                 <span data-ttu-id="de0ad-534">Rejestruje ukryte pole formularza ze stroną.</span><span class="sxs-lookup"><span data-stu-id="de0ad-534">Registers a hidden form field with the page.</span></span>                                                 |
|  <span data-ttu-id="de0ad-535"><strong>RegisterOnSubmitStatement</strong></span><span class="sxs-lookup"><span data-stu-id="de0ad-535"><strong>RegisterOnSubmitStatement</strong></span></span>   |                                  <span data-ttu-id="de0ad-536">Rejestruje kod po stronie klienta, który jest wykonywany po przesłaniu formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="de0ad-536">Registers client-side code that executes when the HTML form is submitted.</span></span>                                   |

