---
title: Tworzenie szkieletu tożsamość w projektach programu ASP.NET Core
author: rick-anderson
description: Informacje o sposobie tworzenia szkieletu tożsamości w projektach programu ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 94ccfc8aa2ad37d89de42f276cb2f808a08cd55e
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090644"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Tworzenie szkieletu tożsamość w projektach programu ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Udostępnia platformy ASP.NET Core 2.1 i nowsze [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jako [biblioteki klas Razor](xref:razor-pages/ui-class). Aplikacje, które zawierają tożsamości można zastosować Generator szkieletu można selektywnie Dodawanie kodu źródłowego, znajdujących się w bibliotece klas Razor tożsamości (RCL). Przydatne może być wygenerowanie kodu źródłowego, aby można było zmodyfikować kod i zmienić zachowanie. Na przykład można nakazać Generatorowi szkieletu wygenerowanie kodu używanego podczas rejestracji. Wygenerowany kod ma pierwszeństwo przed tym samym kodem w bibliotece RCL tożsamości. Aby uzyskać pełną kontrolę nad interfejsu użytkownika i korzysta z domyślnego RCL, zobacz sekcję [Utwórz pełnej tożsamości interfejsu użytkownika źródło](#full).

Aplikacje, które wykonują **nie** obejmują uwierzytelniania można zastosować Generator szkieletu, aby dodać pakiet RCL tożsamości. Masz możliwość wyboru kodu tożsamości do wygenerowania.

Mimo, że Generator szkieletu generuje większość niezbędny kod, musisz zaktualizować projekt, aby ukończyć proces. W tym dokumencie wyjaśniono kroki niezbędne do ukończenia aktualizacji tworzenia szkieletów tożsamości.

Po uruchomieniu Generator szkieletu tożsamości *ScaffoldingReadme.txt* plik jest tworzony w katalogu projektu. *ScaffoldingReadme.txt* plik zawiera ogólne wskazówki, co jest potrzebne do ukończenia aktualizacji tworzenia szkieletów tożsamości. Ten dokument zawiera bardziej szczegółowe instrukcje niż *ScaffoldingReadme.txt* pliku.

Firma Microsoft zaleca korzystanie z systemu kontroli źródła, przedstawiono różnice w pliku, która umożliwia tworzenie kopii poza zmiany. Sprawdź, czy zmiany po uruchomieniu Generator szkieletu tożsamości.

> [!NOTE]
> Wymagane są usługi, korzystając z [uwierzytelnianie dwuskładnikowe](xref:security/authentication/identity-enable-qrcodes), [konta potwierdzenia i hasła odzyskiwania](xref:security/authentication/accconfirm)i inne funkcje zabezpieczeń przy użyciu tożsamości. Usługi lub wycinków usługi nie są generowane podczas tworzenia szkieletów tożsamości. Usługi w celu włączenia tych funkcji, należy dodać ręcznie. Na przykład zobacz [wymagają E-mail z potwierdzeniem](xref:security/authentication/accconfirm#require-email-confirmation).

## <a name="scaffold-identity-into-an-empty-project"></a>Tworzenie szkieletu identity do pustego projektu

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Dodaj następujący wyróżniony wywołania `Startup` klasy:

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Tożsamość szkieletu do projektu Razor bez autoryzacji istniejącej

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

Tożsamość jest skonfigurowana w *Areas/Identity/IdentityHostingStartup.cs*. Aby uzyskać więcej informacji, zobacz [interfejsu IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>Migracje, UseAuthentication i układ

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>Włączanie uwierzytelniania

W `Configure` metody `Startup` klasy, wywołaj [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Zmian układu

Opcjonalnie: Dodaj częściowego logowania (`_LoginPartial`) do pliku układu:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Tożsamość szkieletu do projektu Razor z autoryzacją

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth

dotnet new webapp -au Individual -o RPauth

dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Niektóre opcje tożsamości są konfigurowane w *Areas/Identity/IdentityHostingStartup.cs*. Aby uzyskać więcej informacji, zobacz [interfejsu IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Tożsamość szkieletu do projektu programu MVC, bez autoryzacji istniejącej

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

Opcjonalnie: Dodaj częściowego logowania (`_LoginPartial`) do *Views/Shared/_Layout.cshtml* pliku:

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Przenieś *Pages/Shared/_LoginPartial.cshtml* plik *Views/Shared/_LoginPartial.cshtml*

Tożsamość jest skonfigurowana w *Areas/Identity/IdentityHostingStartup.cs*. Aby uzyskać więcej informacji zobacz interfejsu IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Wywołaj [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Tożsamość szkieletu do projektu MVC z autoryzacją

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Usuń *stron/Shared* folder i pliki w tym folderze.

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>Tworzenie pełnej tożsamości interfejsu użytkownika źródła

Aby zachować pełną kontrolę nad tożsamości interfejsu użytkownika, należy uruchomić Generator szkieletu tożsamości i wybrać **Zastąp wszystkie pliki**.

Następujący wyróżniony kod pokazuje zmiany do zastąpienia domyślnej tożsamości interfejsu użytkownika przy użyciu tożsamości w aplikacji sieci web platformy ASP.NET Core 2.1. Można to zrobić, aby mieć pełną kontrolę nad tożsamości interfejsu użytkownika.

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

Wartość domyślna tożsamości zostanie zastąpiony w poniższym kodzie:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

Następujące zestawy kodów [ścieżki LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [ścieżka LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), i [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Zarejestruj `IEmailSender` implementacji, na przykład:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zmiany kodu uwierzytelniania do platformy ASP.NET Core 2.1 i nowsze](xref:migration/20_21#changes-to-authentication-code)
