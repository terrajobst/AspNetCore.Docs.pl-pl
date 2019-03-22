---
title: Widoki częściowe w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak za pomocą widoków częściowych Podziel znaczników dużych plików i zmniejszenia duplikowania typowych znaczników na stronach sieci web w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2018
uid: mvc/views/partial
ms.openlocfilehash: b7c1545007086053e879bce6781802959da77901
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327381"
---
# <a name="partial-views-in-aspnet-core"></a>Widoki częściowe w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Scott Sauber](https://twitter.com/scottsauber)

Widok częściowy jest [Razor](xref:mvc/views/razor) pliku znaczników (*.cshtml*) który powoduje wyświetlenie danych wyjściowych HTML *w ramach* innego pliku znaczników użytkownika renderowania danych wyjściowych.

::: moniker range=">= aspnetcore-2.1"

Termin *widoku częściowego* jest używany podczas tworzenia obu aplikacji MVC, gdzie są nazywane plikami znaczników *widoków*, lub aplikacji stron Razor, w którym są nazywane plikami znaczników *stron*. W tym temacie ogólnie odnosi się do widoków MVC i stron Razor strony jako *plikami znaczników*.

::: moniker-end

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="when-to-use-partial-views"></a>Kiedy należy używać widoki częściowe

Widoki częściowe są efektywnym sposobem:

* Podziel znaczników dużych plików na mniejsze składniki.

  W dużych złożonych znaczników pliku składa się z wielu części logiczne, ma swoją zaletę do pracy z każdego z nich izolowane do widoku częściowego. Kod w pliku znaczników jest zarządzany, ponieważ znaczniki zawiera tylko ogólną strukturę strony i odwołań pozwalającą na widoki częściowe.
* Zmniejsz duplikowania typowych treść znaczników w plikach kodu znaczników.

  Te same elementy znaczników są używane w plikach kodu znaczników, widoku częściowego usuwa związanych z duplikowaniem treść znaczników w jeden plik widoku częściowego. Po zmianie kodu znaczników w widoku częściowego powoduje zaktualizowanie wyniku renderowania plików znaczników, które używają widoku częściowego.

Widoki częściowe nie powinien być używany do obsługi wspólnych elementów układu. Typowe elementy układu powinny być określone w [_Layout.cshtml](xref:mvc/views/layout) plików.

Nie używaj widoku częściowego których wykonywanie złożonych renderowania logiki lub kodu jest wymagane do renderowania kodu znaczników. Zamiast widoku częściowego, użyj [widoku składnika](xref:mvc/views/view-components).

## <a name="declare-partial-views"></a>Zadeklaruj widoki częściowe

::: moniker range=">= aspnetcore-2.0"

Widok częściowy jest *.cshtml* pliku znaczników utrzymane w *widoków* folder (MVC) lub *stron* folder (stron Razor).

W przypadku platformy ASP.NET Core MVC, kontroler <xref:Microsoft.AspNetCore.Mvc.ViewResult> zwraca widok lub widok częściowy. Podobne możliwości jest planowana na stron Razor programu ASP.NET Core 2.2. W przypadku stron Razor <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> może zwrócić <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>. Odwoływanie się do i renderowania widoków częściowych, które jest opisane w [odwoływać się do widoku częściowego](#reference-a-partial-view) sekcji.

W przeciwieństwie do widoku składnika MVC lub renderowania stron widoku częściowego nie zostanie uruchomiona *_ViewStart.cshtml*. Aby uzyskać więcej informacji na temat *_ViewStart.cshtml*, zobacz <xref:mvc/views/layout>.

Nazwy plików widoku częściowego często rozpoczynają się od znaku podkreślenia (`_`). Konwencja nazewnictwa nie jest wymagana, ale pomaga wizualnie odróżnienie widoki częściowe z widoków i stron.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Widok częściowy jest *.cshtml* pliku znaczników utrzymane w *widoków* folderu.

Kontroler <xref:Microsoft.AspNetCore.Mvc.ViewResult> zwraca widok lub widok częściowy.

W przeciwieństwie do renderowania widoku MVC, nie zostanie uruchomiona widoku częściowego *_ViewStart.cshtml*. Aby uzyskać więcej informacji na temat *_ViewStart.cshtml*, zobacz <xref:mvc/views/layout>.

Nazwy plików widoku częściowego często rozpoczynają się od znaku podkreślenia (`_`). Konwencja nazewnictwa nie jest wymagana, ale pomaga wizualnie odróżnienie widoki częściowe z widoków.

::: moniker-end

## <a name="reference-a-partial-view"></a>Odwołanie do widoku częściowego

::: moniker range=">= aspnetcore-2.1"

W pliku znaczników istnieje kilka sposobów, aby odwoływać się do widoku częściowego. Firma Microsoft zaleca aplikacji użyj jednej z następujących metod asynchronicznych renderowania:

* [Pomocnik tagu częściowego](#partial-tag-helper)
* [Asynchroniczne pomocnika kodu HTML](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

W pliku znaczników istnieją odwoływać się do widoku częściowego na dwa sposoby:

* [Asynchroniczne pomocnika kodu HTML](#asynchronous-html-helper)
* [Synchroniczne pomocnika kodu HTML](#synchronous-html-helper)

Firma Microsoft zaleca, aby używać aplikacji [asynchronicznego pomocnika kodu HTML](#asynchronous-html-helper).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Pomocnik tagu częściowego

[Pomocnik tagu częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) wymaga platformy ASP.NET Core 2.1 lub nowszej.

Częściowe Pomocnik tagu asynchronicznie renderuje zawartość i używa składni HTML:

```cshtml
<partial name="_PartialName" />
```

Jeśli występuje rozszerzenie pliku Pomocnik tagu odwołuje się widok częściowy, który musi znajdować się w tym samym folderze co wywołanie widoku częściowego pliku znaczników:

```cshtml
<partial name="_PartialName.cshtml" />
```

Poniższy przykład odwołuje się do widoku częściowego z katalogu głównego aplikacji. Ścieżki zaczynające się od ukośnika tyldy (`~/`) lub ukośnika (`/`) można znaleźć w katalogu głównym aplikacji:

**Strony Razor**

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

**MVC**

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

Poniższy przykład odwołuje się widok częściowy przy użyciu ścieżki względnej:

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

Aby uzyskać więcej informacji, zobacz <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.

::: moniker-end

### <a name="asynchronous-html-helper"></a>Asynchroniczne pomocnika kodu HTML

Korzystając z Pomocnika kodu HTML, najlepszym rozwiązaniem jest użycie <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>. `PartialAsync` Zwraca <xref:Microsoft.AspNetCore.Html.IHtmlContent> typu zapakowane w <xref:System.Threading.Tasks.Task%601>. Metoda odwołuje się do niej poprzedzania ich oczekiwane wywołanie za pomocą `@` znaków:

```cshtml
@await Html.PartialAsync("_PartialName")
```

Jeśli występuje rozszerzenie pliku pomocnika kodu HTML odwołuje się widok częściowy, który musi znajdować się w tym samym folderze co wywołanie widoku częściowego pliku znaczników:

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

Poniższy przykład odwołuje się do widoku częściowego z katalogu głównego aplikacji. Ścieżki zaczynające się od ukośnika tyldy (`~/`) lub ukośnika (`/`) można znaleźć w katalogu głównym aplikacji:

::: moniker range=">= aspnetcore-2.1"

**Strony Razor**

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

**MVC**

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

Poniższy przykład odwołuje się widok częściowy przy użyciu ścieżki względnej:

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

Alternatywnie można renderować widok częściowy przy użyciu <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>. Ta metoda nie zwraca <xref:Microsoft.AspNetCore.Html.IHtmlContent>. Strumieniowo wyniku renderowania bezpośrednio do odpowiedzi. Ponieważ metoda nie zwraca wyników, musi ona zostać wywołana w bloku kodu Razor:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

Ponieważ `RenderPartialAsync` strumieni renderowana zawartość, zapewnia lepszą wydajność w niektórych scenariuszach. W sytuacjach krytycznych dla wydajności testu porównawczego stronę korzystając z obu metod i użyć metody, która generuje szybciej uzyskać odpowiedź.

### <a name="synchronous-html-helper"></a>Synchroniczne pomocnika kodu HTML

<xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> i <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> są synchroniczne odpowiedników `PartialAsync` i `RenderPartialAsync`, odpowiednio. Odpowiedniki synchroniczne nie są zalecane, ponieważ istnieją scenariusze, w których one zakleszczenie. Także synchroniczne metody są przeznaczone do usunięcia w przyszłej wersji.

> [!IMPORTANT]
> Jeśli zachodzi potrzeba wykonania kodu, należy użyć [widoku składnika](xref:mvc/views/view-components) zamiast widoku częściowego.

::: moniker range=">= aspnetcore-2.1"

Wywoływanie `Partial` lub `RenderPartial` powoduje ostrzeżenie analizatora programu Visual Studio. Na przykład obecność `Partial` pojawi się następujący komunikat ostrzegawczy:

> Użyj IHtmlHelper.Partial może spowodować zakleszczenia aplikacji. Należy rozważyć użycie &lt;częściowe&gt; Pomocnik tagu lub IHtmlHelper.PartialAsync.

Zastępują wywołania `@Html.Partial` z `@await Html.PartialAsync` lub [Pomocnik tagu częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper). Aby uzyskać więcej informacji na temat migracji Pomocnik tagu częściowego, zobacz [migracja z usługi pomocnika kodu HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).

::: moniker-end

## <a name="partial-view-discovery"></a>Widok częściowy odnajdywania

Gdy widok częściowy odwołuje się nazwę bez rozszerzenia pliku, w określonej kolejności, przeszukiwane są następujące lokalizacje:

::: moniker range=">= aspnetcore-2.1"

**Strony Razor**

1. Na stronie folderu w trakcie wykonywania
1. Wykres katalogu powyżej folderu strony
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

**MVC**

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`
1. `/Pages/Shared`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`

::: moniker-end

Następujących Konwencji stosuje się do widoku częściowego odnajdywania:

* Widoki częściowe o takiej samej nazwie pliku są dozwolone, gdy widoki częściowe są w różnych folderach.
* Po odwołujące się do widoku częściowego według nazwy bez rozszerzenia pliku i widoku częściowego znajduje się w folderze zarówno wywołującego i *Shared* folderze widoku częściowego w folderze obiektu wywołującego dostarcza widoku częściowego. Jeśli widok częściowy nie jest obecny w folderze obiektu wywołującego, widoku częściowego znajduje się w *Shared* folderu. Częściowe widoki w *Shared* noszą nazwę folderu *udostępnione widoki częściowe* lub *domyślne widoki częściowe*.
* Widoki częściowe mogą być *łańcuchowa*&mdash;widoku częściowego może wywołać inny widok częściowy, jeśli odwołanie cykliczne nie jest tworzona przez wywołania. Ścieżki względne są zawsze względne wobec bieżącego pliku, a nie do katalogu głównego lub nadrzędnego pliku.

> [!NOTE]
> A [Razor](xref:mvc/views/razor) `section` zdefiniowane w częściowym widok jest niewidoczne dla nadrzędnej plikach kodu znaczników. `section` Jest widoczne tylko dla widoku częściowego, w którym jest zdefiniowany.

## <a name="access-data-from-partial-views"></a>Dostęp do danych z widoki częściowe

Podczas tworzenia wystąpienia widoku częściowego odbiera *kopiowania* z elementu nadrzędnego `ViewData` słownika. Aktualizacje wprowadzone do danych w widoku częściowego nie są utrwalane dla widoku nadrzędnego. `ViewData` zmiany w widoku częściowego zostaną utracone w przypadku, gdy zwraca widok częściowy.

W poniższym przykładzie pokazano, jak przekazać wystąpienia [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) do widoku częściowego:

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

Model można przekazać do widoku częściowego. Model można niestandardowych obiektów. Można przekazać model przy użyciu `PartialAsync` (powoduje wyświetlenie bloku zawartości do obiektu wywołującego) lub `RenderPartialAsync` (przesyła strumieniowo zawartość w danych wyjściowych):

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

**Strony Razor**

Następujący kod zawiera Przykładowa aplikacja pochodzi z *Pages/ArticlesRP/ReadRP.cshtml* strony. Strona zawiera dwa widoki częściowe. Drugi widoku częściowego przekazuje się w modelu i `ViewData` widoku częściowego. `ViewDataDictionary` Przeciążenia konstruktora jest używany do przekazywania nowej `ViewData` słownika przy zachowaniu istniejących `ViewData` słownika.

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-19)]

*Pages/Shared/_AuthorPartialRP.cshtml* jest pierwszy widok częściowy odwołuje się *ReadRP.cshtml* pliku znaczników:

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

*Pages/ArticlesRP/_ArticleSectionRP.cshtml* jest drugim widoku częściowego odwołuje się *ReadRP.cshtml* pliku znaczników:

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

**MVC**

::: moniker-end

Następujące znaczniki w pokazuje aplikacji przykładowej *Views/Articles/Read.cshtml* widoku. Widok zawiera dwa widoki częściowe. Drugi widoku częściowego przekazuje się w modelu i `ViewData` widoku częściowego. `ViewDataDictionary` Przeciążenia konstruktora jest używany do przekazywania nowej `ViewData` słownika przy zachowaniu istniejących `ViewData` słownika.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-19)]

*Views/Shared/_AuthorPartial.cshtml* jest pierwszy widok częściowy odwołuje się *ReadRP.cshtml* pliku znaczników:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

*Views/Articles/_ArticleSection.cshtml* jest drugim widoku częściowego odwołuje się *Read.cshtml* pliku znaczników:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

W czasie wykonywania, częściowe są renderowane w wyniku renderowania nadrzędnego pliku znaczników, który sam jest renderowany w ramach udostępnionej *_Layout.cshtml*. Pierwszy widoku częściowego powoduje wyświetlenie daty nazwy i publikacji Autor artykułu:

> Abraham Lincoln
>
> Ten widok częściowy z &lt;ścieżka pliku udostępnionego widoku częściowego&gt;.
> 11/19/1863 12:00:00 AM

Drugi widoku częściowego powoduje wyświetlenie sekcji tego artykułu:

> Sekcja jednego indeksu: 0
>
> Wynik czterech i siedem lat temu...
>
> Indeks dwóch części: 1
>
> Teraz możemy są zaangażowane w doskonałe wojnę, testowanie...
>
> Indeks sekcji 3: 2
>
> Jednak w sensie większe, firma Microsoft może rezerwuje...

## <a name="additional-resources"></a>Dodatkowe zasoby

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end
