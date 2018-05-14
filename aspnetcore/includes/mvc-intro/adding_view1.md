# <a name="adding-a-view-to-an-aspnet-core-mvc-app"></a>Dodawanie widoku do aplikacji platformy ASP.NET Core MVC

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji możesz zmodyfikować `HelloWorldController` klasę, aby użyć widoku Razor pliki szablonu, aby bezpośrednio Hermetyzowanie proces generowania odpowiedzi HTML do klienta.

Możesz utworzyć plik szablonu widoku przy użyciu aparatu Razor. Szablony razor na podstawie widoku mają *.cshtml* rozszerzenia pliku. Udostępniają one elegancki sposób tworzenia danych wyjściowych HTML przy użyciu języka C#.

Obecnie `Index` metoda zwraca ciąg zawierający komunikat, który jest ustalony klasy kontrolera. W `HelloWorldController` klasy, Zastąp `Index` metodę z następującym kodem:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Zwraca poprzedni kod `View` obiektu. Szablon widoku używa do generowania odpowiedzi HTML do przeglądarki. Metody kontrolera (znanej także jako metody akcji), takich jak `Index` zazwyczaj zwracany przez metodę powyżej [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) (lub klasą pochodną `ActionResult`), nie jest typem, takie jak ciąg.
