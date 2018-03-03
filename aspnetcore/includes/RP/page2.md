`<form method="post">` Jest element [pomocnika Tag formularza](xref:mvc/views/working-with-forms#the-form-tag-helper). Pomocnik Tag formularza automatycznie uwzględnia [antiforgery token](xref:security/anti-request-forgery).

Aparat szkieletów tworzy znaczników Razor dla każdego pola w modelu (z wyjątkiem Identyfikatora) podobny do następującego:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Pomocników tagów weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` i ` <span asp-validation-for`) wyświetla błędy sprawdzania poprawności. Sprawdzanie poprawności zostało opisane bardziej szczegółowo w dalszej części tej serii.

[Pomocnika tagów etykiety](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generuje podpis etykiety i `for` atrybutu dla `Title` właściwości.

[Pomocnika Tag danych wejściowych](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) używa [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów i tworzy wymagane dla technologii jQuery weryfikacji po stronie klienta atrybutów HTML.