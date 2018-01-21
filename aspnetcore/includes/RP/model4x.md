<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu film

* Uruchom następujące polecenie w wierszu polecenia (w katalogu projektu, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików):

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Jeśli zostanie wyświetlony błąd:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Otwórz powłokę poleceń do katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
