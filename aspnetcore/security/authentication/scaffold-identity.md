---
title: Tożsamość szkieletu w projektach ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak zidentyfikować szkielet w projekcie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: f3ae089d344d95ed84c9720ab4ba2c697400901e
ms.sourcegitcommit: dc96d76f6b231de59586fcbb989a7fb5106d26a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703771"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Tożsamość szkieletu w projektach ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 2,1 i nowsze zapewniają [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [Biblioteka klas Razor](xref:razor-pages/ui-class). Aplikacje zawierające tożsamość mogą zastosować szkielet, aby wybiórczo dodać kod źródłowy zawarty w bibliotece klas Razor (RCL). Przydatne może być wygenerowanie kodu źródłowego, aby można było zmodyfikować kod i zmienić zachowanie. Na przykład można nakazać Generatorowi szkieletu wygenerowanie kodu używanego podczas rejestracji. Wygenerowany kod ma pierwszeństwo przed tym samym kodem w bibliotece RCL tożsamości. Aby uzyskać pełną kontrolę nad interfejsem użytkownika i nie używać domyślnego RCL, zobacz sekcję [Tworzenie źródła interfejsu użytkownika pełnej tożsamości](#full).

Aplikacje, które **nie** obejmują uwierzytelniania, mogą zastosować szkieleter w celu dodania pakietu tożsamości RCL. Masz możliwość wyboru kodu tożsamości do wygenerowania.

Chociaż szkielet generuje większość niezbędnego kodu, należy zaktualizować projekt, aby ukończyć proces. W tym dokumencie opisano kroki niezbędne do ukończenia aktualizacji tworzenia szkieletu tożsamości.

Po uruchomieniu szkieletu tożsamości jest tworzony plik *ScaffoldingReadme. txt* w katalogu projektu. Plik *ScaffoldingReadme. txt* zawiera ogólne instrukcje dotyczące tego, co jest potrzebne do ukończenia aktualizacji tworzenia szkieletu tożsamości. Ten dokument zawiera pełniejsze instrukcje niż plik *ScaffoldingReadme. txt* .

Zalecamy używanie systemu kontroli źródła, który pokazuje różnice plików i pozwala na wycofanie zmian. Sprawdź zmiany po uruchomieniu szkieletu tożsamości.

> [!NOTE]
> Usługi są wymagane w przypadku korzystania z [uwierzytelniania dwuskładnikowego](xref:security/authentication/identity-enable-qrcodes), [potwierdzenia konta i odzyskiwania hasła](xref:security/authentication/accconfirm)oraz innych funkcji zabezpieczeń z tożsamością. Podczas tworzenia szkieletów tożsamości usługi lub usługi nie są generowane. Usługi umożliwiające włączenie tych funkcji należy dodać ręcznie. Na przykład zapoznaj się z tematem [Żądaj potwierdzenia wiadomości e-mail](xref:security/authentication/accconfirm#require-email-confirmation).

## <a name="scaffold-identity-into-an-empty-project"></a>Tożsamość szkieletowa do pustego projektu

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Dodaj następujące wyróżnione wywołania do klasy `Startup`:

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Tożsamość szkieletowa w projekcie Razor bez istniejącej autoryzacji

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

Tożsamość jest konfigurowana w *obszarach/Identity/IdentityHostingStartup. cs*. Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>Migracje, UseAuthentication i układ

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>Włącz uwierzytelnianie

W metodzie `Configure` klasy `Startup` Wywołaj [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Zmiany układu

Opcjonalnie: Dodaj część logowania częściowej (`_LoginPartial`) do pliku układu:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Tożsamość szkieletowa w projekcie Razor z autoryzacją

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Niektóre opcje tożsamości są konfigurowane w *obszarach/Identity/IdentityHostingStartup. cs*. Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Tożsamość szkieletowa do projektu MVC bez istniejącej autoryzacji

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

Opcjonalnie: Dodaj częściowe logowanie (`_LoginPartial`) do pliku *views/Shared/_Layout. cshtml* :

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Przenieś plik *Pages/Shared/_LoginPartial. cshtml* do *widoków/Shared/_LoginPartial. cshtml*

Tożsamość jest konfigurowana w *obszarach/Identity/IdentityHostingStartup. cs*. Aby uzyskać więcej informacji, zobacz IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Wywołaj [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Tożsamość szkieletowa do projektu MVC z autoryzacją

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Usuwanie *stron/folderów udostępnionych* i plików w tym folderze.

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>Utwórz źródło interfejsu użytkownika pełnej tożsamości

Aby zachować pełną kontrolę nad interfejsem użytkownika tożsamości, uruchom szkielet tożsamości i wybierz opcję **Zastąp wszystkie pliki**.

Poniższy wyróżniony kod przedstawia zmiany w celu zastąpienia domyślnego interfejsu użytkownika tożsamości tożsamością w aplikacji internetowej ASP.NET Core 2,1. Można to zrobić, aby mieć pełną kontrolę nad interfejsem użytkownika tożsamości.

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

Domyślna tożsamość jest zastępowana w następującym kodzie:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

Poniższy kod ustawia [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)i [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Zarejestruj implementację `IEmailSender`, na przykład:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zmiany w kodzie uwierzytelniania na ASP.NET Core 2,1 i nowsze](xref:migration/20_21#changes-to-authentication-code)
