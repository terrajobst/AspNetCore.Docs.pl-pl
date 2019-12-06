---
title: Dokumentacja składni razor dla platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat składni znacznikowania Razor do osadzania kodu na serwerze do stron sieci Web.
ms.author: riande
ms.date: 11/09/2019
uid: mvc/views/razor
ms.openlocfilehash: a18fd2886124f186b8f35abf4b55a51f1f996958
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881012"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="fc1c7-103">Dokumentacja składni razor dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc1c7-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="fc1c7-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [MULLENA Taylora](https://twitter.com/ntaylormullen), i [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="fc1c7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="fc1c7-105">Razor jest znaczników składnię osadzania kodu na serwerze w stronach sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="fc1c7-106">Składnia Razor składa się z znaczników Razor C#, jak i HTML.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="fc1c7-107">Pliki zawierające Razor zazwyczaj mają *.cshtml* rozszerzenie pliku.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-107">Files containing Razor generally have a *.cshtml* file extension.</span></span> <span data-ttu-id="fc1c7-108">Razor również w plikach [składników Razor](xref:blazor/components) ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="fc1c7-108">Razor is also found in [Razor components](xref:blazor/components) files (*.razor*).</span></span>

## <a name="rendering-html"></a><span data-ttu-id="fc1c7-109">Renderowanie kodu HTML</span><span class="sxs-lookup"><span data-stu-id="fc1c7-109">Rendering HTML</span></span>

<span data-ttu-id="fc1c7-110">Domyślny język Razor ma format HTML.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-110">The default Razor language is HTML.</span></span> <span data-ttu-id="fc1c7-111">Renderowanie kodu HTML, z kodu znaczników Razor nie różni się od renderowania kodu HTML z pliku HTML.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="fc1c7-112">Kod znaczników HTML w *.cshtml* plikach Razor jest renderowany przez serwer bez zmian.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="fc1c7-113">Składnia Razor</span><span class="sxs-lookup"><span data-stu-id="fc1c7-113">Razor syntax</span></span>

<span data-ttu-id="fc1c7-114">Obsługuje razor C# i używa `@` symbol przejście z kodu HTML do C#.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="fc1c7-115">Ocenia razor C# wyrażeń i renderuje je w danych wyjściowych HTML.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="fc1c7-116">Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](#razor-reserved-keywords), przechodzi do znaczników specyficzne dla aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="fc1c7-117">W przeciwnym razie nastąpi przejście do zwykłego C#.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="fc1c7-118">Wyjścia `@` symboli w znacznikach Razor, należy użyć drugiej `@` symbol:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="fc1c7-119">Renderowanie kodu HTML za pomocą jednego `@` symbol:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="fc1c7-120">Atrybuty kodu HTML i zawartość zawierająca adresy e-mail nie traktują `@` symbol znaku przejścia.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="fc1c7-121">Adresy e-mail w poniższym przykładzie są niezmienione, analiza kodu Razor:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="fc1c7-122">Niejawne wyrażeń Razor</span><span class="sxs-lookup"><span data-stu-id="fc1c7-122">Implicit Razor expressions</span></span>

<span data-ttu-id="fc1c7-123">Niejawne wyrażeń Razor rozpoczynać `@` następuje C# kodu:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="fc1c7-124">Z wyjątkiem produktów C# `await` — słowo kluczowe, niejawnego wyrażenia nie może zawierać spacji.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="fc1c7-125">Jeśli C# instrukcji posiada wyraźne zakończenie, miejsca do magazynowania można są:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="fc1c7-126">Niejawne wyrażeń **nie** zawierają C# ogólne jako znaki w nawiasie (`<>`) są interpretowane jako potraktowane jak tag HTML.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="fc1c7-127">Poniższy kod jest **nie** prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="fc1c7-128">Powyższy kod generuje błąd kompilatora podobny do jednego z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-128">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="fc1c7-129">Element "int" nie został zamknięty.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-129">The "int" element wasn't closed.</span></span> <span data-ttu-id="fc1c7-130">Wszystkie elementy muszą być albo samozamykający lub ma zgodnego tagu końcowego.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-130">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="fc1c7-131">Nie można przekonwertować grupy metod "GenericMethod" na typ "object" Niedelegowany.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="fc1c7-132">Czy zamierzane było wywołanie metody? "</span><span class="sxs-lookup"><span data-stu-id="fc1c7-132">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="fc1c7-133">Wywołania metody ogólnej musi być ujęte w [wyrażenie jawne Razor](#explicit-razor-expressions) lub [blok kodu Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="fc1c7-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="fc1c7-134">Jawne Razor wyrażeń</span><span class="sxs-lookup"><span data-stu-id="fc1c7-134">Explicit Razor expressions</span></span>

<span data-ttu-id="fc1c7-135">Jawne Razor wyrażeń składają się z `@` symbol za pomocą nawiasów o zrównoważonym obciążeniu.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-135">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="fc1c7-136">Aby renderować czas ostatniego tygodnia, jest używany następujący kod Razor:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-136">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="fc1c7-137">Zawartości w obrębie `@()` nawiasów jest obliczane i renderowane w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-137">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="fc1c7-138">Niejawne wyrażeń, opisanego w poprzedniej sekcji, zazwyczaj nie może zawierać spacji.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-138">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="fc1c7-139">W poniższym kodzie jeden tydzień nie jest odejmowany od bieżącej godziny:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-139">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="fc1c7-140">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-140">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="fc1c7-141">Jawne wyrażeń może służyć do łączenia tekstu z wynikiem wyrażenia:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-141">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="fc1c7-142">Bez wyraźnej wyrażenia `<p>Age@joe.Age</p>` jest traktowany jak adres e-mail i `<p>Age@joe.Age</p>` jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-142">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="fc1c7-143">Podczas zapisywania jako wyrażenie jawne `<p>Age33</p>` jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-143">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="fc1c7-144">Jawne wyrażeń może zostać użyty do renderowania dane wyjściowe z metod ogólnych w *.cshtml* plików.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-144">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="fc1c7-145">Następujący kod pokazuje, jak rozwiązać błąd pokazany wcześniej spowodowane przez nawiasy z C# ogólnego.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-145">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="fc1c7-146">Kod jest zapisywany jako jawne wyrażenie:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-146">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="fc1c7-147">Wyrażenie kodowania</span><span class="sxs-lookup"><span data-stu-id="fc1c7-147">Expression encoding</span></span>

<span data-ttu-id="fc1c7-148">C#wyrażenia, które obliczają do ciągu są kodowania HTML.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-148">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="fc1c7-149">C#wyrażenia, które dają `IHtmlContent` — zostaną zrenderowane bezpośrednio za pomocą `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-149">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="fc1c7-150">C#wyrażenia, które nie zwrócą `IHtmlContent` są konwertowane na ciąg przez `ToString` i kodowany przed są one renderowane.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-150">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="fc1c7-151">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-151">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="fc1c7-152">Kod HTML jest wyświetlany w przeglądarce jako:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-152">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="fc1c7-153">`HtmlHelper.Raw` dane wyjściowe nie jest zakodowany, ale renderowany jako kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-153">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="fc1c7-154">Za pomocą `HtmlHelper.Raw` unsanitized użytkownika dane wejściowe to zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-154">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="fc1c7-155">Dane wejściowe użytkownika może zawierać złośliwego kodu JavaScript lub inne luki w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-155">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="fc1c7-156">Podczas czyszczenia danych wejściowych użytkownika jest trudne.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-156">Sanitizing user input is difficult.</span></span> <span data-ttu-id="fc1c7-157">Unikaj używania `HtmlHelper.Raw` z danymi wejściowymi użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-157">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="fc1c7-158">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-158">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="fc1c7-159">Bloki kodu razor</span><span class="sxs-lookup"><span data-stu-id="fc1c7-159">Razor code blocks</span></span>

<span data-ttu-id="fc1c7-160">Bloki kodu razor rozpoczynać `@` i są ujęte w `{}`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-160">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="fc1c7-161">W przeciwieństwie do wyrażenia C# kod wewnątrz bloków kodu nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-161">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="fc1c7-162">Bloki kodu i wyrażenia w widoku udostępnianie tego samego zakresu i są definiowane w kolejności:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-162">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

<span data-ttu-id="fc1c7-163">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-163">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fc1c7-164">W blokach kodu Zadeklaruj [funkcje lokalne](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) z adiustacją, aby zapewnić metody tworzenia szablonów:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-164">In code blocks, declare [local functions](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) with markup to serve as templating methods:</span></span>

```cshtml
@{
    void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }

    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}
```

<span data-ttu-id="fc1c7-165">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-165">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a><span data-ttu-id="fc1c7-166">Niejawne przejścia</span><span class="sxs-lookup"><span data-stu-id="fc1c7-166">Implicit transitions</span></span>

<span data-ttu-id="fc1c7-167">Jest to domyślny język, w bloku kodu C#, ale strona Razor można przejść do kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-167">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="fc1c7-168">Jawne przejście rozdzielany</span><span class="sxs-lookup"><span data-stu-id="fc1c7-168">Explicit delimited transition</span></span>

<span data-ttu-id="fc1c7-169">Aby zdefiniować podsekcję bloku kodu, który powinien renderować kod HTML, należy ująć znaki do renderowania za pomocą tagu `<text>` Razor:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-169">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor `<text>` tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="fc1c7-170">Tej metody można użyć do renderowania elementów HTML, który nie jest otoczony potraktowane jak tag HTML.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-170">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="fc1c7-171">Bez tagu HTML lub Razor występuje błąd w czasie wykonywania Razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-171">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="fc1c7-172">Tag `<text>` jest przydatny do kontrolowania odstępów podczas renderowania zawartości:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-172">The `<text>` tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="fc1c7-173">Tylko zawartość między tagiem `<text>` jest renderowana.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-173">Only the content between the `<text>` tag is rendered.</span></span>
* <span data-ttu-id="fc1c7-174">W danych wyjściowych HTML nie ma spacji przed ani po tagu `<text>`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-174">No whitespace before or after the `<text>` tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition"></a><span data-ttu-id="fc1c7-175">Jawne przejście liniowe</span><span class="sxs-lookup"><span data-stu-id="fc1c7-175">Explicit line transition</span></span>

<span data-ttu-id="fc1c7-176">Aby renderować resztę całego wiersza jako HTML wewnątrz bloku kodu, użyj składni `@:`:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-176">To render the rest of an entire line as HTML inside a code block, use `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="fc1c7-177">Bez `@:` w kodzie, jest generowany błąd czasu wykonywania Razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-177">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="fc1c7-178">Dodatkowe `@` znaki w pliku Razor mogą spowodować błędy kompilatora w instrukcjach znajdujących się w dalszej części bloku.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-178">Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="fc1c7-179">Te błędy kompilatora może być trudne do zrozumienia, ponieważ rzeczywista błąd wystąpi przed zgłoszonego błędu.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-179">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="fc1c7-180">Ten błąd jest wspólne po połączeniu wiele wyrażeń niejawnej/jawnej w jednym bloku kodu.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-180">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="fc1c7-181">Struktury sterujące</span><span class="sxs-lookup"><span data-stu-id="fc1c7-181">Control structures</span></span>

<span data-ttu-id="fc1c7-182">Struktury sterujące stanowią rozszerzenie bloków kodu.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-182">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="fc1c7-183">Wszystkie aspekty bloki kodu (przejście do znaczników, wbudowane C#) dotyczą również następujące struktury:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-183">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="fc1c7-184">Warunkowa \@IF, else i \@Switch</span><span class="sxs-lookup"><span data-stu-id="fc1c7-184">Conditionals \@if, else if, else, and \@switch</span></span>

<span data-ttu-id="fc1c7-185">`@if` formanty, gdy kod jest wykonywany:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-185">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="fc1c7-186">`else` i `else if` nie wymagają `@` symbol:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-186">`else` and `else if` don't require the `@` symbol:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

<span data-ttu-id="fc1c7-187">Następujący kod przedstawia sposób użycia instrukcji switch:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-187">The following markup shows how to use a switch statement:</span></span>

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="fc1c7-188">Zapętlenie \@dla, \@foreach, \@while, i \@do while</span><span class="sxs-lookup"><span data-stu-id="fc1c7-188">Looping \@for, \@foreach, \@while, and \@do while</span></span>

<span data-ttu-id="fc1c7-189">Oparte na szablonach HTML może być renderowany przy użyciu pętli instrukcji sterowania.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-189">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="fc1c7-190">Aby renderować listy osób:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-190">To render a list of people:</span></span>

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

<span data-ttu-id="fc1c7-191">Obsługiwane są następujące instrukcje pętli:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-191">The following looping statements are supported:</span></span>

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="fc1c7-192">\@złożone przy użyciu</span><span class="sxs-lookup"><span data-stu-id="fc1c7-192">Compound \@using</span></span>

<span data-ttu-id="fc1c7-193">W C#, `using` instrukcja jest używane, aby upewnić się, obiekt zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-193">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="fc1c7-194">W elemencie Razor ten sam mechanizm służy do tworzenia pomocników HTML, który zawiera dodatkową zawartość.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-194">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="fc1c7-195">W poniższym kodzie, pomocników HTML renderuje tag `<form>` przy użyciu instrukcji `@using`:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-195">In the following code, HTML Helpers render a `<form>` tag with the `@using` statement:</span></span>

```cshtml
@using (Html.BeginForm())
{
    <div>
        Email: <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

### <a name="try-catch-finally"></a><span data-ttu-id="fc1c7-196">\@Wypróbuj, catch, finally</span><span class="sxs-lookup"><span data-stu-id="fc1c7-196">\@try, catch, finally</span></span>

<span data-ttu-id="fc1c7-197">Obsługa wyjątków jest podobne do C#:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-197">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a><span data-ttu-id="fc1c7-198">Blokada \@</span><span class="sxs-lookup"><span data-stu-id="fc1c7-198">\@lock</span></span>

<span data-ttu-id="fc1c7-199">Razor ma możliwość ochrony sekcje krytyczne w instrukcjach blokady:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-199">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="fc1c7-200">Komentarze</span><span class="sxs-lookup"><span data-stu-id="fc1c7-200">Comments</span></span>

<span data-ttu-id="fc1c7-201">Obsługuje razor C# i komentarze HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-201">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="fc1c7-202">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-202">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="fc1c7-203">Komentarz razor są usuwane przez serwer przed wyświetleniem strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-203">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="fc1c7-204">Korzysta z aparatu razor `@*  *@` ograniczającej komentarzy.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-204">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="fc1c7-205">Poniższy kod jest zakomentowany, więc serwer nie renderuje żadnych znaczników:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-205">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="fc1c7-206">Dyrektyw</span><span class="sxs-lookup"><span data-stu-id="fc1c7-206">Directives</span></span>

<span data-ttu-id="fc1c7-207">Dyrektywy razor są reprezentowane przez wyrażenia niejawne następujących zastrzeżonych słów kluczowych `@` symboli.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-207">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="fc1c7-208">Dyrektywy zazwyczaj zmienia sposób, w widoku jest analizowany lub włącza inną funkcję.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-208">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="fc1c7-209">Zrozumienie, jak Razor generuje kod dla widoku ułatwia zrozumienie, jak działają dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-209">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="fc1c7-210">Kod generuje klasę podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-210">The code generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="fc1c7-211">W dalszej części tego artykułu sekcji [sprawdzić Razor C# klasy wygenerowanej w celu wyświetlenia](#inspect-the-razor-c-class-generated-for-a-view) opisano sposób wyświetlania tego wygenerowanej klasy.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-211">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="attribute"></a><span data-ttu-id="fc1c7-212">atrybut \@</span><span class="sxs-lookup"><span data-stu-id="fc1c7-212">\@attribute</span></span>

<span data-ttu-id="fc1c7-213">Dyrektywa `@attribute` dodaje dany atrybut do klasy wygenerowanej strony lub widoku.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-213">The `@attribute` directive adds the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="fc1c7-214">Poniższy przykład dodaje atrybut `[Authorize]`:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-214">The following example adds the `[Authorize]` attribute:</span></span>

```cshtml
@attribute [Authorize]
```

::: moniker range=">= aspnetcore-3.0"

### <a name="code"></a><span data-ttu-id="fc1c7-215">kod \@</span><span class="sxs-lookup"><span data-stu-id="fc1c7-215">\@code</span></span>

<span data-ttu-id="fc1c7-216">*Ten scenariusz dotyczy tylko składników Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-216">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="fc1c7-217">Blok `@code` umożliwia [składnikowi Razor](xref:blazor/components) Dodawanie C# elementów członkowskich (pól, właściwości i metod) do składnika:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-217">The `@code` block enables a [Razor component](xref:blazor/components) to add C# members (fields, properties, and methods) to a component:</span></span>

```cshtml
@code {
    // C# members (fields, properties, and methods)
}
```

<span data-ttu-id="fc1c7-218">W przypadku składników Razor `@code` jest aliasem [`@functions`](#functions) i zalecanym względem `@functions`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-218">For Razor components, `@code` is an alias of [`@functions`](#functions) and recommended over `@functions`.</span></span> <span data-ttu-id="fc1c7-219">Dozwolony jest więcej niż jeden blok `@code`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-219">More than one `@code` block is permissible.</span></span>

::: moniker-end

### <a name="functions"></a><span data-ttu-id="fc1c7-220">funkcje \@</span><span class="sxs-lookup"><span data-stu-id="fc1c7-220">\@functions</span></span>

<span data-ttu-id="fc1c7-221">Dyrektywa `@functions` umożliwia dodawanie C# elementów członkowskich (pól, właściwości i metod) do wygenerowanej klasy:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-221">The `@functions` directive enables adding C# members (fields, properties, and methods) to the generated class:</span></span>

```cshtml
@functions {
    // C# members (fields, properties, and methods)
}
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fc1c7-222">W [składnikach Razor](xref:blazor/components)Użyj `@code` przez `@functions`, aby C# dodać członków.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-222">In [Razor components](xref:blazor/components), use `@code` over `@functions` to add C# members.</span></span>

::: moniker-end

<span data-ttu-id="fc1c7-223">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-223">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="fc1c7-224">Kod generuje następujący kod znaczników HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-224">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="fc1c7-225">Poniższy kod jest wygenerowany Razor C# klasy:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-225">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fc1c7-226">Metody `@functions` służą jako metody tworzenia szablonów, gdy mają znaczniki:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-226">`@functions` methods serve as templating methods when they have markup:</span></span>

```cshtml
@{
    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}

@functions {
    private void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }
}
```

<span data-ttu-id="fc1c7-227">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-227">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

### <a name="implements"></a><span data-ttu-id="fc1c7-228">\@implementuje</span><span class="sxs-lookup"><span data-stu-id="fc1c7-228">\@implements</span></span>

<span data-ttu-id="fc1c7-229">Dyrektywa `@implements` implementuje interfejs dla wygenerowanej klasy.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-229">The `@implements` directive implements an interface for the generated class.</span></span>

<span data-ttu-id="fc1c7-230">Poniższy przykład implementuje <xref:System.IDisposable?displayProperty=fullName>, aby można było wywołać metodę <xref:System.IDisposable.Dispose*>:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-230">The following example implements <xref:System.IDisposable?displayProperty=fullName> so that the <xref:System.IDisposable.Dispose*> method can be called:</span></span>

```cshtml
@implements IDisposable

<h1>Example</h1>

@functions {
    private bool _isDisposed;

    ...

    public void Dispose() => _isDisposed = true;
}
```

::: moniker-end

### <a name="inherits"></a><span data-ttu-id="fc1c7-231">\@dziedziczy</span><span class="sxs-lookup"><span data-stu-id="fc1c7-231">\@inherits</span></span>

<span data-ttu-id="fc1c7-232">`@inherits` Dyrektywy umożliwia pełną kontrolę nad klasa dziedziczy widoku:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-232">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="fc1c7-233">Poniższy kod jest niestandardowy typ strony Razor:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-233">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="fc1c7-234">`CustomText` Jest wyświetlany w widoku:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-234">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="fc1c7-235">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-235">The code renders the following HTML:</span></span>

```html
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

 <span data-ttu-id="fc1c7-236">`@model` i `@inherits` mogą być używane w jednym widoku.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-236">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="fc1c7-237">`@inherits` mogą znajdować się w *_ViewImports.cshtml* pliku, który importuje widoku:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-237">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="fc1c7-238">Poniższy kod jest przykładem silnie typizowane widoku:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-238">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="fc1c7-239">Jeśli "rick@contoso.com" jest przekazywany w modelu widoku generuje następujący kod znaczników HTML:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-239">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>
    Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping
    a slop bucket on the street below.
</div>
```

### <a name="inject"></a><span data-ttu-id="fc1c7-240">\@wstrzyknąć</span><span class="sxs-lookup"><span data-stu-id="fc1c7-240">\@inject</span></span>

<span data-ttu-id="fc1c7-241">`@inject` Dyrektywy umożliwia strony Razor, aby wstawić usługi z [kontener usługi](xref:fundamentals/dependency-injection) w widoku.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-241">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="fc1c7-242">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fc1c7-242">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="layout"></a><span data-ttu-id="fc1c7-243">Układ \@</span><span class="sxs-lookup"><span data-stu-id="fc1c7-243">\@layout</span></span>

<span data-ttu-id="fc1c7-244">*Ten scenariusz dotyczy tylko składników Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-244">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="fc1c7-245">Dyrektywa `@layout` określa układ składnika Razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-245">The `@layout` directive specifies a layout for a Razor component.</span></span> <span data-ttu-id="fc1c7-246">Składniki układu są używane do uniknięcia duplikowania kodu i niespójności.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-246">Layout components are used to avoid code duplication and inconsistency.</span></span> <span data-ttu-id="fc1c7-247">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-247">For more information, see <xref:blazor/layouts>.</span></span>

::: moniker-end

### <a name="model"></a><span data-ttu-id="fc1c7-248">Model \@</span><span class="sxs-lookup"><span data-stu-id="fc1c7-248">\@model</span></span>

<span data-ttu-id="fc1c7-249">*Ten scenariusz dotyczy tylko widoków MVC i Razor Pages (. cshtml).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-249">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="fc1c7-250">Dyrektywa `@model` określa typ modelu przekazaną do widoku lub strony:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-250">The `@model` directive specifies the type of the model passed to a view or page:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="fc1c7-251">W aplikacji ASP.NET Core MVC lub Razor Pages utworzonej przy użyciu poszczególnych kont użytkowników, *widoki/konta/login. cshtml* zawierają następującą deklarację modelu:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-251">In an ASP.NET Core MVC or Razor Pages app created with individual user accounts, *Views/Account/Login.cshtml* contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="fc1c7-252">Dziedziczy po klasie, wygenerowany `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-252">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="fc1c7-253">Udostępnia razor `Model` właściwości do uzyskiwania dostępu do modelu przekazywane do widoku:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-253">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="fc1c7-254">Dyrektywa `@model` określa typ właściwości `Model`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-254">The `@model` directive specifies the type of the `Model` property.</span></span> <span data-ttu-id="fc1c7-255">Dyrektywa określa `T` w `RazorPage<T>` czy wygenerowanej klasy, która widok pochodzi od klasy.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-255">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="fc1c7-256">Jeśli `@model` dyrektywy nie jest określona, `Model` właściwość jest typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-256">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="fc1c7-257">Aby uzyskać więcej informacji, zobacz [silnie wpisane modele i słowo kluczowe @model](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span><span class="sxs-lookup"><span data-stu-id="fc1c7-257">For more information, see [Strongly typed models and the @model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="namespace"></a><span data-ttu-id="fc1c7-258">\@przestrzeń nazw</span><span class="sxs-lookup"><span data-stu-id="fc1c7-258">\@namespace</span></span>

<span data-ttu-id="fc1c7-259">Dyrektywa `@namespace`:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-259">The `@namespace` directive:</span></span>

* <span data-ttu-id="fc1c7-260">Ustawia przestrzeń nazw klasy wygenerowanej strony Razor, widoku MVC lub składnika Razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-260">Sets the namespace of the class of the generated Razor page, MVC view, or Razor component.</span></span>
* <span data-ttu-id="fc1c7-261">Ustawia główne pochodne przestrzenie nazw stron, widoków lub klas składników z najbliższego pliku importów w drzewie katalogów, *_ViewImports. cshtml* (widoki lub strony) lub *_Imports. Razor* (składniki Razor).</span><span class="sxs-lookup"><span data-stu-id="fc1c7-261">Sets the root derived namespaces of a pages, views, or components classes from the closest imports file in the directory tree, *_ViewImports.cshtml* (views or pages) or *_Imports.razor* (Razor components).</span></span>

```cshtml
@namespace Your.Namespace.Here
```

<span data-ttu-id="fc1c7-262">W przykładzie Razor Pages przedstawionym w poniższej tabeli:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-262">For the Razor Pages example shown in the following table:</span></span>

* <span data-ttu-id="fc1c7-263">Każda Strona importuje *strony/_ViewImports. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-263">Each page imports *Pages/_ViewImports.cshtml*.</span></span>
* <span data-ttu-id="fc1c7-264">*Strony/_ViewImports. cshtml* zawierają `@namespace Hello.World`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-264">*Pages/_ViewImports.cshtml* contains `@namespace Hello.World`.</span></span>
* <span data-ttu-id="fc1c7-265">Każda Strona ma `Hello.World` jako element główny przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-265">Each page has `Hello.World` as the root of it's namespace.</span></span>

| <span data-ttu-id="fc1c7-266">logowania</span><span class="sxs-lookup"><span data-stu-id="fc1c7-266">Page</span></span>                                        | <span data-ttu-id="fc1c7-267">Przestrzeń nazw</span><span class="sxs-lookup"><span data-stu-id="fc1c7-267">Namespace</span></span>                             |
| ------------------------------------------- | ------------------------------------- |
| <span data-ttu-id="fc1c7-268">*Pages/index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-268">*Pages/Index.cshtml*</span></span>                        | `Hello.World`                         |
| <span data-ttu-id="fc1c7-269">*Pages/MorePages/Page. cshtml*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-269">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages`               |
| <span data-ttu-id="fc1c7-270">*Pages/MorePages/EvenMorePages/Page. cshtml*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-270">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Hello.World.MorePages.EvenMorePages` |

<span data-ttu-id="fc1c7-271">Poprzednie relacje mają zastosowanie do plików importu używanych z widokami MVC i składnikami Razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-271">The preceding relationships apply to import files used with MVC views and Razor components.</span></span>

<span data-ttu-id="fc1c7-272">Gdy wiele plików importu zawiera dyrektywę `@namespace`, plik znajdujący się najbliżej strony, widoku lub składnika w drzewie katalogów jest używany do ustawiania głównej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-272">When multiple import files have a `@namespace` directive, the file closest to the page, view, or component in the directory tree is used to set the root namespace.</span></span>

<span data-ttu-id="fc1c7-273">Jeśli folder *EvenMorePages* w poprzednim przykładzie zawiera plik Imports z `@namespace Another.Planet` (lub plik *Pages/MorePages/EvenMorePages/Page. cshtml* zawiera `@namespace Another.Planet`), wynik jest przedstawiony w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-273">If the *EvenMorePages* folder in the preceding example has an imports file with `@namespace Another.Planet` (or the *Pages/MorePages/EvenMorePages/Page.cshtml* file contains `@namespace Another.Planet`), the result is shown in the following table.</span></span>

| <span data-ttu-id="fc1c7-274">logowania</span><span class="sxs-lookup"><span data-stu-id="fc1c7-274">Page</span></span>                                        | <span data-ttu-id="fc1c7-275">Przestrzeń nazw</span><span class="sxs-lookup"><span data-stu-id="fc1c7-275">Namespace</span></span>               |
| ------------------------------------------- | ----------------------- |
| <span data-ttu-id="fc1c7-276">*Pages/index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-276">*Pages/Index.cshtml*</span></span>                        | `Hello.World`           |
| <span data-ttu-id="fc1c7-277">*Pages/MorePages/Page. cshtml*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-277">*Pages/MorePages/Page.cshtml*</span></span>               | `Hello.World.MorePages` |
| <span data-ttu-id="fc1c7-278">*Pages/MorePages/EvenMorePages/Page. cshtml*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-278">*Pages/MorePages/EvenMorePages/Page.cshtml*</span></span> | `Another.Planet`        |

### <a name="page"></a><span data-ttu-id="fc1c7-279">Strona \@</span><span class="sxs-lookup"><span data-stu-id="fc1c7-279">\@page</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fc1c7-280">Dyrektywa `@page` ma różne skutki w zależności od typu pliku, w którym występuje.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-280">The `@page` directive has different effects depending on the type of the file where it appears.</span></span> <span data-ttu-id="fc1c7-281">Dyrektywa:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-281">The directive:</span></span>

* <span data-ttu-id="fc1c7-282">W pliku *. cshtml* wskazuje, że plik jest stroną Razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-282">In in a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="fc1c7-283">Aby uzyskać więcej informacji, zobacz [niestandardowe trasy](xref:razor-pages/index#custom-routes) i <xref:razor-pages/index>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-283">For more information, see [Custom routes](xref:razor-pages/index#custom-routes) and <xref:razor-pages/index>.</span></span>
* <span data-ttu-id="fc1c7-284">Określa, że składnik Razor powinien obsługiwać żądania bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-284">Specifies that a Razor component should handle requests directly.</span></span> <span data-ttu-id="fc1c7-285">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/routing>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-285">For more information, see <xref:blazor/routing>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fc1c7-286">Dyrektywa `@page` w pierwszym wierszu pliku *. cshtml* wskazuje, że plik jest stroną Razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-286">The `@page` directive on the first line of a *.cshtml* file indicates that the file is a Razor Page.</span></span> <span data-ttu-id="fc1c7-287">Aby uzyskać więcej informacji, zobacz temat <xref:razor-pages/index>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-287">For more information, see <xref:razor-pages/index>.</span></span>

::: moniker-end

### <a name="section"></a><span data-ttu-id="fc1c7-288">Sekcja \@</span><span class="sxs-lookup"><span data-stu-id="fc1c7-288">\@section</span></span>

<span data-ttu-id="fc1c7-289">*Ten scenariusz dotyczy tylko widoków MVC i Razor Pages (. cshtml).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-289">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="fc1c7-290">Dyrektywa `@section` jest używana w połączeniu z [układem MVC i Razor Pages](xref:mvc/views/layout) , aby umożliwić widokom i stronom renderowanie zawartości w różnych częściach strony html.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-290">The `@section` directive is used in conjunction with [MVC and Razor Pages layouts](xref:mvc/views/layout) to enable views or pages to render content in different parts of the HTML page.</span></span> <span data-ttu-id="fc1c7-291">Aby uzyskać więcej informacji, zobacz temat <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-291">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="using"></a><span data-ttu-id="fc1c7-292">\@przy użyciu</span><span class="sxs-lookup"><span data-stu-id="fc1c7-292">\@using</span></span>

<span data-ttu-id="fc1c7-293">`@using` Dodaje dyrektywy C# `using` dyrektywy do wygenerowanego widoku:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-293">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fc1c7-294">W [składnikach Razor](xref:blazor/components)`@using` również kontroluje składniki, które znajdują się w zakresie.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-294">In [Razor components](xref:blazor/components), `@using` also controls which components are in scope.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="directive-attributes"></a><span data-ttu-id="fc1c7-295">Atrybuty dyrektywy</span><span class="sxs-lookup"><span data-stu-id="fc1c7-295">Directive attributes</span></span>

### <a name="attributes"></a><span data-ttu-id="fc1c7-296">atrybuty \@</span><span class="sxs-lookup"><span data-stu-id="fc1c7-296">\@attributes</span></span>

<span data-ttu-id="fc1c7-297">*Ten scenariusz dotyczy tylko składników Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-297">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="fc1c7-298">`@attributes` zezwala składnikowi na renderowanie niezadeklarowanych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-298">`@attributes` allows a component to render non-declared attributes.</span></span> <span data-ttu-id="fc1c7-299">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-299">For more information, see <xref:blazor/components#attribute-splatting-and-arbitrary-parameters>.</span></span>

### <a name="bind"></a><span data-ttu-id="fc1c7-300">powiązanie \@</span><span class="sxs-lookup"><span data-stu-id="fc1c7-300">\@bind</span></span>

<span data-ttu-id="fc1c7-301">*Ten scenariusz dotyczy tylko składników Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-301">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="fc1c7-302">Powiązanie danych w składnikach jest realizowane przy użyciu atrybutu `@bind`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-302">Data binding in components is accomplished with the `@bind` attribute.</span></span> <span data-ttu-id="fc1c7-303">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#data-binding>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-303">For more information, see <xref:blazor/components#data-binding>.</span></span>

### <a name="onevent"></a><span data-ttu-id="fc1c7-304">\@w {EVENT}</span><span class="sxs-lookup"><span data-stu-id="fc1c7-304">\@on{EVENT}</span></span>

<span data-ttu-id="fc1c7-305">*Ten scenariusz dotyczy tylko składników Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-305">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="fc1c7-306">Razor udostępnia funkcje obsługi zdarzeń dla składników.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-306">Razor provides event handling features for components.</span></span> <span data-ttu-id="fc1c7-307">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#event-handling>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-307">For more information, see <xref:blazor/components#event-handling>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.1"

### <a name="oneventpreventdefault"></a><span data-ttu-id="fc1c7-308">\@w {EVENT}:p reventDefault</span><span class="sxs-lookup"><span data-stu-id="fc1c7-308">\@on{EVENT}:preventDefault</span></span>

<span data-ttu-id="fc1c7-309">*Ten scenariusz dotyczy tylko składników Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-309">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="fc1c7-310">Zapobiega domyślnej akcji dla zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-310">Prevents the default action for the event.</span></span>

### <a name="oneventstoppropagation"></a><span data-ttu-id="fc1c7-311">\@w {EVENT}: stopPropagation</span><span class="sxs-lookup"><span data-stu-id="fc1c7-311">\@on{EVENT}:stopPropagation</span></span>

<span data-ttu-id="fc1c7-312">*Ten scenariusz dotyczy tylko składników Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-312">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="fc1c7-313">Powoduje zatrzymanie propagacji zdarzenia dla zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-313">Stops event propagation for the event.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="key"></a><span data-ttu-id="fc1c7-314">klucz \@</span><span class="sxs-lookup"><span data-stu-id="fc1c7-314">\@key</span></span>

<span data-ttu-id="fc1c7-315">*Ten scenariusz dotyczy tylko składników Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-315">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="fc1c7-316">Atrybut `@key` dyrektywie powoduje, że algorytmy różnicowe składników do zagwarantowania zachowywania elementów lub składników na podstawie wartości klucza.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-316">The `@key` directive attribute causes the components diffing algorithm to guarantee preservation of elements or components based on the key's value.</span></span> <span data-ttu-id="fc1c7-317">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-317">For more information, see <xref:blazor/components#use-key-to-control-the-preservation-of-elements-and-components>.</span></span>

### <a name="ref"></a><span data-ttu-id="fc1c7-318">\@ref</span><span class="sxs-lookup"><span data-stu-id="fc1c7-318">\@ref</span></span>

<span data-ttu-id="fc1c7-319">*Ten scenariusz dotyczy tylko składników Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-319">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="fc1c7-320">Odwołania do składników (`@ref`) umożliwiają odwoływanie się do wystąpienia składnika, dzięki czemu można wydać polecenia do tego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-320">Component references (`@ref`) provide a way to reference a component instance so that you can issue commands to that instance.</span></span> <span data-ttu-id="fc1c7-321">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#capture-references-to-components>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-321">For more information, see <xref:blazor/components#capture-references-to-components>.</span></span>

### <a name="typeparam"></a><span data-ttu-id="fc1c7-322">\@typeparam</span><span class="sxs-lookup"><span data-stu-id="fc1c7-322">\@typeparam</span></span>

<span data-ttu-id="fc1c7-323">*Ten scenariusz dotyczy tylko składników Razor (Razor).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-323">*This scenario only applies to Razor components (.razor).*</span></span>

<span data-ttu-id="fc1c7-324">Dyrektywa `@typeparam` deklaruje parametr typu ogólnego dla wygenerowanej klasy składnika.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-324">The `@typeparam` directive declares a generic type parameter for the generated component class.</span></span> <span data-ttu-id="fc1c7-325">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/components#generic-typed-components>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-325">For more information, see <xref:blazor/components#generic-typed-components>.</span></span>

::: moniker-end

## <a name="templated-razor-delegates"></a><span data-ttu-id="fc1c7-326">Oparte na szablonach delegatów Razor</span><span class="sxs-lookup"><span data-stu-id="fc1c7-326">Templated Razor delegates</span></span>

<span data-ttu-id="fc1c7-327">Szablony razor umożliwiają definiowanie fragmentu interfejsu użytkownika w następującym formacie:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-327">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="fc1c7-328">Poniższy przykład ilustruje sposób określania oparte na szablonach delegata Razor jako <xref:System.Func%602>.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-328">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func%602>.</span></span> <span data-ttu-id="fc1c7-329">[Typu dynamicznego](/dotnet/csharp/programming-guide/types/using-type-dynamic) jest określona dla parametru metody, która hermetyzuje delegata.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-329">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="fc1c7-330">[Typ obiektu](/dotnet/csharp/language-reference/keywords/object) jest określony jako wartość zwracaną przez delegata.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-330">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="fc1c7-331">Ten szablon jest używany z <xref:System.Collections.Generic.List%601> z `Pet` zawierający `Name` właściwości.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-331">The template is used with a <xref:System.Collections.Generic.List%601> of `Pet` that has a `Name` property.</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }
}
```

```cshtml
@{
    Func<dynamic, object> petTemplate = @<p>You have a pet named <strong>@item.Name</strong>.</p>;

    var pets = new List<Pet>
    {
        new Pet { Name = "Rin Tin Tin" },
        new Pet { Name = "Mr. Bigglesworth" },
        new Pet { Name = "K-9" }
    };
}
```

<span data-ttu-id="fc1c7-332">Szablon jest renderowany przy użyciu `pets` dostarczonych przez `foreach` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-332">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="fc1c7-333">Wyniku renderowania:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-333">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="fc1c7-334">Możesz również dostarczyć wbudowany szablon Razor, jako argument do metody.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-334">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="fc1c7-335">W poniższym przykładzie `Repeat` metoda odbiera szablon Razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-335">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="fc1c7-336">Metoda używa tego szablonu w celu wygenerowania zawartość HTML z powtórzeń elementy dostarczane z listy:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-336">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

```cshtml
@using Microsoft.AspNetCore.Html

@functions {
    public static IHtmlContent Repeat(IEnumerable<dynamic> items, int times,
        Func<dynamic, IHtmlContent> template)
    {
        var html = new HtmlContentBuilder();

        foreach (var item in items)
        {
            for (var i = 0; i < times; i++)
            {
                html.AppendHtml(template(item));
            }
        }

        return html;
    }
}
```

<span data-ttu-id="fc1c7-337">Za pomocą listę zwierząt domowych, z poprzedniego przykładu `Repeat` metoda jest wywoływana z:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-337">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="fc1c7-338"><xref:System.Collections.Generic.List%601> z `Pet`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-338"><xref:System.Collections.Generic.List%601> of `Pet`.</span></span>
* <span data-ttu-id="fc1c7-339">Liczba powtórzeń każdego pet.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-339">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="fc1c7-340">Wbudowany szablon do użycia dla elementów listy, listy nieuporządkowane.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-340">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="fc1c7-341">Wyniku renderowania:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-341">Rendered output:</span></span>

```html
<ul>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>K-9</li>
    <li>K-9</li>
    <li>K-9</li>
</ul>
```

## <a name="tag-helpers"></a><span data-ttu-id="fc1c7-342">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="fc1c7-342">Tag Helpers</span></span>

<span data-ttu-id="fc1c7-343">*Ten scenariusz dotyczy tylko widoków MVC i Razor Pages (. cshtml).*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-343">*This scenario only applies to MVC views and Razor Pages (.cshtml).*</span></span>

<span data-ttu-id="fc1c7-344">Istnieją trzy dyrektyw, które odnoszą się do [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="fc1c7-344">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="fc1c7-345">— Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="fc1c7-345">Directive</span></span> | <span data-ttu-id="fc1c7-346">Funkcja</span><span class="sxs-lookup"><span data-stu-id="fc1c7-346">Function</span></span> |
| --------- | -------- |
| [`@addTagHelper`](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="fc1c7-347">Udostępnia pomocników tagów do widoku.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-347">Makes Tag Helpers available to a view.</span></span> |
| [`@removeTagHelper`](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="fc1c7-348">Usuwa pomocnicy tagów, wcześniej dodany z widoku.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-348">Removes Tag Helpers previously added from a view.</span></span> |
| [`@tagHelperPrefix`](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="fc1c7-349">Określa prefiks tagu, aby włączyć obsługę pomocnika tagów i Pomocnik tagu użycie jawnego.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-349">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="fc1c7-350">Razor zastrzeżone słowa kluczowe</span><span class="sxs-lookup"><span data-stu-id="fc1c7-350">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="fc1c7-351">Słowa kluczowe razor</span><span class="sxs-lookup"><span data-stu-id="fc1c7-351">Razor keywords</span></span>

* <span data-ttu-id="fc1c7-352">Strona (wymaga ASP.NET Core 2,1 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="fc1c7-352">page (Requires ASP.NET Core 2.1 or later)</span></span>
* <span data-ttu-id="fc1c7-353">{1&gt; — przestrzeń nazw&lt;1}</span><span class="sxs-lookup"><span data-stu-id="fc1c7-353">namespace</span></span>
* <span data-ttu-id="fc1c7-354">— funkcje</span><span class="sxs-lookup"><span data-stu-id="fc1c7-354">functions</span></span>
* <span data-ttu-id="fc1c7-355">Inherits</span><span class="sxs-lookup"><span data-stu-id="fc1c7-355">inherits</span></span>
* <span data-ttu-id="fc1c7-356">{1&gt;model&lt;1}</span><span class="sxs-lookup"><span data-stu-id="fc1c7-356">model</span></span>
* <span data-ttu-id="fc1c7-357">sekcja</span><span class="sxs-lookup"><span data-stu-id="fc1c7-357">section</span></span>
* <span data-ttu-id="fc1c7-358">Pomocnik (nie jest obecnie obsługiwane przez program ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="fc1c7-358">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="fc1c7-359">Razor słowa kluczowe są poprzedzone znakiem zmiany znaczenia z `@(Razor Keyword)` (na przykład `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="fc1c7-359">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="fc1c7-360">C#Słowa kluczowe razor</span><span class="sxs-lookup"><span data-stu-id="fc1c7-360">C# Razor keywords</span></span>

* <span data-ttu-id="fc1c7-361">case</span><span class="sxs-lookup"><span data-stu-id="fc1c7-361">case</span></span>
* <span data-ttu-id="fc1c7-362">do</span><span class="sxs-lookup"><span data-stu-id="fc1c7-362">do</span></span>
* <span data-ttu-id="fc1c7-363">{1&gt;default&lt;1}</span><span class="sxs-lookup"><span data-stu-id="fc1c7-363">default</span></span>
* <span data-ttu-id="fc1c7-364">dla</span><span class="sxs-lookup"><span data-stu-id="fc1c7-364">for</span></span>
* <span data-ttu-id="fc1c7-365">foreach</span><span class="sxs-lookup"><span data-stu-id="fc1c7-365">foreach</span></span>
* <span data-ttu-id="fc1c7-366">if</span><span class="sxs-lookup"><span data-stu-id="fc1c7-366">if</span></span>
* <span data-ttu-id="fc1c7-367">else</span><span class="sxs-lookup"><span data-stu-id="fc1c7-367">else</span></span>
* <span data-ttu-id="fc1c7-368">lock</span><span class="sxs-lookup"><span data-stu-id="fc1c7-368">lock</span></span>
* <span data-ttu-id="fc1c7-369">— przełącznik</span><span class="sxs-lookup"><span data-stu-id="fc1c7-369">switch</span></span>
* <span data-ttu-id="fc1c7-370">Wypróbuj</span><span class="sxs-lookup"><span data-stu-id="fc1c7-370">try</span></span>
* <span data-ttu-id="fc1c7-371">CATCH</span><span class="sxs-lookup"><span data-stu-id="fc1c7-371">catch</span></span>
* <span data-ttu-id="fc1c7-372">finally</span><span class="sxs-lookup"><span data-stu-id="fc1c7-372">finally</span></span>
* <span data-ttu-id="fc1c7-373">korzystanie</span><span class="sxs-lookup"><span data-stu-id="fc1c7-373">using</span></span>
* <span data-ttu-id="fc1c7-374">while</span><span class="sxs-lookup"><span data-stu-id="fc1c7-374">while</span></span>

<span data-ttu-id="fc1c7-375">C#Słowa kluczowe razor musi być double ucieczki `@(@C# Razor Keyword)` (na przykład `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="fc1c7-375">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="fc1c7-376">Pierwszy `@` specjalne analizator Razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-376">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="fc1c7-377">Drugi `@` specjalne C# analizatora.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-377">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="fc1c7-378">Zastrzeżone słowa kluczowe nie użył ich Razor</span><span class="sxs-lookup"><span data-stu-id="fc1c7-378">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="fc1c7-379">class</span><span class="sxs-lookup"><span data-stu-id="fc1c7-379">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="fc1c7-380">Zbadaj Razor C# klasa generowana dla widoku</span><span class="sxs-lookup"><span data-stu-id="fc1c7-380">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="fc1c7-381">Za pomocą platformy .NET Core SDK 2.1 lub nowszej [Razor SDK](xref:razor-pages/sdk) obsługuje kompilacji plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-381">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="fc1c7-382">Podczas kompilowania projektu, generuje zestaw Razor SDK *obj / < build_configuration > / < target_framework_moniker > / Razor* katalogu w folderze głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-382">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="fc1c7-383">Struktura katalogów w ramach *Razor* katalogu odzwierciedla strukturze katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-383">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="fc1c7-384">Rozważmy następującą strukturę katalogów w projekcie stron Razor programu ASP.NET Core 2.1 przeznaczonych dla platformy .NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-384">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="fc1c7-385">**Obszary /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-385">**Areas/**</span></span>
  * <span data-ttu-id="fc1c7-386">**Administrator /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-386">**Admin/**</span></span>
    * <span data-ttu-id="fc1c7-387">**Strony /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-387">**Pages/**</span></span>
      * <span data-ttu-id="fc1c7-388">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-388">*Index.cshtml*</span></span>
      * <span data-ttu-id="fc1c7-389">*Index.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-389">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="fc1c7-390">**Strony /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-390">**Pages/**</span></span>
  * <span data-ttu-id="fc1c7-391">**Udostępnione /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-391">**Shared/**</span></span>
    * <span data-ttu-id="fc1c7-392">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-392">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="fc1c7-393">*_ViewImports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-393">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="fc1c7-394">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-394">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="fc1c7-395">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-395">*Index.cshtml*</span></span>
  * <span data-ttu-id="fc1c7-396">*Index.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-396">*Index.cshtml.cs*</span></span>

<span data-ttu-id="fc1c7-397">Tworzenie projektu na platformie *debugowania* konfiguracji daje następujące *obj* katalogu:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-397">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="fc1c7-398">**obj /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-398">**obj/**</span></span>
  * <span data-ttu-id="fc1c7-399">**Debugowanie /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-399">**Debug/**</span></span>
    * <span data-ttu-id="fc1c7-400">**netcoreapp2.1 /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-400">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="fc1c7-401">**Razor /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-401">**Razor/**</span></span>
        * <span data-ttu-id="fc1c7-402">**Obszary /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-402">**Areas/**</span></span>
          * <span data-ttu-id="fc1c7-403">**Administrator /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-403">**Admin/**</span></span>
            * <span data-ttu-id="fc1c7-404">**Strony /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-404">**Pages/**</span></span>
              * <span data-ttu-id="fc1c7-405">*Index.g.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-405">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="fc1c7-406">**Strony /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-406">**Pages/**</span></span>
          * <span data-ttu-id="fc1c7-407">**Udostępnione /**</span><span class="sxs-lookup"><span data-stu-id="fc1c7-407">**Shared/**</span></span>
            * <span data-ttu-id="fc1c7-408">*_Layout.g.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-408">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="fc1c7-409">*_ViewImports.g.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-409">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="fc1c7-410">*_ViewStart.g.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-410">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="fc1c7-411">*Index.g.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="fc1c7-411">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="fc1c7-412">Aby wyświetlić wygenerowanej klasy dla *Pages/Index.cshtml*, otwórz *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-412">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="fc1c7-413">Dodaj poniższą klasę do projektu programu ASP.NET Core MVC:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-413">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="fc1c7-414">W `Startup.ConfigureServices`, Zastąp `RazorTemplateEngine` dodane przez wzorzec MVC z `CustomTemplateEngine` klasy:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-414">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="fc1c7-415">Ustaw punkt przerwania na `return csharpDocument;` instrukcja `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-415">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="fc1c7-416">Po zatrzymaniu w punkcie przerwania wykonywania programu wyświetlić wartość `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-416">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![Widok Wizualizator tekstu generatedCode](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="fc1c7-418">Widok wyszukiwania i rozróżnianie wielkości liter</span><span class="sxs-lookup"><span data-stu-id="fc1c7-418">View lookups and case sensitivity</span></span>

<span data-ttu-id="fc1c7-419">Aparat widoku Razor wykonuje wyszukiwanie z uwzględnieniem wielkości liter dla widoków.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-419">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="fc1c7-420">Jednak rzeczywista wyszukiwania jest określany przez sam system plików:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-420">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="fc1c7-421">Na podstawie źródła pliku:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-421">File based source:</span></span>
  * <span data-ttu-id="fc1c7-422">W systemach operacyjnych z systemami plików bez uwzględniania wielkości liter (na przykład Windows) plik fizyczny dostawcy wyszukiwania jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-422">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="fc1c7-423">Na przykład `return View("Test")` skutkuje dopasowań dla */Views/Home/Test.cshtml*, */Views/home/test.cshtml*i inne wariant wielkość liter w wyrazie.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-423">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="fc1c7-424">W systemach plików rozróżniana wielkość liter (na przykład, Linux, OSX i `EmbeddedFileProvider`), wyszukiwania jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-424">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="fc1c7-425">Na przykład `return View("Test")` specjalnie dopasowania */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-425">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="fc1c7-426">Wstępnie skompilowany widoków: Platforma ASP.NET Core 2.0 i nowsze wersje wyszukiwania prekompilowanego widoków jest uwzględniana wielkość liter, we wszystkich systemach operacyjnych.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-426">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="fc1c7-427">Zachowanie jest taka sama jak zachowanie dostawcy plików fizycznych w Windows.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-427">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="fc1c7-428">Jeśli dwa widoki wstępnie skompilowanych różnią się tylko wielkością liter, wynikiem wyszukiwania jest niedeterministyczny.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-428">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="fc1c7-429">Deweloperzy są zachęcani do pasuje do wielkości liter w nazwach plików i katalogów na wielkość liter w wyrazie:</span><span class="sxs-lookup"><span data-stu-id="fc1c7-429">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="fc1c7-430">Nazwy obszaru, kontrolera i akcji.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-430">Area, controller, and action names.</span></span>
* <span data-ttu-id="fc1c7-431">Strony razor.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-431">Razor Pages.</span></span>

<span data-ttu-id="fc1c7-432">Wielkości liter, gwarantuje, że wdrożeń Znajdź swoje opinie, niezależnie od tego, źródłowy system plików.</span><span class="sxs-lookup"><span data-stu-id="fc1c7-432">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
