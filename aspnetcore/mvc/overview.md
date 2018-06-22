---
title: Omówienie platformy ASP.NET Core MVC
author: ardalis
description: Dowiedz się, jak platformy ASP.NET Core MVC jest sformatowany framework do tworzenia aplikacji sieci web i interfejsów API przy użyciu Model-View-Controller projektowanie wzorca.
ms.author: riande
ms.date: 01/08/2018
uid: mvc/overview
ms.openlocfilehash: aca34f91e8c7efaa34263ddf830b1662a2518969
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272595"
---
# <a name="overview-of-aspnet-core-mvc"></a>Omówienie platformy ASP.NET Core MVC

Przez [Steve Smith](https://ardalis.com/)

Platformy ASP.NET Core MVC jest sformatowany framework do tworzenia aplikacji sieci web i interfejsów API przy użyciu Model-View-Controller projektowanie wzorca.

## <a name="what-is-the-mvc-pattern"></a>Co to jest wzorzec MVC?

Wzorzec architektury Model-widok-kontroler (MVC) dzieli aplikację na trzy główne grupy składników: modele, widoki i kontrolery. Ten wzorzec umożliwia uzyskanie [separacji](http://deviq.com/separation-of-concerns/). Za pomocą tego wzorca, żądań użytkowników są kierowane do kontrolera, który jest odpowiedzialny za pracy z modelem akcje użytkownika i/lub pobrać wyniki zapytania. Kontroler wybierze widok, aby wyświetlić dla użytkownika i zapewnia Model danych, które wymaga.

Na poniższym diagramie przedstawiono trzy główne składniki i te, które odwołują się inne:

![Wzorzec MVC](overview/_static/mvc.png)

Ta nakreślenia obowiązki pomaga skalowanie aplikacji pod względem stopnia złożoności, ponieważ ułatwia kodu, debugowania i testowania coś (model, widok lub kontrolera) z jednym zadaniu (i jest zgodna z [jednej zasady odpowiedzialności ](http://deviq.com/single-responsibility-principle/)). Jest trudne do aktualizacji, badanie i debugowania kodu, który ma zależności rozłożyć na co najmniej dwa z tych trzech obszarach. Na przykład logika interfejsu użytkownika zwykle zmieniać częściej niż logiki biznesowej. Jeśli prezentacji kodu i logiki biznesowej są łączone w pojedynczy obiekt, należy zmodyfikować obiekt zawierający logiki biznesowej każdej zmianie interfejsu użytkownika. To często wprowadza błędy i wymaga ponowne logiki biznesowej po każdej zmianie interfejsu użytkownika minimalnej.

> [!NOTE]
> Zarówno widok i kontroler zależą od modelu. Jednak model zależy od widoku ani kontrolera. To jest jednym z kluczowych zalet separacji. Ta separacja umożliwia niezależne od wizualną prezentację modelu, który ma zostać utworzony i przetestowane.

### <a name="model-responsibilities"></a>Obowiązki modelu

Model w aplikacji MVC reprezentuje stan aplikacji i wszelka logika biznesowa lub operacje, które powinny zostać wykonane przez nią. Logika biznesowa powinna hermetyzowany w modelu, wraz z wszelka logika implementacji dla utrwalanie stanu aplikacji. Widoki jednoznacznie zazwyczaj używają typów ViewModel przeznaczone do wyświetlania danych do wyświetlenia w tym widoku. Kontroler tworzy i wypełnia te instancje ViewModel z modelu.

> [!NOTE]
> Istnieje wiele sposobów w celu zorganizowania modelu w aplikacji, która używa wzorzec architektury MVC. Dowiedz się więcej o niektórych [różnych typów modeli](http://deviq.com/kinds-of-models/).

### <a name="view-responsibilities"></a>Obowiązki widoku

Widoki są zobowiązani do prezentowania zawartości za pomocą interfejsu użytkownika. Używają [aparatu widoku Razor](#razor-view-engine) osadzanie kodu platformy .NET w kod znaczników HTML. Powinien być minimalny logikę w obrębie widoków i wszelka logika w nich odnoszą się do przedstawiania zawartości. Jeśli trzeba wykonać dużą logikę w widoku pliki, aby wyświetlić dane z modelu złożonego, należy rozważyć użycie [składnika widoku](views/view-components.md), ViewModel, lub szablon widoku, aby uprościć widoku.

### <a name="controller-responsibilities"></a>Obowiązki kontrolera

Kontrolery są składnikami, które obsługują interakcję z użytkownikiem, pracy z modelem i ostatecznie wybierają widok do renderowania. W aplikacji MVC widoku wyświetlane są tylko informacje; Kontroler obsługuje i ma odpowiadać na dane wejściowe użytkownika i interakcja. We wzorcu MVC kontroler jest punkt wejścia początkowej i jest odpowiedzialny za wybierania który model typy do pracy z i do renderowania widoku (stąd jego nazwa - kontroluje sposób odpowiadania przez aplikację do danego żądania).

> [!NOTE]
> Kontrolery nie powinien zbyt skomplikowany zbyt wiele obowiązki. Aby zachować logiką kontrolera staje się zbyt skomplikowane, użyj [jednej zasady odpowiedzialności](http://deviq.com/single-responsibility-principle/) do wypychania logiki biznesowej z kontrolerem z do modelu domeny.

>[!TIP]
> Jeśli okaże się, że Twoje akcji kontrolera często wykonują te same czynności, można wykonać [nie powtarzaj samodzielnie zasady](http://deviq.com/don-t-repeat-yourself/) przez przeniesienie tych typowych akcji do [filtry](#filters).

## <a name="what-is-aspnet-core-mvc"></a>Co to jest program ASP.NET Core MVC

Platforma ASP.NET Core MVC jest lekki typu open source, wysoce testowalna presentation framework zoptymalizowana dla platformy ASP.NET Core.

Podstawowe ASP.NET MVC umożliwia na podstawie wzorców do tworzenia dynamicznych witryn sieci Web umożliwia czyste rozdzielenie problemy. Umożliwia pełną kontrolę nad znaczników, obsługuje programowanie TDD przyjaznych i korzysta z najnowszych standardów sieci web.

## <a name="features"></a>Funkcje

Platformy ASP.NET Core MVC obejmuje następujące funkcje:

* [Routing](#routing)
* [Wiązanie modelu](#model-binding)
* [Walidacja modelu](#model-validation)
* [Iniekcji zależności](../fundamentals/dependency-injection.md)
* [Filtry](#filters)
* [Obszary](#areas)
* [Interfejsy API sieci Web](#web-apis)
* [Pola](#testability)
* [Aparat widoku razor](#razor-view-engine)
* [Jednoznacznie widoków](#strongly-typed-views)
* [Pomocnicy tagów](#tag-helpers)
* [Składniki w widoku](#view-components)

### <a name="routing"></a>Routing

ASP.NET Core MVC jest oparty na [routingu platformy ASP.NET Core](../fundamentals/routing.md), wydajny składnik Mapowanie adresu URL, który pozwala tworzyć aplikacje, które mają zrozumiałe i można wyszukiwać adresów URL. Dzięki temu można zdefiniować aplikacji wzorców nazewnictwa adresów URL współpracujących dla optymalizacji dla aparatów wyszukiwania (SEO) oraz do generowania łącza, bez względu na sposób organizowania są pliki na serwerze sieci web. Można zdefiniować przy użyciu składni szablonu trasy wygodny obsługującego ograniczenia wartości trasy, wartości domyślne i opcjonalne wartości trasy.

*Na podstawie Konwencji routingu* umożliwia zdefiniowanie globalny adres URL formaty, w których aplikacja akceptuje i jak każdego z tych formatów mapy do metody akcji określonych w podanego kontrolera. Po odebraniu żądania przychodzącego aparat routingu analizuje adres URL i dopasowuje je do jednej z określonych formatów adresów URL, a następnie wywołuje metodę akcji skojarzonego kontrolera.

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*Atrybut routingu* można określić informacji o routingu przez dekoracji z kontrolerów i akcji z atrybutami, które definiowania tras dla aplikacji. Oznacza to, że definicji trasy są umieszczone obok kontrolera i akcji, z którym są powiązane.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a>Wiązanie modelu

Platformy ASP.NET Core MVC [modelu powiązania](models/model-binding.md) konwertuje danych o żądaniach klientów (wartości formularza, danych trasy, parametrów ciągu zapytania, nagłówki HTTP) na obiekty, które może obsłużyć kontrolera. W związku z tym logiki kontrolera nie ma w pracy z ustaleniem, dane żądanie przychodzące; po prostu ma danych jako parametry metody akcji.

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>Weryfikacja modelu

Obsługuje platformy ASP.NET Core MVC [weryfikacji](models/validation.md) przez dekoracji obiektu modelu z atrybutów sprawdzania poprawności adnotacji danych. Atrybuty weryfikacji są sprawdzane po stronie klienta, zanim wartości są przesłane do serwera, jak również na serwerze przed akcji kontrolera jest wywoływana.

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

Akcja kontrolera:

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

Platformę obsługi sprawdzania poprawności danych żądania zarówno na kliencie, jak i na serwerze. Logikę weryfikacji określoną w typach modelu jest dodawany do widoków renderowanych jako adnotacje dyskretnego kodu i jest wymuszana w przeglądarce z [jQuery weryfikacji](https://jqueryvalidation.org/).

### <a name="dependency-injection"></a>Iniekcji zależności

Platformy ASP.NET Core ma wbudowaną obsługę [iniekcji zależności (Podpisane)](../fundamentals/dependency-injection.md). W nazwie wzorca MVC ASP.NET Core [kontrolerów](controllers/dependency-injection.md) można żądania potrzebne usług za pośrednictwem ich konstruktorów, umożliwiając wykonaj [jawne zależności zasady](http://deviq.com/explicit-dependencies-principle/).

Aplikację można również użyć [iniekcji zależności w widoku pliki](views/dependency-injection.md)za pomocą `@inject` dyrektywy:

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a>Filtry

[Filtry](controllers/filters.md) pomóc deweloperom Hermetyzowanie kompleksowymi problemy, takie jak obsługa wyjątków lub autoryzacji. Filtry włączyć uruchamianie niestandardowej logiki przed i przetwarzania końcowego dla metody akcji i może być skonfigurowana do uruchamiania w niektórych punktach w potoku wykonywania dla danego żądania. Filtry można zastosować do kontrolerów i akcji jako atrybuty (lub mogą być uruchamiane globalny). Kilka filtrów (takie jak `Authorize`) znajdują się w ramach.


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a>Obszary

[Obszary](controllers/areas.md) umożliwiają partycji dużych aplikacji sieci Web platformy ASP.NET Core MVC w mniejszych funkcjonalności grupowania. Obszar to struktura MVC wewnątrz aplikacji. W projekcie MVC składników logicznych, takich jak Model, kontrolera i widoku są przechowywane w różnych folderach i MVC używa konwencji nazewnictwa do utworzenia relacji między tymi składnikami. W przypadku dużych aplikacji może być korzystne partycji aplikacji na oddzielnych wysokiej obszary poziomu funkcjonalności. Na przykład aplikacji handlu elektronicznego z wiele jednostek biznesowych, takich jak wyewidencjonowania, rozliczeń i wyszukiwania itd. Każdy z tych jednostek ma własne logiczny składnik widoki, kontrolery i modeli.

### <a name="web-apis"></a>Interfejsy Web API

Oprócz stanowi doskonałe platformę do tworzenia witryn sieci web, ASP.NET Core MVC ma dużą obsługę tworzenia interfejsów API sieci Web. Można utworzyć usługi, które są używane przez szeroki wachlarz klientów, w tym przeglądarki i urządzenia przenośne.

Struktura obejmuje obsługę negocjowanie zawartości HTTP z wbudowaną obsługą do [formatowanie danych](xref:web-api/advanced/formatting) jako JSON i XML. Zapis [niestandardowe elementy formatujące](xref:web-api/advanced/custom-formatters) Aby dodać obsługę własnych formatów.

Użyj generowania łącze, aby włączyć obsługę hipermedialnych. Łatwo włączyć obsługę [współużytkowanie zasobów między źródłami (CORS) do udostępniania](http://www.w3.org/TR/cors/) tak, aby interfejsów API sieci Web może być współużytkowana przez wiele aplikacji sieci Web.

### <a name="testability"></a>Pola

W ramach korzystanie z interfejsów i iniekcji zależności był dobrze nadaje się do przeprowadzania testów jednostkowych i ramach zawiera funkcje (na przykład TestHost i InMemory dostawcy programu Entity Framework) [testy integracji](xref:test/integration-tests) szybki i łatwe również. Dowiedz się więcej o [jak logikę kontrolera testu](controllers/testing.md).

### <a name="razor-view-engine"></a>Aparat widoku razor

[ASP.NET Core MVC widoków](views/overview.md) użyj [aparatu widoku Razor](views/razor.md) do renderowania widoków. Razor to język compact, obszerne i płynne szablonu do definiowania widoków przy użyciu osadzonego kodu C#. Razor jest używana do dynamicznego generowania zawartości sieci web na serwerze. Kod serwera prawidłowo można łączyć z zawartością po stronie klienta i kod.

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

Przy użyciu aparatu widoku Razor można zdefiniować [układów](views/layout.md), [widoki częściowe](views/partial.md) i wymiennych sekcji.

### <a name="strongly-typed-views"></a>Jednoznacznie widoków

Widoków MVC razor można zdecydowanie wpisywać oparte na modelu. Kontrolery można przekazać jednoznacznie modelu z widokami włączanie widoków Sprawdzanie typu i obsługę funkcji IntelliSense.

Na przykład następujący widok renderuje modelu typu `IEnumerable<Product>`:

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>Pomocników tagów

[Pomocników tagów](views/tag-helpers/intro.md) Włącz kod po stronie serwera do tworzenia i renderowania elementów HTML w plikach Razor. Pomocników tagów umożliwia definiowanie niestandardowych tagów (na przykład `<environment>`) lub aby zmodyfikować zachowanie znaczników (na przykład `<label>`). Pomocników tagów powiązać z określonych elementów na podstawie nazwy elementu i jego atrybuty. Zapewniają korzyści wynikające z renderowania po stronie serwera, zachowując nadal edytowania kodu HTML.

Istnieje wiele wbudowanych pomocników tagów do wykonywania typowych zadań — takich jak tworzenie formularzy, łącza, ładowanie zasobów i pakietów więcej — i jeszcze bardziej dostępne w publicznych repozytoriach usługi GitHub i NuGet. Pomocników tagów są tworzone w języku C# i ich elementami docelowymi na podstawie nazwy elementu, atrybutu nazwy lub tagu nadrzędnym elementów HTML. Na przykład wbudowana LinkTagHelper można utworzyć łącza do `Login` akcji `AccountsController`:

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

`EnvironmentTagHelper` Można uwzględnić skrypty różnych w widoków (na przykład raw lub zminimalizowany) oparte na środowisko uruchomieniowe, takich jak projektowanie, tymczasowym czy produkcyjnym:

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

Pomocników tagów zapewnić środowisko programistyczne przyjaznego HTML i bogate środowisko funkcji IntelliSense do tworzenia znaczników HTML i Razor. Większość wbudowanych pomocników tagów target istniejących elementów HTML i udostępniania atrybutów po stronie serwera dla elementu.

### <a name="view-components"></a>Składniki w widoku

[Wyświetlanie składników](views/view-components.md) umożliwiają pakietu logiki renderowania i użyć go ponownie w całej aplikacji. Są one podobne do [widoki częściowe](views/partial.md), ale z skojarzonej logiki.
