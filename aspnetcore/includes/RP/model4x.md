<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="7f67f-101">Tworzenie szkieletu modelu Movie</span><span class="sxs-lookup"><span data-stu-id="7f67f-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="7f67f-102">Uruchom następujące polecenie w wierszu polecenia (w katalogu projektu, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików):</span><span class="sxs-lookup"><span data-stu-id="7f67f-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="7f67f-103">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="7f67f-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="7f67f-104">Otwórz powłokę wiersza polecenia do katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="7f67f-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
