---
title: "Widoki częściowe w platformy ASP.NET Core"
author: ardalis
description: "Dowiedz się, jak jest widok częściowy widoku, który jest umieszczony wewnątrz innego widoku i kiedy powinno się używać w aplikacji platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 03/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/partial
ms.openlocfilehash: abe970b02a62ef58deb259241d7451de0185575c
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="partial-views-in-aspnet-core"></a>Widoki częściowe w platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), i [Rick Anderson](https://twitter.com/RickAndMSFT)

Podstawowe ASP.NET MVC obsługuje widoki częściowe, które są przydatne w przypadku wielokrotnego użytku części stron sieci web, którą chcesz udostępnić między różne widoki.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Co to są widoki częściowe?

Widok częściowy jest widok, w którym jest umieszczony wewnątrz innego widoku. Generowane, wykonując widoku częściowego danych wyjściowych HTML jest renderowany do widoku telefonicznej (lub nadrzędnej). Jak widoki, widoki częściowe użyj *.cshtml* rozszerzenia pliku.

## <a name="when-should-i-use-partial-views"></a>Kiedy należy używać widoki częściowe?

Widoki częściowe są efektywnym sposobem rozdzielanie dużych widoków na mniejsze części. Mogą zmniejszyć duplikatów zawartości widoku i Zezwalaj na wyświetlanie elementów zostanie ponownie. Typowe elementy układu powinny być określone w [_Layout.cshtml](layout.md). Zawartość do ponownego użycia inne niż układu można hermetyzowany w widoki częściowe.

Jeśli masz stronę złożone składają się z wielu części logiczne, może być przydatne do pracy z każdego z nich jako własnego widoku częściowego. Każda część strony można wyświetlić w izolacji od reszty strony, a widok, w którym sama strona jest znacznie prostsza, ponieważ zawiera ona tylko ogólną strukturę strony i wywołania do renderowania widoków częściowych.

Porada: Wykonaj [nie powtarzaj samodzielnie zasady](http://deviq.com/don-t-repeat-yourself/) w widoków.

## <a name="declaring-partial-views"></a>Deklarowanie widoki częściowe

Widoki częściowe są tworzone, podobnie jak inne widoki: tworzenia *.cshtml* pliku w ramach *widoków* folderu. Nie ma żadnej semantycznego różnicy między widok częściowy i widokiem regularnych — jest po prostu renderowane inaczej. Masz widoku, który jest zwracany bezpośrednio z poziomu kontrolera `ViewResult`, i tego samego widoku mogą być używane jako widok częściowy. Główną różnicą między sposobu renderowania widoku oraz widoku częściowego jest, że widoki częściowe, nie uruchamiaj *_ViewStart.cshtml* (gdy widoków Dowiedz się więcej o *_ViewStart.cshtml* w [układu ](layout.md)).

## <a name="referencing-a-partial-view"></a>Odwołuje się do widoku częściowego

Na stronie widoku istnieją z kilka sposobów, w których można renderowania widoku częściowego. Najprostszą jest użycie `Html.Partial`, która zwraca `IHtmlString` i może odwoływać się prefiksu to wywołanie `@`:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

`PartialAsync` Metoda jest dostępna dla częściowego widoków zawierających kod asynchroniczne (chociaż zazwyczaj nie zaleca się kodu w widokach):

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

Można renderowania widoku częściowego z `RenderPartial`. Ta metoda nie zwraca wyniku; jego strumieni przetworzonych wyników bezpośrednio do odpowiedzi. Ponieważ nie zwraca wyników, musi być wywoływana w bloku kodu Razor (można również wywołać `RenderPartialAsync` w razie potrzeby):

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

Ponieważ strumieni wynik bezpośrednio, `RenderPartial` i `RenderPartialAsync` może działać lepiej w niektórych scenariuszach. Jednak w większości przypadków zaleca możesz użyć `Partial` i `PartialAsync`.

> [!NOTE]
> Jeśli widoków konieczne jest wykonanie kodu, wzorzec zalecane jest użycie [składnika widoku](view-components.md) zamiast widoku częściowego.

### <a name="partial-view-discovery"></a>Widok częściowy odnajdywania

Podczas odwoływania się do widoku częściowego, mogą odwoływać się do lokalizacji na kilka sposobów:

```cshtml
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@Html.Partial("ViewName")

// A view with this name must be in the same folder
@Html.Partial("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@Html.Partial("~/Views/Folder/ViewName.cshtml")
@Html.Partial("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@Html.Partial("../Account/LoginPartial.cshtml")
```

W folderach inny widok, może mieć różne widoki częściowe o takiej samej nazwie. Podczas odwoływania się do widoków według nazwy (bez rozszerzenia), widoki w każdym folderze użyje widoku częściowego w tym samym folderze z nimi. Można także określić domyślny widok częściowy do użycia, umieszczając go w *Shared* folderu. Udostępniony widoku częściowego będzie używany przez wszystkie widoki, które nie mają własnych wersji widoku częściowego. Może mieć domyślny widok częściowy (w *Shared*), która zostanie zastąpiona przez widok częściowy o takiej samej nazwie, w tym samym folderze co widoku nadrzędnego.

Widoki częściowe mogą być *łańcuchowej*. Oznacza to widok częściowy można wywołać inny widok częściowy (o ile nie zostanie utworzona pętli). W każdym widoku lub widok częściowy ścieżki względne są zawsze względem tego widoku, nie głównego lub nadrzędnej widoku.

> [!NOTE]
> W przypadku [Razor](razor.md) `section` w widoku częściowego, nie będzie widoczny dla jego ją klas nadrzędnych; będzie ograniczony do widoku częściowego.

## <a name="accessing-data-from-partial-views"></a>Uzyskiwanie dostępu do danych z częściowa widoków

W przypadku wystąpienia widoku częściowego, pobiera on kopię widoku nadrzędnego `ViewData` słownika. Aktualizacje wprowadzone do danych w widoku częściowego nie są zachowywane dla widoku nadrzędnego. `ViewData` zmieniony w częściowym widoku zostaną utracone po powrocie z widoku częściowego.

Można przekazać wystąpienia `ViewDataDictionary` widoku częściowego:

```cshtml
@Html.Partial("PartialName", customViewData)
```

Można również przekazać model do widoku częściowego. Może to być modelu widoku strony, lub niektórych części lub niestandardowy obiekt. Można przekazać przy użyciu modelu `Partial`,`PartialAsync`, `RenderPartial`, lub `RenderPartialAsync`:

```cshtml
@Html.Partial("PartialName", viewModel)
```

Można przekazać wystąpienia `ViewDataDictionary` i widok częściowy przy użyciu modelu widoku:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

Kod znaczników, poniżej przedstawiono *Views/Articles/Read.cshtml* widoku, który zawiera dwa widoki częściowe. Przekazuje drugiego widoku częściowego w modelu i `ViewData` do widoku częściowego. Można przekazać nowy `ViewData` słownika przy zachowaniu istniejących `ViewData` użycie przeładowania konstruktora z `ViewDataDictionary` wyróżniono poniżej:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Widoki/udostępnione/AuthorPartial*:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

*ArticleSection* częściowe:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

W czasie wykonywania, częściowe są renderowane w widoku nadrzędnym, które jest renderowany w ramach udostępnionego *_Layout.cshtml*

![dane wyjściowe widok częściowy](partial/_static/output.png)
