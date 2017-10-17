<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="52772-101">Tworzenie szkieletu modelu film</span><span class="sxs-lookup"><span data-stu-id="52772-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="52772-102">Otwórz okno polecenia w katalogu projektu (katalog, który zawiera *Program.cs*, *Startup.cs*, i *.csproj* plików).</span><span class="sxs-lookup"><span data-stu-id="52772-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="52772-103">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="52772-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="52772-104">Jeśli zostanie wyświetlony błąd:</span><span class="sxs-lookup"><span data-stu-id="52772-104">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="52772-105">Zamknij program Visual Studio i ponownie uruchom polecenie.</span><span class="sxs-lookup"><span data-stu-id="52772-105">Exit Visual Studio and run the command again.</span></span>