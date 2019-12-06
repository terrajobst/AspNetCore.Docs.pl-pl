---
title: Dodawanie, pobieranie i usuwanie danych tożsamości użytkownika w projekcie platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać danych niestandardowych użytkownika w tożsamości w projektach programu ASP.NET Core. Usuń dane na RODO.
ms.author: riande
ms.date: 12/05/2019
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: f54df68834cd3e2493e558aaab9851f036f3f01b
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880754"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="4ae86-104">Dodawanie, pobieranie i usuwanie danych niestandardowych użytkownika w tożsamości w projektach programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4ae86-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="4ae86-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4ae86-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4ae86-106">W tym artykule przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="4ae86-106">This article shows how to:</span></span>

* <span data-ttu-id="4ae86-107">Dodawanie danych niestandardowych użytkownika do aplikacji sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4ae86-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="4ae86-108">Oznacz niestandardowy model danych użytkownika przy użyciu atrybutu <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute>, aby był on automatycznie dostępny do pobrania i usunięcia.</span><span class="sxs-lookup"><span data-stu-id="4ae86-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="4ae86-109">Udostępnianie danych może zostać pobrana i usunąć pomaga spełnić wymagania [RODO](xref:security/gdpr) wymagania.</span><span class="sxs-lookup"><span data-stu-id="4ae86-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="4ae86-110">Przykładowy projekt jest tworzony na podstawie aplikacja internetowa ze stronami Razor, ale instrukcje są podobne dla aplikacji sieci web platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="4ae86-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="4ae86-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4ae86-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ae86-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="4ae86-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="4ae86-113">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="4ae86-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4ae86-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ae86-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="4ae86-115">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="4ae86-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="4ae86-116">Nadaj projektowi nazwę **WebApp1** Jeśli chcesz go odpowiada przestrzeni nazw z [Pobierz przykładowe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) kodu.</span><span class="sxs-lookup"><span data-stu-id="4ae86-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="4ae86-117">Wybierz **ASP.NET Core aplikację sieci Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="4ae86-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="4ae86-118">Na liście rozwijanej wybierz pozycję **ASP.NET Core 3,0**</span><span class="sxs-lookup"><span data-stu-id="4ae86-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="4ae86-119">Wybierz **aplikację sieci Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="4ae86-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="4ae86-120">Skompiluj i uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="4ae86-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="4ae86-121">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="4ae86-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="4ae86-122">Nadaj projektowi nazwę **WebApp1** Jeśli chcesz go odpowiada przestrzeni nazw z [Pobierz przykładowe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) kodu.</span><span class="sxs-lookup"><span data-stu-id="4ae86-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="4ae86-123">Wybierz **ASP.NET Core aplikację sieci Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="4ae86-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="4ae86-124">Na liście rozwijanej wybierz pozycję **ASP.NET Core 2,2**</span><span class="sxs-lookup"><span data-stu-id="4ae86-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="4ae86-125">Wybierz **aplikację sieci Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="4ae86-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="4ae86-126">Skompiluj i uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="4ae86-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4ae86-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4ae86-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="4ae86-128">Uruchom Generator szkieletu tożsamości</span><span class="sxs-lookup"><span data-stu-id="4ae86-128">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4ae86-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ae86-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4ae86-130">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="4ae86-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="4ae86-131">W okienku po lewej stronie **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4ae86-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="4ae86-132">W **tożsamość usługi ADD** okno dialogowe, następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="4ae86-132">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="4ae86-133">Wybierz istniejący plik układu *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4ae86-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="4ae86-134">Wybierz następujące pliki do zastąpienia:</span><span class="sxs-lookup"><span data-stu-id="4ae86-134">Select the following files to override:</span></span>
    * <span data-ttu-id="4ae86-135">**Konto/rejestrowanie**</span><span class="sxs-lookup"><span data-stu-id="4ae86-135">**Account/Register**</span></span>
    * <span data-ttu-id="4ae86-136">**Konto zarządzania/indeks**</span><span class="sxs-lookup"><span data-stu-id="4ae86-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="4ae86-137">Wybierz **+** przycisk, aby utworzyć nową **klasa kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="4ae86-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="4ae86-138">Akceptuje typu (**WebApp1.Models.WebApp1Context** Jeśli projekt nazwano **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="4ae86-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="4ae86-139">Wybierz **+** przycisk, aby utworzyć nową **klasy użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="4ae86-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="4ae86-140">Akceptuje typu (**WebApp1User** Jeśli projekt nazwano **WebApp1**) > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4ae86-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="4ae86-141">Wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="4ae86-141">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4ae86-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4ae86-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4ae86-143">Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:</span><span class="sxs-lookup"><span data-stu-id="4ae86-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="4ae86-144">Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do pliku projektu (.csproj).</span><span class="sxs-lookup"><span data-stu-id="4ae86-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="4ae86-145">Uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="4ae86-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="4ae86-146">Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="4ae86-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="4ae86-147">W folderze projektu Uruchom Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="4ae86-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="4ae86-148">Postępuj zgodnie z instrukcjami w [migracje, UseAuthentication i układ](xref:security/authentication/scaffold-identity#efm) można wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="4ae86-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="4ae86-149">Utwórz migracji i aktualizują bazę danych.</span><span class="sxs-lookup"><span data-stu-id="4ae86-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="4ae86-150">Add `UseAuthentication` to `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="4ae86-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="4ae86-151">Dodaj `<partial name="_LoginPartial" />` do pliku układu.</span><span class="sxs-lookup"><span data-stu-id="4ae86-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="4ae86-152">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4ae86-152">Test the app:</span></span>
  * <span data-ttu-id="4ae86-153">Rejestrowanie użytkownika</span><span class="sxs-lookup"><span data-stu-id="4ae86-153">Register a user</span></span>
  * <span data-ttu-id="4ae86-154">Wybierz nową nazwę użytkownika (obok **wylogowania** link).</span><span class="sxs-lookup"><span data-stu-id="4ae86-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="4ae86-155">Może być konieczne, rozwiń okno lub wybierz ikonę paska nawigacji, aby pokazać, nazwę użytkownika i inne łącza.</span><span class="sxs-lookup"><span data-stu-id="4ae86-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="4ae86-156">Wybierz **danych osobowych** kartę.</span><span class="sxs-lookup"><span data-stu-id="4ae86-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="4ae86-157">Wybierz **Pobierz** przycisk i zbadać *PersonalData.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="4ae86-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="4ae86-158">Test **Usuń** przycisk, który usuwa zalogowanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ae86-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="4ae86-159">Dodawanie danych niestandardowych użytkownika do bazy danych tożsamości</span><span class="sxs-lookup"><span data-stu-id="4ae86-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="4ae86-160">Aktualizacja `IdentityUser` pochodne klasy przy użyciu właściwości niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="4ae86-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="4ae86-161">Jeśli nazwę projektu WebApp1, plik nosi *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="4ae86-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="4ae86-162">Aktualizowanie pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="4ae86-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="4ae86-163">Właściwości z atrybutem [personaldata](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) są następujące:</span><span class="sxs-lookup"><span data-stu-id="4ae86-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="4ae86-164">Usuwane, gdy *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* wywołuje strony Razor `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="4ae86-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="4ae86-165">Uwzględnione w danych pobranych przez *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* strona Razor.</span><span class="sxs-lookup"><span data-stu-id="4ae86-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="4ae86-166">Aktualizuj stronę Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="4ae86-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="4ae86-167">Aktualizacja `InputModel` w *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="4ae86-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="4ae86-168">Aktualizacja *Areas/Identity/Pages/Account/Manage/Index.cshtml* z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="4ae86-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="4ae86-169">Aktualizacja *Areas/Identity/Pages/Account/Manage/Index.cshtml* z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="4ae86-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="4ae86-170">Aktualizuj stronę Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="4ae86-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="4ae86-171">Aktualizacja `InputModel` w *Areas/Identity/Pages/Account/Register.cshtml.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="4ae86-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="4ae86-172">Aktualizacja *Areas/Identity/Pages/Account/Register.cshtml* z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="4ae86-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="4ae86-173">Aktualizacja *Areas/Identity/Pages/Account/Register.cshtml* z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="4ae86-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="4ae86-174">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="4ae86-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="4ae86-175">Dodaj migrację danych niestandardowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="4ae86-175">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4ae86-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ae86-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4ae86-177">W programie Visual Studio **Konsola Menedżera pakietów**:</span><span class="sxs-lookup"><span data-stu-id="4ae86-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4ae86-178">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4ae86-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="4ae86-179">Test tworzenie, wyświetlanie, pobieranie i usuwanie danych niestandardowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="4ae86-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="4ae86-180">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4ae86-180">Test the app:</span></span>

* <span data-ttu-id="4ae86-181">Rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4ae86-181">Register a new user.</span></span>
* <span data-ttu-id="4ae86-182">Wyświetlanie danych niestandardowych użytkownika na `/Identity/Account/Manage` strony.</span><span class="sxs-lookup"><span data-stu-id="4ae86-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="4ae86-183">Pobieranie i wyświetlanie danych osobowych użytkowników z `/Identity/Account/Manage/PersonalData` strony.</span><span class="sxs-lookup"><span data-stu-id="4ae86-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
