---
title: Dodawanie walidacji do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Jak dodać sprawdzanie poprawności do aplikacji ASP.NET Core.
ms.author: riande
ms.date: 04/13/2017
uid: tutorials/first-mvc-app/validation
ms.openlocfilehash: ecf3d011b38347eb32020df00e44d93ca789443a
ms.sourcegitcommit: 99e71ae03319ab386baf2ebde956fc2d511df8b8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2020
ms.locfileid: "80242539"
---
# <a name="add-validation-to-an-aspnet-core-mvc-app"></a>Dodawanie walidacji do aplikacji ASP.NET Core MVC

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji:

* Logika walidacji jest dodawana do modelu `Movie`.
* Upewnij się, że reguły walidacji są wymuszane za każdym razem, gdy użytkownik tworzy lub edytuje film.

## <a name="keeping-things-dry"></a>Przechowywanie SUCHEj zawartości

Jeden z założenia projektu MVC jest [suchy](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("nie powtarzaj się"). ASP.NET Core MVC zachęca do określania funkcjonalności lub zachowania tylko raz, a następnie znajdować się w dowolnym miejscu w aplikacji. Pozwala to zmniejszyć ilość kodu, który trzeba napisać, i sprawia, że kod jest mniej podatny na błędy, łatwiejszy do testowania i łatwiejszy w obsłudze.

Obsługa walidacji świadczona przez MVC i Entity Framework Core Code First jest dobrym przykładem zasady SUCHa w działaniu. Można deklaratywnie określić reguły walidacji w jednym miejscu (w klasie modelu), a reguły są wymuszane wszędzie w aplikacji.

[!INCLUDE[](~/includes/RP-MVC/validation.md)]

## <a name="validation-error-ui"></a>Interfejs użytkownika błędu walidacji

Uruchom aplikację i przejdź do kontrolera filmów.

Naciśnij link **Utwórz nowy** , aby dodać nowy film. Wypełnij formularz nieprawidłowymi wartościami. Gdy tylko Walidacja po stronie klienta jQuery wykryje błąd, zostanie wyświetlony komunikat o błędzie.

![Formularz wyświetlania filmu z wieloma błędami walidacji po stronie klienta jQuery](~/tutorials/first-mvc-app/validation/_static/val.png)

[!INCLUDE[](~/includes/localization/currency.md)]

Zwróć uwagę, jak formularz automatycznie renderuje odpowiedni komunikat o błędzie walidacji w każdym polu zawierającym nieprawidłową wartość. Błędy są wymuszane po stronie klienta (przy użyciu języków JavaScript i jQuery) i po stronie serwera (w przypadku, gdy użytkownik ma wyłączony kod JavaScript).

Znacząca korzyść polega na tym, że nie trzeba zmieniać jednego wiersza kodu w klasie `MoviesController` lub w widoku *Create. cshtml* , aby włączyć ten interfejs użytkownika weryfikacji. Kontroler i widoki utworzone wcześniej w tym samouczku automatycznie pobierają reguły sprawdzania poprawności określone przy użyciu atrybutów walidacji we właściwościach klasy modelu `Movie`. Sprawdzanie poprawności testu przy użyciu metody akcji `Edit` i tej samej walidacji jest stosowane.

Dane formularza nie są wysyłane do serwera, dopóki nie zostaną wykryte błędy weryfikacji po stronie klienta. Można to sprawdzić, umieszczając punkt przerwania w metodzie `HTTP Post` przy użyciu [Narzędzia programu Fiddler](https://www.telerik.com/fiddler) lub [narzędzi programistycznych F12](/microsoft-edge/devtools-guide).

## <a name="how-validation-works"></a>Jak działa Walidacja

Możesz zastanawiać się, jak został wygenerowany interfejs użytkownika weryfikacji bez aktualizacji kodu w kontrolerze lub widokach. Poniższy kod przedstawia dwie metody `Create`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

W pierwszej metodzie akcji `Create` (HTTP GET) jest wyświetlany początkowy formularz tworzenia. Druga wersja (`[HttpPost]`) obsługuje wpis w formularzu. Druga metoda `Create` (wersja `[HttpPost]`) `ModelState.IsValid`, aby sprawdzić, czy film ma błędy walidacji. Wywołanie tej metody szacuje wszystkie atrybuty walidacji, które zostały zastosowane do obiektu. Jeśli obiekt ma błędy walidacji, Metoda `Create` ponowne wyświetli formularz. Jeśli nie ma żadnych błędów, Metoda zapisuje nowy film w bazie danych. W naszym przykładzie filmu nie jest on ogłaszany na serwerze, gdy na stronie klienta wykryto błędy walidacji. Druga metoda `Create` nie jest nigdy wywoływana, gdy występują błędy walidacji po stronie klienta. W przypadku wyłączenia języka JavaScript w przeglądarce sprawdzanie poprawności klienta jest wyłączone i można testować metodę `Create` POST protokołu HTTP `ModelState.IsValid` wykrywania błędów walidacji.

Można ustawić punkt przerwania w metodzie `[HttpPost] Create` i sprawdzić, czy metoda nie jest nigdy wywoływana, podczas walidacji po stronie klienta nie będą przesyłane dane formularza po wykryciu błędów walidacji. Jeśli wyłączysz JavaScript w przeglądarce, a następnie prześlesz formularz z błędami, zostanie osiągnięty punkt przerwania. Nadal będziesz mieć pełną weryfikację bez języka JavaScript. 

Na poniższej ilustracji przedstawiono sposób wyłączania języka JavaScript w przeglądarce Firefox.

![Firefox: na karcie zawartość w obszarze Opcje usuń zaznaczenie pola wyboru Włącz język JavaScript.](~/tutorials/first-mvc-app/validation/_static/ff.png)

Na poniższej ilustracji przedstawiono sposób wyłączania języka JavaScript w przeglądarce Chrome.

![Google Chrome: w sekcji JavaScript ustawień zawartości wybierz opcję nie Zezwalaj na uruchamianie skryptów JavaScript przez żadną lokację.](~/tutorials/first-mvc-app/validation/_static/chrome.png)

Po wyłączeniu języka JavaScript Opublikuj nieprawidłowe dane i przechodzenie przez debuger.

![Podczas debugowania wpisu nieprawidłowych danych funkcja IntelliSense w ModelState. IsValid pokazuje wartość false.](~/tutorials/first-mvc-app/validation/_static/ms.png)

Część szablonu widoku *Create. cshtml* jest pokazana w następującej postaci:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/CreateRatingBrevity.html)]

Poprzedzające znaczniki są używane przez metody akcji, aby wyświetlić początkowy formularz i ponownie wyświetlić go w przypadku błędu.

[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms) używa atrybutów [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) i tworzy atrybuty HTML, które są zbędne do walidacji jQuery po stronie klienta. [Pomocnik tagów walidacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) wyświetla błędy walidacji. Aby uzyskać więcej informacji, zobacz [Walidacja](xref:mvc/models/validation) .

W rzeczywistości to podejście jest takie, że żaden kontroler ani szablon widoku `Create` nie wie o faktycznych regułach walidacji lub o określonych komunikatach o błędach. Reguły walidacji i ciągi błędów są określone tylko w klasie `Movie`. Te same reguły sprawdzania poprawności są automatycznie stosowane do widoku `Edit` i wszystkich innych szablonów widoków, które można utworzyć, edytując model.

W przypadku konieczności zmiany logiki walidacji można to zrobić w dokładnie jednym miejscu przez dodanie atrybutów sprawdzania poprawności do modelu (w tym przykładzie klasy `Movie`). Nie trzeba martwić się o różne części aplikacji, które nie są zgodne z zasadami, w których są wymuszane — Cała logika walidacji zostanie zdefiniowana w jednym miejscu i użyta wszędzie. Dzięki temu kod jest bardzo czysty i ułatwia utrzymanie i rozwój. Oznacza to, że będziesz w pełni przestrzegać SUCHEj zasady.

## <a name="using-datatype-attributes"></a>Używanie atrybutów DataType

Otwórz plik *Movie.cs* i zapoznaj się z klasą `Movie`. Przestrzeń nazw `System.ComponentModel.DataAnnotations` zawiera atrybuty formatowania oprócz wbudowanego zestawu atrybutów walidacji. W dacie wydania i w polach cen została już zastosowana wartość wyliczenia `DataType`. Poniższy kod pokazuje `ReleaseDate` i `Price` właściwości z odpowiednim atrybutem `DataType`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

Atrybuty `DataType` zawierają tylko wskazówki dla aparatu widoku, aby sformatować dane (i dostarczyć elementy/atrybuty, takie jak `<a>` dla adresu URL i `<a href="mailto:EmailAddress.com">` dla poczty e-mail. Możesz użyć atrybutu `RegularExpression`, aby sprawdzić poprawność formatu danych. Atrybut `DataType` służy do określania typu danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych, nie są atrybutami walidacji. W tym przypadku chcemy tylko śledzić datę, a nie godzinę. Wyliczenie `DataType` zapewnia wiele typów danych, takich jak data, godzina, numer telefonu, waluta, EmailAddress i inne. Atrybut `DataType` może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu. Na przykład można utworzyć link `mailto:` dla `DataType.EmailAddress`i można dostarczyć selektor daty dla `DataType.Date` w przeglądarkach obsługujących HTML5. Atrybuty `DataType` emitują `data-` HTML 5 (wymawiane kreski danych), które mogą zrozumieć przeglądarki HTML 5. Atrybuty `DataType` **nie zapewniają żadnych** weryfikacji.

`DataType.Date` nie określa formatu wyświetlanej daty. Domyślnie pole dane jest wyświetlane zgodnie z domyślnymi formatami opartymi na `CultureInfo`serwera.

Atrybut `DisplayFormat` jest używany do jawnego określania formatu daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

Ustawienie `ApplyFormatInEditMode` określa, że formatowanie powinno być również stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Możesz nie chcieć, aby w przypadku niektórych pól — na przykład w przypadku wartości walutowych, prawdopodobnie nie chcesz, aby symbol waluty był widoczny w polu tekstowym do edycji).

Możesz użyć atrybutu `DisplayFormat` przez samego siebie, ale zazwyczaj dobrym pomysłem jest użycie atrybutu `DataType`. Atrybut `DataType` przekazuje semantykę danych w przeciwieństwie do sposobu renderowania na ekranie i zapewnia następujące korzyści, których nie można uzyskać za pomocą DisplayFormat:

* Przeglądarka może włączać funkcje HTML5 (na przykład w celu wyświetlania kontrolki kalendarza, symbolu waluty właściwej dla ustawień regionalnych, linków e-mail itp.).

* Domyślnie przeglądarka będzie renderować dane przy użyciu poprawnego formatu na podstawie ustawień regionalnych.

* Atrybut `DataType` umożliwia wybranie odpowiedniego szablonu pola w celu renderowania danych (`DisplayFormat`, jeśli jest używany przez siebie za pomocą szablonu ciągu).

> [!NOTE]
> Walidacja jQuery nie działa z atrybutem `Range` i `DateTime`. Na przykład poniższy kod zawsze będzie wyświetlał błąd walidacji po stronie klienta, nawet wtedy, gdy data jest w określonym zakresie:
>
> `[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]`

Należy wyłączyć sprawdzanie poprawności daty jQuery, aby użyć atrybutu `Range` z `DateTime`. Ogólnie rzecz biorąc, nie jest dobrym sposobem kompilowania dat stałych w modelach, dlatego przy użyciu `Range` atrybutu i `DateTime` jest niezalecane.

Poniższy kod ilustruje łączenie atrybutów w jednym wierszu:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRatingDAmult.cs?name=snippet1)]

W następnej części serii analizujemy aplikację i wprowadzamy kilka ulepszeń automatycznie generowanych `Details` i `Delete`.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Praca z formularzami](xref:mvc/views/working-with-forms)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)
* [Wprowadzenie do pomocy tagów](xref:mvc/views/tag-helpers/intro)
* [Autorzy tagów](xref:mvc/views/tag-helpers/authoring)

> [!div class="step-by-step"]
> [Poprzednie](new-field.md)
> [dalej](details.md)  
