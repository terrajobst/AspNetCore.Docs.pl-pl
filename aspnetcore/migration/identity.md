---
title: Migrowanie do platformy ASP.NET Core uwierzytelnianie i tożsamość
author: ardalis
description: Dowiedz się, jak przeprowadzić migrację uwierzytelnianie i tożsamość z projektu programu ASP.NET MVC do projektu programu ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: 2a80274e9056b41e370f199c7d41865db5fcedd7
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Migrowanie do platformy ASP.NET Core uwierzytelnianie i tożsamość

Przez [Steve Smith](https://ardalis.com/)

W poprzednim artykule firma Microsoft [migracji konfiguracji z projektu programu ASP.NET MVC do platformy ASP.NET Core MVC](xref:migration/configuration). W tym artykule będziemy migrować funkcji zarządzania rejestracji, logowania i użytkownika.

## <a name="configure-identity-and-membership"></a>Konfiguruj tożsamość i członkostwa

W programie ASP.NET MVC, uwierzytelnianie i tożsamość funkcje są konfigurowane przy użyciu tożsamości platformy ASP.NET w *Startup.Auth.cs* i *IdentityConfig.cs*, który znajduje się w *App_Start* folder. W programie ASP.NET MVC Core, te funkcje są konfigurowane w *Startup.cs*.

Zainstaluj `Microsoft.AspNetCore.Identity.EntityFrameworkCore` i `Microsoft.AspNetCore.Authentication.Cookies` pakietów NuGet.

Następnie otwórz *Startup.cs* i zaktualizuj `Startup.ConfigureServices` sposób korzystania z programu Entity Framework i tożsamość usług:

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

W tym momencie istnieją dwa typy, do którego odwołuje się powyżej kod, który firma Microsoft nie zostały jeszcze zmigrowane z projektu programu ASP.NET MVC: `ApplicationDbContext` i `ApplicationUser`. Utwórz nową *modele* folderu w ASP.NET Core projektu, a następnie dodaj dwie klasy do niego odpowiednie do tych typów. ASP.NET MVC znajdzie wersji tych klas w */Models/IdentityModels.cs*, ale używamy jednego pliku na klasę w projekcie migrowane, ponieważ jest podejmowanie.

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
            // Customize the ASP.NET Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

Projekt sieci Web platformy ASP.NET Core MVC Starter nie zawiera wiele dostosowania użytkowników, lub `ApplicationDbContext`. Podczas migrowania aplikacji prawdziwe, należy również do migracji wszystkie niestandardowe właściwości i metod użytkownika aplikacji i `DbContext` klasy, jak również inne klasy modelu, korzysta z aplikacji. Na przykład jeśli Twoje `DbContext` ma `DbSet<Album>`, należy przeprowadzić migrację `Album` klasy.

Z tych plików w miejscu *Startup.cs* plików może się skompilować, aktualizując jego `using` instrukcji:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Naszej aplikacji jest teraz gotowy do obsługi uwierzytelniania i tożsamości. Tylko musi mieć te funkcje uwidoczniona dla użytkowników.

## <a name="migrate-registration-and-login-logic"></a>Migrowanie logiki rejestracji i logowania

Usługi tożsamości aplikacji i dostępu do danych konfiguracji przy użyciu programu Entity Framework i programu SQL Server jest już dodana obsługa rejestracji i logowania do aplikacji. Odwołania, który [wcześniej w procesie migracji](xref:migration/mvc#migrate-the-layout-file) możemy oznaczone jako komentarz odwołanie do *_LoginPartial* w *_Layout.cshtml*. Teraz nadszedł czas na powrót do tego kodu, Usuń komentarz, a następnie dodaj wymagane kontrolery i widoki do obsługi funkcji logowania.

Usuń znaczniki komentarza `@Html.Partial` wiersz w *_Layout.cshtml*:

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

W tym momencie można odświeżyć witrynę w przeglądarce.

## <a name="summary"></a>Podsumowanie

Platformy ASP.NET Core wprowadzono zmiany do funkcji ASP.NET Identity. W tym artykule jak już wspomniano sposób migracji funkcji zarządzania uwierzytelniania i użytkownik tożsamości ASP.NET do platformy ASP.NET Core.
