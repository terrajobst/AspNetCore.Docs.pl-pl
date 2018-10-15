---
title: Pomocnik tagu częściowego w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, ASP.NET Core częściowe Tag pomocnika i rolę każdego z jego atrybuty odtwarzania w renderowania widoku częściowego.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/25/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 737568330d2dc33868564ea541383e4ddabf8e74
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325435"
---
# <a name="partial-tag-helper-in-aspnet-core"></a>Pomocnik tagu częściowego w programie ASP.NET Core

Przez [Scott Addie](https://github.com/scottaddie)

Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="overview"></a>Omówienie

Częściowe Pomocnik tagu jest używany do renderowania [widoku częściowego](xref:mvc/views/partial) w aplikacjach stronami Razor i programem MVC. Należy wziąć pod uwagę że:

* Wymaga platformy ASP.NET Core 2.1 lub nowszej.
* Jest to alternatywa [składni pomocnika kodu HTML](xref:mvc/views/partial#reference-a-partial-view).
* Renderuje widok częściowy asynchronicznie.

Opcje pomocnika kodu HTML do renderowania widoku częściowego obejmują:

* [@await Html.PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [@await Html.RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

*Produktu* model jest używany w przykładach w tym dokumencie:

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

Spis atrybutów Pomocnik tagu częściowego poniżej.

## <a name="name"></a>nazwa

`name` Atrybut jest wymagany. Wskazuje nazwę lub ścieżkę widoku częściowego do renderowania. Gdy została podana nazwa widoku częściowego, [widok odnajdywania](xref:mvc/views/overview#view-discovery) proces jest inicjowany. Ten proces jest pomijany, gdy została podana jawna ścieżka. Dla wszystkich dopuszczalne `name` wartości, zobacz [odnajdywania widoku częściowego](xref:mvc/views/partial#partial-view-discovery).

Następujące znaczniki jest używana jawna ścieżka, co oznacza, że *_ProductPartial.cshtml* ma zostać załadowane z *Shared* folderu. Za pomocą [dla](#for) atrybut modelu są przekazywane do widoku częściowego do powiązania.

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a>dla

`for` Atrybutu przypisuje [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) ma zostać obliczone dla bieżącego modelu. A `ModelExpression` wnioskuje `@Model.` składni. Na przykład `for="Product"` mogą być używane zamiast `for="@Model.Product"`. To domyślne zachowanie wnioskowania jest zastępowany przy użyciu `@` symbol do definiowania wbudowane wyrażenia. `for` Atrybut nie może być używany z [modelu](#model) atrybutu.

Ładuje następujące znaczniki *_ProductPartial.cshtml*:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

Widok częściowy jest powiązany z modelu skojarzona strona `Product` właściwości:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a>model

`model` Atrybut przypisuje wystąpienie modelu do przekazania do widoku częściowego. `model` Atrybut nie może być używany z [dla](#for) atrybutu.

W niej następujące znaczniki nową `Product` obiektu jest tworzone i przekazywane do `model` atrybut do powiązania:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a>Wyświetlanie danych

`view-data` Atrybutu przypisuje [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) do przekazania do widoku częściowego. Następujące znaczniki sprawia, że całą kolekcję ViewData jest dostępny dla widoku częściowego:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

W poprzednim kodzie `IsNumberReadOnly` wartość klucza jest równa `true` i dodawane do kolekcji ViewData. W związku z tym `ViewData["IsNumberReadOnly"]` jest dostępne w widoku częściowego następujące:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

W tym przykładzie wartość `ViewData["IsNumberReadOnly"]` Określa, czy *numer* pole jest wyświetlane jako tylko do odczytu.

## <a name="migrate-from-an-html-helper"></a>Migracja z Pomocnika kodu HTML

Rozważmy następujący przykład pomocnika kodu HTML, asynchronicznego. Zbiór produktów postanowiliśmy i wyświetlić. Na `PartialAsync` pierwszy parametr metody *_ProductPartial.cshtml* załadowaniu widoku częściowego. Wystąpienie `Product` modelu są przekazywane do widoku częściowego do powiązania.

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_HtmlHelper&highlight=3)]

Następujące Pomocnik tagu częściowego uzyskuje takie samo zachowanie renderowania asynchronicznego jako `PartialAsync` pomocnika kodu HTML. `model` Przypisano atrybut `Product` wystąpienie modelu do powiązania do widoku częściowego.

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_TagHelper&highlight=3)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/partial>
* <xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag>
