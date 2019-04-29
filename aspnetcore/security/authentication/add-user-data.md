---
title: Dodawanie, pobieranie i usuwanie danych tożsamości użytkownika w projekcie platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać danych niestandardowych użytkownika w tożsamości w projektach programu ASP.NET Core. Usuń dane na RODO.
ms.author: riande
ms.date: 6/16/2018
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 1b7cac9206776e9290326511247b6db33f13cb32
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165096"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="2cdfd-104">Dodawanie, pobieranie i usuwanie danych niestandardowych użytkownika w tożsamości w projektach programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2cdfd-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="2cdfd-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2cdfd-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2cdfd-106">W tym artykule przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-106">This article shows how to:</span></span>

* <span data-ttu-id="2cdfd-107">Dodawanie danych niestandardowych użytkownika do aplikacji sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="2cdfd-108">Dekoracji modelu danych niestandardowych użytkownika za pomocą [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atrybutu, więc jest automatycznie dostępne do pobrania i usuwania.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="2cdfd-109">Udostępnianie danych może zostać pobrana i usunąć pomaga spełnić wymagania [RODO](xref:security/gdpr) wymagania.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="2cdfd-110">Przykładowy projekt jest tworzony na podstawie aplikacja internetowa ze stronami Razor, ale instrukcje są podobne dla aplikacji sieci web platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="2cdfd-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2cdfd-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2cdfd-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="2cdfd-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.2-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="2cdfd-113">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="2cdfd-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2cdfd-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cdfd-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2cdfd-115">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="2cdfd-116">Nadaj projektowi nazwę **WebApp1** Jeśli chcesz go odpowiada przestrzeni nazw z [Pobierz przykładowe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) kodu.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="2cdfd-117">Wybierz **aplikacji sieci Web platformy ASP.NET Core** > **OK**</span><span class="sxs-lookup"><span data-stu-id="2cdfd-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="2cdfd-118">Wybierz **platformy ASP.NET Core 2.2** na liście rozwijanej</span><span class="sxs-lookup"><span data-stu-id="2cdfd-118">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="2cdfd-119">Wybierz **aplikacji sieci Web**  > **OK**</span><span class="sxs-lookup"><span data-stu-id="2cdfd-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="2cdfd-120">Skompiluj i uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2cdfd-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2cdfd-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="2cdfd-122">Uruchom Generator szkieletu tożsamości</span><span class="sxs-lookup"><span data-stu-id="2cdfd-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2cdfd-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cdfd-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2cdfd-124">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="2cdfd-125">W okienku po lewej stronie **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="2cdfd-126">W **tożsamość usługi ADD** okno dialogowe, następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="2cdfd-127">Wybierz istniejący plik układu *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2cdfd-127">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="2cdfd-128">Wybierz następujące pliki do zastąpienia:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-128">Select the following files to override:</span></span>
    * <span data-ttu-id="2cdfd-129">**Konto/rejestrowanie**</span><span class="sxs-lookup"><span data-stu-id="2cdfd-129">**Account/Register**</span></span>
    * <span data-ttu-id="2cdfd-130">**Konto zarządzania/indeks**</span><span class="sxs-lookup"><span data-stu-id="2cdfd-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="2cdfd-131">Wybierz **+** przycisk, aby utworzyć nową **klasa kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="2cdfd-132">Akceptuje typu (**WebApp1.Models.WebApp1Context** Jeśli projekt nazwano **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="2cdfd-132">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="2cdfd-133">Wybierz **+** przycisk, aby utworzyć nową **klasy użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="2cdfd-134">Akceptuje typu (**WebApp1User** Jeśli projekt nazwano **WebApp1**) > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-134">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="2cdfd-135">Wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2cdfd-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2cdfd-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2cdfd-137">Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-137">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="2cdfd-138">Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do pliku projektu (.csproj).</span><span class="sxs-lookup"><span data-stu-id="2cdfd-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="2cdfd-139">Uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="2cdfd-140">Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="2cdfd-141">W folderze projektu Uruchom Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="2cdfd-142">Postępuj zgodnie z instrukcjami w [migracje, UseAuthentication i układ](xref:security/authentication/scaffold-identity#efm) można wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="2cdfd-143">Utwórz migracji i aktualizują bazę danych.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="2cdfd-144">Add `UseAuthentication` to `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="2cdfd-145">Dodaj `<partial name="_LoginPartial" />` do pliku układu.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="2cdfd-146">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-146">Test the app:</span></span>
  * <span data-ttu-id="2cdfd-147">Rejestrowanie użytkownika</span><span class="sxs-lookup"><span data-stu-id="2cdfd-147">Register a user</span></span>
  * <span data-ttu-id="2cdfd-148">Wybierz nową nazwę użytkownika (obok **wylogowania** link).</span><span class="sxs-lookup"><span data-stu-id="2cdfd-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="2cdfd-149">Może być konieczne, rozwiń okno lub wybierz ikonę paska nawigacji, aby pokazać, nazwę użytkownika i inne łącza.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="2cdfd-150">Wybierz **danych osobowych** kartę.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="2cdfd-151">Wybierz **Pobierz** przycisk i zbadać *PersonalData.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="2cdfd-152">Test **Usuń** przycisk, który usuwa zalogowanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="2cdfd-153">Dodawanie danych niestandardowych użytkownika do bazy danych tożsamości</span><span class="sxs-lookup"><span data-stu-id="2cdfd-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="2cdfd-154">Aktualizacja `IdentityUser` pochodne klasy przy użyciu właściwości niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="2cdfd-155">Jeśli nazwę projektu WebApp1, plik nosi *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-155">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="2cdfd-156">Aktualizowanie pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="2cdfd-157">Właściwości ozdobione [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="2cdfd-158">Usuwane, gdy *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* wywołuje strony Razor `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="2cdfd-159">Uwzględnione w danych pobranych przez *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* strona Razor.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="2cdfd-160">Aktualizuj stronę Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="2cdfd-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="2cdfd-161">Aktualizacja `InputModel` w *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="2cdfd-162">Aktualizacja *Areas/Identity/Pages/Account/Manage/Index.cshtml* z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="2cdfd-163">Aktualizuj stronę Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="2cdfd-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="2cdfd-164">Aktualizacja `InputModel` w *Areas/Identity/Pages/Account/Register.cshtml.cs* przy użyciu następujących wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="2cdfd-165">Aktualizacja *Areas/Identity/Pages/Account/Register.cshtml* z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="2cdfd-166">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="2cdfd-167">Dodaj migrację danych niestandardowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="2cdfd-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2cdfd-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cdfd-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2cdfd-169">W programie Visual Studio **Konsola Menedżera pakietów**:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2cdfd-170">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2cdfd-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="2cdfd-171">Test tworzenie, wyświetlanie, pobieranie i usuwanie danych niestandardowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="2cdfd-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="2cdfd-172">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="2cdfd-172">Test the app:</span></span>

* <span data-ttu-id="2cdfd-173">Rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-173">Register a new user.</span></span>
* <span data-ttu-id="2cdfd-174">Wyświetlanie danych niestandardowych użytkownika na `/Identity/Account/Manage` strony.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="2cdfd-175">Pobieranie i wyświetlanie danych osobowych użytkowników z `/Identity/Account/Manage/PersonalData` strony.</span><span class="sxs-lookup"><span data-stu-id="2cdfd-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
