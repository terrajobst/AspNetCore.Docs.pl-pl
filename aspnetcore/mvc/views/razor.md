---
title: "Odwołania do składni razor dla platformy ASP.NET Core"
author: rick-anderson
description: "Więcej informacji o składni Razor znaczników do osadzania kodu na serwerze do stron sieci Web."
keywords: Dyrektywy Razor platformy ASP.NET Core, Razor,
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 6df769069fce52755a57d8404f88203a652a1ab9
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="ff621-104">Składnia razor dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff621-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="ff621-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [MULLENA Taylora](https://twitter.com/ntaylormullen), i [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="ff621-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex),  [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="ff621-106">Razor jest składnię znaczników osadzanie kodu na serwerze w witrynach sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ff621-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="ff621-107">Składnia Razor składa się z Razor znaczników, C# i HTML.</span><span class="sxs-lookup"><span data-stu-id="ff621-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="ff621-108">Pliki zawierające Razor zazwyczaj mają *.cshtml* rozszerzenia pliku.</span><span class="sxs-lookup"><span data-stu-id="ff621-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="ff621-109">Renderowanie kodu HTML</span><span class="sxs-lookup"><span data-stu-id="ff621-109">Rendering HTML</span></span>

<span data-ttu-id="ff621-110">Domyślny język Razor jest HTML.</span><span class="sxs-lookup"><span data-stu-id="ff621-110">The default Razor language is HTML.</span></span> <span data-ttu-id="ff621-111">Renderowanie kodu HTML, z znaczników Razor nie różni się od renderowania kodu HTML z pliku HTML.</span><span class="sxs-lookup"><span data-stu-id="ff621-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span>  <span data-ttu-id="ff621-112">Kod znaczników HTML w *.cshtml* pliki Razor jest renderowany na serwerze bez zmian.</span><span class="sxs-lookup"><span data-stu-id="ff621-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="ff621-113">Składnia razor</span><span class="sxs-lookup"><span data-stu-id="ff621-113">Razor syntax</span></span>

<span data-ttu-id="ff621-114">Razor obsługuje C# i używa `@` symbol przejście z kodu HTML dla C#.</span><span class="sxs-lookup"><span data-stu-id="ff621-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="ff621-115">Razor ocenia wyrażeń C# i renderuje je w danych wyjściowych HTML.</span><span class="sxs-lookup"><span data-stu-id="ff621-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="ff621-116">Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](#razor-reserved-keywords), jego przejścia do znaczników specyficzne dla elementu Razor.</span><span class="sxs-lookup"><span data-stu-id="ff621-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="ff621-117">W przeciwnym wypadku przejścia do zwykłego C#.</span><span class="sxs-lookup"><span data-stu-id="ff621-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="ff621-118">Wyjścia `@` symboli w znaczniku Razor, należy użyć drugiej `@` symboli:</span><span class="sxs-lookup"><span data-stu-id="ff621-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="ff621-119">Renderowanie kodu HTML za pomocą jednej `@` symboli:</span><span class="sxs-lookup"><span data-stu-id="ff621-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="ff621-120">Atrybuty HTML i zawartości zawierającego adresy e-mail nie Traktuj `@` symbol jako znak przejścia.</span><span class="sxs-lookup"><span data-stu-id="ff621-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="ff621-121">W poniższym przykładzie adresy e-mail są niezmienione przez Razor analizy:</span><span class="sxs-lookup"><span data-stu-id="ff621-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="ff621-122">Niejawne wyrażenia Razor</span><span class="sxs-lookup"><span data-stu-id="ff621-122">Implicit Razor expressions</span></span>

<span data-ttu-id="ff621-123">Niejawne wyrażenia Razor rozpoczynać `@` następuje kod w języku C#:</span><span class="sxs-lookup"><span data-stu-id="ff621-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="ff621-124">Z wyjątkiem C# `await` — słowo kluczowe, niejawnego wyrażenia nie może zawierać spacji.</span><span class="sxs-lookup"><span data-stu-id="ff621-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="ff621-125">Jeśli wyczyść Kończenie instrukcji C#, mogą wymieszaniu spacji:</span><span class="sxs-lookup"><span data-stu-id="ff621-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="ff621-126">Wyrażenia niejawnego **nie** zawiera typami ogólnymi C#, jako znak wewnątrz nawiasów (`<>`) są interpretowane jako tagu HTML.</span><span class="sxs-lookup"><span data-stu-id="ff621-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="ff621-127">Następujący kod jest **nie** prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="ff621-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="ff621-128">Poprzedni kod generowany jest błąd kompilatora podobny do jednego z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="ff621-128">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="ff621-129">Element "int" nie został zamknięty.</span><span class="sxs-lookup"><span data-stu-id="ff621-129">The "int" element was not closed.</span></span>  <span data-ttu-id="ff621-130">Wszystkie elementy muszą być albo samodzielnie zamknięcie lub ma zgodnego tagu końcowego.</span><span class="sxs-lookup"><span data-stu-id="ff621-130">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="ff621-131">Nie można przekonwertować grupy metod "GenericMethod" na typ "object" Niedelegowany.</span><span class="sxs-lookup"><span data-stu-id="ff621-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="ff621-132">Czy zamierzasz wywołać metodę? "</span><span class="sxs-lookup"><span data-stu-id="ff621-132">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="ff621-133">Wywołania metody rodzajowe muszą być ujęte w [jawne wyrażenie Razor](#explicit-razor-expressions) lub [blok kodu Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="ff621-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="ff621-134">Jawne wyrażenia Razor</span><span class="sxs-lookup"><span data-stu-id="ff621-134">Explicit Razor expressions</span></span>

<span data-ttu-id="ff621-135">Jawne Razor wyrażeń składają się z `@` symbol o zrównoważonym nawiasu.</span><span class="sxs-lookup"><span data-stu-id="ff621-135">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="ff621-136">Do renderowania czas ostatniego tygodnia, jest używany następujący kod Razor:</span><span class="sxs-lookup"><span data-stu-id="ff621-136">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="ff621-137">Zawartość w `@()` nawias jest obliczany i renderowania danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="ff621-137">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="ff621-138">Ogólnie wyrażenia niejawnego, opisane w poprzedniej sekcji, nie może zawierać spacji.</span><span class="sxs-lookup"><span data-stu-id="ff621-138">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="ff621-139">W poniższym kodzie tydzień nie jest odejmowany od bieżącego czasu:</span><span class="sxs-lookup"><span data-stu-id="ff621-139">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="ff621-140">Kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="ff621-140">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="ff621-141">Jawne wyrażenia może służyć do łączenia tekstu w wyniku wyrażenia:</span><span class="sxs-lookup"><span data-stu-id="ff621-141">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="ff621-142">Bez jawnego wyrażenia `<p>Age@joe.Age</p>` jest traktowany jak adres e-mail i `<p>Age@joe.Age</p>` jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="ff621-142">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="ff621-143">Gdy zapisywane jako wyrażenie jawne `<p>Age33</p>` jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="ff621-143">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="ff621-144">Jawne wyrażenia może zostać użyty do renderowania dane wyjściowe metody rodzajowe w *.cshtml* plików.</span><span class="sxs-lookup"><span data-stu-id="ff621-144">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="ff621-145">W wyrażeniu niejawne znak wewnątrz nawiasów (`<>`) są interpretowane jako tagu HTML.</span><span class="sxs-lookup"><span data-stu-id="ff621-145">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="ff621-146">Następujący kod znaczników jest **nie** Razor prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="ff621-146">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="ff621-147">Poprzedni kod generowany jest błąd kompilatora podobny do jednego z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="ff621-147">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="ff621-148">Element "int" nie został zamknięty.</span><span class="sxs-lookup"><span data-stu-id="ff621-148">The "int" element was not closed.</span></span>  <span data-ttu-id="ff621-149">Wszystkie elementy muszą być albo samodzielnie zamknięcie lub ma zgodnego tagu końcowego.</span><span class="sxs-lookup"><span data-stu-id="ff621-149">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="ff621-150">Nie można przekonwertować grupy metod "GenericMethod" na typ "object" Niedelegowany.</span><span class="sxs-lookup"><span data-stu-id="ff621-150">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="ff621-151">Czy zamierzasz wywołać metodę? "</span><span class="sxs-lookup"><span data-stu-id="ff621-151">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="ff621-152">Następujący kod przedstawia sposób poprawne zapisu tego kodu.</span><span class="sxs-lookup"><span data-stu-id="ff621-152">The following markup shows the correct way write this code.</span></span>  <span data-ttu-id="ff621-153">Kod jest zapisywany jako jawne wyrażenie:</span><span class="sxs-lookup"><span data-stu-id="ff621-153">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="ff621-154">Kodowanie wyrażenia</span><span class="sxs-lookup"><span data-stu-id="ff621-154">Expression encoding</span></span>

<span data-ttu-id="ff621-155">Wyrażenia języka C#, określających ciąg są kodowania HTML.</span><span class="sxs-lookup"><span data-stu-id="ff621-155">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="ff621-156">C# wyrażeń określających `IHtmlContent` są renderowane za pomocą `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="ff621-156">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="ff621-157">C# wyrażeń, które nie zwrócą `IHtmlContent` jest konwertowana na ciąg przez `ToString` i kodowany przed ich są renderowane.</span><span class="sxs-lookup"><span data-stu-id="ff621-157">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="ff621-158">Kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="ff621-158">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="ff621-159">Kod HTML jest wyświetlany w przeglądarce jako:</span><span class="sxs-lookup"><span data-stu-id="ff621-159">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="ff621-160">`HtmlHelper.Raw`dane wyjściowe nie jest zakodowany, ale renderowany jako kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="ff621-160">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="ff621-161">Przy użyciu `HtmlHelper.Raw` unsanitized użytkownika dane wejściowe stanowi zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="ff621-161">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="ff621-162">Dane wejściowe użytkownika może zawierać złośliwego kodu JavaScript lub inne luki w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="ff621-162">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="ff621-163">Trudno jest oczyszczania danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ff621-163">Sanitizing user input is difficult.</span></span> <span data-ttu-id="ff621-164">Unikaj używania `HtmlHelper.Raw` z danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ff621-164">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="ff621-165">Kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="ff621-165">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="ff621-166">Bloki kodu razor</span><span class="sxs-lookup"><span data-stu-id="ff621-166">Razor code blocks</span></span>

<span data-ttu-id="ff621-167">Bloki kodu razor rozpoczynać `@` i znajdują się wewnątrz `{}`.</span><span class="sxs-lookup"><span data-stu-id="ff621-167">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="ff621-168">W przeciwieństwie do wyrażenia nie jest renderowany kodu C# wewnątrz bloków kodu.</span><span class="sxs-lookup"><span data-stu-id="ff621-168">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="ff621-169">Bloki kodu i wyrażenia w widoku o tym samym zakresie i są zdefiniowane w kolejności:</span><span class="sxs-lookup"><span data-stu-id="ff621-169">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="ff621-170">Kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="ff621-170">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="ff621-171">Niejawne przejścia</span><span class="sxs-lookup"><span data-stu-id="ff621-171">Implicit transitions</span></span>

<span data-ttu-id="ff621-172">Jest to domyślny język w bloku kodu C#, ale strona Razor można przejść do HTML:</span><span class="sxs-lookup"><span data-stu-id="ff621-172">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="ff621-173">Jawne przejścia rozdzielanego</span><span class="sxs-lookup"><span data-stu-id="ff621-173">Explicit delimited transition</span></span>

<span data-ttu-id="ff621-174">Aby zdefiniować podsekcji bloku kodu, które mają renderować kod HTML, Otocz znaków do renderowania elementu Razor  **\<tekst >** tagu:</span><span class="sxs-lookup"><span data-stu-id="ff621-174">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="ff621-175">Tej metody można użyć do renderowania kodu HTML, który nie jest ujęta w tagu HTML.</span><span class="sxs-lookup"><span data-stu-id="ff621-175">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="ff621-176">Bez tagu HTML lub Razor występuje błąd w czasie wykonywania Razor.</span><span class="sxs-lookup"><span data-stu-id="ff621-176">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="ff621-177"> **\<Tekst >** tag jest przydatne do kontroli odstępu podczas renderowania zawartości:</span><span class="sxs-lookup"><span data-stu-id="ff621-177">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="ff621-178">Tylko zawartość między  **\<tekst >** renderowania tagu.</span><span class="sxs-lookup"><span data-stu-id="ff621-178">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="ff621-179">Nie spacji przed lub po  **\<tekst >** tag jest wyświetlany w danych wyjściowych HTML.</span><span class="sxs-lookup"><span data-stu-id="ff621-179">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="ff621-180">Jawne wiersz przejścia z @:</span><span class="sxs-lookup"><span data-stu-id="ff621-180">Explicit Line Transition with @:</span></span>

<span data-ttu-id="ff621-181">Aby renderować rest całego wiersza jako HTML w bloku kodu, należy użyć `@:` składni:</span><span class="sxs-lookup"><span data-stu-id="ff621-181">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="ff621-182">Bez `@:` w kodzie, generowany jest błąd w czasie wykonywania Razor.</span><span class="sxs-lookup"><span data-stu-id="ff621-182">Without the `@:` in the code,  a Razor runtime error is generated.</span></span>

<span data-ttu-id="ff621-183">Ostrzeżenie: Dodatkowy `@` znaki w pliku Razor może spowodować błędy kompilatora Przyczyna w instrukcji w dalszej części tego bloku.</span><span class="sxs-lookup"><span data-stu-id="ff621-183">Warning: Extra `@` characters in a Razor file can cause  cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="ff621-184">Te błędy kompilatora może być trudne do zrozumienia, ponieważ błędu występuje przed zgłoszonego błędu.</span><span class="sxs-lookup"><span data-stu-id="ff621-184">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span>  <span data-ttu-id="ff621-185">Ten błąd jest typowe po łączenie wielu niejawnej/jawnej wyrażenia w bloku kodu pojedynczego.</span><span class="sxs-lookup"><span data-stu-id="ff621-185">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="ff621-186">Struktury sterujące</span><span class="sxs-lookup"><span data-stu-id="ff621-186">Control Structures</span></span>

<span data-ttu-id="ff621-187">Struktury sterujące stanowią rozszerzenie bloków kodu.</span><span class="sxs-lookup"><span data-stu-id="ff621-187">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="ff621-188">Wszystkie aspekty bloki kodu (przechodzenia do znaczników, wbudowane C#) również dotyczą następujące struktury:</span><span class="sxs-lookup"><span data-stu-id="ff621-188">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="ff621-189">Warunkowe instrukcje @if, else if, #else i@switch</span><span class="sxs-lookup"><span data-stu-id="ff621-189">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="ff621-190">`@if`Formanty po uruchomieniu kodu:</span><span class="sxs-lookup"><span data-stu-id="ff621-190">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="ff621-191">`else`i `else if` nie wymagają `@` symboli:</span><span class="sxs-lookup"><span data-stu-id="ff621-191">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="ff621-192">Następujący kod przedstawia sposób użycia instrukcji switch:</span><span class="sxs-lookup"><span data-stu-id="ff621-192">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="ff621-193">Zapętlenie @for, @foreach, @while, i @do podczas</span><span class="sxs-lookup"><span data-stu-id="ff621-193">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="ff621-194">HTML opartego na szablonie można renderować z pętli instrukcji sterowania.</span><span class="sxs-lookup"><span data-stu-id="ff621-194">Templated HTML can be rendered with looping control statements.</span></span>  <span data-ttu-id="ff621-195">Do renderowania listy osób:</span><span class="sxs-lookup"><span data-stu-id="ff621-195">To render a list of people:</span></span>

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

<span data-ttu-id="ff621-196">Obsługiwane są następujące instrukcje pętli:</span><span class="sxs-lookup"><span data-stu-id="ff621-196">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="ff621-197">Złożone@using</span><span class="sxs-lookup"><span data-stu-id="ff621-197">Compound @using</span></span>

<span data-ttu-id="ff621-198">W języku C# `using` instrukcji służy do zapewnienia obiekt jest usunięty.</span><span class="sxs-lookup"><span data-stu-id="ff621-198">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="ff621-199">W elemencie Razor ten sam mechanizm służy do tworzenia pomocników HTML, która zawiera dodatkową zawartość.</span><span class="sxs-lookup"><span data-stu-id="ff621-199">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="ff621-200">W poniższym kodzie pomocników HTML renderowania tagu form z `@using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="ff621-200">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

<span data-ttu-id="ff621-201">Można wykonywać działania na poziomie zakresu za pomocą [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="ff621-201">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="ff621-202">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="ff621-202">@try, catch, finally</span></span>

<span data-ttu-id="ff621-203">Obsługa wyjątków jest podobny do języka C#:</span><span class="sxs-lookup"><span data-stu-id="ff621-203">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="ff621-204">Razor ma możliwość ochrony sekcje krytyczne w instrukcjach blokady:</span><span class="sxs-lookup"><span data-stu-id="ff621-204">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="ff621-205">Komentarze</span><span class="sxs-lookup"><span data-stu-id="ff621-205">Comments</span></span>

<span data-ttu-id="ff621-206">Razor obsługuje komentarze języka C# i HTML:</span><span class="sxs-lookup"><span data-stu-id="ff621-206">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="ff621-207">Kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="ff621-207">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="ff621-208">Komentarz razor są usuwane przez serwer przed wyświetleniem strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ff621-208">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="ff621-209">Używa razor `@*  *@` ograniczającej komentarze.</span><span class="sxs-lookup"><span data-stu-id="ff621-209">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="ff621-210">Poniższy kod jest oznaczone jako komentarz, dlatego serwer nie renderowania żadnych znaczników:</span><span class="sxs-lookup"><span data-stu-id="ff621-210">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="ff621-211">Dyrektyw</span><span class="sxs-lookup"><span data-stu-id="ff621-211">Directives</span></span>

<span data-ttu-id="ff621-212">Dyrektywy razor są reprezentowane przez niejawnego wyrażenia następujące zastrzeżone słowa kluczowe `@` symbolu.</span><span class="sxs-lookup"><span data-stu-id="ff621-212">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="ff621-213">Dyrektywy zwykle zmienia sposób widok jest analizowana lub umożliwia korzystanie z różnych funkcji.</span><span class="sxs-lookup"><span data-stu-id="ff621-213">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="ff621-214">Opis sposobu Razor generuje kod dla widoku ułatwia zrozumienie, jak działają dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="ff621-214">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="ff621-215">Kod generuje klasę podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="ff621-215">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="ff621-216">Dalszej części tego artykułu, w sekcji [wyświetlanie klasy Razor C# wygenerowany dla widoku](#viewing-the-razor-c-class-generated-for-a-view) opisano sposób wyświetlania tego wygenerowanej klasy.</span><span class="sxs-lookup"><span data-stu-id="ff621-216">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="ff621-217">`@using` Dodaje dyrektywy języka C# `using` dyrektywy do wygenerowanego widoku:</span><span class="sxs-lookup"><span data-stu-id="ff621-217">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="ff621-218">`@model` Dyrektywa określa typ modelu przekazywane do widoku:</span><span class="sxs-lookup"><span data-stu-id="ff621-218">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="ff621-219">W aplikacji ASP.NET Core MVC utworzone za pomocą poszczególnych kont użytkowników *Views/Account/Login.cshtml* widok zawiera następujące oświadczenie modelu:</span><span class="sxs-lookup"><span data-stu-id="ff621-219">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="ff621-220">Wygenerowana klasa dziedziczy `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="ff621-220">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="ff621-221">Przedstawia razor `Model` właściwości do uzyskiwania dostępu do modelu przekazywane do widoku:</span><span class="sxs-lookup"><span data-stu-id="ff621-221">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="ff621-222">`@model` Dyrektywa określa typ tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="ff621-222">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="ff621-223">Określa dyrektywy `T` w `RazorPage<T>` czy wygenerowanej klasy który widok jest pochodną.</span><span class="sxs-lookup"><span data-stu-id="ff621-223">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="ff621-224">Jeśli `@model` dyrektywy jest określony, `Model` właściwość jest typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="ff621-224">If  the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="ff621-225">Wartość modelu jest przekazywany z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="ff621-225">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="ff621-226">Aby uzyskać więcej informacji, zobacz [silnie typizowane modeli i @model — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="ff621-226">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="ff621-227">`@inherits` Dyrektywy umożliwia pełną kontrolę nad klasa dziedziczy widoku:</span><span class="sxs-lookup"><span data-stu-id="ff621-227">The `@inherits` directive provides  full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="ff621-228">Następujący kod jest typu niestandardowego Razor strony:</span><span class="sxs-lookup"><span data-stu-id="ff621-228">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="ff621-229">`CustomText` Jest wyświetlany w widoku:</span><span class="sxs-lookup"><span data-stu-id="ff621-229">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="ff621-230">Kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="ff621-230">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="ff621-231">`@model`i `@inherits` mogą być używane w jednym widoku.</span><span class="sxs-lookup"><span data-stu-id="ff621-231">`@model` and `@inherits` can be used in the same view.</span></span>  <span data-ttu-id="ff621-232">`@inherits`mogą znajdować się w *_ViewImports.cshtml* pliku, który importuje widoku:</span><span class="sxs-lookup"><span data-stu-id="ff621-232">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="ff621-233">Następujący kod jest przykładem silnie typizowanego widoku:</span><span class="sxs-lookup"><span data-stu-id="ff621-233">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="ff621-234">Jeśli "rick@contoso.com" jest przekazywany w modelu widoku generuje następujący kod znaczników HTML:</span><span class="sxs-lookup"><span data-stu-id="ff621-234">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="ff621-235">`@inject` Dyrektywy umożliwia stronę Razor do dodania usługi z [kontenera usług](xref:fundamentals/dependency-injection) w widoku.</span><span class="sxs-lookup"><span data-stu-id="ff621-235">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="ff621-236">Aby uzyskać więcej informacji, zobacz [iniekcji zależności do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ff621-236">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="ff621-237">`@functions` Dyrektywy umożliwia Razor strony do dodania do widoku zawartości na poziomie funkcji:</span><span class="sxs-lookup"><span data-stu-id="ff621-237">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="ff621-238">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ff621-238">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="ff621-239">Kod generuje następujący kod znaczników HTML:</span><span class="sxs-lookup"><span data-stu-id="ff621-239">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="ff621-240">Następujący kod jest wygenerowana klasa Razor C#:</span><span class="sxs-lookup"><span data-stu-id="ff621-240">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="ff621-241">`@section` Dyrektywa jest używany w połączeniu z [układu](xref:mvc/views/layout) umożliwiające widoków do renderowania zawartości w innej części strony HTML.</span><span class="sxs-lookup"><span data-stu-id="ff621-241">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="ff621-242">Aby uzyskać więcej informacji, zobacz [sekcje](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="ff621-242">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="ff621-243">Pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="ff621-243">Tag Helpers</span></span>

<span data-ttu-id="ff621-244">Istnieją trzy dyrektywy, które odnoszą się do [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="ff621-244">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="ff621-245">Dyrektywy</span><span class="sxs-lookup"><span data-stu-id="ff621-245">Directive</span></span> | <span data-ttu-id="ff621-246">Funkcja</span><span class="sxs-lookup"><span data-stu-id="ff621-246">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="ff621-247">Udostępnia pomocników tagów do widoku.</span><span class="sxs-lookup"><span data-stu-id="ff621-247">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="ff621-248">Usuwa pomocników tagów, które wcześniej dodano z widoku.</span><span class="sxs-lookup"><span data-stu-id="ff621-248">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="ff621-249">Określa prefiks tagu, aby włączyć obsługę pomocnika tagów i użycia Pomocnika tagów jawnego.</span><span class="sxs-lookup"><span data-stu-id="ff621-249">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="ff621-250">Razor zastrzeżone słowa kluczowe</span><span class="sxs-lookup"><span data-stu-id="ff621-250">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="ff621-251">Słowa kluczowe razor</span><span class="sxs-lookup"><span data-stu-id="ff621-251">Razor keywords</span></span>

* <span data-ttu-id="ff621-252">Strona (wymaga platformy ASP.NET Core 2.0 i nowsze)</span><span class="sxs-lookup"><span data-stu-id="ff621-252">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="ff621-253">— funkcje</span><span class="sxs-lookup"><span data-stu-id="ff621-253">functions</span></span>
* <span data-ttu-id="ff621-254">dziedziczy</span><span class="sxs-lookup"><span data-stu-id="ff621-254">inherits</span></span>
* <span data-ttu-id="ff621-255">model</span><span class="sxs-lookup"><span data-stu-id="ff621-255">model</span></span>
* <span data-ttu-id="ff621-256">sekcja</span><span class="sxs-lookup"><span data-stu-id="ff621-256">section</span></span>
* <span data-ttu-id="ff621-257">Pomocnik (nie są obecnie obsługiwane przez program ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="ff621-257">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="ff621-258">Słowa kluczowe razor będą miały zmienione znaczenie z `@(Razor Keyword)` (na przykład `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="ff621-258">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="ff621-259">Słowa kluczowe języka C# Razor</span><span class="sxs-lookup"><span data-stu-id="ff621-259">C# Razor keywords</span></span>

* <span data-ttu-id="ff621-260">case</span><span class="sxs-lookup"><span data-stu-id="ff621-260">case</span></span>
* <span data-ttu-id="ff621-261">do</span><span class="sxs-lookup"><span data-stu-id="ff621-261">do</span></span>
* <span data-ttu-id="ff621-262">default</span><span class="sxs-lookup"><span data-stu-id="ff621-262">default</span></span>
* <span data-ttu-id="ff621-263">dla</span><span class="sxs-lookup"><span data-stu-id="ff621-263">for</span></span>
* <span data-ttu-id="ff621-264">foreach</span><span class="sxs-lookup"><span data-stu-id="ff621-264">foreach</span></span>
* <span data-ttu-id="ff621-265">if</span><span class="sxs-lookup"><span data-stu-id="ff621-265">if</span></span>
* <span data-ttu-id="ff621-266">else</span><span class="sxs-lookup"><span data-stu-id="ff621-266">else</span></span>
* <span data-ttu-id="ff621-267">lock</span><span class="sxs-lookup"><span data-stu-id="ff621-267">lock</span></span>
* <span data-ttu-id="ff621-268">— przełącznik</span><span class="sxs-lookup"><span data-stu-id="ff621-268">switch</span></span>
* <span data-ttu-id="ff621-269">Spróbuj</span><span class="sxs-lookup"><span data-stu-id="ff621-269">try</span></span>
* <span data-ttu-id="ff621-270">CATCH</span><span class="sxs-lookup"><span data-stu-id="ff621-270">catch</span></span>
* <span data-ttu-id="ff621-271">finally</span><span class="sxs-lookup"><span data-stu-id="ff621-271">finally</span></span>
* <span data-ttu-id="ff621-272">korzystanie</span><span class="sxs-lookup"><span data-stu-id="ff621-272">using</span></span>
* <span data-ttu-id="ff621-273">while</span><span class="sxs-lookup"><span data-stu-id="ff621-273">while</span></span>

<span data-ttu-id="ff621-274">Słowa kluczowe języka C# Razor musi być podwójne anulowanie z `@(@C# Razor Keyword)` (na przykład `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="ff621-274">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="ff621-275">Pierwszy `@` specjalne analizator Razor.</span><span class="sxs-lookup"><span data-stu-id="ff621-275">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="ff621-276">Drugi `@` specjalne analizatora składni języka C#.</span><span class="sxs-lookup"><span data-stu-id="ff621-276">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="ff621-277">Zastrzeżone słowa kluczowe, które nie są używane przez Razor</span><span class="sxs-lookup"><span data-stu-id="ff621-277">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="ff621-278">— przestrzeń nazw</span><span class="sxs-lookup"><span data-stu-id="ff621-278">namespace</span></span>
* <span data-ttu-id="ff621-279">class</span><span class="sxs-lookup"><span data-stu-id="ff621-279">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="ff621-280">Wyświetlanie klasy Razor C# wygenerowany dla widoku</span><span class="sxs-lookup"><span data-stu-id="ff621-280">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="ff621-281">Dodaj następujące klasy do projektu programu ASP.NET MVC rdzeni:</span><span class="sxs-lookup"><span data-stu-id="ff621-281">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="ff621-282">Zastąpienie `RazorTemplateEngine` dodane przez MVC z `CustomTemplateEngine` klasy:</span><span class="sxs-lookup"><span data-stu-id="ff621-282">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="ff621-283">Ustaw punkt przerwania na `return csharpDocument` instrukcja `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="ff621-283">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="ff621-284">Po przerwaniu wykonywania programu w punkcie przerwania, Wyświetl wartość `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="ff621-284">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Widok wizualizatora tekst generatedCode](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="ff621-286">Widok wyszukiwania i rozróżniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="ff621-286">View lookups and case sensitivity</span></span>

<span data-ttu-id="ff621-287">Aparat widoku Razor wykonuje wyszukiwanie z uwzględnieniem wielkości liter dla widoków.</span><span class="sxs-lookup"><span data-stu-id="ff621-287">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="ff621-288">Jednak rzeczywista wyszukiwania jest określany przez źródłowy system plików:</span><span class="sxs-lookup"><span data-stu-id="ff621-288">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="ff621-289">Źródło opartych na pliku:</span><span class="sxs-lookup"><span data-stu-id="ff621-289">File based source:</span></span> 
  * <span data-ttu-id="ff621-290">W systemach operacyjnych w systemach plików bez uwzględniania wielkości liter (na przykład system Windows) plik fizyczny dostawcy wyszukiwania są bez uwzględniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="ff621-290">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="ff621-291">Na przykład `return View("Test")` powoduje dopasowań dla */Views/Home/Test.cshtml*, */Views/home/test.cshtml*i inne wariant wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="ff621-291">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="ff621-292">W systemach plików z uwzględnieniem wielkości liter (na przykład, Linux, OS x i z `EmbeddedFileProvider`), wyszukiwanie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="ff621-292">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="ff621-293">Na przykład `return View("Test")` dopasowań w szczególności */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ff621-293">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="ff621-294">Wstępnie skompilowana widoków: podstawowych ASP.NET 2.0 i nowsze, wyszukiwania prekompilowanego widoków nie uwzględnia wielkości liter we wszystkich systemach operacyjnych.</span><span class="sxs-lookup"><span data-stu-id="ff621-294">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="ff621-295">Zachowanie jest takie same jak zachowanie dostawcy plików fizycznych w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="ff621-295">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="ff621-296">Jeśli dwa widoki prekompilowany różnią się tylko wielkością liter, wynik wyszukiwania jest deterministyczna.</span><span class="sxs-lookup"><span data-stu-id="ff621-296">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="ff621-297">Deweloperzy są zachęcani do pasuje do wielkości liter nazwy plików i katalogów, aby wielkość liter w wyrazie:</span><span class="sxs-lookup"><span data-stu-id="ff621-297">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="ff621-298">Nazwy obszaru, kontrolera i akcji.</span><span class="sxs-lookup"><span data-stu-id="ff621-298">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="ff621-299">Stron razor.</span><span class="sxs-lookup"><span data-stu-id="ff621-299">Razor Pages.</span></span>
    
<span data-ttu-id="ff621-300">Wielkości liter, gwarantuje, że wdrożeń znaleźć swoje opinie, niezależnie od źródłowy system plików.</span><span class="sxs-lookup"><span data-stu-id="ff621-300">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
