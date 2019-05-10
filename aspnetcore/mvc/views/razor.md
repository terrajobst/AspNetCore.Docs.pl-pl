---
title: Dokumentacja składni razor dla platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat składni znacznikowania Razor do osadzania kodu na serwerze do stron sieci Web.
ms.author: riande
ms.date: 10/26/2018
uid: mvc/views/razor
ms.openlocfilehash: 7f97be651c067e94f29eef4956c10d87ec031bed
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901108"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="907b1-103">Dokumentacja składni razor dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="907b1-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="907b1-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [MULLENA Taylora](https://twitter.com/ntaylormullen), i [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="907b1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="907b1-105">Razor jest znaczników składnię osadzania kodu na serwerze w stronach sieci Web.</span><span class="sxs-lookup"><span data-stu-id="907b1-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="907b1-106">Składnia Razor składa się z znaczników Razor C#, jak i HTML.</span><span class="sxs-lookup"><span data-stu-id="907b1-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="907b1-107">Pliki zawierające Razor zazwyczaj mają *.cshtml* rozszerzenie pliku.</span><span class="sxs-lookup"><span data-stu-id="907b1-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="907b1-108">Renderowanie kodu HTML</span><span class="sxs-lookup"><span data-stu-id="907b1-108">Rendering HTML</span></span>

<span data-ttu-id="907b1-109">Domyślny język Razor ma format HTML.</span><span class="sxs-lookup"><span data-stu-id="907b1-109">The default Razor language is HTML.</span></span> <span data-ttu-id="907b1-110">Renderowanie kodu HTML, z kodu znaczników Razor nie różni się od renderowania kodu HTML z pliku HTML.</span><span class="sxs-lookup"><span data-stu-id="907b1-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="907b1-111">Kod znaczników HTML w *.cshtml* plikach Razor jest renderowany przez serwer bez zmian.</span><span class="sxs-lookup"><span data-stu-id="907b1-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="907b1-112">Składnia razor</span><span class="sxs-lookup"><span data-stu-id="907b1-112">Razor syntax</span></span>

<span data-ttu-id="907b1-113">Obsługuje razor C# i używa `@` symbol przejście z kodu HTML do C#.</span><span class="sxs-lookup"><span data-stu-id="907b1-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="907b1-114">Ocenia razor C# wyrażeń i renderuje je w danych wyjściowych HTML.</span><span class="sxs-lookup"><span data-stu-id="907b1-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="907b1-115">Gdy `@` następuje symbol [Razor zastrzeżone słowa kluczowego](#razor-reserved-keywords), przechodzi do znaczników specyficzne dla aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="907b1-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="907b1-116">W przeciwnym razie nastąpi przejście do zwykłego C#.</span><span class="sxs-lookup"><span data-stu-id="907b1-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="907b1-117">Wyjścia `@` symboli w znacznikach Razor, należy użyć drugiej `@` symbol:</span><span class="sxs-lookup"><span data-stu-id="907b1-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="907b1-118">Renderowanie kodu HTML za pomocą jednego `@` symbol:</span><span class="sxs-lookup"><span data-stu-id="907b1-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="907b1-119">Atrybuty kodu HTML i zawartość zawierająca adresy e-mail nie traktują `@` symbol znaku przejścia.</span><span class="sxs-lookup"><span data-stu-id="907b1-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="907b1-120">Adresy e-mail w poniższym przykładzie są niezmienione, analiza kodu Razor:</span><span class="sxs-lookup"><span data-stu-id="907b1-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="907b1-121">Niejawne wyrażeń Razor</span><span class="sxs-lookup"><span data-stu-id="907b1-121">Implicit Razor expressions</span></span>

<span data-ttu-id="907b1-122">Niejawne wyrażeń Razor rozpoczynać `@` następuje C# kodu:</span><span class="sxs-lookup"><span data-stu-id="907b1-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="907b1-123">Z wyjątkiem produktów C# `await` — słowo kluczowe, niejawnego wyrażenia nie może zawierać spacji.</span><span class="sxs-lookup"><span data-stu-id="907b1-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="907b1-124">Jeśli C# instrukcji posiada wyraźne zakończenie, miejsca do magazynowania można są:</span><span class="sxs-lookup"><span data-stu-id="907b1-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="907b1-125">Niejawne wyrażeń **nie** zawierają C# ogólne jako znaki w nawiasie (`<>`) są interpretowane jako potraktowane jak tag HTML.</span><span class="sxs-lookup"><span data-stu-id="907b1-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="907b1-126">Poniższy kod jest **nie** prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="907b1-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="907b1-127">Powyższy kod generuje błąd kompilatora podobny do jednego z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="907b1-127">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="907b1-128">Element "int" nie został zamknięty.</span><span class="sxs-lookup"><span data-stu-id="907b1-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="907b1-129">Wszystkie elementy muszą być albo samozamykający lub ma zgodnego tagu końcowego.</span><span class="sxs-lookup"><span data-stu-id="907b1-129">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="907b1-130">Nie można przekonwertować grupy metod "GenericMethod" na typ "object" Niedelegowany.</span><span class="sxs-lookup"><span data-stu-id="907b1-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="907b1-131">Czy zamierzane było wywołanie metody? "</span><span class="sxs-lookup"><span data-stu-id="907b1-131">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="907b1-132">Wywołania metody ogólnej musi być ujęte w [wyrażenie jawne Razor](#explicit-razor-expressions) lub [blok kodu Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="907b1-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="907b1-133">Jawne Razor wyrażeń</span><span class="sxs-lookup"><span data-stu-id="907b1-133">Explicit Razor expressions</span></span>

<span data-ttu-id="907b1-134">Jawne Razor wyrażeń składają się z `@` symbol za pomocą nawiasów o zrównoważonym obciążeniu.</span><span class="sxs-lookup"><span data-stu-id="907b1-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="907b1-135">Aby renderować czas ostatniego tygodnia, jest używany następujący kod Razor:</span><span class="sxs-lookup"><span data-stu-id="907b1-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="907b1-136">Zawartości w obrębie `@()` nawiasów jest obliczane i renderowane w danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="907b1-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="907b1-137">Niejawne wyrażeń, opisanego w poprzedniej sekcji, zazwyczaj nie może zawierać spacji.</span><span class="sxs-lookup"><span data-stu-id="907b1-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="907b1-138">W poniższym kodzie jeden tydzień nie jest odejmowany od bieżącej godziny:</span><span class="sxs-lookup"><span data-stu-id="907b1-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="907b1-139">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="907b1-140">Jawne wyrażeń może służyć do łączenia tekstu z wynikiem wyrażenia:</span><span class="sxs-lookup"><span data-stu-id="907b1-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="907b1-141">Bez wyraźnej wyrażenia `<p>Age@joe.Age</p>` jest traktowany jak adres e-mail i `<p>Age@joe.Age</p>` jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="907b1-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="907b1-142">Podczas zapisywania jako wyrażenie jawne `<p>Age33</p>` jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="907b1-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="907b1-143">Jawne wyrażeń może zostać użyty do renderowania dane wyjściowe z metod ogólnych w *.cshtml* plików.</span><span class="sxs-lookup"><span data-stu-id="907b1-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="907b1-144">Następujący kod pokazuje, jak rozwiązać błąd pokazany wcześniej spowodowane przez nawiasy z C# ogólnego.</span><span class="sxs-lookup"><span data-stu-id="907b1-144">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="907b1-145">Kod jest zapisywany jako jawne wyrażenie:</span><span class="sxs-lookup"><span data-stu-id="907b1-145">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="907b1-146">Wyrażenie kodowania</span><span class="sxs-lookup"><span data-stu-id="907b1-146">Expression encoding</span></span>

<span data-ttu-id="907b1-147">C#wyrażenia, które obliczają do ciągu są kodowania HTML.</span><span class="sxs-lookup"><span data-stu-id="907b1-147">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="907b1-148">C#wyrażenia, które dają `IHtmlContent` — zostaną zrenderowane bezpośrednio za pomocą `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="907b1-148">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="907b1-149">C#wyrażenia, które nie zwrócą `IHtmlContent` są konwertowane na ciąg przez `ToString` i kodowany przed są one renderowane.</span><span class="sxs-lookup"><span data-stu-id="907b1-149">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="907b1-150">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-150">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="907b1-151">Kod HTML jest wyświetlany w przeglądarce jako:</span><span class="sxs-lookup"><span data-stu-id="907b1-151">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="907b1-152">`HtmlHelper.Raw` dane wyjściowe nie jest zakodowany, ale renderowany jako kod znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="907b1-152">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="907b1-153">Za pomocą `HtmlHelper.Raw` unsanitized użytkownika dane wejściowe to zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="907b1-153">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="907b1-154">Dane wejściowe użytkownika może zawierać złośliwego kodu JavaScript lub inne luki w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="907b1-154">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="907b1-155">Podczas czyszczenia danych wejściowych użytkownika jest trudne.</span><span class="sxs-lookup"><span data-stu-id="907b1-155">Sanitizing user input is difficult.</span></span> <span data-ttu-id="907b1-156">Unikaj używania `HtmlHelper.Raw` z danymi wejściowymi użytkownika.</span><span class="sxs-lookup"><span data-stu-id="907b1-156">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="907b1-157">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-157">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="907b1-158">Bloki kodu razor</span><span class="sxs-lookup"><span data-stu-id="907b1-158">Razor code blocks</span></span>

<span data-ttu-id="907b1-159">Bloki kodu razor rozpoczynać `@` i są ujęte w `{}`.</span><span class="sxs-lookup"><span data-stu-id="907b1-159">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="907b1-160">W przeciwieństwie do wyrażenia C# kod wewnątrz bloków kodu nie jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="907b1-160">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="907b1-161">Bloki kodu i wyrażenia w widoku udostępnianie tego samego zakresu i są definiowane w kolejności:</span><span class="sxs-lookup"><span data-stu-id="907b1-161">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="907b1-162">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-162">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="907b1-163">W blokach kodu, należy zadeklarować [funkcje lokalne](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) przy użyciu znaczników, która będzie służyć jako metody tworzenia szablonów:</span><span class="sxs-lookup"><span data-stu-id="907b1-163">In code blocks, declare [local functions](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) with markup to serve as templating methods:</span></span>

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

<span data-ttu-id="907b1-164">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-164">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a><span data-ttu-id="907b1-165">Niejawne przejścia</span><span class="sxs-lookup"><span data-stu-id="907b1-165">Implicit transitions</span></span>

<span data-ttu-id="907b1-166">Jest to domyślny język, w bloku kodu C#, ale strona Razor można przejść do kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-166">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="907b1-167">Jawne przejście rozdzielany</span><span class="sxs-lookup"><span data-stu-id="907b1-167">Explicit delimited transition</span></span>

<span data-ttu-id="907b1-168">Aby zdefiniować podsekcję blok kodu, które mają renderować kod HTML, Otocz znaków do renderowania elementu Razor  **\<tekst >** tag:</span><span class="sxs-lookup"><span data-stu-id="907b1-168">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="907b1-169">Tej metody można użyć do renderowania elementów HTML, który nie jest otoczony potraktowane jak tag HTML.</span><span class="sxs-lookup"><span data-stu-id="907b1-169">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="907b1-170">Bez tagu HTML lub Razor występuje błąd w czasie wykonywania Razor.</span><span class="sxs-lookup"><span data-stu-id="907b1-170">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="907b1-171">**\<Tekst >** tag jest przydatne do kontroli odstępu, podczas renderowania zawartości:</span><span class="sxs-lookup"><span data-stu-id="907b1-171">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="907b1-172">Tylko zawartości między  **\<tekst >** renderowanego tagu.</span><span class="sxs-lookup"><span data-stu-id="907b1-172">Only the content between the **\<text>** tag is rendered.</span></span>
* <span data-ttu-id="907b1-173">Nie spacji przed ani po  **\<tekst >** tagów jest wyświetlana w danych wyjściowych HTML.</span><span class="sxs-lookup"><span data-stu-id="907b1-173">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="907b1-174">Jawne wiersz przejścia z @:</span><span class="sxs-lookup"><span data-stu-id="907b1-174">Explicit Line Transition with @:</span></span>

<span data-ttu-id="907b1-175">Aby renderować pozostałą część całego wiersza jako HTML wewnątrz bloku kodu, należy użyć `@:` składni:</span><span class="sxs-lookup"><span data-stu-id="907b1-175">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="907b1-176">Bez `@:` w kodzie, jest generowany błąd czasu wykonywania Razor.</span><span class="sxs-lookup"><span data-stu-id="907b1-176">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="907b1-177">Ostrzeżenie: Dodatkowe `@` znaków w pliku Razor może spowodować błędy kompilatora w instrukcji w dalszej części tego bloku.</span><span class="sxs-lookup"><span data-stu-id="907b1-177">Warning: Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="907b1-178">Te błędy kompilatora może być trudne do zrozumienia, ponieważ rzeczywista błąd wystąpi przed zgłoszonego błędu.</span><span class="sxs-lookup"><span data-stu-id="907b1-178">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="907b1-179">Ten błąd jest wspólne po połączeniu wiele wyrażeń niejawnej/jawnej w jednym bloku kodu.</span><span class="sxs-lookup"><span data-stu-id="907b1-179">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="907b1-180">Struktury sterujące</span><span class="sxs-lookup"><span data-stu-id="907b1-180">Control structures</span></span>

<span data-ttu-id="907b1-181">Struktury sterujące stanowią rozszerzenie bloków kodu.</span><span class="sxs-lookup"><span data-stu-id="907b1-181">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="907b1-182">Wszystkie aspekty bloki kodu (przejście do znaczników, wbudowane C#) dotyczą również następujące struktury:</span><span class="sxs-lookup"><span data-stu-id="907b1-182">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="907b1-183">Instrukcje warunkowe @if, jeśli nie, inny, i @switch</span><span class="sxs-lookup"><span data-stu-id="907b1-183">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="907b1-184">`@if` formanty, gdy kod jest wykonywany:</span><span class="sxs-lookup"><span data-stu-id="907b1-184">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="907b1-185">`else` i `else if` nie wymagają `@` symbol:</span><span class="sxs-lookup"><span data-stu-id="907b1-185">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="907b1-186">Następujący kod przedstawia sposób użycia instrukcji switch:</span><span class="sxs-lookup"><span data-stu-id="907b1-186">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="907b1-187">Tworzenie pętli @for, @foreach, @while, i @do podczas</span><span class="sxs-lookup"><span data-stu-id="907b1-187">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="907b1-188">Oparte na szablonach HTML może być renderowany przy użyciu pętli instrukcji sterowania.</span><span class="sxs-lookup"><span data-stu-id="907b1-188">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="907b1-189">Aby renderować listy osób:</span><span class="sxs-lookup"><span data-stu-id="907b1-189">To render a list of people:</span></span>

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

<span data-ttu-id="907b1-190">Obsługiwane są następujące instrukcje pętli:</span><span class="sxs-lookup"><span data-stu-id="907b1-190">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="907b1-191">Złożona (c) @using</span><span class="sxs-lookup"><span data-stu-id="907b1-191">Compound @using</span></span>

<span data-ttu-id="907b1-192">W C#, `using` instrukcja jest używane, aby upewnić się, obiekt zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="907b1-192">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="907b1-193">W elemencie Razor ten sam mechanizm służy do tworzenia pomocników HTML, który zawiera dodatkową zawartość.</span><span class="sxs-lookup"><span data-stu-id="907b1-193">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="907b1-194">W poniższym kodzie pomocników HTML, renderowania tag formularza z `@using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="907b1-194">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="907b1-195">Można wykonywać działania na poziomie zakresu za pomocą [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="907b1-195">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="907b1-196">@try, catch finally</span><span class="sxs-lookup"><span data-stu-id="907b1-196">@try, catch, finally</span></span>

<span data-ttu-id="907b1-197">Obsługa wyjątków jest podobne do C#:</span><span class="sxs-lookup"><span data-stu-id="907b1-197">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="907b1-198">Razor ma możliwość ochrony sekcje krytyczne w instrukcjach blokady:</span><span class="sxs-lookup"><span data-stu-id="907b1-198">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="907b1-199">Komentarze</span><span class="sxs-lookup"><span data-stu-id="907b1-199">Comments</span></span>

<span data-ttu-id="907b1-200">Obsługuje razor C# i komentarze HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-200">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="907b1-201">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-201">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="907b1-202">Komentarz razor są usuwane przez serwer przed wyświetleniem strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="907b1-202">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="907b1-203">Korzysta z aparatu razor `@*  *@` ograniczającej komentarzy.</span><span class="sxs-lookup"><span data-stu-id="907b1-203">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="907b1-204">Poniższy kod jest zakomentowany, więc serwer nie renderuje żadnych znaczników:</span><span class="sxs-lookup"><span data-stu-id="907b1-204">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="907b1-205">Dyrektyw</span><span class="sxs-lookup"><span data-stu-id="907b1-205">Directives</span></span>

<span data-ttu-id="907b1-206">Dyrektywy razor są reprezentowane przez wyrażenia niejawne następujących zastrzeżonych słów kluczowych `@` symboli.</span><span class="sxs-lookup"><span data-stu-id="907b1-206">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="907b1-207">Dyrektywy zazwyczaj zmienia sposób, w widoku jest analizowany lub włącza inną funkcję.</span><span class="sxs-lookup"><span data-stu-id="907b1-207">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="907b1-208">Zrozumienie, jak Razor generuje kod dla widoku ułatwia zrozumienie, jak działają dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="907b1-208">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="907b1-209">Kod generuje klasę podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="907b1-209">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="907b1-210">W dalszej części tego artykułu sekcji [sprawdzić Razor C# klasy wygenerowanej w celu wyświetlenia](#inspect-the-razor-c-class-generated-for-a-view) opisano sposób wyświetlania tego wygenerowanej klasy.</span><span class="sxs-lookup"><span data-stu-id="907b1-210">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

<a name="using"></a>

### <a name="using"></a>@using

<span data-ttu-id="907b1-211">`@using` Dodaje dyrektywy C# `using` dyrektywy do wygenerowanego widoku:</span><span class="sxs-lookup"><span data-stu-id="907b1-211">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="907b1-212">`@model` Dyrektywa określa typ modelu przekazywane do widoku:</span><span class="sxs-lookup"><span data-stu-id="907b1-212">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="907b1-213">W aplikacji ASP.NET Core MVC utworzonych za pomocą indywidualnych kont użytkowników *Views/Account/Login.cshtml* widok zawiera następującą deklarację modelu:</span><span class="sxs-lookup"><span data-stu-id="907b1-213">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="907b1-214">Dziedziczy po klasie, wygenerowany `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="907b1-214">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="907b1-215">Udostępnia razor `Model` właściwości do uzyskiwania dostępu do modelu przekazywane do widoku:</span><span class="sxs-lookup"><span data-stu-id="907b1-215">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="907b1-216">`@model` Dyrektywa określa typ tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="907b1-216">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="907b1-217">Dyrektywa określa `T` w `RazorPage<T>` czy wygenerowanej klasy, która widok pochodzi od klasy.</span><span class="sxs-lookup"><span data-stu-id="907b1-217">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="907b1-218">Jeśli `@model` dyrektywy nie jest określona, `Model` właściwość jest typu `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="907b1-218">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="907b1-219">Wartość modelu jest przekazywany z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="907b1-219">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="907b1-220">Aby uzyskać więcej informacji, zobacz [silnie typizowane modeli i &commat;modelu — słowo kluczowe](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span><span class="sxs-lookup"><span data-stu-id="907b1-220">For more information, see [Strongly typed models and the &commat;model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="907b1-221">`@inherits` Dyrektywy umożliwia pełną kontrolę nad klasa dziedziczy widoku:</span><span class="sxs-lookup"><span data-stu-id="907b1-221">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="907b1-222">Poniższy kod jest niestandardowy typ strony Razor:</span><span class="sxs-lookup"><span data-stu-id="907b1-222">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="907b1-223">`CustomText` Jest wyświetlany w widoku:</span><span class="sxs-lookup"><span data-stu-id="907b1-223">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="907b1-224">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-224">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="907b1-225">`@model` i `@inherits` mogą być używane w jednym widoku.</span><span class="sxs-lookup"><span data-stu-id="907b1-225">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="907b1-226">`@inherits` mogą znajdować się w *_ViewImports.cshtml* pliku, który importuje widoku:</span><span class="sxs-lookup"><span data-stu-id="907b1-226">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="907b1-227">Poniższy kod jest przykładem silnie typizowane widoku:</span><span class="sxs-lookup"><span data-stu-id="907b1-227">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="907b1-228">Jeśli "rick@contoso.com" jest przekazywany w modelu widoku generuje następujący kod znaczników HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-228">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="907b1-229">`@inject` Dyrektywy umożliwia strony Razor, aby wstawić usługi z [kontener usługi](xref:fundamentals/dependency-injection) w widoku.</span><span class="sxs-lookup"><span data-stu-id="907b1-229">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="907b1-230">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="907b1-230">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="907b1-231">`@functions` Dyrektywy umożliwia stronę Razor, aby dodać C# blok kodu do widoku:</span><span class="sxs-lookup"><span data-stu-id="907b1-231">The `@functions` directive enables a Razor Page to add a C# code block to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="907b1-232">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="907b1-232">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="907b1-233">Kod generuje następujący kod znaczników HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-233">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="907b1-234">Poniższy kod jest wygenerowany Razor C# klasy:</span><span class="sxs-lookup"><span data-stu-id="907b1-234">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="907b1-235">`@functions` metody służą jako metody tworzenia szablonów, gdy kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="907b1-235">`@functions` methods serve as templating methods when they have markup:</span></span>

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

<span data-ttu-id="907b1-236">Ten kod Renderuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="907b1-236">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="section"></a>@section

<span data-ttu-id="907b1-237">`@section` Dyrektywa jest używany w połączeniu z [układ](xref:mvc/views/layout) do przedstawiania do renderowania zawartości w różnych częściach strony HTML.</span><span class="sxs-lookup"><span data-stu-id="907b1-237">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="907b1-238">Aby uzyskać więcej informacji, zobacz [sekcje](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="907b1-238">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="templated-razor-delegates"></a><span data-ttu-id="907b1-239">Oparte na szablonach delegatów Razor</span><span class="sxs-lookup"><span data-stu-id="907b1-239">Templated Razor delegates</span></span>

<span data-ttu-id="907b1-240">Szablony razor umożliwiają definiowanie fragmentu interfejsu użytkownika w następującym formacie:</span><span class="sxs-lookup"><span data-stu-id="907b1-240">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="907b1-241">Poniższy przykład ilustruje sposób określania oparte na szablonach delegata Razor jako <xref:System.Func%602>.</span><span class="sxs-lookup"><span data-stu-id="907b1-241">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func%602>.</span></span> <span data-ttu-id="907b1-242">[Typu dynamicznego](/dotnet/csharp/programming-guide/types/using-type-dynamic) jest określona dla parametru metody, która hermetyzuje delegata.</span><span class="sxs-lookup"><span data-stu-id="907b1-242">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="907b1-243">[Typ obiektu](/dotnet/csharp/language-reference/keywords/object) jest określony jako wartość zwracaną przez delegata.</span><span class="sxs-lookup"><span data-stu-id="907b1-243">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="907b1-244">Ten szablon jest używany z <xref:System.Collections.Generic.List%601> z `Pet` zawierający `Name` właściwości.</span><span class="sxs-lookup"><span data-stu-id="907b1-244">The template is used with a <xref:System.Collections.Generic.List%601> of `Pet` that has a `Name` property.</span></span>

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

<span data-ttu-id="907b1-245">Szablon jest renderowany przy użyciu `pets` dostarczonych przez `foreach` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="907b1-245">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="907b1-246">Wyniku renderowania:</span><span class="sxs-lookup"><span data-stu-id="907b1-246">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="907b1-247">Możesz również dostarczyć wbudowany szablon Razor, jako argument do metody.</span><span class="sxs-lookup"><span data-stu-id="907b1-247">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="907b1-248">W poniższym przykładzie `Repeat` metoda odbiera szablon Razor.</span><span class="sxs-lookup"><span data-stu-id="907b1-248">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="907b1-249">Metoda używa tego szablonu w celu wygenerowania zawartość HTML z powtórzeń elementy dostarczane z listy:</span><span class="sxs-lookup"><span data-stu-id="907b1-249">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

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

<span data-ttu-id="907b1-250">Za pomocą listę zwierząt domowych, z poprzedniego przykładu `Repeat` metoda jest wywoływana z:</span><span class="sxs-lookup"><span data-stu-id="907b1-250">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="907b1-251"><xref:System.Collections.Generic.List%601> z `Pet`.</span><span class="sxs-lookup"><span data-stu-id="907b1-251"><xref:System.Collections.Generic.List%601> of `Pet`.</span></span>
* <span data-ttu-id="907b1-252">Liczba powtórzeń każdego pet.</span><span class="sxs-lookup"><span data-stu-id="907b1-252">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="907b1-253">Wbudowany szablon do użycia dla elementów listy, listy nieuporządkowane.</span><span class="sxs-lookup"><span data-stu-id="907b1-253">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="907b1-254">Wyniku renderowania:</span><span class="sxs-lookup"><span data-stu-id="907b1-254">Rendered output:</span></span>

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

## <a name="tag-helpers"></a><span data-ttu-id="907b1-255">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="907b1-255">Tag Helpers</span></span>

<span data-ttu-id="907b1-256">Istnieją trzy dyrektyw, które odnoszą się do [pomocników tagów](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="907b1-256">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="907b1-257">— Dyrektywa</span><span class="sxs-lookup"><span data-stu-id="907b1-257">Directive</span></span> | <span data-ttu-id="907b1-258">Funkcja</span><span class="sxs-lookup"><span data-stu-id="907b1-258">Function</span></span> |
| --------- | -------- |
| [<span data-ttu-id="907b1-259">&commat;addTagHelper</span><span class="sxs-lookup"><span data-stu-id="907b1-259">&commat;addTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="907b1-260">Udostępnia pomocników tagów do widoku.</span><span class="sxs-lookup"><span data-stu-id="907b1-260">Makes Tag Helpers available to a view.</span></span> |
| [<span data-ttu-id="907b1-261">&commat;removeTagHelper</span><span class="sxs-lookup"><span data-stu-id="907b1-261">&commat;removeTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="907b1-262">Usuwa pomocnicy tagów, wcześniej dodany z widoku.</span><span class="sxs-lookup"><span data-stu-id="907b1-262">Removes Tag Helpers previously added from a view.</span></span> |
| [<span data-ttu-id="907b1-263">&commat;tagHelperPrefix</span><span class="sxs-lookup"><span data-stu-id="907b1-263">&commat;tagHelperPrefix</span></span>](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="907b1-264">Określa prefiks tagu, aby włączyć obsługę pomocnika tagów i Pomocnik tagu użycie jawnego.</span><span class="sxs-lookup"><span data-stu-id="907b1-264">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="907b1-265">Razor zastrzeżone słowa kluczowe</span><span class="sxs-lookup"><span data-stu-id="907b1-265">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="907b1-266">Słowa kluczowe razor</span><span class="sxs-lookup"><span data-stu-id="907b1-266">Razor keywords</span></span>

* <span data-ttu-id="907b1-267">Strona (wymaga platformy ASP.NET Core 2.0 i nowsze)</span><span class="sxs-lookup"><span data-stu-id="907b1-267">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="907b1-268">— przestrzeń nazw</span><span class="sxs-lookup"><span data-stu-id="907b1-268">namespace</span></span>
* <span data-ttu-id="907b1-269">— funkcje</span><span class="sxs-lookup"><span data-stu-id="907b1-269">functions</span></span>
* <span data-ttu-id="907b1-270">Inherits</span><span class="sxs-lookup"><span data-stu-id="907b1-270">inherits</span></span>
* <span data-ttu-id="907b1-271">model</span><span class="sxs-lookup"><span data-stu-id="907b1-271">model</span></span>
* <span data-ttu-id="907b1-272">sekcja</span><span class="sxs-lookup"><span data-stu-id="907b1-272">section</span></span>
* <span data-ttu-id="907b1-273">Pomocnik (nie jest obecnie obsługiwane przez program ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="907b1-273">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="907b1-274">Razor słowa kluczowe są poprzedzone znakiem zmiany znaczenia z `@(Razor Keyword)` (na przykład `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="907b1-274">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="907b1-275">C#Słowa kluczowe razor</span><span class="sxs-lookup"><span data-stu-id="907b1-275">C# Razor keywords</span></span>

* <span data-ttu-id="907b1-276">case</span><span class="sxs-lookup"><span data-stu-id="907b1-276">case</span></span>
* <span data-ttu-id="907b1-277">do</span><span class="sxs-lookup"><span data-stu-id="907b1-277">do</span></span>
* <span data-ttu-id="907b1-278">default</span><span class="sxs-lookup"><span data-stu-id="907b1-278">default</span></span>
* <span data-ttu-id="907b1-279">dla</span><span class="sxs-lookup"><span data-stu-id="907b1-279">for</span></span>
* <span data-ttu-id="907b1-280">foreach</span><span class="sxs-lookup"><span data-stu-id="907b1-280">foreach</span></span>
* <span data-ttu-id="907b1-281">if</span><span class="sxs-lookup"><span data-stu-id="907b1-281">if</span></span>
* <span data-ttu-id="907b1-282">else</span><span class="sxs-lookup"><span data-stu-id="907b1-282">else</span></span>
* <span data-ttu-id="907b1-283">lock</span><span class="sxs-lookup"><span data-stu-id="907b1-283">lock</span></span>
* <span data-ttu-id="907b1-284">— przełącznik</span><span class="sxs-lookup"><span data-stu-id="907b1-284">switch</span></span>
* <span data-ttu-id="907b1-285">Wypróbuj</span><span class="sxs-lookup"><span data-stu-id="907b1-285">try</span></span>
* <span data-ttu-id="907b1-286">CATCH</span><span class="sxs-lookup"><span data-stu-id="907b1-286">catch</span></span>
* <span data-ttu-id="907b1-287">finally</span><span class="sxs-lookup"><span data-stu-id="907b1-287">finally</span></span>
* <span data-ttu-id="907b1-288">korzystanie</span><span class="sxs-lookup"><span data-stu-id="907b1-288">using</span></span>
* <span data-ttu-id="907b1-289">while</span><span class="sxs-lookup"><span data-stu-id="907b1-289">while</span></span>

<span data-ttu-id="907b1-290">C#Słowa kluczowe razor musi być double ucieczki `@(@C# Razor Keyword)` (na przykład `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="907b1-290">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="907b1-291">Pierwszy `@` specjalne analizator Razor.</span><span class="sxs-lookup"><span data-stu-id="907b1-291">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="907b1-292">Drugi `@` specjalne C# analizatora.</span><span class="sxs-lookup"><span data-stu-id="907b1-292">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="907b1-293">Zastrzeżone słowa kluczowe nie użył ich Razor</span><span class="sxs-lookup"><span data-stu-id="907b1-293">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="907b1-294">class</span><span class="sxs-lookup"><span data-stu-id="907b1-294">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="907b1-295">Zbadaj Razor C# klasa generowana dla widoku</span><span class="sxs-lookup"><span data-stu-id="907b1-295">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="907b1-296">Za pomocą platformy .NET Core SDK 2.1 lub nowszej [Razor SDK](xref:razor-pages/sdk) obsługuje kompilacji plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="907b1-296">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="907b1-297">Podczas kompilowania projektu, generuje zestaw Razor SDK *obj / < build_configuration > / < target_framework_moniker > / Razor* katalogu w folderze głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="907b1-297">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="907b1-298">Struktura katalogów w ramach *Razor* katalogu odzwierciedla strukturze katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="907b1-298">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="907b1-299">Rozważmy następującą strukturę katalogów w projekcie stron Razor programu ASP.NET Core 2.1 przeznaczonych dla platformy .NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="907b1-299">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="907b1-300">**Obszary /**</span><span class="sxs-lookup"><span data-stu-id="907b1-300">**Areas/**</span></span>
  * <span data-ttu-id="907b1-301">**Administrator /**</span><span class="sxs-lookup"><span data-stu-id="907b1-301">**Admin/**</span></span>
    * <span data-ttu-id="907b1-302">**Strony /**</span><span class="sxs-lookup"><span data-stu-id="907b1-302">**Pages/**</span></span>
      * <span data-ttu-id="907b1-303">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="907b1-303">*Index.cshtml*</span></span>
      * <span data-ttu-id="907b1-304">*Index.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="907b1-304">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="907b1-305">**Strony /**</span><span class="sxs-lookup"><span data-stu-id="907b1-305">**Pages/**</span></span>
  * <span data-ttu-id="907b1-306">**Udostępnione /**</span><span class="sxs-lookup"><span data-stu-id="907b1-306">**Shared/**</span></span>
    * <span data-ttu-id="907b1-307">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="907b1-307">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="907b1-308">*_ViewImports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="907b1-308">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="907b1-309">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="907b1-309">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="907b1-310">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="907b1-310">*Index.cshtml*</span></span>
  * <span data-ttu-id="907b1-311">*Index.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="907b1-311">*Index.cshtml.cs*</span></span>

<span data-ttu-id="907b1-312">Tworzenie projektu na platformie *debugowania* konfiguracji daje następujące *obj* katalogu:</span><span class="sxs-lookup"><span data-stu-id="907b1-312">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="907b1-313">**obj /**</span><span class="sxs-lookup"><span data-stu-id="907b1-313">**obj/**</span></span>
  * <span data-ttu-id="907b1-314">**Debugowanie /**</span><span class="sxs-lookup"><span data-stu-id="907b1-314">**Debug/**</span></span>
    * <span data-ttu-id="907b1-315">**netcoreapp2.1 /**</span><span class="sxs-lookup"><span data-stu-id="907b1-315">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="907b1-316">**Razor /**</span><span class="sxs-lookup"><span data-stu-id="907b1-316">**Razor/**</span></span>
        * <span data-ttu-id="907b1-317">**Obszary /**</span><span class="sxs-lookup"><span data-stu-id="907b1-317">**Areas/**</span></span>
          * <span data-ttu-id="907b1-318">**Administrator /**</span><span class="sxs-lookup"><span data-stu-id="907b1-318">**Admin/**</span></span>
            * <span data-ttu-id="907b1-319">**Strony /**</span><span class="sxs-lookup"><span data-stu-id="907b1-319">**Pages/**</span></span>
              * <span data-ttu-id="907b1-320">*Index.g.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="907b1-320">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="907b1-321">**Strony /**</span><span class="sxs-lookup"><span data-stu-id="907b1-321">**Pages/**</span></span>
          * <span data-ttu-id="907b1-322">**Udostępnione /**</span><span class="sxs-lookup"><span data-stu-id="907b1-322">**Shared/**</span></span>
            * <span data-ttu-id="907b1-323">*_Layout.g.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="907b1-323">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="907b1-324">*_ViewImports.g.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="907b1-324">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="907b1-325">*_ViewStart.g.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="907b1-325">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="907b1-326">*Index.g.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="907b1-326">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="907b1-327">Aby wyświetlić wygenerowanej klasy dla *Pages/Index.cshtml*, otwórz *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="907b1-327">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="907b1-328">Dodaj poniższą klasę do projektu programu ASP.NET Core MVC:</span><span class="sxs-lookup"><span data-stu-id="907b1-328">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="907b1-329">W `Startup.ConfigureServices`, Zastąp `RazorTemplateEngine` dodane przez wzorzec MVC z `CustomTemplateEngine` klasy:</span><span class="sxs-lookup"><span data-stu-id="907b1-329">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="907b1-330">Ustaw punkt przerwania na `return csharpDocument;` instrukcja `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="907b1-330">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="907b1-331">Po zatrzymaniu w punkcie przerwania wykonywania programu wyświetlić wartość `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="907b1-331">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![Widok Wizualizator tekstu generatedCode](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="907b1-333">Widok wyszukiwania i rozróżnianie wielkości liter</span><span class="sxs-lookup"><span data-stu-id="907b1-333">View lookups and case sensitivity</span></span>

<span data-ttu-id="907b1-334">Aparat widoku Razor wykonuje wyszukiwanie z uwzględnieniem wielkości liter dla widoków.</span><span class="sxs-lookup"><span data-stu-id="907b1-334">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="907b1-335">Jednak rzeczywista wyszukiwania jest określany przez sam system plików:</span><span class="sxs-lookup"><span data-stu-id="907b1-335">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="907b1-336">Na podstawie źródła pliku:</span><span class="sxs-lookup"><span data-stu-id="907b1-336">File based source:</span></span>
  * <span data-ttu-id="907b1-337">W systemach operacyjnych z systemami plików bez uwzględniania wielkości liter (na przykład Windows) plik fizyczny dostawcy wyszukiwania jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="907b1-337">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="907b1-338">Na przykład `return View("Test")` skutkuje dopasowań dla */Views/Home/Test.cshtml*, */Views/home/test.cshtml*i inne wariant wielkość liter w wyrazie.</span><span class="sxs-lookup"><span data-stu-id="907b1-338">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="907b1-339">W systemach plików rozróżniana wielkość liter (na przykład, Linux, OSX i `EmbeddedFileProvider`), wyszukiwania jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="907b1-339">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="907b1-340">Na przykład `return View("Test")` specjalnie dopasowania */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="907b1-340">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="907b1-341">Prekompilowany widoki: Za pomocą platformy ASP.NET Core 2.0 i nowszych wyszukiwania prekompilowanego widoków jest uwzględniana wielkość liter, we wszystkich systemach operacyjnych.</span><span class="sxs-lookup"><span data-stu-id="907b1-341">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="907b1-342">Zachowanie jest taka sama jak zachowanie dostawcy plików fizycznych w Windows.</span><span class="sxs-lookup"><span data-stu-id="907b1-342">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="907b1-343">Jeśli dwa widoki wstępnie skompilowanych różnią się tylko wielkością liter, wynikiem wyszukiwania jest niedeterministyczny.</span><span class="sxs-lookup"><span data-stu-id="907b1-343">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="907b1-344">Deweloperzy są zachęcani do pasuje do wielkości liter w nazwach plików i katalogów na wielkość liter w wyrazie:</span><span class="sxs-lookup"><span data-stu-id="907b1-344">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="907b1-345">Nazwy obszaru, kontrolera i akcji.</span><span class="sxs-lookup"><span data-stu-id="907b1-345">Area, controller, and action names.</span></span>
* <span data-ttu-id="907b1-346">Strony razor.</span><span class="sxs-lookup"><span data-stu-id="907b1-346">Razor Pages.</span></span>

<span data-ttu-id="907b1-347">Wielkości liter, gwarantuje, że wdrożeń Znajdź swoje opinie, niezależnie od tego, źródłowy system plików.</span><span class="sxs-lookup"><span data-stu-id="907b1-347">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
