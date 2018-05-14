## <a name="add-initial-migration-and-update-the-database"></a>Dodaj początkowej migracji i aktualizacji bazy danych

* Otwórz wiersz polecenia i przejdź do katalogu projektu. (Katalog zawierający *Startup.cs* pliku).

* W wierszu polecenia wpisz następujące polecenia:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [Oprogramowanie .NET core](/dotnet/core/tools/index) jest implementacją wieloplatformowych platformy .NET. Oto, wykonaj następujące polecenia:

  * [Przywracanie DotNet](/dotnet/core/tools/dotnet-restore): pobiera pakiety NuGet określone w *.csproj* pliku.
  * `dotnet ef migrations add Initial` Uruchamia polecenie migracje interfejsu wiersza polecenia programu Entity Framework .NET Core i tworzy początkowej migracji. Parametr po "Dodaj" jest nazwą, która zostanie przypisana do migracji. W tym miejscu możesz jest nazw migracji "Początkowego" ponieważ jest on migracji wstępnej bazy danych. Ta operacja powoduje utworzenie *danych/migracje/\<Data i godzina > _Initial.cs* plik zawierający polecenia migracji, aby dodać *film* tabeli w bazie danych.
  * `dotnet ef database update`  Aktualizuje bazę danych do migracji, które właśnie utworzyliśmy.

W następnym samouczku dowiesz się ciąg połączenia i bazy danych. Dowiesz się o zmianach wprowadzonych w modelu danych w [Dodawanie pola](xref:tutorials/first-mvc-app/new-field) samouczka.
