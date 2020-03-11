<!-- USED in RP and MVC tutorial -->

## <a name="add-validation-rules-to-the-movie-model"></a>Dodawanie reguł walidacji do modelu filmu

Przestrzeń nazw DataAnnotations zawiera zestaw wbudowanych atrybutów walidacji, które są stosowane deklaratywnie do klasy lub właściwości. Adnotacje DataAnnotation zawierają również atrybuty formatowania, takie jak `DataType`, które pomagają w formatowaniu i nie zapewniają weryfikacji.

Zaktualizuj klasę `Movie`, aby skorzystać z wbudowanych atrybutów `Required`, `StringLength`, `RegularExpression`i `Range`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRatingDA.cs?name=snippet1)]

Atrybuty walidacji określają zachowanie, które chcesz wymusić na właściwościach modelu, do których są stosowane:

* Atrybuty `Required` i `MinimumLength` wskazują, że właściwość musi mieć wartość; jednak nic nie zapobiega wprowadzaniu przez użytkownika białych znaków w celu zaspokojenia tej walidacji.
* Atrybut `RegularExpression` jest używany do ograniczania, jakie znaki mogą być wprowadzane. W poprzednim kodzie "gatunek":

  * Należy używać tylko liter.
  * Pierwsza litera musi być wielką literą. Odstępy, cyfry i znaki specjalne są niedozwolone.

* `RegularExpression` "Rating":

  * Wymaga, aby pierwszy znak był wielką literą.
  * Zezwala na znaki specjalne i cyfry w kolejnych odstępach. "PG-13" jest prawidłowy dla oceny, ale kończy się niepowodzeniem dla "gatunku".

* Atrybut `Range` ogranicza wartość do określonego zakresu.
* Atrybut `StringLength` pozwala ustawić maksymalną długość właściwości ciągu i opcjonalnie jej długość minimalną.
* Typy wartości (takie jak `decimal`, `int`, `float``DateTime`) są z natury wymagane i nie potrzebują atrybutu `[Required]`.

Automatyczne Wymuszanie reguł sprawdzania poprawności przez ASP.NET Core pomaga zwiększyć niezawodność aplikacji. Gwarantuje to również, że nie można zapomnieć, aby zweryfikować coś i przypadkowo umożliwić niewłaściwe dane w bazie danych.
