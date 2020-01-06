---
title: Tożsamość szkieletu w projektach ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zidentyfikować szkielet w projekcie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 2432d346d9678157848a38fa01d9057cdd7503ff
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75356301"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="b61c1-103">Tożsamość szkieletu w projektach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b61c1-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="b61c1-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b61c1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b61c1-105">ASP.NET Core zapewnia [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [bibliotekę klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="b61c1-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="b61c1-106">Aplikacje zawierające tożsamość mogą zastosować szkielet, aby wybiórczo dodać kod źródłowy zawarty w bibliotece klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="b61c1-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="b61c1-107">Przydatne może być wygenerowanie kodu źródłowego, aby można było zmodyfikować kod i zmienić zachowanie.</span><span class="sxs-lookup"><span data-stu-id="b61c1-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="b61c1-108">Na przykład można nakazać Generatorowi szkieletu wygenerowanie kodu używanego podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="b61c1-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="b61c1-109">Wygenerowany kod ma pierwszeństwo przed tym samym kodem w bibliotece RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b61c1-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="b61c1-110">Aby uzyskać pełną kontrolę nad interfejsem użytkownika i nie używać domyślnego RCL, zobacz sekcję [Tworzenie źródła interfejsu użytkownika pełnej tożsamości](#full).</span><span class="sxs-lookup"><span data-stu-id="b61c1-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="b61c1-111">Aplikacje, które **nie** obejmują uwierzytelniania, mogą zastosować szkieleter w celu dodania pakietu tożsamości RCL.</span><span class="sxs-lookup"><span data-stu-id="b61c1-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="b61c1-112">Masz możliwość wyboru kodu tożsamości do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="b61c1-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="b61c1-113">Chociaż szkielet generuje większość niezbędnego kodu, należy zaktualizować projekt, aby ukończyć proces.</span><span class="sxs-lookup"><span data-stu-id="b61c1-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="b61c1-114">W tym dokumencie opisano kroki niezbędne do ukończenia aktualizacji tworzenia szkieletu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b61c1-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="b61c1-115">Zalecamy używanie systemu kontroli źródła, który pokazuje różnice plików i pozwala na wycofanie zmian.</span><span class="sxs-lookup"><span data-stu-id="b61c1-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="b61c1-116">Sprawdź zmiany po uruchomieniu szkieletu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b61c1-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="b61c1-117">Usługi są wymagane w przypadku korzystania z [uwierzytelniania dwuskładnikowego](xref:security/authentication/identity-enable-qrcodes), [potwierdzenia konta i odzyskiwania hasła](xref:security/authentication/accconfirm)oraz innych funkcji zabezpieczeń z tożsamością.</span><span class="sxs-lookup"><span data-stu-id="b61c1-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="b61c1-118">Podczas tworzenia szkieletów tożsamości usługi lub usługi nie są generowane.</span><span class="sxs-lookup"><span data-stu-id="b61c1-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="b61c1-119">Usługi umożliwiające włączenie tych funkcji należy dodać ręcznie.</span><span class="sxs-lookup"><span data-stu-id="b61c1-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="b61c1-120">Na przykład zapoznaj się z tematem [Żądaj potwierdzenia wiadomości e-mail](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="b61c1-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="b61c1-121">Ten dokument zawiera dokładniejsze instrukcje niż plik *ScaffoldingReadme. txt* , który jest generowany podczas uruchamiania szkieletu.</span><span class="sxs-lookup"><span data-stu-id="b61c1-121">This document contains more complete instructions than the *ScaffoldingReadme.txt* file which is generated when running the the scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="b61c1-122">Tożsamość szkieletowa do pustego projektu</span><span class="sxs-lookup"><span data-stu-id="b61c1-122">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="b61c1-123">Zaktualizuj klasę `Startup` przy użyciu kodu podobnego do poniższego:</span><span class="sxs-lookup"><span data-stu-id="b61c1-123">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="b61c1-124">Tożsamość szkieletowa w projekcie Razor bez istniejącej autoryzacji</span><span class="sxs-lookup"><span data-stu-id="b61c1-124">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef database drop
dotnet ef migrations add CreateIdentitySchema0
dotnet ef database update
-->

<!-- ERROR
There is already an object named 'AspNetRoles' in the database.

Fixed via dotnet ef database drop
before dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="b61c1-125">Tożsamość jest konfigurowana w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="b61c1-125">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b61c1-126">Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="b61c1-126">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="b61c1-127">Migracje, UseAuthentication i układ</span><span class="sxs-lookup"><span data-stu-id="b61c1-127">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="b61c1-128">Włączanie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="b61c1-128">Enable authentication</span></span>

<span data-ttu-id="b61c1-129">Zaktualizuj klasę `Startup` przy użyciu kodu podobnego do poniższego:</span><span class="sxs-lookup"><span data-stu-id="b61c1-129">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="b61c1-130">Zmiany układu</span><span class="sxs-lookup"><span data-stu-id="b61c1-130">Layout changes</span></span>

<span data-ttu-id="b61c1-131">Opcjonalnie: Dodaj częściową nazwę logowania (`_LoginPartial`) do pliku układu:</span><span class="sxs-lookup"><span data-stu-id="b61c1-131">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="b61c1-132">Tożsamość szkieletowa w projekcie Razor z autoryzacją</span><span class="sxs-lookup"><span data-stu-id="b61c1-132">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="b61c1-133">Niektóre opcje tożsamości są konfigurowane w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="b61c1-133">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b61c1-134">Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="b61c1-134">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="b61c1-135">Tożsamość szkieletowa do projektu MVC bez istniejącej autoryzacji</span><span class="sxs-lookup"><span data-stu-id="b61c1-135">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="b61c1-136">Opcjonalnie: Dodaj część logowania częściowej (`_LoginPartial`) do pliku *viewss/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="b61c1-136">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="b61c1-137">Przenieś plik *Pages/Shared/_LoginPartial. cshtml* do *widoków/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="b61c1-137">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="b61c1-138">Tożsamość jest konfigurowana w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="b61c1-138">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b61c1-139">Aby uzyskać więcej informacji, zobacz IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="b61c1-139">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="b61c1-140">Zaktualizuj klasę `Startup` przy użyciu kodu podobnego do poniższego:</span><span class="sxs-lookup"><span data-stu-id="b61c1-140">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="b61c1-141">Tożsamość szkieletowa do projektu MVC z autoryzacją</span><span class="sxs-lookup"><span data-stu-id="b61c1-141">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="b61c1-142">Utwórz źródło interfejsu użytkownika pełnej tożsamości</span><span class="sxs-lookup"><span data-stu-id="b61c1-142">Create full identity UI source</span></span>

<span data-ttu-id="b61c1-143">Aby zachować pełną kontrolę nad interfejsem użytkownika tożsamości, uruchom szkielet tożsamości i wybierz opcję **Zastąp wszystkie pliki**.</span><span class="sxs-lookup"><span data-stu-id="b61c1-143">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="b61c1-144">Poniższy wyróżniony kod przedstawia zmiany w celu zastąpienia domyślnego interfejsu użytkownika tożsamości tożsamością w aplikacji internetowej ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="b61c1-144">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="b61c1-145">Można to zrobić, aby mieć pełną kontrolę nad interfejsem użytkownika tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b61c1-145">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="b61c1-146">Domyślna tożsamość jest zastępowana w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="b61c1-146">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="b61c1-147">Poniższy kod ustawia [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)i [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="b61c1-147">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="b61c1-148">Zarejestruj implementację `IEmailSender`, na przykład:</span><span class="sxs-lookup"><span data-stu-id="b61c1-148">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="b61c1-149">Wyłącz stronę rejestracji</span><span class="sxs-lookup"><span data-stu-id="b61c1-149">Disable register page</span></span>

<span data-ttu-id="b61c1-150">Aby wyłączyć rejestrację użytkownika:</span><span class="sxs-lookup"><span data-stu-id="b61c1-150">To disable user registration:</span></span>

* <span data-ttu-id="b61c1-151">Tożsamość szkieletu.</span><span class="sxs-lookup"><span data-stu-id="b61c1-151">Scaffold Identity.</span></span> <span data-ttu-id="b61c1-152">Uwzględnij konto. Register, Account. login i Account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="b61c1-152">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="b61c1-153">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b61c1-153">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="b61c1-154">Aktualizowanie *obszarów/tożsamości/stron/konta/register. cshtml. cs* , aby użytkownicy nie mogli zarejestrować się z tego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="b61c1-154">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="b61c1-155">Zaktualizuj *obszary/tożsamość/strony/account/register. cshtml* , aby były zgodne z poprzednimi zmianami:</span><span class="sxs-lookup"><span data-stu-id="b61c1-155">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="b61c1-156">Skomentuj lub Usuń link rejestracji z *obszarów/tożsamości/stron/konta/login. cshtml*</span><span class="sxs-lookup"><span data-stu-id="b61c1-156">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="b61c1-157">Zaktualizuj stronę *obszary/tożsamość/strony/konto/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="b61c1-157">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="b61c1-158">Usuń kod i linki z pliku cshtml.</span><span class="sxs-lookup"><span data-stu-id="b61c1-158">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="b61c1-159">Usuń kod potwierdzenia z `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="b61c1-159">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="b61c1-160">Dodawanie użytkowników przy użyciu innej aplikacji</span><span class="sxs-lookup"><span data-stu-id="b61c1-160">Use another app to add users</span></span>

<span data-ttu-id="b61c1-161">Zapewnianie mechanizmu dodawania użytkowników spoza aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b61c1-161">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="b61c1-162">Dostępne są następujące opcje dodawania użytkowników:</span><span class="sxs-lookup"><span data-stu-id="b61c1-162">Options to add users include:</span></span>

* <span data-ttu-id="b61c1-163">Dedykowana aplikacja sieci Web administratora.</span><span class="sxs-lookup"><span data-stu-id="b61c1-163">A dedicated admin web app.</span></span>
* <span data-ttu-id="b61c1-164">Aplikacja konsolowa.</span><span class="sxs-lookup"><span data-stu-id="b61c1-164">A console app.</span></span>

<span data-ttu-id="b61c1-165">Poniższy kod przedstawia jedno podejście do dodawania użytkowników:</span><span class="sxs-lookup"><span data-stu-id="b61c1-165">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="b61c1-166">Lista użytkowników jest odczytywana w pamięci.</span><span class="sxs-lookup"><span data-stu-id="b61c1-166">A list of users is read into memory.</span></span>
* <span data-ttu-id="b61c1-167">Dla każdego użytkownika jest generowany silny unikatowy hasło.</span><span class="sxs-lookup"><span data-stu-id="b61c1-167">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="b61c1-168">Użytkownik zostanie dodany do bazy danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b61c1-168">The user is added to the Identity database.</span></span>
* <span data-ttu-id="b61c1-169">Użytkownik zostanie powiadomiony i zostanie poinformowany o zmianie hasła.</span><span class="sxs-lookup"><span data-stu-id="b61c1-169">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="b61c1-170">Poniższy kod zawiera opis dodawania użytkownika:</span><span class="sxs-lookup"><span data-stu-id="b61c1-170">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="b61c1-171">Podobne podejście może być stosowane w scenariuszach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="b61c1-171">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b61c1-172">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b61c1-172">Additional resources</span></span>

* [<span data-ttu-id="b61c1-173">Zmiany w kodzie uwierzytelniania na ASP.NET Core 2,1 i nowsze</span><span class="sxs-lookup"><span data-stu-id="b61c1-173">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b61c1-174">ASP.NET Core 2,1 i nowsze zapewniają [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [Biblioteka klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="b61c1-174">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="b61c1-175">Aplikacje zawierające tożsamość mogą zastosować szkielet, aby wybiórczo dodać kod źródłowy zawarty w bibliotece klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="b61c1-175">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="b61c1-176">Przydatne może być wygenerowanie kodu źródłowego, aby można było zmodyfikować kod i zmienić zachowanie.</span><span class="sxs-lookup"><span data-stu-id="b61c1-176">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="b61c1-177">Na przykład można nakazać Generatorowi szkieletu wygenerowanie kodu używanego podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="b61c1-177">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="b61c1-178">Wygenerowany kod ma pierwszeństwo przed tym samym kodem w bibliotece RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b61c1-178">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="b61c1-179">Aby uzyskać pełną kontrolę nad interfejsem użytkownika i nie używać domyślnego RCL, zobacz sekcję [Tworzenie źródła interfejsu użytkownika pełnej tożsamości](#full).</span><span class="sxs-lookup"><span data-stu-id="b61c1-179">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="b61c1-180">Aplikacje, które **nie** obejmują uwierzytelniania, mogą zastosować szkieleter w celu dodania pakietu tożsamości RCL.</span><span class="sxs-lookup"><span data-stu-id="b61c1-180">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="b61c1-181">Masz możliwość wyboru kodu tożsamości do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="b61c1-181">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="b61c1-182">Chociaż szkielet generuje większość niezbędnego kodu, należy zaktualizować projekt, aby ukończyć proces.</span><span class="sxs-lookup"><span data-stu-id="b61c1-182">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="b61c1-183">W tym dokumencie opisano kroki niezbędne do ukończenia aktualizacji tworzenia szkieletu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b61c1-183">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="b61c1-184">Po uruchomieniu szkieletu tożsamości jest tworzony plik *ScaffoldingReadme. txt* w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="b61c1-184">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="b61c1-185">Plik *ScaffoldingReadme. txt* zawiera ogólne instrukcje dotyczące tego, co jest potrzebne do ukończenia aktualizacji tworzenia szkieletu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b61c1-185">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="b61c1-186">Ten dokument zawiera pełniejsze instrukcje niż plik *ScaffoldingReadme. txt* .</span><span class="sxs-lookup"><span data-stu-id="b61c1-186">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="b61c1-187">Zalecamy używanie systemu kontroli źródła, który pokazuje różnice plików i pozwala na wycofanie zmian.</span><span class="sxs-lookup"><span data-stu-id="b61c1-187">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="b61c1-188">Sprawdź zmiany po uruchomieniu szkieletu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b61c1-188">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="b61c1-189">Usługi są wymagane w przypadku korzystania z [uwierzytelniania dwuskładnikowego](xref:security/authentication/identity-enable-qrcodes), [potwierdzenia konta i odzyskiwania hasła](xref:security/authentication/accconfirm)oraz innych funkcji zabezpieczeń z tożsamością.</span><span class="sxs-lookup"><span data-stu-id="b61c1-189">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="b61c1-190">Podczas tworzenia szkieletów tożsamości usługi lub usługi nie są generowane.</span><span class="sxs-lookup"><span data-stu-id="b61c1-190">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="b61c1-191">Usługi umożliwiające włączenie tych funkcji należy dodać ręcznie.</span><span class="sxs-lookup"><span data-stu-id="b61c1-191">Services to enable these features must be added manually.</span></span> <span data-ttu-id="b61c1-192">Na przykład zapoznaj się z tematem [Żądaj potwierdzenia wiadomości e-mail](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="b61c1-192">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="b61c1-193">Tożsamość szkieletowa do pustego projektu</span><span class="sxs-lookup"><span data-stu-id="b61c1-193">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="b61c1-194">Dodaj następujące wyróżnione wywołania do klasy `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b61c1-194">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="b61c1-195">Tożsamość szkieletowa w projekcie Razor bez istniejącej autoryzacji</span><span class="sxs-lookup"><span data-stu-id="b61c1-195">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="b61c1-196">Tożsamość jest konfigurowana w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="b61c1-196">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b61c1-197">Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="b61c1-197">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="b61c1-198">Migracje, UseAuthentication i układ</span><span class="sxs-lookup"><span data-stu-id="b61c1-198">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="b61c1-199">Włączanie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="b61c1-199">Enable authentication</span></span>

<span data-ttu-id="b61c1-200">W metodzie `Configure` klasy `Startup` Wywołaj [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="b61c1-200">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="b61c1-201">Zmiany układu</span><span class="sxs-lookup"><span data-stu-id="b61c1-201">Layout changes</span></span>

<span data-ttu-id="b61c1-202">Opcjonalnie: Dodaj częściową nazwę logowania (`_LoginPartial`) do pliku układu:</span><span class="sxs-lookup"><span data-stu-id="b61c1-202">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="b61c1-203">Tożsamość szkieletowa w projekcie Razor z autoryzacją</span><span class="sxs-lookup"><span data-stu-id="b61c1-203">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="b61c1-204">Niektóre opcje tożsamości są konfigurowane w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="b61c1-204">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b61c1-205">Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="b61c1-205">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="b61c1-206">Tożsamość szkieletowa do projektu MVC bez istniejącej autoryzacji</span><span class="sxs-lookup"><span data-stu-id="b61c1-206">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="b61c1-207">Opcjonalnie: Dodaj część logowania częściowej (`_LoginPartial`) do pliku *viewss/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="b61c1-207">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="b61c1-208">Przenieś plik *Pages/Shared/_LoginPartial. cshtml* do *widoków/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="b61c1-208">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="b61c1-209">Tożsamość jest konfigurowana w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="b61c1-209">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="b61c1-210">Aby uzyskać więcej informacji, zobacz IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="b61c1-210">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="b61c1-211">Wywołaj [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="b61c1-211">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="b61c1-212">Tożsamość szkieletowa do projektu MVC z autoryzacją</span><span class="sxs-lookup"><span data-stu-id="b61c1-212">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="b61c1-213">Usuwanie *stron/folderów udostępnionych* i plików w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="b61c1-213">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="b61c1-214">Utwórz źródło interfejsu użytkownika pełnej tożsamości</span><span class="sxs-lookup"><span data-stu-id="b61c1-214">Create full identity UI source</span></span>

<span data-ttu-id="b61c1-215">Aby zachować pełną kontrolę nad interfejsem użytkownika tożsamości, uruchom szkielet tożsamości i wybierz opcję **Zastąp wszystkie pliki**.</span><span class="sxs-lookup"><span data-stu-id="b61c1-215">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="b61c1-216">Poniższy wyróżniony kod przedstawia zmiany w celu zastąpienia domyślnego interfejsu użytkownika tożsamości tożsamością w aplikacji internetowej ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="b61c1-216">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="b61c1-217">Można to zrobić, aby mieć pełną kontrolę nad interfejsem użytkownika tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b61c1-217">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="b61c1-218">Domyślna tożsamość jest zastępowana w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="b61c1-218">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="b61c1-219">Poniższy kod ustawia [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)i [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="b61c1-219">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="b61c1-220">Zarejestruj implementację `IEmailSender`, na przykład:</span><span class="sxs-lookup"><span data-stu-id="b61c1-220">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="b61c1-221">Wyłącz stronę rejestracji</span><span class="sxs-lookup"><span data-stu-id="b61c1-221">Disable register page</span></span>

<span data-ttu-id="b61c1-222">Aby wyłączyć rejestrację użytkownika:</span><span class="sxs-lookup"><span data-stu-id="b61c1-222">To disable user registration:</span></span>

* <span data-ttu-id="b61c1-223">Tożsamość szkieletu.</span><span class="sxs-lookup"><span data-stu-id="b61c1-223">Scaffold Identity.</span></span> <span data-ttu-id="b61c1-224">Uwzględnij konto. Register, Account. login i Account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="b61c1-224">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="b61c1-225">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b61c1-225">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="b61c1-226">Aktualizowanie *obszarów/tożsamości/stron/konta/register. cshtml. cs* , aby użytkownicy nie mogli zarejestrować się z tego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="b61c1-226">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="b61c1-227">Zaktualizuj *obszary/tożsamość/strony/account/register. cshtml* , aby były zgodne z poprzednimi zmianami:</span><span class="sxs-lookup"><span data-stu-id="b61c1-227">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="b61c1-228">Skomentuj lub Usuń link rejestracji z *obszarów/tożsamości/stron/konta/login. cshtml*</span><span class="sxs-lookup"><span data-stu-id="b61c1-228">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="b61c1-229">Zaktualizuj stronę *obszary/tożsamość/strony/konto/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="b61c1-229">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="b61c1-230">Usuń kod i linki z pliku cshtml.</span><span class="sxs-lookup"><span data-stu-id="b61c1-230">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="b61c1-231">Usuń kod potwierdzenia z `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="b61c1-231">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="b61c1-232">Dodawanie użytkowników przy użyciu innej aplikacji</span><span class="sxs-lookup"><span data-stu-id="b61c1-232">Use another app to add users</span></span>

<span data-ttu-id="b61c1-233">Zapewnianie mechanizmu dodawania użytkowników spoza aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b61c1-233">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="b61c1-234">Dostępne są następujące opcje dodawania użytkowników:</span><span class="sxs-lookup"><span data-stu-id="b61c1-234">Options to add users include:</span></span>

* <span data-ttu-id="b61c1-235">Dedykowana aplikacja sieci Web administratora.</span><span class="sxs-lookup"><span data-stu-id="b61c1-235">A dedicated admin web app.</span></span>
* <span data-ttu-id="b61c1-236">Aplikacja konsolowa.</span><span class="sxs-lookup"><span data-stu-id="b61c1-236">A console app.</span></span>

<span data-ttu-id="b61c1-237">Poniższy kod przedstawia jedno podejście do dodawania użytkowników:</span><span class="sxs-lookup"><span data-stu-id="b61c1-237">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="b61c1-238">Lista użytkowników jest odczytywana w pamięci.</span><span class="sxs-lookup"><span data-stu-id="b61c1-238">A list of users is read into memory.</span></span>
* <span data-ttu-id="b61c1-239">Dla każdego użytkownika jest generowany silny unikatowy hasło.</span><span class="sxs-lookup"><span data-stu-id="b61c1-239">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="b61c1-240">Użytkownik zostanie dodany do bazy danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="b61c1-240">The user is added to the Identity database.</span></span>
* <span data-ttu-id="b61c1-241">Użytkownik zostanie powiadomiony i zostanie poinformowany o zmianie hasła.</span><span class="sxs-lookup"><span data-stu-id="b61c1-241">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="b61c1-242">Poniższy kod zawiera opis dodawania użytkownika:</span><span class="sxs-lookup"><span data-stu-id="b61c1-242">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="b61c1-243">Podobne podejście może być stosowane w scenariuszach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="b61c1-243">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b61c1-244">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b61c1-244">Additional resources</span></span>

* [<span data-ttu-id="b61c1-245">Zmiany w kodzie uwierzytelniania na ASP.NET Core 2,1 i nowsze</span><span class="sxs-lookup"><span data-stu-id="b61c1-245">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end