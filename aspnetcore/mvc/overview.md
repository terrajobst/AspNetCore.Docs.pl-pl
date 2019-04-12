---
title: Omówienie platformy ASP.NET Core MVC
author: ardalis
description: Dowiedz się, jak ASP.NET Core MVC zaawansowaną strukturę do tworzenia aplikacji sieci web i interfejsów API za pomocą Model-View-Controller wzorca projektowego.
ms.author: riande
ms.date: 01/08/2018
uid: mvc/overview
ms.openlocfilehash: eb636616ab952f506993ae5f4692e5e966bb20f4
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516303"
---
# <a name="overview-of-aspnet-core-mvc"></a>Omówienie platformy ASP.NET Core MVC

Przez [Steve Smith](https://ardalis.com/)

Platforma ASP.NET Core MVC jest zaawansowaną strukturę do tworzenia aplikacji sieci web i interfejsów API za pomocą Model-View-Controller wzorca projektowego.

## <a name="what-is-the-mvc-pattern"></a>Co to jest wzorzec MVC?

Wzorzec architektury Model-View-Controller (MVC) dzieli aplikację na trzy główne grupy składników: Modeli, widoków i kontrolerów. Ten wzorzec pomaga osiągnąć [separacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns). Korzystając z tego wzorca, żądań użytkowników są kierowane do kontrolera, który jest odpowiedzialny za współpracę z modelu, który ma wykonywać akcje użytkownika i/lub pobrać wyniki zapytania. Kontroler wybiera widok, aby wyświetlić użytkownikowi i dostarcza mu żadnych danych modelu, który wymaga.

Na poniższym diagramie przedstawiono trzy główne składniki i te, które odwołują się inne:

![Wzorzec MVC](overview/_static/mvc.png)

Ta nakreślenia obowiązki ułatwia skalowanie aplikacji w sensie złożoność, ponieważ jest łatwiejsza do kodu, debugowania i testowania (model, widok lub kontroler) coś, co ma pojedynczego zadania. Jest trudniejsze do aktualizacji, testowania i debugowania kodu, obejmującego zależności rozkładają się na dwóch lub więcej z tych trzech obszarów. Na przykład logika interfejsu użytkownika zwykle zmieniać częściej niż logiki biznesowej. Jeśli prezentacji kodu i logiki biznesowej są łączone w pojedynczy obiekt, można zmodyfikować obiekt zawierający logikę biznesową, za każdym razem, gdy interfejs użytkownika zostanie zmieniony. To często wprowadza błędy i wymaga przetestowanie logiki biznesowej po każdej zmianie interfejsu użytkownika minimalnej.

> [!NOTE]
> Widok i kontroler zależeć od modelu. Jednak model jest zależna od widoku ani kontrolera. Jest to jedna z najważniejszych korzyści zapewnianych przez oddzielenie. Ta separacja pozwala modelu, który ma być tworzone i testowane na niezależne od wizualnej prezentacji.

### <a name="model-responsibilities"></a>Obowiązki modelu

Modelu w aplikacji MVC reprezentuje stan aplikacji, wszelka logika biznesowa i operacje, które powinny być wykonywane przez nią. Logika biznesowa powinna hermetyzowane w modelu wraz z wszelka logika implementacji na przechowywanie stanu aplikacji. Silnie typizowane widoki zwykle użyć typów ViewModel przeznaczone do wyświetlania danych do wyświetlenia w tym widoku. Kontroler tworzy i wypełnia te wystąpienia ViewModel z modelu.

### <a name="view-responsibilities"></a>Wyświetl zakres odpowiedzialności

Widoki są odpowiedzialny za prezentowanie zawartości za pomocą interfejsu użytkownika. Używają [aparatu widoku Razor](#razor-view-engine) osadzanie kodu .NET w kod znaczników HTML. Powinien być minimalny logiki w obrębie widoków i wszelka logika w nich odnoszą się do prezentowania zawartości. Jeśli okaże się, trzeba wykonać dużym stopniem logiki w widoku plików w celu wyświetlania danych z modelu złożonego, należy wziąć pod uwagę przy użyciu [widoku składnika](views/view-components.md), ViewModel, lub Wyświetl szablon, aby uprościć widok.

### <a name="controller-responsibilities"></a>Obowiązki kontrolera

Kontrolery są składnikami, które obsługują interakcję z użytkownikiem, pracy z modelem i ostatecznie wybierają widok do renderowania. W aplikacji MVC widok zawiera tylko informacje; Kontroler obsługuje i reaguje na dane wejściowe użytkownika i interakcji. We wzorcu MVC kontroler jest punktem wejścia początkowej i jest odpowiedzialny za wybranie model, który typy do pracy z i widok do renderowania (dlatego jego nazwa - kontroluje sposób aplikacja reaguje na dane żądanie).

> [!NOTE]
> Kontrolery nie powinien zbyt skomplikowane, przez zbyt wiele obowiązki. Aby zapobiec logiką kontrolera staje się zbyt skomplikowana, Wypchnij logika biznesowa z kontrolerem z do modelu domeny.

>[!TIP]
> Jeśli okaże się, że akcje kontrolera często wykonują te same czynności, Przenieś następujące typowe akcje do [filtry](#filters).

## <a name="what-is-aspnet-core-mvc"></a>What is ASP.NET Core MVC

Platforma ASP.NET Core MVC jest uproszczone, typu open source, wysoce testowalna presentation framework zoptymalizowana do użytku z platformą ASP.NET Core.

Platforma ASP.NET Core MVC udostępnia bazujący na wzorcach sposób tworzenia dynamicznych witryn internetowych, który umożliwia czyste rozdzielenie problemów. Zapewnia pełną kontrolę nad znacznikami, obsługuje programowanie TDD przyjaznego i korzysta z najnowszych standardów sieci web.

## <a name="features"></a>Funkcje

Platforma ASP.NET Core MVC obejmuje następujące funkcje:

* [Routing](#routing)
* [Wiązanie modelu](#model-binding)
* [Walidacja modelu](#model-validation)
* [Wstrzykiwanie zależności](../fundamentals/dependency-injection.md)
* [Filtry](#filters)
* [Obszary](#areas)
* [Interfejsy API sieci Web](#web-apis)
* [Testowalności](#testability)
* [Aparat widoku razor](#razor-view-engine)
* [Silnie typizowane widoki](#strongly-typed-views)
* [Pomocnicy tagów](#tag-helpers)
* [Składniki widoków](#view-components)

### <a name="routing"></a>Routing

ASP.NET Core MVC jest wbudowana w górnej części [routingu platformy ASP.NET Core](../fundamentals/routing.md), zaawansowane składnik Mapowanie adresu URL, który pozwala tworzyć aplikacje, które mają adresy URL zrozumiałe i którą można przeszukiwać. Dzięki temu można zdefiniować aplikacji wzorców nazewnictwa adresów URL, które działa dobrze w przypadku optymalizacji dla aparatów wyszukiwania (SEO) oraz do generowania łącza, bez względu na sposób organizowania są pliki na serwerze sieci web. Można zdefiniować przy użyciu składni szablonu dogodnym tras, które obsługuje ograniczenia wartości trasy, wartości domyślne i opcjonalne wartości trasy.

*Routing oparty na Konwencji* formatuje można zdefiniować globalny adres URL, który umożliwia aplikacji akceptuje i jak każdy z tych formatów mapuje do metody akcji określonych w podanego kontrolera. Po odebraniu żądania przychodzącego aparat routingu analizuje adres URL i dopasowuje je do jednej z określonych formatów adresów URL, a następnie wywołuje metodę akcji kontrolera skojarzone.

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*Atrybut routingu* umożliwia określenie informacji o routingu przez urządzanie kontrolerów i akcji przy użyciu atrybutów, które definiują trasy Twojej aplikacji. Oznacza to, że definicji trasy są umieszczane obok kontrolera i akcji, z którymi są powiązane.

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

### <a name="model-binding"></a>Powiązanie modelu

Platforma ASP.NET Core MVC [wiązanie modelu](models/model-binding.md) konwertuje dane z żądania klienta (wartości formularza, danych trasy, parametry ciągu zapytania, nagłówki HTTP) na obiekty, które może obsłużyć kontrolera. Co w efekcie logikę kontrolera nie musi wykonywać pracę ustalenie dane przychodzące żądania; po prostu ma danych jako parametry do jego metod akcji.

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>Walidacja modelu

Obsługuje platformy ASP.NET Core MVC [weryfikacji](models/validation.md) przez urządzanie obiektu modelu przy użyciu atrybutów weryfikacji adnotacji danych. Atrybutów sprawdzania poprawności są sprawdzane po stronie klienta, zanim wartości są ogłaszane na serwerze, a także jest wywoływana na serwerze przed akcji kontrolera.

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

Struktura obsługuje sprawdzanie poprawności danych żądania zarówno na kliencie, jak i na serwerze. Logikę weryfikacji określony dla typów modelu jest dodawana do widoków renderowany jako adnotacje dyskretnego kodu i są wymuszane w przeglądarce z [jQuery weryfikacji](https://jqueryvalidation.org/).

### <a name="dependency-injection"></a>Wstrzykiwanie zależności

Platforma ASP.NET Core ma wbudowaną obsługę [wstrzykiwanie zależności (DI)](../fundamentals/dependency-injection.md). W programie ASP.NET Core MVC [kontrolerów](controllers/dependency-injection.md) można żądania potrzebnych usług za pośrednictwem ich konstruktory, umożliwiając im postępuj zgodnie z [jawne zależności zasady](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).

Aplikację można również użyć [wstrzykiwanie zależności w widoku plików](views/dependency-injection.md)przy użyciu `@inject` dyrektywy:

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

[Filtry](controllers/filters.md) ułatwiają deweloperom hermetyzacji odciąż przekrojowe zagadnienia, takie jak obsługa wyjątków i autoryzacji. Filtry Włącz logiki niestandardowej uruchomiona przed i przetwarzanie końcowe dla metody akcji i mogą być skonfigurowane do uruchamiania w określonych punktach w potoku wykonywania dla danego żądania. Filtry mogą być stosowane do kontrolerów lub akcje jako atrybuty (lub mogą być uruchamiane globalnie). Kilka filtrów (takie jak `Authorize`) znajdują się w ramach. `[Authorize]` to atrybut, który jest używany do tworzenia filtrów autoryzacji platformy MVC.

```csharp
[Authorize]
public class AccountController : Controller
```

### <a name="areas"></a>Obszary

[Obszary](controllers/areas.md) umożliwiają do dzielenia dużych aplikacji sieci Web platformy ASP.NET Core MVC na mniejsze grupy funkcjonalnej. Obszar to struktura MVC wewnątrz aplikacji. W projekcie MVC logiczne składniki, takie jak Model, kontroler i Widok są przechowywane w różnych folderach, i są używane konwencje nazewnictwa do utworzenia relacji między tymi składnikami. W przypadku dużych aplikacji może być korzystne podzielić ją na oddzielnych wysokiego poziomu obszary funkcji. Na przykład aplikacja handlu elektronicznego z wielu jednostek biznesowych, takich jak wyszukiwanie itp wyewidencjonowanie i rozliczeniami. Każda z tych jednostek ma swoje własne widoki logiczny składnik, kontrolery i modeli.

### <a name="web-apis"></a>Interfejsy Web API

Oprócz znakomitą platformę dla tworzenia witryn sieci web, ASP.NET Core MVC obsługuje doskonałe do tworzenia interfejsów API sieci Web. Można tworzyć usługi, którzy osiągną szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych.

Struktura obsługuje negocjowanie zawartości HTTP z wbudowaną obsługą do [formatowania danych](xref:web-api/advanced/formatting) jak JSON lub XML. Zapis [niestandardowe elementy formatujące](xref:web-api/advanced/custom-formatters) dodanie obsługi własnych formatów.

Użyj generowania łącza, aby włączyć obsługę hipermediach. Łatwo włączyć obsługę [współużytkowanie zasobów między źródłami (cors)](http://www.w3.org/TR/cors/) tak, aby interfejsy API sieci Web mogą być współużytkowane przez wiele aplikacji sieci Web.

### <a name="testability"></a>Testowalności

Użyj struktury interfejsów i wstrzykiwanie zależności przekształcić ją w odpowiednie do testowania jednostkowego i struktura zawiera funkcje (na przykład TestHost i InMemory dostawca programu Entity Framework) [testy integracji](xref:test/integration-tests) szybki i łatwe jak również. Dowiedz się więcej o [testowanie logiką kontrolera](controllers/testing.md).

### <a name="razor-view-engine"></a>Aparat widoku razor

[ASP.NET Core MVC widoków](views/overview.md) użyj [aparatu widoku Razor](views/razor.md) do renderowania widoków. Razor jest językiem znaczników compact, ekspresyjny i płynnych szablonu służącego do definiowania widoków przy użyciu osadzonego kodu C#. Razor jest używana do dynamicznego generowania zawartości sieci web na serwerze. Kod serwera można prawidłowo łączyć za pomocą kodu i zawartości po stronie klienta.

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

Za pomocą aparatu widoku Razor, można zdefiniować [układy](views/layout.md), [widoki częściowe](views/partial.md) i wymienne sekcji.

### <a name="strongly-typed-views"></a>Silnie typizowane widoki

Widokami razor, MVC może być silnie typizowane oparte na modelu. Kontrolery można przekazać silnie typizowany model z widokami włączanie widoków sprawdzania typu i obsługuje funkcję IntelliSense.

Na przykład następujący widok renderuje model typu `IEnumerable<Product>`:

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>Pomocnicy tagów

[Pomocników tagów](views/tag-helpers/intro.md) włączyć kodu po stronie serwera wziąć udział w tworzeniu i renderowaniu elementów HTML w plikach Razor. Pomocnicy tagów umożliwia definiowanie tagów niestandardowych (na przykład `<environment>`) lub do zmodyfikowania zachowania istniejących tagów (na przykład `<label>`). Pomocnicy tagów powiązać z określonych elementów na podstawie nazwy elementu i jego atrybuty. Zapewniają korzyści wynikające z renderowania po stronie serwera, zachowując przy tym nadal środowisko edytowania plików języka HTML.

Istnieje wiele wbudowanych pomocników tagów dla typowych zadań — takich jak tworzenie formularzy, łącza, ładowanie zasobów i pakiety więcej — i jeszcze bardziej dostępne w publicznych repozytoriach GitHub oraz jak NuGet. Pomocnicy tagów są tworzone w języku C#, a ich celem są elementy HTML na podstawie nazwy elementu, atrybutu nazwy lub tagu nadrzędnym. Na przykład wbudowane LinkTagHelper można utworzyć łącze do `Login` akcji `AccountsController`:

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

`EnvironmentTagHelper` Może służyć do uwzględnienia różnych skryptów w widoków (na przykład raw lub zminimalizowana) oparte na środowisko uruchomieniowe, takich jak deweloperskie, przejściowe lub produkcyjne:

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

Pomocnicy tagów zapewniają środowisko programistyczne przyjaznego dla kodu HTML i bogate środowisko funkcji IntelliSense do tworzenia znaczników HTML i Razor. Większość wbudowanych pomocników tagów docelowe istniejące elementy HTML i udostępniania atrybutów po stronie serwera dla elementu.

### <a name="view-components"></a>Składniki widoków

[Wyświetlanie składników](views/view-components.md) umożliwiają pakietu logiki renderowania i użyć go ponownie w całej aplikacji. Są one podobne do [widoki częściowe](views/partial.md), ale przy użyciu skojarzonej logiki.

## <a name="compatibility-version"></a>Wersja zgodności

Metoda <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> umożliwia aplikacji włączenie wykorzystania lub rezygnację ze zmian zachowania wprowadzanych w programie ASP.NET Core MVC w wersji 2.1 lub nowszej, które potencjalnie mogą prowadzić do awarii.

Aby uzyskać więcej informacji, zobacz <xref:mvc/compatibility-version>.
