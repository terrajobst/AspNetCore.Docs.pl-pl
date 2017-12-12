<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu film

* Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).
* Uruchom następujące polecenie:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```
  
Jeśli zostanie wyświetlony błąd:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).

Jeśli zostanie wyświetlony błąd:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Zamknij program Visual Studio i ponownie uruchom polecenie.