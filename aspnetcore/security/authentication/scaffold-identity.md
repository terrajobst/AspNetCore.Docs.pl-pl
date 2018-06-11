---
title: Tożsamość szkieletu w projektów platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć szkielet tożsamości w projekcie platformy ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: e7a2cf3633ed48a0d2030739cdc092441fcae2ff
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252038"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Tożsamość szkieletu w projektów platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Udostępnia platformy ASP.NET Core 2.1 i nowsze [ASP.NET Core Identity](xref:security/authentication/identity) jako [biblioteki klas Razor](xref:mvc/razor-pages/ui-class). Aplikacje, które obejmują tożsamości można zastosować tworzenia szkieletu selektywnie dodać kod źródłowy zawartych w bibliotece klasy Razor tożsamości (RCL). Można wygenerować kodu źródłowego, aby można było zmodyfikować kod i zmiany zachowania. Na przykład można nakazać tworzenia szkieletu, aby wygenerować kod używany w rejestracji. Wygenerowany kod mają pierwszeństwo przed ten sam kod w RCL tożsamości.

Aplikacje, które wykonują **nie** obejmują uwierzytelniania można zastosować tworzenia szkieletu, aby dodać pakiet RCL tożsamości. Istnieje możliwość wybrania tożsamości kod zostanie wygenerowany.

Chociaż tworzenia szkieletu generuje większość niezbędne kodu, należy zaktualizować projekt, aby ukończyć proces. W tym dokumencie opisano kroki niezbędne do ukończenia aktualizacji szkieletów tożsamości.

Uruchomienie tworzenia szkieletu tożsamości *ScaffoldingReadme.txt* plik jest tworzony w katalogu projektu. *ScaffoldingReadme.txt* plik zawiera ogólne wskazówki, co jest potrzebne do ukończenia aktualizacji szkieletów tożsamości. Ten dokument zawiera bardziej szczegółowe instrukcje niż *ScaffoldingReadme.txt* pliku.

Zalecamy użycie systemu kontroli źródła, przedstawiono różnice pliku, który umożliwia tworzenie kopii poza zmiany. Sprawdź, czy zmiany po zakończeniu tworzenia szkieletu tożsamości.

## <a name="scaffold-identity-into-an-empty-project"></a>Szkieletu tożsamości do pustego projektu

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Dodaj następujący wyróżniony wywołania metody `Startup` klasy:

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Tożsamość szkieletu do projektu Razor bez autoryzacji istniejących

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

Tożsamość jest skonfigurowana w *Areas/Identity/IdentityHostingStartup.cs*. Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>Migracje, UseAuthentication i układu

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

W `Configure` metody `Startup` klasy, należy wywołać [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Zmian układu

Opcjonalnie: Dodaj częściowego logowania (`_LoginPartial`) do pliku układu:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Tożsamość szkieletu do projektu Razor z autoryzacji

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Niektóre opcje tożsamości są konfigurowane w *Areas/Identity/IdentityHostingStartup.cs*. Aby uzyskać więcej informacji, zobacz [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Tożsamość szkieletu do projektu programu MVC bez autoryzacji istniejących

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

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Przenieś *Pages/Shared/_LoginPartial.cshtml* pliku *Views/Shared/_LoginPartial.cshtml*

Tożsamość jest skonfigurowana w *Areas/Identity/IdentityHostingStartup.cs*. Aby uzyskać więcej informacji zobacz IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Wywołanie [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Tożsamość szkieletu do projektu programu MVC z autoryzacji

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Usuń *stron/Shared* folderów i plików w tym folderze.
