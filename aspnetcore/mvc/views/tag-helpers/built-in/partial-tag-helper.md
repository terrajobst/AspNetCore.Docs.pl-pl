---
title: "Pomocnik Tag częściowe"
author: scottaddie
description: "Dostęp do platformy ASP.NET Core częściowe Tag pomocnika i roli każdego z jego atrybuty odtwarzana renderowania widoku częściowego."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: e5c71ccb7a355ffe1c24f389ab490d614d333589
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="partial-tag-helper"></a>Pomocnik Tag częściowe

Przez [Scott Addie](https://github.com/scottaddie)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="overview"></a>Omówienie

Częściowe pomocnika Tag jest używany do renderowania [widoku częściowego](xref:mvc/views/partial) w aplikacjach stron Razor i MVC. Należy wziąć pod uwagę że:

* Wymaga platformy ASP.NET Core 2.1 lub nowszej.
* Jest to alternatywa dla [składni pomocnika kodu HTML](xref:mvc/views/partial#referencing-a-partial-view).

Aby recap, dostępne są następujące opcje pomocnika kodu HTML do renderowania widoku częściowego:

* [@await Html.PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [@await Html.RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

*Produktu* model jest używany w przykłady w tym dokumencie:

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

Spis atrybutów częściowe pomocnika tagów jest zgodna.

## <a name="name"></a>nazwa

`name` Atrybut jest wymagany. Wskazuje on, nazwa lub ścieżka widoku częściowego do renderowania. Jeśli podano nazwę widoku częściowego, [widok odnajdywania](xref:mvc/views/overview#view-discovery) proces jest inicjowany. Ten proces jest pomijane, gdy została podana ścieżka jawna.

Następujący kod używa jawne ścieżki, co oznacza, że *_ProductPartial.cshtml* ma zostać załadowane z *Shared* folderu. Przy użyciu [asp — dla](#asp-for) atrybut modelu są przekazywane do widoku częściowego dla powiązania.

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="asp-for"></a>ASP — dla

`asp-for` Atrybutu przypisuje [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) ma zostać obliczone dla bieżącego modelu. A `ModelExpression` wnioskuje `@Model.` składni. Na przykład `asp-for="Product"` można użyć zamiast `asp-for="@Model.Product"`. To domyślne zachowanie wnioskowania zostanie zastąpiona przy użyciu `@` symbolu, aby zdefiniować wyrażenie wbudowanego.

Następujący kod znaczników ładuje *_ProductPartial.cshtml*:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_AspFor)]

Widok częściowy jest powiązana z modelem skojarzonej strony `Product` właściwości:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="view-data"></a>Wyświetlanie danych

`view-data` Atrybutu przypisuje [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) do przekazania do widoku częściowego. Następujący kod znaczników sprawia, że całą kolekcję ViewData dostępne do widoku częściowego:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

W powyższym kodzie `IsNumberReadOnly` ma ustawioną wartość klucza `true` i dodać do kolekcji ViewData. W rezultacie `ViewData["IsNumberReadOnly"]` jest udostępniane w widoku częściowego następujące:

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

W tym przykładzie wartość `ViewData["IsNumberReadOnly"]` Określa, czy *numer* pole jest wyświetlane jako tylko do odczytu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Widoki częściowe](xref:mvc/views/partial)
* [Słabą danych (ViewData i obiekt ViewBag)](xref:mvc/views/overview#weakly-typed-data-viewdata-and-viewbag)
