## <a name="add-validation-rules-to-the-movie-model"></a>Dodawanie reguł sprawdzania poprawności do modelu movie

Otwórz *Movie.cs* pliku. Przestrzeń nazw DataAnnotations zawiera zestaw atrybutów weryfikacji wbudowanych, które są stosowane w sposób deklaratywny do klasa lub właściwość. DataAnnotations zawiera też atrybuty formatowania, takich jak `DataType` , ułatwić formatowanie i nie udostępniamy żadnych sprawdzania poprawności.

Aktualizacja `Movie` klasy, aby skorzystać z wbudowanych `Required`, `StringLength`, `RegularExpression`, i `Range` atrybutów sprawdzania poprawności.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie22/Models/MovieDateRatingDA.cs?name=snippet1)]

Atrybuty weryfikacji określić zachowanie, które mają zostać wymuszone we właściwościach modelu, w których są one stosowane do:

* `Required` i `MinimumLength` atrybuty wskazują, że właściwość musi mieć wartość, ale nic nie uniemożliwia użytkownikowi wprowadzanie odstępów do zaspokojenia tej weryfikacji.
* `RegularExpression` Atrybut jest używany do ograniczania znaków, które można danych wejściowych. W poprzednim kodzie "Gatunku":

  * Należy używać tylko liter.
  * Pierwszą literą jest wymagany do być pisane dużą literą. Biały znak, cyfry i znaki specjalne są niedozwolone.

* `RegularExpression` "Ocena":

  * Wymaga, aby pierwszy znak wielkiej litery.
  * Umożliwia znaki specjalne i liczby w kolejnych miejsca do magazynowania. "PG-13" jest prawidłowy dla klasyfikacji, ale nie powiedzie się "Gatunku".

* `Range` Atrybut ogranicza wartości do określonego zakresu.
* `StringLength` Atrybut pozwala ustawić maksymalną długość właściwości ciągu i opcjonalnie długości minimalnej.
* Typy wartości (takie jak `decimal`, `int`, `float`, `DateTime`) są założenia wymagane i nie ma potrzeby `[Required]` atrybutu.

Posiadanie reguły sprawdzania poprawności, które automatycznie wymuszanych przez platformy ASP.NET Core ułatwia zapewnienie Twojej aplikacji bardziej niezawodne. Gwarantuje również, że nie pamiętasz do sprawdzania poprawności coś i przypadkowo umożliwiają złe dane do bazy danych.
