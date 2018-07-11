---
title: Widoki częściowe w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak widok częściowy widoku, który jest renderowany w ramach innego widoku, a kiedy powinny być używane w aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/partial
ms.openlocfilehash: 983f3caae34b21b46d8f556e70673cf3c97abbd3
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938462"
---
# <a name="partial-views-in-aspnet-core"></a>Widoki częściowe w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Scott Sauber](https://twitter.com/scottsauber)

Platforma ASP.NET Core MVC obsługuje widoków częściowych, które są przydatne w przypadku udostępniania wielokrotnego użytku części strony sieci web w różnych widoków.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Co to są widoki częściowe

Widok częściowy jest widok, który jest renderowany w innym widoku. Dane wyjściowe HTML generowane przez wykonanie widoku częściowego jest renderowany w widoku telefonicznej (lub nadrzędnej). Jak widoki, widoki częściowe używają *.cshtml* rozszerzenie pliku.

Na przykład platformy ASP.NET Core 2.1 **aplikacji sieci Web** szablonu projektu obejmuje *_CookieConsentPartial.cshtml* widoku częściowego. Widok częściowy jest ładowany z poziomu *_Layout.cshtml*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_Layout.cshtml?name=snippet_CookieConsentPartial)]

## <a name="when-to-use-partial-views"></a>Kiedy należy używać widoki częściowe

Widoki częściowe są efektywnym sposobem rozdzielanie dużych widoki na mniejsze składniki. Mogą zmniejszyć dublowania wyświetlanie zawartości i elementy widoku ponowne użycie. Typowe elementy układu powinny być określone w [_Layout.cshtml](xref:mvc/views/layout). Zawartość do wielokrotnego użytku non układu są umieszczane na widoki częściowe.

Na stronie złożonego składa się z wielu części logiczne jest przydatne do pracy z każdego z nich jako swój własny widok częściowy. Każda część strony mogą być wyświetlane w izolacji od pozostałej części strony. Widok, w którym sama strona staje się prostszy, ponieważ zawiera on tylko ogólną strukturę strony i wywołań do renderowania widoków częściowych.

> [!TIP]
> Postępuj zgodnie z [nie Powtórz samodzielnie zasady](https://deviq.com/don-t-repeat-yourself/) w widoków.

## <a name="declare-partial-views"></a>Zadeklaruj widoki częściowe

Widoki częściowe są tworzone tak jak zwykły widoku&mdash;, tworząc *.cshtml* plików w ramach *widoków* folderu. Nie ma żadnej różnicy semantycznego między widok częściowy i regularnego widoku; jednak są one renderowane inaczej. Może mieć widoku, który jest zwracany bezpośrednio z poziomu kontrolera [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), a tym samym widoku może służyć jako widok częściowy. Główna różnica między sposób renderowania widoku oraz widoku częściowego jest, że widoki częściowe, nie uruchamiaj *_ViewStart.cshtml*. Widoki regularne uruchamianie *_ViewStart.cshtml*. Dowiedz się więcej o *_ViewStart.cshtml* w [układ](xref:mvc/views/layout)).

Konwencją nazw plików widoku częściowego często zaczynają się od `_`. Konwencja nazewnictwa nie jest to wymagane, ale pomaga wizualnie odróżnienie widoki częściowe z regularnych widoków.

## <a name="reference-a-partial-view"></a>Odwołanie do widoku częściowego

Na stronie widoku istnieje kilka sposobów wyświetlania widoku częściowego. Najlepszym rozwiązaniem jest do użycia renderowania asynchronicznego.

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Pomocnik tagu częściowego

Częściowe Pomocnik tagu wymaga platformy ASP.NET Core 2.1 lub nowszej. On renderowany asynchronicznie i używa składni HTML:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialTagHelper)]

Aby uzyskać więcej informacji, zobacz <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.

::: moniker-end

### <a name="asynchronous-html-helper"></a>Asynchroniczne pomocnika kodu HTML

Korzystając z Pomocnika kodu HTML, najlepszym rozwiązaniem jest użycie [PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync#Microsoft_AspNetCore_Mvc_Rendering_HtmlHelperPartialExtensions_PartialAsync_Microsoft_AspNetCore_Mvc_Rendering_IHtmlHelper_System_String_). Zwraca [IHtmlContent](/dotnet/api/microsoft.aspnetcore.html.ihtmlcontent) typu zapakowane w `Task`. Metoda odwołuje się do niej poprzedzania ich wywołania z `@`:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialAsync)]

Alternatywnie można renderować widok częściowy przy użyciu [RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync). Ta metoda nie zwraca wyniku. Strumieniowo wyniku renderowania bezpośrednio do odpowiedzi. Ponieważ metoda nie zwraca wyników, musi ona zostać wywołana w bloku kodu Razor:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

Ponieważ strumieniowo bezpośrednio, wynik `RenderPartialAsync` może działać lepiej w niektórych scenariuszach. Jednak zaleca się, że używasz `PartialAsync`.

### <a name="synchronous-html-helper"></a>Synchroniczne pomocnika kodu HTML

[Częściowe](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial) i [RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial) są synchroniczne odpowiedników `PartialAsync` i `RenderPartialAsync`, odpowiednio. Użyj odpowiedników synchroniczne nie jest zalecane, ponieważ istnieją scenariusze, w których one zakleszczenie. Przyszłe wersje nie będą zawierać metod synchronicznych.

> [!IMPORTANT]
> Jeśli widoków konieczne jest wykonanie kodu, należy użyć [widoku składnika](xref:mvc/views/view-components) zamiast widoku częściowego.

::: moniker range=">= aspnetcore-2.1"

W programie ASP.NET Core 2.1 lub nowszej, wywołanie `Partial` lub `RenderPartial` skutkuje to ostrzeżenie analizatora. Na przykład użycie `Partial` pojawi się następujący komunikat ostrzegawczy:

> Użyj IHtmlHelper.Partial może spowodować zakleszczenia aplikacji. Należy rozważyć użycie `<partial>` Pomocnik tagu lub `IHtmlHelper.PartialAsync`.

Zastępują wywołania `@Html.Partial` z `@await Html.PartialAsync` lub częściowe Pomocnik tagu. Aby uzyskać więcej informacji na temat migracji Pomocnik tagu częściowego, zobacz [migracja z usługi pomocnika kodu HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).

::: moniker-end

## <a name="partial-view-discovery"></a>Widok częściowy odnajdywania

Podczas odwoływania się do widoku częściowego, mogą odwoływać się do lokalizacji na kilka sposobów. Na przykład:

::: moniker range=">= aspnetcore-2.1"

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
<partial name="_ViewName" />

// A view with this name must be in the same folder
<partial name="_ViewName.cshtml" />

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
<partial name="~/Views/Folder/_ViewName.cshtml" />
<partial name="/Views/Folder/_ViewName.cshtml" />

// Locate the view using a relative path
<partial name="../Account/_LoginPartial.cshtml" />
```

W poprzednim przykładzie użyto Pomocnik tagu częściowego wymaga platformy ASP.NET Core 2.1 lub nowszej. W poniższym przykładzie użyto asynchronicznego pomocników HTML do zrealizowania tego samego zadania.

::: moniker-end

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
@await Html.PartialAsync("_ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("_ViewName.cshtml")

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
@await Html.PartialAsync("~/Views/Folder/_ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/_ViewName.cshtml")

// Locate the view using a relative path
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

Może mieć różne widoki częściowe o takiej samej nazwie pliku w folderach inny widok. Podczas odwoływania się do widoków, według nazwy (bez rozszerzenia pliku), widoki dla każdego folderu, użyj widoku częściowego w tym samym folderze, z nimi. Można również określić domyślny widok częściowy do użycia, umieszczając go w *Shared* folderu. Udostępnione widoku częściowego, jest używany przez wszystkie widoki, które nie mają swoją własną wersję widoku częściowego. Może mieć domyślny widok częściowy (w *Shared*), który jest zastępowany przez widok częściowy przy użyciu tej samej nazwie, w tym samym folderze, co w widoku nadrzędnym.

Widoki częściowe mogą być *łańcuchowa*&mdash;widoku częściowego może wywołać inny widok częściowy (o ile nie utworzy pętli). W każdym widoku lub widok częściowy ścieżki względne są zawsze względem tego widoku, a nie do katalogu głównego lub w widoku nadrzędnym.

> [!NOTE]
> A [Razor](xref:mvc/views/razor) `section` zdefiniowane w częściowym widok jest niewidoczne dla widoków elementów nadrzędnych. `section` Jest widoczne tylko dla widoku częściowego, w którym jest zdefiniowany.

## <a name="access-data-from-partial-views"></a>Dostęp do danych z widoki częściowe

Podczas tworzenia wystąpienia widoku częściowego otrzymuje kopię widoku nadrzędnego `ViewData` słownika. Aktualizacje wprowadzone do danych w widoku częściowego nie są utrwalane dla widoku nadrzędnego. `ViewData` zmiany w widoku częściowego zostaną utracone w przypadku, gdy zwraca widok częściowy.

Można przekazać wystąpienia [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) widoku częściowego:

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

Model można przekazać do widoku częściowego. Może on być modelu widoku strony lub niestandardowy obiekt. Można przekazać model do `PartialAsync` lub `RenderPartialAsync`:

```cshtml
@await Html.PartialAsync("_PartialName", viewModel)
```

Można przekazać wystąpienia `ViewDataDictionary` i model widoku częściowego widoku:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_PartialAsync)]

Ilustruje poniższy kod znaczników *Views/Articles/Read.cshtml* widoku, który zawiera dwa widoki częściowe. Drugi widoku częściowego przekazuje się w modelu i `ViewData` widoku częściowego. Użyj wyróżnionych `ViewDataDictionary` przeciążenia konstruktora, aby przekazać nowy `ViewData` słownika przy zachowaniu istniejących `ViewData` słownika.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=17-20)]

*Widoki/udostępnione/_AuthorPartial*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

*_ArticleSection* częściowej:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

W czasie wykonywania, częściowe są renderowane w widoku nadrzędnym, który sam jest renderowany w ramach udostępnionej *_Layout.cshtml*.

![dane wyjściowe widoku częściowego](partial/_static/output.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>

::: moniker-end
