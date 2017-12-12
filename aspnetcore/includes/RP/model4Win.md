<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="e6aa4-101">Tworzenie szkieletu modelu film</span><span class="sxs-lookup"><span data-stu-id="e6aa4-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="e6aa4-102">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="e6aa4-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e6aa4-103">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e6aa4-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```
  
<span data-ttu-id="e6aa4-104">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="e6aa4-104">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="e6aa4-105">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="e6aa4-105">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

<span data-ttu-id="e6aa4-106">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="e6aa4-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="e6aa4-107">Zamknij program Visual Studio i ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="e6aa4-107">Exit Visual Studio and run the command again.</span></span>