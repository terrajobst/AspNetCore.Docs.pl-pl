---
title: Tożsamość szkieletu w projektach ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zidentyfikować szkielet w projekcie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: security/authentication/scaffold-identity
ms.openlocfilehash: a0e9603cbca8c7f5771b0acf1a60839dffc89d4e
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146488"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="3533e-103">Tożsamość szkieletu w projektach ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3533e-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="3533e-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3533e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3533e-105">ASP.NET Core zapewnia [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [bibliotekę klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="3533e-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="3533e-106">Aplikacje zawierające tożsamość mogą zastosować szkielet, aby wybiórczo dodać kod źródłowy zawarty w bibliotece klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="3533e-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="3533e-107">Przydatne może być wygenerowanie kodu źródłowego, aby można było zmodyfikować kod i zmienić zachowanie.</span><span class="sxs-lookup"><span data-stu-id="3533e-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="3533e-108">Na przykład można nakazać Generatorowi szkieletu wygenerowanie kodu używanego podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="3533e-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="3533e-109">Wygenerowany kod ma pierwszeństwo przed tym samym kodem w bibliotece RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3533e-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="3533e-110">Aby uzyskać pełną kontrolę nad interfejsem użytkownika i nie używać domyślnego RCL, zobacz sekcję [Tworzenie źródła interfejsu użytkownika pełnej tożsamości](#full).</span><span class="sxs-lookup"><span data-stu-id="3533e-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="3533e-111">Aplikacje, które **nie** obejmują uwierzytelniania, mogą zastosować szkieleter w celu dodania pakietu tożsamości RCL.</span><span class="sxs-lookup"><span data-stu-id="3533e-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="3533e-112">Masz możliwość wyboru kodu tożsamości do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="3533e-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="3533e-113">Chociaż szkielet generuje większość niezbędnego kodu, należy zaktualizować projekt, aby ukończyć proces.</span><span class="sxs-lookup"><span data-stu-id="3533e-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="3533e-114">W tym dokumencie opisano kroki niezbędne do ukończenia aktualizacji tworzenia szkieletu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3533e-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="3533e-115">Zalecamy używanie systemu kontroli źródła, który pokazuje różnice plików i pozwala na wycofanie zmian.</span><span class="sxs-lookup"><span data-stu-id="3533e-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="3533e-116">Sprawdź zmiany po uruchomieniu szkieletu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3533e-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="3533e-117">Usługi są wymagane w przypadku korzystania z [uwierzytelniania dwuskładnikowego](xref:security/authentication/identity-enable-qrcodes), [potwierdzenia konta i odzyskiwania hasła](xref:security/authentication/accconfirm)oraz innych funkcji zabezpieczeń z tożsamością.</span><span class="sxs-lookup"><span data-stu-id="3533e-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="3533e-118">Podczas tworzenia szkieletów tożsamości usługi lub usługi nie są generowane.</span><span class="sxs-lookup"><span data-stu-id="3533e-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="3533e-119">Usługi umożliwiające włączenie tych funkcji należy dodać ręcznie.</span><span class="sxs-lookup"><span data-stu-id="3533e-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="3533e-120">Na przykład zapoznaj się z tematem [Żądaj potwierdzenia wiadomości e-mail](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="3533e-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="3533e-121">Ten dokument zawiera dokładniejsze instrukcje niż plik *ScaffoldingReadme. txt* , który jest generowany podczas uruchamiania szkieletu.</span><span class="sxs-lookup"><span data-stu-id="3533e-121">This document contains more complete instructions than the *ScaffoldingReadme.txt* file which is generated when running the the scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="3533e-122">Tożsamość szkieletowa do pustego projektu</span><span class="sxs-lookup"><span data-stu-id="3533e-122">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="3533e-123">Zaktualizuj klasę `Startup` przy użyciu kodu podobnego do poniższego:</span><span class="sxs-lookup"><span data-stu-id="3533e-123">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="3533e-124">Tożsamość szkieletowa w projekcie Razor bez istniejącej autoryzacji</span><span class="sxs-lookup"><span data-stu-id="3533e-124">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="3533e-125">Tożsamość jest konfigurowana w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="3533e-125">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="3533e-126">Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="3533e-126">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="3533e-127">Migracje, UseAuthentication i układ</span><span class="sxs-lookup"><span data-stu-id="3533e-127">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="3533e-128">Włączanie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="3533e-128">Enable authentication</span></span>

<span data-ttu-id="3533e-129">Zaktualizuj klasę `Startup` przy użyciu kodu podobnego do poniższego:</span><span class="sxs-lookup"><span data-stu-id="3533e-129">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="3533e-130">Zmiany układu</span><span class="sxs-lookup"><span data-stu-id="3533e-130">Layout changes</span></span>

<span data-ttu-id="3533e-131">Opcjonalnie: Dodaj częściową nazwę logowania (`_LoginPartial`) do pliku układu:</span><span class="sxs-lookup"><span data-stu-id="3533e-131">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="3533e-132">Tożsamość szkieletowa w projekcie Razor z autoryzacją</span><span class="sxs-lookup"><span data-stu-id="3533e-132">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="3533e-133">Niektóre opcje tożsamości są konfigurowane w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="3533e-133">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="3533e-134">Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="3533e-134">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="3533e-135">Tożsamość szkieletowa do projektu MVC bez istniejącej autoryzacji</span><span class="sxs-lookup"><span data-stu-id="3533e-135">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="3533e-136">Opcjonalnie: Dodaj część logowania częściowej (`_LoginPartial`) do pliku *viewss/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3533e-136">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="3533e-137">Przenieś plik *Pages/Shared/_LoginPartial. cshtml* do *widoków/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="3533e-137">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="3533e-138">Tożsamość jest konfigurowana w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="3533e-138">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="3533e-139">Aby uzyskać więcej informacji, zobacz IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="3533e-139">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="3533e-140">Zaktualizuj klasę `Startup` przy użyciu kodu podobnego do poniższego:</span><span class="sxs-lookup"><span data-stu-id="3533e-140">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="3533e-141">Tożsamość szkieletowa do projektu MVC z autoryzacją</span><span class="sxs-lookup"><span data-stu-id="3533e-141">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="3533e-142">Utwórz źródło interfejsu użytkownika pełnej tożsamości</span><span class="sxs-lookup"><span data-stu-id="3533e-142">Create full identity UI source</span></span>

<span data-ttu-id="3533e-143">Aby zachować pełną kontrolę nad interfejsem użytkownika tożsamości, uruchom szkielet tożsamości i wybierz opcję **Zastąp wszystkie pliki**.</span><span class="sxs-lookup"><span data-stu-id="3533e-143">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="3533e-144">Poniższy wyróżniony kod przedstawia zmiany w celu zastąpienia domyślnego interfejsu użytkownika tożsamości tożsamością w aplikacji internetowej ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="3533e-144">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="3533e-145">Można to zrobić, aby mieć pełną kontrolę nad interfejsem użytkownika tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3533e-145">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="3533e-146">Domyślna tożsamość jest zastępowana w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="3533e-146">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="3533e-147">Poniższy kod ustawia [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)i [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="3533e-147">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="3533e-148">Zarejestruj implementację `IEmailSender`, na przykład:</span><span class="sxs-lookup"><span data-stu-id="3533e-148">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="3533e-149">Wyłącz stronę rejestracji</span><span class="sxs-lookup"><span data-stu-id="3533e-149">Disable register page</span></span>

<span data-ttu-id="3533e-150">Aby wyłączyć rejestrację użytkownika:</span><span class="sxs-lookup"><span data-stu-id="3533e-150">To disable user registration:</span></span>

* <span data-ttu-id="3533e-151">Tożsamość szkieletu.</span><span class="sxs-lookup"><span data-stu-id="3533e-151">Scaffold Identity.</span></span> <span data-ttu-id="3533e-152">Uwzględnij konto. Register, Account. login i Account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="3533e-152">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="3533e-153">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3533e-153">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="3533e-154">Aktualizowanie *obszarów/tożsamości/stron/konta/register. cshtml. cs* , aby użytkownicy nie mogli zarejestrować się z tego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="3533e-154">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="3533e-155">Zaktualizuj *obszary/tożsamość/strony/account/register. cshtml* , aby były zgodne z poprzednimi zmianami:</span><span class="sxs-lookup"><span data-stu-id="3533e-155">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="3533e-156">Skomentuj lub Usuń link rejestracji z *obszarów/tożsamości/stron/konta/login. cshtml*</span><span class="sxs-lookup"><span data-stu-id="3533e-156">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="3533e-157">Zaktualizuj stronę *obszary/tożsamość/strony/konto/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="3533e-157">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="3533e-158">Usuń kod i linki z pliku cshtml.</span><span class="sxs-lookup"><span data-stu-id="3533e-158">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="3533e-159">Usuń kod potwierdzenia z `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="3533e-159">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="3533e-160">Dodawanie użytkowników przy użyciu innej aplikacji</span><span class="sxs-lookup"><span data-stu-id="3533e-160">Use another app to add users</span></span>

<span data-ttu-id="3533e-161">Zapewnianie mechanizmu dodawania użytkowników spoza aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3533e-161">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="3533e-162">Dostępne są następujące opcje dodawania użytkowników:</span><span class="sxs-lookup"><span data-stu-id="3533e-162">Options to add users include:</span></span>

* <span data-ttu-id="3533e-163">Dedykowana aplikacja sieci Web administratora.</span><span class="sxs-lookup"><span data-stu-id="3533e-163">A dedicated admin web app.</span></span>
* <span data-ttu-id="3533e-164">Aplikacja konsolowa.</span><span class="sxs-lookup"><span data-stu-id="3533e-164">A console app.</span></span>

<span data-ttu-id="3533e-165">Poniższy kod przedstawia jedno podejście do dodawania użytkowników:</span><span class="sxs-lookup"><span data-stu-id="3533e-165">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="3533e-166">Lista użytkowników jest odczytywana w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3533e-166">A list of users is read into memory.</span></span>
* <span data-ttu-id="3533e-167">Dla każdego użytkownika jest generowany silny unikatowy hasło.</span><span class="sxs-lookup"><span data-stu-id="3533e-167">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="3533e-168">Użytkownik zostanie dodany do bazy danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3533e-168">The user is added to the Identity database.</span></span>
* <span data-ttu-id="3533e-169">Użytkownik zostanie powiadomiony i zostanie poinformowany o zmianie hasła.</span><span class="sxs-lookup"><span data-stu-id="3533e-169">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="3533e-170">Poniższy kod zawiera opis dodawania użytkownika:</span><span class="sxs-lookup"><span data-stu-id="3533e-170">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="3533e-171">Podobne podejście może być stosowane w scenariuszach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="3533e-171">A similar approach can be followed for production scenarios.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="3533e-172">Zapobiegaj publikowaniu zasobów tożsamości statycznej</span><span class="sxs-lookup"><span data-stu-id="3533e-172">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="3533e-173">Aby zapobiec publikowaniu zasobów tożsamości statycznej do katalogu głównego sieci Web, zobacz <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="3533e-173">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3533e-174">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3533e-174">Additional resources</span></span>

* [<span data-ttu-id="3533e-175">Zmiany w kodzie uwierzytelniania na ASP.NET Core 2,1 i nowsze</span><span class="sxs-lookup"><span data-stu-id="3533e-175">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3533e-176">ASP.NET Core 2,1 i nowsze zapewniają [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [Biblioteka klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="3533e-176">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="3533e-177">Aplikacje zawierające tożsamość mogą zastosować szkielet, aby wybiórczo dodać kod źródłowy zawarty w bibliotece klas Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="3533e-177">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="3533e-178">Przydatne może być wygenerowanie kodu źródłowego, aby można było zmodyfikować kod i zmienić zachowanie.</span><span class="sxs-lookup"><span data-stu-id="3533e-178">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="3533e-179">Na przykład można nakazać Generatorowi szkieletu wygenerowanie kodu używanego podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="3533e-179">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="3533e-180">Wygenerowany kod ma pierwszeństwo przed tym samym kodem w bibliotece RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3533e-180">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="3533e-181">Aby uzyskać pełną kontrolę nad interfejsem użytkownika i nie używać domyślnego RCL, zobacz sekcję [Tworzenie źródła interfejsu użytkownika pełnej tożsamości](#full).</span><span class="sxs-lookup"><span data-stu-id="3533e-181">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="3533e-182">Aplikacje, które **nie** obejmują uwierzytelniania, mogą zastosować szkieleter w celu dodania pakietu tożsamości RCL.</span><span class="sxs-lookup"><span data-stu-id="3533e-182">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="3533e-183">Masz możliwość wyboru kodu tożsamości do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="3533e-183">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="3533e-184">Chociaż szkielet generuje większość niezbędnego kodu, należy zaktualizować projekt, aby ukończyć proces.</span><span class="sxs-lookup"><span data-stu-id="3533e-184">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="3533e-185">W tym dokumencie opisano kroki niezbędne do ukończenia aktualizacji tworzenia szkieletu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3533e-185">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="3533e-186">Po uruchomieniu szkieletu tożsamości jest tworzony plik *ScaffoldingReadme. txt* w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="3533e-186">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="3533e-187">Plik *ScaffoldingReadme. txt* zawiera ogólne instrukcje dotyczące tego, co jest potrzebne do ukończenia aktualizacji tworzenia szkieletu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3533e-187">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="3533e-188">Ten dokument zawiera pełniejsze instrukcje niż plik *ScaffoldingReadme. txt* .</span><span class="sxs-lookup"><span data-stu-id="3533e-188">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="3533e-189">Zalecamy używanie systemu kontroli źródła, który pokazuje różnice plików i pozwala na wycofanie zmian.</span><span class="sxs-lookup"><span data-stu-id="3533e-189">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="3533e-190">Sprawdź zmiany po uruchomieniu szkieletu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3533e-190">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="3533e-191">Usługi są wymagane w przypadku korzystania z [uwierzytelniania dwuskładnikowego](xref:security/authentication/identity-enable-qrcodes), [potwierdzenia konta i odzyskiwania hasła](xref:security/authentication/accconfirm)oraz innych funkcji zabezpieczeń z tożsamością.</span><span class="sxs-lookup"><span data-stu-id="3533e-191">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="3533e-192">Podczas tworzenia szkieletów tożsamości usługi lub usługi nie są generowane.</span><span class="sxs-lookup"><span data-stu-id="3533e-192">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="3533e-193">Usługi umożliwiające włączenie tych funkcji należy dodać ręcznie.</span><span class="sxs-lookup"><span data-stu-id="3533e-193">Services to enable these features must be added manually.</span></span> <span data-ttu-id="3533e-194">Na przykład zapoznaj się z tematem [Żądaj potwierdzenia wiadomości e-mail](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="3533e-194">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="3533e-195">Tożsamość szkieletowa do pustego projektu</span><span class="sxs-lookup"><span data-stu-id="3533e-195">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="3533e-196">Dodaj następujące wyróżnione wywołania do klasy `Startup`:</span><span class="sxs-lookup"><span data-stu-id="3533e-196">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="3533e-197">Tożsamość szkieletowa w projekcie Razor bez istniejącej autoryzacji</span><span class="sxs-lookup"><span data-stu-id="3533e-197">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="3533e-198">Tożsamość jest konfigurowana w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="3533e-198">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="3533e-199">Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="3533e-199">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="3533e-200">Migracje, UseAuthentication i układ</span><span class="sxs-lookup"><span data-stu-id="3533e-200">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="3533e-201">Włączanie uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="3533e-201">Enable authentication</span></span>

<span data-ttu-id="3533e-202">W metodzie `Configure` klasy `Startup` Wywołaj [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="3533e-202">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="3533e-203">Zmiany układu</span><span class="sxs-lookup"><span data-stu-id="3533e-203">Layout changes</span></span>

<span data-ttu-id="3533e-204">Opcjonalnie: Dodaj częściową nazwę logowania (`_LoginPartial`) do pliku układu:</span><span class="sxs-lookup"><span data-stu-id="3533e-204">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="3533e-205">Tożsamość szkieletowa w projekcie Razor z autoryzacją</span><span class="sxs-lookup"><span data-stu-id="3533e-205">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="3533e-206">Niektóre opcje tożsamości są konfigurowane w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="3533e-206">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="3533e-207">Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="3533e-207">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="3533e-208">Tożsamość szkieletowa do projektu MVC bez istniejącej autoryzacji</span><span class="sxs-lookup"><span data-stu-id="3533e-208">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="3533e-209">Opcjonalnie: Dodaj część logowania częściowej (`_LoginPartial`) do pliku *viewss/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="3533e-209">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="3533e-210">Przenieś plik *Pages/Shared/_LoginPartial. cshtml* do *widoków/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="3533e-210">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="3533e-211">Tożsamość jest konfigurowana w *obszarach/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="3533e-211">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="3533e-212">Aby uzyskać więcej informacji, zobacz IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="3533e-212">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="3533e-213">Wywołaj [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="3533e-213">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="3533e-214">Tożsamość szkieletowa do projektu MVC z autoryzacją</span><span class="sxs-lookup"><span data-stu-id="3533e-214">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="3533e-215">Usuwanie *stron/folderów udostępnionych* i plików w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="3533e-215">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="3533e-216">Utwórz źródło interfejsu użytkownika pełnej tożsamości</span><span class="sxs-lookup"><span data-stu-id="3533e-216">Create full identity UI source</span></span>

<span data-ttu-id="3533e-217">Aby zachować pełną kontrolę nad interfejsem użytkownika tożsamości, uruchom szkielet tożsamości i wybierz opcję **Zastąp wszystkie pliki**.</span><span class="sxs-lookup"><span data-stu-id="3533e-217">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="3533e-218">Poniższy wyróżniony kod przedstawia zmiany w celu zastąpienia domyślnego interfejsu użytkownika tożsamości tożsamością w aplikacji internetowej ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="3533e-218">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="3533e-219">Można to zrobić, aby mieć pełną kontrolę nad interfejsem użytkownika tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3533e-219">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="3533e-220">Domyślna tożsamość jest zastępowana w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="3533e-220">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="3533e-221">Poniższy kod ustawia [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)i [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="3533e-221">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="3533e-222">Zarejestruj implementację `IEmailSender`, na przykład:</span><span class="sxs-lookup"><span data-stu-id="3533e-222">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="3533e-223">Wyłącz stronę rejestracji</span><span class="sxs-lookup"><span data-stu-id="3533e-223">Disable register page</span></span>

<span data-ttu-id="3533e-224">Aby wyłączyć rejestrację użytkownika:</span><span class="sxs-lookup"><span data-stu-id="3533e-224">To disable user registration:</span></span>

* <span data-ttu-id="3533e-225">Tożsamość szkieletu.</span><span class="sxs-lookup"><span data-stu-id="3533e-225">Scaffold Identity.</span></span> <span data-ttu-id="3533e-226">Uwzględnij konto. Register, Account. login i Account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="3533e-226">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="3533e-227">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3533e-227">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="3533e-228">Aktualizowanie *obszarów/tożsamości/stron/konta/register. cshtml. cs* , aby użytkownicy nie mogli zarejestrować się z tego punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="3533e-228">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="3533e-229">Zaktualizuj *obszary/tożsamość/strony/account/register. cshtml* , aby były zgodne z poprzednimi zmianami:</span><span class="sxs-lookup"><span data-stu-id="3533e-229">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="3533e-230">Skomentuj lub Usuń link rejestracji z *obszarów/tożsamości/stron/konta/login. cshtml*</span><span class="sxs-lookup"><span data-stu-id="3533e-230">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="3533e-231">Zaktualizuj stronę *obszary/tożsamość/strony/konto/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="3533e-231">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="3533e-232">Usuń kod i linki z pliku cshtml.</span><span class="sxs-lookup"><span data-stu-id="3533e-232">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="3533e-233">Usuń kod potwierdzenia z `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="3533e-233">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="3533e-234">Dodawanie użytkowników przy użyciu innej aplikacji</span><span class="sxs-lookup"><span data-stu-id="3533e-234">Use another app to add users</span></span>

<span data-ttu-id="3533e-235">Zapewnianie mechanizmu dodawania użytkowników spoza aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3533e-235">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="3533e-236">Dostępne są następujące opcje dodawania użytkowników:</span><span class="sxs-lookup"><span data-stu-id="3533e-236">Options to add users include:</span></span>

* <span data-ttu-id="3533e-237">Dedykowana aplikacja sieci Web administratora.</span><span class="sxs-lookup"><span data-stu-id="3533e-237">A dedicated admin web app.</span></span>
* <span data-ttu-id="3533e-238">Aplikacja konsolowa.</span><span class="sxs-lookup"><span data-stu-id="3533e-238">A console app.</span></span>

<span data-ttu-id="3533e-239">Poniższy kod przedstawia jedno podejście do dodawania użytkowników:</span><span class="sxs-lookup"><span data-stu-id="3533e-239">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="3533e-240">Lista użytkowników jest odczytywana w pamięci.</span><span class="sxs-lookup"><span data-stu-id="3533e-240">A list of users is read into memory.</span></span>
* <span data-ttu-id="3533e-241">Dla każdego użytkownika jest generowany silny unikatowy hasło.</span><span class="sxs-lookup"><span data-stu-id="3533e-241">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="3533e-242">Użytkownik zostanie dodany do bazy danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3533e-242">The user is added to the Identity database.</span></span>
* <span data-ttu-id="3533e-243">Użytkownik zostanie powiadomiony i zostanie poinformowany o zmianie hasła.</span><span class="sxs-lookup"><span data-stu-id="3533e-243">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="3533e-244">Poniższy kod zawiera opis dodawania użytkownika:</span><span class="sxs-lookup"><span data-stu-id="3533e-244">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="3533e-245">Podobne podejście może być stosowane w scenariuszach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="3533e-245">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3533e-246">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3533e-246">Additional resources</span></span>

* [<span data-ttu-id="3533e-247">Zmiany w kodzie uwierzytelniania na ASP.NET Core 2,1 i nowsze</span><span class="sxs-lookup"><span data-stu-id="3533e-247">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end