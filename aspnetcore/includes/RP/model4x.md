<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="3451b-101">Tworzenie szkieletu modelu film</span><span class="sxs-lookup"><span data-stu-id="3451b-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="3451b-102">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="3451b-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="3451b-103">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3451b-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```