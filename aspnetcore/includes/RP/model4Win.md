<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Tworzenie szkieletu modelu film

* Uruchom następujące polecenie w wierszu polecenia (w katalogu projektu, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików):

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Jeśli zostanie wyświetlony błąd:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Błąd poprzedzających odbywa się w niewłaściwego katalogu. Otwórz powłokę poleceń do katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików), a następnie uruchom polecenie poprzedzających.

Jeśli zostanie wyświetlony błąd:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Zamknij program Visual Studio i ponownie uruchom polecenie.
