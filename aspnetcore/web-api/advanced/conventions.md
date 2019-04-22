---
title: Użyj interfejsu API sieci web Konwencji
author: pranavkm
description: Dowiedz się więcej na temat Konwencji interfejsu API sieci web w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 25e8d5209f02683c533ef7c316b91d447f1b20ba
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705461"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="e1e00-103">Użyj interfejsu API sieci web Konwencji</span><span class="sxs-lookup"><span data-stu-id="e1e00-103">Use web API conventions</span></span>

<span data-ttu-id="e1e00-104">Przez [Pranav Krishnamoorthy](https://github.com/pranavkm) i [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="e1e00-104">By [Pranav Krishnamoorthy](https://github.com/pranavkm) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="e1e00-105">ASP.NET Core 2,2 lub nowszym zawiera sposób wyodrębniania typowe [dokumentacji interfejsu API](xref:tutorials/web-api-help-pages-using-swagger) i zastosować je do wielu akcji kontrolerów i wszystkich kontrolerów w zestawie.</span><span class="sxs-lookup"><span data-stu-id="e1e00-105">ASP.NET Core 2.2 and later includes a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="e1e00-106">Konwencje interfejsu API sieci Web są stanowi zastępstwa dla dekoracji poszczególne akcje za pomocą [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span><span class="sxs-lookup"><span data-stu-id="e1e00-106">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span>

<span data-ttu-id="e1e00-107">Konwencję umożliwia:</span><span class="sxs-lookup"><span data-stu-id="e1e00-107">A convention allows you to:</span></span>

* <span data-ttu-id="e1e00-108">Zdefiniuj najczęściej używane typy zwracane i kodów stanu zwrócony z określonego typu działania.</span><span class="sxs-lookup"><span data-stu-id="e1e00-108">Define the most common return types and status codes returned from a specific type of action.</span></span>
* <span data-ttu-id="e1e00-109">Zidentyfikuj akcji odbiegających od zdefiniowany standard.</span><span class="sxs-lookup"><span data-stu-id="e1e00-109">Identify actions that deviate from the defined standard.</span></span>

<span data-ttu-id="e1e00-110">Platforma ASP.NET Core MVC 2,2 lub nowszym zawiera zestaw domyślnych Konwencji w <xref:Microsoft.AspNetCore.Mvc.DefaultApiConventions?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="e1e00-110">ASP.NET Core MVC 2.2 and later includes a set of default conventions in <xref:Microsoft.AspNetCore.Mvc.DefaultApiConventions?displayProperty=fullName>.</span></span> <span data-ttu-id="e1e00-111">Konwencje są oparte na kontrolerze (*ValuesController.cs*) podany w programie ASP.NET Core **API** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="e1e00-111">The conventions are based on the controller (*ValuesController.cs*) provided in the ASP.NET Core **API** project template.</span></span> <span data-ttu-id="e1e00-112">Jeśli Twoje działania wzorców w szablonie, powinno być pomyślnego używania domyślnych Konwencji.</span><span class="sxs-lookup"><span data-stu-id="e1e00-112">If your actions follow the patterns in the template, you should be successful using the default conventions.</span></span> <span data-ttu-id="e1e00-113">Jeśli domyślnych Konwencji nie odpowiada Twoim potrzebom, zobacz [utworzyć internetowy interfejs API konwencje](#create-web-api-conventions).</span><span class="sxs-lookup"><span data-stu-id="e1e00-113">If the default conventions don't meet your needs, see [Create web API conventions](#create-web-api-conventions).</span></span>

<span data-ttu-id="e1e00-114">W czasie wykonywania <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> rozumie Konwencji.</span><span class="sxs-lookup"><span data-stu-id="e1e00-114">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="e1e00-115">`ApiExplorer` to Abstrakcja MVC do komunikowania się z [OpenAPI](https://www.openapis.org/) (Swagger) generatorów dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e1e00-115">`ApiExplorer` is MVC's abstraction to communicate with [OpenAPI](https://www.openapis.org/) (also known as Swagger) document generators.</span></span> <span data-ttu-id="e1e00-116">Atrybuty z Konwencji stosowane są skojarzone z akcją i znajdują się w dokumentacji interfejsu OpenAPI akcji.</span><span class="sxs-lookup"><span data-stu-id="e1e00-116">Attributes from the applied convention are associated with an action and are included in the action's OpenAPI documentation.</span></span> <span data-ttu-id="e1e00-117">[Interfejs API analizatorów](xref:web-api/advanced/analyzers) również zrozumienie Konwencji.</span><span class="sxs-lookup"><span data-stu-id="e1e00-117">[API analyzers](xref:web-api/advanced/analyzers) also understand conventions.</span></span> <span data-ttu-id="e1e00-118">Jeśli Twoja akcja jest nietypowe (na przykład zwraca kod stanu, który nie jest udokumentowany zgodnie z Konwencją stosowane), ostrzeżenie zachęca do dokumentowania kodu stanu.</span><span class="sxs-lookup"><span data-stu-id="e1e00-118">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), a warning encourages you to document the status code.</span></span>

<span data-ttu-id="e1e00-119">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e1e00-119">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="e1e00-120">Zastosuj konwencje interfejsu API sieci web</span><span class="sxs-lookup"><span data-stu-id="e1e00-120">Apply web API conventions</span></span>

<span data-ttu-id="e1e00-121">Konwencje nie tworzą; Każde działanie może być skojarzony z dokładnie jednego Konwencji.</span><span class="sxs-lookup"><span data-stu-id="e1e00-121">Conventions don't compose; each action may be associated with exactly one convention.</span></span> <span data-ttu-id="e1e00-122">Bardziej szczegółowe konwencje mają pierwszeństwo przed mniej określonych konwencji.</span><span class="sxs-lookup"><span data-stu-id="e1e00-122">More specific conventions take precedence over less specific conventions.</span></span> <span data-ttu-id="e1e00-123">Zaznaczenie jest deterministyczna, gdy co najmniej dwóch Konwencji ten sam priorytet zastosować do akcji.</span><span class="sxs-lookup"><span data-stu-id="e1e00-123">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="e1e00-124">Istnieją następujące opcje, aby zastosować Konwencję do działania z najbardziej specyficznych do najmniej specyficznych:</span><span class="sxs-lookup"><span data-stu-id="e1e00-124">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="e1e00-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Ma zastosowanie do poszczególnych działań i określa typ Konwencji i metody Konwencji, która ma zastosowanie.</span><span class="sxs-lookup"><span data-stu-id="e1e00-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span>

    <span data-ttu-id="e1e00-126">W poniższym przykładzie, domyślny typ Konwencji firmy `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` metoda Konwencji jest stosowana do `Update` akcji:</span><span class="sxs-lookup"><span data-stu-id="e1e00-126">In the following example, the default convention type's `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    <span data-ttu-id="e1e00-127">`Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` Metoda Konwencji stosuje się następujące atrybuty do akcji:</span><span class="sxs-lookup"><span data-stu-id="e1e00-127">The `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method applies the following attributes to the action:</span></span>

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(StatusCodes.Status204NoContent)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    ```

<span data-ttu-id="e1e00-128">Aby uzyskać więcej informacji na temat `[ProducesDefaultResponseType]`, zobacz [odpowiedź domyślna](https://swagger.io/docs/specification/describing-responses/#default).</span><span class="sxs-lookup"><span data-stu-id="e1e00-128">For more information on `[ProducesDefaultResponseType]`, see [Default Response](https://swagger.io/docs/specification/describing-responses/#default).</span></span>

1. <span data-ttu-id="e1e00-129">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` stosowane do kontrolera &mdash; dotyczy typ określonej Konwencji wszystkich akcji w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="e1e00-129">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the specified convention type to all actions on the controller.</span></span> <span data-ttu-id="e1e00-130">Metoda Konwencji zostanie nadany wskazówek, które określają akcje, do których zostanie zastosowana metoda Konwencji.</span><span class="sxs-lookup"><span data-stu-id="e1e00-130">A convention method is decorated with hints that determine the actions to which the convention method applies.</span></span> <span data-ttu-id="e1e00-131">Aby uzyskać więcej informacji na temat wskazówek dotyczących serwerów, zobacz [utworzyć internetowy interfejs API konwencje](#create-web-api-conventions)).</span><span class="sxs-lookup"><span data-stu-id="e1e00-131">For more information on hints, see [Create web API conventions](#create-web-api-conventions)).</span></span>

    <span data-ttu-id="e1e00-132">W poniższym przykładzie na wybranie domyślnego zestawu Konwencji jest stosowana do wszystkich akcji w *ContactsConventionController*:</span><span class="sxs-lookup"><span data-stu-id="e1e00-132">In the following example, the default set of conventions is applied to all actions in *ContactsConventionController*:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. <span data-ttu-id="e1e00-133">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` zastosowany do zestawu &mdash; zastosowanie Konwencji określony typ do wszystkich kontrolerów w bieżącym zestawie.</span><span class="sxs-lookup"><span data-stu-id="e1e00-133">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the specified convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="e1e00-134">Jako zalecenie, Zastosuj atrybuty poziomu zestawu w *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="e1e00-134">As a recommendation, apply assembly-level attributes in the *Startup.cs* file.</span></span>

    <span data-ttu-id="e1e00-135">W poniższym przykładzie na wybranie domyślnego zestawu Konwencji jest stosowana do wszystkich kontrolerów w zestawie:</span><span class="sxs-lookup"><span data-stu-id="e1e00-135">In the following example, the default set of conventions is applied to all controllers in the assembly:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="e1e00-136">Tworzenie konwencje interfejsu API sieci web</span><span class="sxs-lookup"><span data-stu-id="e1e00-136">Create web API conventions</span></span>

<span data-ttu-id="e1e00-137">Jeśli domyślnych Konwencji interfejsu API nie odpowiada Twoim potrzebom, tworzenie własnych Konwencji odpowiadającym.</span><span class="sxs-lookup"><span data-stu-id="e1e00-137">If the default API conventions don't meet your needs, create your own conventions.</span></span> <span data-ttu-id="e1e00-138">Konwencja to:</span><span class="sxs-lookup"><span data-stu-id="e1e00-138">A convention is:</span></span>

* <span data-ttu-id="e1e00-139">Typu statycznego za pomocą metod.</span><span class="sxs-lookup"><span data-stu-id="e1e00-139">A static type with methods.</span></span>
* <span data-ttu-id="e1e00-140">Możliwość definiowania [typów odpowiedzi](#response-types) i [wymaganiami w zakresie nazewnictwa](#naming-requirements) na akcje.</span><span class="sxs-lookup"><span data-stu-id="e1e00-140">Capable of defining [response types](#response-types) and [naming requirements](#naming-requirements) on actions.</span></span>

### <a name="response-types"></a><span data-ttu-id="e1e00-141">Typy odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="e1e00-141">Response types</span></span>

<span data-ttu-id="e1e00-142">Te metody są oznaczony za pomocą `[ProducesResponseType]` lub `[ProducesDefaultResponseType]` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="e1e00-142">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span> <span data-ttu-id="e1e00-143">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e1e00-143">For example:</span></span>

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public static void Find(int id)
    {
    }
}
```

<span data-ttu-id="e1e00-144">W przypadku nieobecności dokładniejszą atrybuty metadanych na stosowanie niniejszej Konwencji, do zestawu wymusza który:</span><span class="sxs-lookup"><span data-stu-id="e1e00-144">If more specific metadata attributes are absent, applying this convention to an assembly enforces that:</span></span>

* <span data-ttu-id="e1e00-145">Metoda Konwencji odnosi się do dowolnej akcji o nazwie `Find`.</span><span class="sxs-lookup"><span data-stu-id="e1e00-145">The convention method applies to any action named `Find`.</span></span>
* <span data-ttu-id="e1e00-146">Parametr o nazwie `id` znajduje się na `Find` akcji.</span><span class="sxs-lookup"><span data-stu-id="e1e00-146">A parameter named `id` is present on the `Find` action.</span></span>

### <a name="naming-requirements"></a><span data-ttu-id="e1e00-147">Wymagania dotyczące nazewnictwa</span><span class="sxs-lookup"><span data-stu-id="e1e00-147">Naming requirements</span></span>

<span data-ttu-id="e1e00-148">`[ApiConventionNameMatch]` i `[ApiConventionTypeMatch]` atrybuty mogą być stosowane do metody Konwencji, która określa akcje, których dotyczą.</span><span class="sxs-lookup"><span data-stu-id="e1e00-148">The `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` attributes can be applied to the convention method that determines the actions to which they apply.</span></span> <span data-ttu-id="e1e00-149">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e1e00-149">For example:</span></span>

```csharp
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status404NotFound)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

<span data-ttu-id="e1e00-150">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="e1e00-150">In the preceding example:</span></span>

* <span data-ttu-id="e1e00-151">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` Opcja zastosowany do metody wskazuje, że Konwencji pasuje do żadnych działań z prefiksem "Znajdź".</span><span class="sxs-lookup"><span data-stu-id="e1e00-151">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method indicates that the convention matches any action prefixed with "Find".</span></span> <span data-ttu-id="e1e00-152">Przykłady działań pasujących `Find`, `FindPet`, i `FindById`.</span><span class="sxs-lookup"><span data-stu-id="e1e00-152">Examples of matching actions include `Find`, `FindPet`, and `FindById`.</span></span>
* <span data-ttu-id="e1e00-153">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` Zastosowany do parametru wskazuje Konwencji zgodność metody przy użyciu dokładnie jeden parametr końcówce identyfikatora sufiksu.</span><span class="sxs-lookup"><span data-stu-id="e1e00-153">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention matches methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="e1e00-154">Przykłady obejmują parametry `id` lub `petId`.</span><span class="sxs-lookup"><span data-stu-id="e1e00-154">Examples include parameters such as `id` or `petId`.</span></span> <span data-ttu-id="e1e00-155">`ApiConventionTypeMatch` można podobnie można zastosować do typów ograniczenie z typem parametru.</span><span class="sxs-lookup"><span data-stu-id="e1e00-155">`ApiConventionTypeMatch` can be similarly applied to types to constrain the parameter type.</span></span> <span data-ttu-id="e1e00-156">A `params[]` argument wskazuje pozostałe parametry, których nie trzeba jawnie można dopasować.</span><span class="sxs-lookup"><span data-stu-id="e1e00-156">A `params[]` argument indicates remaining parameters that don't need to be explicitly matched.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1e00-157">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e1e00-157">Additional resources</span></span>

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
