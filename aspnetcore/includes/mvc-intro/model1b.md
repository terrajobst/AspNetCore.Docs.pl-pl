Dodaj następujące właściwości do klasy `Movie`:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/Movie.cs?name=snippet1)]

Klasa `Movie` zawiera:

* Pole `Id`, które jest wymagane przez bazę danych dla klucza podstawowego.
* `[DataType(DataType.Date)]`: atrybut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) określa typ danych (`Date`). Z tym atrybutem:

  * Użytkownik nie musi wprowadzać informacji o czasie w polu Data.
  * Tylko data jest wyświetlana, a nie informacje o czasie.

[Adnotacje DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) są omówione w kolejnym samouczku.