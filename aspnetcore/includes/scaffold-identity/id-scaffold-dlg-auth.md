<span data-ttu-id="35ec8-101">Uruchom tworzenia szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="35ec8-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="35ec8-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35ec8-102">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="35ec8-103">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="35ec8-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="35ec8-104">W lewym okienku **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **dodać**.</span><span class="sxs-lookup"><span data-stu-id="35ec8-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="35ec8-105">W **tożsamości Dodaj** okno dialogowe, wybierz odpowiednie opcje.</span><span class="sxs-lookup"><span data-stu-id="35ec8-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="35ec8-106">Wybierz stronę układu istniejących lub pliku układu zostaną zastąpione niepoprawny kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="35ec8-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="35ec8-107">Po wybraniu istniejący plik _Layout.cshtml jest **nie** zastąpione.</span><span class="sxs-lookup"><span data-stu-id="35ec8-107">When an existing _Layout.cshtml file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="35ec8-108">Na przykład `~/Pages/Shared/_Layout.cshtml` dla stron Razor `~/Views/Shared/_Layout.cshtml` dla projektów MVC</span><span class="sxs-lookup"><span data-stu-id="35ec8-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span> 
* <span data-ttu-id="35ec8-109">Aby użyć istniejącego kontekstu danych, wybierz co najmniej jeden plik do zastąpienia.</span><span class="sxs-lookup"><span data-stu-id="35ec8-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="35ec8-110">Należy wybrać co najmniej jeden plik, aby dodać kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="35ec8-110">You must select at least one file to add your data context.</span></span> 
  * <span data-ttu-id="35ec8-111">Wybierz klasy kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="35ec8-111">Select your data context class.</span></span>
  * <span data-ttu-id="35ec8-112">Wybierz **dodać**.</span><span class="sxs-lookup"><span data-stu-id="35ec8-112">Select **ADD**.</span></span>
* <span data-ttu-id="35ec8-113">Aby utworzyć nowy kontekst użytkownika i ewentualnie utworzyć klasę użytkownika niestandardowego dla tożsamości:</span><span class="sxs-lookup"><span data-stu-id="35ec8-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="35ec8-114">Wybierz **+** przycisk, aby utworzyć nowy **klasa kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="35ec8-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="35ec8-115">Wybierz **dodać**.</span><span class="sxs-lookup"><span data-stu-id="35ec8-115">Select **ADD**.</span></span>
  
<span data-ttu-id="35ec8-116">Uwaga: Jeśli tworzysz nowy kontekst użytkownika, nie trzeba wybrać plik do zastąpienia.</span><span class="sxs-lookup"><span data-stu-id="35ec8-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="35ec8-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="35ec8-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="35ec8-118">Jeśli tworzenia szkieletu ASP.NET nie został wcześniej zainstalowany, zainstaluj go:</span><span class="sxs-lookup"><span data-stu-id="35ec8-118">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator --version 2.1.0-rc1-final
```

<span data-ttu-id="35ec8-119">Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) pliku.</span><span class="sxs-lookup"><span data-stu-id="35ec8-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="35ec8-120">Uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="35ec8-120">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v "2.1.0-rc1-final"
dotnet restore
```

<span data-ttu-id="35ec8-121">Uruchom następujące polecenie, aby wyświetlić listę opcji tworzenia szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="35ec8-121">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="35ec8-122">W folderze projektu Uruchom tworzenia szkieletu tożsamości z wybranymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="35ec8-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="35ec8-123">Na przykład można skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalna liczba plików, uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="35ec8-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="35ec8-124">Użyj poprawnej nazwy FQDN kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="35ec8-124">Use the correct fully qualified name for your DB context:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```
-------------
