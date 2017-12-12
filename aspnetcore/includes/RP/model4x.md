<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="96869-101">Tworzenie szkieletu modelu film</span><span class="sxs-lookup"><span data-stu-id="96869-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="96869-102">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="96869-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="96869-103">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="96869-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```
  
<span data-ttu-id="96869-104">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="96869-104">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="96869-105">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="96869-105">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>