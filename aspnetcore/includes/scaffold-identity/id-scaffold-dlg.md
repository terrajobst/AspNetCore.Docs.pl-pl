<span data-ttu-id="a3734-101">Uruchom Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="a3734-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a3734-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3734-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a3734-103">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="a3734-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a3734-104">W okienku po lewej stronie **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="a3734-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="a3734-105">W **tożsamość usługi ADD** okno dialogowe, wybierz odpowiednie opcje.</span><span class="sxs-lookup"><span data-stu-id="a3734-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="a3734-106">Wybierz istniejący stronę układu lub plik układu zostanie zastąpiony niepoprawny kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="a3734-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="a3734-107">Na przykład `~/Pages/Shared/_Layout.cshtml` dla stron Razor `~/Views/Shared/_Layout.cshtml` dla projektów MVC</span><span class="sxs-lookup"><span data-stu-id="a3734-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="a3734-108">Wybierz **+** przycisk, aby utworzyć nową **klasa kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="a3734-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="a3734-109">Wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="a3734-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a3734-110">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a3734-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a3734-111">Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:</span><span class="sxs-lookup"><span data-stu-id="a3734-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="a3734-112">Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) pliku.</span><span class="sxs-lookup"><span data-stu-id="a3734-112">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="a3734-113">Uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="a3734-113">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="a3734-114">Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="a3734-114">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="a3734-115">W folderze projektu należy uruchomić Generator szkieletu tożsamości z wybranymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="a3734-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="a3734-116">Na przykład aby skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalną liczbę plików, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a3734-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```

-------------
