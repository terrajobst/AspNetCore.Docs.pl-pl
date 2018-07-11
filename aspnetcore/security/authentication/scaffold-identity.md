---
title: Tworzenie szkieletu tożsamość w projektach programu ASP.NET Core
author: rick-anderson
description: Informacje o sposobie tworzenia szkieletu tożsamości w projektach programu ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: cf6544d8b671f026c8466fa8dff506027b64cf1f
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38217685"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="2d83b-103">Tworzenie szkieletu tożsamość w projektach programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2d83b-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="2d83b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2d83b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2d83b-105">Udostępnia platformy ASP.NET Core 2.1 i nowsze [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jako [biblioteki klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="2d83b-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="2d83b-106">Aplikacje, które zawierają tożsamości można zastosować Generator szkieletu można selektywnie Dodawanie kodu źródłowego, znajdujących się w bibliotece klas Razor tożsamości (RCL).</span><span class="sxs-lookup"><span data-stu-id="2d83b-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="2d83b-107">Można wygenerować kod źródłowy, aby można było zmodyfikować kod i zmienić zachowanie.</span><span class="sxs-lookup"><span data-stu-id="2d83b-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="2d83b-108">Na przykład można nakazać Generator szkieletu do generowania kodu, używane podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="2d83b-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="2d83b-109">Wygenerowany kod mają pierwszeństwo przed ten sam kod w RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="2d83b-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="2d83b-110">Aby uzyskać pełną kontrolę nad interfejsu użytkownika i korzysta z domyślnego RCL, zobacz sekcję [Utwórz pełnej tożsamości interfejsu użytkownika źródło](#full).</span><span class="sxs-lookup"><span data-stu-id="2d83b-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="2d83b-111">Aplikacje, które wykonują **nie** obejmują uwierzytelniania można zastosować Generator szkieletu, aby dodać pakiet RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="2d83b-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="2d83b-112">Masz możliwość wyboru tożsamości kodu do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="2d83b-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="2d83b-113">Mimo, że Generator szkieletu generuje większość niezbędny kod, musisz zaktualizować projekt, aby ukończyć proces.</span><span class="sxs-lookup"><span data-stu-id="2d83b-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="2d83b-114">W tym dokumencie wyjaśniono kroki niezbędne do ukończenia aktualizacji tworzenia szkieletów tożsamości.</span><span class="sxs-lookup"><span data-stu-id="2d83b-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="2d83b-115">Po uruchomieniu Generator szkieletu tożsamości *ScaffoldingReadme.txt* plik jest tworzony w katalogu projektu.</span><span class="sxs-lookup"><span data-stu-id="2d83b-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="2d83b-116">*ScaffoldingReadme.txt* plik zawiera ogólne wskazówki, co jest potrzebne do ukończenia aktualizacji tworzenia szkieletów tożsamości.</span><span class="sxs-lookup"><span data-stu-id="2d83b-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="2d83b-117">Ten dokument zawiera bardziej szczegółowe instrukcje niż *ScaffoldingReadme.txt* pliku.</span><span class="sxs-lookup"><span data-stu-id="2d83b-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="2d83b-118">Firma Microsoft zaleca korzystanie z systemu kontroli źródła, przedstawiono różnice w pliku, która umożliwia tworzenie kopii poza zmiany.</span><span class="sxs-lookup"><span data-stu-id="2d83b-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="2d83b-119">Sprawdź, czy zmiany po uruchomieniu Generator szkieletu tożsamości.</span><span class="sxs-lookup"><span data-stu-id="2d83b-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="2d83b-120">Tworzenie szkieletu identity do pustego projektu</span><span class="sxs-lookup"><span data-stu-id="2d83b-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="2d83b-121">Dodaj następujący wyróżniony wywołania `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="2d83b-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="2d83b-122">Tożsamość szkieletu do projektu Razor bez autoryzacji istniejącej</span><span class="sxs-lookup"><span data-stu-id="2d83b-122">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
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

<span data-ttu-id="2d83b-123">Tożsamość jest skonfigurowana w *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2d83b-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="2d83b-124">Aby uzyskać więcej informacji, zobacz [interfejsu IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="2d83b-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="2d83b-125">Migracje, UseAuthentication i układ</span><span class="sxs-lookup"><span data-stu-id="2d83b-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="2d83b-126">W `Configure` metody `Startup` klasy, wywołaj [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="2d83b-126">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="2d83b-127">Zmian układu</span><span class="sxs-lookup"><span data-stu-id="2d83b-127">Layout changes</span></span>

<span data-ttu-id="2d83b-128">Opcjonalnie: Dodaj częściowego logowania (`_LoginPartial`) do pliku układu:</span><span class="sxs-lookup"><span data-stu-id="2d83b-128">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="2d83b-129">Tożsamość szkieletu do projektu Razor z autoryzacją</span><span class="sxs-lookup"><span data-stu-id="2d83b-129">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]<span data-ttu-id="2d83b-130"> Niektóre opcje tożsamości są konfigurowane w *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2d83b-130"> Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="2d83b-131">Aby uzyskać więcej informacji, zobacz [interfejsu IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="2d83b-131">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="2d83b-132">Tożsamość szkieletu do projektu programu MVC, bez autoryzacji istniejącej</span><span class="sxs-lookup"><span data-stu-id="2d83b-132">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="2d83b-133">Opcjonalnie: Dodaj częściowego logowania (`_LoginPartial`) do *Views/Shared/_Layout.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="2d83b-133">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="2d83b-134">Przenieś *Pages/Shared/_LoginPartial.cshtml* plik *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2d83b-134">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="2d83b-135">Tożsamość jest skonfigurowana w *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2d83b-135">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="2d83b-136">Aby uzyskać więcej informacji zobacz interfejsu IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="2d83b-136">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="2d83b-137">Wywołaj [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="2d83b-137">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="2d83b-138">Tożsamość szkieletu do projektu MVC z autoryzacją</span><span class="sxs-lookup"><span data-stu-id="2d83b-138">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="2d83b-139">Usuń *stron/Shared* folder i pliki w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="2d83b-139">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="2d83b-140">Tworzenie pełnej tożsamości interfejsu użytkownika źródła</span><span class="sxs-lookup"><span data-stu-id="2d83b-140">Create full identity UI source</span></span>

<span data-ttu-id="2d83b-141">Aby zachować pełną kontrolę nad tożsamości interfejsu użytkownika, należy uruchomić Generator szkieletu tożsamości i wybrać **Zastąp wszystkie pliki**.</span><span class="sxs-lookup"><span data-stu-id="2d83b-141">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="2d83b-142">Następujący wyróżniony kod pokazuje zmiany do zastąpienia domyślnej tożsamości interfejsu użytkownika przy użyciu tożsamości w aplikacji sieci web platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2d83b-142">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="2d83b-143">Można to zrobić, aby mieć pełną kontrolę nad tożsamości interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2d83b-143">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="2d83b-144">Wartość domyślna tożsamości zostanie zastąpiony w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="2d83b-144">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="2d83b-145">Następujące zestawy kodów [ścieżki LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [ścieżka LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), i [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="2d83b-145">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="2d83b-146">Zarejestruj `IEmailSender` implementacji, na przykład:</span><span class="sxs-lookup"><span data-stu-id="2d83b-146">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
