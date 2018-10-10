---
title: Zapobiegaj Cross-Site skrypty (XSS) w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat skryptów między witrynami (XSS) i technik do adresowania tę lukę w zabezpieczeniach w aplikacji ASP.NET Core.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 50f0211a2c64708d9b788dd10ce9064e66014d55
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910528"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>Zapobiegaj Cross-Site skrypty (XSS) w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Wykonywanie skryptów między witrynami (XSS) jest luki w zabezpieczeniach, która umożliwia osobie atakującej umieszcza skrypty po stronie klienta (zazwyczaj JavaScript) w stronach sieci web. Gdy inni użytkownicy załadować dotyczy stron, które zostaną uruchomione skrypty osoby atakującej, włączanie atakującemu kradzieży plików cookie i tokenów sesji zmiany zawartości strony sieci web za pomocą modelu DOM, manipulowanie lub przekierować przeglądarkę do innej strony. XSS luk w zabezpieczeniach są zazwyczaj wykonywane, gdy aplikacja akceptuje dane wejściowe użytkownika i wysyła go do strony bez weryfikowanie, kodowanie i jej anulowania zapewnianego element.

## <a name="protecting-your-application-against-xss"></a>Ochrona aplikacji przed XSS

Co podstawowa XSS poziomu polega na oszukanie aplikacji do wstawiania `<script>` tag na renderowanej stronie lub przez wstawienie `On*` zdarzeń do elementu. Deweloperzy należy używać następujące czynności prewencyjnych, aby uniknąć wprowadzenia XSS do swojej aplikacji.

1. Nigdy nie umieść niezaufane dane swojej input języka HTML, chyba że wykonaj pozostałe czynności. Niezaufane dane są wszelkie dane, które mogą być kontrolowane przez osobę atakującą, danych wejściowych z formularza HTML, ciągi zapytań, nagłówki HTTP, a nawet danych, skąd pochodzi z bazy danych, jak osoba atakująca może mieć możliwość naruszenia zabezpieczeń bazy danych, nawet wtedy, gdy nie można ich naruszenia zabezpieczeń aplikacji.

2. Przed wprowadzeniem niezaufanych danych wewnątrz elementu HTML upewnij się, że jest zakodowany w formacie HTML. Kodowanie HTML przyjmuje znaków takich jak &lt; i zmiany ich do bezpiecznego formularza, takich jak &amp;lt;

3. Przed wprowadzeniem niezaufane dane do atrybutu HTML upewnij się, że jest zakodowany w formacie HTML. Kodowanie atrybutu HTML jest podzbiorem kodowanie HTML i koduje znaki dodatkowe, takie jak "a".

4. Przed wprowadzeniem niezaufane dane w JavaScript należy umieścić dane w elemencie HTML, którego zawartość, możesz pobrać w czasie wykonywania. Jeśli nie jest to możliwe, upewnij się, że dane są zakodowane języka JavaScript. Kodowanie JavaScript przyjmuje niebezpiecznych znaków dla języka JavaScript i zastępuje je ich hex, na przykład &lt; mogłoby być kodowany jako `\u003C`.

5. Przed wprowadzeniem niezaufanych danych w ciągu zapytania adresu URL upewnij się, że jest zakodowany w adresie URL.

## <a name="html-encoding-using-razor"></a>Kodowanie HTML przy użyciu Razor

Używane w MVC automatycznie aparatu Razor koduje wszystkie dane wyjściowe źródło zmiennych, chyba że użytkownik pracuje się naprawdę trudne do zapobiegania jego zresetowanie. Używa reguł kodowania atrybutów HTML w każdym przypadku, gdy używasz *@* dyrektywy. Jako HTML kodowanie atrybutu jest nadzbiorem kodowanie HTML, oznacza to, że nie masz obawy samodzielnie za pomocą tego, czy należy używać kodowania HTML lub kodowanie atrybutu HTML. Należy się upewnić, że używasz tylko w kontekście HTML nie, podczas próby wstawienia niezaufane dane wejściowe bezpośrednio na język JavaScript. Pomocnicy tagów będzie również kodowania danych wejściowych używanych w parametrów tag.

Wykonaj następujący widok Razor:

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Ten widok wyświetla zawartość *untrustedInput* zmiennej. Ta zmienna zawiera niektóre znaki, które są używane w atakom XSS, a mianowicie &lt;, "i &gt;. Badanie źródła pokazuje wyniku renderowania zakodowanymi w formacie:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC udostępnia `HtmlString` klasę, która automatycznie nie jest kodowany na dane wyjściowe. To nie mogą być używane w połączeniu z niezaufane dane wejściowe zgodnie z tym umożliwia wyświetlenie luk w zabezpieczeniach XSS.

## <a name="javascript-encoding-using-razor"></a>JavaScript, kodowanie, za pomocą Razor

Mogą wystąpić sytuacje, które chcesz wstawić wartość na język JavaScript do przetworzenia w danym widoku. Istnieją dwa sposoby, aby to zrobić. Najbezpieczniejszy sposób wstawiania wartości jest umieszczenie wartości w atrybucie dane tagu i pobierać Twój kod JavaScript. Na przykład:

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

Pozwoli to osiągnąć, poniższy kod HTML

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

Który, po jego uruchomieniu, spowoduje, że następujące czynności:

```none
<"123">
   <"123">
   ```

Można również bezpośrednio wywołać kodera JavaScript:

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

To będą renderowane w przeglądarce w następujący sposób:

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> Nie należy łączyć niezaufane dane wejściowe w języku JavaScript do tworzenia elementów DOM w LICZBIE. Należy używać `createElement()` i odpowiednio takich jak przypisywanie wartości właściwości `node.TextContent=`, lub użyj `element.SetAttribute()` / `element[attribute]=` w przeciwnym razie można się narazić na opartej na modelu DOM XSS.

## <a name="accessing-encoders-in-code"></a>Uzyskiwanie dostępu do kodery w kodzie

Kodery HTML, JavaScript i adres URL są dostępne w kodzie na dwa sposoby, można wstawić je przy użyciu [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) lub można użyć koderów domyślne zawarte w `System.Text.Encodings.Web` przestrzeni nazw. Użycia kodery domyślne, a następnie dowolne został zastosowany do zakresy znaków należy traktować jako bezpieczne zaczyna obowiązywać — koderów domyślne używane najbezpieczniejszy możliwe reguły kodowania.

Aby użyć koderów można konfigurować za pośrednictwem DI z konstruktorów powinno zająć *HtmlEncoder*, *JavaScriptEncoder* i *UrlEncoder* parametru, zgodnie z potrzebami. Na przykład;

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

## <a name="encoding-url-parameters"></a>Kodowanie parametrów adresu URL

Jeśli chcesz utworzyć ciągu zapytania adresu URL, za pomocą niezaufane dane wejściowe jako wartość wykorzystanie `UrlEncoder` zakodować wartości. Na przykład

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Po zakodowaniu encodedValue będzie zawierać zmienną `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Miejsca do magazynowania, oferty, znaki interpunkcyjne i inne znaki niebezpieczne będzie procent zakodowany w celu ich wartość szesnastkową, na przykład znak spacji staną się % 20.

>[!WARNING]
> Nie używaj niezaufane dane wejściowe jako część ścieżki adresu URL. Zawsze należy przekazać niezaufane dane wejściowe jako wartość ciągu zapytania.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Dostosowywanie koderów

Domyślnie koderów, użyj listy bezpiecznych ograniczone do zakresu podstawowe alfabetu łacińskiego Unicode i kodowanie wszystkie znaki spoza tego zakresu jako ich odpowiedników kodów znaku. To zachowanie wpływa również na renderowanie pomocnika tagów Razor i HtmlHelper jak w przypadku koderów zostanie użyty do wyprowadzenia Twoimi ciągami.

Uzasadnienie to jest ochrona przed błędami nieznany lub przyszłe przeglądarki (poprzednie błędy przeglądarki ma wysyłać, zwracać się podczas analizowania oparte na przetwarzanie znaków innych niż angielski). Jeśli witryna sieci web sprawia, że intensywne użycie znaków innych niż łacińskie, takich jak chińskich, cyrylica lub inne osoby prawdopodobnie nie jest zachowanie, które chcesz.

Można dostosować list bezpiecznych kodera obejmujący Unicode zakresów odpowiednimi dla aplikacji, podczas uruchamiania w `ConfigureServices()`.

Na przykład przy użyciu domyślnej konfiguracji można użyć Razor HtmlHelper w następujący sposób;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Po wyświetleniu źródło strony sieci web pojawi się go zrenderowaniu w następujący sposób, przy użyciu chińskiego tekstu zakodowane;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Aby poszerzyć znaki traktowane jako bezpieczne przez koder jak w przypadku wstawiania następujący wiersz do `ConfigureServices()` method in Class metoda `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

W tym przykładzie rozszerzenie listy bezpiecznych, aby uwzględnić CjkUnifiedIdeographs zakresu Unicode. Staje się teraz wyniku renderowania

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Listy bezpiecznych zakresy są określane jako wykresy kodu Unicode i nie języków. [Standardu Unicode](http://unicode.org/) zawiera listę [kodu wykresy](http://www.unicode.org/charts/index.html) umożliwia znalezienie wykresów zawierających znaki. Każdy encoder, Html, JavaScript i adres Url, musi być skonfigurowany oddzielnie.

> [!NOTE]
> Dostosowywanie listy bezpiecznych dotyczy tylko koderów źródło za pośrednictwem DI. Jeśli bezpośrednio uzyskać dostęp za pośrednictwem koder `System.Text.Encodings.Web.*Encoder.Default` następnie domyślnie Łaciński podstawowy tylko bezpiecznej liście będzie używany.

## <a name="where-should-encoding-take-place"></a>Gdzie należy umieścić take kodowania

Ogólne zaakceptowane praktyką jest, czy kodowanie odbywa się w punkcie danych wyjściowych i wartości zakodowany nigdy nie powinny być przechowywane w bazie danych. Kodowanie w punkcie wyjścia umożliwia zmianę użycie danych, na przykład z kodu HTML do wartości ciągu zapytania. Ponadto pozwala na łatwe wyszukiwanie danych bez konieczności kodowania wartości przed wyszukiwaniem i pozwala na korzystanie z zalet zmiany lub poprawki wprowadzone koderów.

## <a name="validation-as-an-xss-prevention-technique"></a>Sprawdzanie poprawności jako technikę zapobiegania XSS

Sprawdzanie poprawności może być przydatne narzędzie ograniczanie atakom XSS. Na przykład ciągów liczbowych, zawierający tylko znaki 0 – 9 nie spowoduje wyzwolenia ataku XSS. Sprawdzanie poprawności staje się bardziej skomplikowane, akceptując HTML w danych wejściowych użytkownika. Analizowanie input języka HTML jest trudne lub niemożliwe. Markdown, w połączeniu z analizatora, wyodrębniania osadzonego kodu HTML, jest bezpieczniejsze do akceptowania bogate dane wejściowe. Nigdy nie zależą od samego sprawdzania poprawności. Zawsze należy zakodować niezaufane dane wejściowe, zanim dane wyjściowe, niezależnie od tego, jakie sprawdzania poprawności lub ich oczyszczania została wykonana.
