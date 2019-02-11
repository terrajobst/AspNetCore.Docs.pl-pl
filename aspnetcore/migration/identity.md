---
title: Migrowanie do platformy ASP.NET Core uwierzytelnianie i tożsamość
author: ardalis
description: Dowiedz się, jak przeprowadzić migrację uwierzytelnianie i tożsamość z projektu programu ASP.NET MVC do projektu programu ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: b72bbc9ae91e4bd37c9ea9b2d09ebf47afb25dd7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/11/2019
ms.locfileid: "41757365"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="d5f9a-103">Migrowanie do platformy ASP.NET Core uwierzytelnianie i tożsamość</span><span class="sxs-lookup"><span data-stu-id="d5f9a-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="d5f9a-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d5f9a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d5f9a-105">W poprzednim artykule będziemy [migracji konfiguracji z projektu programu ASP.NET MVC ASP.NET Core MVC](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="d5f9a-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="d5f9a-106">W tym artykule migracji funkcji zarządzania rejestracji, logowania i użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="d5f9a-107">Konfigurowanie tożsamości i członkostwa</span><span class="sxs-lookup"><span data-stu-id="d5f9a-107">Configure Identity and Membership</span></span>

<span data-ttu-id="d5f9a-108">We wzorcu ASP.NET MVC, uwierzytelnianie i tożsamość funkcje są skonfigurowane za pomocą tożsamości ASP.NET w *Startup.Auth.cs* i *IdentityConfig.cs*, który znajduje się w *App_Start* folder.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="d5f9a-109">W programie ASP.NET Core MVC te funkcje są konfigurowane w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="d5f9a-110">Zainstaluj `Microsoft.AspNetCore.Identity.EntityFrameworkCore` i `Microsoft.AspNetCore.Authentication.Cookies` pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="d5f9a-111">Następnie otwórz *Startup.cs* i zaktualizuj `Startup.ConfigureServices` metody do korzystania z programu Entity Framework i tożsamość usług:</span><span class="sxs-lookup"><span data-stu-id="d5f9a-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="d5f9a-112">W tym momencie są dwa typy przywoływane w powyższym kodzie, który firma Microsoft nie zostały jeszcze zmigrowane z projektu platformy ASP.NET MVC: `ApplicationDbContext` i `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="d5f9a-113">Utwórz nową *modeli* folderu w programie ASP.NET Core projektu i dodać dwie klasy do niego odpowiednie do tych typów.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="d5f9a-114">ASP.NET MVC znajdzie wersje tych klas w */Models/IdentityModels.cs*, ale użyjemy jednego pliku na klasy w projekcie migrowane, ponieważ jest podejmowanie.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="d5f9a-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="d5f9a-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="d5f9a-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="d5f9a-116">*ApplicationDbContext.cs*:</span></span>

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

<span data-ttu-id="d5f9a-117">Projekt sieci Web platformy ASP.NET Core MVC Starter nie obejmuje wiele dostosowywania użytkowników, lub `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="d5f9a-118">Podczas migrowania rzeczywistej aplikacji, trzeba będzie również migrację wszystkie niestandardowe właściwości i metod użytkowników aplikacji i `DbContext` klasy, jak również inne klasy modelu, korzysta z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="d5f9a-119">Na przykład jeśli Twoja `DbContext` ma `DbSet<Album>`, chcesz przeprowadzić migrację `Album` klasy.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="d5f9a-120">Z tymi plikami w miejscu *Startup.cs* pliku może się skompilować, aktualizując jej `using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="d5f9a-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="d5f9a-121">Nasza aplikacja jest teraz gotowy do obsługi uwierzytelniania i usługi zarządzania tożsamościami.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="d5f9a-122">Po prostu musi ona mieć tych funkcji, widoczna dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="d5f9a-123">Migrowanie rejestracji i logowania logiki</span><span class="sxs-lookup"><span data-stu-id="d5f9a-123">Migrate registration and login logic</span></span>

<span data-ttu-id="d5f9a-124">Za pomocą usługi zarządzania tożsamościami dla nich skonfigurowane i dostęp do danych skonfigurowane za pomocą platformy Entity Framework i programu SQL Server możemy przystąpić do dodano obsługę rejestracji i logowania do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="d5f9a-125">Pamiętamy [wcześniej w procesie migracji](xref:migration/mvc#migrate-the-layout-file) możemy komentarzami odwołanie do *_LoginPartial* w *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="d5f9a-126">Teraz nadszedł czas, aby powrócić do tego kodu, usuń znaczniki komentarza i dodawać wymagane kontrolery i widoki, do obsługi funkcji logowania.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="d5f9a-127">Usuń znaczniki komentarza `@Html.Partial` linię *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d5f9a-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="d5f9a-128">Teraz Dodaj nowy widok Razor o nazwie *_LoginPartial* do *widoków/Shared* folderu:</span><span class="sxs-lookup"><span data-stu-id="d5f9a-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="d5f9a-129">Aktualizacja *_LoginPartial.cshtml* następującym kodem (Zastąp całą jego zawartość):</span><span class="sxs-lookup"><span data-stu-id="d5f9a-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="d5f9a-130">W tym momencie użytkownik powinien móc Odśwież witrynę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="d5f9a-131">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="d5f9a-131">Summary</span></span>

<span data-ttu-id="d5f9a-132">Platforma ASP.NET Core wprowadza zmiany do funkcji produktu ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="d5f9a-133">W tym artykule mają już, jak przeprowadzić migrację uwierzytelniania użytkownika funkcji zarządzania i tożsamości ASP.NET do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d5f9a-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
