
## <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację i wybierz **Mvc Movie** łącza.
* Wybierz **Utwórz nowy** i utworzyć filmu łącza.

  ![Utwórz widok z polami genre, cen, Data wydania i tytuł](~/tutorials/first-mvc-app/adding-model/_static/movies.png)

* Nie można wprowadzić kropki i przecinki w `Price` pola. Do obsługi [weryfikacji jQuery](https://jqueryvalidation.org/) dla innych niż angielski, które użyj przecinka (",") dla punktu dziesiętnego i formaty daty z systemem innym niż angielski, należy wykonać kroki, aby globalize aplikacji. Zobacz [ https://github.com/aspnet/Docs/issues/4076 ](https://github.com/aspnet/Docs/issues/4076) i [dodatkowe zasoby](#additional-resources) Aby uzyskać więcej informacji. Teraz wprowadź tylko liczby całkowite, takie jak 10.

<a name="displayformatdatelocal"></a>

* W kilku lokalizacjach należy określić format daty. Zobacz wyróżniony kod poniżej.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

Będzie omawianiu `DataAnnotations` dalszej części samouczka.

Naciskając **Utwórz** powoduje, że formularz, aby opublikować na serwerze, gdzie informacje filmu są zapisywane w bazie danych. Aplikacja przekierowuje do */Movies* adres URL, w którym wyświetlane są informacje filmu nowo utworzony.

![Lista Film przedstawiający widok nowo utworzone filmy](~/tutorials/first-mvc-app/adding-model/_static/h.png)

Utwórz kilka więcej wpisów filmu. Spróbuj **Edytuj**, **szczegóły**, i **usunąć** łącza, które są wszystkie funkcjonalności.
