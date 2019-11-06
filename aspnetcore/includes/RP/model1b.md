<!-- THIS INCLUDE USED BY MVC AND RP -->
Dodaj następujące właściwości do klasy `Movie`:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

Klasa `Movie` zawiera:

* Wartość pola `ID` jest wymagana przez bazę danych klucza podstawowego.
* `[DataType(DataType.Date)]`: atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) określa typ danych (Data). Z tym atrybutem:

  * Użytkownik nie musi wprowadzać informacji o czasie w polu Data.
  * Tylko data jest wyświetlana, a nie informacje o czasie.

[Adnotacje DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) są omówione w kolejnym samouczku.
