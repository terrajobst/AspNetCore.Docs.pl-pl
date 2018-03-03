<!--
[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Poprzedni `Index` metody:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

Zaktualizowany interfejs `Index` metody z `id` parametru:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

Tytuł wyszukiwania mogą być obecnie przekazywane jako dane trasy (segment adresu URL) zamiast jako wartość ciągu zapytania.

![Widok indeksu z widma word dodane do adresu Url i filmu zwrócona lista filmów Ghostbusters i Ghostbusters 2](../../tutorials/first-mvc-app/search/_static/g2.png)

Nie można jednak spodziewać się użytkowników, aby zmodyfikować adres URL, za każdym razem, gdy chcą, aby wyszukać filmu. Dlatego teraz dodasz interfejsu użytkownika, aby ułatwić im filtrować filmów. Zmiana podpisu `Index` metody do testowania sposób przekazywania trasy wiązaniem `ID` parametr, wróć, aby przyspieszyć parametr o nazwie `searchString`:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

Otwórz *Views/Movies/Index.cshtml* pliku, a następnie dodaj `<form>` znaczników wskazanych poniżej:

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

Kod HTML `<form>` tagów używa [pomocnika Tag formularza](../../mvc/views/working-with-forms.md), więc po przesłaniu formularza ciąg filtru jest zamieszczana `Index` akcji kontrolera filmów. Zapisz zmiany, a następnie sprawdź filtr.

![Widok indeksu z widma word wpisane w polu tekstowym filtru tytułu](../../tutorials/first-mvc-app/search/_static/filter.png)

Brak nie `[HttpPost]` przeciążenia z `Index` metodę jako użytkownik może spodziewać się. Nie jest konieczne, ponieważ metoda nie jest zmiana stanu aplikacji, po prostu filtrowania danych.

Można dodać następujące `[HttpPost] Index` metody.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` Parametr jest używany do tworzenia przeciążenia `Index` metody. Będzie omawianiu który później w samouczku.

Jeśli dodasz tej metody, wywołujący akcji spowoduje dopasowanie `[HttpPost] Index` metody i `[HttpPost] Index` metoda może działać, jak pokazano na poniższej ilustracji.

![Okno przeglądarki z odpowiedzi aplikacji z indeksu HttpPost: Filtr widma](../../tutorials/first-mvc-app/search/_static/fo.png)

Jednak nawet jeśli dodasz to `[HttpPost]` wersji `Index` metody, jest to ograniczenie, w jaki sposób to wszystko zostało zaimplementowane. Załóżmy, że chcesz utworzyć zakładkę określonego wyszukiwania lub chcesz wysłać łącze do znajomych, mogą kliknąć w celu wyświetlenia tego samego filtrowane listy filmów. Zwróć uwagę, że adres URL żądania HTTP POST jest taki sam jak adres URL dla żądania GET (localhost:xxxxx/filmów/indeksu) — nie ma żadnych informacji wyszukiwania w adresie URL. Informacji o ciągu wyszukiwania są wysyłane do serwera jako [tworzą wartość pola](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data). Możesz sprawdzić, które z narzędzi deweloperskich przeglądarki lub znakomity [narzędzie Fiddler](http://www.telerik.com/fiddler). Na poniższym obrazie pokazano narzędzi deweloperskich przeglądarki Chrome:

![Karta Sieć narzędzi dla deweloperów w programie Microsoft Edge przedstawiający treści żądania z wartością parametru Wyszukiwany_ciąg widma](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

Parametr wyszukiwania jest widoczny i [XSRF](../../security/anti-request-forgery.md) tokenu w treści żądania. Należy zwrócić uwagę, jak wspomniano w poprzedniej samouczka [pomocnika Tag formularza](../../mvc/views/working-with-forms.md) generuje [XSRF](../../security/anti-request-forgery.md) token zabezpieczający przed sfałszowaniem. Firma Microsoft nie modyfikacji danych, więc nie musimy zweryfikować token metody kontrolera.

Ponieważ parametr wyszukiwania znajduje się w treści żądania, a nie adres URL, nie można przechwytywanie informacji wyszukiwania do zakładki lub udostępniać innym osobom. Firma Microsoft będzie rozwiązać ten problem, określając żądania powinien być `HTTP GET`.
