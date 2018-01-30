---
title: "Migrowanie uwierzytelnianie i tożsamość"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: f02d9472ea0aa1dceae3f53c812776aab85ab54e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-authentication-and-identity"></a>Migrowanie uwierzytelnianie i tożsamość

<a name="migration-identity"></a>

Przez [Steve Smith](https://ardalis.com/)

W poprzednim artykule firma Microsoft [migracji konfiguracji z projektu programu ASP.NET MVC do platformy ASP.NET Core MVC](configuration.md). W tym artykule będziemy migrować funkcji zarządzania rejestracji, logowania i użytkownika.

## <a name="configure-identity-and-membership"></a>Konfiguruj tożsamość i członkostwa

Na platformie ASP.NET MVC uwierzytelnianie i tożsamość funkcje są skonfigurowane przy użyciu tożsamości ASP.NET Startup.Auth.cs i IdentityConfig.cs znajdujące się w folderze App_Start. W programie ASP.NET MVC Core, te funkcje są konfigurowane w *Startup.cs*.

Zainstaluj `Microsoft.AspNetCore.Identity.EntityFrameworkCore` i `Microsoft.AspNetCore.Authentication.Cookies` pakietów NuGet.

Następnie należy otworzyć pliku Startup.cs i aktualizacji `ConfigureServices()` sposób korzystania z programu Entity Framework i tożsamość usług:

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

W tym momencie istnieją dwa typy, do którego odwołuje się powyżej kod, który firma Microsoft nie zostały jeszcze zmigrowane z projektu programu ASP.NET MVC: `ApplicationDbContext` i `ApplicationUser`. Utwórz nową *modele* folderu w ASP.NET Core projektu, a następnie dodaj dwie klasy do niego odpowiednie do tych typów. ASP.NET MVC znajdzie wersji tych klas w `/Models/IdentityModels.cs`, ale używamy jednego pliku na klasę w projekcie migrowane, ponieważ jest podejmowanie.

ApplicationUser.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

ApplicationDbContext.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

Projekt sieci Web platformy ASP.NET Core MVC Starter nie zawiera wiele dostosowanie użytkowników lub ApplicationDbContext. Podczas migrowania rzeczywistej aplikacji, należy również do migracji wszystkie niestandardowe właściwości i metod użytkownika aplikacji i klasy DbContext, jak również inne klasy modelu, który korzysta z aplikacji (na przykład, jeśli Twoje DbContext ma DbSet<Album>, oczywiście należy migrować albumów klasy).

Z tych plików w miejscu pliku Startup.cs może się skompilować, aktualizując za jego pomocą instrukcje:

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

Naszej aplikacji jest teraz gotowy do obsługi uwierzytelniania i tożsamość usług — tylko musi mieć te funkcje uwidoczniona dla użytkowników.

## <a name="migrate-registration-and-login-logic"></a>Migrowanie rejestracji i logowania logiki

Z usługi tożsamości aplikacji i dostępu do danych konfiguracji przy użyciu programu Entity Framework i programu SQL Server możemy teraz przystąpić do dodać obsługę rejestracji i logowania do aplikacji. Odwołania, który [wcześniej w procesie migracji](mvc.md#migrate-layout-file) możemy oznaczone jako komentarz odwołanie do _LoginPartial w _Layout.cshtml. Teraz nadszedł czas na powrót do tego kodu, Usuń komentarz, a następnie dodaj wymagane kontrolery i widoki do obsługi funkcji logowania.

Aktualizacja _Layout.cshtml; Usuń znaczniki komentarza @Html.Partial wiersza:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Teraz Dodaj nowe strona widoku MVC wywołuje _LoginPartial do folderu widoki/Shared:

Zaktualizuj _LoginPartial.cshtml następującym kodem (Zastąp całą jego zawartość):

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
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

Platformy ASP.NET Core wprowadzono zmiany do funkcji ASP.NET Identity. W tym artykule jak już wspomniano sposób migracji uwierzytelniania i użytkownika funkcji do zarządzania tożsamością ASP.NET do platformy ASP.NET Core.
