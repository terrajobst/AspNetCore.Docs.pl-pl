<span data-ttu-id="baa88-101">Uruchom szkielet tożsamości:</span><span class="sxs-lookup"><span data-stu-id="baa88-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="baa88-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="baa88-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="baa88-103">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="baa88-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="baa88-104">W lewym okienku okna dialogowego **Dodawanie szkieletu** wybierz pozycję**Dodaj** **tożsamość** > .</span><span class="sxs-lookup"><span data-stu-id="baa88-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="baa88-105">W oknie dialogowym **Dodawanie tożsamości** wybierz odpowiednie opcje.</span><span class="sxs-lookup"><span data-stu-id="baa88-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="baa88-106">Wybierz istniejącą stronę układu lub plik układu zostanie zastąpiony nieprawidłowym znacznikiem.</span><span class="sxs-lookup"><span data-stu-id="baa88-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="baa88-107">Gdy zostanie wybrany istniejący  *\_plik Layout. cshtml* , **nie** zostanie on nadpisany.</span><span class="sxs-lookup"><span data-stu-id="baa88-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="baa88-108">Na przykład: `~/Pages/Shared/_Layout.cshtml` dla Razor Pages `~/Views/Shared/_Layout.cshtml` dla projektów MVC</span><span class="sxs-lookup"><span data-stu-id="baa88-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="baa88-109">Aby użyć istniejącego kontekstu danych, wybierz co najmniej jeden plik do przesłonięcia.</span><span class="sxs-lookup"><span data-stu-id="baa88-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="baa88-110">Musisz wybrać co najmniej jeden plik, aby dodać kontekst danych.</span><span class="sxs-lookup"><span data-stu-id="baa88-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="baa88-111">Wybierz klasę kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="baa88-111">Select your data context class.</span></span>
  * <span data-ttu-id="baa88-112">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="baa88-112">Select **Add**.</span></span>
* <span data-ttu-id="baa88-113">Aby utworzyć nowy kontekst użytkownika i ewentualnie utworzyć niestandardową klasę użytkownika dla tożsamości:</span><span class="sxs-lookup"><span data-stu-id="baa88-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="baa88-114">Wybierz **+** przycisk, aby utworzyć nową **klasa kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="baa88-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="baa88-115">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="baa88-115">Select **Add**.</span></span>

<span data-ttu-id="baa88-116">Uwaga: W przypadku tworzenia nowego kontekstu użytkownika nie trzeba wybierać pliku do przesłonięcia.</span><span class="sxs-lookup"><span data-stu-id="baa88-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="baa88-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="baa88-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="baa88-118">Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:</span><span class="sxs-lookup"><span data-stu-id="baa88-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="baa88-119">Dodaj odwołanie do pakietu do [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do pliku projektu (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="baa88-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="baa88-120">Uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="baa88-120">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="baa88-121">Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="baa88-121">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="baa88-122">W folderze projektu uruchom program do tworzenia szkieletu tożsamości z żądanymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="baa88-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="baa88-123">Na przykład, aby skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalnej liczby plików, uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="baa88-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="baa88-124">Użyj prawidłowej w pełni kwalifikowanej nazwy dla kontekstu bazy danych:</span><span class="sxs-lookup"><span data-stu-id="baa88-124">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

<span data-ttu-id="baa88-125">Program PowerShell używa średnika jako separatora poleceń.</span><span class="sxs-lookup"><span data-stu-id="baa88-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="baa88-126">W przypadku korzystania z programu PowerShell wpisz średniki na liście plików lub Umieść listę plików w podwójnych cudzysłowach.</span><span class="sxs-lookup"><span data-stu-id="baa88-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="baa88-127">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="baa88-127">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="baa88-128">W przypadku uruchomienia szkieletu tożsamości bez określenia `--files` flagi `--useDefaultUI` lub flagi wszystkie dostępne strony interfejsu użytkownika tożsamości zostaną utworzone w projekcie.</span><span class="sxs-lookup"><span data-stu-id="baa88-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---
