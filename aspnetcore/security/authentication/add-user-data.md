---
title: Dodawanie, pobieranie i usuwanie danych tożsamości użytkownika w projekcie platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodać danych niestandardowych użytkownika w tożsamości w projektach programu ASP.NET Core. Usuń dane na RODO.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 7a67f55da0e685ed3fd5badb30e8be683411a5ae
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662688"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="7d522-104">Dodawanie, pobieranie i usuwanie danych niestandardowych użytkownika w tożsamości w projektach programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d522-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="7d522-105">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d522-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7d522-106">W tym artykule przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="7d522-106">This article shows how to:</span></span>

* <span data-ttu-id="7d522-107">Dodawanie danych niestandardowych użytkownika do aplikacji sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d522-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="7d522-108">Oznacz niestandardowy model danych użytkownika przy użyciu atrybutu <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute>, aby był on automatycznie dostępny do pobrania i usunięcia.</span><span class="sxs-lookup"><span data-stu-id="7d522-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="7d522-109">Aby można było pobrać i usunąć dane, można spełnić wymagania [Rodo](xref:security/gdpr) .</span><span class="sxs-lookup"><span data-stu-id="7d522-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="7d522-110">Przykładowy projekt jest tworzony na podstawie aplikacja internetowa ze stronami Razor, ale instrukcje są podobne dla aplikacji sieci web platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="7d522-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="7d522-111">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7d522-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d522-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7d522-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="7d522-113">Tworzenie aplikacji sieci web Razor</span><span class="sxs-lookup"><span data-stu-id="7d522-113">Create a Razor web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="7d522-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d522-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="7d522-115">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt** > .</span><span class="sxs-lookup"><span data-stu-id="7d522-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="7d522-116">Nadaj projektowi nazwę **WebApp1** , jeśli chcesz dopasować ją do przestrzeni nazw [przykładowego kodu pobierania](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .</span><span class="sxs-lookup"><span data-stu-id="7d522-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="7d522-117">Wybierz **ASP.NET Core aplikację sieci Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="7d522-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="7d522-118">Na liście rozwijanej wybierz pozycję **ASP.NET Core 3,0**</span><span class="sxs-lookup"><span data-stu-id="7d522-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="7d522-119">Wybierz **aplikację sieci Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="7d522-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="7d522-120">Skompiluj i uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="7d522-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="7d522-121">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt** > .</span><span class="sxs-lookup"><span data-stu-id="7d522-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="7d522-122">Nadaj projektowi nazwę **WebApp1** , jeśli chcesz dopasować ją do przestrzeni nazw [przykładowego kodu pobierania](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .</span><span class="sxs-lookup"><span data-stu-id="7d522-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="7d522-123">Wybierz **ASP.NET Core aplikację sieci Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="7d522-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="7d522-124">Na liście rozwijanej wybierz pozycję **ASP.NET Core 2,2**</span><span class="sxs-lookup"><span data-stu-id="7d522-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="7d522-125">Wybierz **aplikację sieci Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="7d522-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="7d522-126">Skompiluj i uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="7d522-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-cli"></a>[<span data-ttu-id="7d522-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7d522-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="7d522-128">Uruchom Generator szkieletu tożsamości</span><span class="sxs-lookup"><span data-stu-id="7d522-128">Run the Identity scaffolder</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="7d522-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d522-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7d522-130">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt > **Dodaj** > **nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="7d522-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="7d522-131">W lewym okienku okna dialogowego **Dodawanie szkieletu** wybierz pozycję **tożsamość** > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7d522-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="7d522-132">W oknie dialogowym **Dodawanie tożsamości** można wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7d522-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="7d522-133">Wybierz istniejący plik układu *~/Pages/Shared/_Layout. cshtml*</span><span class="sxs-lookup"><span data-stu-id="7d522-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="7d522-134">Wybierz następujące pliki do zastąpienia:</span><span class="sxs-lookup"><span data-stu-id="7d522-134">Select the following files to override:</span></span>
    * <span data-ttu-id="7d522-135">**Konto/rejestr**</span><span class="sxs-lookup"><span data-stu-id="7d522-135">**Account/Register**</span></span>
    * <span data-ttu-id="7d522-136">**Konto/Zarządzanie/indeks**</span><span class="sxs-lookup"><span data-stu-id="7d522-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="7d522-137">Wybierz przycisk **+** , aby utworzyć nową **klasę kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="7d522-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="7d522-138">Zaakceptuj typ (**WebApp1. models. WebApp1Context** , jeśli projekt jest nazwany **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="7d522-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="7d522-139">Wybierz przycisk **+** , aby utworzyć nową **klasę użytkownika**.</span><span class="sxs-lookup"><span data-stu-id="7d522-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="7d522-140">Zaakceptuj typ (**WebApp1User** , jeśli projekt ma nazwę **WebApp1**) > **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7d522-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="7d522-141">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7d522-141">Select **Add**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="7d522-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7d522-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7d522-143">Jeśli nie zainstalowano wcześniej Generator szkieletu ASP.NET Core, zainstalowanie go teraz:</span><span class="sxs-lookup"><span data-stu-id="7d522-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="7d522-144">Dodaj odwołanie do pakietu do [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do pliku projektu (. csproj).</span><span class="sxs-lookup"><span data-stu-id="7d522-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="7d522-145">Uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="7d522-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="7d522-146">Uruchom następujące polecenie, aby wyświetlić listę opcji Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="7d522-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="7d522-147">W folderze projektu Uruchom Generator szkieletu tożsamości:</span><span class="sxs-lookup"><span data-stu-id="7d522-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="7d522-148">Postępuj zgodnie z instrukcjami w sekcji [migracje, UseAuthentication i układ](xref:security/authentication/scaffold-identity#efm) , aby wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7d522-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="7d522-149">Utwórz migracji i aktualizują bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7d522-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="7d522-150">Add `UseAuthentication` to `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="7d522-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="7d522-151">Dodaj `<partial name="_LoginPartial" />` do pliku układu.</span><span class="sxs-lookup"><span data-stu-id="7d522-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="7d522-152">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7d522-152">Test the app:</span></span>
  * <span data-ttu-id="7d522-153">Rejestrowanie użytkownika</span><span class="sxs-lookup"><span data-stu-id="7d522-153">Register a user</span></span>
  * <span data-ttu-id="7d522-154">Wybierz nową nazwę użytkownika (obok linku **wylogowania** ).</span><span class="sxs-lookup"><span data-stu-id="7d522-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="7d522-155">Może być konieczne, rozwiń okno lub wybierz ikonę paska nawigacji, aby pokazać, nazwę użytkownika i inne łącza.</span><span class="sxs-lookup"><span data-stu-id="7d522-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="7d522-156">Wybierz kartę **dane osobowe** .</span><span class="sxs-lookup"><span data-stu-id="7d522-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="7d522-157">Wybierz przycisk **Pobierz** i zbadaj plik *personaldata. JSON* .</span><span class="sxs-lookup"><span data-stu-id="7d522-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="7d522-158">Przetestuj przycisk **Usuń** , który spowoduje usunięcie zalogowanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7d522-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="7d522-159">Dodawanie danych niestandardowych użytkownika do bazy danych tożsamości</span><span class="sxs-lookup"><span data-stu-id="7d522-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="7d522-160">Zaktualizuj klasę pochodną `IdentityUser` przy użyciu właściwości niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="7d522-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="7d522-161">Jeśli nazwa projektu WebApp1, plik ma nazwę *obszary/tożsamość/Data/WebApp1User. cs*.</span><span class="sxs-lookup"><span data-stu-id="7d522-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="7d522-162">Aktualizowanie pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7d522-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="7d522-163">Właściwości z atrybutem [personaldata](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) są następujące:</span><span class="sxs-lookup"><span data-stu-id="7d522-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="7d522-164">Usunięte, gdy strona Razor *obszary/tożsamość/Pages/Account/Manage/DeletePersonalData. cshtml* jest wywoływana `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="7d522-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="7d522-165">Zawarte w pobranych danych przez stronę *obszary/tożsamość/strony/konto/Zarządzanie/DownloadPersonalData. cshtml* Razor.</span><span class="sxs-lookup"><span data-stu-id="7d522-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="7d522-166">Aktualizuj stronę Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="7d522-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="7d522-167">Zaktualizuj `InputModel` w *obszarze obszary/tożsamość/strony/konto/Zarządzanie/index. cshtml. cs* przy użyciu następującego wyróżnionego kodu:</span><span class="sxs-lookup"><span data-stu-id="7d522-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="7d522-168">Zaktualizuj *obszary/tożsamość/strony/konto/Zarządzanie/index. cshtml* przy użyciu następującego wyróżnionego znacznika:</span><span class="sxs-lookup"><span data-stu-id="7d522-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="7d522-169">Zaktualizuj *obszary/tożsamość/strony/konto/Zarządzanie/index. cshtml* przy użyciu następującego wyróżnionego znacznika:</span><span class="sxs-lookup"><span data-stu-id="7d522-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="7d522-170">Aktualizuj stronę Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="7d522-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="7d522-171">Zaktualizuj `InputModel` w *obszarach/Identity/Pages/Account/register. cshtml. cs* przy użyciu następującego wyróżnionego kodu:</span><span class="sxs-lookup"><span data-stu-id="7d522-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="7d522-172">Zaktualizuj *obszary/tożsamość/strony/account/register. cshtml* przy użyciu następującego wyróżnionego znacznika:</span><span class="sxs-lookup"><span data-stu-id="7d522-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="7d522-173">Zaktualizuj *obszary/tożsamość/strony/account/register. cshtml* przy użyciu następującego wyróżnionego znacznika:</span><span class="sxs-lookup"><span data-stu-id="7d522-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="7d522-174">Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="7d522-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="7d522-175">Dodaj migrację danych niestandardowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="7d522-175">Add a migration for the custom user data</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="7d522-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d522-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7d522-177">W **konsoli Menedżera pakietów**programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7d522-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="7d522-178">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="7d522-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="7d522-179">Test tworzenie, wyświetlanie, pobieranie i usuwanie danych niestandardowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="7d522-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="7d522-180">Testowanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7d522-180">Test the app:</span></span>

* <span data-ttu-id="7d522-181">Rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7d522-181">Register a new user.</span></span>
* <span data-ttu-id="7d522-182">Wyświetlanie niestandardowych danych użytkownika na stronie `/Identity/Account/Manage`.</span><span class="sxs-lookup"><span data-stu-id="7d522-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="7d522-183">Pobierz i Przejrzyj dane osobowe użytkowników na stronie `/Identity/Account/Manage/PersonalData`.</span><span class="sxs-lookup"><span data-stu-id="7d522-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
