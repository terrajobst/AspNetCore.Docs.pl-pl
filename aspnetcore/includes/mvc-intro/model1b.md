Dodaj następujące właściwości do `Movie` klasy:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/Movie.cs?name=snippet1)]

`Movie` Klasa zawiera:

* `Id` Pola, które jest wymagane przez bazę danych dla klucza podstawowego.
* `[DataType(DataType.Date)]`:  [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) atrybut określa typ danych (`Date`). Z tego atrybutu:

  * Użytkownik nie musi wprowadzić informacje o czasie w polu daty.
  * Tylko data jest wyświetlany, nie czas informacji.

[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) zostały omówione później w samouczku.