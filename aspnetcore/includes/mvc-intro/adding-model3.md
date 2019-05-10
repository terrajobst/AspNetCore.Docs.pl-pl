
## <a name="test-the-app"></a>Testowanie aplikacji

* Uruchom aplikację, a następnie naciśnij przycisk **filmu Mvc** łącza.
* Naciśnij pozycję **Utwórz nowy** link i utworzyć filmu.

  ![Utwórz widok z polami gatunku, ceny, Data wydania i tytuł](~/tutorials/first-mvc-app/adding-model/_static/movies.png)

* Nie można wprowadzić, kropki i przecinki w `Price` pola. Aby obsługiwać [dotyczącą weryfikacji jQuery](https://jqueryvalidation.org/) dla ustawień regionalnych innych niż angielski, które należy użyć przecinka (",") dla punktu dziesiętnego i formaty daty inne niż angielski, należy wykonać kroki, aby sprzedawać aplikację. Zobacz [ https://github.com/aspnet/AspNetCore.Docs/issues/4076 ](https://github.com/aspnet/AspNetCore.Docs/issues/4076) i [dodatkowe zasoby](#additional-resources) Aby uzyskać więcej informacji. Teraz po prostu wprowadź liczbami całkowitymi, takich jak 10.

<a name="displayformatdatelocal"></a>

* W kilku lokalizacjach, należy określić format daty. Zobacz wyróżniony kod poniżej.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateFormat.cs?name=snippet_1&highlight=2,10)]

Omówimy `DataAnnotations` później w samouczku.

Naciskając **Utwórz** powoduje, że formularz do opublikowania na serwerze, gdzie informacje filmu są zapisywane w bazie danych. Aplikacja przekierowuje do */Movies* adresu URL, w którym zostaną wyświetlone informacje filmu nowo utworzony.

![Lista Film przedstawiający widok nowo utworzone filmy](~/tutorials/first-mvc-app/adding-model/_static/h.png)

Utwórz kilka więcej wpisów filmu. Spróbuj **Edytuj**, **szczegóły**, i **Usuń** łącza, które są wszystkie funkcjonalności.
