---
title: Zapobiegaj Cross-Site skrypty (XSS) w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat skryptów między witrynami (XSS) i technik do adresowania tę lukę w zabezpieczeniach w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: e937ce47b7151155197cd607832eeb6bf62e3a19
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577447"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="5347c-103">Zapobiegaj Cross-Site skrypty (XSS) w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5347c-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="5347c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5347c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5347c-105">Wykonywanie skryptów między witrynami (XSS) jest luki w zabezpieczeniach, która umożliwia osobie atakującej umieszcza skrypty po stronie klienta (zazwyczaj JavaScript) w stronach sieci web.</span><span class="sxs-lookup"><span data-stu-id="5347c-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="5347c-106">Gdy inni użytkownicy załadować dotyczy stron, które zostaną uruchomione skrypty osoby atakujące, włączanie atakującemu kradzieży plików cookie i tokenów sesji zmiany zawartości strony sieci web za pomocą modelu DOM, manipulowanie lub przekierować przeglądarkę do innej strony.</span><span class="sxs-lookup"><span data-stu-id="5347c-106">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="5347c-107">XSS luk w zabezpieczeniach są zazwyczaj wykonywane, gdy aplikacja akceptuje dane wejściowe użytkownika i wyświetla go na stronie bez weryfikowanie, kodowanie i jej anulowania zapewnianego element.</span><span class="sxs-lookup"><span data-stu-id="5347c-107">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="5347c-108">Ochrona aplikacji przed XSS</span><span class="sxs-lookup"><span data-stu-id="5347c-108">Protecting your application against XSS</span></span>

<span data-ttu-id="5347c-109">Co podstawowa XSS poziomu polega na oszukanie aplikacji do wstawiania `<script>` tag na renderowanej stronie lub przez wstawienie `On*` zdarzeń do elementu.</span><span class="sxs-lookup"><span data-stu-id="5347c-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="5347c-110">Deweloperzy należy używać następujące czynności prewencyjnych, aby uniknąć wprowadzenia XSS do swojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5347c-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="5347c-111">Nigdy nie umieść niezaufane dane swojej input języka HTML, chyba że wykonaj pozostałe czynności.</span><span class="sxs-lookup"><span data-stu-id="5347c-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="5347c-112">Niezaufane dane są wszelkie dane, które mogą być kontrolowane przez osobę atakującą, danych wejściowych z formularza HTML, ciągi zapytań, nagłówki HTTP, a nawet danych, skąd pochodzi z bazy danych, jak osoba atakująca może mieć możliwość naruszenia zabezpieczeń bazy danych, nawet wtedy, gdy nie można ich naruszenia zabezpieczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5347c-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="5347c-113">Przed wprowadzeniem niezaufanych danych wewnątrz elementu HTML upewnij się, że jest zakodowany w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="5347c-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="5347c-114">Kodowanie HTML przyjmuje znaków takich jak &lt; i zmiany ich do bezpiecznego formularza, takich jak &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="5347c-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="5347c-115">Przed wprowadzeniem niezaufane dane do atrybutu HTML upewnij się, że jest zakodowany atrybutu w kodzie HTML.</span><span class="sxs-lookup"><span data-stu-id="5347c-115">Before putting untrusted data into an HTML attribute ensure it's HTML attribute encoded.</span></span> <span data-ttu-id="5347c-116">Kodowanie atrybutu HTML jest podzbiorem kodowanie HTML i koduje znaki dodatkowe, takie jak "a".</span><span class="sxs-lookup"><span data-stu-id="5347c-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="5347c-117">Przed wprowadzeniem niezaufane dane w JavaScript należy umieścić dane w elemencie HTML, którego zawartość, możesz pobrać w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="5347c-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="5347c-118">Jeśli to nie jest możliwe, upewnij się, dane są kodowane języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5347c-118">If this isn't possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="5347c-119">Kodowanie JavaScript przyjmuje niebezpiecznych znaków dla języka JavaScript i zastępuje je ich hex, na przykład &lt; mogłoby być kodowany jako `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="5347c-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="5347c-120">Przed wprowadzeniem niezaufanych danych w ciągu zapytania adresu URL upewnij się, że jest zakodowany w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="5347c-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="5347c-121">Kodowanie HTML przy użyciu Razor</span><span class="sxs-lookup"><span data-stu-id="5347c-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="5347c-122">Używane w MVC automatycznie aparatu Razor koduje wszystkie dane wyjściowe źródło zmiennych, chyba że użytkownik pracuje się naprawdę trudne do zapobiegania jego zresetowanie.</span><span class="sxs-lookup"><span data-stu-id="5347c-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="5347c-123">Używa atrybutu HTML reguły kodowania, zawsze gdy używasz *@* dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="5347c-123">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="5347c-124">Jako HTML kodowanie atrybutu jest nadzbiorem kodowanie HTML, oznacza to, że nie masz obawy samodzielnie za pomocą tego, czy należy używać kodowania HTML lub kodowanie atrybutu HTML.</span><span class="sxs-lookup"><span data-stu-id="5347c-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="5347c-125">Należy się upewnić, że używasz tylko w kontekście HTML nie, podczas próby wstawienia niezaufane dane wejściowe bezpośrednio na język JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5347c-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="5347c-126">Pomocnicy tagów będzie również kodowania danych wejściowych używanych w parametrów tag.</span><span class="sxs-lookup"><span data-stu-id="5347c-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="5347c-127">Wykonaj następujący widok Razor:</span><span class="sxs-lookup"><span data-stu-id="5347c-127">Take the following Razor view:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="5347c-128">Ten widok wyświetla zawartość *untrustedInput* zmiennej.</span><span class="sxs-lookup"><span data-stu-id="5347c-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="5347c-129">Ta zmienna zawiera niektóre znaki, które są używane w atakom XSS, a mianowicie &lt;, "i &gt;.</span><span class="sxs-lookup"><span data-stu-id="5347c-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="5347c-130">Badanie źródła pokazuje wyniku renderowania zakodowanymi w formacie:</span><span class="sxs-lookup"><span data-stu-id="5347c-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="5347c-131">ASP.NET Core MVC udostępnia `HtmlString` klasę, która automatycznie nie jest kodowany na dane wyjściowe.</span><span class="sxs-lookup"><span data-stu-id="5347c-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="5347c-132">To nie mogą być używane w połączeniu z niezaufane dane wejściowe zgodnie z tym umożliwia wyświetlenie luk w zabezpieczeniach XSS.</span><span class="sxs-lookup"><span data-stu-id="5347c-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="5347c-133">JavaScript, kodowanie, za pomocą Razor</span><span class="sxs-lookup"><span data-stu-id="5347c-133">Javascript Encoding using Razor</span></span>

<span data-ttu-id="5347c-134">Mogą wystąpić sytuacje, które chcesz wstawić wartość na język JavaScript do przetworzenia w danym widoku.</span><span class="sxs-lookup"><span data-stu-id="5347c-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="5347c-135">Istnieją dwa sposoby, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="5347c-135">There are two ways to do this.</span></span> <span data-ttu-id="5347c-136">Najbezpieczniejszy sposób wstawiania wartości jest umieszczenie wartości w atrybucie dane tagu i pobierać Twój kod JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5347c-136">The safest way to insert  values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="5347c-137">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5347c-137">For example:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="5347c-138">Pozwoli to osiągnąć, poniższy kod HTML</span><span class="sxs-lookup"><span data-stu-id="5347c-138">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="5347c-139">Który, po jego uruchomieniu, spowoduje, że następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="5347c-139">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="5347c-140">Można również bezpośrednio wywołać kodera JavaScript:</span><span class="sxs-lookup"><span data-stu-id="5347c-140">You can also call the JavaScript encoder directly:</span></span>

```cshtml
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="5347c-141">To spowoduje, że w przeglądarce w następujący sposób;</span><span class="sxs-lookup"><span data-stu-id="5347c-141">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="5347c-142">Nie należy łączyć niezaufane dane wejściowe w języku JavaScript do tworzenia elementów DOM w LICZBIE.</span><span class="sxs-lookup"><span data-stu-id="5347c-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="5347c-143">Należy używać `createElement()` i odpowiednio takich jak przypisywanie wartości właściwości `node.TextContent=`, lub użyj `element.SetAttribute()` / `element[attribute]=` w przeciwnym razie można się narazić na opartej na modelu DOM XSS.</span><span class="sxs-lookup"><span data-stu-id="5347c-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="5347c-144">Uzyskiwanie dostępu do kodery w kodzie</span><span class="sxs-lookup"><span data-stu-id="5347c-144">Accessing encoders in code</span></span>

<span data-ttu-id="5347c-145">Kodery HTML, JavaScript i adres URL są dostępne w kodzie na dwa sposoby, można wstawić je przy użyciu [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) lub można użyć koderów domyślne zawarte w `System.Text.Encodings.Web` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="5347c-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="5347c-146">Użycia kodery domyślne, a następnie dowolne został zastosowany do zakresy znaków należy traktować jako bezpieczne zaczyna obowiązywać — koderów domyślne używane najbezpieczniejszy możliwe reguły kodowania.</span><span class="sxs-lookup"><span data-stu-id="5347c-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="5347c-147">Aby użyć koderów można konfigurować za pośrednictwem DI z konstruktorów powinno zająć *HtmlEncoder*, *JavaScriptEncoder* i *UrlEncoder* parametru, zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="5347c-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="5347c-148">Na przykład;</span><span class="sxs-lookup"><span data-stu-id="5347c-148">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="5347c-149">Kodowanie parametrów adresu URL</span><span class="sxs-lookup"><span data-stu-id="5347c-149">Encoding URL Parameters</span></span>

<span data-ttu-id="5347c-150">Jeśli chcesz utworzyć ciągu zapytania adresu URL, za pomocą niezaufane dane wejściowe jako wartość wykorzystanie `UrlEncoder` zakodować wartości.</span><span class="sxs-lookup"><span data-stu-id="5347c-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="5347c-151">Na przykład</span><span class="sxs-lookup"><span data-stu-id="5347c-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="5347c-152">Po zakodowaniu encodedValue będzie zawierać zmienną `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="5347c-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="5347c-153">Miejsca do magazynowania, oferty, znaki interpunkcyjne i inne znaki niebezpieczne będzie procent zakodowany w celu ich wartość szesnastkową, na przykład znak spacji staną się % 20.</span><span class="sxs-lookup"><span data-stu-id="5347c-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="5347c-154">Nie używaj niezaufane dane wejściowe jako część ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="5347c-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="5347c-155">Zawsze należy przekazać niezaufane dane wejściowe jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="5347c-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="5347c-156">Dostosowywanie koderów</span><span class="sxs-lookup"><span data-stu-id="5347c-156">Customizing the Encoders</span></span>

<span data-ttu-id="5347c-157">Domyślnie koderów, użyj listy bezpiecznych ograniczone do zakresu podstawowe alfabetu łacińskiego Unicode i kodowanie wszystkie znaki spoza tego zakresu jako ich odpowiedników kodów znaku.</span><span class="sxs-lookup"><span data-stu-id="5347c-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="5347c-158">To zachowanie wpływa również na renderowanie pomocnika tagów Razor i HtmlHelper jak w przypadku koderów zostanie użyty do wyprowadzenia Twoimi ciągami.</span><span class="sxs-lookup"><span data-stu-id="5347c-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="5347c-159">Uzasadnienie to jest ochrona przed błędami nieznany lub przyszłe przeglądarki (poprzednie błędy przeglądarki ma wysyłać, zwracać się podczas analizowania oparte na przetwarzanie znaków innych niż angielski).</span><span class="sxs-lookup"><span data-stu-id="5347c-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="5347c-160">Jeśli witryna sieci web sprawia, że intensywne użycie znaków innych niż łacińskie, takich jak chińskich, cyrylica lub inne osoby prawdopodobnie nie jest zachowanie, które chcesz.</span><span class="sxs-lookup"><span data-stu-id="5347c-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="5347c-161">Można dostosować list bezpiecznych kodera obejmujący Unicode zakresów odpowiednimi dla aplikacji, podczas uruchamiania w `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="5347c-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="5347c-162">Na przykład przy użyciu domyślnej konfiguracji można użyć Razor HtmlHelper w następujący sposób;</span><span class="sxs-lookup"><span data-stu-id="5347c-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="5347c-163">Po wyświetleniu źródło strony sieci web pojawi się go zrenderowaniu w następujący sposób, przy użyciu chińskiego tekstu zakodowane;</span><span class="sxs-lookup"><span data-stu-id="5347c-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="5347c-164">Aby poszerzyć znaki traktowane jako bezpieczne przez koder jak w przypadku wstawiania następujący wiersz do `ConfigureServices()` method in Class metoda `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="5347c-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="5347c-165">W tym przykładzie rozszerzenie listy bezpiecznych, aby uwzględnić CjkUnifiedIdeographs zakresu Unicode.</span><span class="sxs-lookup"><span data-stu-id="5347c-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="5347c-166">Staje się teraz wyniku renderowania</span><span class="sxs-lookup"><span data-stu-id="5347c-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="5347c-167">Listy bezpiecznych zakresy są określane jako wykresy kodu Unicode i nie języków.</span><span class="sxs-lookup"><span data-stu-id="5347c-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="5347c-168">[Standardu Unicode](http://unicode.org/) zawiera listę [kodu wykresy](http://www.unicode.org/charts/index.html) umożliwia znalezienie wykresów zawierających znaki.</span><span class="sxs-lookup"><span data-stu-id="5347c-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="5347c-169">Każdy encoder, Html, JavaScript i adres Url, musi być skonfigurowany oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="5347c-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="5347c-170">Dostosowywanie listy bezpiecznych dotyczy tylko koderów źródło za pośrednictwem DI.</span><span class="sxs-lookup"><span data-stu-id="5347c-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="5347c-171">Jeśli bezpośrednio uzyskać dostęp za pośrednictwem koder `System.Text.Encodings.Web.*Encoder.Default` następnie domyślnie Łaciński podstawowy tylko bezpiecznej liście będzie używany.</span><span class="sxs-lookup"><span data-stu-id="5347c-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="5347c-172">Gdzie należy umieścić take kodowania</span><span class="sxs-lookup"><span data-stu-id="5347c-172">Where should encoding take place?</span></span>

<span data-ttu-id="5347c-173">Ogólne zaakceptowane praktyką jest, czy kodowanie odbywa się w punkcie danych wyjściowych i wartości zakodowany nigdy nie powinny być przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5347c-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="5347c-174">Kodowanie w punkcie wyjścia umożliwia zmianę użycie danych, na przykład z kodu HTML do wartości ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="5347c-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="5347c-175">Ponadto pozwala na łatwe wyszukiwanie danych bez konieczności kodowania wartości przed wyszukiwaniem i pozwala na korzystanie z zalet zmiany lub poprawki wprowadzone koderów.</span><span class="sxs-lookup"><span data-stu-id="5347c-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="5347c-176">Sprawdzanie poprawności jako technikę zapobiegania XSS</span><span class="sxs-lookup"><span data-stu-id="5347c-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="5347c-177">Sprawdzanie poprawności może być przydatne narzędzie ograniczanie atakom XSS.</span><span class="sxs-lookup"><span data-stu-id="5347c-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="5347c-178">Na przykład ciągów liczbowych, zawierający tylko znaki 0 – 9 nie spowoduje wyzwolenia ataku XSS.</span><span class="sxs-lookup"><span data-stu-id="5347c-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="5347c-179">Sprawdzanie poprawności staje się bardziej skomplikowane, akceptując HTML w danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5347c-179">Validation becomes more complicated when accepting HTML in user input.</span></span> <span data-ttu-id="5347c-180">Analizowanie input języka HTML jest trudne lub niemożliwe.</span><span class="sxs-lookup"><span data-stu-id="5347c-180">Parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="5347c-181">Markdown, w połączeniu z analizatora, wyodrębniania osadzonego kodu HTML, jest bezpieczniejsze do akceptowania bogate dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="5347c-181">Markdown, coupled with a parser that strips embedded HTML, is a safer option for accepting rich input.</span></span> <span data-ttu-id="5347c-182">Nigdy nie zależą od samego sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="5347c-182">Never rely on validation alone.</span></span> <span data-ttu-id="5347c-183">Zawsze należy zakodować niezaufane dane wejściowe, zanim dane wyjściowe, niezależnie od tego, jakie sprawdzania poprawności lub ich oczyszczania została wykonana.</span><span class="sxs-lookup"><span data-stu-id="5347c-183">Always encode untrusted input before output, no matter what validation or sanitization has been performed.</span></span>
