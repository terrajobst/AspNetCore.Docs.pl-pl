---
title: Dodawanie, pobieranie i usuwanie danych użytkownika niestandardowego do tożsamości w projekcie platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodawać dane użytkownika niestandardowego do tożsamości w projekcie platformy ASP.NET Core. Usuń dane na GDPR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/add-user-data
ms.openlocfilehash: 23fd792c0d93c038f31ce947e7885ad6e36d119e
ms.sourcegitcommit: d4cefc0c63550c64a8040b11867cc05efcfb7e86
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34758802"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="813b4-104">Dodawanie, pobieranie i usuwanie danych użytkownika niestandardowego do tożsamości w projekcie platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="813b4-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="813b4-105">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="813b4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="813b4-106">W tym artykule przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="813b4-106">This article shows how to:</span></span>

* <span data-ttu-id="813b4-107">Dodawanie danych użytkownika do aplikacji sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="813b4-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="813b4-108">Dekoracji modelu danych użytkownika z [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atrybutu, więc jest automatycznie dostępny do pobrania i usuwanie.</span><span class="sxs-lookup"><span data-stu-id="813b4-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="813b4-109">Udostępniania danych może zostać pobrana i usunąć pomaga spełnić [GDPR](xref:security/gdpr) wymagania.</span><span class="sxs-lookup"><span data-stu-id="813b4-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="813b4-110">Przykładowy projekt został utworzony na podstawie stron Razor aplikacji sieci web, ale instrukcje są podobne dla aplikacji sieci web platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="813b4-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="813b4-111">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="813b4-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="813b4-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="813b4-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="813b4-113">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="813b4-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="813b4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="813b4-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="813b4-115">W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="813b4-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="813b4-116">Nazwij projekt **WebApp1** aby go odpowiada przestrzeni nazw z [pobieranie próbki](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) kodu.</span><span class="sxs-lookup"><span data-stu-id="813b4-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="813b4-117">Wybierz **aplikacji sieci Web platformy ASP.NET Core** > **OK**</span><span class="sxs-lookup"><span data-stu-id="813b4-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="813b4-118">Wybierz **platformy ASP.NET Core 2.1** na liście rozwijanej</span><span class="sxs-lookup"><span data-stu-id="813b4-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="813b4-119">Wybierz **aplikacji sieci Web**  > **OK**</span><span class="sxs-lookup"><span data-stu-id="813b4-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="813b4-120">Tworzenie i uruchamianie projektu.</span><span class="sxs-lookup"><span data-stu-id="813b4-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="813b4-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="813b4-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

------

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="813b4-122">Uruchamianie tworzenia szkieletu tożsamości</span><span class="sxs-lookup"><span data-stu-id="813b4-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="813b4-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="813b4-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="813b4-124">Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy na Projekt > **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="813b4-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="813b4-125">W lewym okienku **Dodawanie szkieletu** okno dialogowe, wybierz opcję **tożsamości** > **dodać**.</span><span class="sxs-lookup"><span data-stu-id="813b4-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="813b4-126">W **tożsamości Dodaj** okno dialogowe, następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="813b4-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="813b4-127">Wybierz istniejący plik układu *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="813b4-127">Select your existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="813b4-128">Wybierz do zastąpienia następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="813b4-128">Select the following files to override:</span></span>
    * <span data-ttu-id="813b4-129">**Rejestr/kont**</span><span class="sxs-lookup"><span data-stu-id="813b4-129">**Account/Register**</span></span>
    * <span data-ttu-id="813b4-130">**Konto/Zarządzanie/indeksu**</span><span class="sxs-lookup"><span data-stu-id="813b4-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="813b4-131">Wybierz **+** przycisk, aby utworzyć nowy **klasa kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="813b4-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="813b4-132">Akceptuje typu (**WebApp1.Models.WebApp1Context** Jeśli nazwę projektu **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="813b4-132">Accept the type (**WebApp1.Models.WebApp1Context** if you named the project **WebApp1**).</span></span>
  * <span data-ttu-id="813b4-133">Wybierz **+** przycisk, aby utworzyć nowy **klasy użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="813b4-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="813b4-134">Akceptuje typu (**WebApp1User** Jeśli nazwę projektu **WebApp1**) > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="813b4-134">Accept the type (**WebApp1User** if you named the project **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="813b4-135">Wybierz **dodać**.</span><span class="sxs-lookup"><span data-stu-id="813b4-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="813b4-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="813b4-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="813b4-137">Jeśli tworzenia szkieletu ASP.NET nie został wcześniej zainstalowany, zainstaluj go:</span><span class="sxs-lookup"><span data-stu-id="813b4-137">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="813b4-138">Dodaj odwołanie do pakietu [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do pliku projektu (.csproj).</span><span class="sxs-lookup"><span data-stu-id="813b4-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="813b4-139">Uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="813b4-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="813b4-140">Uruchom następujące polecenie, aby wyświetlić listę opcji tworzenia szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="813b4-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="813b4-141">W folderze projektu Uruchom tworzenia szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="813b4-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="813b4-142">Postępuj zgodnie z instrukcjami w [migracji, UseAuthentication i układ](xref:security/authentication/scaffold-identity#efm) można wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="813b4-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="813b4-143">Utwórz migracji i aktualizacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="813b4-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="813b4-144">Add `UseAuthentication` to `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="813b4-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="813b4-145">Dodaj `<partial name="_LoginPartial" />` do pliku układu.</span><span class="sxs-lookup"><span data-stu-id="813b4-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="813b4-146">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="813b4-146">Test the app:</span></span>
  * <span data-ttu-id="813b4-147">Zarejestruj użytkownika</span><span class="sxs-lookup"><span data-stu-id="813b4-147">Register a user</span></span>
  * <span data-ttu-id="813b4-148">Wybierz nową nazwę użytkownika (obok **wylogowania** link).</span><span class="sxs-lookup"><span data-stu-id="813b4-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="813b4-149">Może być konieczne, rozwiń okno lub wybierz ikony paska nawigacji, aby pokazać nazwy użytkownika i inne łącza.</span><span class="sxs-lookup"><span data-stu-id="813b4-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="813b4-150">Wybierz **danych osobowych** kartę.</span><span class="sxs-lookup"><span data-stu-id="813b4-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="813b4-151">Wybierz **Pobierz** przycisk i zbadane *PersonalData.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="813b4-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="813b4-152">Test **usunąć** przycisku, który usuwa zalogowanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="813b4-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="813b4-153">Dodaj użytkownika niestandardowego danych do bazy danych tożsamości</span><span class="sxs-lookup"><span data-stu-id="813b4-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="813b4-154">Aktualizacja `IdentityUser` klasy z właściwości niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="813b4-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="813b4-155">Jeśli nazwa projektu WebApp1 nosi nazwę pliku *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="813b4-155">If you named your project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="813b4-156">Aktualizacja pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="813b4-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="813b4-157">Właściwości ozdobione [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) są:</span><span class="sxs-lookup"><span data-stu-id="813b4-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="813b4-158">Po usunięciu *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* wywołuje Razor strony `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="813b4-158">Are deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="813b4-159">Uwzględnione w danych pobranych przez *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor strony.</span><span class="sxs-lookup"><span data-stu-id="813b4-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="813b4-160">Zaktualizuj strony Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="813b4-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="813b4-161">Aktualizacja `InputModel` w *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* z następującymi wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="813b4-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95)]

<span data-ttu-id="813b4-162">Aktualizacja *Areas/Identity/Pages/Account/Manage/Index.cshtml* z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="813b4-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="813b4-163">Zaktualizuj strony Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="813b4-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="813b4-164">Aktualizacja `InputModel` w *Areas/Identity/Pages/Account/Register.cshtml.cs* z następującymi wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="813b4-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="813b4-165">Aktualizacja *Areas/Identity/Pages/Account/Register.cshtml* z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="813b4-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="813b4-166">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="813b4-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="813b4-167">Dodaj migracji danych użytkownika niestandardowego</span><span class="sxs-lookup"><span data-stu-id="813b4-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="813b4-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="813b4-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="813b4-169">W programie Visual Studio **Konsola Menedżera pakietów**:</span><span class="sxs-lookup"><span data-stu-id="813b4-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="813b4-170">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="813b4-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="813b4-171">Test tworzenie, wyświetlanie, Pobierz, Usuń dane użytkownika niestandardowego</span><span class="sxs-lookup"><span data-stu-id="813b4-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="813b4-172">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="813b4-172">Test the app:</span></span>

* <span data-ttu-id="813b4-173">Zarejestruj nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="813b4-173">Register a new user.</span></span>
* <span data-ttu-id="813b4-174">Wyświetl dane użytkownika niestandardowego na `/Identity/Account/Manage` strony.</span><span class="sxs-lookup"><span data-stu-id="813b4-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="813b4-175">Pobierz i Wyświetl danych osobowych użytkowników z `/Identity/Account/Manage/PersonalData` strony.</span><span class="sxs-lookup"><span data-stu-id="813b4-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>