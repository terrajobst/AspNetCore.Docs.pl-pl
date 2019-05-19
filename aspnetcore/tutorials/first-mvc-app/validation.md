---
title: Dodawanie walidacji do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Jak dodać sprawdzanie poprawności do aplikacji ASP.NET Core.
ms.author: riande
ms.date: 04/13/2017
uid: tutorials/first-mvc-app/validation
ms.openlocfilehash: 6c59d0188f67872c7dd5599967551d7d390bfdcf
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/19/2019
ms.locfileid: "65875034"
---
# <a name="add-validation-to-an-aspnet-core-mvc-app"></a>Dodawanie walidacji do aplikacji ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji:

* Logika sprawdzania poprawności jest dodawany do `Movie` modelu.
* Upewnij się, że reguły sprawdzania poprawności są wymuszane ilekroć użytkownik tworzy lub edytowania filmu.

## <a name="keeping-things-dry"></a>Utrzymywanie rzeczy PRÓBNEGO

Jednym z założenia projektowania MVC jest [susz](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("nie należy powtórzyć samodzielnie"). Platforma ASP.NET Core MVC zachęca można określić funkcji lub zachowanie tylko raz, a następnie go wszędzie, gdzie odzwierciedlone w aplikacji. Zmniejsza ilość kodu, który trzeba było pisać i sprawia, że kod, który pisanie mniej błędów, podatne, łatwiejsze testowanie i łatwiejsze w utrzymaniu.

Obsługa sprawdzania poprawności, dostarczone przez MVC i Entity Framework Core Code First jest dobrym przykładem susz zasady w akcji. Można deklaratywne określenie reguł sprawdzania poprawności w jednym miejscu (w klasie modelu), a zasady są wymuszane wszędzie, gdzie w aplikacji.

[!INCLUDE[](~/includes/RP-MVC/validation.md)]

## <a name="validation-error-ui"></a>Błąd sprawdzania poprawności UI

Uruchom aplikację i przejdź do kontrolera filmów.

Naciśnij pozycję **Utwórz nowy** łącze, aby dodać nowy film. Wypełnij formularz z niektórych z nieprawidłowymi wartościami. Jak najszybciej po weryfikacji po stronie klienta jQuery wykryje błąd, wyświetla komunikat o błędzie.

![Film wyświetlanie formularza za pomocą wielu błędy weryfikacji po stronie klienta jQuery](~/tutorials/first-mvc-app/validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

Zwróć uwagę, jak formularz automatycznie renderowany komunikat o błędzie weryfikacji odpowiednie w każdym polu zawierający nieprawidłową wartość. Błędy są wymuszane, zarówno po stronie klienta (przy użyciu języków JavaScript i jQuery) i po stronie serwera (w przypadku, gdy użytkownik ma Obsługa skryptów JavaScript wyłączona).

Znaczące korzyści jest, że nie trzeba zmieniać jednego wiersza kodu w `MoviesController` klasy lub *Create.cshtml* widoku w celu włączenia tej weryfikacji interfejsu użytkownika. Kontrolera i widoki utworzone wcześniej w tym samouczku automatycznie wybrany w górę sprawdzania poprawności reguły określona za pomocą atrybutów weryfikacji właściwości `Movie` klasa modelu. Walidacja testu za pomocą `Edit` metody akcji i tego samego sprawdzania poprawności jest stosowana.

Dane formularza nie jest wysyłana do serwera, aż nie wystąpią żadne błędy weryfikacji po stronie klienta. Można to sprawdzić przez umieszczenie punkt przerwania w `HTTP Post` metody, używając [narzędzie Fiddler](http://www.telerik.com/fiddler) , lub [narzędzi deweloperskich F12](/microsoft-edge/devtools-guide).

## <a name="how-validation-works"></a>Działanie sprawdzania poprawności

Być może zastanawiasz się, jak sprawdzanie poprawności UI został wygenerowany bez wykonywania żadnych aktualizacji do kodu w kontrolerze lub widoków. W poniższym kodzie pokazano dwa `Create` metody.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

Pierwszy (HTTP GET) `Create` metody akcji Wyświetla początkowej formularza tworzenia. Drugi (`[HttpPost]`) wersja obsługuje post formularza. Drugi `Create` — metoda ( `[HttpPost]` wersji) wywołań `ModelState.IsValid` do sprawdzenia, czy ten film zawiera wszystkie błędy weryfikacji. Wywołanie tej metody ocenia wszelkie atrybuty weryfikacji, które zostały zastosowane do obiektu. Jeśli obiekt ma błędy sprawdzania poprawności `Create` metoda ponownie zostanie wyświetlony formularz. Jeśli nie ma żadnych błędów, metoda zapisuje ten nowy film w bazie danych. W naszym przykładzie filmu formularza nie jest opublikowane w do serwera, gdy występują błędy sprawdzania poprawności wykrywane po stronie klienta; drugi `Create` metoda nigdy nie jest wywoływana, gdy występują błędy sprawdzania poprawności po stronie klienta. Jeśli wyłączysz JavaScript w przeglądarce, sprawdzanie poprawności klienta jest wyłączona, a można przetestować HTTP POST `Create` metoda `ModelState.IsValid` wykrywanie wszelkie błędy sprawdzania poprawności.

Możesz ustawić punkt przerwania w `[HttpPost] Create` metody i sprawdź, nigdy nie jest wywoływana metoda, weryfikacji po stronie klienta nie będzie przesyłać dane formularza, gdy wykryto błędy sprawdzania poprawności. Jeśli można wyłączyć języka JavaScript w przeglądarce, a następnie Prześlij formularz z błędami, punkt przerwania zostanie osiągnięty. Będzie nadal się pojawiać pełna Walidacja bez kodu JavaScript. 

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce FireFox.

![Firefox: Na karcie Zawartość opcje Usuń zaznaczenie pola wyboru Włącz język Javascript.](~/tutorials/first-mvc-app/validation/_static/ff.png)

Na poniższej ilustracji przedstawiono sposób wyłączania JavaScript w przeglądarce Chrome.

![Google Chrome: W sekcji kodu Javascript w zawartości ustawień wybierz opcję nie zezwalają na dowolnej lokacji do uruchomienia kodu JavaScript.](~/tutorials/first-mvc-app/validation/_static/chrome.png)

Po wyłączeniu JavaScript Opublikuj nieprawidłowe dane i krok po kroku debugera.

![Podczas debugowania na wpis nieprawidłowych danych, funkcję Intellisense w ModelState.IsValid pokazuje, że wartość to false.](~/tutorials/first-mvc-app/validation/_static/ms.png)

Część *Create.cshtml* Wyświetl szablon jest wyświetlany w niej następujące znaczniki:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/CreateRatingBrevity.html)]

Poprzedni kod znaczników jest używany przez metody akcji, aby wyświetlić początkowy formularz i wyświetlić ją ponownie w przypadku wystąpienia błędu.

[Pomocnik tagu dane wejściowe](xref:mvc/views/working-with-forms) używa [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atrybutów, a następnie tworzy atrybutów HTML potrzebne dla technologii jQuery weryfikacji po stronie klienta. [Pomocnik tagu weryfikacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) wyświetla błędy sprawdzania poprawności. Zobacz [weryfikacji](xref:mvc/models/validation) Aby uzyskać więcej informacji.

Co to jest bardzo NAS cieszy się o tego podejścia jest to, że żaden kontroler ani `Create` Wyświetl szablon wie, nic o regułach rzeczywista weryfikacja wymuszany ani o zbyt małą określone komunikaty o błędach wyświetlane. Reguł sprawdzania poprawności i ciągi błędów są określane tylko w `Movie` klasy. Te same zasady sprawdzania poprawności są automatycznie stosowane do `Edit` widoku i wszystkich innych widoków szablonów można utworzyć, które edytować modelu.

Jeśli potrzebujesz zmienić logikę weryfikacji, możesz to zrobić w dokładnie jednego miejsca przez dodanie atrybutów sprawdzania poprawności do modelu (w tym przykładzie `Movie` klasy). Nie trzeba już martwić się o różnych części aplikacji jest niespójna z jak zasady są wymuszane — całą logikę weryfikacji będą zdefiniowane w jednym miejscu i użyć wszędzie. Zapewnia bardzo czystym kodzie i ułatwia utrzymanie i rozwój. I oznacza, że można będzie można w pełni zapewniane susz zasady.

## <a name="using-datatype-attributes"></a>Przy użyciu atrybutów typu danych

Otwórz *Movie.cs* plików i zbadaj `Movie` klasy. `System.ComponentModel.DataAnnotations` Przestrzeń nazw zawiera atrybuty formatowania, oprócz wbudowanych zestaw atrybutów weryfikacji. Firma Microsoft została już zastosowana `DataType` wartości wyliczenia, Data wydania i pola Cena. Poniższy kod przedstawia `ReleaseDate` i `Price` właściwości z odpowiednią `DataType` atrybutu.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`DataType` Atrybuty zawierają tylko wskazówki dotyczące aparatu widoku do formatowania danych (i dostarcza elementy i atrybuty, takie jak `<a>` dla adresu URL i `<a href="mailto:EmailAddress.com">` do obsługi poczty e-mail. Możesz użyć `RegularExpression` atrybutu, aby sprawdzić poprawność formatu danych. `DataType` Atrybut jest używany do określenia typu danych, który jest bardziej szczegółowe niż typ wewnętrznej bazy danych, nie ma atrybutów sprawdzania poprawności. W tym przypadku ma być uruchamiany tylko do śledzenia daty, a nie godziny. `DataType` Wyliczenie udostępnia dla wielu typów danych, takie jak data, w czasie, numer telefonu, waluty, EmailAddress i wiele innych. `DataType` Atrybut można również włączyć automatyczne udostępnianie funkcji specyficznych dla typu aplikacji. Na przykład `mailto:` łącza mogą być tworzone dla `DataType.EmailAddress`, i można podać selektora daty `DataType.Date` w przeglądarkach obsługujących HTML5. `DataType` Atrybuty emitować HTML 5 `data-` atrybutów (Wymowa: dane dash), które może zrozumieć przeglądarki HTML 5. `DataType` Atrybuty czy **nie** Podaj wszelkie sprawdzania poprawności.

`DataType.Date` nie określa format daty, która jest wyświetlana. Domyślnie pole danych są wyświetlane domyślne formaty oparte na tym serwerze `CultureInfo`.

`DisplayFormat` Atrybut jest używany jawnie określić format daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinien również będą stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Nie może być, w przypadku niektórych pól — na przykład w przypadku wartości waluty, nie będzie prawdopodobnie symbol waluty w polu tekstowym do edycji.)

Możesz użyć `DisplayFormat` atrybutu przez sam, ale zazwyczaj jest dobry pomysł, aby użyć `DataType` atrybutu. `DataType` Atrybut umożliwia przekazywanie semantykę dane, a nie jak renderować ją na ekranie i zapewnia następujące korzyści, które nie można uzyskać za pomocą DisplayFormat:

* Przeglądarka można włączyć funkcje HTML5 (na przykład pokazać kontrolki kalendarza, symbol waluty odpowiednich ustawień regionalnych, przesyłanie pocztą e-mail łączy, itp.)

* Domyślnie przeglądarka wyświetli dane przy użyciu poprawny format, w oparciu o ustawienia regionalne.

* `DataType` Atrybutu aby umożliwić MVC wybrać szablon po prawej stronie pola w celu przedstawienia tych danych ( `DisplayFormat` Jeśli używany przez samego korzysta z szablonu ciągu).

> [!NOTE]
> dotyczącą weryfikacji jQuery nie działa w przypadku `Range` atrybutu i `DateTime`. Na przykład poniższy kod zawsze będzie wyświetlała błąd weryfikacji po stronie klienta, nawet wtedy, gdy jest to data mieści się w określonym zakresie:
>
> `[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]`

Należy wyłączyć sprawdzanie poprawności Data jQuery, aby użyć `Range` atrybutem `DateTime`. Ogólnie nie jest dobrą praktyką jest kompilowanie twardych dat w ramach modeli za pomocą `Range` atrybutu i `DateTime` jest niezalecane.

Poniższy kod pokazuje atrybuty łączenie w jednym wierszu:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRatingDAmult.cs?name=snippet1)]

W następnej części serii, firma Microsoft recenzji aplikacji i wprowadzić kilka ulepszeń do automatycznie generowanego `Details` i `Delete` metody.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Praca z formularzami](xref:mvc/views/working-with-forms)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)
* [Wprowadzenie do pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
* [Tworzenie pomocników tagów](xref:mvc/views/tag-helpers/authoring)

> [!div class="step-by-step"]
> [Poprzednie](new-field.md)
> [dalej](details.md)  
