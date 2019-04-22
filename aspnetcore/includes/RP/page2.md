---
ms.openlocfilehash: 45fb8a8c0d6e98b002789a6fb7b2b0fce893ad63
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705570"
---
`<form method="post">` Element jest [Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper). Pomocnik tagu formularza automatycznie uwzględnia [antiforgery token](xref:security/anti-request-forgery).

Aparat tworzenia szkieletów tworzy znaczników Razor dla każdego pola w modelu (z wyjątkiem identyfikator) podobny do następującego:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Pomocników tagów weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i `<span asp-validation-for`) wyświetla błędy sprawdzania poprawności. Sprawdzanie poprawności jest omówiona bardziej szczegółowo w dalszej części tej serii.

[Pomocnik tagu etykiet](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety i `for` atrybutu dla `Title` właściwości.

[Pomocnik tagu dane wejściowe](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) używa [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów, a następnie tworzy atrybutów HTML potrzebne dla technologii jQuery weryfikacji po stronie klienta.
