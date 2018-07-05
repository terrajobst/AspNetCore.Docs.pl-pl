<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu Movie

* Uruchom następujące polecenie w wierszu polecenia (w katalogu projektu, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików):

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Jeśli zostanie wyświetlony błąd:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Błąd poprzedzający występuje, gdy jesteś w niewłaściwej katalogu. Otwórz powłokę wiersza polecenia do katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików), a następnie uruchom polecenie poprzedzający.

Jeśli zostanie wyświetlony błąd:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Zamknij program Visual Studio i ponownie uruchom polecenie.
