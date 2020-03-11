::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="94d35-101">Uruchom szkielet tożsamości:</span><span class="sxs-lookup"><span data-stu-id="94d35-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="94d35-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94d35-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="94d35-103">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="94d35-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="94d35-104">W lewym okienku okna dialogowego **Dodawanie szkieletu** wybierz pozycję **tożsamość** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="94d35-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="94d35-105">W oknie dialogowym **Dodawanie tożsamości** wybierz odpowiednie opcje.</span><span class="sxs-lookup"><span data-stu-id="94d35-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="94d35-106">Wybierz istniejącą stronę układu lub plik układu zostanie zastąpiony nieprawidłowym znacznikiem.</span><span class="sxs-lookup"><span data-stu-id="94d35-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="94d35-107">Gdy zostanie wybrany istniejący plik *\_Layout. cshtml* , **nie** zostanie on nadpisany.</span><span class="sxs-lookup"><span data-stu-id="94d35-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="94d35-108">Na przykład: `~/Pages/Shared/_Layout.cshtml` dla Razor Pages `~/Views/Shared/_Layout.cshtml` dla projektów MVC</span><span class="sxs-lookup"><span data-stu-id="94d35-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="94d35-109">Aby użyć istniejącego kontekstu danych, wybierz co najmniej jeden plik do przesłonięcia.</span><span class="sxs-lookup"><span data-stu-id="94d35-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="94d35-110">Musisz wybrać co najmniej jeden plik, aby dodać kontekst danych.</span><span class="sxs-lookup"><span data-stu-id="94d35-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="94d35-111">Wybierz klasę kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="94d35-111">Select your data context class.</span></span>
  * <span data-ttu-id="94d35-112">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="94d35-112">Select **Add**.</span></span>
* <span data-ttu-id="94d35-113">Aby utworzyć nowy kontekst użytkownika i ewentualnie utworzyć niestandardową klasę użytkownika dla tożsamości:</span><span class="sxs-lookup"><span data-stu-id="94d35-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="94d35-114">Wybierz przycisk **+** , aby utworzyć nową **klasę kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="94d35-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="94d35-115">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="94d35-115">Select **Add**.</span></span>

<span data-ttu-id="94d35-116">Uwaga: w przypadku tworzenia nowego kontekstu użytkownika nie trzeba wybierać pliku do przesłonięcia.</span><span class="sxs-lookup"><span data-stu-id="94d35-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="94d35-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="94d35-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="94d35-118">Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:</span><span class="sxs-lookup"><span data-stu-id="94d35-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="94d35-119">Dodaj wymagane odwołania pakietu NuGet do pliku projektu (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="94d35-119">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="94d35-120">Uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="94d35-120">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="94d35-121">Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="94d35-121">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="94d35-122">W folderze projektu uruchom program do tworzenia szkieletu tożsamości z żądanymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="94d35-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="94d35-123">Na przykład, aby skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalnej liczby plików, uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="94d35-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="94d35-124">Użyj prawidłowej w pełni kwalifikowanej nazwy dla kontekstu bazy danych:</span><span class="sxs-lookup"><span data-stu-id="94d35-124">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="94d35-125">Program PowerShell używa średnika jako separatora poleceń.</span><span class="sxs-lookup"><span data-stu-id="94d35-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="94d35-126">W przypadku korzystania z programu PowerShell wpisz średniki na liście plików lub Umieść listę plików w podwójnych cudzysłowach.</span><span class="sxs-lookup"><span data-stu-id="94d35-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="94d35-127">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="94d35-127">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="94d35-128">Jeśli uruchomisz szkielet tożsamości bez określenia flagi `--files` lub flagi `--useDefaultUI`, wszystkie dostępne strony interfejsu użytkownika tożsamości zostaną utworzone w projekcie.</span><span class="sxs-lookup"><span data-stu-id="94d35-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="94d35-129">Uruchom szkielet tożsamości:</span><span class="sxs-lookup"><span data-stu-id="94d35-129">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="94d35-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94d35-130">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="94d35-131">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="94d35-131">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="94d35-132">W lewym okienku okna dialogowego **Dodawanie szkieletu** wybierz pozycję **tożsamość** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="94d35-132">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="94d35-133">W oknie dialogowym **Dodawanie tożsamości** wybierz odpowiednie opcje.</span><span class="sxs-lookup"><span data-stu-id="94d35-133">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="94d35-134">Wybierz istniejącą stronę układu lub plik układu zostanie zastąpiony nieprawidłowym znacznikiem.</span><span class="sxs-lookup"><span data-stu-id="94d35-134">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="94d35-135">Gdy zostanie wybrany istniejący plik *\_Layout. cshtml* , **nie** zostanie on nadpisany.</span><span class="sxs-lookup"><span data-stu-id="94d35-135">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="94d35-136">Na przykład: `~/Pages/Shared/_Layout.cshtml` dla Razor Pages `~/Views/Shared/_Layout.cshtml` dla projektów MVC</span><span class="sxs-lookup"><span data-stu-id="94d35-136">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="94d35-137">Aby użyć istniejącego kontekstu danych, wybierz co najmniej jeden plik do przesłonięcia.</span><span class="sxs-lookup"><span data-stu-id="94d35-137">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="94d35-138">Musisz wybrać co najmniej jeden plik, aby dodać kontekst danych.</span><span class="sxs-lookup"><span data-stu-id="94d35-138">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="94d35-139">Wybierz klasę kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="94d35-139">Select your data context class.</span></span>
  * <span data-ttu-id="94d35-140">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="94d35-140">Select **Add**.</span></span>
* <span data-ttu-id="94d35-141">Aby utworzyć nowy kontekst użytkownika i ewentualnie utworzyć niestandardową klasę użytkownika dla tożsamości:</span><span class="sxs-lookup"><span data-stu-id="94d35-141">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="94d35-142">Wybierz przycisk **+** , aby utworzyć nową **klasę kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="94d35-142">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="94d35-143">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="94d35-143">Select **Add**.</span></span>

<span data-ttu-id="94d35-144">Uwaga: w przypadku tworzenia nowego kontekstu użytkownika nie trzeba wybierać pliku do przesłonięcia.</span><span class="sxs-lookup"><span data-stu-id="94d35-144">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="94d35-145">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="94d35-145">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="94d35-146">Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:</span><span class="sxs-lookup"><span data-stu-id="94d35-146">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="94d35-147">Dodaj odwołanie do pakietu do [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do pliku projektu (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="94d35-147">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="94d35-148">Uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="94d35-148">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="94d35-149">Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="94d35-149">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="94d35-150">W folderze projektu uruchom program do tworzenia szkieletu tożsamości z żądanymi opcjami.</span><span class="sxs-lookup"><span data-stu-id="94d35-150">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="94d35-151">Na przykład, aby skonfigurować tożsamość przy użyciu domyślnego interfejsu użytkownika i minimalnej liczby plików, uruchom następujące polecenie.</span><span class="sxs-lookup"><span data-stu-id="94d35-151">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="94d35-152">Użyj prawidłowej w pełni kwalifikowanej nazwy dla kontekstu bazy danych:</span><span class="sxs-lookup"><span data-stu-id="94d35-152">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="94d35-153">Program PowerShell używa średnika jako separatora poleceń.</span><span class="sxs-lookup"><span data-stu-id="94d35-153">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="94d35-154">W przypadku korzystania z programu PowerShell wpisz średniki na liście plików lub Umieść listę plików w podwójnych cudzysłowach.</span><span class="sxs-lookup"><span data-stu-id="94d35-154">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="94d35-155">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="94d35-155">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="94d35-156">Jeśli uruchomisz szkielet tożsamości bez określenia flagi `--files` lub flagi `--useDefaultUI`, wszystkie dostępne strony interfejsu użytkownika tożsamości zostaną utworzone w projekcie.</span><span class="sxs-lookup"><span data-stu-id="94d35-156">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end