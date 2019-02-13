---
title: Migrowanie do platformy ASP.NET Core uwierzytelnianie i tożsamość
author: ardalis
description: Dowiedz się, jak przeprowadzić migrację uwierzytelnianie i tożsamość z projektu programu ASP.NET MVC do projektu programu ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/13/2019
ms.locfileid: "41757365"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Migrowanie do platformy ASP.NET Core uwierzytelnianie i tożsamość

Przez [Steve Smith](https://ardalis.com/)

W poprzednim artykule będziemy [migracji konfiguracji z projektu programu ASP.NET MVC ASP.NET Core MVC](xref:migration/configuration). W tym artykule migracji funkcji zarządzania rejestracji, logowania i użytkownika.

## <a name="configure-identity-and-membership"></a>Konfigurowanie tożsamości i członkostwa

We wzorcu ASP.NET MVC, uwierzytelnianie i tożsamość funkcje są skonfigurowane za pomocą tożsamości ASP.NET w *Startup.Auth.cs* i *IdentityConfig.cs*, który znajduje się w *App_Start* folder. W programie ASP.NET Core MVC te funkcje są konfigurowane w *Startup.cs*.

Zainstaluj `Microsoft.AspNetCore.Identity.EntityFrameworkCore` i `Microsoft.AspNetCore.Authentication.Cookies` pakietów NuGet.

Następnie otwórz *Startup.cs* i zaktualizuj `Startup.ConfigureServices` metody do korzystania z programu Entity Framework i tożsamość usług:

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

W tym momencie są dwa typy przywoływane w powyższym kodzie, który firma Microsoft nie zostały jeszcze zmigrowane z projektu platformy ASP.NET MVC: `ApplicationDbContext` i `ApplicationUser`. Utwórz nową *modeli* folderu w programie ASP.NET Core projektu i dodać dwie klasy do niego odpowiednie do tych typów. ASP.NET MVC znajdzie wersje tych klas w */Models/IdentityModels.cs*, ale użyjemy jednego pliku na klasy w projekcie migrowane, ponieważ jest podejmowanie.

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
using Microsoft.AspNetCore.Identity.EntityFramework;
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

Projekt sieci Web platformy ASP.NET Core MVC Starter nie obejmuje wiele dostosowywania użytkowników, lub `ApplicationDbContext`. Podczas migrowania rzeczywistej aplikacji, trzeba będzie również migrację wszystkie niestandardowe właściwości i metod użytkowników aplikacji i `DbContext` klasy, jak również inne klasy modelu, korzysta z aplikacji. Na przykład jeśli Twoja `DbContext` ma `DbSet<Album>`, chcesz przeprowadzić migrację `Album` klasy.

Z tymi plikami w miejscu *Startup.cs* pliku może się skompilować, aktualizując jej `using` instrukcji:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Nasza aplikacja jest teraz gotowy do obsługi uwierzytelniania i usługi zarządzania tożsamościami. Po prostu musi ona mieć tych funkcji, widoczna dla użytkowników.

## <a name="migrate-registration-and-login-logic"></a>Migrowanie rejestracji i logowania logiki

Za pomocą usługi zarządzania tożsamościami dla nich skonfigurowane i dostęp do danych skonfigurowane za pomocą platformy Entity Framework i programu SQL Server możemy przystąpić do dodano obsługę rejestracji i logowania do aplikacji. Pamiętamy [wcześniej w procesie migracji](xref:migration/mvc#migrate-the-layout-file) możemy komentarzami odwołanie do *_LoginPartial* w *_Layout.cshtml*. Teraz nadszedł czas, aby powrócić do tego kodu, usuń znaczniki komentarza i dodawać wymagane kontrolery i widoki, do obsługi funkcji logowania.

Usuń znaczniki komentarza `@Html.Partial` linię *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Teraz Dodaj nowy widok Razor o nazwie *_LoginPartial* do *widoków/Shared* folderu:

Aktualizacja *_LoginPartial.cshtml* następującym kodem (Zastąp całą jego zawartość):

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

W tym momencie użytkownik powinien móc Odśwież witrynę w przeglądarce.

## <a name="summary"></a>Podsumowanie

Platforma ASP.NET Core wprowadza zmiany do funkcji produktu ASP.NET Identity. W tym artykule mają już, jak przeprowadzić migrację uwierzytelniania użytkownika funkcji zarządzania i tożsamości ASP.NET do ASP.NET Core.
