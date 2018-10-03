---
title: Dodawanie walidacji do strony ASP.NET Core Razor
author: rick-anderson
description: Dowiedz się, jak dodać sprawdzanie poprawności na stronę Razor programu ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/validation
ms.openlocfilehash: cd958b9c084de4b3e12784774544610873a519f9
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045526"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a>Dodawanie walidacji do strony ASP.NET Core Razor

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji logikę weryfikacji jest dodawany do `Movie` modelu. Reguły sprawdzania poprawności są wymuszane w dowolnym momencie użytkownik tworzy lub edytowania filmu.

## <a name="validation"></a>Walidacja

Nosi nazwę główną cechą Wytwarzanie oprogramowania [susz](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D** **R**epeat **Y**ourself"). Strony razor zachęca do którego funkcje określono raz i znajduje odzwierciedlenie w całej aplikacji. PRÓBNEGO może zmniejszyć ilość kodu w aplikacji. PRÓBNEGO sprawia, że kod błędu mniej podatne i łatwiejsze do testowania i obsługi.

Sprawdzanie poprawności wsparcie ze stronami Razor i programem Entity Framework jest dobrym przykładem susz zasady. Reguły sprawdzania poprawności deklaratywne są określone w jednym miejscu (w klasie modelu), a zasady są wymuszane wszędzie, gdzie w aplikacji.

### <a name="adding-validation-rules-to-the-movie-model"></a>Dodawania reguł sprawdzania poprawności do modelu movie

Otwórz *Models/Movie.cs* pliku. [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) zawiera zestaw wbudowanych atrybutów sprawdzania poprawności, które są stosowane w sposób deklaratywny do klasa lub właściwość. DataAnnotations zawiera też atrybuty formatowania, takich jak `DataType` ułatwić formatowanie i nie zapewniają weryfikacji.

Aktualizacja `Movie` klasy, aby korzystać z zalet `Required`, `StringLength`, `RegularExpression`, i `Range` atrybutów sprawdzania poprawności.

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

Atrybuty weryfikacji określić zachowanie, który jest wymuszany dla właściwości modelu:

* `Required` i `MinimumLength` atrybuty wskazują, że właściwość musi mieć wartość. Jednakże nic nie uniemożliwia użytkownikowi wprowadzanie odstępów, aby spełniać ograniczenie sprawdzania poprawności dla typu dopuszczającego wartość null. Nieprzyjmujące [typy wartości](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (takie jak `decimal`, `int`, `float`, i `DateTime`) są założenia wymagane i nie ma potrzeby `Required` atrybutu.
* `RegularExpression` Atrybut ogranicza znaki, które użytkownik może wprowadzić. W poprzednim kodzie `Genre` musi zaczynać się wielkie litery i postępuj zgodnie z zero lub więcej liter, pojedynczym lub podwójnym cudzysłowie, białych znaków lub kreski. `Rating` musi rozpoczynać się wielkie litery, a następnie postępuj zgodnie z zero lub więcej liter, cyfr, pojedynczym lub podwójnym cudzysłowie, białych znaków lub łączniki.
* `Range` Atrybut ogranicza wartości do określonego zakresu.
* `StringLength` Atrybut Ustawia maksymalną długość ciągu i opcjonalnie minimalnej długości. 

Posiadanie reguły sprawdzania poprawności, które automatycznie wymuszanych przez platformy ASP.NET Core ułatwia tworzenie aplikacji bardziej niezawodne. Automatyczna Walidacja modeli pomaga w ochronie aplikacji, ponieważ nie masz Pamiętaj, aby zastosować je, gdy zostanie dodany nowy kod.

### <a name="validation-error-ui-in-razor-pages"></a>Błąd sprawdzania poprawności interfejsu użytkownika w stron Razor

Uruchom aplikację i przejdź do strony/filmów.

Wybierz **Utwórz nowy** łącza. Wypełnij formularz z niektórych z nieprawidłowymi wartościami. Podczas weryfikacji po stronie klienta jQuery wykryje błąd, wyświetla komunikat o błędzie.

![Film wyświetlanie formularza za pomocą wielu błędów weryfikacji po stronie klienta jQuery](validation/_static/val.png)

> [!NOTE]
> Nie można wprowadzić, kropki i przecinki w `Price` pola. Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) w innych niż angielski, które należy użyć przecinka (",") dla punktu dziesiętnego i formaty daty inne niż angielski, należy wykonać kroki, aby sprzedawać aplikację. Zobacz [dodatkowe zasoby](#additional-resources) Aby uzyskać więcej informacji. Teraz po prostu wprowadź liczbami całkowitymi, takich jak 10.

Zwróć uwagę, jak formularz automatycznie renderowany komunikat o błędzie weryfikacji w każdym polu zawierający nieprawidłową wartość. Błędy są wymuszane, zarówno po stronie klienta (przy użyciu języków JavaScript i jQuery) i po stronie serwera (Jeśli użytkownik ma Obsługa skryptów JavaScript wyłączona).

Jest to znaczące korzyści, że **nie** zmian w kodzie były konieczne na stronach Utwórz lub zmodyfikuj. Po DataAnnotations zastosowano do modelu, została włączona weryfikacja interfejsu użytkownika. Strony Razor utworzonych w tym samouczku automatycznie pobierane reguł sprawdzania poprawności (przy użyciu atrybutów weryfikacji właściwości `Movie` klasa modelu). Walidacja testu przy użyciu strony edytowania tego samego sprawdzania poprawności jest stosowana.

Dane formularza nie jest opublikowana na serwerze, aż nie wystąpią żadne błędy weryfikacji po stronie klienta. Sprawdź formularza, którego dane nie są opublikowane przez co najmniej jeden z następujących metod:

* Umieść punkt przerwania w `OnPostAsync` metody. Walidacja (wybierz **Utwórz** lub **Zapisz**). Nigdy nie zostanie osiągnięty punkt przerwania.
* Użyj [narzędzie Fiddler](http://www.telerik.com/fiddler).
* Narzędzia dla deweloperów przeglądarki do monitorowania ruchu sieciowego.

### <a name="server-side-validation"></a>Weryfikacja po stronie serwera

Po wyłączeniu JavaScript w przeglądarce przesyłania formularza z błędami opublikuje do serwera.

Opcjonalne, Walidacja po stronie serwera testu:

* Wyłącz JavaScript w przeglądarce. Można to zrobić za pomocą narzędzi dla deweloperów w przeglądarce. Jeśli nie można wyłączyć języka JavaScript w przeglądarce, spróbuj innej przeglądarki.
* Ustaw punkt przerwania w `OnPostAsync` metody tworzenia lub edycji strony.
* Prześlij formularz z błędami sprawdzania poprawności.
* Sprawdź, czy stan modelu jest nieprawidłowy:

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

Poniższy kod ilustruje część *Create.cshtml* strona, która szkieletu we wcześniejszej części tego samouczka. Jest używany przez tworzenie i edytowanie strony początkowej formularz wyświetlania i wyświetl ponownie formularz w przypadku wystąpienia błędu.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

[Pomocnik tagu dane wejściowe](xref:mvc/views/working-with-forms) używa [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów, a następnie tworzy atrybutów HTML potrzebne dla technologii jQuery weryfikacji po stronie klienta. [Pomocnik tagu weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) wyświetla błędy sprawdzania poprawności. Zobacz [weryfikacji](xref:mvc/models/validation) Aby uzyskać więcej informacji.

Tworzenie i edytowanie strony mają reguł sprawdzania poprawności w nich. Reguł sprawdzania poprawności i ciągi błędów są określane tylko w `Movie` klasy. Te reguły sprawdzania poprawności są automatycznie stosowane do stron Razor, którą edytować `Movie` modelu.

Gdy logikę weryfikacji wymaga wprowadzenia zmian, odbywa się tylko w modelu. Sprawdzania poprawności jest stosowane spójnie w całej aplikacji (logika sprawdzania poprawności jest zdefiniowany w jednym miejscu). Sprawdzanie poprawności w jednym miejscu pomaga zachować czyste kodu i ułatwia utrzymanie i aktualizowanie.

## <a name="using-datatype-attributes"></a>Przy użyciu atrybutów typu danych

Sprawdź `Movie` klasy. `System.ComponentModel.DataAnnotations` Przestrzeń nazw zawiera atrybuty formatowania, oprócz wbudowanych zestaw atrybutów weryfikacji. `DataType` Atrybut jest stosowany do `ReleaseDate` i `Price` właściwości.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` Atrybuty zawierają tylko wskazówki dotyczące aparatu widoku do formatowania danych (i dostarcza atrybutów, takich jak `<a>` dla adresu URL i `<a href="mailto:EmailAddress.com">` do obsługi poczty e-mail). Użyj `RegularExpression` atrybutu, aby sprawdzić poprawność formatu danych. `DataType` Atrybut jest używany do określenia typu danych, który jest bardziej szczegółowe niż typ wewnętrznej bazy danych. `DataType` atrybuty nie są atrybutów sprawdzania poprawności. W przykładowej aplikacji tylko data jest wyświetlana bez godziny.

`DataType` Wyliczenie udostępnia wiele typów danych, takich jak daty, godziny, numer telefonu, waluty, EmailAddress i więcej. `DataType` Atrybut można również włączyć automatyczne udostępnianie funkcji specyficznych dla typu aplikacji. Na przykład `mailto:` łącza mogą być tworzone dla `DataType.EmailAddress`. Selektor daty można określić dla `DataType.Date` w przeglądarkach obsługujących HTML5. `DataType` Atrybuty emituje HTML 5 `data-` atrybutów (Wymowa: dane dash), korzystających z przeglądarki HTML 5. `DataType` Atrybuty czy **nie** Podaj wszelkie sprawdzania poprawności.

`DataType.Date` nie określa format daty, która jest wyświetlana. Domyślnie pole danych są wyświetlane domyślne formaty oparte na tym serwerze `CultureInfo`.

::: moniker range=">= aspnetcore-2.1"

`[Column(TypeName = "decimal(18, 2)")]` Wymagana jest adnotacja danych, dzięki czemu można poprawnie mapowane na platformy Entity Framework Core `Price` walutę w bazie danych. Aby uzyskać więcej informacji, zobacz [typy danych](/ef/core/modeling/relational/data-types).

::: moniker-end

`DisplayFormat` Atrybut jest używany jawnie określić format daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinna być stosowana, gdy wartość jest wyświetlana do edycji. Nie można tego zachowania w przypadku niektórych pól. Na przykład w wartości waluty, prawdopodobnie nie chcesz, symbol waluty w trakcie edycji interfejsu użytkownika.

`DisplayFormat` Atrybut może być używany przez siebie, ale zazwyczaj jest to dobry pomysł, aby użyć `DataType` atrybutu. `DataType` Atrybut umożliwia przekazywanie semantykę dane, a nie jak renderować ją na ekranie i zapewnia następujące korzyści, które nie można uzyskać za pomocą DisplayFormat:

* Przeglądarka można włączyć funkcje HTML5 (na przykład pokazać kontrolki kalendarza, symbol waluty odpowiednich ustawień regionalnych, przesyłanie pocztą e-mail łączy, itp.)
* Domyślnie przeglądarka wyświetli dane przy użyciu poprawny format, w oparciu o ustawienia regionalne.
* `DataType` Atrybut można włączyć platformę ASP.NET Core, aby wybrać szablon po prawej stronie pola w celu przedstawienia tych danych. `DisplayFormat` Jeśli używany przez samego korzysta z szablonu ciągu.

Uwaga: dotyczącą weryfikacji jQuery nie działa w przypadku `Range` atrybutu i `DateTime`. Na przykład poniższy kod zawsze wyświetli błąd sprawdzania poprawności po stronie klienta, nawet wtedy, gdy jest to data mieści się w określonym zakresie:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Ogólnie nie jest dobrą praktyką jest kompilowanie twardych dat w ramach modeli za pomocą `Range` atrybutu i `DateTime` jest niezalecane.

Poniższy kod pokazuje atrybuty łączenie w jednym wierszu:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

[Rozpoczynanie pracy ze stronami Razor i programem EF Core](xref:data/ef-rp/intro) pokazuje zaawansowane operacji programu EF Core przy użyciu stron Razor.

### <a name="publish-to-azure"></a>Publikowanie na platformie Azure

Aby uzyskać informacje na temat wdrażania na platformie Azure, zobacz [samouczek: tworzenie aplikacji ASP.NET na platformie Azure przy użyciu bazy danych SQL](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase). Instrukcje są przeznaczone dla aplikacji platformy ASP.NET, nie aplikacji ASP.NET Core, ale kroki są takie same.

Dziękujemy za wypełnienie tego wprowadzenia do stron Razor. Dziękujemy za opinię. [Rozpoczynanie pracy ze stronami Razor i programem EF Core](xref:data/ef-rp/intro) jest doskonałym uzupełnianie w tym samouczku.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Praca z formularzami](xref:mvc/views/working-with-forms)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)
* [Wprowadzenie do pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
* [Tworzenie pomocników tagów](xref:mvc/views/tag-helpers/authoring)

> [!div class="step-by-step"]
> [Poprzedni: Dodanie nowego pola](xref:tutorials/razor-pages/new-field)
