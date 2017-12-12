---
title: "Zapobieganie skryptów krzyżowych"
author: rick-anderson
description: "Tym dokumencie przedstawiono skryptów między witrynami (XSS) i techniki adresowania tę lukę w zabezpieczeniach w aplikacji platformy ASP.NET Core."
keywords: Luki w zabezpieczeniach platformy ASP.NET Core XSS,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 95790927-2bfe-445e-b1fd-429c2c7030ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: fdb26a8338b98135cfc3f6bce9d87285e9a7eb12
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-cross-site-scripting"></a><span data-ttu-id="9e86b-104">Zapobieganie skryptów krzyżowych</span><span class="sxs-lookup"><span data-stu-id="9e86b-104">Preventing Cross-Site Scripting</span></span>

<span data-ttu-id="9e86b-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9e86b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9e86b-106">Wykonywanie skryptów między witrynami (XSS) jest luki w zabezpieczeniach, która umożliwia osobie atakującej umieść skrypty po stronie klienta (zazwyczaj JavaScript) na stronach sieci web.</span><span class="sxs-lookup"><span data-stu-id="9e86b-106">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="9e86b-107">Gdy inni użytkownicy załadować dotyczy stron, które zostaną uruchomione skrypty osoby atakujące, włączanie atakującemu wykradać pliki cookie i tokenów sesji zmiany zawartości strony sieci web za pośrednictwem manipulowania modelu DOM lub przekierowania przeglądarki do innej strony.</span><span class="sxs-lookup"><span data-stu-id="9e86b-107">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="9e86b-108">Luki w zabezpieczeniach XSS ogólnie występują, jeśli aplikacja ma danych wprowadzonych przez użytkownika i zapisuje go na stronie bez konieczności sprawdzania poprawności, kodowanie i anulowanie go.</span><span class="sxs-lookup"><span data-stu-id="9e86b-108">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="9e86b-109">Ochrona aplikacji przed XSS</span><span class="sxs-lookup"><span data-stu-id="9e86b-109">Protecting your application against XSS</span></span>

<span data-ttu-id="9e86b-110">At podstawowe XSS poziomu działa przez aplikację na wstawianie oszukanie `<script>` tag do renderowanej strony lub przez wstawienie `On*` zdarzeń do elementu.</span><span class="sxs-lookup"><span data-stu-id="9e86b-110">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="9e86b-111">Deweloperzy należy używać czynności związanych z zapobieganiem, aby uniknąć wprowadzenia XSS do swojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e86b-111">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="9e86b-112">Nigdy nie poddane niezaufanych danych Twojej input języka HTML, chyba że wykonaj pozostałe czynności.</span><span class="sxs-lookup"><span data-stu-id="9e86b-112">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="9e86b-113">Niezaufane dane są wszystkie dane, które mogą być kontrolowane przez osoba atakująca, dane wejściowe formularza HTML, ciągi zapytań, nagłówki HTTP, nawet danych pochodzących z bazy danych, osoba atakująca może być w stanie naruszenia bazy danych, nawet jeśli ich nie może naruszyć aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e86b-113">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="9e86b-114">Przed wprowadzeniem niezaufanych danych wewnątrz elementu HTML upewnij się, że jest zakodowany w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="9e86b-114">Before putting untrusted data inside an HTML element ensure it is HTML encoded.</span></span> <span data-ttu-id="9e86b-115">Kodowanie HTML przyjmuje takie jak znaki &lt; i zmiany ich w formie bezpieczne, takich jak &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="9e86b-115">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="9e86b-116">Przed wprowadzeniem niezaufanych danych do atrybutu HTML upewnij się, że jest zakodowany atrybutu HTML.</span><span class="sxs-lookup"><span data-stu-id="9e86b-116">Before putting untrusted data into an HTML attribute ensure it is HTML attribute encoded.</span></span> <span data-ttu-id="9e86b-117">Kodowanie atrybutu HTML jest podzbiorem kodowanie HTML i koduje dodatkowe znaki, takie jak "i".</span><span class="sxs-lookup"><span data-stu-id="9e86b-117">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="9e86b-118">Przed wprowadzeniem niezaufanych danych w JavaScript Umieść dane w elemencie HTML, którego zawartość można pobrać w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="9e86b-118">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="9e86b-119">Jeśli to nie jest możliwe, upewnij się, dane są zakodowane JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9e86b-119">If this is not possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="9e86b-120">Kodowanie JavaScript przyjmuje niebezpiecznych znaków dla języka JavaScript i zastępuje je ich hex, na przykład &lt; będą zakodowane jako `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="9e86b-120">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="9e86b-121">Przed wprowadzeniem niezaufanych danych w ciągu kwerendy adresu URL upewnij się, że jest zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="9e86b-121">Before putting untrusted data into a URL query string ensure it is URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="9e86b-122">Kodowanie HTML przy użyciu Razor</span><span class="sxs-lookup"><span data-stu-id="9e86b-122">HTML Encoding using Razor</span></span>

<span data-ttu-id="9e86b-123">Używane w nazwie wzorca MVC automatycznie aparatu Razor koduje wszystkie dane wyjściowe pochodzących z zmiennych, chyba że naprawdę twardego pracy zapobiegania w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="9e86b-123">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="9e86b-124">Używa atrybutu HTML reguły kodowania przy każdym użyciu  *@*  dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="9e86b-124">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="9e86b-125">Jako HTML kodowanie atrybutu jest nadzbiorem kodowanie HTML, który oznacza to, że nie trzeba samodzielnie dotyczą z tego, czy należy użyć kodowanie HTML lub kodowanie atrybutu HTML.</span><span class="sxs-lookup"><span data-stu-id="9e86b-125">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="9e86b-126">Należy się upewnić, czy używać tylko w kontekście HTML nie podczas próby wstawienia niezaufanych danych wejściowych bezpośrednio na język JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9e86b-126">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="9e86b-127">Pomocników tagów będzie również kodowanie danych wejściowych używanych w tagu parametrów.</span><span class="sxs-lookup"><span data-stu-id="9e86b-127">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="9e86b-128">Podejmij następujące widoku Razor;</span><span class="sxs-lookup"><span data-stu-id="9e86b-128">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="9e86b-129">Ten widok wyświetla zawartość *untrustedInput* zmiennej.</span><span class="sxs-lookup"><span data-stu-id="9e86b-129">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="9e86b-130">Ta zmienna zawiera niektóre znaki, które są używane w atakom XSS, czyli &lt;, "i &gt;.</span><span class="sxs-lookup"><span data-stu-id="9e86b-130">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="9e86b-131">Badanie źródło zawiera renderowanych zakodowane jako dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="9e86b-131">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="9e86b-132">Program ASP.NET Core MVC udostępnia `HtmlString` klasy, która nie jest zakodowany automatycznie po danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="9e86b-132">ASP.NET Core MVC provides an `HtmlString` class which is not automatically encoded upon output.</span></span> <span data-ttu-id="9e86b-133">Tej opcji należy nigdy używać w połączeniu z niezaufanych danych wejściowych, ponieważ spowoduje to ujawnienie luki w zabezpieczeniach XSS.</span><span class="sxs-lookup"><span data-stu-id="9e86b-133">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="9e86b-134">Kodowanie JavaScript za pomocą Razor</span><span class="sxs-lookup"><span data-stu-id="9e86b-134">Javascript Encoding using Razor</span></span>

<span data-ttu-id="9e86b-135">Może to być razy wstawiona wartość na język JavaScript do przetworzenia w danym widoku.</span><span class="sxs-lookup"><span data-stu-id="9e86b-135">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="9e86b-136">Istnieją dwa sposoby, w tym celu.</span><span class="sxs-lookup"><span data-stu-id="9e86b-136">There are two ways to do this.</span></span> <span data-ttu-id="9e86b-137">Jest to najbezpieczniejszy sposób na wstawienie wartości prostego umieścić wartości w atrybucie danych tagu i pobrać go w sieci JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9e86b-137">The safest way to insert simple values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="9e86b-138">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="9e86b-138">For example:</span></span>

```none
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

<span data-ttu-id="9e86b-139">Spowoduje to utworzenie poniższy kod HTML</span><span class="sxs-lookup"><span data-stu-id="9e86b-139">This will produce the following HTML</span></span>

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

<span data-ttu-id="9e86b-140">Które, po uruchomieniu spowoduje, że następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="9e86b-140">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="9e86b-141">Można również bezpośrednio wywołać kodera JavaScript</span><span class="sxs-lookup"><span data-stu-id="9e86b-141">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="9e86b-142">To spowoduje, że w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="9e86b-142">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="9e86b-143">Nie łącz niezaufanych danych wejściowych w języku JavaScript do tworzenia modelu DOM elementów.</span><span class="sxs-lookup"><span data-stu-id="9e86b-143">Do not concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="9e86b-144">Należy używać `createElement()` i odpowiednio takich jak przypisywanie wartości właściwości `node.TextContent=`, lub użyj `element.SetAttribute()` / `element[attribute]=` w przeciwnym razie ujawnia siebie na podstawie DOM XSS.</span><span class="sxs-lookup"><span data-stu-id="9e86b-144">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="9e86b-145">Uzyskiwanie dostępu do koderów w kodzie</span><span class="sxs-lookup"><span data-stu-id="9e86b-145">Accessing encoders in code</span></span>

<span data-ttu-id="9e86b-146">Kodery HTML, JavaScript i adres URL są dostępne do kodu na dwa sposoby, można wprowadzić je przy użyciu [iniekcji zależności](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) lub skorzystać z koderów domyślne zawarte w `System.Text.Encodings.Web` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="9e86b-146">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="9e86b-147">Jeśli używasz koderów domyślne wszelkie zastosowane do zakresów znaków, a następnie należy traktować jako bezpieczne nie zacznie obowiązywać — koderów domyślne Użyj najbezpieczniejszy reguł kodowania możliwe.</span><span class="sxs-lookup"><span data-stu-id="9e86b-147">If you use the default encoders then any  you applied to character ranges to be treated as safe will not take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="9e86b-148">Do użycia można konfigurować koderów za pośrednictwem Podpisane z konstruktorów powinno zająć *HtmlEncoder*, *JavaScriptEncoder* i *UrlEncoder* parametru zależnie od potrzeb.</span><span class="sxs-lookup"><span data-stu-id="9e86b-148">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="9e86b-149">Na przykład;</span><span class="sxs-lookup"><span data-stu-id="9e86b-149">For example;</span></span>

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

## <a name="encoding-url-parameters"></a><span data-ttu-id="9e86b-150">Kodowanie parametry adresu URL</span><span class="sxs-lookup"><span data-stu-id="9e86b-150">Encoding URL Parameters</span></span>

<span data-ttu-id="9e86b-151">Jeśli chcesz utworzyć ciągu adresu URL zapytania z niezaufanych danych wejściowych jako Użyj wartości `UrlEncoder` do kodowania wartość.</span><span class="sxs-lookup"><span data-stu-id="9e86b-151">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="9e86b-152">Na przykład</span><span class="sxs-lookup"><span data-stu-id="9e86b-152">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="9e86b-153">Po kodowaniu encodedValue zmienna będzie zawierać `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="9e86b-153">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="9e86b-154">Spacje, cudzysłowy, znaki interpunkcyjne i innych znaków niebezpieczny będzie procent kodowane w celu ich wartość szesnastkowa, na przykład znak spacji staną się % 20.</span><span class="sxs-lookup"><span data-stu-id="9e86b-154">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="9e86b-155">Nie używaj danych wejściowych niezaufanych jako część ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9e86b-155">Do not use untrusted input as part of a URL path.</span></span> <span data-ttu-id="9e86b-156">Zawsze przekazywać niezaufanych danych wejściowych jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="9e86b-156">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="9e86b-157">Dostosowywanie koderów</span><span class="sxs-lookup"><span data-stu-id="9e86b-157">Customizing the Encoders</span></span>

<span data-ttu-id="9e86b-158">Domyślnie koderów przy użyciu listy bezpiecznych ograniczone do zakresu podstawowe Unicode alfabetu łacińskiego, a wszystkie znaki poza tym zakresem jako odpowiedniki kod znaku kodowania.</span><span class="sxs-lookup"><span data-stu-id="9e86b-158">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="9e86b-159">To zachowanie dotyczy również pomocnika tagów Razor i HtmlHelper renderowania zgodnie z koderów zostanie użyty do danych wyjściowych z ciągów.</span><span class="sxs-lookup"><span data-stu-id="9e86b-159">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="9e86b-160">Jest to uzasadnienie do ochrony przed usterki nieznany lub przyszłe przeglądarki (poprzednie błędy przeglądarki ma zwracać się podczas analizowania oparte na przetwarzanie znaków innych niż angielska).</span><span class="sxs-lookup"><span data-stu-id="9e86b-160">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="9e86b-161">Jeśli witryny sieci web umożliwia użycie znaków innych niż łacińskie, takich jak angielski, chiński, cyrylica lub innych to prawdopodobnie nie jest zachowanie, które mają.</span><span class="sxs-lookup"><span data-stu-id="9e86b-161">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="9e86b-162">Można dostosować bezpieczne listy kodera uwzględnienie Unicode zakresów odpowiednie do aplikacji podczas uruchamiania w `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="9e86b-162">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="9e86b-163">Na przykład użycie domyślnej konfiguracji można użyć Razor HtmlHelper, takich jak;</span><span class="sxs-lookup"><span data-stu-id="9e86b-163">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="9e86b-164">Po wyświetleniu źródło strony sieci web pojawi się, że ma zostało wyrenderowane w następujący sposób tekstem chińskiej wersji językowej zakodowane;</span><span class="sxs-lookup"><span data-stu-id="9e86b-164">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="9e86b-165">Aby poszerzyć znaki traktowane jako bezpieczne przez koder jak w przypadku wstawiania następujący wiersz do `ConfigureServices()` metody w `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="9e86b-165">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="9e86b-166">W tym przykładzie rozszerzenie listy bezpiecznych, aby uwzględnić CjkUnifiedIdeographs zakres Unicode.</span><span class="sxs-lookup"><span data-stu-id="9e86b-166">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="9e86b-167">Staje się teraz przetworzonych wyników</span><span class="sxs-lookup"><span data-stu-id="9e86b-167">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="9e86b-168">Listy bezpiecznych zakresy są określone jako wykresów kodu Unicode nie języków.</span><span class="sxs-lookup"><span data-stu-id="9e86b-168">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="9e86b-169">[Unicode standard](http://unicode.org/) zawiera listę [kodu wykresy](http://www.unicode.org/charts/index.html) umożliwia znalezienie wykresu zawierające znaki.</span><span class="sxs-lookup"><span data-stu-id="9e86b-169">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="9e86b-170">Każdy kodera, Html, JavaScript i adres Url, muszą zostać skonfigurowane osobno.</span><span class="sxs-lookup"><span data-stu-id="9e86b-170">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="9e86b-171">Dostosowanie listy bezpiecznych dotyczy tylko koderów powierzając jej ich konserwację za pośrednictwem Podpisane.</span><span class="sxs-lookup"><span data-stu-id="9e86b-171">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="9e86b-172">Jeśli bezpośredni dostęp do kodera za pośrednictwem `System.Text.Encodings.Web.*Encoder.Default` następnie domyślna, podstawowa alfabetu łacińskiego tylko bezpiecznej liście będzie używany.</span><span class="sxs-lookup"><span data-stu-id="9e86b-172">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="9e86b-173">Gdzie należy umieścić podjęcia kodowania</span><span class="sxs-lookup"><span data-stu-id="9e86b-173">Where should encoding take place?</span></span>

<span data-ttu-id="9e86b-174">Ogólne zaakceptowane rozwiązaniem jest, że kodowanie odbywa się w punkcie danych wyjściowych i wartości zakodowanej nigdy nie powinny być przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9e86b-174">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="9e86b-175">Kodowanie w punkcie wyjścia umożliwia zmianę wykorzystanie danych, na przykład z kodu HTML na ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="9e86b-175">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="9e86b-176">Również umożliwia łatwe wyszukiwanie danych bez konieczności kodowania wartości przed wyszukiwaniem i pozwala korzystać z wszelkie zmiany i poprawki dotyczące koderów.</span><span class="sxs-lookup"><span data-stu-id="9e86b-176">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="9e86b-177">Sprawdzanie poprawności jako technikę zapobiegania XSS</span><span class="sxs-lookup"><span data-stu-id="9e86b-177">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="9e86b-178">Sprawdzanie poprawności mogą być przydatne narzędzie ograniczanie atakom XSS.</span><span class="sxs-lookup"><span data-stu-id="9e86b-178">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="9e86b-179">Na przykład prostego ciągu numerycznego zawierającą tylko znaki 0-9, nie powoduje wyzwolenia atak XSS.</span><span class="sxs-lookup"><span data-stu-id="9e86b-179">For example, a simple numeric string containing only the characters 0-9 will not trigger an XSS attack.</span></span> <span data-ttu-id="9e86b-180">Sprawdzanie poprawności staje się bardziej skomplikowany, powinien chcesz zaakceptować HTML w danych wejściowych użytkownika - analiza input języka HTML jest trudne lub niemożliwe.</span><span class="sxs-lookup"><span data-stu-id="9e86b-180">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="9e86b-181">Składni języka markDown i innych formatach tekstowych będą bezpieczniejsze dla zaawansowanych danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="9e86b-181">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="9e86b-182">Nigdy nie należy polegać na samych sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="9e86b-182">You should never rely on validation alone.</span></span> <span data-ttu-id="9e86b-183">Zawsze kodowania niezaufanych dane wejściowe przed danych wyjściowych, niezależnie od tego, jakie walidacji mogły być wykonane.</span><span class="sxs-lookup"><span data-stu-id="9e86b-183">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
