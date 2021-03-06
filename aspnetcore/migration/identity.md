---
title: Migrowanie uwierzytelniania i tożsamości do ASP.NET Core
author: ardalis
description: Dowiedz się, jak migrować uwierzytelnianie i tożsamość z projektu ASP.NET MVC do projektu MVC ASP.NET Core.
ms.author: riande
ms.date: 3/22/2020
uid: migration/identity
ms.openlocfilehash: c5727c974e455144d04e66fe14ea591e160cb963
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219197"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Migrowanie uwierzytelniania i tożsamości do ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

W poprzednim artykule [przeprowadzono migrację konfiguracji z projektu ASP.NET MVC do ASP.NET Core MVC](xref:migration/configuration). W tym artykule przeprowadzimy migrację funkcji rejestracji, logowania i zarządzania użytkownikami.

## <a name="configure-identity-and-membership"></a>Konfigurowanie tożsamości i członkostwa

W ASP.NET MVC funkcje uwierzytelniania i tożsamości są konfigurowane przy użyciu ASP.NET Identity w *Startup.auth.cs* i *IdentityConfig.cs*, które znajdują się w folderze *App_Start* . W ASP.NET Core MVC te funkcje są konfigurowane w *Startup.cs*.

Zainstaluj następujące pakiety NuGet:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`
* `Microsoft.AspNetCore.Authentication.Cookies`
* `Microsoft.EntityFrameworkCore.SqlServer`

W *Startup.cs*zaktualizuj metodę `Startup.ConfigureServices`, aby użyć usług Entity Framework i Identity Services:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

W tym momencie istnieją dwa typy, do których odwołuje się powyższy kod, który nie został jeszcze zmigrowany z projektu ASP.NET MVC: `ApplicationDbContext` i `ApplicationUser`. Utwórz nowy folder *models* w projekcie ASP.NET Core i Dodaj do niego dwie klasy odpowiadające tym typom. Wersje ASP.NET MVC tych klas można znaleźć w */models/IdentityModels.cs*, ale w zmigrowanym projekcie będziemy używać jednego pliku dla każdej klasy, ponieważ jest to bardziej jasne.

*ApplicationUser.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Core Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Core Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

Projekt sieci Web ASP.NET Core MVC Starter nie obejmuje znacznie dostosowywania użytkowników ani `ApplicationDbContext`. Podczas migrowania prawdziwej aplikacji należy również migrować wszystkie niestandardowe właściwości i metody klas użytkownika i `DbContext` aplikacji, a także inne klasy modelu używane przez aplikację. Na przykład jeśli `DbContext` ma `DbSet<Album>`, należy przeprowadzić migrację klasy `Album`.

W przypadku tych plików można wykonać plik *Startup.cs* , aby skompilować go przez zaktualizowanie jego instrukcji `using`:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Nasza aplikacja jest teraz gotowa do obsługi uwierzytelniania i usług tożsamości. Wystarczy, że te funkcje są dostępne dla użytkowników.

## <a name="migrate-registration-and-login-logic"></a>Migrowanie logiki rejestracji i logowania

Za pomocą usług Identity Services skonfigurowanych dla aplikacji i dostępu do danych skonfigurowanych przy użyciu Entity Framework i SQL Server, jesteśmy gotowi do dodawania obsługi rejestracji i logowania do aplikacji. Odwołaj się [wcześniej w procesie migracji](xref:migration/mvc#migrate-the-layout-file) , który dodał odwołanie do *_LoginPartial* w *_Layout. cshtml*. Teraz czas na powrót do tego kodu, usunięcie komentarza i dodanie do niezbędnych kontrolerów i widoków do obsługi funkcji logowania.

Usuń komentarz z wiersza `@Html.Partial` w *_Layout. cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Teraz Dodaj nowy widok Razor o nazwie *_LoginPartial* do folderu *widoki/udostępnione* :

Zaktualizuj *_LoginPartial. cshtml* przy użyciu następującego kodu (Zastąp całą jego zawartość):

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

W tym momencie powinno być możliwe odświeżenie witryny w przeglądarce.

## <a name="summary"></a>Podsumowanie

W ASP.NET Core wprowadzono zmiany w ASP.NET Identity funkcjach. W tym artykule pokazano, jak przeprowadzić migrację funkcji zarządzania uwierzytelnianiem i użytkownikami ASP.NET Identity, aby ASP.NET Core.
