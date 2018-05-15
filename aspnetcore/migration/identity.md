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
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="9ca19-103">Migrowanie do platformy ASP.NET Core uwierzytelnianie i tożsamość</span><span class="sxs-lookup"><span data-stu-id="9ca19-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="9ca19-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9ca19-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9ca19-105">W poprzednim artykule firma Microsoft [migracji konfiguracji z projektu programu ASP.NET MVC do platformy ASP.NET Core MVC](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="9ca19-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="9ca19-106">W tym artykule będziemy migrować funkcji zarządzania rejestracji, logowania i użytkownika.</span><span class="sxs-lookup"><span data-stu-id="9ca19-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="9ca19-107">Konfiguruj tożsamość i członkostwa</span><span class="sxs-lookup"><span data-stu-id="9ca19-107">Configure Identity and Membership</span></span>

<span data-ttu-id="9ca19-108">W programie ASP.NET MVC, uwierzytelnianie i tożsamość funkcje są konfigurowane przy użyciu tożsamości platformy ASP.NET w *Startup.Auth.cs* i *IdentityConfig.cs*, który znajduje się w *App_Start* folder.</span><span class="sxs-lookup"><span data-stu-id="9ca19-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="9ca19-109">W programie ASP.NET MVC Core, te funkcje są konfigurowane w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="9ca19-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="9ca19-110">Zainstaluj `Microsoft.AspNetCore.Identity.EntityFrameworkCore` i `Microsoft.AspNetCore.Authentication.Cookies` pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="9ca19-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="9ca19-111">Następnie otwórz *Startup.cs* i zaktualizuj `Startup.ConfigureServices` sposób korzystania z programu Entity Framework i tożsamość usług:</span><span class="sxs-lookup"><span data-stu-id="9ca19-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="9ca19-112">W tym momencie istnieją dwa typy, do którego odwołuje się powyżej kod, który firma Microsoft nie zostały jeszcze zmigrowane z projektu programu ASP.NET MVC: `ApplicationDbContext` i `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="9ca19-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="9ca19-113">Utwórz nową *modele* folderu w ASP.NET Core projektu, a następnie dodaj dwie klasy do niego odpowiednie do tych typów.</span><span class="sxs-lookup"><span data-stu-id="9ca19-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="9ca19-114">ASP.NET MVC znajdzie wersji tych klas w */Models/IdentityModels.cs*, ale używamy jednego pliku na klasę w projekcie migrowane, ponieważ jest podejmowanie.</span><span class="sxs-lookup"><span data-stu-id="9ca19-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="9ca19-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ca19-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="9ca19-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ca19-116">*ApplicationDbContext.cs*:</span></span>

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

<span data-ttu-id="9ca19-117">Projekt sieci Web platformy ASP.NET Core MVC Starter nie zawiera wiele dostosowania użytkowników, lub `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="9ca19-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="9ca19-118">Podczas migrowania aplikacji prawdziwe, należy również do migracji wszystkie niestandardowe właściwości i metod użytkownika aplikacji i `DbContext` klasy, jak również inne klasy modelu, korzysta z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9ca19-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="9ca19-119">Na przykład jeśli Twoje `DbContext` ma `DbSet<Album>`, należy przeprowadzić migrację `Album` klasy.</span><span class="sxs-lookup"><span data-stu-id="9ca19-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="9ca19-120">Z tych plików w miejscu *Startup.cs* plików może się skompilować, aktualizując jego `using` instrukcji:</span><span class="sxs-lookup"><span data-stu-id="9ca19-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="9ca19-121">Naszej aplikacji jest teraz gotowy do obsługi uwierzytelniania i tożsamości.</span><span class="sxs-lookup"><span data-stu-id="9ca19-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="9ca19-122">Tylko musi mieć te funkcje uwidoczniona dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="9ca19-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="9ca19-123">Migrowanie logiki rejestracji i logowania</span><span class="sxs-lookup"><span data-stu-id="9ca19-123">Migrate registration and login logic</span></span>

<span data-ttu-id="9ca19-124">Usługi tożsamości aplikacji i dostępu do danych konfiguracji przy użyciu programu Entity Framework i programu SQL Server jest już dodana obsługa rejestracji i logowania do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9ca19-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="9ca19-125">Odwołania, który [wcześniej w procesie migracji](xref:migration/mvc#migrate-the-layout-file) możemy oznaczone jako komentarz odwołanie do *_LoginPartial* w *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9ca19-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="9ca19-126">Teraz nadszedł czas na powrót do tego kodu, Usuń komentarz, a następnie dodaj wymagane kontrolery i widoki do obsługi funkcji logowania.</span><span class="sxs-lookup"><span data-stu-id="9ca19-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="9ca19-127">Usuń znaczniki komentarza `@Html.Partial` wiersz w *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="9ca19-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="9ca19-128">Teraz Dodaj nowy widok Razor o nazwie *_LoginPartial* do *widoków/Shared* folderu:</span><span class="sxs-lookup"><span data-stu-id="9ca19-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="9ca19-129">Aktualizacja *_LoginPartial.cshtml* następującym kodem (Zastąp całą jego zawartość):</span><span class="sxs-lookup"><span data-stu-id="9ca19-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="9ca19-130">W tym momencie można odświeżyć witrynę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="9ca19-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="9ca19-131">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="9ca19-131">Summary</span></span>

<span data-ttu-id="9ca19-132">Platformy ASP.NET Core wprowadzono zmiany do funkcji ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="9ca19-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="9ca19-133">W tym artykule jak już wspomniano sposób migracji funkcji zarządzania uwierzytelniania i użytkownik tożsamości ASP.NET do platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9ca19-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
