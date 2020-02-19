---
title: Scenariusze ASP.NET Core Blazor Advanced
author: guardrex
description: Dowiedz się więcej na temat zaawansowanych scenariuszy w Blazor, w tym sposobu włączania ręcznej logiki RenderTreeBuilder do aplikacji.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5e0618faa7b1b5e4cc15e30d9c16afaf7ccabaf0
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453262"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a><span data-ttu-id="bd2ea-103">Zaawansowane scenariusze ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="bd2ea-103">ASP.NET Core Blazor advanced scenarios</span></span>

<span data-ttu-id="bd2ea-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="bd2ea-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="bd2ea-105">Ręczna logika RenderTreeBuilder</span><span class="sxs-lookup"><span data-stu-id="bd2ea-105">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="bd2ea-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` udostępnia metody manipulowania składnikami i elementami, w tym ręczne Kompilowanie C# składników w kodzie.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="bd2ea-107">Użycie `RenderTreeBuilder` do tworzenia składników jest zaawansowanym scenariuszem.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-107">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="bd2ea-108">Nieprawidłowo sformułowany składnik (na przykład niezamknięty tag znacznika) może spowodować niezdefiniowane zachowanie.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-108">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="bd2ea-109">Rozważmy następujący składnik `PetDetails`, który można utworzyć ręcznie w innym składniku:</span><span class="sxs-lookup"><span data-stu-id="bd2ea-109">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="bd2ea-110">W poniższym przykładzie pętla w metodzie `CreateComponent` generuje trzy składniki `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-110">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="bd2ea-111">Podczas wywoływania `RenderTreeBuilder` metod tworzenia składników (`OpenComponent` i `AddAttribute`) numery sekwencji są numerami wierszy kodu źródłowego.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-111">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="bd2ea-112">Algorytm Blazor różnica polega na numerach sekwencji odpowiadających odrębnym wierszom kodu, a nie odrębnym wywoływaniu wywołań.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-112">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="bd2ea-113">Podczas tworzenia składnika przy użyciu metod `RenderTreeBuilder` umieszczaj argumenty dla numerów sekwencji.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-113">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="bd2ea-114">**Użycie obliczenia lub licznika do wygenerowania numeru sekwencji może prowadzić do niskiej wydajności.**</span><span class="sxs-lookup"><span data-stu-id="bd2ea-114">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="bd2ea-115">Aby uzyskać więcej informacji, zobacz sekcję [numery sekwencji powiązane z numerami wierszy kodu i kolejnością niewykonania](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="bd2ea-115">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="bd2ea-116">składnik `BuiltContent`:</span><span class="sxs-lookup"><span data-stu-id="bd2ea-116">`BuiltContent` component:</span></span>

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> <span data-ttu-id="bd2ea-117">Typy w `Microsoft.AspNetCore.Components.RenderTree` umożliwiają przetwarzanie *wyników* operacji renderowania.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-117">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="bd2ea-118">Są to wewnętrzne szczegóły implementacji platformy Blazor Framework.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-118">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="bd2ea-119">Te typy powinny być uznawane za *niestabilne* i mogą ulec zmianie w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-119">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="bd2ea-120">Numery sekwencji odnoszą się do numerów wierszy kodu, a nie kolejności wykonywania</span><span class="sxs-lookup"><span data-stu-id="bd2ea-120">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="bd2ea-121">Pliki składników Razor ( *. Razor*) są zawsze kompilowane.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-121">Razor component files (*.razor*) are always compiled.</span></span> <span data-ttu-id="bd2ea-122">Kompilacja jest potencjalną możliwością przekroczenia interpretacji kodu, ponieważ krok kompilacji może służyć do iniekcji informacji, które zwiększają wydajność aplikacji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-122">Compilation is a potential advantage over interpreting code because the compile step can be used to inject information that improves app performance at runtime.</span></span>

<span data-ttu-id="bd2ea-123">Najważniejszym przykładem tych ulepszeń są *numery sekwencji*.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-123">A key example of these improvements involves *sequence numbers*.</span></span> <span data-ttu-id="bd2ea-124">Numery sekwencji wskazują na środowisko uruchomieniowe, które pochodzą z różnych i uporządkowanych wierszy kodu.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-124">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="bd2ea-125">Środowisko uruchomieniowe używa tych informacji do generowania wydajnych różnic drzewa w czasie liniowym, które są znacznie szybsze niż zwykle jest to możliwe dla algorytmu różnicowego drzewa ogólnego.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-125">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="bd2ea-126">Rozważmy następujący plik składnika Razor (*Razor*):</span><span class="sxs-lookup"><span data-stu-id="bd2ea-126">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="bd2ea-127">Poprzedni kod kompiluje się w taki sposób, aby wyglądał następująco:</span><span class="sxs-lookup"><span data-stu-id="bd2ea-127">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="bd2ea-128">Gdy kod jest wykonywany po raz pierwszy, jeśli `someFlag` jest `true`, Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="bd2ea-128">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="bd2ea-129">Sequence</span><span class="sxs-lookup"><span data-stu-id="bd2ea-129">Sequence</span></span> | <span data-ttu-id="bd2ea-130">Typ</span><span class="sxs-lookup"><span data-stu-id="bd2ea-130">Type</span></span>      | <span data-ttu-id="bd2ea-131">Dane</span><span class="sxs-lookup"><span data-stu-id="bd2ea-131">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="bd2ea-132">0</span><span class="sxs-lookup"><span data-stu-id="bd2ea-132">0</span></span>        | <span data-ttu-id="bd2ea-133">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="bd2ea-133">Text node</span></span> | <span data-ttu-id="bd2ea-134">Pierwszego</span><span class="sxs-lookup"><span data-stu-id="bd2ea-134">First</span></span>  |
| <span data-ttu-id="bd2ea-135">1</span><span class="sxs-lookup"><span data-stu-id="bd2ea-135">1</span></span>        | <span data-ttu-id="bd2ea-136">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="bd2ea-136">Text node</span></span> | <span data-ttu-id="bd2ea-137">Sekunda</span><span class="sxs-lookup"><span data-stu-id="bd2ea-137">Second</span></span> |

<span data-ttu-id="bd2ea-138">Załóżmy, że `someFlag` `false`, a znaczniki są renderowane ponownie.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-138">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="bd2ea-139">Tym razem Konstruktor odbiera:</span><span class="sxs-lookup"><span data-stu-id="bd2ea-139">This time, the builder receives:</span></span>

| <span data-ttu-id="bd2ea-140">Sequence</span><span class="sxs-lookup"><span data-stu-id="bd2ea-140">Sequence</span></span> | <span data-ttu-id="bd2ea-141">Typ</span><span class="sxs-lookup"><span data-stu-id="bd2ea-141">Type</span></span>       | <span data-ttu-id="bd2ea-142">Dane</span><span class="sxs-lookup"><span data-stu-id="bd2ea-142">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="bd2ea-143">1</span><span class="sxs-lookup"><span data-stu-id="bd2ea-143">1</span></span>        | <span data-ttu-id="bd2ea-144">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="bd2ea-144">Text node</span></span>  | <span data-ttu-id="bd2ea-145">Sekunda</span><span class="sxs-lookup"><span data-stu-id="bd2ea-145">Second</span></span> |

<span data-ttu-id="bd2ea-146">Gdy środowisko uruchomieniowe wykonuje różnicę, zobaczy, że element w sekwencji `0` został usunięty, więc generuje następujący skrypt uproszczonej *edycji*:</span><span class="sxs-lookup"><span data-stu-id="bd2ea-146">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="bd2ea-147">Usuń pierwszy węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-147">Remove the first text node.</span></span>

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a><span data-ttu-id="bd2ea-148">Problem z programowo generowanymi numerami sekwencji</span><span class="sxs-lookup"><span data-stu-id="bd2ea-148">The problem with generating sequence numbers programmatically</span></span>

<span data-ttu-id="bd2ea-149">Wyobraź sobie, że została zapisana następująca logika konstruktora drzewa renderowania:</span><span class="sxs-lookup"><span data-stu-id="bd2ea-149">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="bd2ea-150">Teraz pierwsze dane wyjściowe to:</span><span class="sxs-lookup"><span data-stu-id="bd2ea-150">Now, the first output is:</span></span>

| <span data-ttu-id="bd2ea-151">Sequence</span><span class="sxs-lookup"><span data-stu-id="bd2ea-151">Sequence</span></span> | <span data-ttu-id="bd2ea-152">Typ</span><span class="sxs-lookup"><span data-stu-id="bd2ea-152">Type</span></span>      | <span data-ttu-id="bd2ea-153">Dane</span><span class="sxs-lookup"><span data-stu-id="bd2ea-153">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="bd2ea-154">0</span><span class="sxs-lookup"><span data-stu-id="bd2ea-154">0</span></span>        | <span data-ttu-id="bd2ea-155">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="bd2ea-155">Text node</span></span> | <span data-ttu-id="bd2ea-156">Pierwszego</span><span class="sxs-lookup"><span data-stu-id="bd2ea-156">First</span></span>  |
| <span data-ttu-id="bd2ea-157">1</span><span class="sxs-lookup"><span data-stu-id="bd2ea-157">1</span></span>        | <span data-ttu-id="bd2ea-158">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="bd2ea-158">Text node</span></span> | <span data-ttu-id="bd2ea-159">Sekunda</span><span class="sxs-lookup"><span data-stu-id="bd2ea-159">Second</span></span> |

<span data-ttu-id="bd2ea-160">Ten wynik jest identyczny z poprzednim przypadkiem, dlatego nie istnieją żadne negatywne problemy.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-160">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="bd2ea-161">`someFlag` jest `false` podczas drugiego renderowania, a dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="bd2ea-161">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="bd2ea-162">Sequence</span><span class="sxs-lookup"><span data-stu-id="bd2ea-162">Sequence</span></span> | <span data-ttu-id="bd2ea-163">Typ</span><span class="sxs-lookup"><span data-stu-id="bd2ea-163">Type</span></span>      | <span data-ttu-id="bd2ea-164">Dane</span><span class="sxs-lookup"><span data-stu-id="bd2ea-164">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="bd2ea-165">0</span><span class="sxs-lookup"><span data-stu-id="bd2ea-165">0</span></span>        | <span data-ttu-id="bd2ea-166">Węzeł tekstu</span><span class="sxs-lookup"><span data-stu-id="bd2ea-166">Text node</span></span> | <span data-ttu-id="bd2ea-167">Sekunda</span><span class="sxs-lookup"><span data-stu-id="bd2ea-167">Second</span></span> |

<span data-ttu-id="bd2ea-168">Tym razem algorytm diff widzi, że pojawiły się *dwie* zmiany, a algorytm generuje następujący skrypt edycji:</span><span class="sxs-lookup"><span data-stu-id="bd2ea-168">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="bd2ea-169">Zmień wartość pierwszego węzła tekstowego na `Second`.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-169">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="bd2ea-170">Usuń drugi węzeł tekstu.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-170">Remove the second text node.</span></span>

<span data-ttu-id="bd2ea-171">Generowanie numerów sekwencji utraciło wszystkie przydatne informacje o tym, gdzie `if/else` gałęzie i pętle były obecne w oryginalnym kodzie.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-171">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="bd2ea-172">Wynikiem tego jest różnica **dwa razy** , tak długo, jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-172">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="bd2ea-173">Jest to prosty przykład.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-173">This is a trivial example.</span></span> <span data-ttu-id="bd2ea-174">W bardziej realistycznych przypadkach ze złożonymi i głęboko zagnieżdżonymi strukturami, w szczególności z pętlami, koszt wydajności jest zwykle wyższy.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-174">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is usually higher.</span></span> <span data-ttu-id="bd2ea-175">Zamiast natychmiastowego identyfikowania, które bloki lub gałęzie pętli zostały wstawione lub usunięte, algorytm diff musi odnosił się do drzewa renderowania.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-175">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees.</span></span> <span data-ttu-id="bd2ea-176">Zwykle polega to na konieczności kompilowania dłużej edytowanych skryptów, ponieważ algorytm różnicowy jest niewiadome o tym, jak stare i nowe struktury odnoszą się do siebie.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-176">This usually results in having to build longer edit scripts because the diff algorithm is misinformed about how the old and new structures relate to each other.</span></span>

### <a name="guidance-and-conclusions"></a><span data-ttu-id="bd2ea-177">Wskazówki i wnioski</span><span class="sxs-lookup"><span data-stu-id="bd2ea-177">Guidance and conclusions</span></span>

* <span data-ttu-id="bd2ea-178">Wydajność aplikacji ma wpływ na to, że numery sekwencji są generowane dynamicznie.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-178">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="bd2ea-179">Struktura nie może automatycznie tworzyć własnych numerów sekwencji w czasie wykonywania, ponieważ niezbędne informacje nie istnieją, chyba że są przechwytywane w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-179">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="bd2ea-180">Nie zapisuj długich bloków ręcznie zaimplementowanej logiki `RenderTreeBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-180">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="bd2ea-181">Preferuj pliki *Razor* i Zezwalaj kompilatorowi na rozpatrywanie numerów sekwencyjnych.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-181">Prefer *.razor* files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="bd2ea-182">Jeśli nie możesz uniknąć ręcznej logiki `RenderTreeBuilder`, Podziel długie bloki kodu na mniejsze fragmenty opakowane w `OpenRegion`/`CloseRegion` wywołania.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-182">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="bd2ea-183">Każdy region ma własne oddzielne miejsce numerów sekwencyjnych, więc można uruchomić ponownie od zera (lub dowolnego innego numeru) w każdym regionie.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-183">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="bd2ea-184">Jeśli numery sekwencji są stałee, algorytm diff wymaga tylko zwiększenia wartości sekwencji.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-184">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="bd2ea-185">Początkowa wartość i przerwy są nieistotne.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-185">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="bd2ea-186">Jedną z wiarygodnych opcji jest użycie numeru wiersza kodu jako numeru sekwencyjnego lub rozpoczęcie od zera i zwiększenie według wartości lub setek (lub dowolnego preferowanego interwału).</span><span class="sxs-lookup"><span data-stu-id="bd2ea-186">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="bd2ea-187"> używa numerów sekwencji, podczas gdy inne struktury interfejsu użytkownika rozróżniania drzewa nie są używane.</span><span class="sxs-lookup"><span data-stu-id="bd2ea-187"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="bd2ea-188">Różnica jest znacznie szybsza, gdy są używane numery sekwencji, a Blazor ma zalety kroku kompilacji, który zajmuje się automatycznie numerami sekwencyjnymi dla deweloperów tworzących pliki *. Razor* .</span><span class="sxs-lookup"><span data-stu-id="bd2ea-188">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>
