---
title: Dodawanie walidacji do strony platformy ASP.NET Core Razor
author: rick-anderson
description: Dowiedzieć się, jak dodać weryfikacji do elementu Razor strony platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 271a5ce517ae550845d96e3969b39b1eda6ae51b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a>Dodawanie walidacji do strony platformy ASP.NET Core Razor

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji logikę weryfikacji jest dodawany do `Movie` modelu. Reguł sprawdzania poprawności są wymuszane w dowolnym momencie użytkownik tworzy lub edytuje filmu.

## <a name="validation"></a>Walidacja

Wywoływana jest główną cechą rozwoju oprogramowania [suchej](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**taj **R**epeat **Y**ourself"). Stron razor zachęca programowanie, której funkcji określono raz, a ta jest uwzględniana w całej aplikacji. SUCHEJ może pomóc zmniejszyć ilość kodu w aplikacji. SUCHEJ sprawia, że kod błędu mniej podatnych na błędy i łatwiejsze do testowania i obsługi.

Sprawdzania poprawności wsparcie ze strony Razor i Entity Framework jest dobrym przykładem suchej zasady. Reguły sprawdzania poprawności deklaratywnie są określone w jednym miejscu (w klasie modelu), a zasady są wymuszane wszędzie w aplikacji.

### <a name="adding-validation-rules-to-the-movie-model"></a>Dodawanie reguł walidacji modelu film

Otwórz *Movie.cs* pliku. [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) zawiera zestaw wbudowanych atrybuty weryfikacji, które są stosowane deklaratywnie klas lub właściwości. DataAnnotations zawiera także formatowania atrybutów, takich jak `DataType` czy pomoc w formacie i nie mają funkcje sprawdzania poprawności.

Aktualizacja `Movie` klasy, aby móc korzystać z `Required`, `StringLength`, `RegularExpression`, i `Range` atrybutów sprawdzania poprawności.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

Atrybuty weryfikacji Określ zachowanie, który jest wymuszany dla właściwości modelu:

* `Required` i `MinimumLength` atrybuty oznacza, że właściwość musi mieć wartość. Jednak nie uniemożliwia użytkownikowi wprowadzanie odstępów, aby spełniać ograniczenie sprawdzania poprawności dla typu dopuszczającego wartość null. Niedopuszczająca wartości null [typów wartości](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (takich jak `decimal`, `int`, `float`, i `DateTime`) są z założenia wymagane i nie wymagają `Required` atrybutu.
* `RegularExpression` Atrybut ogranicza znaki, które użytkownik może wprowadzić. W powyższym kodzie `Genre` i `Rating` należy używać tylko liter (odstępów, cyfr i znaków specjalnych nie są dozwolone).
* `Range` Atrybut ogranicza wartości do określonego zakresu.
* `StringLength` Atrybut Ustawia maksymalną długość ciągu i opcjonalnie, minimalną długość. 

Posiadanie reguły sprawdzania poprawności, automatycznie są wymuszane przez platformy ASP.NET Core podnosi poziom aplikacji bardziej niezawodne. Automatyczne sprawdzanie poprawności modeli pomaga w ochronie aplikacji, ponieważ nie trzeba pamiętać o ich zastosowania w przypadku dodania nowego kodu.

### <a name="validation-error-ui-in-razor-pages"></a>Błąd sprawdzania poprawności interfejsu użytkownika na stronach Razor

Uruchom aplikację i przejdź do strony/filmów.

Wybierz **Utwórz nowy** łącza. Wypełnij formularz z niektórych z nieprawidłowymi wartościami. Podczas weryfikacji po stronie klienta jQuery wykryje błąd, wyświetla komunikat o błędzie.

![Formularz widoku film z wiele błędów weryfikacji po stronie klienta jQuery](validation/_static/val.png)

> [!NOTE]
> Nie można wprowadzić kropki i przecinki w `Price` pola. Do obsługi [weryfikacji jQuery](https://jqueryvalidation.org/) w innych niż angielski, które użyj przecinka (",") dla punktu dziesiętnego i formaty daty z systemem innym niż angielski, należy wykonać kroki, aby globalize aplikacji. Zobacz [dodatkowe zasoby](#additional-resources) Aby uzyskać więcej informacji. Teraz wprowadź tylko liczby całkowite, takie jak 10.

Zwróć uwagę, jak formularz automatycznie renderowany komunikat o błędzie weryfikacji w każdym polu zawierający nieprawidłową wartość. Błędy są wymuszane zarówno po stronie klienta (przy użyciu języka JavaScript i jQuery) i po stronie serwera (Jeśli użytkownik ma JavaScript wyłączone).

Jest to znaczące korzyści, że **nie** zmiany kodu zostały niezbędne na stronach Tworzenie lub edytowanie. Po DataAnnotations zostały zastosowane do modelu, została włączona weryfikacja interfejsu użytkownika. Stron Razor utworzonej w tym samouczku automatycznie pobierane reguł sprawdzania poprawności (przy użyciu atrybutów weryfikacji właściwości `Movie` klasa modelu). Weryfikacja testów za pomocą edycji strony tego samego sprawdzania poprawności jest stosowany.

Dane formularza nie jest opublikowana do serwera, dopóki nie ma żadnych błędów weryfikacji po stronie klienta. Sprawdź formularza, do którego dane nie jest przesyłane przez jedną lub więcej z następujących metod:

* Umieść punkt przerwania w `OnPostAsync` metody. Walidacja (wybierz **Utwórz** lub **zapisać**). Nigdy nie zostaje trafiony punkt przerwania.
* Użyj [narzędzie Fiddler](http://www.telerik.com/fiddler).
* Narzędzia developer przeglądarki do monitorowania ruchu sieciowego.

### <a name="server-side-validation"></a>Weryfikacja po stronie serwera

Po wyłączeniu JavaScript w przeglądarce przesyłania formularza z błędami zostanie wysłany do serwera.

Opcjonalne, testów weryfikacji po stronie serwera:

* Wyłącz JavaScript w przeglądarce. Jeśli nie można wyłączyć JavaScript w przeglądarce, spróbuj użyć innej przeglądarki.
* Ustaw punkt przerwania `OnPostAsync` metody tworzenia lub edycji strony.
* Prześlij formularz z błędami sprawdzania poprawności.
* Sprawdź, czy stan modelu jest nieprawidłowy:

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

Poniższy kod przedstawia część *Create.cshtml* strona, która szkieletu wcześniej w samouczku. Jest on używany przez tworzenie i edytowanie strony do wyświetlania formularza początkowego i ponownie wyświetlić formularza w przypadku wystąpienia błędu.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

[Pomocnika Tag danych wejściowych](xref:mvc/views/working-with-forms) używa [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów i tworzy wymagane dla technologii jQuery weryfikacji po stronie klienta atrybutów HTML. [Pomocnika tagów weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) przedstawia błędy sprawdzania poprawności. Zobacz [weryfikacji](xref:mvc/models/validation) Aby uzyskać więcej informacji.

Tworzenie i edytowanie strony mają reguł sprawdzania poprawności w nich. Reguły sprawdzania poprawności i ciągi błąd są określane tylko w `Movie` klasy. Te reguły sprawdzania poprawności są automatycznie stosowane do stron Razor, które edytować `Movie` modelu.

Gdy logikę weryfikacji musi zmienić, odbywa się tylko w modelu. Sprawdzanie poprawności jest stosowane w sposób spójny w całej aplikacji (logika sprawdzania poprawności jest zdefiniowany w jednym miejscu). Sprawdzanie poprawności w jednym miejscu pomaga zabezpieczyć kod czysty i ułatwia utrzymanie i aktualizowanie.

## <a name="using-datatype-attributes"></a>Przy użyciu atrybutów typu danych

Sprawdź `Movie` klasy. `System.ComponentModel.DataAnnotations` Przestrzeń nazw zawiera atrybuty formatowania oprócz wbudowanych zestaw atrybutów weryfikacji. `DataType` Atrybut jest stosowany do `ReleaseDate` i `Price` właściwości.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` Atrybuty zapewniają tylko wskazówki dotyczące aparatu widoku do formatowania danych (i dostarcza atrybutów, takich jak `<a>` dla adresu URL i `<a href="mailto:EmailAddress.com">` do obsługi poczty e-mail). Użyj `RegularExpression` atrybut do zweryfikowania formatu danych. `DataType` Atrybut służy do określania typu danych, który jest bardziej szczegółowy niż typ wewnętrznej bazy danych. `DataType` atrybuty nie są atrybutów sprawdzania poprawności. W przykładowej aplikacji wyświetlane jest tylko data, bez czasu.

`DataType` Wyliczenie zawiera wiele typów danych, takich jak daty, godziny, numer telefonu, waluty, EmailAddress i więcej. `DataType` Atrybut można również włączyć aplikacji w celu umożliwienia automatycznie funkcji specyficznych dla typu. Na przykład `mailto:` można tworzyć łącza `DataType.EmailAddress`. Można podać selektora daty `DataType.Date` w przeglądarkach obsługujących HTML5. `DataType` Atrybuty emituje HTML 5 `data-` atrybutów (dash wyraźnym danych), które korzystać z przeglądarki HTML 5. `DataType` Czy atrybuty **nie** Podaj wszystkich sprawdzania poprawności.

`DataType.Date` nie określono format daty, która jest wyświetlana. Domyślnie pole danych są wyświetlane domyślne formaty oparte na tym serwerze `CultureInfo`.

`DisplayFormat` Atrybut służy do jawnie określić format daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinna być stosowana, gdy wartość jest wyświetlana do edycji. Nie można tego zachowania w przypadku niektórych pól. Na przykład w wartości waluty, prawdopodobnie nie ma symbol waluty w edycji interfejsu użytkownika.

`DisplayFormat` Atrybut może być używany przez samego siebie, ale jest zwykle warto użyć `DataType` atrybutu. `DataType` Atrybut przekazuje semantykę danych zamiast sposób renderowania jej na ekranie i zapewnia następujące korzyści, które nie można uzyskać z DisplayFormat:

* Przeglądarki, można włączyć funkcje HTML5 (na przykład pokazać formant kalendarza, symbol waluty odpowiednie ustawienia regionalne, przesyłanie pocztą e-mail łączy, itp.)
* Domyślnie przeglądarka wyświetli danych przy użyciu właściwego formatu oparte na ustawienia regionalne.
* `DataType` Atrybut można włączyć platformę ASP.NET Core, aby wybrać szablon pola prawo do renderowania danych. `DisplayFormat` Jeśli używany przez samego używa szablonu ciągu.

Uwaga: weryfikacji jQuery nie działa z `Range` atrybutu i `DateTime`. Na przykład następujący kod zawsze spowoduje wyświetlenie błędu weryfikacji po stronie klienta, nawet wtedy, gdy data jest w określonym zakresie:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Zazwyczaj nie jest dobrym rozwiązaniem do skompilowania w modelach przy użyciu twardych daty `Range` atrybutu i `DateTime` jest niezalecane.

Poniższy kod przedstawia łączenie atrybutów w jednym wierszu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

[Wprowadzenie do stron Razor i podstawowe EF](xref:data/ef-rp/intro) przedstawiono bardziej zaawansowane EF podstawowych operacji ze stronami Razor.

### <a name="publish-to-azure"></a>Publikowanie na platformie Azure

Zobacz [publikowania aplikacji sieci web platformy ASP.NET Core w usłudze Azure App Service przy użyciu programu Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) instrukcje dotyczące sposobu publikowania tej aplikacji na platformie Azure.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Praca z formularzami](xref:mvc/views/working-with-forms)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)
* [Wprowadzenie do pomocników tagów](xref:mvc/views/tag-helpers/intro)
* [Autor pomocników tagów](xref:mvc/views/tag-helpers/authoring)

> [!div class="step-by-step"]
> [Poprzedni: Dodanie nowego pola](xref:tutorials/razor-pages/new-field)
> [dalej: przekazywanie plików](xref:tutorials/razor-pages/uploading-files)
