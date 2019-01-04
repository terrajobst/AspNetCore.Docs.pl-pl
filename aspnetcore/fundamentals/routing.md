---
title: Routing w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak platformy ASP.NET Core, routing jest odpowiedzialny za mapowania żądania identyfikatorów URI punktu końcowego selektory i wysyłania żądania przychodzące do punktów końcowych.
ms.author: riande
ms.custom: mvc
ms.date: 12/29/2018
uid: fundamentals/routing
ms.openlocfilehash: c57b309e4474f9aff5c0594a3d9d1c796990d31e
ms.sourcegitcommit: e1cc4c1ef6c9e07918a609d5ad7fadcb6abe3e12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53997360"
---
# <a name="routing-in-aspnet-core"></a>Routing w programie ASP.NET Core

Przez [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), i [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="<= aspnetcore-1.1"

Dla wersji 1.1 w tym temacie, Pobierz [routingu w programie ASP.NET Core (w wersji 1.1, plików PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Routing_1.x.pdf).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Routing jest odpowiedzialny za mapowanie żądania identyfikatorów URI punktu końcowego selektory i wysyłania żądań przychodzących do punktów końcowych. Trasy są zdefiniowane w aplikacji i skonfigurowane, po uruchomieniu aplikacji. Trasy Opcjonalnie można wyodrębnić wartości z adresu URL zawartych w żądaniu, a te wartości mogą następnie służyć do przetwarzania żądania. Korzystając z informacji o trasie z aplikacji, routing jest również możliwe do generowania adresów URL, które mapują do endpoint selektorów.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Aby użyć scenariusze najnowsza wersja routingu platformy ASP.NET Core 2.2, określ [zgodność wersji](xref:mvc/compatibility-version) aby MVC usług rejestracji w `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

`EnableEndpointRouting` Opcja określa, w przypadku routingu należy wewnętrznie używać oparte na punktach końcowych logiki lub <xref:Microsoft.AspNetCore.Routing.IRouter>— na podstawie logiki platformy ASP.NET Core 2.1 lub wcześniej. Gdy wersja zgodności jest ustawiona do wersji 2.2 lub nowszej, wartość domyślna to `true`. Ustaw wartość `false` do użycia z poprzedniego logikę routingu:

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

Aby uzyskać więcej informacji na temat <xref:Microsoft.AspNetCore.Routing.IRouter>-routingu, zobacz artykuł [platformy ASP.NET Core 2.1 wersję tego tematu](xref:fundamentals/routing?view=aspnetcore-2.1).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Routing jest odpowiedzialny za mapowania żądania identyfikatorów URI do obsługi trasy i wysyłania żądań przychodzących. Trasy są zdefiniowane w aplikacji i skonfigurowane, po uruchomieniu aplikacji. Trasy Opcjonalnie można wyodrębnić wartości z adresu URL zawartych w żądaniu, a te wartości mogą następnie służyć do przetwarzania żądania. Za pomocą skonfigurowanych tras z aplikacji, routing jest w stanie do generowania adresów URL, które mapują do obsługi trasy.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Aby użyć scenariusze najnowsza wersja routingu platformy ASP.NET Core 2.1, należy określić [zgodność wersji](xref:mvc/compatibility-version) aby MVC usług rejestracji w `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

::: moniker-end

> [!IMPORTANT]
> W tym dokumencie opisano niskiego poziomu routingu platformy ASP.NET Core. Aby uzyskać informacji na temat routingu platformy ASP.NET Core MVC, zobacz <xref:mvc/controllers/routing>. Aby uzyskać informacji na temat Konwencji tras w stron Razor, zobacz <xref:razor-pages/razor-pages-conventions>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Podstawy routingu

Większość aplikacji należy wybrać podstawowe i opisowe schemat routingu, aby adresy URL są czytelne i zrozumiałe. Domyślną trasę konwencjonalne `{controller=Home}/{action=Index}/{id?}`:

* Obsługuje routingu schemat podstawowy i opisowe.
* To dobry punkt wyjścia dla oparty na interfejsie użytkownika aplikacji.

Deweloperzy zazwyczaj dodania dodatkowych tras zwięzła do obszarów o dużym natężeniu ruchu aplikacji w sytuacjach wyspecjalizowanego (na przykład, blogów i handlu elektronicznego punkty końcowe) przy użyciu [trasowanie atrybutów](xref:mvc/controllers/routing#attribute-routing) lub trasy są konwencjonalne funkcje w wersji dedykowanej.

Interfejsy API sieci Web należy używać trasowanie atrybutów do modelu funkcjonalność aplikacji jako zestaw zasobów, której operacje są reprezentowane przez zleceń HTTP. Oznacza to, że wiele operacji (na przykład GET, POST) na ten sam zasób logicznej będzie używać tego samego adresu URL. Trasowanie atrybutów zapewnia poziom kontroli, która jest potrzebne do starannie zaprojektować układ publiczny punkt końcowy interfejsu API.

Aplikacje stron razor Użyj domyślnej konwencjonalne routingu w celu obsługi zasobów o nazwie *stron* folderu aplikacji. Dostępne są dodatkowe konwencje umożliwiające dostosowywanie zachowania routingu stron Razor. Aby uzyskać więcej informacji, zobacz <xref:razor-pages/index> i <xref:razor-pages/razor-pages-conventions>.

Obsługa generowania adresu URL umożliwia aplikacji, które można rozwijać bez zakodowane na stałe adresy URL, aby połączyć ze sobą aplikacji. Ta funkcja umożliwia począwszy od podstawowej konfiguracji routingu i modyfikowania trasy, po jest określana przez aplikację zasobu układu.

::: moniker range=">= aspnetcore-2.2"

Używa routingu *punktów końcowych* (`Endpoint`) do reprezentowania logicznej punktów końcowych w aplikacji.

Punkt końcowy Definiuje delegata do przetwarzania żądań i kolekcję dowolnego metadanych. Metadane są używane Implementowanie odciąż przekrojowe zagadnienia, na podstawie zasad i konfiguracji dołączone do każdego punktu końcowego.

Routing system ma następujące cechy:

* Składni szablonu trasy jest używane do definiowania trasy z parametrami trasy z tokenami.
* Konfiguracja punktu końcowego konwencjonalne stylu i stylu dla atrybutu jest dozwolone.
* `IRouteConstraint` Służy do określenia, czy parametr adresu URL zawiera prawidłową wartość dla ograniczenia danego punktu końcowego.
* Modele aplikacji, takich jak strony MVC i Razor zarejestrować wszystkie ich punktów końcowych, które mają przewidywalny implementacji scenariuszy routingu.
* Routing implementacji sprawia, że decyzje w kwestii routingu wszędzie tam, gdzie pożądana w potoku oprogramowania pośredniczącego.
* Oprogramowanie pośredniczące, która pojawia się po oprogramowanie pośredniczące routingu można sprawdzić wynik decyzja punktu końcowego pośredniczącego routingu dla danego identyfikatora URI żądania.
* Jest możliwe wyliczenie wszystkich punktów końcowych w aplikacji w dowolnym miejscu w potoku oprogramowania pośredniczącego.
* Aplikację można używać routingu do generowania adresów URL (na przykład przekierowanie lub łączy) na podstawie informacji o punkcie końcowym i związku z tym należy unikać ustaloną adresów URL, co pomoże w utrzymaniu.
* Generowanie adresu URL jest oparty na adresy, które obsługują dowolną rozszerzalności:

  * Interfejs API Generator Link (`LinkGenerator`) może zostać rozpoznana w dowolnym miejscu przy użyciu [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) do generowania adresów URL.
  * W przypadku, gdy interfejs API Generator Link nie jest dostępna za pośrednictwem DI, `IUrlHelper` zapewnia metody do tworzenia adresów URL.

> [!NOTE]
> Wraz z wydaniem punktu końcowego routingu w programie ASP.NET Core 2.2 łączenie punktu końcowego jest ograniczona do stron MVC i Razor akcje i stron. Rozszerzenia typu łączenia punktu końcowego w przyszłych wersjach planowane jest wprowadzenie możliwości.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Używa routingu *trasy* (implementacje <xref:Microsoft.AspNetCore.Routing.IRouter>) do:

* Mapowanie żądań przychodzących do *trasy obsługi*.
* Generowanie adresy URL używane w odpowiedzi.

Domyślnie aplikacja ma jedną kolekcję tras. Po odebraniu żądania trasy w kolekcji są przetwarzane w kolejności, w jakiej znajdują się w kolekcji. Struktura podejmuje próbę dopasowania przychodzącego adresu URL żądania do ścieżki w kolekcji przez wywołanie metody <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> metody dla każdej trasy w kolekcji. Odpowiedź może używać routingu do generowania adresów URL (na przykład przekierowanie lub linki), w oparciu o trasach i związku z tym należy unikać ustaloną adresów URL, co pomoże w utrzymaniu.

Routing system ma następujące cechy:

* Składni szablonu trasy jest używane do definiowania trasy z parametrami trasy z tokenami.
* Konfiguracja punktu końcowego konwencjonalne stylu i stylu dla atrybutu jest dozwolone.
* `IRouteConstraint` Służy do określenia, czy parametr adresu URL zawiera prawidłową wartość dla ograniczenia danego punktu końcowego.
* Modele aplikacji, takich jak strony MVC i Razor zarejestrować wszystkie ich tras, które mają przewidywalny implementacji scenariuszy routingu.
* Odpowiedź może używać routingu do generowania adresów URL (na przykład przekierowanie lub linki), w oparciu o trasach i związku z tym należy unikać ustaloną adresów URL, co pomoże w utrzymaniu.
* Generowanie adresu URL opiera się na trasach, które obsługują dowolną rozszerzalności. `IUrlHelper` udostępnia metody do tworzenia adresów URL.

::: moniker-end

Routing jest podłączony do [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) potoku przez <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> klasy. [Platforma ASP.NET Core MVC](xref:mvc/overview) dodaje routingu do potoku oprogramowania pośredniczącego jako część swojej konfiguracji i obsługuje routing w aplikacji MVC i stron Razor. Aby dowiedzieć się, jak używać routingu jako składnik autonomicznych, zobacz [użycia routingu oprogramowania pośredniczącego](#use-routing-middleware) sekcji.

### <a name="url-matching"></a>Dopasowanie adresu URL

::: moniker range=">= aspnetcore-2.2"

Dopasowanie adresu URL jest proces, który wysyła routingu przychodzącego żądania *punktu końcowego*. Ten proces jest na podstawie danych ze ścieżki adresu URL, ale może zostać rozszerzony do należy wziąć pod uwagę wszystkie dane w żądaniu. Możliwość wysyłania żądań do rozdzielenia obsługi to klucz do skalowania, rozmiar i złożoność aplikacji.

System routingu w routingu punkt końcowy jest odpowiedzialny za wszystkie dispatching decyzje. Ponieważ oprogramowanie pośredniczące stosuje zasady, w oparciu o wybrany punkt końcowy, ważne jest, czy każda decyzja może wpłynąć na wysyłanie lub stosowania zasad zabezpieczeń składa się w systemie routingu.

Kiedy delegat punkt końcowy jest wykonywana, sprawdzanie właściwości `RouteContext.RouteData` są ustawione na odpowiednie wartości oparte na przetwarzanie żądań wykonywane do tej pory.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Dopasowanie adresu URL jest proces, który wysyła routingu przychodzącego żądania *obsługi*. Ten proces jest na podstawie danych ze ścieżki adresu URL, ale może zostać rozszerzony do należy wziąć pod uwagę wszystkie dane w żądaniu. Możliwość wysyłania żądań do rozdzielenia obsługi to klucz do skalowania, rozmiar i złożoność aplikacji.

Wprowadź żądań przychodzących `RouterMiddleware`, która wywołuje metodę <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> metody dla każdej trasy w sekwencji. <xref:Microsoft.AspNetCore.Routing.IRouter> Wybierze wystąpienie czy *obsługi* żądania przez ustawienie [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) do innych niż null <xref:Microsoft.AspNetCore.Http.RequestDelegate>. Jeśli trasa Ustawia program obsługi żądania, route zatrzymanie przetwarzania i program obsługi jest wywoływane w celu przetworzenia żądania. Jeśli żadna procedura obsługi trasy zostanie znaleziony można przetworzyć żądania, oprogramowanie pośredniczące przekazywało żądanie do następnego oprogramowania pośredniczącego w potoku żądania.

Podstawowe dane wejściowe `RouteAsync` jest [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) skojarzone z bieżącego żądania. `RouteContext.Handler` i [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) są dane wyjściowe po dopasowaniu trasy.

Dopasowanie, która wywołuje `RouteAsync` spowoduje także ustawienie właściwości `RouteContext.RouteData` odpowiednie wartości oparte na przetwarzanie żądań wykonywane do tej pory.

::: moniker-end

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) jest słownikiem *wartości trasy* wyprodukowanych z trasy. Wartości te są zwykle określane przez tokenizowanie adres URL i może służyć do przyjmowania danych wejściowych użytkownika lub dodatkowo dispatching decyzje wewnątrz aplikacji.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) to zbiór właściwości dodatkowych danych związanych z dopasowanej trasy. `DataTokens` dostarczone dane o pomocy technicznej kojarzenie stanu z każdej trasy tak, aby aplikacja podejmować decyzje oparte na trasie, która pasuje. Te wartości są definiowane przez projektanta i wykonaj **nie** mają wpływ na zachowanie routingu w dowolny sposób. Ponadto przechowalni wartości w `RouteData.DataTokens` może być dowolnego typu, w przeciwieństwie do `RouteData.Values`, która musi być konwertowany do i z ciągów.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) znajduje się lista tras, na których uczestniczyła w pomyślnie dopasowywania żądania. Trasy mogą być zagnieżdżone wewnątrz siebie nawzajem. `Routers` Właściwość odzwierciedla drogę przez drzewo logiczne tras, które spowodowały dopasowanie. Ogólnie rzecz biorąc, pierwszy element `Routers` jest kolekcją tras i powinny być używane do generowania adresu URL. Ostatnim elementem w `Routers` jest programu obsługi trasy, który jest zgodny.

### <a name="url-generation"></a>Generowanie adresu URL

::: moniker range=">= aspnetcore-2.2"

Generowanie adresu URL to proces, routingu, które można utworzyć ścieżki adresu URL na podstawie zestawu wartości trasy. Dzięki temu do logicznego rozdzielania między punktami końcowymi usługi oraz w adresach URL, które do nich dostęp.

Routing punktu końcowego zawiera interfejsu API Generator łącza (`LinkGenerator`). `LinkGenerator` to usługa singleton, który można pobrać z DI. Interfejs API mogą być używane poza Kontekst wykonywania żądania. MVC `IUrlHelper` i scenariusze, które zależą od `IUrlHelper`, takich jak [pomocników tagów](xref:mvc/views/tag-helpers/intro), pomocników HTML i [wyników akcji](xref:mvc/controllers/actions), użyj generatora łącze, aby podać możliwości generowania łącza.

Generator łącze opiera się na koncepcji *adres* i *adres, systemy*. Schemat adresów to sposób określenia punktów końcowych, które należy rozważyć dotyczącymi generowania łączy. Na przykład nazwy trasy i scenariusze wartości trasy, przez wielu użytkowników są znane ze strony MVC i Razor są implementowane jako schemat adresów.

Generator łącza można połączyć do stron MVC i Razor akcje i stron za pomocą następujących metod rozszerzenia:

* `GetPathByAction`
* `GetUriByAction`
* `GetPathByPage`
* `GetUriByPage`

Przeciążenia metody te akceptuje argumenty, które obejmują `HttpContext`. Te metody są funkcjonalnie równoważne `Url.Action` i `Url.Page` , ale oferują one większą elastyczność i opcje.

`GetPath*` Metody są najbardziej podobny do `Url.Action` i `Url.Page` , generują identyfikatorem URI zawierającym ścieżką bezwzględną. `GetUri*` Metody zawsze Generuj bezwzględny identyfikator URI zawierający schematu i hosta. Metody, które akceptują `HttpContext` wygenerować identyfikator URI w kontekście wykonywania żądania. Wartości trasy otoczenia, adres URL ścieżki podstawowej, schematu i hosta z wykonywania żądania są używane, chyba że zastąpione.

`LinkGenerator` jest wywoływana przy użyciu adresu. Generowanie identyfikatora URI odbywa się w dwóch etapach:

1. Adres jest powiązany z listy punktów końcowych, które pasują do adresu.
1. Każdy punkt końcowy `RoutePattern` jest szacowana aż do znalezienia wzorzec trasy, który pasuje do podanej wartości. Dane wyjściowe w połączeniu z innymi częściami URI dostarczane do generatora łącze i zwrócony.

Metod dostarczonych przez `LinkGenerator` obsługują możliwości generowania łącza standardowego dla dowolnego typu adresu. Jest Najwygodniejszym sposobem użycia link generator za pośrednictwem metody rozszerzenia, wykonujących operacje dla typu określonego adresu.

| Metoda rozszerzenia   | Opis                                                         |
| ------------------ | ------------------------------------------------------------------- |
| `GetPathByAddress` | Generuje identyfikator URI przy użyciu ścieżki bezwzględnej na podstawie podanej wartości. |
| `GetUriByAddress`  | Generuje bezwzględny identyfikator URI na podstawie podanej wartości.             |

> [!WARNING]
> Należy zwrócić uwagę na następujące implikacje wywoływania `LinkGenerator` metody:
>
> * Użyj `GetUri*` metody rozszerzenia z rozwagą w konfiguracji aplikacji, która nie weryfikuje `Host` nagłówka żądania przychodzące. Jeśli `Host` nie jest zweryfikowany nagłówka żądania przychodzące, żądania niezaufanych danych wejściowych może zostać odesłany do klienta w identyfikatorach URI w widoku/strony. Zaleca się, że wszystkie aplikacje produkcyjne skonfigurować ich serwera, aby sprawdzić poprawność `Host` nagłówka przed znanym prawidłowych wartości.
>
> * Użyj `LinkGenerator` ostrożnie w oprogramowaniu pośredniczącym w połączeniu z `Map` lub `MapWhen`. `Map*` zmiany ścieżki podstawowej wykonywania żądania ma wpływ na dane wyjściowe generowania łączy. Wszystkie `LinkGenerator` interfejsy API umożliwiają określenie ścieżki podstawowej. Zawsze określać pustej ścieżki podstawowej, aby cofnąć `Map*`jego wpływ na generowanie konsolidacji.

## <a name="differences-from-earlier-versions-of-routing"></a>Różnice z wcześniejszych wersji routingu

Kilka istnieją różnice między punktu końcowego routingu w programie ASP.NET Core 2.2 lub nowszej i wcześniejszych wersjach routingu w programie ASP.NET Core:

* System routingu punkt końcowy nie obsługuje `IRouter`— na podstawie rozszerzalności, w tym dziedziczenie z `Route`.

* Punkt końcowy routingu nie obsługuje [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim). Użyj 2.1 [zgodność wersji](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) aby kontynuować korzystanie z podkładki zgodności.

* Routingu punkt końcowy ma różne zachowania dla małych i wielkich liter wygenerowanego identyfikatorów URI, korzystając z konwencjonalnych trasy.

  Należy wziąć pod uwagę następujący szablon trasy domyślne:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Załóżmy, że wygenerować łącze do akcji, używając następujących tras:

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  Za pomocą `IRouter`-routing oparty na, ten kod generuje identyfikator URI zestawu `/blog/ReadPost/17`, który uwzględnia wielkość liter w wyrazie wartość podana trasy. Tworzy punkt końcowy routingu w programie ASP.NET Core 2.2 lub nowszym `/Blog/ReadPost/17` (Wielka "Blog"). Udostępnia punkt końcowy routingu `IOutboundParameterTransformer` interfejsu, który może służyć do dostosowywania zachowania globalnie lub aby zastosować różnych konwencji mapowania adresów URL.

  Aby uzyskać więcej informacji, zobacz [odwołania do parametru transformatora](#parameter-transformer-reference) sekcji.

* Generowanie konsolidacji używane przez strony MVC i Razor z konwencjonalnych trasy zachowuje się inaczej, podczas próby łącze do kontrolera/akcji lub strona, która nie istnieje.

  Należy wziąć pod uwagę następujący szablon trasy domyślne:

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Załóżmy, że wygenerować łącze do akcji, używając domyślnego szablonu z następujących czynności:

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  Za pomocą `IRouter`-routing oparty na, wynik jest zawsze `/Blog/ReadPost/17`, nawet jeśli `BlogController` nie istnieje lub nie ma `ReadPost` metody akcji. Tworzy punkt końcowy routingu w programie ASP.NET Core 2.2 lub nowszym zgodnie z oczekiwaniami, `/Blog/ReadPost/17` istnienia metody akcji. *Jednak routingu punkt końcowy tworzy ciąg pusty, jeśli akcja nie istnieje.* Model routingu punkt końcowy nie zakłada, że punkt końcowy istnieje, jeśli akcja nie istnieje.

* Generowanie konsolidacji *algorytm unieważniania otoczenia wartość* zachowuje się inaczej, gdy jest używana z routingiem punktu końcowego.

  *Wartość otoczenia unieważniania* to algorytm, który decyduje, wartości trasy, które z obecnie wykonywanym żądania (wartości otoczenia) mogą być używane w operacje generowania łączy. Tradycyjnie routing zawsze unieważniony wartości dodatkowe trasy, podczas łączenia się różne akcje. Routing atrybutów w tamtym tego zachowania, przed wydaniem programu ASP.NET Core 2.2. We wcześniejszych wersjach programu ASP.NET Core łącza do innej akcji, używanego przez takie same nazwy parametrów trasy spowodowały błędy generowania łączy. W programie ASP.NET Core 2.2 lub nowszej obie formy routingu unieważnienie wartości podczas łączenia się z inną akcję.

  Rozważmy następujący przykład platformy ASP.NET Core 2.1 lub wcześniej. Podczas łączenia z inną akcję (lub innej strony), wartości trasy można będzie wykorzystać ponownie w sposób niepożądane.

  W */Pages/Store/Product.cshtml*:

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  W */Pages/Login.cshtml*:

  ```cshtml
  @page "{id?}"
  ```

  Jeśli identyfikator URI jest `/Store/Product/18` platformy ASP.NET Core 2.1 lub wcześniej, łącze na stronie Store/informacji przez `@Url.Page("/Login")` jest `/Login/18`. `id` Wartość 18 zostanie ponownie użyty, nawet jeśli miejsce docelowe łącza jest całkowicie różne części aplikacji. `id` Trasy wartości w kontekście `/Login` strony jest prawdopodobnie wartość Identyfikatora użytkownika, nie wartość Identyfikatora produktu magazynu.

  W punkcie końcowym routingu za pomocą platformy ASP.NET Core 2.2 lub nowszej, wynik jest `/Login`. Otoczenia wartości nie są ponownie użyte w przypadku połączonych miejscem docelowym jest inną akcję lub strony.

* Składnia parametru trasy Pełna zgodnooć wersji: Ukośników nie są zakodowane, korzystając z double — gwiazdka (`**`) parametrze wychwytywania składni.

  Podczas generowania łącza routingu systemu koduje wartość przechwycone z double — gwiazdka (`**`) parametrze wychwytywania (na przykład `{**myparametername}`) z wyjątkiem kreski ułamkowe. Przechwytującą cały gwiazdki podwójnej precyzji jest obsługiwane w przypadku `IRouter`— na podstawie routingu w programie ASP.NET Core 2.2 lub nowszym.

  Składnia parametru wychwytywania jednej gwiazdki w poprzednich wersjach programu ASP.NET Core (`{*myparametername}`) będą nadal obsługiwane i są zakodowane ukośników.

  | trasy              | Link generowane przez program<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts` (ukośnik jest zakodowany)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Przykład oprogramowania pośredniczącego

W poniższym przykładzie korzysta z oprogramowania pośredniczącego `LinkGenerator` interfejsu API, aby utworzyć łącze do metody akcji, która wyświetla listę przechowywane produktów. Za pomocą generatora łącza, wstawianie go do klasy i wywoływania `GenerateLink` jest dostępny dla każdej klasy w aplikacji.

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Generowanie adresu URL to proces, routingu, które można utworzyć ścieżki adresu URL na podstawie zestawu wartości trasy. Dzięki temu do logicznego rozdzielania między obsługi trasy i adresów URL, które do nich dostęp.

Generowanie adresu URL następuje podobny proces iteracyjny, ale zaczyna się od kodu użytkownika lub framework wywoływanie <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> metoda kolekcji tras. Każdy *trasy* ma jego `GetVirtualPath` metoda wywoływana w sekwencji, aż do innych niż null <xref:Microsoft.AspNetCore.Routing.VirtualPathData> jest zwracana.

Podstawowy danych wejściowych do `GetVirtualPath` są:

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext*)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)

Trasy przede wszystkim używasz wartości trasy, dostarczone przez `Values` i `AmbientValues` zdecydować, czy jest możliwe do generowania adresu URL i wartości, których do uwzględnienia. `AmbientValues` Zestaw wartości trasy, które zostały utworzone z dopasowywania bieżącego żądania. Z kolei `Values` są wartości trasy, które określają sposób generowania żądany adres URL dla bieżącej operacji. `HttpContext` Znajduje się w przypadku, gdy trasa należy pozyskać services lub dodatkowe dane skojarzone z bieżącym kontekstem.

> [!TIP]
> Traktować [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) jako zbiór zastąpienia [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). Generowanie adresu URL podejmie próbę ponownego użycia wartości tras z bieżącego żądania do generowania adresów URL dla łącza przy użyciu tego samego trasę lub wartości trasy.

Dane wyjściowe `GetVirtualPath` jest `VirtualPathData`. `VirtualPathData` jest równolegle z `RouteData`. `VirtualPathData` zawiera `VirtualPath` dla adresu URL danych wyjściowych i niektóre dodatkowe właściwości, które powinny zostać ustawione przez trasę.

[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) właściwość zawiera *ścieżka wirtualna* produkowane przez trasę. W zależności od potrzeb może być konieczne ścieżkę do dalszego przetwarzania. Jeśli chcesz renderować wygenerowany adres URL w formacie HTML, należy poprzedzić ścieżki podstawowej aplikacji.

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) jest odwołaniem do pomyślnie wygenerowano adres URL trasy.

[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) właściwości jest słownikiem dodatkowych danych dotyczących trasy, który wygenerował adresu URL. Jest to równoległego z [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

::: moniker-end

### <a name="create-routes"></a>Tworzenie tras

::: moniker range="< aspnetcore-2.2"

Routing zapewnia <xref:Microsoft.AspNetCore.Routing.Route> klasy jako standardowej implementacji <xref:Microsoft.AspNetCore.Routing.IRouter>. `Route` używa *szablon trasy* Składnia umożliwiająca zdefiniowanie wzorców do dopasowywania ścieżki adresu URL po <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> jest wywoływana. `Route` używa tego samego szablonu trasy do wygenerowania adresu URL po `GetVirtualPath` jest wywoływana.

::: moniker-end

Większość aplikacji utworzyć trasy, wywołując <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> lub jednego z podobne metody rozszerzenia zdefiniowane na <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Jedną z `IRouteBuilder` metody rozszerzenia Utwórz wystąpienie obiektu <xref:Microsoft.AspNetCore.Routing.Route> i dodać go do kolekcji tras.

::: moniker range=">= aspnetcore-2.2"

`MapRoute` nie akceptuje parametrów programu obsługi trasy. `MapRoute` tylko dodaje trasy, które są obsługiwane przez <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Aby dowiedzieć się więcej o trasach na platformie MVC, zobacz <xref:mvc/controllers/routing>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

`MapRoute` nie akceptuje parametrów programu obsługi trasy. `MapRoute` tylko dodaje trasy, które są obsługiwane przez <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Domyślny program obsługi jest `IRouter`, a program obsługi nie może obsłużyć żądania. Na przykład ASP.NET Core MVC jest zazwyczaj skonfigurowany jako domyślny program obsługi, który obsługuje tylko żądania które pasują do dostępnych kontrolerów i akcji. Aby dowiedzieć się więcej o trasach na platformie MVC, zobacz <xref:mvc/controllers/routing>.

::: moniker-end

Poniższy przykład kodu jest przykładem `MapRoute` wywołania używany przez typowy definicję trasy ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Ten szablon jest zgodny ścieżki adresu URL i wyodrębnia wartości trasy. Na przykład ścieżka `/Products/Details/17` generuje następujące wartości trasy: `{ controller = Products, action = Details, id = 17 }`.

Wartości trasy są określane przez dopasowanie każdego segmentu i dzielenia Ścieżka adresu URL na segmenty *kierowanie parametru* nazwy szablonu trasy. Noszą nazwy parametrów trasy. Parametry zdefiniowane, umieszczając nazwę parametru w nawiasach klamrowych `{ ... }`.

Powyższy szablon może być również zgodna ze ścieżką URL `/` i generować wartości `{ controller = Home, action = Index }`. Dzieje się tak dlatego `{controller}` i `{action}` parametrów trasy mają przypisane wartości domyślne i `id` parametr trasy jest opcjonalny. Znak równości (`=`) następuje wartość po nazwę parametru trasa określa wartość domyślną dla parametru. Znak zapytania (`?`) po nazwę parametru trasa określa opcjonalny parametr.

Parametry o wartości domyślne trasy *zawsze* uzyskiwania wartości trasy, gdy jest zgodna z trasy. Parametry opcjonalne nie tworzy wartości trasy, jeśli nie było żadnych odpowiadającym segmencie ścieżki adresu URL. Zobacz [przekierować odwołanie do szablonu](#route-template-reference) sekcji dokładny opis scenariuszy szablon trasy i składni.

W poniższym przykładzie definicji parametru trasy `{id:int}` definiuje [trasy ograniczenie](#route-constraint-reference) dla `id` kierowanie parametru:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Ten szablon jest zgodny Ścieżka adresu URL, takich jak `/Products/Details/17` , ale nie `/Products/Details/Apples`. Implementowanie ograniczenia trasy <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> i badać wartości trasy, aby je zweryfikować. W tym przykładzie wartość trasy `id` musi być konwertowany na liczbę całkowitą. Zobacz [odwołanie w przypadku ograniczenia trasy](#route-constraint-reference) objaśnienia dotyczące ograniczenia trasy, dostarczanych przez szablon.

Dodatkowe przeciążenia `MapRoute` akceptowanych wartości `constraints`, `dataTokens`, i `defaults`. Jest typowy tych parametrów do przekazania obiektu anonimowo wpisane, gdzie nazwy właściwości typu anonimowego dopasowania trasy nazwy parametrów.

Następujące `MapRoute` przykłady tworzą równoważne:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> Składnię w tekście do definiowania ograniczeń i ustawień domyślnych może być wygodną proste trasy. Jednak istnieją scenariusze, takie jak tokeny danych, które nie są obsługiwane przez składnię w tekście.

W poniższym przykładzie pokazano kilka dodatkowych scenariuszy:

::: moniker range=">= aspnetcore-2.2"

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Powyższy szablon odpowiada Ścieżka adresu URL, takich jak `/Blog/All-About-Routing/Introduction` i wyodrębnia wartości `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Trasy domyślnej wartości `controller` i `action` są produkowane przez trasy, nawet jeśli w szablonie nie istnieją odpowiednie parametry trasy. Można określić wartości domyślnych w szablonie trasy. `article` Parametr trasy jest zdefiniowany jako *wychwytywania* za wyglądu double gwiazdki (`**`) przed nazwą parametru trasy. Parametry trasy wychwytywania Przechwytywanie pozostałą część ścieżki adresu URL i można także dopasować pusty ciąg.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Powyższy szablon odpowiada Ścieżka adresu URL, takich jak `/Blog/All-About-Routing/Introduction` i wyodrębnia wartości `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Trasy domyślnej wartości `controller` i `action` są produkowane przez trasy, nawet jeśli w szablonie nie istnieją odpowiednie parametry trasy. Można określić wartości domyślnych w szablonie trasy. `article` Parametr trasy jest zdefiniowany jako *wychwytywania* za wyglądu znak gwiazdki (`*`) przed nazwą parametru trasy. Parametry trasy wychwytywania Przechwytywanie pozostałą część ścieżki adresu URL i można także dopasować pusty ciąg.

::: moniker-end

W poniższym przykładzie dodano tokeny ograniczeń i dane trasy:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Powyższy szablon odpowiada Ścieżka adresu URL, takich jak `/en-US/Products/5` i wyodrębnia wartości `{ controller = Products, action = Details, id = 5 }` i tokeny danych `{ locale = en-US }`.

![Tokeny Windows zmiennych lokalnych](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Generowanie adresu URL klasy trasy

`Route` Klasy można również wykonać Generowanie adresu URL, łącząc zestaw wartości trasy przy użyciu szablonu trasy. Jest to logicznie procesu zgodnych ze ścieżką URL.

> [!TIP]
> Aby lepiej zrozumieć Generowanie adresu URL, Wyobraź sobie adresu URL, które chcesz wygenerować, a następnie zastanów się, jak szablon trasy będzie odpowiadać tego adresu URL. Wartości, których będzie generowany? Jest to równoważne nierównej działania Generowanie adresu URL `Route` klasy.

W poniższym przykładzie użyto ogólne domyślnej trasy ASP.NET Core MVC:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Za pomocą wartości trasy `{ controller = Products, action = List }`, adres URL `/Products/List` jest generowany. Wartości trasy są zastępowane odpowiednich parametrów trasy w celu utworzenia ze ścieżką URL. Ponieważ `id` jest opcjonalny parametr trasy, adres URL został pomyślnie wygenerowany bez wartości dla `id`.

Za pomocą wartości trasy `{ controller = Home, action = Index }`, adres URL `/` jest generowany. Wartości podane trasy zgodne z wartościami domyślnymi i segmentów odpowiadający wartości domyślne są bezpiecznie pominięte.

Zarówno Rundy adresy URL generowane przy użyciu następującej definicji trasy (`/Home/Index` i `/`) generuje te same wartości trasy, które były używane do generowania adresu URL.

> [!NOTE]
> Skorzystaj z aplikacji za pomocą platformy ASP.NET Core MVC <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> do generowania adresów URL zamiast wywoływać metodę do routingu bezpośrednio.

Aby uzyskać więcej informacji na temat generowania adresu URL, zobacz [odwołanie do generowania adresu Url](#url-generation-reference) sekcji.

## <a name="use-routing-middleware"></a>Użyj oprogramowania pośredniczącego routingu

Odwołanie [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) w pliku projektu aplikacji.

Dodaj routingu do kontenera usługi w `Startup.ConfigureServices`:

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Tras muszą być skonfigurowane w `Startup.Configure` metody. Przykładowa aplikacja korzysta z poniższych interfejsów API:

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Dopasowuje tylko żądania HTTP GET.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

W poniższej tabeli przedstawiono odpowiedzi z danym identyfikatorów URI.

| Identyfikator URI                    | Odpowiedź                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Cześć! Wartości trasy: [operacji tworzenia], [id, 3] |
| `/package/track/-3`    | Cześć! Wartości trasy: [operacji, Śledź], [identyfikator -3] |
| `/package/track/-3/`   | Cześć! Wartości trasy: [operacji, Śledź], [identyfikator -3] |
| `/package/track/`      | Wypada żądania za pośrednictwem Brak dopasowania.              |
| `GET /hello/Joe`       | Witaj, Jan!                                          |
| `POST /hello/Joe`      | Żądanie znajduje się za pośrednictwem, odpowiada tylko HTTP GET. |
| `GET /hello/Joe/Smith` | Wypada żądania za pośrednictwem Brak dopasowania.              |

::: moniker range="< aspnetcore-2.2"

Jeśli konfigurujesz jedną trasę wywołać <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> przekazując `IRouter` wystąpienia. Nie będzie konieczne użycie <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

::: moniker-end

Framework udostępnia zestaw metod rozszerzenia do tworzenia tras (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):

* `MapDelete`
* `MapGet`
* `MapMiddlewareDelete`
* `MapMiddlewareGet`
* `MapMiddlewarePost`
* `MapMiddlewarePut`
* `MapMiddlewareRoute`
* `MapMiddlewareVerb`
* `MapPost`
* `MapPut`
* `MapRoute`
* `MapVerb`

::: moniker range="< aspnetcore-2.2"

Niektóre z wymienionych metod, takich jak `MapGet`, wymagają `RequestDelegate`. `RequestDelegate` Służy jako *programu obsługi trasy* gdy trasa odpowiada. Inne metody w tej rodzinie umożliwiają konfigurowanie potoku oprogramowania pośredniczącego do użytku jako program obsługi trasy. Jeśli `Map*` metody nie zaakceptuje program obsługi, takie jak `MapRoute`, używa ona <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

::: moniker-end

`Map[Verb]` Metody używać ograniczeń w celu ograniczenia trasy do czasownik HTTP w nazwie metody. Na przykład zobacz <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> i <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Odwołanie do szablonu trasy

Tokeny w nawiasy klamrowe (`{ ... }`) Zdefiniuj *parametry trasy* , są powiązane, jeśli trasa jest zgodny. Można zdefiniować więcej niż jeden parametr trasy w segmencie trasy, ale musi być oddzielony wartości literału. Na przykład `{controller=Home}{action=Index}` nie jest prawidłową trasę, ponieważ nie ma żadnych wartości literału między `{controller}` i `{action}`. Te parametry trasy musi mieć nazwę i może określić dodatkowe atrybuty.

Tekst dosłowny niż parametrów trasy (na przykład `{id}`) i separatora ścieżki `/` musi być zgodna z tekstem w adresie URL. Dopasowywanie tekstu odbywa się bez uwzględniania wielkości liter i jest oparta na zdekodowany reprezentacja ścieżki URL. Aby dopasować Ogranicznik parametru trasy literału (`{` lub `}`), znak ucieczki ogranicznik, powtarzając znak (`{{` lub `}}`).

Wzorce adresów URL, które próbują przechwytywania nazwę pliku z rozszerzeniem nazwy pliku opcjonalne mają dodatkowe zagadnienia. Rozważmy na przykład szablon `files/{filename}.{ext?}`. Gdy obie wartości `filename` i `ext` istnieje, obie wartości są wypełnione. Jeśli tylko wartości `filename` istnieje w adresie URL, dopasowania tras, ponieważ końcowej kropki (`.`) jest opcjonalny. Następujące adresy URL dopasowania tej trasy:

* `/files/myFile.txt`
* `/files/myFile`

::: moniker range=">= aspnetcore-2.2"

Można używać gwiazdki (`*`) lub podwójny gwiazdki (`**`) jako prefiks parametru trasy, aby powiązać pozostałą część identyfikatora URI. Są to tak zwane *wychwytywania* parametrów. Na przykład `blog/{**slug}` dopasowuje dowolny identyfikator URI, który rozpoczyna się od `/blog` i ma jakąkolwiek wartość operatorze, która jest przypisana do `slug` wartości trasy. Parametry przechwytującą cały mogą być również zgodna pusty ciąg.

Parametrze wychwytywania odpowiednie znaki specjalne, gdy trasa służy do generowania adresu URL, w tym separatora ścieżki (`/`) znaków. Na przykład tras `foo/{*path}` wartościami trasy `{ path = "my/path" }` generuje `foo/my%2Fpath`. Należy pamiętać o zmienionym znaczeniu ukośnikiem. Aby znakach separatora ścieżki Rundy, należy użyć `**` prefiks parametru trasy. Trasa `foo/{**path}` z `{ path = "my/path" }` generuje `foo/my/path`.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Możesz użyć gwiazdki (`*`) jako prefiks parametru trasy, aby powiązać pozostałą część identyfikatora URI. Jest to nazywane *wychwytywania* parametru. Na przykład `blog/{*slug}` dopasowuje dowolny identyfikator URI, który rozpoczyna się od `/blog` i ma jakąkolwiek wartość operatorze, która jest przypisana do `slug` wartości trasy. Parametry przechwytującą cały mogą być również zgodna pusty ciąg.

Parametrze wychwytywania odpowiednie znaki specjalne, gdy trasa służy do generowania adresu URL, w tym separatora ścieżki (`/`) znaków. Na przykład tras `foo/{*path}` wartościami trasy `{ path = "my/path" }` generuje `foo/my%2Fpath`. Należy pamiętać o zmienionym znaczeniu ukośnikiem.

::: moniker-end

Może mieć parametrów trasy *wartości domyślne* wyznaczony przez określenie wartości domyślnej po nazwie parametru, rozdzielone znakiem równości (`=`). Na przykład `{controller=Home}` definiuje `Home` jako wartość domyślna dla `controller`. Wartością domyślną jest używany, jeśli wartość nie jest obecny w adresie URL, dla parametru. Parametry trasy są zmieniona na opcjonalną, dodając znak zapytania (`?`) na końcu nazwy parametru, jak `id?`. Różnica między wartościami opcjonalne i domyślne parametry trasy jest parametr trasy z wartością domyślną będzie zawsze daje wartość&mdash;opcjonalny parametr ma wartość tylko wtedy, gdy wartość zostanie podana w adresie URL żądania.

Parametry trasy może mieć ograniczenia, które musi odpowiadać wartości trasy, powiązany z adresu URL. Dodawanie dwukropka (`:`) i nazwa ograniczenia po Określa nazwę parametru trasy *ograniczenie w tekście* parametru trasy. Jeśli ograniczenie wymaga argumentów, są ujęte w nawiasy (`(...)`) po nazwie ograniczenia. Można określić wiele ograniczeń w tekście, dodając inny dwukropka (`:`) i nazwa ograniczenia.

Nazwa ograniczenia i argumenty są przekazywane do <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> usługi w celu utworzenia wystąpienia <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> do użycia podczas przetwarzania adresu URL. Na przykład szablon trasy `blog/{article:minlength(10)}` Określa `minlength` ograniczenie z argumentem `10`. Aby uzyskać więcej informacji na temat ograniczenia trasy i listy ograniczeń, dostarczanych przez szablon, zobacz [przekierować odwołanie do ograniczenia](#route-constraint-reference) sekcji.

::: moniker range=">= aspnetcore-2.2"

Parametry trasy może mieć również transformatory parametrów, które przekształcania wartości parametru podczas generowania łączy pasującego działania i stron, które mają adresy URL. Ograniczenia, np. parametr transformatory można dodać wbudowane do parametru trasy, dodając dwukropek (`:`) i nazwa transformatora po nazwie parametru trasy. Na przykład szablon trasy `blog/{article:slugify}` Określa `slugify` przekształcania. Aby uzyskać więcej informacji na transformatory parametru, zobacz [odwołania do parametru transformatora](#parameter-transformer-reference) sekcji.

::: moniker-end

Poniższa tabela przedstawia przykładowe szablony trasy i ich działania.

| Szablon trasy                           | Przykład dopasowywania identyfikatora URI    | Identyfikator URI żądania&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Jest zgodny tylko pojedynczą ścieżkę `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Dopasowuje i ustawia `Page` do `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Dopasowuje i ustawia `Page` do `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Mapuje `Products` kontrolera i `List` akcji.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Mapuje `Products` kontrolera i `Details` akcji (`id` równa 123). |
| `{controller=Home}/{action=Index}/{id?`} | `/`                     | Mapuje `Home` kontrolera i `Index` — metoda (`id` jest ignorowana).        |

Przy użyciu szablonu ogólnie jest najprostszym podejściem do obsługi routingu. Ograniczenia i ustawienia domyślne można również określić poza szablon trasy.

> [!TIP]
> Włącz [rejestrowania](xref:fundamentals/logging/index) aby zobaczyć, jak wbudowane, takie jak routing implementacji, `Route`, zgodne z żądaniami.

## <a name="reserved-routing-names"></a>Zastrzeżone nazwy routingu

Następujące słowa kluczowe są zarezerwowane nazwy i nie można użyć jako nazwy trasy i parametry:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Odwołanie do ograniczenia trasy

Ograniczenia trasy wykonania, gdy dopasowanie jest przeprowadzana w celu przychodzącego adresu URL i ścieżki adresu URL jest stokenizowana do wartości trasy. Ograniczenia trasy ogólnie badać wartości tras skojarzone za pośrednictwem szablon trasy i wprowadzić tak, / nie decyzję o tym, czy wartość jest dopuszczalne. Niektóre ograniczenia trasy umożliwia należy wziąć pod uwagę, czy żądania można kierować dane poza wartości trasy. Na przykład <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> można zaakceptować lub odrzucić żądanie oparte na jego zlecenie HTTP. Ograniczenia są używane w kierowania żądań i połącz generacji.

> [!WARNING]
> Nie używaj ograniczeń dla **sprawdzania danych wejściowych**. Jeżeli ograniczenia są używane na potrzeby **sprawdzania danych wejściowych**, nieprawidłowe dane wejściowe wyniki w *404 — Nie można odnaleźć* odpowiedzi zamiast *400 — Nieprawidłowe żądanie* przy użyciu odpowiedniego komunikatu o błędzie. Ograniczenia trasy są używane do **odróżnić** podobne tras, nie można sprawdzić poprawność danych wejściowych dla określonej trasy.

Poniższa tabela przedstawia przykład ograniczenia trasy, a ich oczekiwane zachowanie.

| ograniczenie | Przykład | Przykład dopasowań | Uwagi |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Dopasowuje dowolną liczbę całkowitą |
| `bool` | `{active:bool}` | `true`, `FALSE` | Dopasowuje `true` lub `false` (bez uwzględniania wielkości liter) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Pasuje do prawidłowego `DateTime` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Pasuje do prawidłowego `decimal` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Pasuje do prawidłowego `double` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Pasuje do prawidłowego `float` wartość (niezmiennej kultury — Zobacz ostrzeżenie) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Pasuje do prawidłowego `Guid` wartość |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Pasuje do prawidłowego `long` wartość |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Ciąg musi być co najmniej 4 znaki |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Ciąg musi zawierać nie więcej niż 8 znaków |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Ciąg musi być dokładnie 12 znaków. |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Ciąg musi zawierać co najmniej 8 i nie więcej niż 16 znaków |
| `min(value)` | `{age:min(18)}` | `19` | Wartość całkowita musi być co najmniej 18 |
| `max(value)` | `{age:max(120)}` | `91` | Wartość całkowita musi być nie więcej niż 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Wartość całkowita musi być co najmniej 18 lat, ale nie więcej niż 120 |
| `alpha` | `{name:alpha}` | `Rick` | Ciąg musi zawierać co najmniej jeden znak alfabetyczny (`a`-`z`, bez uwzględniania wielkości liter) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Ciąg musi być zgodny z wyrażeniem regularnym (Zobacz porady dotyczące definiując wyrażenie regularne) |
| `required` | `{name:required}` | `Rick` | Używane do wymuszania, że wartość parametru nie jest obecny podczas generowania adresu URL |

Wiele ograniczeń rozdzielana średnikami można zastosować do jednego parametru. Na przykład następujące ograniczenia ogranicza parametr na wartość całkowitą równą 1 lub większą:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Trasy, ograniczenia, sprawdź adres URL, które są konwertowane na typ CLR (takie jak `int` lub `DateTime`) zawsze używają niezmiennej kultury. Te ograniczenia przyjęto założenie, że adres URL jest niemożliwe do zlokalizowania. Ograniczenia trasy dostarczone przez framework nie należy modyfikować wartości przechowywane w wartości trasy. Wszystkie wartości trasy, pochodzącą z adresu URL analizy są przechowywane jako ciągi. Na przykład `float` ograniczenie podejmuje próbę przekonwertowania na format zmiennoprzecinkowy wartości trasy, ale przekonwertowana wartość jest używana tylko po to, aby sprawdzić, może on zostać przekonwertowany na format zmiennoprzecinkowy.

## <a name="regular-expressions"></a>Wyrażenia regularne

Dodaje platformę ASP.NET Core `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` do konstruktora wyrażenia regularnego. Zobacz <xref:System.Text.RegularExpressions.RegexOptions> opis tych elementów członkowskich.

Wyrażenia regularne używać ograniczników i tokeny podobne do używanych przez usługę Routing i języka C#. Wyrażenie regularne tokeny muszą być wyjściowym. Można użyć wyrażenia regularnego `^\d{3}-\d{2}-\d{4}$` routingu, wyrażenie musi mieć `\` znaków (pojedynczy ukośnik odwrotny) podany w ciągu jako `\\` (podwójny ukośnik odwrotny), znaków w C# pliku źródłowego, aby można było wprowadzić `\` ciąg znaku ucieczki (chyba że za pomocą [literały ciąg verbatim](/dotnet/csharp/language-reference/keywords/string)). Jako znak ucieczki dla routingu znaki ogranicznika parametru (`{`, `}`, `[`, `]`), znaków w wyrażeniu dwukrotnie (`{{`, `}`, `[[`, `]]`). W poniższej tabeli przedstawiono wyrażeń regularnych i wersji o zmienionym znaczeniu.

| Wyrażenie regularne    | Wyrażenie regularne o zmienionym znaczeniu     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Wyrażenia regularne użyte w routingu często rozpoczynać się od karetki (`^`) znak i sprostania szczytowemu pozycję początkową ciągu. Wyrażenia często kończyć się znakiem dolara (`$`) znaków i dopasowania końca ciągu. `^` i `$` znaków upewnij się, że wartość parametru trasy całe dopasowanie wyrażenia regularnego. Bez `^` i `$` znaków, wyrażenie regularne odpowiada wszelkich podciągu wewnątrz ciągu, który jest często niepożądane. Poniższa tabela zawiera przykłady i wyjaśnia, dlaczego zgodne lub nie odpowiada.

| Wyrażenie   | String    | Dopasowanie | Komentarz               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | Cześć     | Tak   | podciąg dopasowania     |
| `[a-z]{2}`   | 123abc456 | Tak   | podciąg dopasowania     |
| `[a-z]{2}`   | mz        | Tak   | zgodne z wyrażeniem    |
| `[a-z]{2}`   | MZ        | Tak   | bez uwzględniania wielkości liter    |
| `^[a-z]{2}$` | Cześć     | Nie    | zobacz `^` i `$` powyżej |
| `^[a-z]{2}$` | 123abc456 | Nie    | zobacz `^` i `$` powyżej |

Aby uzyskać więcej informacji na temat składni wyrażeń regularnych, zobacz [wyrażeń regularnych programu .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Aby ograniczyć parametr znany zestaw możliwych wartości, należy użyć wyrażenia regularnego. Na przykład `{action:regex(^(list|get|create)$)}` jest zgodny tylko `action` trasy wartość `list`, `get`, lub `create`. Jeśli przekazywana do słownika ograniczenia, ciąg `^(list|get|create)$` odpowiada. Ograniczenia, które są przekazywane w słowniku ograniczenia (niewyrównane w szablonie), która nie pasuje do jednej znane ograniczenia również są traktowane jako wyrażenia regularne.

::: moniker range=">= aspnetcore-2.2"

## <a name="parameter-transformer-reference"></a>Odwołania do parametru transformatora

Parametr transformatory:

* Wykonania podczas generowania łącza dla `Route`.
* Implementowanie `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Zostały skonfigurowane za pomocą <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Pobrać parametru, wartości trasy i przekształć go w nową wartość ciągu.
* Wynikiem przy użyciu wartości przekształcone w wygenerowane łącze.

Na przykład niestandardowy `slugify` transformatora parametr we wzorcu trasy `blog\{article:slugify}` z `Url.Action(new { article = "MyTestArticle" })` generuje `blog\my-test-article`.

Aby użyć transformatora parametr we wzorcu trasy, jest skonfigurowana najpierw za pomocą <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> w `Startup.ConfigureServices`:

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

Parametr transformatory są używane przez platformę do przekształcania identyfikatora URI, gdzie jest rozpoznawany jako punkt końcowy. Na przykład ASP.NET Core MVC używa parametru transformatory do przekształcania wartości trasy, używany do dopasowywania `area`, `controller`, `action`, i `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

Trasa poprzedniej akcji `SubscriptionManagementController.GetAll()` jest dopasowany do identyfikatora URI `/subscription-management/get-all`. Transformer parametru nie zmienia wartości trasy, używanego do generowania łącza. Na przykład `Url.Action("GetAll", "SubscriptionManagement")` generuje `/subscription-management/get-all`.

ASP.NET Core zawiera konwencje interfejsu API transformatory parametru przy użyciu wygenerowanego trasy:

* ASP.NET Core MVC ma `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` Konwencji interfejsu API. Ta Konwencja dotyczy transformatora określony parametr wszystkich tras atrybutów w aplikacji. Transformer parametr przekształca tokeny trasy atrybutu, ponieważ zostaną zastąpione. Aby uzyskać więcej informacji, zobacz [Użyj transformatora parametru, aby dostosować zastępowania tokenu](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Strony razor ma `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` Konwencji interfejsu API. Ta Konwencja dotyczy transformatora określony parametr wszystkie automatycznie odnalezione stron Razor. Transformer parametr przekształca folderu i pliku segmentów nazwy tras stron Razor. Aby uzyskać więcej informacji, zobacz [Użyj transformatora parametru, aby dostosować stronę trasy](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

::: moniker-end

## <a name="url-generation-reference"></a>Odwołanie do generowania adresu URL

Poniższy przykład pokazuje, jak wygenerować łącze do trasy, biorąc pod uwagę słownika wartości trasy i <xref:Microsoft.AspNetCore.Routing.RouteCollection>.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

`VirtualPath` Generowane na końcu poprzedniego przykładowe dane stanowią `/package/create/123`. Słownik dostarcza `operation` i `id` wartości szablonu "Śledzenie trasy pakietu" trasy `package/{operation}/{id}`. Aby uzyskać szczegółowe informacje, zobacz przykładowy kod [użycia routingu w oprogramowaniu pośredniczącym](#use-routing-middleware) sekcji lub [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Drugi parametr `VirtualPathContext` Konstruktor jest kolekcją *otoczenia wartości*. Otoczenia wartości są łatwe w użyciu, ponieważ ich Ogranicz liczbę wartości, które Deweloper należy określić w ramach kontekstu żądania. Bieżące wartości trasy, bieżącego żądania są traktowane jako wartości otoczenia dotyczącymi generowania łączy. W aplikacji ASP.NET Core MVC `About` akcji `HomeController`, nie musisz określić wartość trasy kontrolera, aby połączyć `Index` akcji&mdash;otoczenia wartość `Home` jest używany.

Otoczenia wartości, które nie jest zgodny z parametrem są ignorowane. Otoczenia wartości również są ignorowane, gdy jawnie podana jest wartość zastępuje wartość otoczenia. Dopasowywanie występuje od lewej do prawej w adresie URL.

Wartości podane jawnie, ale nie są zgodne segment trasy są dodawane do ciągu zapytania. W poniższej tabeli przedstawiono wyniki, korzystając z szablonu trasy `{controller}/{action}/{id?}`.

| Wartości otoczenia                     | Jawne wartości                        | Wynik                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| Kontroler = "Strona główna"                | Akcja = "About"                       | `/Home/About`           |
| Kontroler = "Strona główna"                | Kontroler = "Order", Akcja = "About" | `/Order/About`          |
| Kontroler = "Home", color = "Red" | Akcja = "About"                       | `/Home/About`           |
| Kontroler = "Strona główna"                | Akcja = "About" kolor = "Red"        | `/Home/About?color=Red` |

Jeśli ta wartość nie zostanie podany wprost trasy ma wartość domyślną, która nie jest zgodny z parametrem, musi być zgodna wartość domyślna:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

Generowanie konsolidacji tylko generuje link dla tej trasy podczas dopasowywania wartości `controller` i `action` są dostarczane.
