---
title: Zapobiegaj krzyżowe skryptów (XSS) w platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat skryptów między witrynami (XSS) i techniki adresowania tę lukę w zabezpieczeniach w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/cross-site-scripting
ms.openlocfilehash: ce6bb273034c56890e0cd98b890436602b5acc69
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272451"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>Zapobiegaj krzyżowe skryptów (XSS) w platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Wykonywanie skryptów między witrynami (XSS) jest luki w zabezpieczeniach, która umożliwia osobie atakującej umieść skrypty po stronie klienta (zazwyczaj JavaScript) na stronach sieci web. Gdy inni użytkownicy załadować dotyczy stron, które zostaną uruchomione skrypty osoby atakujące, włączanie atakującemu wykradać pliki cookie i tokenów sesji zmiany zawartości strony sieci web za pośrednictwem manipulowania modelu DOM lub przekierowania przeglądarki do innej strony. Luki w zabezpieczeniach XSS ogólnie występują, jeśli aplikacja ma danych wprowadzonych przez użytkownika i zapisuje go na stronie bez konieczności sprawdzania poprawności, kodowanie i anulowanie go.

## <a name="protecting-your-application-against-xss"></a>Ochrona aplikacji przed XSS

At podstawowe XSS poziomu działa przez aplikację na wstawianie oszukanie `<script>` tag do renderowanej strony lub przez wstawienie `On*` zdarzeń do elementu. Deweloperzy należy używać czynności związanych z zapobieganiem, aby uniknąć wprowadzenia XSS do swojej aplikacji.

1. Nigdy nie poddane niezaufanych danych Twojej input języka HTML, chyba że wykonaj pozostałe czynności. Niezaufane dane są wszystkie dane, które mogą być kontrolowane przez osoba atakująca, dane wejściowe formularza HTML, ciągi zapytań, nagłówki HTTP, nawet danych pochodzących z bazy danych, osoba atakująca może być w stanie naruszenia bazy danych, nawet jeśli ich nie może naruszyć aplikacji.

2. Przed wprowadzeniem niezaufanych danych wewnątrz elementu HTML upewnij się, że jest zakodowany w formacie HTML. Kodowanie HTML przyjmuje takie jak znaki &lt; i zmiany ich w formie bezpieczne, takich jak &amp;lt;

3. Przed wprowadzeniem niezaufanych danych do atrybutu HTML upewnij się, że jest zakodowany atrybutu HTML. Kodowanie atrybutu HTML jest podzbiorem kodowanie HTML i koduje dodatkowe znaki, takie jak "i".

4. Przed wprowadzeniem niezaufanych danych w JavaScript Umieść dane w elemencie HTML, którego zawartość można pobrać w czasie wykonywania. Jeśli to nie jest możliwe, upewnij się, dane są zakodowane JavaScript. Kodowanie JavaScript przyjmuje niebezpiecznych znaków dla języka JavaScript i zastępuje je ich hex, na przykład &lt; będą zakodowane jako `\u003C`.

5. Przed wprowadzeniem niezaufanych danych w ciągu kwerendy adresu URL upewnij się, że jest zakodowane w adresie URL.

## <a name="html-encoding-using-razor"></a>Kodowanie HTML przy użyciu Razor

Używane w nazwie wzorca MVC automatycznie aparatu Razor koduje wszystkie dane wyjściowe pochodzących z zmiennych, chyba że naprawdę twardego pracy zapobiegania w ten sposób. Używa atrybutu HTML reguły kodowania przy każdym użyciu *@* dyrektywy. Jako HTML kodowanie atrybutu jest nadzbiorem kodowanie HTML, który oznacza to, że nie trzeba samodzielnie dotyczą z tego, czy należy użyć kodowanie HTML lub kodowanie atrybutu HTML. Należy się upewnić, czy używać tylko w kontekście HTML nie podczas próby wstawienia niezaufanych danych wejściowych bezpośrednio na język JavaScript. Pomocników tagów będzie również kodowanie danych wejściowych używanych w tagu parametrów.

Podejmij następujące widoku Razor;

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Ten widok wyświetla zawartość *untrustedInput* zmiennej. Ta zmienna zawiera niektóre znaki, które są używane w atakom XSS, czyli &lt;, "i &gt;. Badanie źródło zawiera renderowanych zakodowane jako dane wyjściowe:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> Program ASP.NET Core MVC udostępnia `HtmlString` klasy, które nie jest automatycznie kodowane po danych wyjściowych. Tej opcji należy nigdy używać w połączeniu z niezaufanych danych wejściowych, ponieważ spowoduje to ujawnienie luki w zabezpieczeniach XSS.

## <a name="javascript-encoding-using-razor"></a>Kodowanie JavaScript za pomocą Razor

Może to być razy wstawiona wartość na język JavaScript do przetworzenia w danym widoku. Istnieją dwa sposoby, w tym celu. Jest to najbezpieczniejszy sposób, aby wstawić wartości umieścić wartości w atrybucie danych tagu i pobrać go w sieci JavaScript. Na przykład:

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

Spowoduje to utworzenie poniższy kod HTML

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

Które, po uruchomieniu spowoduje, że następujące czynności:

```none
<"123">
   <"123">
   ```

Można również bezpośrednio wywołać kodera JavaScript

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

To spowoduje, że w przeglądarce

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> Nie należy łączyć niezaufanych danych wejściowych w języku JavaScript do tworzenia elementów modelu DOM. Należy używać `createElement()` i odpowiednio takich jak przypisywanie wartości właściwości `node.TextContent=`, lub użyj `element.SetAttribute()` / `element[attribute]=` w przeciwnym razie ujawnia siebie na podstawie DOM XSS.

## <a name="accessing-encoders-in-code"></a>Uzyskiwanie dostępu do koderów w kodzie

Kodery HTML, JavaScript i adres URL są dostępne do kodu na dwa sposoby, można wprowadzić je przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) lub skorzystać z koderów domyślne zawarte w `System.Text.Encodings.Web` przestrzeni nazw. Jeśli używasz koderów domyślne, a następnie żadnego stosowane do zakres znaków należy traktować jako bezpieczne nie odniesie żadnego skutku — koderów domyślne Użyj najbezpieczniejszy reguł kodowania możliwe.

Do użycia można konfigurować koderów za pośrednictwem Podpisane z konstruktorów powinno zająć *HtmlEncoder*, *JavaScriptEncoder* i *UrlEncoder* parametru zależnie od potrzeb. Na przykład;

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

## <a name="encoding-url-parameters"></a>Kodowanie parametry adresu URL

Jeśli chcesz utworzyć ciągu adresu URL zapytania z niezaufanych danych wejściowych jako Użyj wartości `UrlEncoder` do kodowania wartość. Na przykład

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Po kodowaniu encodedValue zmienna będzie zawierać `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Spacje, cudzysłowy, znaki interpunkcyjne i innych znaków niebezpieczny będzie procent kodowane w celu ich wartość szesnastkowa, na przykład znak spacji staną się % 20.

>[!WARNING]
> Nie używaj danych wejściowych niezaufanych jako część ścieżki adresu URL. Zawsze przekazywać niezaufanych danych wejściowych jako wartość ciągu zapytania.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Dostosowywanie koderów

Domyślnie koderów przy użyciu listy bezpiecznych ograniczone do zakresu podstawowe Unicode alfabetu łacińskiego, a wszystkie znaki poza tym zakresem jako odpowiedniki kod znaku kodowania. To zachowanie dotyczy również pomocnika tagów Razor i HtmlHelper renderowania zgodnie z koderów zostanie użyty do danych wyjściowych z ciągów.

Jest to uzasadnienie do ochrony przed usterki nieznany lub przyszłe przeglądarki (poprzednie błędy przeglądarki ma zwracać się podczas analizowania oparte na przetwarzanie znaków innych niż angielska). Jeśli witryny sieci web umożliwia użycie znaków innych niż łacińskie, takich jak angielski, chiński, cyrylica lub innych to prawdopodobnie nie jest zachowanie, które mają.

Można dostosować bezpieczne listy kodera uwzględnienie Unicode zakresów odpowiednie do aplikacji podczas uruchamiania w `ConfigureServices()`.

Na przykład użycie domyślnej konfiguracji można użyć Razor HtmlHelper, takich jak;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Po wyświetleniu źródło strony sieci web pojawi się, że ma zostało wyrenderowane w następujący sposób tekstem chińskiej wersji językowej zakodowane;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Aby poszerzyć znaki traktowane jako bezpieczne przez koder jak w przypadku wstawiania następujący wiersz do `ConfigureServices()` metody w `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

W tym przykładzie rozszerzenie listy bezpiecznych, aby uwzględnić CjkUnifiedIdeographs zakres Unicode. Staje się teraz przetworzonych wyników

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Listy bezpiecznych zakresy są określone jako wykresów kodu Unicode nie języków. [Unicode standard](http://unicode.org/) zawiera listę [kodu wykresy](http://www.unicode.org/charts/index.html) umożliwia znalezienie wykresu zawierające znaki. Każdy kodera, Html, JavaScript i adres Url, muszą zostać skonfigurowane osobno.

> [!NOTE]
> Dostosowanie listy bezpiecznych dotyczy tylko koderów powierzając jej ich konserwację za pośrednictwem Podpisane. Jeśli bezpośredni dostęp do kodera za pośrednictwem `System.Text.Encodings.Web.*Encoder.Default` następnie domyślna, podstawowa alfabetu łacińskiego tylko bezpiecznej liście będzie używany.

## <a name="where-should-encoding-take-place"></a>Gdzie należy umieścić podjęcia kodowania

Ogólne zaakceptowane rozwiązaniem jest, że kodowanie odbywa się w punkcie danych wyjściowych i wartości zakodowanej nigdy nie powinny być przechowywane w bazie danych. Kodowanie w punkcie wyjścia umożliwia zmianę wykorzystanie danych, na przykład z kodu HTML na ciąg zapytania. Również umożliwia łatwe wyszukiwanie danych bez konieczności kodowania wartości przed wyszukiwaniem i pozwala korzystać z wszelkie zmiany i poprawki dotyczące koderów.

## <a name="validation-as-an-xss-prevention-technique"></a>Sprawdzanie poprawności jako technikę zapobiegania XSS

Sprawdzanie poprawności mogą być przydatne narzędzie ograniczanie atakom XSS. Na przykład liczbowych ciąg zawierający tylko znaki 0-9, nie spowoduje wyzwolenia atak XSS. Sprawdzanie poprawności staje się bardziej skomplikowany, powinien chcesz zaakceptować HTML w danych wejściowych użytkownika - analiza input języka HTML jest trudne lub niemożliwe. Składni języka markDown i innych formatach tekstowych będą bezpieczniejsze dla zaawansowanych danych wejściowych. Nigdy nie należy polegać na samych sprawdzania poprawności. Zawsze kodowania niezaufanych dane wejściowe przed danych wyjściowych, niezależnie od tego, jakie walidacji mogły być wykonane.
