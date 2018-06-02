<span data-ttu-id="2e997-101">Uruchom tworzenia szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="2e997-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2e997-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2e997-102">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="2e997-103">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="2e997-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="2e997-104">W lewym okienku **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **dodać**.</span><span class="sxs-lookup"><span data-stu-id="2e997-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="2e997-105">W **tożsamości Dodaj** okno dialogowe, wybierz odpowiednie opcje.</span><span class="sxs-lookup"><span data-stu-id="2e997-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="2e997-106">Wybierz stronę układu istniejących lub pliku układu zostaną zastąpione niepoprawny kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="2e997-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="2e997-107">Na przykład `~/Pages/Shared/_Layout.cshtml` dla stron Razor `~/Views/Shared/_Layout.cshtml` dla projektów MVC</span><span class="sxs-lookup"><span data-stu-id="2e997-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span> 
  * <span data-ttu-id="2e997-108">Wybierz **+** przycisk, aby utworzyć nowy **klasa kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="2e997-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="2e997-109">Wybierz **dodać**.</span><span class="sxs-lookup"><span data-stu-id="2e997-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2e997-110">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2e997-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2e997-111">Jeśli tworzenia szkieletu ASP.NET nie został wcześniej zainstalowany, zainstaluj go:</span><span class="sxs-lookup"><span data-stu-id="2e997-111">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="2e997-112">Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) pliku.</span><span class="sxs-lookup"><span data-stu-id="2e997-112">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="2e997-113">Uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="2e997-113">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
```

<span data-ttu-id="2e997-114">Uruchom następujące polecenie, aby wyświetlić listę opcji tworzenia szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="2e997-114">Run the following command to list the Identity scaffolder options:</span></span>


```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="2e997-115">W folderze projektu Uruchom tworzenia szkieletu tożsamości z wybranymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="2e997-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="2e997-116">Na przykład można skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalna liczba plików, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2e997-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```
-------------